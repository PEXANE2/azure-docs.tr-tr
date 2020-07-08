---
title: Şirket içi güvenlik duvarı ve proxy ayarlarını Azure Dosya Eşitleme | Microsoft Docs
description: Şirket içi ağ yapılandırması Azure Dosya Eşitleme
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 06/24/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 7410e30c892eb083f9ed71b1d9ce379ae9a036b5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85515286"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Azure Dosya Eşitleme proxy’si ve güvenli duvarı ayarları
Azure Dosya Eşitleme, şirket içi sunucularınızı Azure dosyalarına bağlayarak çok siteli eşitlemeyi ve bulut katmanlama özelliklerini etkinleştirir. Bu nedenle, bir şirket içi sunucu internet 'e bağlı olmalıdır. BT yöneticisinin, sunucunun Azure Cloud Services 'e ulaşması için en iyi yolu karar vermesini gerektirir.

Bu makale, başarıyla sunulan belirli gereksinimlere ve seçeneklere ilişkin öngörüler sağlar ve sunucunuzu Azure Dosya Eşitleme için güvenli bir şekilde bağlayın.

Bu nasıl yapılır Kılavuzu ' nu okumadan önce [Azure dosya eşitleme ağ konularını](storage-sync-files-networking-overview.md) okumanızı öneririz.

## <a name="overview"></a>Genel Bakış
Azure Dosya Eşitleme, Windows sunucunuz, Azure dosya paylaşımınız ve diğer birçok Azure hizmeti arasında, eşitleme grubunuzda açıklandığı gibi verileri eşitlemek için bir Orchestration hizmeti görevi görür. Azure Dosya Eşitleme düzgün şekilde çalışması için sunucularınızı aşağıdaki Azure hizmetleriyle iletişim kuracak şekilde yapılandırmanız gerekecektir:

- Azure Storage
- Azure Dosya Eşitleme
- Azure Resource Manager
- Kimlik doğrulama hizmetleri

> [!Note]  
> Windows Server 'daki Azure Dosya Eşitleme Aracısı, bulut hizmetlerine yönelik tüm istekleri başlatır ve yalnızca bir güvenlik duvarı perspektifinden giden trafiği göz önünde bulundurmasına neden olur. <br /> Azure hizmeti Azure Dosya Eşitleme aracısıyla bir bağlantı başlatır.

## <a name="ports"></a>Bağlantı noktaları
Azure Dosya Eşitleme dosya verilerini ve meta verileri HTTPS üzerinden özel olarak taşımalıdır ve 443 numaralı bağlantı noktasını açık giden şekilde gerektirir.
Sonuç olarak tüm trafik şifrelenir.

## <a name="networks-and-special-connections-to-azure"></a>Azure ile ağlar ve özel bağlantılar
Azure Dosya Eşitleme aracısının [ExpressRoute](../../expressroute/expressroute-introduction.md)vb. gibi özel kanallarla ilgili hiçbir gereksinimi yoktur.

Azure Dosya Eşitleme, Azure 'a erişime izin veren ve bant genişliği, gecikme süresi gibi çeşitli ağ özelliklerine otomatik olarak uyum sağlayan ve ince ayar için yönetici denetimi sunan tüm yollarla çalışacaktır. Şu anda tüm özellikler kullanılamaz. Belirli davranışı yapılandırmak isterseniz, [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670)aracılığıyla bize bilgi verin.

## <a name="proxy"></a>Ara sunucu
Azure Dosya Eşitleme, uygulamaya özgü ve makine genelindeki proxy ayarlarını destekler.

**Uygulamaya özgü ara sunucu ayarları** , bir proxy 'nin özel olarak Azure dosya eşitleme trafiği yapılandırmasına izin verir. Uygulamaya özgü ara sunucu ayarları, aracı sürümü 4.0.1.0 veya daha yeni bir sürümde desteklenir ve aracı yüklemesi sırasında veya set-StorageSyncProxyConfiguration PowerShell cmdlet 'i kullanılarak yapılandırılabilir.

