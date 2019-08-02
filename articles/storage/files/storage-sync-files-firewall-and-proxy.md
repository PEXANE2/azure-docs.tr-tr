---
title: Şirket içi güvenlik duvarı ve proxy ayarlarını Azure Dosya Eşitleme | Microsoft Docs
description: Şirket içi ağ yapılandırması Azure Dosya Eşitleme
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 69f4c767b9fc1da90db021ffb3eb8704983ca69b
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699297"
---
# <a name="azure-file-sync-proxy-and-firewall-settings"></a>Azure Dosya Eşitleme proxy’si ve güvenli duvarı ayarları
Azure Dosya Eşitleme, şirket içi sunucularınızı Azure dosyalarına bağlayarak çok siteli eşitlemeyi ve bulut katmanlama özelliklerini etkinleştirir. Bu nedenle, bir şirket içi sunucu internet 'e bağlı olmalıdır. BT yöneticisinin, sunucunun Azure Cloud Services 'e ulaşması için en iyi yolu karar vermesini gerektirir.

Bu makale, başarıyla sunulan belirli gereksinimlere ve seçeneklere ilişkin öngörüler sağlar ve sunucunuzu Azure Dosya Eşitleme için güvenli bir şekilde bağlayın.

## <a name="overview"></a>Genel Bakış
Azure Dosya Eşitleme, Windows sunucunuz, Azure dosya paylaşımınız ve diğer birçok Azure hizmeti arasında, eşitleme grubunuzda açıklandığı gibi verileri eşitlemek için bir Orchestration hizmeti görevi görür. Azure Dosya Eşitleme düzgün şekilde çalışması için sunucularınızı aşağıdaki Azure hizmetleriyle iletişim kuracak şekilde yapılandırmanız gerekecektir:

- Azure Storage
- Azure Dosya Eşitleme
- Azure Resource Manager
- Kimlik doğrulama hizmetleri

> [!Note]  
> Windows Server 'daki Azure Dosya Eşitleme Aracısı, bulut hizmetlerine yönelik tüm istekleri başlatır ve yalnızca bir güvenlik duvarı perspektifinden giden trafiği göz önünde bulundurmasına neden olur. <br /> Azure hizmeti Azure Dosya Eşitleme aracısıyla bir bağlantı başlatır.

## <a name="ports"></a>Bağlantı Noktaları
Azure Dosya Eşitleme dosya verilerini ve meta verileri HTTPS üzerinden özel olarak taşımalıdır ve 443 numaralı bağlantı noktasını açık giden şekilde gerektirir.
Sonuç olarak tüm trafik şifrelenir.

## <a name="networks-and-special-connections-to-azure"></a>Azure ile ağlar ve özel bağlantılar
Azure Dosya Eşitleme aracısının [ExpressRoute](../../expressroute/expressroute-introduction.md)vb. gibi özel kanallarla ilgili hiçbir gereksinimi yoktur.

Azure Dosya Eşitleme, Azure 'a erişime izin veren ve bant genişliği, gecikme süresi gibi çeşitli ağ özelliklerine otomatik olarak uyum sağlayan ve ince ayar için yönetici denetimi sunan tüm yollarla çalışacaktır. Şu anda tüm özellikler kullanılamaz. Belirli davranışı yapılandırmak isterseniz, [Azure dosyaları UserVoice](https://feedback.azure.com/forums/217298-storage?category_id=180670)aracılığıyla bize bilgi verin.

## <a name="proxy"></a>Proxy
Azure Dosya Eşitleme, uygulamaya özgü ve makine genelindeki proxy ayarlarını destekler.

**Uygulamaya özgü ara sunucu ayarları** , bir proxy 'nin özel olarak Azure dosya eşitleme trafiği yapılandırmasına izin verir. Uygulamaya özgü ara sunucu ayarları, aracı sürümü 4.0.1.0 veya daha yeni bir sürümde desteklenir ve aracı yüklemesi sırasında veya set-StorageSyncProxyConfiguration PowerShell cmdlet 'i kullanılarak yapılandırılabilir.

