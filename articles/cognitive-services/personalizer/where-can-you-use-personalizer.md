---
title: Nerede ve nasıl kullanılır - Personalizer
description: Kişiselleştirici, deneyimi daha iyi hale getirmek, daha iyi iş sonuçları elde etmek veya üretkenliği artırmak için uygulamanızın görüntülenecek doğru öğeyi, eylemi veya ürünü seçebileceği her durumda uygulanabilir.
ms.topic: conceptual
ms.date: 02/18/2020
ms.openlocfilehash: c562d7a1853736204a7a03262547e083bd85fb75
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617675"
---
# <a name="where-and-how-to-use-personalizer"></a>Personalizer nerede ve nasıl kullanılır

Deneyimi daha iyi hale getirmek, daha iyi iş sonuçları elde etmek veya üretkenliği artırmak için uygulamanızın görüntülemek için doğru eylemi (içeriği) seçmesi gereken her durumda Personalizer'ı kullanın.

Personalizer, kullanıcıyı göstermek için hangi eylemi (içeriği) seçmek için güçlendirme öğrenmesini kullanır. Seçim, hizmete gönderilen verilerin miktarına, kalitesine ve dağıtımına bağlı olarak büyük ölçüde değişebilir.

## <a name="example-use-cases-for-personalizer"></a>Personalizer için örnek kullanım örnekleri

* **Niyet açıklama & anlam ayrımı**: kullanıcılarınızın niyetleri kişiselleştirilmiş bir seçenek sunarak net olmadığında daha iyi bir deneyim yaşamalarına yardımcı olun.
* Menüler & seçenekleri için **varsayılan öneriler:** bot kişisel olmayan bir menü veya alternatifler listesi sunmak yerine, ilk adım olarak kişiselleştirilmiş bir şekilde en olası öğeyi önermek var.
* **Bot özellikleri & sesi:** ton, ayrıntılılık ve yazma stili değişebilir botlar için, bu özellikleri değişen düşünün.
* **Uyarı içeriği ne**& bildirim: kullanıcıların ilgisini daha fazla çekmek için uyarılar için hangi metni kullanacağınıze karar verin.
* **Bildirim & uyarı zamanlaması**: kullanıcılara bildirimlerin ne zaman gönderileceklerine daha fazla bilgi verme konusunda kişiselleştirilmiş öğrenme.


## <a name="expectations-required-to-use-personalizer"></a>Personalizer kullanmak için gerekli beklentiler

Personalizer'ı, aşağıdaki yönergeleri karşıladığınız veya uygulayabileceğiniz durumlarda uygulayabilirsiniz.

|Yönerge|Açıklama|
|--|--|
|İş hedefi|Uygulamanız için bir iş veya kullanılabilirlik hedefiniz var.|
|İçerik|Uygulamanızda, kullanıcılara ne göstereceğinize ilişkin bağlamsal bir karar vermenin bu hedefi geliştireceği bir yer vardır.|
|İçerik miktarı|Arama başına sıralamanız gereken 50'den az eyleminvar.|
|Toplam veri|En iyi seçim, kolektif kullanıcı davranışından ve toplam ödül puanından öğrenilebilir ve öğrenilmelidir.|
|Etik kullanım|Kişiselleştirme için makine öğreniminin [kullanımı, sorumlu kullanım yönergelerini](ethics-responsible-use.md) ve seçtiğiniz seçimleri izler.
|En iyi tek seçenek|Bağlamsal karar, sınırlı bir seçenek kümesinden en iyi seçeneği (eylem) sıralamak olarak ifade edilebilir.|
|Atılan sonuç|Uygulamanız için ne kadar iyi çalışılan seçim, kullanıcı davranışının bazı yönünü ölçerek ve bunu bir _[ödül puanıyla](concept-rewards.md)_ ifade ederek belirlenebilir.|
|İlgili zamanlama|Ödül puanı çok fazla kafa karıştırıcı veya dış etken getirmez. Deneme süresi, ödül puanının hala alakalı yken hesaplanabilecek kadar düşüktür.|
|Yeterli bağlam özellikleri|Sıralamanın bağlamını, doğru seçimi yapmaya yardımcı olacağını düşündüğünüz ve kullanıcıya özel tanımlanabilir bilgileri içermeyen en az 5 [özellikten](concepts-features.md) oluşan bir liste olarak ifade edebilirsiniz.|
|Yeterli eylem özellikleri|Her içerik seçimi hakkında bilgi var, _eylem_, Personalizer doğru seçim yapmak yardımcı olacağını düşünüyorum en az 5 [özellikleribir](concepts-features.md) listesi olarak.|
|Günlük veriler|Sorun zaman içinde sürükleniyorsa (haber veya moda tercihleri gibi) en iyi kişiselleştirmenin üstünde kalmak için yeterli etkinlik vardır. Personalizer gerçek dünyadaki sürekli değişime uyum sağlayacaktır, ancak yeni desenleri keşfetmek ve yerleşmek için öğrenilecek yeterli etkinlik ve veri yoksa sonuçlar en iyi olmaz. Yeterince sık gerçekleşen bir kullanım örneği seçmelisiniz. Günde en az 500 kez meydana gelen kullanım durumlarını aramayı düşünün.|
|Geçmiş veriler|Uygulamanız, en az 100.000 etkileşimgeçmişi biriktirecek kadar uzun süre veri saklayabilir. Bu, Personalizer çevrimdışı değerlendirmeler ve ilke optimizasyonu gerçekleştirmek için yeterli veri toplamak için izin verir.|