uygulamaya özgü ara sunucu ayarlarını yapılandırmaya yönelik PowerShell komutlarıyla yapılandırılabilir:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
Sunucunun tüm trafiği proxy üzerinden yönlendirilirken, **makine genelindeki proxy ayarları** Azure dosya eşitleme aracısına saydamdır.

Makine genelindeki ara sunucu ayarlarını yapılandırmak için aşağıdaki adımları izleyin: 

1. .NET uygulamaları için proxy ayarlarını yapılandırma 

   - Şu iki dosyayı düzenleyin:  
     C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
     C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

   - machine.config dosyalarına <system.net> bölümünü ekleyin (<System. serviceModel> bölümünün altında).  127.0.01:8888 öğesini, proxy sunucu için IP adresine ve bağlantı noktasına değiştirin. 
     ```
      <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
          <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
      </system.net>
     ```

2. WinHTTP proxy ayarlarını ayarla 

   - Mevcut proxy ayarını görmek için, yükseltilmiş bir komut isteminden veya PowerShell 'den aşağıdaki komutu çalıştırın:   

     Netsh WinHTTP proxy göster

   - Proxy ayarını ayarlamak için yükseltilmiş bir komut isteminden veya PowerShell 'den aşağıdaki komutu çalıştırın (127.0.01:8888 öğesini ara sunucu için IP adresini ve bağlantı noktasını değiştirin):  

     Netsh WinHTTP set proxy 127.0.0.1:8888

3. Yükseltilmiş bir komut isteminden veya PowerShell 'ten aşağıdaki komutu çalıştırarak depolama eşitleme Aracısı hizmetini yeniden başlatın: 

      net stop filesyncsvc

      Note: depolama eşitleme Aracısı (filesyncsvc) hizmeti durdurulduktan sonra otomatik olarak başlayacak.

## <a name="firewall"></a>Güvenlik Duvarı
Önceki bölümde belirtildiği gibi, 443 numaralı bağlantı noktasının giden trafik açık olması gerekir. Veri merkezinizdeki, dalınızdaki veya bölgenizdeki ilkelere bağlı olarak, bu bağlantı noktası üzerinden trafiği belirli etki alanlarına kısıtlamak istenebilir veya gerekli olabilir.

Aşağıdaki tabloda iletişim için gerekli etki alanları açıklanmaktadır:

| Hizmet | Genel bulut uç noktası | Azure Kamu uç noktası | Kullanım |
|---------|----------------|---------------|------------------------------|
| **Azure Resource Manager** | `https://management.azure.com` | https://management.usgovcloudapi.net | Herhangi bir kullanıcı çağrısı (PowerShell gibi), ilk sunucu kayıt çağrısı da dahil olmak üzere bu URL 'ye gider. |
| **Azure Active Directory** | https://login.windows.net<br>`https://login.microsoftonline.com` | https://login.microsoftonline.us | Azure Resource Manager çağrılarının kimliği doğrulanmış bir kullanıcı tarafından yapılması gerekir. Başarılı olmak için, bu URL kullanıcı kimlik doğrulaması için kullanılır. |
| **Azure Active Directory** | https://graph.microsoft.com/ | https://graph.microsoft.com/ | Azure Dosya Eşitleme dağıtmanın bir parçası olarak, aboneliğin Azure Active Directory bir hizmet sorumlusu oluşturulur. Bu URL bunun için kullanılır. Bu asıl, Azure Dosya Eşitleme hizmetine en düşük düzeyde haklar vermek için kullanılır. Azure Dosya Eşitleme ilk kurulumunu gerçekleştiren kullanıcı, abonelik sahibi ayrıcalıklarına sahip kimliği doğrulanmış bir kullanıcı olmalıdır. |
| **Azure Active Directory** | https://secure.aadcdn.microsoftonline-p.com | Genel uç nokta URL 'sini kullanın. | Bu URL 'ye, Azure Dosya Eşitleme sunucusu kayıt Kullanıcı arabiriminin yönetici 'de oturum açmak için kullandığı Active Directory kimlik doğrulaması kitaplığı tarafından erişilir. |
| **Azure Depolama** | &ast;. core.windows.net | &ast;. core.usgovcloudapi.net | Sunucu bir dosyayı indirdiğinde, sunucu bu veri hareketini depolama hesabındaki Azure dosya paylaşımından doğrudan görüşüp daha verimli bir şekilde gerçekleştirir. Sunucuda yalnızca hedeflenen dosya paylaşımında erişime izin veren bir SAS anahtarı vardır. |
| **Azure Dosya Eşitleme** | &ast;. one.microsoft.com<br>&ast;. afs.azure.net | &ast;. afs.azure.us | İlk sunucu kaydından sonra, sunucu, bu bölgedeki Azure Dosya Eşitleme hizmet örneği için bölgesel bir URL alır. Sunucu, eşitlemesini işleme örneği ile doğrudan ve verimli bir şekilde iletişim kurmak için URL 'YI kullanabilir. |
| **Microsoft PKI** | https://www.microsoft.com/pki/mscorp/cps<br><http://ocsp.msocsp.com> | https://www.microsoft.com/pki/mscorp/cps<br><http://ocsp.msocsp.com> | Azure Dosya Eşitleme aracısı yüklendikten sonra, Azure Dosya Eşitleme hizmeti ve Azure dosya paylaşımıyla iletişim kurmak için gereken ara sertifikaları indirmek için PKI URL 'SI kullanılır. Bir sertifikanın durumunu denetlemek için OCSP URL 'SI kullanılır. |

