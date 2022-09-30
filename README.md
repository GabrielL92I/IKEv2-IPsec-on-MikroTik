# IKEv2/IPsec using certificate on Mikrotik by Gabriel Lami
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
  ![ex](https://user-images.githubusercontent.com/44748406/193257541-b06fc928-af1c-4af9-8d03-8107037356bd.png)

5- Fifth, we will create Policie, Proposal, Group, Peer, Identities, Profile, Mode Config. `IP->>IPsec`
     
 - Mode Config 
 ![modeconfigs](https://user-images.githubusercontent.com/44748406/193271245-e901a172-ebd4-4710-b7f6-0feb4056ddbd.png)
 
 - Profile
 ![profile](https://user-images.githubusercontent.com/44748406/193271557-408810fa-e298-4486-b6f5-db14cfd4fd33.png)
 
 - Group
 ![group](https://user-images.githubusercontent.com/44748406/193272029-019cbee3-2f78-4b98-a1e9-ccf3e61c92c0.png)
 
 - Proposal
 ![proposal](https://user-images.githubusercontent.com/44748406/193272276-70d6e0db-dbb0-47d5-8477-b242e827e99e.png)
 
 - Policie
 ![policies](https://user-images.githubusercontent.com/44748406/193272641-327cebcb-d98e-43bd-91b2-2e18533019df.png)
 
 - Peer
 ![peer](https://user-images.githubusercontent.com/44748406/193272865-b69101e6-927c-467f-bed9-f0bf5251dad1.png)
 
 - Indentity
 ![Identity](https://user-images.githubusercontent.com/44748406/193273092-93b4a7da-e2d7-4c5e-ac2a-4f643f1097e2.png)
 
 6- Sixth, we will install certificates and configure VPN connection on client computer.
 
  - Certificates installation
  
    Right click and install VPN_IKE2_CA.crt certificate on "Local Machine" and "Trusted Root Certification"
   ![certcaaaa](https://user-images.githubusercontent.com/44748406/193274779-fbddeb9a-4c3e-4762-8f57-df744d2b7a39.png)
    
    Right click and install VPN_IKE2_CLIENT.p12 certificate on "Local Machine" and "Personal"
    ![certclient](https://user-images.githubusercontent.com/44748406/193274723-887e644c-8412-414c-ba2d-c12d3eda30e9.png)
    
   - Client VPN Connection
   
     Go to `Settings->Network & internet->VPN->Add VPN` and fill in the needed information
     
     Connection name: Whatever name you want
     Server name or address: your public IP address or DNS
     VPN Type: IKEv2
     Type of sing-in info: Certificate
     
     ![vpnconn](https://user-images.githubusercontent.com/44748406/193276226-f0b7e726-c024-492a-9172-ee924e352e3a.png)
     
   - Change some adapter information
   
     Go to `Control Panel->Network and Internet->Network Connections` and right click on your  `WAN Miniport(IKEv2)`, than go to `Security` tab and change:
     
     Data encryption: Maximum strength encryption(disconnect if server declines)
     Authentication: Use machine certificates
     
   [ ![adapter](https://user-images.githubusercontent.com/44748406/193277220-06488326-35f9-41f7-8efb-48187f949caf.png)](https://github.com/GabrielL92I/IKEv2-IPsec-on-MikroTik/issues/1#issuecomment-1263558262)
    
     After this you will have a successfully VPN connection over IKEv2/IPsec using certificates.
   [ ![connected](https://user-images.githubusercontent.com/44748406/193277636-71f6c456-9956-44f7-8168-2d682fb70121.png)](https://github.com/GabrielL92I/IKEv2-IPsec-on-MikroTik/issues/1#issuecomment-1263560632)
     
     Peer connected proof
   [ ![conrouter](https://user-images.githubusercontent.com/44748406/193278006-0e9fc93a-1226-4d81-98cb-6b180288cca8.png)](https://github.com/GabrielL92I/IKEv2-IPsec-on-MikroTik/issues/1#issuecomment-1263563414)
     
     Thank you.
