---
title: Azure Dosya Eşitleme şirket içi güvenlik duvarı ve proxy ayarları | Microsoft Dokümanlar
description: Azure Dosya Eşitleme şirket içi ağ yapılandırması
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 7f398012edc25ba6a04e230fa8049e7264f857bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294517"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Azure Dosya Eşitleme proxy’si ve güvenli duvarı ayarları
Azure Dosya Eşitlemesi, şirket içi sunucularınızı Azure Dosyaları'na bağlayarak çok siteli eşitleme ve bulut katmanlama özellikleri sağlar. Bu nedenle, şirket içi sunucu internete bağlı olmalıdır. Bir BT yöneticisinin, sunucunun Azure bulut hizmetlerine ulaşması için en iyi yolu belirlemesi gerekir.

Bu makale, sunucunuzu Azure Dosya Eşitlemasına başarılı ve güvenli bir şekilde bağlamak için kullanılabilen belirli gereksinimler ve seçenekler hakkında bilgi sağlar.

## <a name="overview"></a>Genel Bakış
Azure Dosya Eşitlemesi, eşitleme grubunuzda açıklandığı gibi verileri eşitlemek için Windows Server'ınız, Azure dosya paylaşımınız ve diğer bazı Azure hizmetleri arasında bir düzenleme hizmeti görevi görür. Azure Dosya Eşitlemi'nin doğru çalışması için sunucularınızı aşağıdaki Azure hizmetleriyle iletişim kuracak şekilde yapılandırmanız gerekir:

- Azure Storage
- Azure Dosya Eşitleme
- Azure Resource Manager
- Kimlik doğrulama hizmetleri

> [!Note]  
> Windows Server'daki Azure Dosya Eşitleme aracısı, bulut hizmetlerine yönelik tüm istekleri başlatır ve bu da yalnızca giden trafiği güvenlik duvarı açısından dikkate almak zorunda kalmayla sonuçlanır. <br /> Hiçbir Azure hizmeti Azure Dosya Eşitleme aracısına bağlantı başlatmaz.

## <a name="ports"></a>Bağlantı Noktaları
Azure Dosya Eşitlemi dosya verilerini ve meta verileri yalnızca HTTPS üzerinden taşır ve bağlantı noktası 443'ün giden açık olmasını gerektirir.
Sonuç olarak tüm trafik şifrelenir.

## <a name="networks-and-special-connections-to-azure"></a>Azure'a ağlar ve özel bağlantılar
Azure Dosya Eşitleme aracısının Azure'a [ExpressRoute](../../expressroute/expressroute-introduction.md), vb. gibi özel kanallarla ilgili gereksinimleri yoktur.

Azure Dosya Eşitlemi, Azure'a erişime izin veren, bant genişliği, gecikme sonu gibi çeşitli ağ özelliklerine otomatik olarak uyum sağlayarak ve ince ayar için yönetici denetimi sunan tüm yollarla çalışır. Şu anda tüm özellikler kullanılamıyor. Belirli bir davranışı yapılandırmak istiyorsanız, Azure [Files UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670)aracılığıyla bize bildirin.

## <a name="proxy"></a>Ara sunucu
Azure Dosya Eşitlemi, uygulamaya özgü ve makine genelinde proxy ayarlarını destekler.

**Uygulamaya özel proxy ayarları,** Azure Dosya Eşitleme trafiği için özel olarak bir proxy yapılandırması sağlar. Uygulamaya özel proxy ayarları aracı sürüm 4.0.1.0 veya daha yeni olarak desteklenir ve aracı yüklemesi sırasında veya Set-StorageSyncProxyConfiguration PowerShell cmdlet kullanılarak yapılandırılabilir.

PowerShell, uygulamaya özgü proxy ayarlarını yapılandırmakomutları:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
Sunucunun tüm trafiği proxy üzerinden yönlendirilir gibi **makine genelinde proxy ayarları** Azure Dosya Eşitleme aracısı için saydamdır.

Makine genelinde proxy ayarlarını yapılandırmak için aşağıdaki adımları izleyin: 