Uygulamaya özgü ara sunucu ayarlarını yapılandırmak için PowerShell komutları:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Set-StorageSyncProxyConfiguration -Address <url> -Port <port number> -ProxyCredential <credentials>
```
Sunucunun tüm trafiği proxy üzerinden yönlendirilirken, **makine genelindeki proxy ayarları** Azure dosya eşitleme aracısına saydamdır.

Makine genelindeki proxy ayarlarını yapılandırmak için aşağıdaki adımları izleyin: 

1. .NET uygulamaları için proxy ayarlarını yapılandırma 

   - Şu iki dosyayı düzenleyin:  
     C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config  
     C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config

   - Machine. config dosyalarına < System. net > bölümünü ekleyin (< System. serviceModel > bölümünün altında).  127.0.01:8888 öğesini, proxy sunucu için IP adresine ve bağlantı noktasına değiştirin. 
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

      Not: Depolama eşitleme Aracısı (filesyncsvc) hizmeti durdurulduktan sonra otomatik olarak başlayacak.

## <a name="firewall"></a>Güvenlik Duvarı
Önceki bölümde belirtildiği gibi, 443 numaralı bağlantı noktasının giden trafik açık olması gerekir. Veri merkezinizdeki, dalınızdaki veya bölgenizdeki ilkelere bağlı olarak, bu bağlantı noktası üzerinden trafiği belirli etki alanlarına kısıtlamak istenebilir veya gerekli olabilir.

Aşağıdaki tabloda iletişim için gerekli etki alanları açıklanmaktadır:

| Hizmet | Genel bulut uç noktası | Azure Kamu uç noktası | Kullanım |
|---------|----------------|---------------|------------------------------|
| **Azure Resource Manager** | https://management.azure.com | https://management.usgovcloudapi.net | Herhangi bir kullanıcı çağrısı (PowerShell gibi), ilk sunucu kayıt çağrısı da dahil olmak üzere bu URL 'ye gider. |
| **Azure Active Directory** | https://login.windows.net | https://login.microsoftonline.us | Azure Resource Manager çağrılarının kimliği doğrulanmış bir kullanıcı tarafından yapılması gerekir. Başarılı olmak için, bu URL kullanıcı kimlik doğrulaması için kullanılır. |
| **Azure Active Directory** | https://graph.windows.net/ | https://graph.windows.net/ | Azure Dosya Eşitleme dağıtmanın bir parçası olarak, aboneliğin Azure Active Directory bir hizmet sorumlusu oluşturulur. Bu URL bunun için kullanılır. Bu asıl, Azure Dosya Eşitleme hizmetine en düşük düzeyde haklar vermek için kullanılır. Azure Dosya Eşitleme ilk kurulumunu gerçekleştiren kullanıcı, abonelik sahibi ayrıcalıklarına sahip kimliği doğrulanmış bir kullanıcı olmalıdır. |
| **Azure Depolama** | &ast;. core.windows.net | &ast;. core.usgovcloudapi.net | Sunucu bir dosyayı indirdiğinde, sunucu bu veri hareketini depolama hesabındaki Azure dosya paylaşımından doğrudan görüşüp daha verimli bir şekilde gerçekleştirir. Sunucuda yalnızca hedeflenen dosya paylaşımında erişime izin veren bir SAS anahtarı vardır. |
| **Azure Dosya Eşitleme** | &ast;.one.microsoft.com | &ast;. afs.azure.us | İlk sunucu kaydından sonra, sunucu, bu bölgedeki Azure Dosya Eşitleme hizmet örneği için bölgesel bir URL alır. Sunucu, eşitlemesini işleme örneği ile doğrudan ve verimli bir şekilde iletişim kurmak için URL 'YI kullanabilir. |
| **Microsoft PKI** | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | `https://www.microsoft.com/pki/mscorp`<br /><http://ocsp.msocsp.com> | Azure Dosya Eşitleme aracısı yüklendikten sonra, Azure Dosya Eşitleme hizmeti ve Azure dosya paylaşımıyla iletişim kurmak için gereken ara sertifikaları indirmek için PKI URL 'SI kullanılır. Bir sertifikanın durumunu denetlemek için OCSP URL 'SI kullanılır. |

> [!Important]
> &ast;. One.Microsoft.com trafiğine izin verirken sunucudan yalnızca eşitleme hizmetinden daha fazlasına trafik sağlanır. Alt etki alanları altında çok daha fazla Microsoft hizmeti mevcuttur.

