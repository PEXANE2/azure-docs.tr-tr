---
title: Bing Özel Arama deneyiminizi yapılandırın | Microsoft Dokümanlar
titleSuffix: Azure Cognitive Services
description: Portal, web dilimlerini belirten bir arama örneği oluşturmanıza olanak tanır; etki alanları, alt sayfalar ve web sayfaları.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: c14376cc80373371ec5fcb8f22a00584a6b2f714
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79220220"
---
# <a name="configure-your-bing-custom-search-experience"></a>Bing Özel Arama deneyiminizi yapılandırın

Özel Arama örneği, arama deneyimini yalnızca kullanıcılarınızın önemsediği web sitelerinden gelen içeriği içerecek şekilde uyarlamanıza olanak tanır. Bing, web genelinde arama yapmak yerine yalnızca web'in ilginizi çeken dilimlerini arar. Size özel web görünümünü oluşturmak için Bing Özel Arama [portalını](https://customsearch.ai) kullanın.

Portal, web'in dilimlerini belirten bir arama örneği oluşturmanıza olanak tanır: etki alanları, alt sayfalar ve web sayfaları, Bing'in aramasını istediğiniz ve aramasını istemediğiniz alanlar. Portal ayrıca eklemek isteyebileceğiniz içerikönerebilir.

Web dilimlerinizi tanımlarken aşağıdakileri kullanın:

| Dilim adı | Açıklama                                                                                                                                                                                                                                                                                                |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Domain     | Etki alanı dilimi, internet etki alanında bulunan tüm içeriği içerir. Örneğin, `www.microsoft.com`. Atlayış `www.` Bing'in etki alanının alt etki alanlarında da arama sına neden olur. Örneğin, belirtirseniz, `microsoft.com`Bing de sonuçları `support.microsoft.com` `technet.microsoft.com`döndürür veya . |
| Alt Sayfa    | Alt sayfa dilimi, alt sayfada bulunan tüm içeriği ve altındaki yolları içerir. Yolda en fazla iki alt sayfa belirtebilirsiniz. Örneğin, `www.microsoft.com/en-us/windows/`                                                                                                                       |
| Web sayfasının    | Bir web sayfası dilimi, özel bir aramada yalnızca bu web sayfasını içerebilir. Alt sayfaların eklenip eklenmeyeceğini isteğe bağlı olarak belirtebilirsiniz.                                                                                                                                                                                  |

> [!IMPORTANT]
> Belirttiğiniz tüm etki alanları, alt sayfalar ve web sayfaları genel olarak açık olmalı ve Bing tarafından dizine eklenmelidir. Aramaya eklemek istediğiniz ortak bir siteniz varsa ve Bing bu siteyi dizine eklemediyse, Bing'in dizine eklemesini sağlamakla ilgili ayrıntılar için Bing [web yöneticisi belgelerine](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) bakın. Ayrıca, dizin güncel değilse, Bing'in taranmış sitenizi güncelleştirmesini sağlamakla ilgili ayrıntılar için web yöneticisi belgelerine bakın.

## <a name="add-slices-of-the-web-to-your-custom-search-instance"></a>Özel arama örneğinize web dilimleri ekleme

Özel arama örneğinizi oluşturduğunuzda, web'in dilimlerini belirtebilirsiniz: arama sonuçlarınızdan dahil etmek veya engellemek istediğiniz etki alanları, alt sayfalar ve web sayfaları. 

Özel arama örneğinize eklemek istediğiniz dilimleri biliyorsanız, bunları örneğinizin **Etkin** listesine ekleyin. 

Hangi dilimlerin ekleneceğinden emin değilseniz, **Önizleme** bölmesinde Bing'e arama sorguları gönderebilir ve istediğiniz dilimleri seçebilirsiniz. Bunu yapmak için: 

1. Önizleme bölmesindeki açılır listeden "Bing"i seçin ve bir arama sorgusu girin

2. Eklemek istediğiniz sonucun yanındaki **site ekle'yi** tıklatın. Daha sonra Tamam'a tıklayın.