1. .NET uygulamaları için proxy ayarlarını yapılandırma 

   - Bu iki dosyayı edin:  
     C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
     C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

   - machine.config dosyalarındaki <system.net> bölümünü ekleyin (<system.serviceModel> bölümünün altında).  Proxy sunucusunun IP adresi ve bağlantı noktası olarak 127.0.01:8888'i değiştirin. 
     ```
      <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
          <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
      </system.net>
     ```

2. WinHTTP proxy ayarlarını ayarlama 

   - Varolan proxy ayarını görmek için yükseltilmiş bir komut isteminden veya PowerShell'den aşağıdaki komutu çalıştırın:   

     netsh winhttp göster proxy

   - Proxy ayarını ayarlamak için yükseltilmiş bir komut istemiveya PowerShell'den aşağıdaki komutu çalıştırın (proxy sunucusunun IP adresi ve bağlantı noktasına 127.0.01:8888'i değiştirin):  

     netsh winhttp set proxy 127.0.0.1:8888

3. Aşağıdaki komutu yükseltilmiş bir komut isteminden veya PowerShell'den çalıştırarak Depolama Eşitleme Aracısı hizmetini yeniden başlatın: 

      net stop filesyncsvc

      Not: Depolama Eşitleme Aracısı (filesyncsvc) hizmeti durdurulduktan sonra otomatik olarak başlatılacaktır.

## <a name="firewall"></a>Güvenlik duvarı
Bir önceki bölümde belirtildiği gibi, bağlantı noktası 443 giden açık olması gerekir. Veri merkezinizdeki, şubenizdeki veya bölgenizdeki ilkelere bağlı olarak, bu bağlantı noktası üzerindeki trafiği belirli etki alanlarıyla daha da kısıtlamak istenebilir veya gerekli olabilir.

Aşağıdaki tabloda iletişim için gerekli etki alanları açıklanmaktadır:

| Hizmet | Genel bulut bitiş noktası | Azure Kamu bitiş noktası | Kullanım |
|---------|----------------|---------------|------------------------------|
| **Azure Resource Manager** | `https://management.azure.com` | https://management.usgovcloudapi.net | Herhangi bir kullanıcı çağrısı (PowerShell gibi) ilk sunucu kayıt çağrısı da dahil olmak üzere bu URL'ye gider/aracılığıyla. |
| **Azure Active Directory** | https://login.windows.net<br>`https://login.microsoftonline.com` | https://login.microsoftonline.us | Azure Kaynak Yöneticisi aramaları kimlik doğrulaması yapılmış bir kullanıcı tarafından yapılmalıdır. Başarılı olmak için bu URL kullanıcı kimlik doğrulaması için kullanılır. |
| **Azure Active Directory** | https://graph.microsoft.com/ | https://graph.microsoft.com/ | Azure Dosya Eşitle'sini dağıtmanın bir parçası olarak, aboneliğin Azure Etkin Dizini'nde bir hizmet yöneticisi oluşturulur. Bu URL bunun için kullanılır. Bu anabilim, en az hak kümesini Azure Dosya Eşitleme hizmetine vermek için kullanılır. Azure Dosya Eşitlemesi'nin ilk kurulumlarını gerçekleştiren kullanıcı, abonelik sahibi ayrıcalıklarına sahip kimlik doğrulaması yapılan bir kullanıcı olmalıdır. |
| **Azure Depolama** | &ast;.core.windows.net | &ast;.core.usgovcloudapi.net | Sunucu bir dosya indirdiğinde, sunucu depolama hesabındaki Azure dosya paylaşımıyla doğrudan konuşurken bu veri hareketini daha verimli bir şekilde gerçekleştirir. Sunucu, yalnızca hedeflenen dosya paylaşımı erişimine izin veren bir SAS anahtarına sahiptir. |
| **Azure Dosya Eşitleme** | &ast;.one.microsoft.com<br>&ast;.afs.azure.net | &ast;.afs.azure.us | İlk sunucu kaydından sonra sunucu, o bölgedeki Azure Dosya Eşitleme hizmeti örneği için bölgesel bir URL alır. Sunucu, eşitleme işlemlerini işleyen örnekle doğrudan ve verimli bir şekilde iletişim kurmak için URL'yi kullanabilir. |
| **Microsoft PKI** | https://www.microsoft.com/pki/mscorp/cps<br><http://ocsp.msocsp.com> | https://www.microsoft.com/pki/mscorp/cps<br><http://ocsp.msocsp.com> | Azure Dosya Eşitleme aracısı yüklendikten sonra, Azure Dosya Eşitleme hizmeti ve Azure dosya paylaşımıyla iletişim kurmak için gereken ara sertifikaları indirmek için PKI URL'si kullanılır. OCSP URL, sertifikanın durumunu denetlemek için kullanılır. |

