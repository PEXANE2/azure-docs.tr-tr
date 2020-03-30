---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/12/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: feaf72de1d2c578d2b2d0df9e86ec0fbe0b49445
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371792"
---
Bir aygıt tünelinin başarılı bir şekilde oluşturulabilmesi için aşağıdaki gereksinimlerin karşılanması gerekir:

* Aygıt, Windows 10 Enterprise veya Education sürüm 1809 veya daha sonra çalıştıran bir etki alanı birleştirilmiş bilgisayar olmalıdır.
* Tünel yalnızca Windows dahili VPN çözümü için yapılandırılabilir ve bilgisayar sertifikası kimlik doğrulaması ile IKEv2 kullanılarak kurulmuştur.
* Aygıt başına yalnızca bir aygıt tüneli yapılandırılabilir.

1. Site noktası [VPN istemcisi](../articles/vpn-gateway/point-to-site-how-to-vpn-client-install-azure-cert.md) makalesini kullanarak istemci sertifikalarını Windows 10 istemcisine yükleyin. Sertifikanın Yerel Makine mağazasında olması gerekir.
1. Vpn Profili oluşturun ve [bu yönergeleri](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config#vpn-device-tunnel-configuration)kullanarak YEREL SİsteM hesabı bağlamında aygıt tünelini yapılandırın.

### <a name="configuration-example-for-device-tunnel"></a>Aygıt tüneli için yapılandırma örneği

Sanal ağ ağ ağ geçidini yapılandırdıktan ve istemci sertifikasını Windows 10 istemcisine Yerel Makine deposuna yükledikten sonra, istemci aygıt tünelini yapılandırmak için aşağıdaki örnekleri kullanın:

1. Aşağıdaki metni kopyalayın ve ***devicecert.ps1***olarak kaydedin.

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
1. Aşağıdaki metni kopyalayın ve **devicecert.ps1**ile aynı klasöre ***VPNProfile.xml*** olarak kaydedin. Ortamınızla eşleşecek şekilde aşağıdaki metni edin.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers> <= Can be found in the VpnSettings.xml in the downloaded profile zip file`
   * `<Address>192.168.3.5</Address> <= IP of resource in the vnet or the vnet address space`
   * `<Address>192.168.3.4</Address> <= IP of resource in the vnet or the vnet address space`

   ```
   <VPNProfile>  
     <NativeProfile>  
   <Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  
   <NativeProtocolType>IKEv2</NativeProtocolType>  
   <Authentication>  
     <MachineMethod>Certificate</MachineMethod>  
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
   <!-- need to specify always on = true --> 
     <AlwaysOn>true</AlwaysOn> 
   <!-- new node to specify that this is a device tunnel -->  
    <DeviceTunnel>true</DeviceTunnel>
   <!--new node to register client IP address in DNS to enable manage out -->
   <RegisterDNS>true</RegisterDNS>
   </VPNProfile>
   ```
1. [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) **gelen PsExec** indirin ve **C:\PSTools**için dosyaları ayıklayın.
1. Bir Admin CMD isteminden PowerShell'i çalıştırarak başlatın:

   ```
   PsExec.exe Powershell for 32-bit Windows
   PsExec64.exe Powershell for 64-bit Windows
   ```

   ![powershell](./media/vpn-gateway-vwan-always-on-device/powershell.png)
1. PowerShell'de **devicecert.ps1** ve **VPNProfile.xml'in** bulunduğu klasöre geçin ve aşağıdaki komutu çalıştırın:

   ```powershell
   .\devicecert.ps1 .\VPNProfile.xml MachineCertTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-vwan-always-on-device/machinecerttest.png)
1. **Rasphone**çalıştırın.

   ![rasphone](./media/vpn-gateway-vwan-always-on-device/rasphone.png)
1. **MachineCertTest** girişini arayın ve **Bağlan'ı**tıklatın.

   ![Bağlan](./media/vpn-gateway-vwan-always-on-device/connect.png)
1. Bağlantı başarılı olursa, bilgisayarı yeniden başlatın. Tünel otomatik olarak bağlanır.
