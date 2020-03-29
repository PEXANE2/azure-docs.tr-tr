---
title: Yenilikler - Dil Anlayışı (LUIS)
description: Bu makale, Azure Bilişsel Hizmetler Dil Anlama API'si hakkındaki haberlerle düzenli olarak güncelleştirilir.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 562f7f8b4974363daab91991e6a8219b352432fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80156013"
---
# <a name="whats-new-in-language-understanding"></a>Dil Anlayışında yenilikler

Hizmetteki yenilikleri öğrenin. Bu öğeler sürüm notları, videolar, blog gönderileri ve diğer bilgi türlerini içerir. Hizmetten haberdar olmak için bu sayfayı yer imi.

## <a name="release-notes"></a>Sürüm notları

### <a name="march-2020"></a>Mart 2020

* TLS 1.2 artık bu hizmete tüm HTTP istekleri için uygulanır. Daha fazla bilgi için Azure [Bilişsel Hizmetler güvenliği'ne](../cognitive-services-security.md)bakın.

### <a name="november-4-2019---ignite"></a>4 Kasım 2019 - Ignite

* Video - [LUIS ve Azure Bilişsel Hizmetleri kullanan Gelişmiş Doğal Dil Anlama (NLU) modelleri | BRK2188](https://www.youtube.com/watch?v=JdJEV2jV0_Y)

* Geliştirilmiş geliştirici üretkenliği
    * Tahmin bitiş [noktası V3](luis-migration-api-v3.md)genel durumu .
    * .lu ([LUDown](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)) biçiminde uygulama alma ve dışa aktarma özelliği. Bu, etkili bir CI/CD işleminin önünü açıyor.
* Dil genişletme
    * Resmi önizlemede [Arapça ve Hintçe.](luis-language-support.md)
* Önceden oluşturulmuş modeller
    * [Önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md) artık genel olarak kullanılabilir (GA)
    * Japon [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md#japanese-entity-support) - yaş, para birimi, sayı ve yüzde V3 desteklenmez.
    * İtalyan [önceden inşa edilmiş varlıklar](luis-reference-prebuilt-entities.md#italian-entity-support) - yaş, para birimi, boyut, sayı ve yüzde çözünürlüğü V2'den değiştirildi.
* [preview.luis.ai portalında](https://preview.luis.ai) gelişmiş kullanıcı deneyimi - karmaşık modellerin oluşturulmasını ve hata ayıklanmasını sağlamak için yenilenmiş etiketleme deneyimi. Önizleme portalı öğreticilerini deneyin:
    * [Yalnızca niyetler](tutorial-intents-only.md)
    * [Decomposable makine öğrenilen varlık](tutorial-machine-learned-entity.md)
* Dil anlama yeteneklerini ilerletin - daha az çabayla [gelişmiş dil modelleri oluşturun.](luis-concept-entity-types.md)
* Model düzeyinde makine öğrenimi özelliklerini tanımlayın ve modellerin diğer modellere sinyal olarak kullanılmasını etkinleştirin, örneğin varlıkları niyetlere ve diğer varlıklara özellik olarak kullanma.
* Yeni, genişletilmiş [sınırlar](luis-boundaries.md) - ifade listeleri ve toplam tümcecikler için daha yüksek maksimum, özellik sınırları olarak yeni model
* Metinden derin hiyerarşi yapısı biçiminde bilgi ayıklayın ve konuşma uygulamalarını daha güçlü hale getirin.

    ![makine de öğrenilen varlık görüntüsü](./media/whats-new/deep-entity-extraction-example.png)

### <a name="september-3-2019"></a>Eylül 3, 2019

* Azure yazma kaynağı - [şimdi geçirin.](luis-migration-authoring.md)
    * Azure kaynağı başına 500 uygulama
    * Uygulama başına 100 sürüm
* Türkiye'den önceden inşa edilmiş kuruluşlara destek
* datetimeV2 için İtalya desteği

### <a name="july-23-2019"></a>Temmuz 23, 2019

* [Tanıyanlar-Metni](https://github.com/microsoft/Recognizers-Text/releases/tag/dotnet-v1.2.3) 1.2.3'e Güncelleştir
    * Yaş, Sıcaklık, Boyut ve Para Birimi tanıyanlar İtalyanca.
    * Şükran Günü tabanlı tarihleri doğru hesaplamak için İngilizce tatil tanıma iyileştirme.
    * Tarih ve Zaman dışı varlıkların yanlış pozitiflerini azaltmak için French DateTime'daki iyileştirmeler.
    * İngilizce DateRange'de takvim/okul/mali yıl ve kısaltmalar için destek.
    * Çince ve Japonca'da Geliştirilmiş PhoneNumber tanıması.
    * İngilizce NumberRange için geliştirilmiş destek.
    * Performans geliştirmeleri.

### <a name="june-24-2019"></a>Haziran 24, 2019

* [OrdinalV2 sonraki,](luis-reference-prebuilt-ordinal-v2.md) önceki ve son gibi sipariş desteklemek için önceden oluşturulmuş varlık. Sadece İngiliz kültürü.

### <a name="may-6-2019---build-conference"></a>6 Mayıs 2019 - //Yapı Konferansı

Build 2019 Konferansı'nda aşağıdaki özellikler yayınlandı:

* [V3 API geçiş kılavuzuönizlemesi](luis-migration-api-v3.md)
* [Geliştirilmiş analiz panosu](luis-how-to-use-dashboard.md)
* [Geliştirilmiş önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md)
* [Dinamik liste varlıkları](luis-migration-api-v3.md#dynamic-lists-passed-in-at-prediction-time)
* [Dış varlıklar](luis-migration-api-v3.md#external-entities-passed-in-at-prediction-time)

## <a name="blogs"></a>Bloglar

[Bot Altyapısı](https://blog.botframework.com/)

## <a name="videos"></a>Videolar

### <a name="2019-ignite-videos"></a>2019 Ignite videoları

[LUIS ve Azure Bilişsel Hizmetleri kullanan Gelişmiş Doğal Dil Anlayışı (NLU) modelleri | BRK2188](https://www.youtube.com/watch?v=JdJEV2jV0_Y)

### <a name="2019-build-videos"></a>2019 Videolar oluşturun

[İşletmenizi bir sonraki nesil için ölçeklendirmek için Azure Konuşma AI'yi nasıl kullanılır?](https://www.youtube.com/watch?v=_k97jd-csuk&feature=youtu.be)

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

[Bilişsel Hizmetler için Azure güncelleştirme duyuruları](https://azure.microsoft.com/updates/?product=cognitive-services)
