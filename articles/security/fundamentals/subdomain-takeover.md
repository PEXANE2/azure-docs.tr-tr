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
ms.openlocfilehash: 4e5969b4c3a42fc8a2c4b1cd537c22a4422ca131
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85269094"
---
# <a name="prevent-dangling-dns-entries-and-avoid-subdomain-takeover"></a>DNS girişlerinin tehlikini önleyin ve alt etki alanı devralmayı önleyin

Bu makalede, alt etki alanının genel güvenlik tehdidi ve buna karşı hafifletmek için uygulayabileceğiniz adımlar açıklanmaktadır.


## <a name="what-is-subdomain-takeover"></a>Alt etki alanı nedir?

Alt etki alanı kaynakları, çok sayıda kaynağı düzenli olarak oluşturan ve silen kuruluşlar için ortak, yüksek öneme sahip bir tehdittir. Sağlanabilen bir Azure kaynağına işaret eden bir DNS kaydınız varsa, alt etki alanı ele alma işlemi gerçekleşebilir. Bu tür DNS kayıtları, "dangze DNS" girişleri olarak da bilinir. CNAME kayıtları özellikle bu tehdide karşı savunmasız.

Bir alt etki alanı için ortak senaryo:

1. Bir Web sitesi oluşturulur. 

    Bu örnekte, `app-contogreat-dev-001.azurewebsites.net` .

1. Web sitesine işaret eden DNS 'ye bir CNAME girişi eklenir. 

    Bu örnekte, şu kolay ad oluşturuldu: `greatapp.contoso.com` .

1. Birkaç aydan sonra, site artık gerekli değildir, bu nedenle ilgili DNS girişi silinmeden **silinir.** 

    CNAME DNS girişi artık "Dangling" dir.

1. Site silindikten hemen sonra, bir tehdit aktör eksik siteyi bulur ve üzerinde kendi Web sitesini oluşturur `app-contogreat-dev-001.azurewebsites.net` .

    Artık, için amaçlanan trafik `greatapp.contoso.com` tehdit aktörün Azure sitesine gitmektedir ve tehdit aktör, görüntülenen içeriğin denetimidir. 

    Saldırgan DNS ile kullanım dışı bırakıldı ve contoso 'nun "GreatApp" alt etki alanı, alt etki alanının bir kurbanı oldu. 

![Sağlaması kaldırılmış bir Web sitesinden alınan alt etki alanı](./media/subdomain-takeover/subdomain-takeover.png)



## <a name="the-risks-of-dangling-dns-records"></a>DNS kayıtlarının tehlikeleri

Bir DNS kaydı, kullanılamayan bir kaynağı işaret ediyorsa, kaydın kendisi DNS bölgeinizden kaldırılmış olmalıdır. Silinmemişse, bu bir "Dangling DNS" kaydı ve güvenlik riski oluşturur.

Kuruluşa yönelik risk, bir tehdit aktörün kötü amaçlı bir Web sitesi veya hizmeti barındırmak üzere ilişkili DNS adının denetimini almasını sağlar. Kuruluşun alt etki alanındaki bu kötü amaçlı Web sitesi şu şekilde olabilir:

- Alt **etki alanının içeriği üzerinde denetim kaybı** -kuruluşunuzun içeriğini güvenli hale getirme sorunu, Ayrıca marka hasarı ve güven kaybı hakkında daha fazla bilgi.

- Çok fazla **ziyaretçi olmadan tanımlama bilgisi alma** -Web uygulamalarının, oturum tanımlama bilgilerini alt etki alanlarına (*. contoso.com) sunmaları yaygındır, bu nedenle herhangi bir alt etki alanı bunlara erişebilir. Tehdit aktörleri, gerçek bir arama sayfası oluşturmak, kullanıcıların bunu ziyaret etmesini ve tanımlama bilgilerinin (hatta güvenli tanımlama bilgileri) sayması için bir alt etki alanı kullanabilirler. Yaygın bir yanıltıcı, SSL sertifikalarının kullanılması, sitenizi ve kullanıcılarınızın tanımlama bilgilerini bir yük devrkaynağından korumakla aynıdır. Ancak tehdit aktör, geçerli bir SSL sertifikası için uygulama ve alma için ele geçirilmiş alt etki alanını kullanabilir. Böylece, güvenli tanımlama bilgilerine erişim izni verir ve kötü amaçlı sitenin yasallığını daha da artırabilir.