Kişiselleştirilmiş davranışın tüm kullanıcılar arasında keşfilebilen bir şey olmadığı durumlarda **Kilikatör kullanmayın.** Örneğin, 20 olası menü öğesi listesinden ilk pizza siparişi önermek için Personalizer'ı kullanmak yararlıdır, ancak çocuk bakımı yla ilgili yardım gerektiren kişiler ("Büyükanne" gibi) kullanıcı tabanınızda kişiselleştirilebilir bir şey değildir.

## <a name="how-to-use-personalizer-in-a-web-application"></a>Bir web uygulamasında Personalizer nasıl kullanılır?

Bir web uygulamasına öğrenme döngüsü ekleme şunları içerir:

* Hangi deneyimi kişiselleştireceğiniz, hangi eylemlere ve özelliklere sahip olduğunuzu, hangi bağlam özelliklerini kullanacağınızı ve hangi ödülü belirleyeceğinize karar ayarlayın.
* Uygulamanızdaki Kişiselleştirme SDK'sına bir başvuru ekleyin.
* Kişiselleştirmeye hazır olduğunuzda Rank API'sini arayın.
* EventId'yi saklayın. Daha sonra Ödül API ile bir ödül gönderin.
1. Kullanıcının kişiselleştirilmiş sayfanızı gördüğünden emin olduktan sonra etkinlik için Etkinleştir'i arayın.
1. Sıralanan içeriğin kullanıcı seçimini bekleyin.
1. Rank API çıktısının ne kadar iyi olduğunu belirtmek için Reward API'yi arayın.

## <a name="how-to-use-personalizer-with-a-chat-bot"></a>Nasıl bir sohbet bot u ile Personalizer kullanmak

Bu örnekte, kullanıcıyı her seferinde bir dizi menü veya seçenek göndermek yerine varsayılan bir öneride bulunmak için Kişiselleştirme'yi nasıl kullanacağınızı görürsünüz.

* Bu örneğin [kodunu](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/ChatbotExample) alın.
* Bot çözümünüzü ayarlayın. LUIS uygulamanızı yayımladıknızdan emin olun.
* Bot için Rank ve Reward API çağrılarını yönetin.
    * LUIS amacıyla işlemeyi yönetmek için kod ekleyin. **Hiçbiri** en yüksek amaç olarak döndürülürse veya en yüksek niyetpuanı işletme mantığı eşiğinizin altındaysa, niyet listesini Niyetleri Sıralamak için Personalizer'a gönderin.
    * İlk amaç Rank API yanıtından en üst sırada yer alan niyet olmak üzere seçilebilir bağlantılar olarak kullanıcıya niyet listesini gösterin.
    * Kullanıcının seçimini yakalayın ve bunu Ödül API çağrısına gönderin.

### <a name="recommended-bot-patterns"></a>Önerilen bot desenleri

* Her kullanıcı için önbelleğe alma sonuçları yerine, her zaman bir anlam ayrımı gerektiğinde Personalizer Rank API aramaları yapın. Farklılaşma niyetinin sonucu bir kişi için zaman içinde değişebilir ve Rank API'nin varyansları keşfetmesine izin vermek genel öğrenmeyi hızlandıracaktır.
* Kişiselleştirmek için yeterli veriye sahip olmak için birçok kullanıcıyla ortak olan bir etkileşim seçin. Örneğin, giriş soruları, yalnızca birkaç kullanıcının ulaşabileceği konuşma grafiğindeki daha küçük açıklamalara göre daha uygun olabilir.
* Kullanıcıya "X ister misiniz?" sorulur "ilk öneri doğru" konuşmaları etkinleştirmek için Rank API çağrılarını kullanın. ya da "X'i mi kastettin?" ve kullanıcı sadece teyit edebilirsiniz; bir menüden seçim yapması gereken kullanıcıya seçenekler vermek yerine. Örneğin, Kullanıcı:"Bir kahve sipariş etmek istiyorum" Bot:"Bir çift espresso ister misiniz?". Bu şekilde ödül sinyali de doğrudan bir öneri ile ilgili olarak güçlüdür.

