If one centos machine, used by many clients

every client need ssh access to take backup then Its better to create a linux user in the name of client

lets assume **paytm** is cloud services client

```sh
useradd paytm
passwd paytm
```

Share ssh details to client **paytm**
Client can login with below command
```sh
ssh paytm@db1.silkcloud.com
```
