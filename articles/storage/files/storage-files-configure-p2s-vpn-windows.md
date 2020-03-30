---
title: Azure Dosyaları ile kullanılmak üzere Windows'da Bir Noktaya Sayfa (P2S) VPN'i yapılandırma | Microsoft Dokümanlar
description: Azure Dosyaları ile kullanılmak üzere Windows'da Bir Sayfa-To-Site (P2S) VPN yapılandırma
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 5f12b77f5baa1a3b06a093aac7267c65a038881e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80061009"
---
# <a name="configure-a-point-to-site-p2s-vpn-on-windows-for-use-with-azure-files"></a>Azure Dosyaları ile kullanılmak üzere Windows'da Bir Noktaya Sayfa (P2S) VPN'i yapılandırma
445 bağlantı noktasını açmadan Azure dosya paylaşımlarınızı SMB üzerinden SMB'ye monte etmek için Bir Yerden Kullanıma (P2S) VPN bağlantısını kullanabilirsiniz. Bir Noktadan Siteye VPN bağlantısı, Azure ile tek bir istemci arasındaki VPN bağlantısıdır. Azure Dosyaları ile Bir P2S VPN bağlantısı kullanmak için, bağlanmak isteyen her istemci için bir P2S VPN bağlantısının yapılandırılması gerekir. Şirket içi ağınızdan Azure dosya paylaşımlarınıza bağlanması gereken çok sayıda istemciniz varsa, her istemci için Site'den Siteye (S2S) VPN bağlantısı yerine kullanabilirsiniz. Daha fazla bilgi için Bkz. [Azure Dosyaları ile kullanılmak üzere Siteden Siteye VPN Yapılandır'](storage-files-configure-s2s-vpn.md)a bakın.

Azure Dosyaları için kullanılabilir ağ seçeneklerinin tam olarak tartışılması için makaleye devam etmeden önce doğrudan Azure dosyası paylaşımı erişimi için Ağ la ilgili [hususları](storage-files-networking-overview.md) okumanızı öneririz.

Makalede, Windows'ta (Windows istemcisi ve Windows Server) Bir Noktaya Sayfa VPN'i doğrudan şirket içinde Azure dosya paylaşımlarına monte etmek üzere yapılandırma adımları ayrıntılı olarak açıklanmaktadır. Azure Dosya Eşitleme trafiğini VPN üzerinden yönlendirmek istiyorsanız, lütfen [Azure Dosya Eşitleme proxy'sini ve güvenlik duvarı ayarlarını yapılandırmaya](storage-sync-files-firewall-and-proxy.md)bakın.

## <a name="prerequisites"></a>Ön koşullar
- Azure PowerShell modülünün en son sürümü. Azure PowerShell'i nasıl yükleyin hakkında daha fazla bilgi için Azure [PowerShell modülünü yükleyin](https://docs.microsoft.com/powershell/azure/install-az-ps) ve işletim sisteminizi seçin. Windows'da Azure CLI'yi kullanmayı tercih ederseniz, aşağıdaki yönergeler Azure PowerShell için sunulabilir.

- Şirket içinde monte etmek istediğiniz bir Azure dosya paylaşımı. Azure dosya paylaşımları, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları veya kuyruklar gibi diğer depolama kaynaklarını dağıtabileceğiniz paylaşılan bir depolama havuzunun yanı sıra yönetim yapıları olan depolama hesaplarında dağıtılır. Azure dosya paylaşımı oluştur'da Azure dosya paylaşımlarını ve depolama hesaplarını nasıl dağıtabileceğiniz hakkında daha fazla bilgi [edinebilirsiniz.](storage-how-to-create-file-share.md)

- Şirket içinde monte etmek istediğiniz Azure dosya paylaşımını içeren depolama hesabı için özel bir bitiş noktası. Özel bir bitiş noktası oluşturma hakkında daha fazla bilgi edinmek için Azure [Dosyaları ağ uç noktalarını yapılandırma](storage-files-networking-endpoints.md?tabs=azure-powershell)konusuna bakın. 

## <a name="deploy-a-virtual-network"></a>Sanal ağ dağıtma
Azure dosya paylaşımınıza ve diğer Azure kaynaklarına şirket içi bir Giriş VPN üzerinden erişmek için bir sanal ağ veya VNet oluşturmanız gerekir. Otomatik olarak oluşturacağınız P2S VPN bağlantısı, şirket içi Windows makineniz ile bu Azure sanal ağı arasında bir köprüdür.

Aşağıdaki PowerShell üç alt ağa sahip bir Azure sanal ağı oluşturur: biri depolama hesabınızın hizmet bitiş noktası için, diğeri depolama hesabınızın özel bitiş noktası için, depolama hesabına şirket içinde oluşturmadan erişmek için gereklidir değişebilecek depolama hesabının genel IP'si ve VPN hizmetini sağlayan sanal ağ ağ ağ geçidiniz için özel yönlendirme. 

Çevreniz `<region>`için `<resource-group>`uygun `<desired-vnet-name>` değerleri değiştirmeyi ve değiştirmeyi unutmayın.

```PowerShell
$region = "<region>"
$resourceGroupName = "<resource-group>" 
$virtualNetworkName = "<desired-vnet-name>"

$virtualNetwork = New-AzVirtualNetwork `
    -ResourceGroupName $resourceGroupName `
    -Name $virtualNetworkName `
    -Location $region `
    -AddressPrefix "192.168.0.0/16"

Add-AzVirtualNetworkSubnetConfig `
    -Name "ServiceEndpointSubnet" `
    -AddressPrefix "192.168.0.0/24" `
    -VirtualNetwork $virtualNetwork `
    -ServiceEndpoint "Microsoft.Storage" `
    -WarningAction SilentlyContinue | Out-Null

Add-AzVirtualNetworkSubnetConfig `
    -Name "PrivateEndpointSubnet" `
    -AddressPrefix "192.168.1.0/24" `
    -VirtualNetwork $virtualNetwork `
    -WarningAction SilentlyContinue | Out-Null

Add-AzVirtualNetworkSubnetConfig `
    -Name "GatewaySubnet" `
    -AddressPrefix "192.168.2.0/24" `
    -VirtualNetwork $virtualNetwork `
    -WarningAction SilentlyContinue | Out-Null

$virtualNetwork | Set-AzVirtualNetwork | Out-Null
$virtualNetwork = Get-AzVirtualNetwork `
    -ResourceGroupName $resourceGroupName `
    -Name $virtualNetworkName

$serviceEndpointSubnet = $virtualNetwork.Subnets | `
    Where-Object { $_.Name -eq "ServiceEndpointSubnet" }
$privateEndpointSubnet = $virtualNetwork.Subnets | `
    Where-Object { $_.Name -eq "PrivateEndpointSubnet" }
$gatewaySubnet = $virtualNetwork.Subnets | ` 
    Where-Object { $_.Name -eq "GatewaySubnet" }
```

## <a name="create-root-certificate-for-vpn-authentication"></a>VPN kimlik doğrulaması için kök sertifika oluşturma
Şirket içi Windows makinelerinizden gelen VPN bağlantılarının sanal ağınıza erişmek için kimlik doğrulaması yapabilmesi için iki sertifika oluşturmanız gerekir: sanal makine ağ geçidine sağlanacak bir kök sertifika ve istemci sertifikası, kök sertifikası ile imzalanabilir. Aşağıdaki PowerShell kök sertifikasını oluşturur; istemci sertifikası, Azure sanal ağ ağ ağ geçidiağdan gelen bilgilerle oluşturulduktan sonra oluşturulur. 

```PowerShell
$rootcertname = "CN=P2SRootCert"
$certLocation = "Cert:\CurrentUser\My"
$vpnTemp = "C:\vpn-temp\"
$exportedencodedrootcertpath = $vpnTemp + "P2SRootCertencoded.cer"
$exportedrootcertpath = $vpnTemp + "P2SRootCert.cer"

if (-Not (Test-Path $vpnTemp)) {
    New-Item -ItemType Directory -Force -Path $vpnTemp | Out-Null
}

if ($PSVersionTable.PSVersion -ge [System.Version]::new(6, 0)) {
    Install-Module WindowsCompatibility
    Import-WinModule PKI
}

$rootcert = New-SelfSignedCertificate `
    -Type Custom `
    -KeySpec Signature `
    -Subject $rootcertname `
    -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 `
    -KeyLength 2048 `
    -CertStoreLocation $certLocation `
    -KeyUsageProperty Sign `
    -KeyUsage CertSign

Export-Certificate `
    -Cert $rootcert `
    -FilePath $exportedencodedrootcertpath `
    -NoClobber | Out-Null

certutil -encode $exportedencodedrootcertpath $exportedrootcertpath | Out-Null

$rawRootCertificate = Get-Content -Path $exportedrootcertpath

[System.String]$rootCertificate = ""
foreach($line in $rawRootCertificate) { 
    if ($line -notlike "*Certificate*") { 
        $rootCertificate += $line 
    } 
}
```

## <a name="deploy-virtual-network-gateway"></a>Sanal ağ ağ geçidini dağıtma
Azure sanal ağ ağ ağ geçidi, şirket içi Windows makinelerinizin bağlanacak hizmetidir. Bu hizmeti dağıtmak iki temel bileşen gerektirir: müşterilerinizin dünyanın neresinde olurlarsa olsunlar için ağ geçidini tanımlayacak genel bir IP ve daha önce oluşturduğunuz ve müşterilerinizin kimliğini doğrulamak için kullanılacak bir kök sertifika.

Bu kaynaklar `<desired-vpn-name-here>` için istediğiniz adla değiştirmeyi unutmayın.

> [!Note]  
> Azure sanal ağ ağ ağ geçidini dağıtmak 45 dakika kadar sürebilir. Bu kaynak dağıtılırken, bu PowerShell komut dosyası dağıtımın tamamlanmasını engeller. Bu beklenen bir durumdur.

```PowerShell
$vpnName = "<desired-vpn-name-here>" 
$publicIpAddressName = "$vpnName-PublicIP"

$publicIPAddress = New-AzPublicIpAddress `
    -ResourceGroupName $resourceGroupName ` 
    -Name $publicIpAddressName `
    -Location $region `
    -Sku Basic `
    -AllocationMethod Dynamic

$gatewayIpConfig = New-AzVirtualNetworkGatewayIpConfig `
    -Name "vnetGatewayConfig" `
    -SubnetId $gatewaySubnet.Id `
    -PublicIpAddressId $publicIPAddress.Id

$azRootCertificate = New-AzVpnClientRootCertificate `
    -Name "P2SRootCert" `
    -PublicCertData $rootCertificate

$vpn = New-AzVirtualNetworkGateway `
    -ResourceGroupName $resourceGroupName `
    -Name $vpnName `
    -Location $region `
    -GatewaySku VpnGw1 `
    -GatewayType Vpn `
    -VpnType RouteBased `
    -IpConfigurations $gatewayIpConfig `
    -VpnClientAddressPool "172.16.201.0/24" `
    -VpnClientProtocol IkeV2 `
    -VpnClientRootCertificates $azRootCertificate
```

## <a name="create-client-certificate"></a>İstemci sertifikası oluşturma
İstemci sertifikası, sanal ağ ağ geçidinin URI'si ile oluşturulur. Bu sertifika, daha önce oluşturduğunuz kök sertifikası ile imzalanır.

```PowerShell
$clientcertpassword = "1234"

$vpnClientConfiguration = New-AzVpnClientConfiguration `
    -ResourceGroupName $resourceGroupName `
    -Name $vpnName `
    -AuthenticationMethod EAPTLS

Invoke-WebRequest `
    -Uri $vpnClientConfiguration.VpnProfileSASUrl `
    -OutFile "$vpnTemp\vpnclientconfiguration.zip"

Expand-Archive `
    -Path "$vpnTemp\vpnclientconfiguration.zip" `
    -DestinationPath "$vpnTemp\vpnclientconfiguration"

$vpnGeneric = "$vpnTemp\vpnclientconfiguration\Generic"
$vpnProfile = ([xml](Get-Content -Path "$vpnGeneric\VpnSettings.xml")).VpnProfile

$exportedclientcertpath = $vpnTemp + "P2SClientCert.pfx"
$clientcertname = "CN=" + $vpnProfile.VpnServer

$clientcert = New-SelfSignedCertificate `
    -Type Custom `
    -DnsName $vpnProfile.VpnServer `
    -KeySpec Signature `
    -Subject $clientcertname `
    -KeyExportPolicy Exportable `
    -HashAlgorithm sha256 `
    -KeyLength 2048 `
    -CertStoreLocation $certLocation `
    -Signer $rootcert `
    -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")

$mypwd = ConvertTo-SecureString -String $clientcertpassword -Force -AsPlainText

Export-PfxCertificate `
    -FilePath $exportedclientcertpath `
    -Password $mypwd `
    -Cert $clientcert | Out-Null
```

## <a name="configure-the-vpn-client"></a>VPN istemcisini yapılandırma
Azure sanal ağ ağ ağ geçidi, şirket içi Windows makinenizde VPN bağlantısını başlatması gereken yapılandırma dosyalarını içeren indirilebilir bir paket oluşturur. VPN bağlantısını Windows 10/Windows Server 2016+'nın [Her Zaman VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/) özelliğini kullanarak yapılandıracağız. Bu paket, istenirse eski Windows VPN istemcisini yapılandıracak yürütülebilir paketler de içerir. Bu kılavuzda, eski Windows VPN istemcisi yerine Her Zaman VPN istemcisi kullanır, çünkü son kullanıcıların makinelerine yönetici izni olmadan Azure VPN'den bağlanmalarına/bağlantılarını kesmelerine izin verir. 

Aşağıdaki komut dosyası sanal ağ ağ ağ geçidine karşı kimlik doğrulama için gerekli istemci sertifikasını yükler, vpn paketini karşıdan yükler ve yükler. Değiştirmeyi `<computer1>` ve `<computer2>` istediğiniz bilgisayarları değiştirmeyi unutmayın. `$sessions` Diziye daha fazla PowerShell oturumu ekleyerek bu komut dosyasını istediğiniz kadar makinede çalıştırabilirsiniz. Kullanım hesabınız bu makinelerin her birinde yönetici olmalıdır. Bu makinelerden biri komut dosyasını çalıştırdığınız yerel makineyse, komut dosyasını yükseltilmiş bir PowerShell oturumundan çalıştırmanız gerekir. 

```PowerShell
$sessions = [System.Management.Automation.Runspaces.PSSession[]]@()
$sessions += New-PSSession -ComputerName "<computer1>"
$sessions += New-PSSession -ComputerName "<computer2>"

foreach ($session in $sessions) {
    Invoke-Command -Session $session -ArgumentList $vpnTemp -ScriptBlock { 
        $vpnTemp = $args[0]
        if (-Not (Test-Path $vpnTemp)) {
            New-Item `
                -ItemType Directory `
                -Force `
                -Path "C:\vpn-temp" | Out-Null
        }
    }

    Copy-Item `
        -Path $exportedclientcertpath, $exportedrootcertpath, "$vpnTemp\vpnclientconfiguration.zip" `
        -Destination $vpnTemp `
        -ToSession $session

    Invoke-Command `
        -Session $session `
        -ArgumentList `
            $mypwd, `
            $vpnTemp, `
            $virtualNetworkName
        -ScriptBlock { 
            $mypwd = $args[0] 
            $vpnTemp = $args[1]
            $virtualNetworkName = $args[2]

            Import-PfxCertificate `
                -Exportable `
                -Password $mypwd `
                -CertStoreLocation "Cert:\LocalMachine\My" `
                -FilePath "$vpnTemp\P2SClientCert.pfx" | Out-Null

            Import-Certificate `
                -FilePath "$vpnTemp\P2SRootCert.cer" `
                -CertStoreLocation "Cert:\LocalMachine\Root" | Out-Null

            Expand-Archive `
                -Path "$vpnTemp\vpnclientconfiguration.zip" `
                -DestinationPath "$vpnTemp\vpnclientconfiguration"
            $vpnGeneric = "$vpnTemp\vpnclientconfiguration\Generic"

            $vpnProfile = ([xml](Get-Content -Path "$vpnGeneric\VpnSettings.xml")).VpnProfile

            Add-VpnConnection `
                -Name $virtualNetworkName `
                -ServerAddress $vpnProfile.VpnServer ` 
                -TunnelType Ikev2 `
                -EncryptionLevel Required `
                -AuthenticationMethod MachineCertificate `
                -SplitTunneling `
                -AllUserConnection

            Add-VpnConnectionRoute `
                -Name $virtualNetworkName `
                -DestinationPrefix $vpnProfile.Routes `
                -AllUserConnection

            Add-VpnConnectionRoute `
                -Name $virtualNetworkName `
                -DestinationPrefix $vpnProfile.VpnClientAddressPool `
                -AllUserConnection

            rasdial $virtualNetworkName
        }
}

Remove-Item -Path $vpnTemp -Recurse
```

## <a name="mount-azure-file-share"></a>Azure dosya paylaşımını başlat
Artık Sayfadan Siteye VPN'inizi kurduğunuza göre, Azure dosya paylaşımını PowerShell üzerinden kurduğunuz bilgisayarlara monte etmek için kullanabilirsiniz. Aşağıdaki örnek, payı monte edecek, payın aslında monte olduğunu kanıtlamak için payın kök dizinini listeleyecek ve paylaşımın söküleceğini listeleyecek. Ne yazık ki, PowerShell remoting üzerinde ısrarla pay monte etmek mümkün değildir. Kalıcı olarak monte etmek için bkz. [Windows ile Azure dosya paylaşımı kullan.](storage-how-to-use-files-windows.md) 

```PowerShell
$myShareToMount = "<file-share>"

$storageAccountKeys = Get-AzStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $storageAccountName
$storageAccountKey = ConvertTo-SecureString `
    -String $storageAccountKeys[0].Value `
    -AsPlainText `
    -Force

$nic = Get-AzNetworkInterface -ResourceId $privateEndpoint.NetworkInterfaces[0].Id
$storageAccountPrivateIP = $nic.IpConfigurations[0].PrivateIpAddress

Invoke-Command `
    -Session $sessions `
    -ArgumentList  `
        $storageAccountName, `
        $storageAccountKey, `
        $storageAccountPrivateIP, `
        $myShareToMount `
    -ScriptBlock {
        $storageAccountName = $args[0]
        $storageAccountKey = $args[1]
        $storageAccountPrivateIP = $args[2]
        $myShareToMount = $args[3]

        $credential = [System.Management.Automation.PSCredential]::new(
            "AZURE\$storageAccountName", 
            $storageAccountKey)

        New-PSDrive `
            -Name Z `
            -PSProvider FileSystem `
            -Root "\\$storageAccountPrivateIP\$myShareToMount" `
            -Credential $credential `
            -Persist | Out-Null
        Get-ChildItem -Path Z:\
        Remove-PSDrive -Name Z
    }
```

## <a name="see-also"></a>Ayrıca bkz.
- [Doğrudan Azure dosya paylaşımı erişimi için ağ la ilgili hususlar](storage-files-networking-overview.md)
- [Azure Dosyaları ile kullanılmak üzere Linux'ta Bir Noktadan Siteye (P2S) VPN yapılandırın](storage-files-configure-p2s-vpn-linux.md)
- [Azure Dosyaları ile kullanılmak üzere Siteden Siteye (S2S) VPN yapılandırın](storage-files-configure-s2s-vpn.md)