> [!Important]
> . Afs.azure.net trafiğine izin verirken &ast; trafik yalnızca eşitleme hizmeti için mümkündür. Bu etki alanını kullanan başka bir Microsoft hizmeti yok.
> . One.microsoft.com trafiğine izin verirken &ast; sunucudan yalnızca eşitleme hizmetinden daha fazlasına trafik sağlanır. Alt etki alanları altında çok daha fazla Microsoft hizmeti mevcuttur.

&ast;. AFS.Azure.NET veya &ast; . One.Microsoft.com çok geniş Ise, Azure dosya eşitleme hizmeti 'nin yalnızca açık bölgesel örneklerine iletişime izin vererek sunucunun iletişimini sınırlayabilirsiniz. Hangi örnek seçme, dağıttığınız ve sunucusuna kaydettiğiniz depolama eşitleme hizmeti bölgesine bağlıdır. Bu bölgeye aşağıdaki tabloda "birincil uç nokta URL 'SI" adı verilir.

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) nedenleriyle Azure dosya paylaşımlarınızı küresel olarak yedekli (GRS) depolama hesabında belirtmiş olabilirsiniz. Böyle bir durum söz konusu ise, Azure dosya paylaşımlarınız, coğrafi bölge kesintisi durumunda eşleştirilmiş bölgeye yük devreder. Azure Dosya Eşitleme depolama ile aynı bölgesel eşleştirmeleri kullanır. Bu nedenle, GRS depolama hesapları kullanıyorsanız, sunucunuzun Azure Dosya Eşitleme eşleştirilmiş bölge ile iletişim kurmasına izin vermek için ek URL 'Ler etkinleştirmeniz gerekir. Aşağıdaki tablo bu "eşleştirilmiş bölgeyi" çağırır. Ayrıca, aynı zamanda etkinleştirilmesi gereken bir Traffic Manager profil URL 'SI vardır. Bu, ağ trafiğinin yük devretme olayında eşleştirilmiş bölgeye sorunsuz bir şekilde yeniden yönlendirilmesini ve aşağıdaki tabloda "keşif URL 'SI" olarak adlandırılmasına olanak sağlar.

