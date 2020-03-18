
    [openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-384 -out /home/myusername/.ssh/id_ecdsa](openssl.md#generating-an-ec-key)

# Generate a CSR
    # if it complains about you missing the ~/.rnd just "touch" it.
    [openssl req -new -key /home/myusername/.ssh/id_ecdsa -out myusername.csr -subj "/CN=myusername/O=system:masters"](openssl.md)

## csr.yaml

```yaml
apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  #name can be the result of uuidgen
  name: 0a63daf2-e114-41c3-add1-08b0ceef4f8b
spec:
  groups:
    - system:authenticated
  #request is $(cat myusername.csr | base64 | tr -d '\n')
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQkpEQ0Jxd0lCQURBc01SRXdEd1lEVlFRRERBaGxjbWxyWW1WeVp6RVhNQlVHQTFVRUNnd09jM2x6ZEdWdApPbTFoYzNSbGNuTXdkakFRQmdjcWhrak9QUUlCQmdVcmdRUUFJZ05pQUFSaWZicFE3cmF4bzF4bFREWHlZa1R6ClE4Z1JibHlEb3Z6dVVkYi9iUW1GY0VJcnFockVOUUR6UWlGVEJLd0daOW5rdlc0TjdzVzlPNjZndmkwY2NWRGgKZU5nWTFFOG8rYXRxZnZpR2Jwamd1VmZTRkZvR2ptMkdIZ05FNVM1d3l5Q2dBREFLQmdncWhrak9QUVFEQWdObwpBREJsQWpCZ3dVVFNLRlJORG1rZllia25TVTJYK1BmQ3FZajhBVDVia1ZJNTNaL1BRZTBBcnhUZFppZ2tEV3Q1Ck15d0FrRHNDTVFEbWRWTUxvQVo1TDlDQXpncXlmYmNMNzVYTnVxUE5Nbm9Oa2JEWlR5OGtHam5yZzZRQ1cwVlYKemlsWjZXRFJJQjA9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  usages:
    - digital signature
    - key encipherment
    - client auth
```

microk8s.kubectl create -f csr.yaml
microk8s.kubectl certificate approve 0a63daf2-e114-41c3-add1-08b0ceef4f8b
microk8s.kubectl get csr 0a63daf2-e114-41c3-add1-08b0ceef4f8b -o jsonpath='{.status.certificate}' | base64 -d > myusername.crt

