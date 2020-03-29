---
title: Microsoft Translator Hub çalışma alanını ve projelerini geçirin? - Özel Çevirmen
titleSuffix: Azure Cognitive Services
description: Bu makalede, Hub çalışma alanınızı ve projelerinizi Azure Bilişsel Hizmetler Özel Çevirmenine nasıl geçirilir.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 2fa90a8099778bf37ce8534e968a2b1b4345c2d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75446776"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Hub çalışma alanını ve projeleri Özel Çevirmene geçirin

[Microsoft Translator Hub](https://hub.microsofttranslator.com/) çalışma alanınızı ve projelerinizi kolayca Özel Çevirmen'e geçirebilirsiniz. Geçiş, Microsoft Hub'dan bir çalışma alanı veya proje seçerek, ardından Özel Çevirmen'de bir çalışma alanı seçerek ve sonra aktarmak istediğiniz eğitimleri seçerek başlatılır. Geçiş başladıktan sonra, seçilen eğitim ayarları ilgili tüm belgelerle birlikte aktarılır. Dağıtılan modeller eğitilir ve tamamlandıktan sonra otomatik olarak dağıtılabilir.

Bu eylemler geçiş sırasında gerçekleştirilir:
* Tüm belgeler ve proje tanımları adlarına önceden belirlenmiş "hub_" eklenmesiyle adlarını aktaracaktır. Otomatik oluşturulan test ve tuning verileri\<modellenmiş>\<veya hub_systemtest_ modellenmiş> hub_systemtune_ adlandırılacaktır.
* Geçiş gerçekleştiğinde dağıtılan durumda olan tüm eğitimler, Hub eğitiminin belgeleri kullanılarak otomatik olarak eğitilir. Bu eğitim aboneliğinizden ücretlendirilmez. Geçiş için otomatik dağıtım seçildiyse, eğitilen model tamamlandıktan sonra dağıtılır. Düzenli barındırma ücretleri uygulanacaktır.
* Konuşlandırılmış durumda olmayan tüm göç eğitimleri, geçirilen taslak duruma geçirilir. Bu durumda, geçirilen tanımı olan bir modeli eğitim seçeneğiniz olacaktır, ancak düzenli eğitim ücretleri geçerli olacaktır.
* Herhangi bir noktada, Hub eğitiminden geçirilen BLEU puanı, modelin TrainingDetails sayfasında "MT Hub'daki Bleu puanı" başlığında bulunabilir.

> [!Note] 
> Bir eğitimin başarılı olabilmek için, Custom Translator en az 10.000 benzersiz çıkarılmış cümle gerektirir. Özel Çevirmen [önerilen minimumdan](https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/sentence-alignment#suggested-minimum-number-of-sentences)daha az bir eğitim yürütemez.

## <a name="find-custom-translator-workspace-id"></a>Özel Çevirmen Çalışma Alanı Kimliği Bul

[Microsoft Translator Hub](https://hub.microsofttranslator.com/) çalışma alanını geçirmek için Özel Çevirmen'de hedef Çalışma Alanı Kimliği'ne ihtiyacınız vardır. Özel Çevirmen'deki hedef çalışma alanı, tüm Hub çalışma alanlarınızın ve projelerinizin geçirilebileceği yerdir.

Hedef Çalışma Alanı Kimliğinizi Özel Çevirmen Ayarları sayfasında bulabilirsiniz:

1. Özel Çevirmen portalındaki "Ayar" sayfasına gidin.

2. Çalışma Alanı Kimliğini Temel Bilgiler bölümünde bulabilirsiniz.

    ![Hedef çalışma alanı kimliği nasıl bulabilirim?](media/how-to/how-to-find-destination-ws-id.png)

3. Geçiş işlemi sırasında başvurmak için hedef Çalışma Alanı kimliğinizi tutun.

## <a name="migrate-a-project"></a>Projeyi geçirme

Projelerinizi seçiçi olarak taşımak istiyorsanız, Microsoft Translator Hub size bu yüzü

Bir projeyi geçirmek için:

1. Microsoft Translator Hub'da oturum açın.

2. "Projeler" sayfasına gidin.

3. Uygun proje için "Geçir" bağlantısını tıklatın.

    ![Hub'dan nasıl geçirilir?](media/how-to/how-to-migrate-from-hub.png)

4. Geçiş linkine bastığınızda size aşağıdakileri sağlayan bir form sunulacaktır:
   * Özel Çevirmen'de aktarmak istediğiniz çalışma alanını belirtin
   * Tüm eğitimleri başarılı eğitimlerle mi yoksa sadece dağıtılan eğitimlerle mi aktarmak istediğinizi belirtin. Varsayılan olarak tüm başarılı eğitimler aktarılır.
   * Eğitim tamamlandığında eğitim otomatik dağıtımınızı isteyip istemeyeceğiniz belirtin. Varsayılan olarak, eğitiminiz tamamlandıktan sonra otomatik olarak dağıtılmayacaktır.

5. "İstek Gönder"i tıklatın.

## <a name="migrate-a-workspace"></a>Çalışma alanını geçirme

Tek bir projeyi geçirmenin yanı sıra, bir çalışma alanında başarılı eğitimlere sahip tüm projeleri de geçirebilirsiniz. Bu, çalışma alanındaki her projenin geçiş bağlantısıbasılmış gibi değerlendirilmesine neden olur. Bu özellik, tüm lerini aynı ayarlarla Özel Çevirmen'e geçirmek isteyen birçok projesi olan kullanıcılar için uygundur. Translator Hub'ın ayarlar sayfasından çalışma alanı geçişi başlatılabilir.

Çalışma alanını geçirmek için:

1. Microsoft Translator Hub'da oturum açın.

2. "Ayarlar" sayfasına gidin.

3. "Ayarlar" sayfasında "Çalışma Alanı verilerini Özel Çevirmene Geçir" seçeneğini tıklayın.

    ![Hub'dan nasıl geçirilir?](media/how-to/how-to-migrate-workspace-from-hub.png)

4. Sonraki sayfada şu iki seçenekten birini seçin:

    a. Yalnızca Dağıtılan Eğitimler: Bu seçeneğin seçilmesi yalnızca dağıtılan sistemlerinizi ve ilgili belgelerinizi geçirecektir.

    b. Tüm Başarılı Eğitimler: Bu seçeneğin seçilmesi tüm başarılı eğitimlerinizi ve ilgili belgelerinizi geçirecektir.

    c. Hedef Çalışma Alanı Kimliğinizi Özel Çevirmen'e girin.

    ![Hub'dan nasıl geçirilir?](media/how-to/how-to-migrate-from-hub-screen.png)

5. İstek Gönder'i tıklatın.

## <a name="migration-history"></a>Geçiş Geçmişi

Hub'dan çalışma alanı/ proje geçişi istediğinizde, geçiş geçmişinizi Özel Çevirmen Ayarları sayfasında bulabilirsiniz.

Geçiş geçmişini görüntülemek için aşağıdaki adımları izleyin:

1. Özel Çevirmen portalındaki "Ayar" sayfasına gidin.

2. Ayarlar sayfasının Geçiş Geçmişi bölümünde, Geçiş Geçmişi'ni tıklatın.

    ![Geçiş geçmişi](media/how-to/how-to-migration-history.png)

Geçiş Geçmişi sayfası, istediğiniz her geçiş için aşağıdaki bilgileri özet olarak görüntüler.

1. Tarafından Geçirilen: Kullanıcının adı ve e-postası bu geçiş isteğini gönderdi

2. Geçirilen Açık: Geçiştarihi ve saat damgası

3. Projeler: Geçiş için istenen proje sayısı v/s başarıyla geçirilen proje sayısı.

4. Eğitimler: Göç için istenen eğitim sayısı v/s başarıyla geçirilen eğitim sayısıdır.

5. Belgeler: Geçiş için istenen belge sayısı v/s başarıyla geçirilen belge sayısı.

    ![Geçiş geçmişi ayrıntıları](media/how-to/how-to-migration-history-details.png)

Projeleriniz, eğitimleriniz ve belgeleriniz hakkında daha ayrıntılı geçiş raporu istiyorsanız, CSV olarak dışa aktarma seçeneğiniz vardır.

## <a name="implementation-notes"></a>Uygulama Notları
* Özel Çevirmen'de henüz bulunmayan dil çiftleri olan sistemler yalnızca verilere erişmek veya Özel Çevirmen aracılığıyla dağıtılmak için kullanılabilir. Bu projeler Projeler sayfasında "Kullanılamıyor" olarak işaretlenecektir. Custom Translator ile yeni dil çiftleri etkinleştirdiğimizde, projeler eğitmek ve dağıtmak için aktif hale gelecektir. 
* Bir projeyi Hub'dan Özel Çevirmen'e geçirmek, Hub eğitimleriniz veya projeleriniz üzerinde hiçbir etki yaratmaz. Geçiş sırasında Hub'daki projeleri veya belgeleri silmeyiz ve modelleri dağıtmayız.
* Proje başına yalnızca bir kez geçiş izniniz vardır. Bir projede geçişi tekrarlamanız gerekiyorsa, lütfen bize ulaşın.
* Custom Translator, İngilizce'den NMT dil çiftlerini destekler. [Desteklenen dillerin tam listesini görüntüleyin.](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization) Hub temel modeller gerektirmez ve bu nedenle birkaç bin dili destekler. Desteklenmeyen bir dil çiftini geçirebilirsiniz, ancak yalnızca belgelerin ve proje tanımlarının geçişini gerçekleştiririz. Yeni modeli eğitemayacağız. Ayrıca, bu belgeler ve projeler şu anda kullanılamayacaklarını belirtmek için etkin değil olarak görüntülenir. Bu projeler ve/veya belgeler için destek eklenirse, aktif ve eğitilebilir hale gelecektir.
* Custom Translator şu anda tek dilli eğitim verilerini desteklememektedir. Desteklenmeyen dil çiftleri gibi, tek dilli belgeleri geçirebilirsiniz, ancak tek dilli veriler desteklenene kadar etkin olmayan olarak gösterin.
* Özel Çevirmen eğitmek için 10k paralel cümleler gerektirir. Microsoft Hub daha küçük bir veri kümesi üzerinde eğitim verebilir. Bu gereksinimi karşılamayan bir eğitim taşınırsa, eğitilmez.

## <a name="custom-translator-versus-hub"></a>Özel Çevirmen ve Hub

Bu tablo, Microsoft Translator Hub ve Özel Çevirmen arasındaki özellikleri karşılaştırır.

|   | Hub | Özel Çevirmen |
|:-----|:----:|:----:|
|Özelleştirme özelliği durumu   | Genel Kullanılabilirlik  | Genel Kullanılabilirlik |
| Metin API sürümü  | V2    | V3  |
| SMT özelleştirme | Evet   | Hayır |
| NMT özelleştirme | Hayır    | Evet |
| Yeni birleştirilmiş Konuşma hizmetleri özelleştirme | Hayır    | Evet |
| İz Yok | Evet | Evet |

## <a name="new-languages"></a>Yeni diller

Microsoft Translator için yeni bir dil sistemi oluşturmaya çalışan bir topluluk [custommt@microsoft.com](mailto:custommt@microsoft.com) veya kuruluşsanız, daha fazla bilgi için ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğit.](how-to-train-model.md)
- Microsoft [Translator Text API V3](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl)üzerinden dağıtılan özel çeviri modelinizi kullanmaya başlayın.
