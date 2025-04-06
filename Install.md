# Install Scripts

```shell
# Install
./google-cloud-sdk/install.sh

gcloud compute regions list
gcloud compute zones list | grep europe-central2

gcloud config set compute/zone europe-central2-a # warsaw

gcloud compute instances list

NAME                      ZONE               MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP     STATUS
instance-20250405-131135  europe-central2-a  e2-medium     true         10.186.0.2   34.116.132.117  RUNNING

# Create a new instance via CLI

gcloud compute instances create instance-sdk1
Created [https://www.googleapis.com/compute/v1/projects/continual-lodge-455900-h4/zones/europe-central2-a/instances/instance-sdk1].
NAME           ZONE               MACHINE_TYPE   PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP  STATUS
instance-sdk1  europe-central2-a  n1-standard-1               10.186.0.3   34.118.74.8  RUNNING

# Create new instance in zone
gcloud compute instances create instance-sdk2 --zone us-central1-a

gcloud compute disk-types list
NAME                                  ZONE                       VALID_DISK_SIZES
hyperdisk-balanced-high-availability                             4GB-65536GB
pd-balanced                                                      10GB-65536GB
pd-ssd                                                           10GB-65536GB

# Create new instance with disk SSD
gcloud compute instances create instance-sdk3 --boot-disk-type=pd-ssd

gcloud compute instances create instance-sdk3 --machine-type=n1-standard-8

gcloud compute machine-types list
```