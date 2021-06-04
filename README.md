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