>[!NOTE]
> [!INCLUDE[publish or revert](./includes/publish-revert.md)]

<a name="active-and-blocked-lists"></a>

### <a name="customize-your-search-experience-with-active-and-blocked-lists"></a>Etkin ve Engellenen listelerle arama deneyiminizi özelleştirin 

Özel arama örneğinizdeki **Etkin** ve **Engellenen** sekmeleri tıklatarak etkin ve engellenen dilimler listesine erişebilirsiniz. Etkin listeye eklenen dilimler özel aramanıza dahil edilir. Engellenen dilimler aranmaz ve arama sonuçlarınızda görünmez.

Bing'in aramasını istediğiniz web dilimlerini belirtmek için **Etkin** sekmesini tıklatın ve bir veya daha fazla URL ekleyin. URL'leri düzenlemek veya silmek için **Denetimler** sütununaltındaki seçenekleri kullanın. 

**Etkin** listeye URL eklerken, yükleme simgesini kullanarak bir metin dosyası yükleyerek aynı anda tek URL veya birden fazla URL ekleyebilirsiniz.

![Bing Özel Arama Etkin sekmesi](media/file-upload-icon.png)

Dosya yüklemek için bir metin dosyası oluşturun ve satır başına tek bir etki alanı, alt sayfa veya web sayfası belirtin. Doğru biçimlendirilmemişse dosyanız reddedilir.

> [!NOTE]
> * Bir dosyayı yalnızca **Etkin** listesine yükleyebilirsiniz. **Engellenenler** listesine dilim eklemek için kullanamazsınız.  
> * **Engellenenler** listesinde yükleme dosyasında belirttiğiniz bir etki alanı, alt sayfa veya web sayfası varsa, **engellenenler** listesinden kaldırılır ve **Etkin** listesine eklenir.
> * Yükleme dosyanızdaki yinelenen girişler Bing Özel Arama tarafından yoksayılır. 

### <a name="get-website-suggestions-for-your-search-experience"></a>Arama deneyiminiz için web sitesi önerileri alın

**Etkin** listeye web dilimleri ekledikten sonra, Bing Özel Arama portalı sekmenin alt kısmında web sitesi ve alt sayfa önerileri oluşturur. Bunlar, Bing Özel Arama'nın eklemek isteyebileceğini düşündüğü dilimlerdir. Özel arama örneğinizin ayarlarını güncelleştirdikten sonra güncelleştirilmiş öneriler almak için **Yenile'yi** tıklatın. Bu bölüm yalnızca öneriler varsa görünür.

## <a name="search-for-images-and-videos"></a>Resim ve video ara

Bing Özel Resim Arama API'sini veya Bing Özel Video Arama [API'sini](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-videos-api-v7-reference)kullanarak web içeriğine benzer resim ve videoları arayabilirsiniz. [Bing Custom Image Search API](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-images-api-v7-reference) Bu sonuçları [barındırılan ui](hosted-ui.md)veya API'lerle görüntüleyebilirsiniz. 

Bu API'ler özel olmayan [Bing Resim Arama](../Bing-Image-Search/overview.md) ve Bing Video [Arama](../Bing-Video-Search/search-the-web.md) API'lerine benzer, ancak tüm web'de arama yapar ve sorgu parametresini `customConfig` gerektirmez. Resimler ve videolar ile çalışma hakkında daha fazla bilgi için bu dokümantasyon kümelerini görün. 

## <a name="test-your-search-instance-with-the-preview-pane"></a>Arama örneğini Önizleme bölmesiyle test edin

Arama sorguları göndermek ve sonuçları görüntülemek için portalın sağ tarafındaki önizleme bölmesini kullanarak arama örneğini test edebilirsiniz. 

