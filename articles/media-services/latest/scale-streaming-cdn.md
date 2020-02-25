---
title: CDN ile akış ölçeklendirme
titleSuffix: Azure Media Services
description: Doğrudan bir istemci oynatıcı uygulamasına veya bir Content Delivery Network (CDN) içerik teslim eden bir akış hizmeti hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/13/2020
ms.author: juliako
ms.openlocfilehash: 90fa3b06e2696e9b45c333c75c8a8e117d5c0c96
ms.sourcegitcommit: dd3db8d8d31d0ebd3e34c34b4636af2e7540bd20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2020
ms.locfileid: "77563121"
---
# <a name="scaling-streaming-with-cdn"></a>CDN ile akış ölçeklendirme

Azure Content Delivery Network (CDN), dünya genelindeki stratejik olarak yerleştirilen fiziksel düğümlerde içeriği önbelleğe alarak, yüksek bant genişliği içeriğinin hızlı bir şekilde kullanıcılara teslimi konusunda geliştiricilere genel bir çözüm sunar.  

CDN, her codec için bir Media Services [akış uç noktası (başlangıç)](streaming-endpoint-concept.md) , her akış protokolü, her bir bir kapsayıcı biçimi başına ve ŞIFRELEME/DRM başına bir akış uç noktası (Origin) akışı Her codec-akış protokolü birleşimi için-bit hızı-şifreleme, ayrı bir CDN önbelleği olacaktır. 

Video parçası önbelleğe alındığı sürece popüler içerik doğrudan CDN önbelleğinden sunulacaktır. Tam olarak aynı şeyi izleyen çok sayıda kişi olduğu için canlı içeriğin önbelleğe alınması olasıdır. Çok daha popüler ve bazıları olmayan bazı içeriklere sahip olabileceğinden isteğe bağlı içerik biraz karmaşık olabilir. Bunlardan hiçbirinin popüler olduğu milyonlarca video varlığınız varsa (yalnızca bir veya iki Görüntüleyici), ancak tüm farklı videoları izlerken binlerce insan varsa, CDN çok daha az etkili hale gelir. 

Ayrıca, uyarlamalı akışın nasıl çalıştığını göz önünde bulundurmanız gerekir. Her tek video parçası kendi varlığı olarak önbelleğe alınır. Örneğin, belirli bir videonun ilk kez nasıl izlenen hakkında düşünün. Görüntüleyici burada yalnızca birkaç saniye izlemeyi atlar ve burada yalnızca, izlenen kişilerin CDN 'de önbelleğe alınması ile ilişkili video parçaları vardır. Uyarlamalı akış sayesinde genellikle 5 ile 7 arasında farklı bit fiyatları vardır. Bir kişi bir bit hızı izlerken ve başka bir kişi farklı bir bit hızı izlerse, her biri CDN 'de ayrı olarak önbelleğe alınır. İki kişi aynı bit hızını izliyor olsa da, farklı protokollerde akış olabilir. Her protokol (HLS, MPEG-DASH, Kesintisiz Akış) ayrı olarak önbelleğe alınır. Böylece her bit hızı ve protokol ayrı olarak önbelleğe alınır ve yalnızca istenen video parçaları önbelleğe alınır.

Media Services [akış uç NOKTASıNDA](streaming-endpoint-concept.md)CDN 'nin etkinleştirilip etkinleştirilmeyeceğini saptarken, beklenen görüntüleyiciler sayısını göz önünde bulundurun. CDN, yalnızca içeriğiniz için benimsemeyi bekleme birçok görüntüleyicinizi kullanıyorsanız yardımcı olur. Görüntüleyicinin en fazla eşzamanlılık 500 ' den düşükse, CDN 'nin eşzamanlılık ile en iyi şekilde ölçeklendirilmiş olduğundan CDN 'yi devre dışı bırakmanız önerilir. 