&ast;. One.Microsoft.com çok geniş ise, Azure dosya eşitleme hizmeti 'nin yalnızca açık bölgesel örneklerine iletişime izin vererek sunucunun iletişimini sınırlayabilirsiniz. Hangi örnek seçme, dağıttığınız ve sunucusuna kaydettiğiniz depolama eşitleme hizmeti bölgesine bağlıdır. Bu bölgeye aşağıdaki tabloda "birincil uç nokta URL 'SI" adı verilir.

İş sürekliliği ve olağanüstü durum kurtarma (BCDR) nedenleriyle Azure dosya paylaşımlarınızı küresel olarak yedekli (GRS) depolama hesabında belirtmiş olabilirsiniz. Böyle bir durum söz konusu ise, Azure dosya paylaşımlarınız, coğrafi bölge kesintisi durumunda eşleştirilmiş bölgeye yük devreder. Azure Dosya Eşitleme depolama ile aynı bölgesel eşleştirmeleri kullanır. Bu nedenle, GRS depolama hesapları kullanıyorsanız, sunucunuzun Azure Dosya Eşitleme eşleştirilmiş bölge ile iletişim kurmasına izin vermek için ek URL 'Ler etkinleştirmeniz gerekir. Aşağıdaki tablo bu "eşleştirilmiş bölgeyi" çağırır. Ayrıca, aynı zamanda etkinleştirilmesi gereken bir Traffic Manager profil URL 'SI vardır. Bu, ağ trafiğinin yük devretme olayında eşleştirilmiş bölgeye sorunsuz bir şekilde yeniden yönlendirilmesini ve aşağıdaki tabloda "keşif URL 'SI" olarak adlandırılmasına olanak sağlar.