1. Arama kutusunun altında **Örneğim'i**seçin. **Bing'i**seçerek arama deneyiminizdeki sonuçları Bing ile karşılaştırabilirsiniz. 
2. Güvenli bir arama filtresi seçin ve hangi pazarda arama yapacaklarını seçin [(Bkz. Sorgu Parametreleri).](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#query-parameters)
3. Geçerli yapılandırmadan sonuçları görüntülemek için bir sorgu girin ve gir'e basın veya arama simgesine tıklayın. Karşılık gelen sonuçları almak için **Web,** **Resim**veya **Video'ya** tıklayarak gerçekleştirdiğiniz arama türünü değiştirebilirsiniz. 

<a name="adjustrank"></a>

## <a name="adjust-the-rank-of-specific-search-results"></a>Belirli arama sonuçlarının sıralamasını ayarlama

Portal, belirli etki alanlarından, alt sayfalardan ve web sayfalarından gelen içeriğin arama sıralamasını ayarlamanızı sağlar. Önizleme bölmesine bir arama sorgusu gönderdikten sonra, her arama sonucu bunun için yapabileceğiniz ayarlamaların bir listesini içerir:  

|            |                                                                                                                                                                      |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Blok      | Etki alanını, alt sayfayı veya web sayfasını Engellenenler listesine taşır. Bing, seçilen sitedeki içeriğin arama sonuçlarında görünmesini hariç tutar.                    |
| Artırmak      | Arama sonuçlarında daha yüksek olması için etki alanından veya alt sayfadan gelen içeriği artırır.                                                                                        |
| İndirgeme     | Arama sonuçlarında etki alanından veya alt sayfadan içeriği düşürür. Web sayfasının ait olduğu etki alanından veya alt sayfasından içeriği düşürüp düşürmeyeceğinizi seçersiniz. |
| En üste sabitleme | Etki alanını, alt sayfayı veya web sayfasını **Sabitlenmiş** listeye taşır. Bu, web sayfasını belirli bir arama sorgusunun en üst arama sonucu olarak görünmeye zorlar.                   |

Resim veya video aramaları için sıralamayı ayarlama kullanılamıyor.

### <a name="boosting-and-demoting-search-results"></a>Arama sonuçlarını artırma ve düşürme

**Etkin** listesindeki herhangi bir etki alanını veya alt sayfasını süper olarak artırabilir, öne çıkarabilir veya düşürebilirsiniz. Varsayılan olarak, tüm dilimler sıralama ayarlamaları yapılmadan eklenir. Süper öne çıkan veya Öne Çıkan web dilimleri arama sonuçlarında daha yüksek sıralanır (süper artış sıralaması öne sürülmeden daha yüksektir). İndirilen öğeler arama sonuçlarında daha düşük sıralanır.

**Etkin** listedeki **Sıralama Ayarlama** denetimlerini kullanarak veya Önizleme bölmesindeki Öne Çıkarma ve Düşür denetimlerini kullanarak öğeleri süper olarak artırabilir, artırabilir veya düşürebilirsiniz. Hizmet, dilimi Etkin listenize ekler ve sıralamayı buna göre ayarlar.

> [!NOTE] 
> Etki alanlarını ve alt sayfaları artırma ve düşürme, Bing Özel Arama'nın arama sonuçlarının sırasını belirlemek için kullandığı birçok yöntemden biridir. Farklı web içeriğinin sıralamasını etkileyen diğer faktörler nedeniyle, sıralamayı ayarlamanın etkileri değişebilir. Arama sonuçlarınızın sıralamasını ayarlamanın etkilerini test etmek için Önizleme bölmesini kullanın. 

Görüntü ve video aramaları için süper destek, artırma ve düşürme kullanılamıyor.

## <a name="pin-slices-to-the-top-of-search-results"></a>Dilimleri arama sonuçlarının en üstüne sabitleme

Portal ayrıca, **Pinned** sekmesini kullanarak belirli arama terimleri için URL'leri arama sonuçlarının en üstüne sabitlemenizi sağlar. Arama sorgusu başına en fazla bir web sayfasını sabitleyebileceğinizi ve aramalarda yalnızca dizine eklenmiş web sayfalarının görüntüleneceğini unutmayın. Sabitleme sonuçları görüntü veya video aramaları için kullanılamaz.

Bir web sayfasını en üste iki şekilde sabitleyebilirsiniz:

* **Sabitlenmiş** sekmesine, web sayfasının URL'sini girin ve ilgili sorgusunu en üste sabitleyin.

* **Önizleme** bölmesinde, bir arama sorgusu girin ve arama'yı tıklatın. Sorgunuz için sabitlemek istediğiniz web sayfasını bulun ve **en üste Sabitle'yi**tıklatın. web sayfası ve sorgu **Sabitlenmiş** listeye eklenir.

### <a name="specify-the-pins-match-condition"></a>Pinin eşleşme durumunu belirtin

Varsayılan olarak, web sayfaları yalnızca bir kullanıcının sorgu dizesi Sabitlenmiş ler listesinde listelenen bir diziyle eşleştiğinde arama sonuçlarının en üstüne **sabitlenir.** Aşağıdaki eşleşme koşullarından birini belirterek bu davranışı değiştirebilirsiniz:

> [!NOTE]
> Kullanıcının arama sorgusu ile pinin arama sorgusu arasındaki tüm karşılaştırmalar büyük/küçük harf duyarsızdır.

| Değer | Açıklama                                                                          |
|---------------|----------------------------------------------------------------------------------|
| İle başlar | Kullanıcının sorgu dizesi pinin sorgu dizesiyle başlarsa pin eşleşir |
| Ile biter   | Kullanıcının sorgu dizesi pinin sorgu dizesiyle biterse pin eşleşir.  |
| Contains    | Kullanıcının sorgu dizesi pinin sorgu dizesini içeriyorsa, pin eşleşir.   |


Pinin eşleşme durumunu değiştirmek için, pinin edit simgesini tıklatın. Sorgu **maç durumu** sütununda açılır listeyi tıklatın ve kullanılacak yeni koşulu seçin. Ardından, değişikliği kaydetmek için kaydet simgesini tıklatın.

### <a name="change-the-order-of-your-pinned-sites"></a>Sabitlenmiş sitelerinizin sırasını değiştirme

Pinlerinizin sırasını değiştirmek için, **pinned** listesinin **Denetimler** Sütunu'ndaki "düzenle" simgesine tıklayarak bunları sürükleyip bırakabilirsiniz veya sipariş numarasını düzenleyebilirsiniz.

Birden çok pin bir eşleşme koşulunu karşılarsa, Bing Özel Arama listedeki en yüksek pini kullanır.

## <a name="view-statistics"></a>İstatistikleri görüntüleme

Uygun düzeyde Özel Arama'ya abone olduysanız [(fiyatlandırma sayfalarına](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)bakın), üretim örneklerinize bir **İstatistik** sekmesi eklenir. İstatistikler sekmesi, arama hacmi, üst sorgular, coğrafi dağılım, yanıt kodları ve güvenli arama dahil olmak üzere Özel Arama uç noktalarınızın nasıl kullanıldığıyla ilgili ayrıntıları gösterir. Sağlanan denetimleri kullanarak ayrıntıları filtreleyebilirsiniz.

## <a name="usage-guidelines"></a>Kullanım yönergeleri

- Her özel arama örneği **için, Etkin** ve **Engellenen** dilimlerde yapabileceğiniz maksimum sıralama ayarlamasayısı 400 ile sınırlıdır.
- Etkin veya Engellenen sekmelere bir dilim eklemek tek bir sıralama ayarlaması olarak sayılır.
- Artırma ve düşürme sayısı iki sıralama ayarlaması olarak sayılır.
- Her özel arama örneği için, yapacağınız maksimum pin sayısı 200 ile sınırlıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel arama örneğinizi çağırma](./search-your-custom-view.md)
- [Barındırılan kullanıcı arabirimi deneyiminizi yapılandırma](./hosted-ui.md)
- [Metni vurgulamak için süsleme işaretçilerini kullanma](../bing-web-search/hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
