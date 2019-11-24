
#Setup Site to Site VPN with Azure and Cisco ASA IOS version 9.8 and below

Azure has multiple configuration recipes when comes to establishing Site to Site VPN tunnel with ASA. They have sample ASA configuration outlined on their web site but speaking from my own experience, it did not help at all. Despite having the exact same configuration which was provided by Azure, we kept getting the following error:

KEv2-PROTO-5: (211): Failed to verify the proposed policies  
IKEv2-PROTO-1: (211): Failed to find a matching policy  
IKEv2-PROTO-1: (211): Received Policies:  
IKEv2-PROTO-1: (211): Failed to find a matching policy  
IKEv2-PROTO-1: (211): Expected Policies:  
IKEv2-PROTO-1: (211): Failed to find a matching policy

We spent hours in troubleshooting the problem and finally came up with a working configuration after spending a couple of hours with Azure support. I thought to share the solution with others facing the same challenge.

If the Cisco IOS version is below 9.8, you are required to setup a custom ipsec policy and enable Traffic Selector on Virtual Network Gateway Connection in Azure using PowerShell. You can follow the instructions in this document to find the PowerShell cmdlets [https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell)

Below is the configuration that worked for us on both sides:

**Cisco ASA:**

crypto ikev2 policy 1  
encryption aes-256  
integrity sha384  
prf sha384  
group 24  
lifetime seconds 86400

Phase 2,

encryption aes-256  
integrity sha-256 sha-1  
lifetime seconds 27000  
lifetime kilobytes 102400000

## Azure:

IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup None -SALifeTimeSeconds 27000 -SADataSizeKilobytes 102400000

All the best!