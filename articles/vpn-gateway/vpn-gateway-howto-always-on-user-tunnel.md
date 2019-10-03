---
title: VPN Gateway için her zaman açık VPN tüneli yapılandırma
description: VPN Gateway için Always on Kullanıcı VPN tünelini yapılandırma adımları
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: cherylmc
ms.openlocfilehash: dc0abf12c60f845fde0d16bd874a1436aef3b7ab
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71846478"
---
# <a name="configure-an-always-on-vpn-user-tunnel"></a>Always on VPN Kullanıcı tüneli yapılandırma

Windows 10 sanal özel ağ (VPN) istemcisinin yeni özelliklerinden biri, VPN bağlantısını sürdürme olanağıdır. Her zaman açık, etkin VPN profilinin otomatik olarak bağlanmasını ve tetikleyicilere göre bağlı kalmasını sağlayan bir Windows 10 özelliğidir; yani Kullanıcı oturum açma, ağ durumu değişikliği veya cihaz ekranı etkin olur.

Azure sanal ağ geçitleri, kalıcı Kullanıcı tünellerinin yanı sıra Azure 'a yönelik cihaz tünellerini oluşturmak için Windows 10 her zaman açık ile kullanılabilir. Bu makale, her zaman açık VPN Kullanıcı tüneli yapılandırmanıza yardımcı olur.

Her zaman VPN bağlantılarında iki tür tünel vardır:

* **Cihaz tüneli** , kullanıcılar cihazda oturum açmadan önce belirtilen VPN sunucularına bağlanır. Oturum açma öncesi bağlantı senaryoları ve cihaz yönetimi amaçları cihaz tüneli kullanır.

* **Kullanıcı tüneli** yalnızca, bir Kullanıcı cihazda oturum açtıktan sonra bağlanır. Kullanıcı tüneli, kullanıcıların VPN sunucuları aracılığıyla kuruluş kaynaklarına erişmesini sağlar.

Hem cihaz tüneli hem de Kullanıcı tüneli VPN profilleriyle bağımsız olarak çalışır. Bunlar aynı anda birbirine bağlanabilir ve uygun şekilde farklı kimlik doğrulama yöntemleri ve diğer VPN yapılandırma ayarlarını kullanabilir.

## <a name="1-configure-the-gateway"></a>1. ağ geçidini yapılandırma

VPN ağ geçidini, bu [Noktadan siteye bu makaleyi](vpn-gateway-howto-point-to-site-resource-manager-portal.md)kullanarak Ikev2 ve sertifika tabanlı kimlik doğrulaması kullanacak şekilde yapılandırın.

## <a name="2-configure-the-user-tunnel"></a>2. Kullanıcı tüneli yapılandırma

1. İstemci sertifikalarını, bu [noktadan sıteye VPN istemci makalesinde](point-to-site-how-to-vpn-client-install-azure-cert.md)gösterildiği gibi Windows 10 istemcisine yükler. Sertifikanın geçerli kullanıcı deposunda olması gerekir
2. [Bu yönergeleri](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)kullanarak her zaman on VPN istemcisini POWERSHELL, SCCM veya Intune aracılığıyla yapılandırın.

### <a name="configuration-example-for-user-tunnel"></a>Kullanıcı tüneli için yapılandırma örneği

Sanal ağ geçidini yapılandırdıktan ve istemci sertifikasını Windows 10 istemcisindeki yerel makine deposuna yükledikten sonra, bir istemci cihaz tüneli yapılandırmak için aşağıdaki örnekleri kullanın.

1. Aşağıdaki metni kopyalayın ve ***Usercert. ps1***olarak kaydedin.

   ```
   Param(
   [string]$xmlFilePath,
   [string]$ProfileName
   )

   $a = Test-Path $xmlFilePath
   echo $a

   $ProfileXML = Get-Content $xmlFilePath

   echo $XML

   $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

   $Version = 201606090004

   $ProfileXML = $ProfileXML -replace '<', '&lt;'
   $ProfileXML = $ProfileXML -replace '>', '&gt;'
   $ProfileXML = $ProfileXML -replace '"', '&quot;'

   $nodeCSPURI = './Vendor/MSFT/VPNv2'
   $namespaceName = "root\cimv2\mdm\dmmap"
   $className = "MDM_VPNv2_01"

   $session = New-CimSession

   try
   {
   $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
   $newInstance.CimInstanceProperties.Add($property)

   $session.CreateInstance($namespaceName, $newInstance)
   $Message = "Created $ProfileName profile."
   Write-Host "$Message"
   }
   catch [Exception]
   {
   $Message = "Unable to create $ProfileName profile: $_"
   Write-Host "$Message"
   exit
   }
   $Message = "Complete."
   Write-Host "$Message"
   ```
1. Aşağıdaki metni kopyalayın ve ***Vpnprofile. xml*** olarak **Usercert. ps1**ile aynı klasöre kaydedin. Ortamınıza uyması için aşağıdaki metni düzenleyin.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
    <VPNProfile>  
      <NativeProfile>  
    <Servers>azuregateway-b115055e-0882-49bc-a9b9-7de45cba12c0-8e6946892333.vpn.azure.com</Servers>  
    <NativeProtocolType>IKEv2</NativeProtocolType>  
    <Authentication>  
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>
    <EapHostConfig xmlns="http://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="http://www.microsoft.com/provisioning/EapCommon">13</Type><VendorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="http://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="http://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>false</DisableUserPromptForServerValidation><ServerNames></ServerNames></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</PerformServerValidation><AcceptServerName xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName></EapType></Eap></Config></EapHostConfig>
    </Configuration>
    </Eap>
    </Authentication>  
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
     <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
    <DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
      </NativeProfile> 
      <!-- use host routes(/32) to prevent routing conflicts -->  
      <Route>  
    <Address>192.168.3.5</Address>  
    <PrefixSize>32</PrefixSize>  
      </Route>  
      <Route>  
    <Address>192.168.3.4</Address>  
    <PrefixSize>32</PrefixSize>  
      </Route>  
    <!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
      <TrafficFilter>  
    <RemoteAddressRanges>192.168.3.4, 192.168.3.5</RemoteAddressRanges>  
      </TrafficFilter>
    <!-- need to specify always on = true --> 
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <!--new node to register client IP address in DNS to enable manage out -->
    <RegisterDNS>true</RegisterDNS>
    </VPNProfile>
   ```
1. PowerShell 'i yönetici olarak çalıştırın.

1. PowerShell 'de, **Usercert. ps1** ve **vpnprofile. xml** ' nin bulunduğu klasöre geçin ve aşağıdaki komutu çalıştırın:

   ```powershell
   C:\> .\usercert.ps1 .\VPNProfile.xml UserTest
   ```
   
   ![Machineccerttest](./media/vpn-gateway-howto-always-on-user-tunnel/p2s2.jpg)
1. VPN ayarları bölümüne bakın.

1. **Usertest** girişini bulun ve **Bağlan**' a tıklayın.

1. Bağlantı başarılı olursa, her zaman açık Kullanıcı tünelini başarıyla yapılandırdınız.

## <a name="cleanup"></a>Temizleme

Profili kaldırmak için aşağıdaki komutu çalıştırın:

1. Bağlantının bağlantısını kesin ve "otomatik olarak bağlan" seçeneğinin işaretini kaldırın

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

![Temizleme](./media/vpn-gateway-howto-always-on-user-tunnel/p2s4..jpg)

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme için bkz. [Azure Noktadan siteye bağlantı sorunları](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
