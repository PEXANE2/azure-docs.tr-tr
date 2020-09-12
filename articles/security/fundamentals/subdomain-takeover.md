---
title: Azure DNS diğer ad kayıtlarıyla ve Azure App Service özel etki alanı doğrulamasına sahip alt etki alanı engellemesini engelle
description: Alt etki alanının genel yüksek önem derecesine sahip bir tehdidi nasıl önleyeceğinizi öğrenin
services: security
author: memildin
manager: rkarlin
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2020
ms.author: memildin
ms.openlocfilehash: faa61dc351bebd3d2a85ad229036e5b9fba9256e
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514620"
---
# <a name="prevent-dangling-dns-entries-and-avoid-subdomain-takeover"></a>DNS girişlerinin tehlikini önleyin ve alt etki alanı devralmayı önleyin

Bu makalede, alt etki alanının genel güvenlik tehdidi ve buna karşı hafifletmek için uygulayabileceğiniz adımlar açıklanmaktadır.


## <a name="what-is-subdomain-takeover"></a>Alt etki alanı nedir?

Alt etki alanı kaynakları, çok sayıda kaynağı düzenli olarak oluşturan ve silen kuruluşlar için ortak, yüksek öneme sahip bir tehdittir. Sağlanabilen bir Azure kaynağına işaret eden bir [DNS kaydınız](https://docs.microsoft.com/azure/dns/dns-zones-records#dns-records) varsa, alt etki alanı ele alma işlemi gerçekleşebilir. Bu tür DNS kayıtları, "dangze DNS" girişleri olarak da bilinir. CNAME kayıtları özellikle bu tehdide karşı savunmasız. Alt etki alanı cılar, kötü amaçlı aktörlerin bir kuruluşun etki alanı için tasarlanan trafiği kötü amaçlı etkinlik gerçekleştiren bir siteye yeniden yönlendirmesine olanak sağlar.

Bir alt etki alanı için ortak senaryo:

1. **OLUŞTURULURKEN**

    1. Tam etki alanı adı (FQDN) ile bir Azure kaynağı sağlayın `app-contogreat-dev-001.azurewebsites.net` .

    1. Trafiği Azure kaynağına yönlendiren alt etki alanı ile DNS bölgenize bir CNAME kaydı atarsınız `greatapp.contoso.com` .

1. **SAĞLAMAYı KALDıRMA**

    1. Azure kaynağı artık gerekli olmadığında veya silinir. 
    
        Bu noktada, CNAME kaydı `greatapp.contoso.com` *should* DNS bölgeinizden kaldırılmalıdır. CNAME kaydı kaldırılmazsa, etkin bir etki alanı olarak tanıtılıp trafiği etkin bir Azure kaynağına yönlendirmez. Bu, "Dangling" DNS kaydının tanımıdır.

    1. Bu alt etki alanı, `greatapp.contoso.com` artık güvenlik açığından etkilenir ve başka bir Azure aboneliğinin kaynağına atanarak üzerinden alınabilir.

1. **DEVRALMA**

    1. Tehdit aktör, yaygın olarak kullanılan yöntemleri ve araçları kullanarak tehdit alt etki alanını bulur.  

    1. Tehdit aktör, daha önce denetlediğiniz kaynakla aynı FQDN 'ye sahip bir Azure kaynağı sağlar. Bu örnekte, `app-contogreat-dev-001.azurewebsites.net` .

    1. Alt etki alanına gönderilen trafik `myapp.contoso.com` artık içeriği kontrol ettikleri kötü amaçlı aktör kaynağına yönlendirilir.



![Sağlaması kaldırılmış bir Web sitesinden alınan alt etki alanı](./media/subdomain-takeover/subdomain-takeover.png)



## <a name="the-risks-of-subdomain-takeover"></a>Alt etki alanının riskleri

Bir DNS kaydı, kullanılamayan bir kaynağı işaret ediyorsa, kaydın kendisi DNS bölgeinizden kaldırılmış olmalıdır. Silinmemişse, bu bir "dangze DNS" kaydıdır ve alt etki alanı için bir olasılık oluşturur.

Saldırgan DNS girişleri, tehdit aktörlerini kötü amaçlı bir Web sitesi veya hizmeti barındırmak üzere ilişkili DNS adının denetimini ele geçirmesine olanak tanır. Bir kuruluşun alt etki alanındaki kötü amaçlı sayfalar ve hizmetler şu şekilde sonuçlanabilir:

- Alt **etki alanının içeriği üzerinde denetim kaybı** -kuruluşunuzun içeriğini güvenli hale getirme sorunu, Ayrıca marka hasarı ve güven kaybı hakkında daha fazla bilgi.

- Çok fazla **ziyaretçi olmadan tanımlama bilgisi alma** -Web uygulamalarının, oturum tanımlama bilgilerini alt etki alanlarına (*. contoso.com) sunmaları yaygındır, bu nedenle herhangi bir alt etki alanı bunlara erişebilir. Tehdit aktörleri, gerçek bir arama sayfası oluşturmak, kullanıcıların bunu ziyaret etmesini ve tanımlama bilgilerinin (hatta güvenli tanımlama bilgileri) sayması için bir alt etki alanı kullanabilirler. Yaygın bir yanıltıcı, SSL sertifikalarının kullanılması, sitenizi ve kullanıcılarınızın tanımlama bilgilerini bir yük devrkaynağından korumakla aynıdır. Ancak tehdit aktör, geçerli bir SSL sertifikası için uygulama ve alma için ele geçirilmiş alt etki alanını kullanabilir. Geçerli SSL sertifikaları güvenli tanımlama bilgilerine erişim izni verir ve kötü amaçlı sitenin yasallığını daha da artırabilir.

- **Kimlik avı kampanyaları** -gerçek-arayan alt etki alanları, kimlik avı kampanyalarda kullanılabilir. Bu, tehdit aktörün bilinen bir markasının meşru bir alt etki alanıyla ilgili e-postaları almasına imkan tanıyan, kötü amaçlı siteler ve MX kayıtları için geçerlidir.

- **Daha fazla risk** -kötü AMAÇLı siteler XSS, CSRF, CORS atlama ve daha fazlası gibi diğer klasik saldırılara yönelik olarak kullanılabilir.



## <a name="identify-dangling-dns-entries"></a>Geçiciye DNS girdilerini tanımla

Kuruluşunuzdaki tehlikeden DNS girişlerini belirlemek için, Microsoft 'un GitHub 'da barındırılan ["Get-Danglındnsrecords"](https://aka.ms/DanglingDNSDomains)PowerShell araçlarını kullanın.

Bu araç, Azure müşterilerinin aboneliklerinde veya kiracılarında oluşturulan mevcut bir Azure kaynağıyla ilişkili bir CNAME 'e sahip tüm etki alanlarını listelemeye yardımcı olur.

CNAMEs 'ler diğer DNS hizmetleriyle ve Azure kaynakları ' na işaret alıyorsa, bir giriş dosyasındaki CNAMEs 'leri araca girin.

Araç, aşağıdaki tabloda listelenen Azure kaynaklarını destekler. Araç, tüm kiracının CNAMEs 'leri ayıklar veya giriş olarak alır.


| Hizmet                   | Tür                                        | FQDNproperty                               | Örnek                         |
|---------------------------|---------------------------------------------|--------------------------------------------|---------------------------------|
| Azure Front Door          | Microsoft. Network/frontkapaklı                | Properties. cName                           | `abc.azurefd.net`               |
| Azure Blob Depolama        | Microsoft. Storage/storageaccounts           | Properties. BID. blob           | `abc. blob.core.windows.net`    |
| Azure CDN                 | Microsoft. CDN/profiller/uç noktaları            | Properties. hostName                        | `abc.azureedge.net`             |
| Genel IP adresleri       | Microsoft. Network/publicıpaddresses         | Properties. dnsSettings. FQDN                | `abc.EastUs.cloudapp.azure.com` |
| Azure Traffic Manager     | Microsoft. Network/trafficmanagerprofiles    | Properties. dnsConfig. FQDN                  | `abc.trafficmanager.net`        |
| Azure Container Örneği  | Microsoft. containerınstance/containergroups | Properties. IPAddress. FQDN                  | `abc.EastUs.azurecontainer.io`  |
| Azure API Management      | Microsoft. apimanayönetimi/hizmeti             | Properties. hostnameConfigurations. hostName | `abc.azure-api.net`             |
| Azure App Service         | Microsoft. Web/siteler                         | Properties. defaultHostName                 | `abc.azurewebsites.net`         |
| Azure App Service Yuvaları | Microsoft. Web/Sites/Yuvaları                   | Properties. defaultHostName                 | `abc-def.azurewebsites.net`     |



### <a name="prerequisites"></a>Ön koşullar

Sorguyu şu sahip olan bir kullanıcı olarak çalıştırın:

- Azure aboneliklerine en az okuyucu düzeyinde erişim
- Azure Kaynak grafiğine erişimi oku

Kuruluşunuzun kiracısının genel yöneticisiyseniz, [tüm Azure aboneliklerini ve Yönetim gruplarını yönetmek için erişimi yükseltme](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin)' deki Kılavuzu kullanarak tüm kuruluşunuzun aboneliğine erişimi sağlamak için hesabınızı yükseltin.


> [!TIP]
> Azure Kaynak grafiğinde, büyük bir Azure ortamınız varsa göz önünde bulundurmanız gereken azaltma ve sayfalama limitleri vardır. Büyük Azure Kaynak veri kümeleriyle çalışma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) . 
> 
> Araç, bu sınırlamaların oluşmaması için abonelik toplu işleme kullanır.

### <a name="run-the-script"></a>Betiği çalıştırın

Betiğin iki sürümü vardır, her ikisi de aynı giriş parametrelerine sahiptir ve benzer çıktılar üretir:

|Komut Dosyası  |Bilgi  |
|---------|---------|
|**Get-DanglingDnsRecordsPsCore.ps1**    |Paralel mod yalnızca PowerShell sürüm 7 ve üzeri sürümlerde desteklenir, aksi takdirde seri modu çalıştırılır.|
|**Get-DanglingDnsRecordsPsDesktop.ps1** |Bu betik [Windows Iş akışını](https://docs.microsoft.com/dotnet/framework/windows-workflow-foundation/overview)kullandığından, yalnızca PowerShell masaüstü/sürümü 6 ' dan düşük olan sürümde desteklenir.|

Daha fazla bilgi edinin ve GitHub 'dan PowerShell betiklerini indirin: https://aka.ms/DanglingDNSDomains .

## <a name="remediate-dangling-dns-entries"></a>Geçiciye DNS girdilerini düzelt 

DNS bölgelerinizi gözden geçirin ve sallaştırılmış veya daha fazla alınmış CNAME kayıtlarını belirlersiniz. Alt etki alanlarının sallanmak veya daha fazla alınmış olması bulunursa, güvenlik açığı bulunan alt etki alanlarını kaldırın ve riskleri aşağıdaki adımlarla azaltabilirsiniz:

1. DNS bölgeinizden, kaynakları artık sağlanmamıştır.

1. Trafiğin, denetiminizden gelen kaynaklara yönlendirilmesini sağlamak için, sallantıze alt etki alanlarının CNAME kayıtlarında belirtilen FQDN 'Ler ile ek kaynaklar sağlayın.

1. Uygulama kodunuzu belirli alt etki alanlarına yapılan başvurular için gözden geçirin ve yanlış veya eski alt etki alanı başvurularını güncelleştirin.

1. Her türlü güvenliğin yapılıp yapılmayacağını araştırın ve kuruluşunuzun olay yanıtı yordamlarına göre işlem yapın. Bu sorunu araştırmak için ipuçları ve en iyi uygulamalar aşağıda bulabilirsiniz.

    Uygulama mantığınız, OAuth kimlik bilgileri gibi gizli dizi alt etki alanına gönderiliyorsa veya tehlikşa alt etki alanlarına gizlilik duyarlı bilgiler gönderildiyse, bu veriler üçüncü taraflara açık olabilir.

1. Kaynağın sağlanması sırasında CNAME kaydının neden DNS bölgeinizden kaldırılmadığını anlayın ve daha sonra Azure kaynakları sağlandığında DNS kayıtlarının uygun şekilde güncelleştirildiğinden emin olmak için adımları uygulayın.


## <a name="prevent-dangling-dns-entries"></a>Tehlikeden DNS girdilerini engelle

Kuruluşunuzun DNS girdilerini engelleyen işlemleri gerçekleştirmesinin ve elde edilen alt etki alanı kuruluşlarının güvenlik programınızın önemli bir parçası olduğundan emin olma.

Bazı Azure Hizmetleri, önleyici ölçüler oluşturmaya yardımcı olacak özellikler sunar ve aşağıda ayrıntılı olarak açıklanmıştır. Bu sorunu önleyen diğer yöntemler, kuruluşunuzun en iyi uygulamaları veya standart işletim yordamları aracılığıyla oluşturulmalıdır.


### <a name="use-azure-dns-alias-records"></a>Azure DNS diğer ad kayıtlarını kullanma

Azure DNS [diğer ad kayıtları](https://docs.microsoft.com/azure/dns/dns-alias#scenarios) , bir Azure kaynağı Ile bir DNS kaydı yaşam döngüsüne eşlenerek tehlikeden başvuruların oluşmasını engelleyebilir. Örneğin, bir genel IP adresini veya bir Traffic Manager profilini işaret etmek için bir diğer ad kaydı olarak nitelenen bir DNS kaydını göz önünde bulundurun. Bu temel alınan kaynakları silerseniz, DNS diğer ad kaydı boş bir kayıt kümesi haline gelir. Artık silinen kaynağa başvurmuyor. Diğer ad kayıtlarıyla neleri koruyabileceğiniz sınırlandırmalar olduğunu unutmayın. Bugün, liste şu şekilde sınırlıdır:

- Azure Front Door
- Traffic Manager profilleri
- Azure Content Delivery Network (CDN) uç noktaları
- Genel IP'ler

Günümüzde sınırlı hizmet sunumlarına rağmen, mümkün olan her durumda alt etki alanı üzerinde savunmak için diğer ad kayıtlarını kullanmanızı öneririz.

Azure DNS diğer ad kayıtlarının özellikleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/dns/dns-alias#capabilities) .



### <a name="use-azure-app-services-custom-domain-verification"></a>Azure App Service özel etki alanı doğrulamasını kullan

Azure App Service için DNS girişleri oluştururken bir asuıd oluşturun. alanınızın Etki alanı doğrulama KIMLIĞIYLE TXT kaydı. Böyle bir TXT kaydı olduğunda, başka hiçbir Azure aboneliği, diğer özel etki alanını doğrulayabilirler. 

Bu kayıtlar, birisinin CNAME girişinizdeki aynı ada sahip Azure App Service oluşturmasını engellemez. Etki alanı adının sahipliğini kanıtlayabilme özelliği olmadan, tehdit aktörleri trafiği alamaz veya içeriği denetleyebilir.

Mevcut bir özel DNS adını Azure App Service eşleme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain) .



### <a name="build-and-automate-processes-to-mitigate-the-threat"></a>Tehdidi azaltmak için süreçler oluşturun ve bunları otomatikleştirin

Genellikle, geliştiricilerin ve operasyon ekiplerinin, çok fazla DNS tehditleri önlemek için temizleme işlemlerini çalıştırmasına olanak sağlar. Aşağıdaki uygulamalar, kuruluşunuzun bu tehdidi olumsuz şekilde korumanıza yardımcı olur. 

- **Önlemeye yönelik yordamlar oluşturun:**

    - Uygulama geliştiricilerinizi kaynakları her sildiklerinde yeniden yönlendirmek için eğitin.

    - Bir hizmetin yetkisini alırken gerekli denetimler listesinde "DNS girişini kaldır" öğesini yerleştir.

    - Özel bir DNS girişi olan herhangi bir kaynağa [silme kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) koyun. Silme kilidi, kaynağın sağlanması tamamlanmadan önce eşlemenin kaldırılması gerektiğini belirten bir gösterge görevi görür. Buna benzer ölçüler yalnızca dahili eğitim programları ile birleştirildiğinde çalışır.

- **Bulma için yordamlar oluşturun:**

    - Alt etki alanınızı Azure kaynaklarıyla eşleştirilmiş olduğundan emin olmak için DNS kayıtlarınızı düzenli olarak gözden geçirin:

        - Var: *. azurewebsites.net veya *. cloudapp.azure.com gibi Azure alt etki alanlarını işaret eden kaynaklar için DNS bölgelerinizi sorgulayın ( [Bu başvuru listesine](azure-domains.md)bakın).
        - Kendi DNS alt etki alanları 'nın hedeflediği tüm kaynakların sahip olduğunu doğrulayın.

    - Azure tam etki alanı adı (FQDN) uç noktalarınızın ve uygulama sahiplerinin hizmet kataloğunu saklayın. Hizmet kataloğunuzu derlemek için aşağıdaki Azure Kaynak Grafiği sorgu betiğini çalıştırın. Bu betik, erişim sahibi olduğunuz kaynakların FQDN uç nokta bilgilerini projeler ve bir CSV dosyasında çıkarır. Kiracınız için tüm aboneliklere erişiminiz varsa, komut dosyası aşağıdaki örnek betikte gösterildiği gibi tüm abonelikleri dikkate alır. Sonuçları belirli bir abonelik kümesiyle sınırlamak için betiği gösterildiği gibi düzenleyin.


- **Düzeltme için yordamlar oluşturun:**
    - DNS girişleri bulunduğunda, takımınızın herhangi bir tehlikeye atılıp oluşup oluşmadığını araştırması gerekir.
    - Kaynak kullanımdan çalıştırıldığında adresin neden tekrar yönlendirilmediğini araştırın.
    - Artık kullanımda değilse DNS kaydını silin veya kuruluşunuzun sahip olduğu doğru Azure kaynağına (FQDN) işaret edin.
 

## <a name="next-steps"></a>Sonraki adımlar

Alt etki alanı devrmasına karşı savunmak için kullanabileceğiniz ilgili hizmetler ve Azure özellikleri hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın.

- [Azure DNS özel etki alanları için diğer ad kayıtlarını kullanmayı destekler](https://docs.microsoft.com/azure/dns/dns-alias#prevent-dangling-dns-records)

- [Azure App Service özel etki alanları eklerken etki alanı doğrulama KIMLIĞINI kullanın](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#get-domain-verification-id) 

- [Hızlı başlangıç: Azure PowerShell kullanarak ilk kaynak grafik sorgunuzu çalıştırın](https://docs.microsoft.com/azure/governance/resource-graph/first-query-powershell)