| Bulut  | Bölge | Birincil uç nokta URL 'SI | Eşleştirilmiş bölge | Bulma URL'si |
|--------|--------|----------------------|---------------|---------------|
| Genel |Avustralya Doğu | https:\//Kailani-Aue.One.Microsoft.com | Avustralya Güneydoğu | https:\//TM-Kailani-Aue.One.Microsoft.com |
| Genel |Avustralya Güneydoğu | https:\//Kailani-aus.One.Microsoft.com | Avustralya Doğu | https:\//TM-Kailani-aus.One.Microsoft.com |
| Genel | Güney Brezilya | https:\//brazilsouth01.AFS.Azure.net | Orta Güney ABD | https:\//TM-brazilsouth01.AFS.Azure.net |
| Genel | Orta Kanada | https:\//Kailani-CAC.One.Microsoft.com | Doğu Kanada | https:\//TM-Kailani-CAC.One.Microsoft.com |
| Genel | Doğu Kanada | https:\//Kailani-CAE.One.Microsoft.com | Orta Kanada | https:\//TM-Kailani.CAE.One.Microsoft.com |
| Genel | Orta Hindistan | https:\//Kailani-cin.One.Microsoft.com | Güney Hindistan | https:\//TM-Kailani-cin.One.Microsoft.com |
| Genel | Orta ABD | https:\//Kailani-cus.One.Microsoft.com | Doğu ABD 2 | https:\//TM-Kailani-cus.One.Microsoft.com |
| Genel | Doğu Asya | https:\//kailani11.One.Microsoft.com | Güneydoğu Asya | https:\//TM-kailani11.One.Microsoft.com |
| Genel | East US | https:\//kailani1.One.Microsoft.com | Batı ABD | https:\//TM-kailani1.One.Microsoft.com |
| Genel | Doğu ABD 2 | https:\//Kailani-ESS.One.Microsoft.com | Orta ABD | https:\//TM-Kailani-ESS.One.Microsoft.com |
| Genel | Japonya Doğu | https:\//japaneast01.AFS.Azure.net | Japonya Batı | https:\//TM-japaneast01.AFS.Azure.net |
| Genel | Japonya Batı | https:\//japanwest01.AFS.Azure.net | Japonya Doğu | https:\//TM-japanwest01.AFS.Azure.net |
| Genel | Kore Orta | https:\//koreacentral01.AFS.Azure.net/ | Kore Güney | https:\//TM-koreacentral01.AFS.Azure.net/ |
| Genel | Kore Güney | https:\//koreasouth01.AFS.Azure.net/ | Kore Orta | https:\//TM-koreasouth01.AFS.Azure.net/ |
| Genel | Orta Kuzey ABD | https:\//northcentralus01.AFS.Azure.net | Orta Güney ABD | https:\//TM-northcentralus01.AFS.Azure.net |
| Genel | Kuzey Avrupa | https:\//kailani7.One.Microsoft.com | Batı Avrupa | https:\//TM-kailani7.One.Microsoft.com |
| Genel | Orta Güney ABD | https:\//southcentralus01.AFS.Azure.net | Orta Kuzey ABD | https:\//TM-southcentralus01.AFS.Azure.net |
| Genel | Güney Hindistan | https:\//Kailani-sin.One.Microsoft.com | Orta Hindistan | https:\//TM-Kailani-sin.One.Microsoft.com |
| Genel | Güneydoğu Asya | https:\//kailani10.One.Microsoft.com | Doğu Asya | https:\//TM-kailani10.One.Microsoft.com |
| Genel | Birleşik Krallık Güney | https:\//Kailani-UKS.One.Microsoft.com | Birleşik Krallık Batı | https:\//TM-Kailani-UKS.One.Microsoft.com |
| Genel | Birleşik Krallık Batı | https:\//Kailani-UKW.One.Microsoft.com | Birleşik Krallık Güney | https:\//TM-Kailani-UKW.One.Microsoft.com |
| Genel | Batı Orta ABD | https:\//westcentralus01.AFS.Azure.net | Batı ABD 2 | https:\//TM-westcentralus01.AFS.Azure.net |
| Genel | Batı Avrupa | https:\//kailani6.One.Microsoft.com | Kuzey Avrupa | https:\//TM-kailani6.One.Microsoft.com |
| Genel | Batı ABD | https:\//Kailani.One.Microsoft.com | East US | https:\//TM-Kailani.One.Microsoft.com |
| Genel | Batı ABD 2 | https:\//westus201.AFS.Azure.net | Batı Orta ABD | https:\//TM-westus201.AFS.Azure.net |
| Kamu | ABD Devleti Arizona | https:\//usgovarizona01.AFS.Azure.us | ABD Devleti Texas | https:\//TM-usgovarizona01.AFS.Azure.us |
| Kamu | ABD Devleti Texas | https:\//usgovtexas01.AFS.Azure.us | ABD Devleti Arizona | https:\//TM-usgovtexas01.AFS.Azure.us |

- Yerel olarak yedekli (LRS) veya bölge yedekli (ZRS) depolama hesapları kullanıyorsanız, yalnızca "birincil uç nokta URL 'SI" altında listelenen URL 'YI etkinleştirmeniz gerekir.

- Küresel olarak yedekli (GRS) depolama hesapları kullanıyorsanız, üç URL 'yi etkinleştirin.

**Örnek:** ' De bir depolama eşitleme hizmeti dağıtıp `"West US"` sunucunuza kayıt yaptırın. Sunucunun bu durum için iletişim kurmasına izin veren URL 'Ler şunlardır:

> - https:\//Kailani.One.Microsoft.com (birincil uç nokta: Batı ABD)
> - https:\//kailani1.One.Microsoft.com (eşleştirilmiş başarısız bölge: Doğu ABD)
> - https:\//TM-Kailani.One.Microsoft.com (birincil bölgenin bulma URL 'si)

## <a name="summary-and-risk-limitation"></a>Özet ve risk sınırlaması
Bu belgede daha önce bulunan listeler, şu anda iletişim kuran Azure Dosya Eşitleme URL 'Leri içerir. Güvenlik duvarlarının bu etki alanlarına giden trafiğe izin alabilmesi gerekir. Microsoft bu listeyi güncel tutmaya devam eden bir çaba harcar.

Etki alanı sınırlandırma güvenlik duvarı kurallarını ayarlamak, güvenliği artırmak için bir ölçü olabilir. Bu güvenlik duvarı yapılandırmalarının kullanılması durumunda, URL 'Lerin ekleneceğini ve zaman içinde değişebileceğini göz önünde bulundurmanız gerekir. Bu makaleye düzenli olarak bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