| Bulut  | Bölge | Birincil uç nokta URL 'SI | Eşleştirilmiş bölge | Bulma URL 'SI |
|--------|--------|----------------------|---------------|---------------|
| Ortak |Doğu Avustralya | https: \/ /australiaeast01.AFS.Azure.net<br>https: \/ /Kailani-Aue.One.Microsoft.com | Güneydoğu Avustralya | https: \/ /TM-australiaeast01.AFS.Azure.net<br>https: \/ /TM-Kailani-Aue.One.Microsoft.com |
| Ortak |Güneydoğu Avustralya | https: \/ /australiasoutheast01.AFS.Azure.net<br>https: \/ /Kailani-aus.One.Microsoft.com | Doğu Avustralya | https: \/ /TM-australiasoutheast01.AFS.Azure.net<br>https: \/ /TM-Kailani-aus.One.Microsoft.com |
| Ortak | Güney Brezilya | https: \/ /brazilsouth01.AFS.Azure.net | Orta Güney ABD | https: \/ /TM-brazilsouth01.AFS.Azure.net |
| Ortak | Orta Kanada | https: \/ /canadacentral01.AFS.Azure.net<br>https: \/ /Kailani-CAC.One.Microsoft.com | Doğu Kanada | https: \/ /TM-canadacentral01.AFS.Azure.net<br>https: \/ /TM-Kailani-CAC.One.Microsoft.com |
| Ortak | Doğu Kanada | https: \/ /canadaeast01.AFS.Azure.net<br>https: \/ /Kailani-CAE.One.Microsoft.com | Orta Kanada | https: \/ /TM-canadaeast01.AFS.Azure.net<br>https: \/ /TM-Kailani.CAE.One.Microsoft.com |
| Ortak | Orta Hindistan | https: \/ /centralindia01.AFS.Azure.net<br>https: \/ /Kailani-cin.One.Microsoft.com | Güney Hindistan | https: \/ /TM-centralindia01.AFS.Azure.net<br>https: \/ /TM-Kailani-cin.One.Microsoft.com |
| Ortak | Orta ABD | https: \/ /centralus01.AFS.Azure.net<br>https: \/ /Kailani-cus.One.Microsoft.com | Doğu ABD 2 | https: \/ /TM-centralus01.AFS.Azure.net<br>https: \/ /TM-Kailani-cus.One.Microsoft.com |
| Ortak | Doğu Asya | https: \/ /eastasia01.AFS.Azure.net<br>https: \/ /kailani11.One.Microsoft.com | Güneydoğu Asya | https: \/ /TM-eastasia01.AFS.Azure.net<br>https: \/ /TM-kailani11.One.Microsoft.com |
| Ortak | Doğu ABD | https: \/ /eastus01.AFS.Azure.net<br>https: \/ /kailani1.One.Microsoft.com | Batı ABD | https: \/ /TM-eastus01.AFS.Azure.net<br>https: \/ /TM-kailani1.One.Microsoft.com |
| Ortak | Doğu ABD 2 | https: \/ /eastus201.AFS.Azure.net<br>https: \/ /Kailani-ESS.One.Microsoft.com | Orta ABD | https: \/ /TM-eastus201.AFS.Azure.net<br>https: \/ /TM-Kailani-ESS.One.Microsoft.com |
| Ortak | Doğu Japonya | https: \/ /japaneast01.AFS.Azure.net | Batı Japonya | https: \/ /TM-japaneast01.AFS.Azure.net |
| Ortak | Batı Japonya | https: \/ /japanwest01.AFS.Azure.net | Doğu Japonya | https: \/ /TM-japanwest01.AFS.Azure.net |
| Ortak | Güney Kore - Orta | https: \/ /koreacentral01.AFS.Azure.net/ | Güney Kore - Güney | https: \/ /TM-koreacentral01.AFS.Azure.net/ |
| Ortak | Güney Kore - Güney | https: \/ /koreasouth01.AFS.Azure.net/ | Güney Kore - Orta | https: \/ /TM-koreasouth01.AFS.Azure.net/ |
| Ortak | Orta Kuzey ABD | https: \/ /northcentralus01.AFS.Azure.net | Orta Güney ABD | https: \/ /TM-northcentralus01.AFS.Azure.net |
| Ortak | Kuzey Avrupa | https: \/ /northeurope01.AFS.Azure.net<br>https: \/ /kailani7.One.Microsoft.com | Batı Avrupa | https: \/ /TM-northeurope01.AFS.Azure.net<br>https: \/ /TM-kailani7.One.Microsoft.com |
| Ortak | Orta Güney ABD | https: \/ /southcentralus01.AFS.Azure.net | Orta Kuzey ABD | https: \/ /TM-southcentralus01.AFS.Azure.net |
| Ortak | Güney Hindistan | https: \/ /southindia01.AFS.Azure.net<br>https: \/ /Kailani-sin.One.Microsoft.com | Orta Hindistan | https: \/ /TM-southindia01.AFS.Azure.net<br>https: \/ /TM-Kailani-sin.One.Microsoft.com |
| Ortak | Güneydoğu Asya | https: \/ /southeastasia01.AFS.Azure.net<br>https: \/ /kailani10.One.Microsoft.com | Doğu Asya | https: \/ /TM-southeastasia01.AFS.Azure.net<br>https: \/ /TM-kailani10.One.Microsoft.com |
| Ortak | Güney Birleşik Krallık | https: \/ /uksouth01.AFS.Azure.net<br>https: \/ /Kailani-UKS.One.Microsoft.com | Batı Birleşik Krallık | https: \/ /TM-uksouth01.AFS.Azure.net<br>https: \/ /TM-Kailani-UKS.One.Microsoft.com |
| Ortak | Batı Birleşik Krallık | https: \/ /ukwest01.AFS.Azure.net<br>https: \/ /Kailani-UKW.One.Microsoft.com | Güney Birleşik Krallık | https: \/ /TM-ukwest01.AFS.Azure.net<br>https: \/ /TM-Kailani-UKW.One.Microsoft.com |
| Ortak | Orta Batı ABD | https: \/ /westcentralus01.AFS.Azure.net | Batı ABD 2 | https: \/ /TM-westcentralus01.AFS.Azure.net |
| Ortak | Batı Avrupa | https: \/ /westeurope01.AFS.Azure.net<br>https: \/ /kailani6.One.Microsoft.com | Kuzey Avrupa | https: \/ /TM-westeurope01.AFS.Azure.net<br>https: \/ /TM-kailani6.One.Microsoft.com |
| Ortak | Batı ABD | https: \/ /westus01.AFS.Azure.net<br>https: \/ /Kailani.One.Microsoft.com | Doğu ABD | https: \/ /TM-westus01.AFS.Azure.net<br>https: \/ /TM-Kailani.One.Microsoft.com |
| Ortak | Batı ABD 2 | https: \/ /westus201.AFS.Azure.net | Orta Batı ABD | https: \/ /TM-westus201.AFS.Azure.net |
| Devlet | US Gov Arizona | https: \/ /usgovarizona01.AFS.Azure.us | US Gov Texas | https: \/ /TM-usgovarizona01.AFS.Azure.us |
| Devlet | US Gov Texas | https: \/ /usgovtexas01.AFS.Azure.us | US Gov Arizona | https: \/ /TM-usgovtexas01.AFS.Azure.us |