> [!Important]
> Trafiğin .one.microsoft.com'a &ast;izin verirken, sunucudan eşitleme hizmetinden daha fazlasına trafik vermek mümkündür. Alt etki alanları altında çok daha fazla Microsoft hizmeti mevcuttur.

&ast;.one.microsoft.com çok genişse, iletişimin Yalnızca Azure Dosyaları Eşitleme hizmetinin açık bölgesel örnekleriyle sınırlandırılmasına izin vererek sunucunun iletişimini sınırlandırabilirsiniz. Hangi örneği seçeceğiniz, sunucuyu dağıttığınız ve kaydettiğiniz depolama eşitleme hizmetinin bölgesine bağlıdır. Bu bölge aşağıdaki tabloda "Birincil uç nokta URL" olarak adlandırılır.

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) nedenleriyle Azure dosya paylaşımlarınızı genel olarak gereksiz (GRS) bir depolama hesabında belirtmiş olabilirsiniz. Bu durumda, kalıcı bir bölgesel kesinti durumunda Azure dosya paylaşımlarınız eşleştirilmiş bölgeye geçmezse. Azure Dosya Eşitlemi depolama yla aynı bölgesel eşleştirmeleri kullanır. Bu nedenle, GRS depolama hesapları kullanıyorsanız, sunucunuzun Azure Dosya Eşitlemi için eşleştirilmiş bölgeyle konuşmasına izin vermek için ek URL'leri etkinleştirmeniz gerekir. Aşağıdaki tabloda buna "Eşleştirilmiş bölge" denir. Ayrıca, etkinleştirilmesi gereken bir trafik yöneticisi profil URL'si de vardır. Bu, ağ trafiğinin, bir arıza olması durumunda eşleştirilmiş bölgeye sorunsuz bir şekilde yeniden yönlendirilmesini sağlar ve aşağıdaki tabloda "Bulma URL'si" olarak adlandırılır.

