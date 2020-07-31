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
ms.openlocfilehash: 3d63ccc2c47bca9410b5b9105b90aa1f0cf5854a
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87439266"
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

    Artık, için amaçlanan trafik `greatapp.contoso.com` tehdit aktörün Azure sitesine ve tehdit aktör 'in görüntülenen içeriğin denetiminde gitmesidir. 

    Saldırgan DNS ile kullanım dışı bırakıldı ve contoso 'nun "GreatApp" alt etki alanı, alt etki alanının bir kurbanı oldu. 

![Sağlaması kaldırılmış bir Web sitesinden alınan alt etki alanı](./media/subdomain-takeover/subdomain-takeover.png)



## <a name="the-risks-of-subdomain-takeover"></a>Alt etki alanının riskleri

Bir DNS kaydı, kullanılamayan bir kaynağı işaret ediyorsa, kaydın kendisi DNS bölgeinizden kaldırılmış olmalıdır. Silinmemişse, bu bir "dangze DNS" kaydıdır ve alt etki alanı için bir olasılık oluşturur.

Saldırgan DNS girişleri, tehdit aktörlerini kötü amaçlı bir Web sitesi veya hizmeti barındırmak üzere ilişkili DNS adının denetimini ele geçirmesine olanak tanır. Bir kuruluşun alt etki alanındaki kötü amaçlı sayfalar ve hizmetler şu şekilde olabilir:

- Alt **etki alanının içeriği üzerinde denetim kaybı** -kuruluşunuzun içeriğini güvenli hale getirme sorunu, Ayrıca marka hasarı ve güven kaybı hakkında daha fazla bilgi.

- Çok fazla **ziyaretçi olmadan tanımlama bilgisi alma** -Web uygulamalarının, oturum tanımlama bilgilerini alt etki alanlarına (*. contoso.com) sunmaları yaygındır, bu nedenle herhangi bir alt etki alanı bunlara erişebilir. Tehdit aktörleri, gerçek bir arama sayfası oluşturmak, kullanıcıların bunu ziyaret etmesini ve tanımlama bilgilerinin (hatta güvenli tanımlama bilgileri) sayması için bir alt etki alanı kullanabilirler. Yaygın bir yanıltıcı, SSL sertifikalarının kullanılması, sitenizi ve kullanıcılarınızın tanımlama bilgilerini bir yük devrkaynağından korumakla aynıdır. Ancak tehdit aktör, geçerli bir SSL sertifikası için uygulama ve alma için ele geçirilmiş alt etki alanını kullanabilir. Geçerli SSL sertifikaları güvenli tanımlama bilgilerine erişim izni verir ve kötü amaçlı sitenin yasallığını daha da artırabilir.

- **Kimlik avı kampanyaları** -gerçek-arayan alt etki alanları, kimlik avı kampanyalarda kullanılabilir. Bu, kötü amaçlı siteler için ve ayrıca, tehdit aktörün bilinen bir markasının meşru bir alt etki alanıyla ilgili e-postaları almasına imkan tanıyan MX kayıtları için geçerlidir.

- **Daha fazla risk** -kötü amaçlı sıteler, XSS, CSRF, CORS atlama ve daha fazlası gibi diğer klasik saldırılara yönelik olarak kullanılabilir.



## <a name="preventing-dangling-dns-entries"></a>Geçiciye DNS girdilerini önlemek

Kuruluşunuzun DNS girdilerini engelleyen işlemleri gerçekleştirmesinin ve elde edilen alt etki alanı kuruluşlarının güvenlik programınızın önemli bir parçası olduğundan emin olma.

