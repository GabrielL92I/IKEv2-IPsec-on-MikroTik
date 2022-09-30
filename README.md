# IKEv2-IPsec-on-MikroTik
Configure IKEv2/IPsec on MikroTik(Site-to-client)

1- First, we choose and create a network for the VPN clients. In this tutorial I will use `192.168.50.0/24`. Than we will create the bridge and IP Pool.
 - Creation of the bridge where the network addresses will be added. `Bridge->Create new bridge`
![lanbridge](https://user-images.githubusercontent.com/44748406/193254479-f1898c81-8f45-4d17-9e12-902a34fed6ff.png)
 - Creation of the network address. `IP->Addresses`
![address](https://user-images.githubusercontent.com/44748406/193254730-3a1779bc-62c4-4bf6-9309-b6e6e05a2d86.png)
 - Creation of the IP Pool. You can set a desidered addresses range depending on how many clients will connect to the VPN. `IP->Pool`
![pool](https://user-images.githubusercontent.com/44748406/193254910-0f2760db-7dfb-4fb4-942c-b99b5aa4fce7.png)

2- Secondly, we have to create the firewall and NAT rule for the VPN to be able for the clients to communicate with the VPN Server.

- Creation of the firewall filter rule. `IP->Firewall->Filter Rules`
![nat](https://user-images.githubusercontent.com/44748406/193255259-ccc0eac7-3642-43e4-91b3-8e10cc913a5b.png)
- Creation of the NAT rule(only if you don't have this rule already). `IP->Firewall->NAT`
![masqeurade](https://user-images.githubusercontent.com/44748406/193255383-4fbf71a8-15c1-46d7-b58b-007e75ef67b9.png)
  
 4- Forth, we will create and export certificates(VPN_IKE2_CA,VPN_IKE2_SERVER and VPN_IKE2_CLIENT) needed for authentication between VPN server and client.

- Creation of CA certificate. `System->Certificates`

  On General Tab:
   -Name: VPN_IKE2_CA
   -Common Name: VPN_IKE2_CA
   -Subject Alt Name: your public IP address or your DNS
   -Key Size: 2048
  
  On Key Usage Tab:
   -key cert. sing
   -crl sing
  
- Creation of server certificate. `System->Certificates`
  
  On General Tab:
   -Name: VPN_IKE2_SERVER
   -Common Name: VPN_IKE2_SERVER
   -Subject Alt Name: your public IP address or your DNS
   -Key Size: 2048
  
  On Key Usage Tab:
   -digital signature
   -key encipherment
   -tls server
  
  On Sing button click:
   -CA: VPN_IKE2_CA

- Creation of client certificate. `System->Certificates`
  
  On General Tab:
   -Name: VPN_IKE2_CLIENT
   -Common Name: VPN_IKE2_CLIENT
   -Subject Alt Name: your public IP address or your DNS
   -Key Size: 2048
  
  On Key Usage Tab:
   -tls client
  
  On Sing button click:
   -CA: VPN_IKE2_CA
  
![cert](https://user-images.githubusercontent.com/44748406/193256721-7018fcf6-f236-4ff9-bcdf-33d6a49c6967.png)

- Export certificates. `System->Certificates`

  Right click on VPN_IKE2_CA certificate and click export and than export as PEM.
  Right click on VPN_IKE2_CLIENT certificate and click export, enter a passphrase and than export as PKCS12.
  
  After export you can find the certificates on `Files` where you can download them because will need them on the client part when you will connect to the VPN.

5- Fifth, we will create Policie, Proposal, Group, Peer, Identities, Profile, Mode Configs 
     
     Thank you.
