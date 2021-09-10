## Build and Run instructions for projects I am contributing

## FluentBit Core inside EC2 instance

**Login to EC2:**

- `ssh -i "raykeypair.pem" ec2-user@ec2-34-222-196-23.us-west-2.compute.amazonaws.com` 

**Normal run:**

- `cd Work/flb-core/fluent-bit/build/`
- `make`
- `bin/fluent-bit -i cpu -o stdout -f 5`
- `bin/fluent-bit -c fluent-bit.conf`

**Run fluent-bit with Valgrind:**

- `cd Work/flb-core/fluent-bit/build/`
- `make`
- `valgrind --leak-check=yes --track-origins=yes bin/fluent-bit -c fluent-bit.conf`



**Go checksum mismatch error:**
```
verifying github.com/StackExchange/wmi@v1.2.0/go.mod: checksum mismatch
	downloaded: h1:rcmrprowKIVzvc+NUiLncP2uuArMWLCbu9SBzvHz7e8=
	go.sum:     h1:3eOhrUMpNV+6aFIbp5/iudMxNCF27Vw2OZgy4xEx0Fg=

SECURITY ERROR
This download does NOT match an earlier download recorded in go.sum.
The bits may have been replaced on the origin server, or an attacker may
have intercepted the download attempt.
```

Solution:

In the `go.sum` file search for `github.com/StackExchange/wmi@v1.2.0/go.mod` and replace with
the downloaded version.

find: `github.com/StackExchange/wmi v1.2.0/go.mod h1:h1:3eOhrUMpNV+6aFIbp5/iudMxNCF27Vw2OZgy4xEx0Fg=`

replace with: `github.com/StackExchange/wmi v1.2.0/go.mod h1:rcmrprowKIVzvc+NUiLncP2uuArMWLCbu9SBzvHz7e8=`



## Need to Update

Build & Run



*FluentBit Core: C code*

*Login to EC2:*
ssh -i "raykeypair.pem" ec2-user@ec2-34-222-196-23.us-west-2.compute.amazonaws.com 

*Normal run:*
cd Work/flb-core/fluent-bit/build/
make
bin/fluent-bit -i cpu -o stdout -f 5
bin/fluent-bit -c fluent-bit.conf

*With Valgrind:*
cd Work/flb-core/fluent-bit/build/
make
valgrind --leak-check=yes --track-origins=yes bin/fluent-bit -c fluent-bit.conf


git add ../plugins/out_cloudwatch_logs/cloudwatch_api.h ../plugins/out_cloudwatch_logs/cloudwatch_api.c  ../plugins/out_cloudwatch_logs/cloudwatch_logs.c ../plugins/out_cloudwatch_logs/cloudwatch_logs.h
git add ../include/fluent-bit/flb_intermediate_metric.h


aws-otel-test-framework

cd terraform/setup && terraform init && terraform apply -var=“region=us-east-1”

cd terraform/imagebuild && terraform init && terraform apply -var="region=us-east-1"

cd terraform/ecs && terraform init && terraform apply -var="aoc_image_repo=rhossai2/aoc" -var="aoc_version=v0.0.12" -var="testcase=../testcases/ecsmetrics" -var-file="../testcases/ecsmetrics/parameters.tfvars" -var="region=us-east-1"


opentelemetry-collector-contrib

make otelcontribcol
bin/otelcontribcol_darwin_amd64 --config test_config.yaml 

*Build docker image in the collector-contrib repo:*
make docker-otelcontribcol
docker tag otelcontribcol:latest rhossai2/otel-contrib:v0.0.0
docker push rhossai2/otel-contrib:v0.0.0

OpenTelemetry Collector

make otelcol
./bin/otelcol_darwin_amd64 —config test_config.yaml

make lint exporter/exporterhelper
cd exporter/exporterhelper
go test -v ./... -cover -coverprofile mytest.out
go tool cover -html=mytest.out



Run ADOT

*Run locally:*
make build
./build/darwin/aoc_darwin_amd64 —config config_test.yaml


*Build docker image in the collector repo:*
make build
make docker-build
docker tag amazon/awscollector:v0.4.0 rhossai2/aoc:v0.0.20
docker push rhossai2/aoc:v0.0.20


Add Receiver to ADOT

1. Get the code and update go.mod file (two lines, require and replace)- https://github.com/mxiamxia/aws-opentelemetry-collector/blob/master/go.mod
2. Comment out receiver enable option- https://github.com/mxiamxia/aws-opentelemetry-collector/blob/master/pkg/defaultcomponents/defaults.go
    1. //add custom receivers
            //receivers := []component.ReceiverFactoryBase{
            //    awsstatsd.NewFactory(),
            //}
            //for _, exp := range factories.Receivers {
            //    receivers = append(receivers, exp)
            //}
            //factories.Receivers, err = component.MakeReceiverFactoryMap(receivers...)
            //if err != nil {
            //    errs = append(errs, err)
            //}
3. Follow docker run guide- https://github.com/mxiamxia/aws-opentelemetry-collector/blob/master/docs/developers/docker-demo.md