| Bulut  | Bölge | Birincil bitiş noktası URL'si | Eşleştirilmiş bölge | Bulma URL'si |
|--------|--------|----------------------|---------------|---------------|
| Genel |Doğu Avustralya | https:\//kailani-aue.one.microsoft.com | Güneydoğu Avustralya | https:\//tm-kailani-aue.one.microsoft.com |
| Genel |Güneydoğu Avustralya | https:\//kailani-aus.one.microsoft.com | Doğu Avustralya | https:\//tm-kailani-aus.one.microsoft.com |
| Genel | Güney Brezilya | https:\//brazilsouth01.afs.azure.net | Orta Güney ABD | https:\//tm-brazilsouth01.afs.azure.net |
| Genel | Orta Kanada | https:\//kailani-cac.one.microsoft.com | Doğu Kanada | https:\//tm-kailani-cac.one.microsoft.com |
| Genel | Doğu Kanada | https:\//kailani-cae.one.microsoft.com | Orta Kanada | https:\//tm-kailani.cae.one.microsoft.com |
| Genel | Orta Hindistan | https:\//kailani-cin.one.microsoft.com | Güney Hindistan | https:\//tm-kailani-cin.one.microsoft.com |
| Genel | Orta ABD | https:\//kailani-cus.one.microsoft.com | Doğu ABD 2 | https:\//tm-kailani-cus.one.microsoft.com |
| Genel | Doğu Asya | https:\//kailani11.one.microsoft.com | Güneydoğu Asya | https:\//tm-kailani11.one.microsoft.com |
| Genel | Doğu ABD | https:\//kailani1.one.microsoft.com | Batı ABD | https:\//tm-kailani1.one.microsoft.com |
| Genel | Doğu ABD 2 | https:\//kailani-ess.one.microsoft.com | Orta ABD | https:\//tm-kailani-ess.one.microsoft.com |
| Genel | Doğu Japonya | https:\//japaneast01.afs.azure.net | Batı Japonya | https:\//tm-japaneast01.afs.azure.net |
| Genel | Batı Japonya | https:\//japanwest01.afs.azure.net | Doğu Japonya | https:\//tm-japanwest01.afs.azure.net |
| Genel | Güney Kore - Orta | https:\//koreacentral01.afs.azure.net/ | Güney Kore - Güney | https:\//tm-koreacentral01.afs.azure.net/ |
| Genel | Güney Kore - Güney | https:\//koreasouth01.afs.azure.net/ | Güney Kore - Orta | https:\//tm-koreasouth01.afs.azure.net/ |
| Genel | Orta Kuzey ABD | https:\//northcentralus01.afs.azure.net | Orta Güney ABD | https:\//tm-northcentralus01.afs.azure.net |
| Genel | Kuzey Avrupa | https:\//kailani7.one.microsoft.com | Batı Avrupa | https:\//tm-kailani7.one.microsoft.com |
| Genel | Orta Güney ABD | https:\//southcentralus01.afs.azure.net | Orta Kuzey ABD | https:\//tm-southcentralus01.afs.azure.net |
| Genel | Güney Hindistan | https:\//kailani-sin.one.microsoft.com | Orta Hindistan | https:\//tm-kailani-sin.one.microsoft.com |
| Genel | Güneydoğu Asya | https:\//kailani10.one.microsoft.com | Doğu Asya | https:\//tm-kailani10.one.microsoft.com |
| Genel | Güney Birleşik Krallık | https:\//kailani-uks.one.microsoft.com | Batı Birleşik Krallık | https:\//tm-kailani-uks.one.microsoft.com |
| Genel | Batı Birleşik Krallık | https:\//kailani-ukw.one.microsoft.com | Güney Birleşik Krallık | https:\//tm-kailani-ukw.one.microsoft.com |
| Genel | Orta Batı ABD | https:\//westcentralus01.afs.azure.net | Batı ABD 2 | https:\//tm-westcentralus01.afs.azure.net |
| Genel | Batı Avrupa | https:\//kailani6.one.microsoft.com | Kuzey Avrupa | https:\//tm-kailani6.one.microsoft.com |
| Genel | Batı ABD | https:\//kailani.one.microsoft.com | Doğu ABD | https:\//tm-kailani.one.microsoft.com |
| Genel | Batı ABD 2 | https:\//westus201.afs.azure.net | Orta Batı ABD | https:\//tm-westus201.afs.azure.net |
| Devlet | US Gov Arizona | https:\//usgovarizona01.afs.azure.us | US Gov Texas | https:\//tm-usgovarizona01.afs.azure.us |
| Devlet | US Gov Texas | https:\//usgovtexas01.afs.azure.us | US Gov Arizona | https:\//tm-usgovtexas01.afs.azure.us |

- Yerel olarak yedekli (LRS) veya bölge gereksiz (ZRS) depolama hesapları kullanıyorsanız, yalnızca "Birincil uç nokta URL" altında listelenen URL'yi etkinleştirmeniz gerekir.

- Genel olarak gereksiz (GRS) depolama hesapları kullanıyorsanız, üç URL etkinleştirin.

**Örnek:** Bir depolama eşitleme `"West US"` hizmeti dağıtarak sunucunuzu bu hizmetle kaydedebilirsiniz. Sunucunun bu durum için iletişim kurmasına izin veren URL'ler şunlardır:

> - https:\//kailani.one.microsoft.com (birincil bitiş noktası: Batı ABD)
> - https:\//kailani1.one.microsoft.com (eşleştirilmiş başarısız bölge: Doğu ABD)
> - https:\//tm-kailani.one.microsoft.com (birincil bölgenin bulma URL'si)

### <a name="allow-list-for-azure-file-sync-ip-addresses"></a>Azure Dosya Eşitleme IP adresleri için listeye izin ver
Azure Dosya Eşitlemi, belirli bir Azure hizmeti için bir grup IP adresi önekisini temsil eden [hizmet etiketlerinin](../../virtual-network/service-tags-overview.md)kullanımını destekler. Azure Dosya Eşitleme hizmetiyle iletişimi sağlayan güvenlik duvarı kuralları oluşturmak için hizmet etiketlerini kullanabilirsiniz. Azure Dosya Eşitlemi `StorageSyncService`için hizmet etiketi.