## <a name="how-to-use-personalizer-with-a-recommendation-solution"></a>Bir tavsiye çözümü ile Personalizer nasıl kullanılır

Birçok şirket, büyük bir katalogdan müşterilere ürün önermek için öneri motorları, pazarlama ve kampanya araçları, hedef kitle segmentasyonu ve kümeleme, işbirlikçi filtreleme ve diğer araçları kullanır.

[Microsoft Recommenders GitHub deposu,](https://github.com/Microsoft/Recommenders) Jupyter dizüstü bilgisayarlar olarak sağlanan öneri sistemleri oluşturmak için örnekler ve en iyi uygulamalar sağlar. XDeepFM, SAR, ALS, RBM, DKN gibi birçok yaygın yaklaşım için veri hazırlama, modeller oluşturma, değerlendirme, tuning ve işletime yönelik çalışma örnekleri sağlar.

Personalizer mevcut olduğunda bir tavsiye motoru ile çalışabilir.

* Öneri motorları büyük miktarda öğe (örneğin, 500.000) alır ve yüzlerce veya binlerce seçenekten bir alt küme (ilk 20 gibi) önerir.
* Personalizer onlar hakkında çok sayıda bilgi ile eylemleri az sayıda alır ve belirli bir zengin bağlam için gerçek zamanlı olarak sıralar, çoğu tavsiye motorları sadece kullanıcılar hakkında birkaç öznitelikleri kullanırken, ürünler ve etkileşimleri.
* Personalizer, haberler, canlı etkinlikler, canlı topluluk içeriği, günlük güncellemelerle içerik veya mevsimsel içerik gibi içeriğin hızla değiştiği yerlerde daha iyi sonuçlar veren kullanıcı tercihlerini her zaman bağımsız olarak keşfetmek üzere tasarlanmıştır.

Yaygın bir kullanım bir tavsiye motoru (örneğin, belirli bir müşteri için en iyi 20 ürün) çıktı almak ve Personalizer için giriş eylemleri olarak kullanmaktır.

## <a name="adding-content-safeguards-to-your-application"></a>Uygulamanız için içerik korumaları ekleme

Uygulamanız kullanıcılara gösterilen içerikte büyük farklılıklara izin veriyorsa ve bu içeriğin bir kısmı bazı kullanıcılar için güvenli olmayabilir veya uygunsuz olabilirse, kullanıcılarınızın kabul edilemez içeriği görmesini önlemek için doğru güvenlik önlemlerinin mevcut olduğundan emin olmak için önceden plan yapmalısınız. Güvenlik önlemlerini uygulamak için en iyi model:
    * Sıralamak için eylemlerin listesini edinin.
    * İzleyiciler için uygun olmayanları filtreleyin.
    * Sadece bu uygun eylemleri sırala.
    * En üst sırada yer alan eylemi kullanıcıya görüntüleyin.

Bazı mimarilerde, yukarıdaki sırayı uygulamak zor olabilir. Bu durumda, sıralamadan sonra güvenlik önlemlerinin uygulanmasına alternatif bir yaklaşım vardır, ancak personalizer modelini eğitmek için korumanın dışında kalan eylemlerin kullanılmaması için bir hükmün uygulanması gerekir.

* Öğrenme devre dışı bırakıldığında sıralamak için eylemlerin listesini alın.
* Eylemleri sırala.
* Üst eylemin uygun olup olmadığını kontrol edin.
    * En iyi eylem uygunsa, bu sıralama için öğrenmeyi etkinleştirin ve kullanıcıya gösterin.
    * En iyi eylem uygun değilse, bu sıralama için öğrenmeyi etkinleştirmeyin ve kullanıcıya neyi göstereceğine kendi mantığınız veya alternatif yaklaşımlarınızla karar verin. İkinci en iyi sıralanan seçeneği kullansanız bile, bu sıralama için öğrenmeyi etkinleştirmeyin.


## <a name="next-steps"></a>Sonraki adımlar

[Etik & sorumlu kullanım](ethics-responsible-use.md).
