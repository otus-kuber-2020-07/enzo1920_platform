```
helm status vault
NAME: vault
LAST DEPLOYED: Thu Nov 26 15:41:46 2020
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing HashiCorp Vault!

Now that you have deployed Vault, you should look over the docs on using
Vault with Kubernetes available here:

https://www.vaultproject.io/docs/


Your release is named vault. To learn more about the release, try:

  $ helm status vault
  $ helm get manifest vault
```

-------------------------------------------------------------------
```
kubectl get pod -l app.kubernetes.io/instance=vault
NAME                                    READY   STATUS    RESTARTS   AGE
vault-0                                 0/1     Running   0          35m
vault-1                                 0/1     Running   0          35m
vault-2                                 0/1     Running   0          35m
vault-agent-injector-56bf46695f-n2fm2   1/1     Running   0          35m
```

-----------------------
```
kubectl exec -it vault-0 -- vault operator init --key-shares=1 --key-threshold=1
Unseal Key 1: fwjektcsG3uAbj6WKIkWkm+q5iFqfXdgufH2eMguwdY=

Initial Root Token: s.w2uKtVPi3c4fZx1UX3wuGriQ

Vault initialized with 1 key shares and a key threshold of 1. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 1 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 1 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```
-----------------------------
```
kubectl exec -it vault-0 -- vault status
Key                Value
---                -----
Seal Type          shamir
Initialized        true
Sealed             true
Total Shares       1
Threshold          1
Unseal Progress    0/1
Unseal Nonce       n/a
Version            1.5.4
HA Enabled         true
command terminated with exit code 2
```
-----------------------------
```
kubectl exec -it vault-0 -- vault operator unseal
Unseal Key (will be hidden):
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.5.4
Cluster Name    vault-cluster-4d84e6b4
Cluster ID      47115f1a-8da2-167c-454c-7dfd4f3c169a
HA Enabled      true
HA Cluster      https://vault-0.vault-internal:8201
HA Mode         active
 
kubectl exec -it vault-1 -- vault operator unseal
Unseal Key (will be hidden):
Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.5.4
Cluster Name           vault-cluster-4d84e6b4
Cluster ID             47115f1a-8da2-167c-454c-7dfd4f3c169a
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.96.1.8:8200
 
kubectl exec -it vault-2 -- vault operator unseal
Unseal Key (will be hidden):
Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.5.4
Cluster Name           vault-cluster-4d84e6b4
Cluster ID             47115f1a-8da2-167c-454c-7dfd4f3c169a
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.96.1.8:8200
---------------------------------------------

kubectl exec -it vault-0 -- vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.5.4
Cluster Name    vault-cluster-4d84e6b4
Cluster ID      47115f1a-8da2-167c-454c-7dfd4f3c169a
HA Enabled      true
HA Cluster      https://vault-0.vault-internal:8201
HA Mode         active
 kubectl exec -it vault-1 -- vault status
Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.5.4
Cluster Name           vault-cluster-4d84e6b4
Cluster ID             47115f1a-8da2-167c-454c-7dfd4f3c169a
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.96.1.8:8200
 kubectl exec -it vault-2 -- vault status
Key                    Value
---                    -----
Seal Type              shamir
Initialized            true
Sealed                 false
Total Shares           1
Threshold              1
Version                1.5.4
Cluster Name           vault-cluster-4d84e6b4
Cluster ID             47115f1a-8da2-167c-454c-7dfd4f3c169a
HA Enabled             true
HA Cluster             https://vault-0.vault-internal:8201
HA Mode                standby
Active Node Address    http://10.96.1.8:8200
```
----------------------------------------------------
```
kubectl exec -it vault-0 -- vault login
Token (will be hidden):
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.w2uKtVPi3c4fZx1UX3wuGriQ
token_accessor       zaaiZZIw7kesHPU0d0WvQXos
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```
-----------------------------------------------------
```
kubectl exec -it vault-0 -- vault login
Token (will be hidden):
Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.w2uKtVPi3c4fZx1UX3wuGriQ
token_accessor       zaaiZZIw7kesHPU0d0WvQXos
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]

kubectl exec -it vault-0 -- vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_ee851ea5    token based credentials
```
------------------------------------------------------------------
```
kubectl exec -it vault-0 -- vault read otus/otus-ro/config
Key                 Value
---                 -----
refresh_interval    768h
password            asajkjkahs
username            otus
 
kubectl exec -it vault-0 -- vault kv get otus/otus-rw/config
====== Data ======
Key         Value
---         -----
password    asajkjkahs
username    otus
```
-------------------------------------------------------------------
```
kubectl exec -it vault-0 -- vault auth list
Path           Type          Accessor                    Description
----           ----          --------                    -----------
kubernetes/    kubernetes    auth_kubernetes_51a10776    n/a
token/         token         auth_token_ee851ea5         token based credentials

```
-------------------------------------------------------------------
```
kubectl cp otus-policy.hcl vault-0:/tmp
kubectl exec -it -n default vault-0 -- ls -lh /tmp
total 8K
-rw-r--r--    1 vault    vault        135 Nov 28 11:39 otus-policy.hcl
-rw-r--r--    1 vault    vault        595 Nov 26 15:41 storageconfig.hcl

kubectl exec -it vault-0 -- vault policy write otus-policy /tmp/otus-policy.hcl
Success! Uploaded policy: otus-policy

```
-------------------------------------------------------------------
```
kubectl exec -it vault-0 -- vault write auth/kubernetes/role/otus \
> bound_service_account_names=vault-auth \
> bound_service_account_namespaces=default policies=otus-policy ttl=24h
Success! Data written to: auth/kubernetes/role/otus

```
-------------------------------------------------------------------
```
/ # curl --header "X-Vault-Token:$TOKEN" $VAULT_ADDR/v1/otus/otus-ro/config
{"errors":["missing client token"]}
/ # curl --header "X-Vault-Token:s.w2uKtVPi3c4fZx1UX3wuGriQ" $VAULT_ADDR/v1/otus/otus-ro/config
{"request_id":"3cd2a6ce-1ec7-0376-db32-00dbbe5e5f11","lease_id":"","renewable":false,"lease_duration":2764800,"data":{"password":"asajkjkahs","username":"otus"},"wrap_info":null,"warnings":null,"auth":null}

 curl --header "X-Vault-Token:s.w2uKtVPi3c4fZx1UX3wuGriQ" $VAULT_ADDR/v1/otus/otus-ro/config
{"request_id":"e145ffe8-78c3-a165-35ee-9902f482d831","lease_id":"","renewable":false,"lease_duration":2764800,"data":{"password":"asajkjkahs","username":"otus"},"wrap_info":null,"warnings":null,"auth":null}

```