- Yerel olarak yedekli (LRS) veya bölge yedekli (ZRS) depolama hesapları kullanıyorsanız, yalnızca "birincil uç nokta URL 'SI" altında listelenen URL 'YI etkinleştirmeniz gerekir.

- Küresel olarak yedekli (GRS) depolama hesapları kullanıyorsanız, üç URL 'yi etkinleştirin.

**Örnek:** ' De bir depolama eşitleme hizmeti dağıtıp `"West US"` sunucunuza kayıt yaptırın. Sunucunun bu durum için iletişim kurmasına izin veren URL 'Ler şunlardır:

> - https: \/ /westus01.AFS.Azure.net (birincil uç nokta: Batı ABD)
> - https: \/ /eastus01.AFS.Azure.net (eşlenen başarısız bölge: Doğu ABD)
> - https: \/ /TM-westus01.AFS.Azure.net (birincil bölgenin bulma URL 'si)

### <a name="allow-list-for-azure-file-sync-ip-addresses"></a>Azure Dosya Eşitleme IP adresleri listesine izin ver
Azure Dosya Eşitleme, belirli bir Azure hizmeti için bir IP adresi önekleri grubunu temsil eden [hizmet etiketlerinin](../../virtual-network/service-tags-overview.md)kullanımını destekler. Hizmet etiketlerini, Azure Dosya Eşitleme hizmetiyle iletişimi etkinleştiren güvenlik duvarı kuralları oluşturmak için kullanabilirsiniz. Azure Dosya Eşitleme için hizmet etiketi `StorageSyncService` .

Azure içinde Azure Dosya Eşitleme kullanıyorsanız, trafiğe izin vermek için hizmet etiketinin adını doğrudan ağ güvenlik grubunuzda kullanabilirsiniz. Bunun nasıl yapılacağı hakkında daha fazla bilgi edinmek için bkz. [ağ güvenlik grupları](../../virtual-network/security-overview.md).

Şirket içi Azure Dosya Eşitleme kullanıyorsanız, güvenlik duvarınızın izin verilenler listenize belirli IP adresi aralıklarını almak için hizmet etiketi API 'sini kullanabilirsiniz. Bu bilgileri almak için iki yöntem vardır:

- Hizmet etiketlerini destekleyen tüm Azure hizmetleri için IP adresi aralıklarının geçerli listesi, haftalık olarak Microsoft Indirme merkezi 'nde bir JSON belgesi biçiminde yayımlanır. Her Azure bulutu, bu bulut için uygun olan IP adresi aralıklarına sahip kendi JSON belgesine sahiptir:
    - [Azure genel](https://www.microsoft.com/download/details.aspx?id=56519)
    - [Azure ABD Kamu](https://www.microsoft.com/download/details.aspx?id=57063)
    - [Azure Çin](https://www.microsoft.com/download/details.aspx?id=57062)
    - [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=57064)
- Hizmet etiketi bulma API 'SI (Önizleme), geçerli hizmet etiketleri listesinin programlı olarak alınmasına izin verir. Önizleme aşamasında hizmet etiketi bulma API 'SI, Microsoft Indirme Merkezi ' nde yayınlanan JSON belgelerinden daha az güncel bilgiler döndürebilir. Otomasyon tercihinizi temel alarak API yüzeyini kullanabilirsiniz:
    - [REST API](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
    - [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag)
    - [Azure CLI](https://docs.microsoft.com/cli/azure/network#az-network-list-service-tags)

Hizmet etiketi bulma API 'SI, Microsoft Indirme merkezi 'nde yayınlanan JSON belgeleri kadar sık güncelleştirilemediğinden, şirket içi güvenlik duvarınızın izin verilenler listesini güncelleştirmek için JSON belgesini kullanmanızı öneririz. Bu, aşağıdaki gibi yapılabilir:

```PowerShell
# The specific region to get the IP address ranges for. Replace westus2 with the desired region code 
# from Get-AzLocation.
$region = "westus2"

# The service tag for Azure File Sync. Do not change unless you're adapting this
# script for another service.
$serviceTag = "StorageSyncService"

# Download date is the string matching the JSON document on the Download Center. 
$possibleDownloadDates = 0..7 | `
    ForEach-Object { [System.DateTime]::Now.AddDays($_ * -1).ToString("yyyyMMdd") }

# Verify the provided region
$validRegions = Get-AzLocation | `
    Where-Object { $_.Providers -contains "Microsoft.StorageSync" } | `
    Select-Object -ExpandProperty Location

if ($validRegions -notcontains $region) {
    Write-Error `
            -Message "The specified region $region is not available. Either Azure File Sync is not deployed there or the region does not exist." `
            -ErrorAction Stop
}

# Get the Azure cloud. This should automatically based on the context of 
# your Az PowerShell login, however if you manually need to populate, you can find
# the correct values using Get-AzEnvironment.
$azureCloud = Get-AzContext | `
    Select-Object -ExpandProperty Environment | `
    Select-Object -ExpandProperty Name

# Build the download URI
$downloadUris = @()
switch($azureCloud) {
    "AzureCloud" { 
        $downloadUris = $possibleDownloadDates | ForEach-Object {  
            "https://download.microsoft.com/download/7/1/D/71D86715-5596-4529-9B13-DA13A5DE5B63/ServiceTags_Public_$_.json"
        }
    }

    "AzureUSGovernment" {
        $downloadUris = $possibleDownloadDates | ForEach-Object { 
            "https://download.microsoft.com/download/6/4/D/64DB03BF-895B-4173-A8B1-BA4AD5D4DF22/ServiceTags_AzureGovernment_$_.json"
        }
    }

    "AzureChinaCloud" {
        $downloadUris = $possibleDownloadDates | ForEach-Object { 
            "https://download.microsoft.com/download/9/D/0/9D03B7E2-4B80-4BF3-9B91-DA8C7D3EE9F9/ServiceTags_China_$_.json"
        }
    }

    "AzureGermanCloud" {
        $downloadUris = $possibleDownloadDates | ForEach-Object { 
            "https://download.microsoft.com/download/0/7/6/076274AB-4B0B-4246-A422-4BAF1E03F974/ServiceTags_AzureGermany_$_.json"
        }
    }

    default {
        Write-Error -Message "Unrecognized Azure Cloud: $_" -ErrorAction Stop
    }
}

# Find most recent file
$found = $false 
foreach($downloadUri in $downloadUris) {
    try { $response = Invoke-WebRequest -Uri $downloadUri -UseBasicParsing } catch { }
    if ($response.StatusCode -eq 200) {
        $found = $true
        break
    }
}

if ($found) {
    # Get the raw JSON 
    $content = [System.Text.Encoding]::UTF8.GetString($response.Content)

    # Parse the JSON
    $serviceTags = ConvertFrom-Json -InputObject $content -Depth 100

    # Get the specific $ipAddressRanges
    $ipAddressRanges = $serviceTags | `
        Select-Object -ExpandProperty values | `
        Where-Object { $_.id -eq "$serviceTag.$region" } | `
        Select-Object -ExpandProperty properties | `
        Select-Object -ExpandProperty addressPrefixes
} else {
    # If the file cannot be found, that means there hasn't been an update in
    # more than a week. Please verify the download URIs are still accurate
    # by checking https://docs.microsoft.com/azure/virtual-network/service-tags-overview
    Write-Verbose -Message "JSON service tag file not found."
    return
}
```

Daha sonra `$ipAddressRanges` Güvenlik duvarınızı güncelleştirmek için IÇINDEKI IP adresi aralıklarını kullanabilirsiniz. Güvenlik duvarınızı güncelleştirme hakkında bilgi edinmek için güvenlik duvarı/ağ gerecinizin Web sitesini denetleyin.

## <a name="test-network-connectivity-to-service-endpoints"></a>Hizmet uç noktalarına ağ bağlantısını test etme
Bir sunucu Azure Dosya Eşitleme hizmetine kaydedildikten sonra, test-StorageSyncNetworkConnectivity cmdlet 'i ve ServerRegistration.exe bu sunucuya özgü tüm uç noktalar (URL) ile iletişimleri test etmek için kullanılabilir. Bu cmdlet, tamamlanmamış iletişim, sunucunun Azure Dosya Eşitleme ile tamamen çalışmasını engellediğinde ve proxy ve güvenlik duvarı yapılandırmalarının ince ayar yapmak için kullanılabilir olduğunda sorun gidermeye yardımcı olabilir.

Ağ bağlantısı testini çalıştırmak için, Azure Dosya Eşitleme Aracısı sürüm 9,1 veya üstünü yükledikten sonra aşağıdaki PowerShell komutlarını çalıştırın:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Test-StorageSyncNetworkConnectivity
```

## <a name="summary-and-risk-limitation"></a>Özet ve risk sınırlaması
Bu belgede daha önce bulunan listeler, şu anda iletişim kuran Azure Dosya Eşitleme URL 'Leri içerir. Güvenlik duvarlarının bu etki alanlarına giden trafiğe izin alabilmesi gerekir. Microsoft bu listeyi güncel tutmaya devam eden bir çaba harcar.

Etki alanı sınırlandırma güvenlik duvarı kurallarını ayarlamak, güvenliği artırmak için bir ölçü olabilir. Bu güvenlik duvarı yapılandırmalarının kullanılması durumunda, URL 'Lerin ekleneceğini ve zaman içinde değişebileceğini göz önünde bulundurmanız gerekir. Bu makaleye düzenli olarak bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