- **Kimlik avı kampanyaları** -gerçek-arayan alt etki alanları, kimlik avı kampanyalarda kullanılabilir. Bu, kötü amaçlı siteler için ve ayrıca, tehdit aktörün bilinen bir markasının meşru bir alt etki alanıyla ilgili e-postaları almasına imkan tanıyan MX kayıtları için geçerlidir.

- **Daha fazla risk** -XSS, CSRF, CORS atlama ve daha fazlası gibi diğer klasik saldırılara karşı ilerletin.



## <a name="preventing-dangling-dns-entries"></a>Geçiciye DNS girdilerini önlemek

Kuruluşunuzun DNS girdilerini engelleyen işlemleri gerçekleştirmesinin ve elde edilen alt etki alanı kuruluşlarının güvenlik programınızın önemli bir parçası olduğundan emin olma.

Bugün size sunulan önleyici ölçüler aşağıda listelenmiştir.


### <a name="use-azure-dns-alias-records"></a>Azure DNS diğer ad kayıtlarını kullanma

Azure kaynağına sahip bir DNS kaydı yaşam döngüsünün sıkı bir şekilde eşlenmesiyle Azure DNS [diğer ad kayıtları](https://docs.microsoft.com/azure/dns/dns-alias#scenarios) özelliği, sallaştırılmış başvuruların oluşmasını engelleyebilir. Örneğin, bir genel IP adresini veya bir Traffic Manager profilini işaret etmek için bir diğer ad kaydı olarak nitelenen bir DNS kaydını göz önünde bulundurun. Bu temel alınan kaynakları silerseniz, DNS diğer ad kaydı boş bir kayıt kümesi haline gelir. Artık silinen kaynağa başvurmuyor. Diğer ad kayıtlarıyla neleri koruyabileceğiniz sınırlandırmalar olduğunu unutmayın. Bugün, liste şu şekilde sınırlıdır:

- Azure Front Door
- Traffic Manager profilleri
- Azure Content Delivery Network (CDN) uç noktaları
- Genel IP'ler

Diğer ad kayıtlarıyla birlikte bulunan alt etki alanından korunabilecek kaynaklarınız varsa, günümüzde sınırlı hizmet sunumlarına rağmen bunu yapmanızı öneririz.

Azure DNS diğer ad kayıtlarının özellikleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/dns/dns-alias#capabilities) .



### <a name="use-azure-app-services-custom-domain-verification"></a>Azure App Service özel etki alanı doğrulamasını kullan

Azure App Service için DNS girişleri oluştururken bir asuıd oluşturun. alanınızın Etki alanı doğrulama KIMLIĞIYLE TXT kaydı. Böyle bir TXT kaydı olduğunda, başka hiçbir Azure aboneliği, diğer özel etki alanını doğrulayabilirler. 

Bu kayıtlar, birisinin CNAME girişinizdeki ile aynı ada sahip Azure App Service oluşturmasını engellemez, ancak etki alanı adının sahipliğini kanıtlayamayacağından trafiği alamaz veya içeriği denetleyemez.

Mevcut bir özel DNS adını Azure App Service eşleme hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/Azure/app-service/app-service-web-tutorial-custom-domain) .



### <a name="build-and-automate-processes-to-mitigate-the-threat"></a>Tehdidi azaltmak için süreçler oluşturun ve bunları otomatikleştirin

Genellikle, geliştiricilerin ve operasyon ekiplerinin, çok fazla DNS tehditleri önlemek için temizleme işlemlerini çalıştırmasına olanak sağlar. Aşağıdaki uygulamalar, kuruluşunuzun bu tehdidi olumsuz şekilde korumanıza yardımcı olur. 