Azure içinde Azure Dosya Eşitlemini kullanıyorsanız, trafiğe izin vermek için doğrudan ağ güvenlik grubunuzda hizmet etiketinin adını kullanabilirsiniz. Bunu nasıl yapacağınız hakkında daha fazla bilgi edinmek için [Ağ güvenlik gruplarına](../../virtual-network/security-overview.md)bakın.

Azure Dosya Eşitlemeyi'ni şirket içinde kullanıyorsanız, güvenlik duvarınızın izin veren listesi için belirli IP adresi aralıkları almak için hizmet etiketi API'yi kullanabilirsiniz. Bu bilgileri almak için iki yöntem vardır:

- Hizmet etiketlerini destekleyen tüm Azure hizmetleri için geçerli IP adresi aralıkları listesi, Microsoft Download Center'da haftalık olarak JSON belgesi şeklinde yayınlanır. Her Azure bulutunun, bu bulutla ilgili IP adres aralıklarına sahip kendi JSON belgesi vardır:
    - [Azure Genel](https://www.microsoft.com/download/details.aspx?id=56519)
    - [Azure US Government](https://www.microsoft.com/download/details.aspx?id=57063)
    - [Azure Çin](https://www.microsoft.com/download/details.aspx?id=57062)
    - [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=57064)
- Hizmet etiketi bulma API (önizleme) hizmet etiketleri geçerli listesini programlı alma sağlar. Önizlemede, hizmet etiketi bulma API'si, Microsoft İndirme Merkezi'nde yayınlanan JSON belgelerinden döndürülen bilgilerden daha az güncel bilgileri döndürebilir. OTOMASYON tercihinize göre API yüzeyini kullanabilirsiniz:
    - [REST API](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
    - [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag)
    - [Azure CLI](https://docs.microsoft.com/cli/azure/network#az-network-list-service-tags)

Hizmet etiketi bulma API'si Microsoft İndirme Merkezi'nde yayınlanan JSON belgeleri kadar sık güncelleştirilmedığından, şirket içi güvenlik duvarınızın izin veren listesini güncelleştirmek için JSON belgesini kullanmanızı öneririz. Bu aşağıdaki gibi yapılabilir:

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

Ardından, güvenlik duvarınızı `$ipAddressRanges` güncelleştirmek için IP adresi aralıklarını kullanabilirsiniz. Güvenlik duvarınızı nasıl güncelleştirebildiğini öğrenmek için güvenlik duvarınızı/ağ cihazınızın web sitesini kontrol edin.

## <a name="test-network-connectivity-to-service-endpoints"></a>Hizmet bitiş noktalarına ağ bağlantısını test etme
Bir sunucu Azure Dosya Eşitleme hizmetine kaydolduktan sonra, Test-StorageSyncNetworkConnectivity cmdlet ve ServerRegistration.exe, bu sunucuya özgü tüm uç noktaları (URL'ler) ile iletişimi test etmek için kullanılabilir. Bu cmdlet, eksik iletişim sunucunun Azure Dosya Eşitlemesi ile tam olarak çalışmasını engellediğinde sorun gidermeye yardımcı olabilir ve proxy ve güvenlik duvarı yapılandırmalarında ince ayar yapmak için kullanılabilir.

Ağ bağlantısı testini çalıştırmak için Azure Dosya Eşitleme aracısı sürümü 9.1 veya sonraki sürümlerini yükleyin ve aşağıdaki PowerShell komutlarını çalıştırın:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Test-StorageSyncNetworkConnectivity
```

## <a name="summary-and-risk-limitation"></a>Özet ve risk sınırlaması
Bu belgenin önceki listeleri, şu anda iletişim halinde olan URL'leri Azure Dosya Eşitlemesini içerir. Güvenlik duvarları, bu etki adlarına giden trafiğe izin verebilmelidir. Microsoft bu listeyi güncel tutmaya çalışır.

Güvenlik duvarı kurallarını kısıtlayan etki alanının ayarlanması, güvenliği artırmak için bir önlem olabilir. Bu güvenlik duvarı yapılandırmaları kullanılırsa, URL'lerin ekolacağını ve hatta zaman içinde değişebileceğini unutmamak gerekir. Bu makaleyi düzenli olarak denetleyin.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
