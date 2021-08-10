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