*Replace from personal fork:*
replace github.com/open-telemetry/opentelemetry-collector-contrib/receiver/awsecscontainermetricsreceiver (http://github.com/open-telemetry/opentelemetry-collector-contrib/receiver/awsecscontainermetricsreceiver) v0.12.1-0.20201014141611-601e05747671 => github.com/hossain-rayhan/opentelemetry-collector-contrib/receiver/awsecscontainermetricsreceiver (http://github.com/hossain-rayhan/opentelemetry-collector-contrib/receiver/awsecscontainermetricsreceiver) v0.0.0-20201016151158-f1ac229a9f61

Add Receiver to ot-collector and build

Add receiver to opentelemetry-collector:

1. Add contrib receiver directory under receiver/
2. remove your go.mod and go.sum file
3. update service/defaultcomponents/defaults.go to add your receiver
4. Update your import url with - “go.opentelemetry.io/collector/receiver/awsecscontainermetricsreceiver (http://go.opentelemetry.io/collector/receiver/awsecscontainermetricsreceiver)” in receiver.go factory.go and other places where needed.



EKS- Kubernetes

kubectl config view 
kubectl config current-context
kubectl config use-context hrayhan-Isengard@ec2-test-1.us-east-2.eksctl.io

kubectl get pods -A
kubectl logs -n adot-col adot-collector-0
kubectl get events --sort-by=.metadata.creationTimestamp -n adot-col

kubectl apply -f k8scluster-far.yaml
kubectl replace —force -f k8scluster-far.yaml
kubectl delete -f k8scluster-far.yaml


*Get metrics from cAdvisor endpoint:*
kubectl get —raw “/api/v1/nodes/fargate-ip-192-168-137-185.us-east-2.compute.internal/proxy/metrics/cadvisor”



*Core dump:*

1. Before running your binary use ulimit -c unlimited
2. Run your binary valgrind --track-origins=yes bin/fluent-bit -c /home/ec2-user/fluent-bit/include/data/fluent-bit.conf > /home/ec2-user/fluent-bit/include/data/out.log 2>&1
3. You should see vgcore.xxx or core.xxx or something like this in the current directory
4. Run gdb <executable_file> <core_dump_file> to open your core dump with gdb debugger. In our case gdb bin/fluent-bit vgcore.14353
5. Run bt to see backtrace
6. Run thread apply all bt
7. 



Git Rebase

Git Rebase:
Say I have two branches- master and my-feature. my-feature is the local one I am working on. When, I am done with my work, build, and test, lets follow these steps:

* git checkout master
* git pull
* git checkout my-feature
* git rebase master

If we find any conflicts, let’s merge them manually first. If we see any conflicts in a big file (go.sum) and want to keep the theirs/ours one, run the following command- (while rebasing, --ours means master branch change,--theirs means your current branch changes)

* git checkout --theirs <path_to_file>
* git add <modified_files>
* git rebase --continue
* Build, test, and push (--force)


While resolving rebase conflicts, accept current change means “Changes in your local branch”


GitHub personal token sept2021:
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

*Coredump EC2:*
sudo su
echo "core.%e.%p.%h.%t" > /proc/sys/kernel/core_pattern
ulimit -c unlimited

*EC2 Disk Utilization:*

sudo du --max-depth=1 -h /
sudo du --max-depth=1 -h /usr
sudo du --max-depth=1 -h /home
sudo du --max-depth=1 -h /home/ec2-user/
sudo du --max-depth=1 -h /home/ec2-user/.vscode-server/
sudo du --max-depth=1 -h /home/ec2-user/.cache/

[ec2-user@ip-172-31-35-232 build]$ df -kh
Filesystem Size Used Avail Use% Mounted on
devtmpfs 16G 0 16G 0% /dev
tmpfs 16G 0 16G 0% /dev/shm
tmpfs 16G 448K 16G 1% /run
tmpfs 16G 0 16G 0% /sys/fs/cgroup
/dev/nvme0n1p1 100G 7.7G 93G 8% /
tmpfs 3.2G 0 3.2G 0% /run/user/1000


*FluentBit on Session Manager:*
aws ssm start-session --target i-05cb66b05b7cc4e91 --region us-west-2
ping firehose.us-west-2.amazonaws.com (http://firehose.us-west-2.amazonaws.com/)
sudo /home/ec2-user/fluent-bit/build/bin/fluent-bit -c /home/ec2-user/fluent-bit/include/data/fluent-bit.conf

*SSH to Private EC2:*
a483e7119b0b:Downloads hrayhan$ scp -i "raykeypair.pem" /Users/hrayhan/Downloads/raykeypair.pem ec2-user@ec2-35-165-87-33.us-west-2.compute.amazonaws.com:/home/ec2-user/

ssh to ssh_client_ec2: 
a483e7119b0b:Downloads hrayhan$ ssh -i "raykeypair.pem" ec2-user@ec2-35-165-87-33.us-west-2.compute.amazonaws.com

[ec2-user@ip-10-0-101-162 ~]$ ssh -i "/home/ec2-user/raykeypair.pem" ec2-user@10.0.104.101

[ec2-user@ip-10-0-104-101 ~]$ aws firehose list-delivery-streams
success

[ec2-user@ip-10-0-104-101 ~]$ aws kinesis list-streams
timeout

[ec2-user@ip-10-0-104-101 data]$ aws firehose list-delivery-streams --limit 1 --endpoint-url "https://vpce-0465c45a4adc5a4bd-zvqw708s.firehose.us-west-2.vpce.amazonaws.com"
{
 "DeliveryStreamNames": [
 "aws-fluent-bit-test-environ-firehoseDeliveryStream-6EL59VB28D1C"
 ],
 "HasMoreDeliveryStreams": true
}