- **Önlemeye yönelik yordamlar oluşturun:**

    - Uygulama geliştiricilerinizi kaynakları her sildiklerinde yeniden yönlendirmek için eğitin.

    - Bir hizmetin yetkisini alırken gerekli denetimler listesinde "DNS girişini kaldır" öğesini yerleştir.

    - Özel bir DNS girişi olan herhangi bir kaynağa [silme kilitleri](https://docs.microsoft.com/azure/azure-resource-manager/management/lock-resources) koyun. Bu, kaynağın sağlanması tamamlanmadan önce eşlemenin kaldırılması gereken bir gösterge olarak kullanılmalıdır. Buna benzer ölçüler yalnızca dahili eğitim programları ile birleştirildiğinde çalışır.

- **Bulma için yordamlar oluşturun:**

    - Alt etki alanınızı Azure kaynaklarıyla eşleştirilmiş olduğundan emin olmak için DNS kayıtlarınızı düzenli olarak gözden geçirin:

        - **Var** : *. azurewebsites.NET veya *. cloudapp.Azure.com gibi Azure alt etki alanlarını işaret eden kaynaklar için DNS bölgelerinizi sorgulayın ( [Bu başvuru listesine](azure-domains.md)bakın).
        - **Kendı** DNS alt etki alanları 'nın hedeflediği tüm kaynakların sahip olduğunu doğrulayın.

    - Azure tam etki alanı adı (FQDN) uç noktalarınızın ve uygulama sahiplerinin hizmet kataloğunu saklayın. Hizmet kataloğunuzu derlemek için aşağıdaki tablodaki parametrelerle aşağıdaki Azure Kaynak Grafiği sorgusunu çalıştırın:
    
        >[!IMPORTANT]
        > **İzinler** -sorguyu tüm Azure aboneliklerinize erişimi olan bir kullanıcı olarak çalıştırın. 
        >
        > **Sınırlamalar** -Azure Kaynak grafiğinde, büyük bir Azure ortamınız varsa göz önünde bulundurmanız gereken azaltma ve sayfalama limitleri vardır. Büyük Azure Kaynak veri kümeleriyle çalışma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) .  

        ```
        Search-AzGraph -Query "resources | where type == '[ResourceType]' | project tenantId, subscriptionId, type, resourceGroup, name, endpoint = [FQDNproperty]"
        ``` 
        
        Örneğin, bu sorgu Azure App Service kaynakları döndürür:

        ```
        Search-AzGraph -Query "resources | where type == 'microsoft.web/sites' | project tenantId, subscriptionId, type, resourceGroup, name, endpoint = properties.defaultHostName"
        ```
        
        Ayrıca, birden çok kaynak türünü birleştirebilirsiniz. Bu örnek sorgu Azure App Service **ve** Azure App Service yuvalardan kaynakları döndürür:

        ```
        Search-AzGraph -Query "resources | where type in ('microsoft.web/sites', 'microsoft.web/sites/slots') | project tenantId, subscriptionId, type, resourceGroup, name, endpoint = properties.defaultHostName"
        ```

        |Kaynak adı  |[ResourceType]  | [FQDNproperty]  |
        |---------|---------|---------|
        |Azure Front Door|Microsoft. Network/frontkapaklı|Properties. cName|
        |Azure Blob Depolama|Microsoft. Storage/storageaccounts|Properties. BID. blob|
        |Azure CDN|Microsoft. CDN/profiller/uç noktaları|Properties. hostName|
        |Genel IP adresleri|Microsoft. Network/publicıpaddresses|Properties. dnsSettings. FQDN|
        |Azure Traffic Manager|Microsoft. Network/trafficmanagerprofiles|Properties. dnsConfig. FQDN|
        |Azure Container Örneği|Microsoft. containerınstance/containergroups|Properties. IPAddress. FQDN|
        |Azure API Management|Microsoft. apimanayönetimi/hizmeti|Properties. hostnameConfigurations. hostName|
        |Azure App Service|Microsoft. Web/siteler|Properties. defaultHostName|
        |Azure App Service Yuvaları|Microsoft. Web/Sites/Yuvaları|Properties. defaultHostName|


- **Düzeltme için yordamlar oluşturun:**
    - DNS girişleri bulunduğunda, takımınızın herhangi bir tehlikeye atılıp oluşup oluşmadığını araştırması gerekir.
    - Kaynak kullanımdan çalıştırıldığında adresin neden tekrar yönlendirilmediğini araştırın.
    - Artık kullanımda değilse DNS kaydını silin veya kuruluşunuzun sahip olduğu doğru Azure kaynağına (FQDN) işaret edin.
 

## <a name="next-steps"></a>Sonraki adımlar

Alt etki alanı devrmasına karşı savunmak için kullanabileceğiniz ilgili hizmetler ve Azure özellikleri hakkında daha fazla bilgi edinmek için aşağıdaki sayfalara bakın.

- [Azure DNS özel etki alanları için diğer ad kayıtlarını kullanmayı destekler](https://docs.microsoft.com/azure/dns/dns-alias#prevent-dangling-dns-records)

- [Azure App Service özel etki alanları eklerken etki alanı doğrulama KIMLIĞINI kullanın](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#get-domain-verification-id) 

-    [Hızlı başlangıç: Azure PowerShell kullanarak ilk kaynak grafik sorgunuzu çalıştırın](https://docs.microsoft.com/azure/governance/resource-graph/first-query-powershell)