Bu konu, [CDN tümleştirmesini](#enable-azure-cdn-integration)etkinleştirmeyi tartışır. Ayrıca, önceden getirme (etkin önbelleğe alma) ve [kaynak-yardım CDN-önceden getirme](#origin-assist-cdn-prefetch) kavramını da açıklar.

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* [Akış uç noktası](streaming-endpoint-concept.md) `hostname` ve akış URL 'SI, CDN 'yi etkinleştirip etkinleştirmeksizin aynı kalır.
* İçeriğinizi CDN ile veya CDN olmadan test etmek istiyorsanız, CDN etkin olmayan başka bir akış uç noktası oluşturun.

## <a name="enable-azure-cdn-integration"></a>Azure CDN tümleştirmeyi etkinleştir

> [!IMPORTANT]
> Deneme veya öğrenci Azure hesapları için CDN 'yi etkinleştiremezsiniz.
>
> CDN tümleştirmesi, Federal kamu ve Çin bölgeleri dışındaki tüm Azure veri merkezlerinde etkinleştirilir.

CDN etkinken bir akış uç noktası sağlandıktan sonra, akış uç noktasını CDN uç noktası ile eşlemek için DNS güncelleştirme işlemi yapılmadan önce Media Services tanımlı bir bekleme süresi vardır.

Daha sonra CDN 'yi devre dışı bırakmak/etkinleştirmek istiyorsanız, akış uç noktanızın **durdurulmuş** durumda olması gerekir. Azure CDN tümleştirmenin etkinleştirilmesi ve değişikliklerin tüm CDN pop 'larda etkin olması iki saate kadar sürebilir. Ancak, akış uç noktasından kesintiler olmadan akış uç noktanızı ve akışınızı başlatabilir ve tümleştirme tamamlandıktan sonra akış CDN 'den dağıtılır. Sağlama süresi boyunca, akış uç noktanız **Başlangıç** durumunda olur ve performans düşüklüğü gözlemleyebilirsiniz.

Standart akış uç noktası oluşturulduğunda, standart Verizon ile varsayılan olarak yapılandırılır. REST API 'Lerini kullanarak Premium Verizon veya standart Akamai sağlayıcılarını yapılandırabilirsiniz.

Azure CDN ile tümleştirme Azure Media Services standart akış uç noktaları için **Verizon 'tan Azure CDN** uygulanır. Premium akış uç noktaları, tüm **Azure CDN fiyatlandırma katmanları ve sağlayıcıları**kullanılarak yapılandırılabilir. 

> [!NOTE]
> Azure CDN hakkındaki ayrıntılar için bkz. [CDN 'ye genel bakış](../../cdn/cdn-overview.md).

## <a name="determine-if-a-dns-change-was-made"></a>DNS değişikliğinin yapıldığını belirleme

Bir akış uç noktasında DNS değişikliğinin (trafik Azure CDN yönlendirilmekte) https://www.digwebinterface.comkullanılarak saptanamıyor. Sonuçlarda azureedge.net etki alanı adları görürseniz, trafik artık CDN 'ye işaret ediyor.

## <a name="origin-assist-cdn-prefetch"></a>Kaynak-yardımcı CDN-önceden getirme

CDN önbelleği, reaktif bir işlemdir. CDN bir sonraki nesnenin isteneceğini tahmin edebiliyorsa, CDN, bir sonraki nesneyi Pro 'yu etkin bir şekilde ister ve önbelleğe alabilir. Bu işlemle, nesnelerin tümü (veya çoğu) için bir ön isabet elde edebilir, bu nedenle performansı geliştirir.

Önceden getirme kavramı, olasılığına içindeki "Internet Edge" de nesneleri, bu nesnenin Player tarafından istenme süresini azaltarak, bu sayede bu nesneyi yürütücüye sunmaya yönelik süreyi azaltan bir konuma konumlandırır.

Bu hedefe ulaşmak için bir akış uç noktası (Origin) ve CDN 'nin el ile çalışması gerekir: 

- Media Services kaynağı, bir sonraki nesneyi önceden getirme için CDN 'ye bildirmek üzere "zeka" (Origin-yardım) içermelidir ve 
- CDN, önceden getirme ve önbelleğe alma (CDN-önceden getirme bölümü) yapar. CDN 'nin bir önceden getirme mi yoksa düzenli bir getirme mi olduğunu, 404 yanıtlarını işleme ve sonsuz önceden getirme döngüsünden kaçınmak için bir yol olduğunu bildirmek üzere "zeka" sahip olması gerekir.

### <a name="benefits"></a>Avantajlar

*Kaynak-yardım CDN-önceden getirme* özelliğinin avantajları şunları içerir:

- Önceden getirme, video kayıttan yürütme kalitesini şu şekilde geliştirir: oynatma sırasında beklenen video segmentlerini önceden konumlandırma, görüntüleyiciye gecikme süresini azaltma ve video segmenti karşıdan yükleme sürelerini iyileştirme. Bu, daha hızlı video başlangıç zamanına ve düşük yeniden arabelleğe alma oluşumlarına neden olur.
- Bu kavram, medyayla sınırlı olmamak üzere genel CDN-Origin senaryosu için geçerlidir.
- Akamai bu özelliği [Akamai Cloud embed (ACE)](https://learn.akamai.com/en-us/products/media_delivery/cloud_embed.html)öğesine ekledi.

> [!NOTE]
> Bu özellik henüz Media Services akış uç noktası ile tümleşik Akamai CDN için geçerli değildir. Ancak, önceden var olan Akamai sözleşmesi olan ve Akamai CDN ile Media Services kaynağı arasında özel tümleştirme gerektiren Media Services müşteriler için kullanılabilir.

### <a name="how-it-works"></a>Nasıl çalışır?

*Kaynak-yardım CDN-önceden getirme* ÜSTBILGILERI için CDN desteği (hem canlı hem de video isteğe bağlı akış için), Akamai CDN ile doğrudan sözleşme olan müşteriler tarafından kullanılabilir. Özelliği, Akamai CDN ve Media Services kaynağı arasındaki aşağıdaki HTTP üst bilgi alışverişlerini içerir:

|HTTP üstbilgisi|Değerler|Gönderen|Alıcı|Amaç|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-yardım-önceden getirme-etkin | 1 (varsayılan) veya 0 |CDN|Kaynak|CDN 'in önceden getirme etkin olduğunu göstermek için|
|CDN-Origin-yardım-önceden getirme-yol| Örnek: <br/>Parçalar (video = 1400000000, Format = MPD-Time-cmaf)|Kaynak|CDN|CDN 'ye önceden getirme yolu sağlamak için|
|CDN-Origin-yardım-önceden getirme-Istek|1 (önceden getirme isteği) veya 0 (normal istek)|CDN|Kaynak|CDN 'den gelen isteğin bir önceden getirme olduğunu belirtmek için|

Üst bilgi değişimi 'nin bir parçasını eylemde görmek için aşağıdaki adımları deneyebilirsiniz:

1. Bir ses veya video segmenti veya parçası için Media Services kaynağına bir istek vermek üzere Postman veya kıvrımlı kullanın. İstekte CDN-Origin-yardım-önceden getirme-etkin: 1 üst bilgisini eklediğinizden emin olun.
2. Yanıtta, değeri olarak göreli bir yol ile CDN-Origin-yardım-önceden getirme-yol başlığını görmeniz gerekir.

### <a name="supported-streaming-protocols"></a>Desteklenen akış protokolleri 

*Kaynak-yardım CDN-önceden getirme* özelliği, canlı ve isteğe bağlı akış için aşağıdaki akış protokollerini destekler:

* HLS v3
* HLS v4
* HLS CMAF
* TIRE (CSF)
* TIRE (CMAF)
* Kesintisiz Akış

### <a name="faqs"></a>SSS

* CDN önceden getirme işlemi 404 olarak bir önceden getirme yolu URL 'SI geçersizse ne olur? 

    CDN yalnızca 10 saniye (veya yapılandırılmış diğer değer) için 404 yanıtını önbelleğe alacak.
* İsteğe bağlı bir videonuz olduğunu varsayalım. CDN-önceden getirme etkinse, bu özellik bir istemci ilk video kesimini istediğinde, önceden getirme işlemi, sonraki tüm video segmentlerini aynı bit hızında önceden alacak bir döngü başlatacak mı? 

    Hayır, CDN-önceden getirme yalnızca istemci tarafından başlatılan bir istek/yanıt sonrasında gerçekleştirilir. CDN-önceden getirme, önceden getirme döngüsünden kaçınmak için hiçbir şekilde önceden getirme tarafından tetiklenmez. 
* Kaynak-CDN-önceden getirme özelliği her zaman açık olur mi? Nasıl açılıp kapatılabilir? 

    Bu özellik varsayılan olarak kapalıdır. Müşterilerin Akamai API aracılığıyla açık olması gerekir.
* Canlı akış için, sonraki segment veya parça henüz kullanılamıyorsa, kaynak yardımı 'nda ne olur? 

    Bu durumda Media Services kaynağı CDN-Origin-yardım-önceden getirme-yol üst bilgisini sağlamaz ve CDN-önceden getirme gerçekleşmeyecektir.
* *Kaynak Yardımcısı CDN-önceden getirme* dinamik bildirim filtreleriyle nasıl çalışır? 

    Bu özellik bildirim filtresinden bağımsız olarak çalışmaktadır. Sonraki parça bir filtre penceresinin dışında olduğunda, bunun URL 'SI ham istemci bildirimine bakarak ve daha sonra CDN önceden getirme yanıt üst bilgisi olarak döndürüldüğünden de bulunur. Bu, CDN 'nin DASH/HLS/Düzgünleştir bildiriminden filtrelenmiş bir parçanın URL 'sini alması anlamına gelir. Ancak, oynatıcı bu parçayı getirmek için hiçbir şekilde CDN 'ye bir GET isteği yapmaz, çünkü bu parça Player tarafından tutulan DASH/HLS/pürüzsüz bildirimde yer almamaktadır (oyuncu bu parçanın varlığını bilmez).
* DASH/HLS çalma listesi/Düzgünleştir bildirimi önceden getirilsin mi?

    Hayır, DASH MPD, HLS ana çalma listesi, HLS Variant çalma listesi veya düzgün bildirim URL 'SI önceden getirme başlığına eklenmez.
* Önceden hazırlık URL 'Leri göreli veya mutlak mı? 

    Akamai CDN her ikisine de izin veriyorsa, mutlak URL 'Ler kullanmanın hiçbir avantajı olmadığından Media Services kaynağı yalnızca önceden getirme yolu için göreli URL 'Ler sağlar.
* Bu özellik, DRM korumalı içerikle birlikte çalışıyor mu?

    Evet, bu özellik HTTP düzeyinde çalıştığından, hiçbir segment/parçanın kodunu çözer veya ayrıştırır. İçeriğin şifrelenip şifrelenmediğini dikkate almaz.
* Bu özellik sunucu tarafı ad eklemesi (SSAı) ile çalışıyor mu?
    
    Asıl/ana içerik için (ad eklemeden önceki orijinal video içeriği), SSAı kaynak içeriğin zaman damgasını Media Services kaynaktan değiştirmediğinden, bu yana. Bu özelliğin ad içeriğiyle birlikte çalışıp çalışmadığını, ad kaynağının kaynak-Yardım ' ı destekleyip desteklemediğini belirtir. Örneğin, ad içeriği de Azure Media Services (aynı veya ayrı bir kaynak) içinde barındırılıyorsa, ad içerikleri de önceden getirilir.
* Bu özellik UıHD/HEVC içeriğiyle çalışıyor mu?

    Evet.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

[Bu depodaki](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) örnek, .NET ile varsayılan akış uç noktasının nasıl başlatılacağını gösterir.
