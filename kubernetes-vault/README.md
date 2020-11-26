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


----------------------
kubectl get pod -l app.kubernetes.io/instance=vault
NAME                                    READY   STATUS    RESTARTS   AGE
vault-0                                 0/1     Running   0          35m
vault-1                                 0/1     Running   0          35m
vault-2                                 0/1     Running   0          35m
vault-agent-injector-56bf46695f-n2fm2   1/1     Running   0          35m


-----------------------

root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault operator init --key-shares=1 --key-threshold=1
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
-----------------------------
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
-----------------------------

root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault operator unseal
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
root@SRV7NRMA69P2V:~# kubectl exec -it vault-1 -- vault operator unseal
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
root@SRV7NRMA69P2V:~# kubectl exec -it vault-2 -- vault operator unseal
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

root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault status
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
root@SRV7NRMA69P2V:~# kubectl exec -it vault-1 -- vault status
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
root@SRV7NRMA69P2V:~# kubectl exec -it vault-2 -- vault status
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
----------------------------------------------------
root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault login
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
root@SRV7NRMA69P2V:~#
---------
root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault login
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
root@SRV7NRMA69P2V:~# ^C
root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_ee851ea5    token based credentials
------------------------------------------------------------------

 kubectl exec -it vault-0 -- vault read otus/otus-ro/config
Key                 Value
---                 -----
refresh_interval    768h
password            asajkjkahs
username            otus
root@SRV7NRMA69P2V:~# kubectl exec -it vault-0 -- vault kv get otus/otus-rw/config
====== Data ======
Key         Value
---         -----
password    asajkjkahs
username    otus