Bugün size sunulan önleyici ölçüler aşağıda listelenmiştir.


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

        >[!IMPORTANT]
        > **İzinler** -sorguyu tüm Azure aboneliklerinize erişimi olan bir kullanıcı olarak çalıştırın. 
        >
        > **Sınırlamalar** -Azure Kaynak grafiğinde, büyük bir Azure ortamınız varsa göz önünde bulundurmanız gereken azaltma ve sayfalama limitleri vardır. Büyük Azure Kaynak veri kümeleriyle çalışma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/resource-graph/concepts/work-with-data) . Aşağıdaki örnek betik, bu sınırlamaların önüne geçmek için abonelik toplu işleme kullanır.

        ```powershell
        
            # Fetch the full array of subscription IDs.
            $subscriptions = Get-AzSubscription

            $subscriptionIds = $subscriptions.Id
                   # Output file path and names
                   $date = get-date
                   $fdate = $date.ToString("MM-dd-yyy hh_mm_ss tt")
                   $fdate #log to console
                   $rpath = [Environment]::GetFolderPath("MyDocuments") + '\' # Feel free to update your path.
                   $rname = 'Tenant_FQDN_Report_' + $fdate + '.csv' # Feel free to update the document name.
                   $fpath = $rpath + $rname
                   $fpath #This is the output file of FQDN report.

            # query
            $query = "where type in ('microsoft.network/frontdoors',
                                    'microsoft.storage/storageaccounts',
                                    'microsoft.cdn/profiles/endpoints',
                                    'microsoft.network/publicipaddresses',
                                    'microsoft.network/trafficmanagerprofiles',
                                    'microsoft.containerinstance/containergroups',
                                    'microsoft.apimanagement/service',
                                    'microsoft.web/sites',
                                    'microsoft.web/sites/slots')
                        | extend FQDN = case(
                            type =~ 'microsoft.network/frontdoors', properties['cName'],
                            type =~ 'microsoft.storage/storageaccounts', parse_url(tostring(properties['primaryEndpoints']['blob'])).Host,
                            type =~ 'microsoft.cdn/profiles/endpoints', properties['hostName'],
                            type =~ 'microsoft.network/publicipaddresses', properties['dnsSettings']['fqdn'],
                            type =~ 'microsoft.network/trafficmanagerprofiles', properties['dnsConfig']['fqdn'],
                            type =~ 'microsoft.containerinstance/containergroups', properties['ipAddress']['fqdn'],
                            type =~ 'microsoft.apimanagement/service', properties['hostnameConfigurations']['hostName'],
                            type =~ 'microsoft.web/sites', properties['defaultHostName'],
                            type =~ 'microsoft.web/sites/slots', properties['defaultHostName'],
                            '')
                        | project id, ['type'], name, FQDN
                        | where isnotempty(FQDN)";

            # Paging helper cursor
            $Skip = 0;
            $First = 1000;

            # If you have large number of subscriptions, process them in batches of 2,000.
            $counter = [PSCustomObject] @{ Value = 0 }
            $batchSize = 2000
            $response = @()

            # Group the subscriptions into batches.
            $subscriptionsBatch = $subscriptionIds | Group -Property { [math]::Floor($counter.Value++ / $batchSize) }

            # Run the query for each subscription batch with paging.
            foreach ($batch in $subscriptionsBatch)
            { 
                $Skip = 0; #Reset after each batch.

                $response += do { Start-Sleep -Milliseconds 500;   if ($Skip -eq 0) {$y = Search-AzGraph -Query $query -First $First -Subscription $batch.Group ; } `
                else {$y = Search-AzGraph -Query $query -Skip $Skip -First $First -Subscription $batch.Group } `
                $cont = $y.Count -eq $First; $Skip = $Skip + $First; $y; } while ($cont)
            }

            # View the completed results of the query on all subscriptions.
            $response | Export-Csv -Path $fpath -Append 

        ```

        `FQDNProperty`Yukarıdaki kaynak Graph sorgusunda belirtilen türlerin ve bunların değerlerinin listesi:

        |Kaynak adı  | `<ResourceType>`  | `<FQDNproperty>`  |
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

- [Hızlı başlangıç: Azure PowerShell kullanarak ilk kaynak grafik sorgunuzu çalıştırın](https://docs.microsoft.com/azure/governance/resource-graph/first-query-powershell)
