---
title: Yenilikler-Language Understanding (LUSıS)
description: Bu makale, Azure bilişsel hizmetler Language Understanding API hakkındaki haberlerle düzenli olarak güncelleştirilir.
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: 8e3b6f729f69866cdb797cdfd1887de4f5e2be05
ms.sourcegitcommit: 086d7c0cf812de709f6848a645edaf97a7324360
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82097736"
---
# <a name="whats-new-in-language-understanding"></a>Language Understanding yenilikleri

Hizmette nelerin yeni olduğunu öğrenin. Bu öğeler sürüm notlarını, Videoları, blog gönderilerini ve diğer bilgi türlerini içerir. Hizmetle güncel kalmasını sağlamak için bu sayfaya yer işareti ekleyin.

## <a name="release-notes"></a>Sürüm notları

### <a name="march-2020"></a>Mart 2020

* TLS 1,2, bu hizmete yönelik tüm HTTP istekleri için de zorlanır. Daha fazla bilgi için bkz. Azure bilişsel [Hizmetler güvenliği](../cognitive-services-security.md).

### <a name="november-4-2019---ignite"></a>4 Kasım 2019-Menite

* Vısıs [ve Azure bilişsel hizmetler kullanan video gelişmiş doğal Language Understanding (NLU) modelleri | BRK2188](https://www.youtube.com/watch?v=JdJEV2jV0_Y)

* Geliştirilmiş geliştirici verimliliği
    * [Tahmin uç noktası v3](luis-migration-api-v3.md)'nin genel kullanılabilirliği.
    * Uygulamaları. lu ([Luaşağı](https://github.com/microsoft/botbuilder-tools/tree/master/packages/Ludown)) biçimiyle içeri ve dışarı aktarma özelliği. Bu, etkin bir CI/CD işleminin yolunu ele alır.
* Dil genişletmesi
    * [Arapça ve Hintçe](luis-language-support.md) genel önizlemede.
* Önceden oluşturulmuş modeller
    * [Önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md) artık genel kullanıma sunuldu (GA)
    * Japonca [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md#japanese-entity-support) -yaş, para birimi, sayı ve yüzde v3 'de desteklenmez.
    * İtalyanca [önceden oluşturulmuş varlıklar](luis-reference-prebuilt-entities.md#italian-entity-support) -yaş, para birimi, boyut, sayı ve yüzde çözünürlüğü v2 'den değiştirilmiştir.
* [Preview.Luis.ai Portal](https://preview.luis.ai) 'da geliştirilmiş Kullanıcı deneyimi-karmaşık modellerin oluşturulmasını ve hata ayıklamasını etkinleştirmek için etiketleme deneyimidir. Önizleme portalı öğreticilerini deneyin:
    * [Yalnızca amaçlar](tutorial-intents-only.md)
    * [Ayrıştırılmış makine tarafından öğrenilen varlık](tutorial-machine-learned-entity.md)
* Gelişmiş dil özelliklerini anlama-daha az çaba ile [Gelişmiş dil modelleri oluşturma](luis-concept-entity-types.md) .
* Model düzeyinde makine öğrenimi özelliklerini tanımlayın ve modellerin diğer modellere sinyal olarak kullanılmasını etkinleştirin. Örneğin, varlıkları amaçlar ve diğer varlıklara özellikler olarak kullanma.
* Yeni, genişletilmiş [sınırlar](luis-limits.md) -tümcecik listeleri ve toplam ifadeler için yüksek maksimum sınır, özellik sınırları olarak yeni model
* Derin hiyerarşi yapısı biçimindeki metinden bilgi ayıklayın ve konuşma uygulamalarını daha güçlü hale getirir.

    ![makine tarafından öğrenilen varlık görüntüsü](./media/whats-new/deep-entity-extraction-example.png)

### <a name="september-3-2019"></a>3 Eylül 2019

* Azure yazma kaynağı- [Şimdi geçirin](luis-migration-authoring.md).
    * Azure kaynağı başına 500 uygulama
    * Uygulama başına 100 sürüm
* Önceden oluşturulmuş varlıklar için Türkçe desteği
* DatetimeV2 için İtalyanca desteği

### <a name="july-23-2019"></a>23 Temmuz 2019

* [Tanıyıcıları-metin](https://github.com/microsoft/Recognizers-Text/releases/tag/dotnet-v1.2.3) ile 1.2.3 güncelleştirme
    * Italyanca ' de yaş, sıcaklık, boyut ve para birimi tanıyıcıları.
    * Tatil tabanlı tarihleri doğru şekilde hesaplamak için Ingilizce olarak tatil tanıma geliştirme.
    * Tarih ve saat olmayan varlıkların yanlış pozitiflerini azaltmak için Fransız Tarih/saatteki geliştirmeler.
    * Ingilizce gün içinde takvim/okul/mali yıl ve kısaltmalar için destek.
    * Çince ve Japonca için, iyileştirilmiş PhoneNumber Tanıma.
    * Ingilizce olarak NumberRange desteği geliştirildi.
    * Performans geliştirmeleri.

### <a name="june-24-2019"></a>24 Haziran 2019

* İleri, önceki ve son gibi sıralamayı desteklemek için [OrdinalV2 önceden oluşturulmuş varlık](luis-reference-prebuilt-ordinal-v2.md) . Yalnızca İngilizce kültürü.

### <a name="may-6-2019---build-conference"></a>6 Mayıs 2019-/sürüm Konferansı

Aşağıdaki özellikler Build 2019 Konferansı 'nda yayımlanmıştır:

* [V3 API geçiş kılavuzu önizlemesi](luis-migration-api-v3.md)
* [Geliştirilmiş analiz panosu](luis-how-to-use-dashboard.md)
* [Geliştirilmiş önceden oluşturulmuş etki alanları](luis-reference-prebuilt-domains.md)
* [Dinamik liste varlıkları](schema-change-prediction-runtime.md#dynamic-lists-passed-in-at-prediction-time)
* [Dış varlıklar](schema-change-prediction-runtime.md#external-entities-passed-in-at-prediction-time)

## <a name="blogs"></a>Bloglar

[Bot Altyapısı](https://blog.botframework.com/)

## <a name="videos"></a>Videolar

### <a name="2019-ignite-videos"></a>2019 Ignite videoları

[LUVE Azure bilişsel hizmetler kullanan gelişmiş doğal Language Understanding (NLU) modeller | BRK2188](https://www.youtube.com/watch?v=JdJEV2jV0_Y)

### <a name="2019-build-videos"></a>2019 derleme videoları

[Azure konuşma AI kullanarak işinizi bir sonraki nesil için ölçeklendirin](https://www.youtube.com/watch?v=_k97jd-csuk&feature=youtu.be)

## <a name="service-updates"></a>Hizmet güncelleştirmeleri

[Bilişsel hizmetler için Azure Update duyuruları](https://azure.microsoft.com/updates/?product=cognitive-services)