-------------------------------------------------------------------
```
curl --request POST --data '{"bar": "baz"}' --header "X-Vault-Token:s.w2uKtVPi3c4fZx1UX3wuGriQ" $VAULT_ADDR/v1/otus/otus-ro/config
curl --request POST --data '{"bar": "baz"}' --header "X-Vault-Token:s.w2uKtVPi3c4fZx1UX3wuGriQ" $VAULT_ADDR/v1/otus/otus-rw/config
curl --request POST --data '{"bar": "baz"}' --header "X-Vault-Token:s.w2uKtVPi3c4fZx1UX3wuGriQ" $VAULT_ADDR/v1/otus/otus-rw/config1
```
--------------------------------------------------------------------
```
path "otus/otus-rw/*" {
capabilities = ["read", "create", "list", "updata"]
```

---------------------------------------------------------------------
```
root@nginx-container:/# cd /usr/share/nginx/html
root@nginx-container:/usr/share/nginx/html# ls
50x.html  index.html
root@nginx-container:/usr/share/nginx/html# cat index.html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

-------------------------------------------------------------------
```
kubectl exec -it vault-0 -- vault write pki_int/issue/example-dot-ru common_name="gitlab.example.ru" ttl="24h"
Key                 Value
---                 -----
ca_chain            [-----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUTmfFqEn0wUZIC47Zz2liCl/Q/ygwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMDExMjgxNTI3MDlaFw0yNTEx
MjcxNTI3MzlaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALnjgDjYeK6F
HF6Y0Ey3m49q6+OLEnhpslvNR/BnN4+HcRI4Rbx+M3kIXgj++euVlV81QC7Y9Bh5
2xneB71Cols9iFLdpwJj/5OrxRa0tKBg2WApc89RSuip8k4V86koAxtJmUt/Msi9
tAj8yk6jSGR5HkKzlwqI7r2m6r2kAKbMXGbG4zqXq2u9d3IO5/pdhRupzqmgx0og
+31otB+LOxdPjD0d+9I0reMC6zm+VtPmFngmktQ5tjhwASag4ioUDfO02tm7p1Tw
fHFLx8jPWwoSW1yJcPR2SF0bC8HZC1TYglt7xcheDDJz6RYLEOBxcIRS1U5Ow5C0
U5iCLmBvm98CAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUOtwewHJgmxhLNVbVjlb5yRWT/HIwHwYDVR0jBBgwFoAU
nYYs4P5JQrTT/2Lzzm5BpAGM13MwNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
kKhwnehhNNSwDDJXGslQ0SIaprIspyR16vELKMRfEL1oDoFhn5S81IXINZUGAOuh
yVC3uw75I2kDCJjohKISAvkzRsKFCuA0Z5z5NOjWi9gQiche6yHEFNpf37AtQlIq
3USDebsZRi+ytCC30NVDHjTXqp0Ya/q/qFmT6Ig3pG6zRQO4Nf3gOse8eew/pb2s
n8CC18Y2Njvk6XPu6PpJj8ektJJG+T5MImE5VQCXoIbIrhBuaEQCpdfLYG9c+LCx
a41qSMUjZCN3W/OxGdcGEHG03kqCDnd9J9p96l3dlHKjtGP0N3tAPgGGnxDAstI1
/GEARV9y+zUOHpHJ5I4iYA==
-----END CERTIFICATE-----]
certificate         -----BEGIN CERTIFICATE-----
MIIDZzCCAk+gAwIBAgIUeiY2xmzx3G10DPybMEXNHWdLeMgwDQYJKoZIhvcNAQEL
BQAwLDEqMCgGA1UEAxMhZXhhbXBsZS5ydSBJbnRlcm1lZGlhdGUgQXV0aG9yaXR5
MB4XDTIwMTEyODE1Mjg1MloXDTIwMTEyOTE1MjkyMlowHDEaMBgGA1UEAxMRZ2l0
bGFiLmV4YW1wbGUucnUwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCm
WcKWxohJofMFWOWGNpU/5QU2zmx3i1CsoIMQKfN1+0GWolI6tOmS0oo9uMq3AXgE
oF4koSSX+2tHMnj1fEv6Ym+EuT9e50ePH+c6dj6rfo+g8FyPVjqM/bpHfwGR2FLx
o2sEhFftxb39e95/95dSKL1q/jYmDZZBkXZk72t/9/TId3oRFKCHwgqhEUFlCIeP
dNoYgDIMbsfZjTQgFHxZU5/j8uO3r8gwhQt/9a+HBHPM54B9b59cnjngX+uycfTi
sAOL5CvZ3A+W/VUY+LEB0Z+IASkVitpqIlDBqlCpu2Mcf2JMT0zLCBNns4Wb01mq
9Yp98QaBPdTC/Zw2yuuTAgMBAAGjgZAwgY0wDgYDVR0PAQH/BAQDAgOoMB0GA1Ud
JQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjAdBgNVHQ4EFgQUon90BOQqEwAkhB+i
ozLtN8ytjMwwHwYDVR0jBBgwFoAUOtwewHJgmxhLNVbVjlb5yRWT/HIwHAYDVR0R
BBUwE4IRZ2l0bGFiLmV4YW1wbGUucnUwDQYJKoZIhvcNAQELBQADggEBAC+HcCCR
Tonvwa6r7myISWX20vvOOHAkS9B7kBSyiooq97NBEDcM8wrp1+vGx2GlAmVZnuA0
AnFkKrntDTXroQIDtRuYZEmTQIcR2GRnlbWx9pkw0yLGn8226afx/J12KIimqmJJ
oSfQGa3tVjFlLlnMtvZZbEFC/jRu3wrremounlbNMbHjW/afgFjTv4b/I9Be5ptz
Y7XCjUo2ny+n6wz7LNXbCtBNV76gUKrFt9e3DGp78DQv7LkpNmexN5bFP4+5NGGM
2LsDNi3R6a7cCrTzIYiFmNhcm4MRawb/i/gwnYsRpSFFYpIMu4y1s7/bhWs7fgeQ
y9me5UPPD6YhNWE=
-----END CERTIFICATE-----
expiration          1606663762
issuing_ca          -----BEGIN CERTIFICATE-----
MIIDnDCCAoSgAwIBAgIUTmfFqEn0wUZIC47Zz2liCl/Q/ygwDQYJKoZIhvcNAQEL
BQAwFTETMBEGA1UEAxMKZXhtYXBsZS5ydTAeFw0yMDExMjgxNTI3MDlaFw0yNTEx
MjcxNTI3MzlaMCwxKjAoBgNVBAMTIWV4YW1wbGUucnUgSW50ZXJtZWRpYXRlIEF1
dGhvcml0eTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALnjgDjYeK6F
HF6Y0Ey3m49q6+OLEnhpslvNR/BnN4+HcRI4Rbx+M3kIXgj++euVlV81QC7Y9Bh5
2xneB71Cols9iFLdpwJj/5OrxRa0tKBg2WApc89RSuip8k4V86koAxtJmUt/Msi9
tAj8yk6jSGR5HkKzlwqI7r2m6r2kAKbMXGbG4zqXq2u9d3IO5/pdhRupzqmgx0og
+31otB+LOxdPjD0d+9I0reMC6zm+VtPmFngmktQ5tjhwASag4ioUDfO02tm7p1Tw
fHFLx8jPWwoSW1yJcPR2SF0bC8HZC1TYglt7xcheDDJz6RYLEOBxcIRS1U5Ow5C0
U5iCLmBvm98CAwEAAaOBzDCByTAOBgNVHQ8BAf8EBAMCAQYwDwYDVR0TAQH/BAUw
AwEB/zAdBgNVHQ4EFgQUOtwewHJgmxhLNVbVjlb5yRWT/HIwHwYDVR0jBBgwFoAU
nYYs4P5JQrTT/2Lzzm5BpAGM13MwNwYIKwYBBQUHAQEEKzApMCcGCCsGAQUFBzAC
hhtodHRwOi8vdmF1bHQ6ODIwMC92MS9wa2kvY2EwLQYDVR0fBCYwJDAioCCgHoYc
aHR0cDovL3ZhdWx0OjgyMDAvdjEvcGtpL2NybDANBgkqhkiG9w0BAQsFAAOCAQEA
kKhwnehhNNSwDDJXGslQ0SIaprIspyR16vELKMRfEL1oDoFhn5S81IXINZUGAOuh
yVC3uw75I2kDCJjohKISAvkzRsKFCuA0Z5z5NOjWi9gQiche6yHEFNpf37AtQlIq
3USDebsZRi+ytCC30NVDHjTXqp0Ya/q/qFmT6Ig3pG6zRQO4Nf3gOse8eew/pb2s
n8CC18Y2Njvk6XPu6PpJj8ektJJG+T5MImE5VQCXoIbIrhBuaEQCpdfLYG9c+LCx
a41qSMUjZCN3W/OxGdcGEHG03kqCDnd9J9p96l3dlHKjtGP0N3tAPgGGnxDAstI1
/GEARV9y+zUOHpHJ5I4iYA==
-----END CERTIFICATE-----
private_key         -----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEAplnClsaISaHzBVjlhjaVP+UFNs5sd4tQrKCDECnzdftBlqJS
OrTpktKKPbjKtwF4BKBeJKEkl/trRzJ49XxL+mJvhLk/XudHjx/nOnY+q36PoPBc
j1Y6jP26R38BkdhS8aNrBIRX7cW9/Xvef/eXUii9av42Jg2WQZF2ZO9rf/f0yHd6
ERSgh8IKoRFBZQiHj3TaGIAyDG7H2Y00IBR8WVOf4/Ljt6/IMIULf/WvhwRzzOeA
fW+fXJ454F/rsnH04rADi+Qr2dwPlv1VGPixAdGfiAEpFYraaiJQwapQqbtjHH9i
TE9MywgTZ7OFm9NZqvWKffEGgT3Uwv2cNsrrkwIDAQABAoIBAG4qf63uqQ9qi1D9
QHQT7d+0wjE7n6XDw5evbVzlC3EVBqjFswNiEFXFzPt3R/jOpjcaJX7IoT1tqewq
xuR8fqzBR+1z6T3mmQN2M2IKMRLN7Dy/Me1TQ89fsV/sApQd2UBh41vIcZrsHBil
UgMZa0Nj653R4PZuhRXDSc8qp9zEvaXt/x4mjt8oK3fOVaQVzN1jFodsSsVINoCe
3CPrWmR77FnuFMVbCpFzcI/cq4fHh4O/9abWE8Yi3XWGFmGhnv3r7LiNiRUPG0vr
Njo7ErPZhHsFSw43W0E6C3MqISJrDuZ4VfXiVrCt4bFcn9xh+bfW/7A8COPvQ5fs
XM0659ECgYEAwiTi1Dvo6vhz0N5KTHTPDvxX7latgXjEC0AxF8cFeYDkBLT9Z8+i
u0/x0lThibmsonuJ1O1kCYwYHFXpUTbTsHYUh7PmnIcZ6K3eqZTD26BIkWyKSz5t
RdDVa2I134Ir70xI0V1aqsACNs4SaoHTU5a2RpBywJfl0eoEwvIxhc8CgYEA21nv
niDrURzWDI5alXccdRNqAEG6zOEtXN0uyIoix9DWR5fLJ0G3/yGWLCMVx3kexeQe
rerVjYInmknhC6l40icDZaO3ws/0oMCqe8dMXMcpnjBbi4Oatgn8i3OMDqYgPLm+
tY+T3IBffaQmbMOdJ7NkXmRn0T4OD3mMg/Hk8v0CgYEApDcjDjXmm+mvvPppiiY7
hgd4MQdsF1mZKZMQc3ZKcS3OsRSOS6LbmOjUOP7HDwHYvk8UlOB/f3vaYQ1iHunr
QRV/5BENfVThW2jGMFa/HCSFfUx75Y1WTHmKLByUSQwIMY8rALygFlH3DXhdJ23M
3KVcZG+GQNW69UWm5WaK8Q0CgYB0Y1YnW2RBF8Hjy6z43ci1vmViWX4as+hz+9CV
C+bEi2UHe92JqO9Oaq3aIYuu2e+gJRi6Q36opUUKMuU8YOIA8D53MdWRgvJUUaak
Iu271NmaXCJW9q+io83i2J6aLz+vuQceI17bgyon6m+RCoJazG8XPsNvXTPsthv+
7t0CqQKBgAwtIWFuRAeMrsNHeYKv87UeqLRTrqrJEyKW0UhiMQp2WTu7kgT6ryK8
NQRsaj03NWY5jO5rVy4Ykd1j3q+0PwC1n+Ap/iN7OJbcjx4un9s8lPJXRhJobXJk
TywKQ2rzcxX4CHvq/VkVYsMFxIr/NZ636ELYaOW/VsnPhgf79ss/
-----END RSA PRIVATE KEY-----
private_key_type    rsa
serial_number       7a:26:36:c6:6c:f1:dc:6d:74:0c:fc:9b:30:45:cd:1d:67:4b:78:c8
```
---------------------------------------------------------------------
```
kubectl exec -it vault-0 -- vault write \ pki_int/revokeserial_number="7a:26:36:c6:6c:f1:dc:6d:74:0c:fc:9b:30:45:cd:1d:67:4b:78:c8"
```
---------------------------------------------------------------------