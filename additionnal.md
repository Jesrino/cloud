####################################################
# Replace these values with your lab's information #
####################################################

ZONE=##YOUR_ZONE##
REGION=##YOUR_REGION##

####################################################
# Task 1 - Create multiple web server instances
####################################################

for VM in web1 web2 web3
do
gcloud compute instances create $VM \
    --zone=$ZONE \
    --machine-type=e2-small \
    --network=default \
    --tags=network-lb-tag \
    --image-family=debian-12 \
    --image-project=debian-cloud
done

for VM in web1 web2 web3
do
gcloud compute ssh $VM \
    --zone=$ZONE \
    --quiet \
    --command="sudo apt-get update &&
    sudo apt-get install apache2 -y &&
    sudo service apache2 restart &&
    echo '<h3>Web Server: $VM</h3>' | sudo tee /var/www/html/index.html"
done

gcloud compute firewall-rules create www-firewall-network-lb \
    --network=default \
    --allow=tcp:80 \
    --target-tags=network-lb-tag

####################################################
# Task 2 - Configure the network load balancer
####################################################

gcloud compute addresses create network-lb-ip-1 \
    --region=$REGION

gcloud compute http-health-checks create basic-check

gcloud compute target-pools create www-pool \
    --region=$REGION \
    --http-health-check=basic-check

gcloud compute target-pools add-instances www-pool \
    --instances=web1,web2,web3 \
    --instances-zone=$ZONE \
    --region=$REGION

gcloud compute forwarding-rules create www-rule \
    --region=$REGION \
    --ports=80 \
    --address=network-lb-ip-1 \
    --target-pool=www-pool

LB_IP=$(gcloud compute forwarding-rules describe www-rule \
    --region=$REGION \
    --format="value(IPAddress)")

echo "Network Load Balancer IP: $LB_IP"

curl http://$LB_IP

####################################################
# Task 3 - Create an HTTP Load Balancer
####################################################

gcloud compute instance-templates create lb-backend-template \
    --machine-type=e2-medium \
    --network=default \
    --tags=allow-health-check \
    --image-family=debian-12 \
    --image-project=debian-cloud \
    --metadata=startup-script='#! /bin/bash
apt-get update
apt-get install -y apache2
systemctl enable apache2
systemctl restart apache2
echo "<h1>HTTP Load Balancer Backend: $(hostname)</h1>" > /var/www/html/index.html'

gcloud compute instance-groups managed create lb-backend-group \
    --template=lb-backend-template \
    --size=2 \
    --zone=$ZONE

gcloud compute instance-groups managed wait-until lb-backend-group \
    --stable \
    --zone=$ZONE

gcloud compute firewall-rules create fw-allow-health-check \
    --network=default \
    --action=ALLOW \
    --direction=INGRESS \
    --rules=tcp:80 \
    --source-ranges=130.211.0.0/22,35.191.0.0/16 \
    --target-tags=allow-health-check

gcloud compute health-checks create http http-basic-check \
    --port=80

gcloud compute backend-services create web-backend-service \
    --protocol=HTTP \
    --health-checks=http-basic-check \
    --global

gcloud compute backend-services add-backend web-backend-service \
    --instance-group=lb-backend-group \
    --instance-group-zone=$ZONE \
    --global

gcloud compute url-maps create web-map-http \
    --default-service=web-backend-service

gcloud compute target-http-proxies create http-lb-proxy \
    --url-map=web-map-http

gcloud compute addresses create lb-ipv4-1 \
    --ip-version=IPV4 \
    --global

gcloud compute forwarding-rules create http-content-rule \
    --address=lb-ipv4-1 \
    --global \
    --target-http-proxy=http-lb-proxy \
    --ports=80

HTTP_LB_IP=$(gcloud compute forwarding-rules describe http-content-rule \
    --global \
    --format="value(IPAddress)")

echo "HTTP Load Balancer IP: $HTTP_LB_IP"

curl http://$HTTP_LB_IP
