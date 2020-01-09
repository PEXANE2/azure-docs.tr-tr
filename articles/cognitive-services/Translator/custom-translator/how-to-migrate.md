---
title: Microsoft Translator Hub çalışma ve projeleri geçişini? -Özel Translator
titleSuffix: Azure Cognitive Services
description: Bu makalede, hub çalışma alanınızı ve projelerinizi Azure bilişsel hizmetler özel çeviricisine nasıl geçirebileceğiniz açıklanır.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: 2fa90a8099778bf37ce8534e968a2b1b4345c2d8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446776"
---
# <a name="migrate-hub-workspace-and-projects-to-custom-translator"></a>Hub çalışma ve projeleri için özel Translator geçirme

Kolayca geçirebilirsiniz, [Microsoft Translator Hub](https://hub.microsofttranslator.com/) çalışma alanı ve özel Translator projelerinin. Geçiş, bir çalışma alanı veya proje, ardından çalışma alanı seçme içinde özel Translator ve ardından aktarmak istediğiniz eğitimleri seçerek Microsoft Hub'ından başlatılır. Geçiş başladıktan sonra seçilen eğitim ayarları ile ilgili tüm belgelerinin aktarılır. Dağıtılan modellerinde eğitilir ve tamamlandıktan sonra autodeployed olabilir.

Bu Eylemler, geçiş sırasında gerçekleştirilir:
* Tüm belgeler ve proje tanımları "hub_ adının önüne" öğesine aktarılan adları gerekir. Otomatik olarak oluşturulan test ve ayarlama verileri hub_systemtune_\<ModelId > veya hub_systemtest_\<ModelId > olarak adlandırılır.
* Geçiş başladığında, dağıtılmış durumda olan tüm eğitimleri Hub eğitim belgeleri kullanarak otomatik olarak Eğitilecek. Bu eğitim, aboneliğiniz için ücretlendirilmez. Otomatik dağıtımı seçildi geçiş için eğitilen modeli tamamlanmasından sonra dağıtılacak. Normal barındırma ücretleri uygulanır.
* Dağıtılmış durumda değildi geçirilen tüm eğitimleri geçirilen taslak durumuna yerleştirilir. Bu durumda, geçirilen tanımıyla modeli seçeneğine sahip olur, ancak normal eğitim ücretleri geçerli olacaktır.
* Herhangi bir noktada, eğitim, "Bleu puan" MT hub'ında modelinde TrainingDetails sayfasında bulunabilir Hub'ından BLEU puanı geçişi başlığı.

> [!Note] 
> Bir eğitimin başarılı olması için, özel çevirmen en az 10.000 benzersiz ayıklanan tümce gerektirir. Özel çevirmen [Önerilen en düşük](https://docs.microsoft.com/azure/cognitive-services/translator/custom-translator/sentence-alignment#suggested-minimum-number-of-sentences)boyuttan daha az bir eğitim yapamıyor.

## <a name="find-custom-translator-workspace-id"></a>Özel Translator çalışma alanı kimliği bulunamıyor

Geçirilecek [Microsoft Translator Hub](https://hub.microsofttranslator.com/) çalışma alanında, hedef çalışma alanı Kimliğini özel Translator gerekir. Burada tüm Hub çalışma alanları ve projeler için geçirilmesi özel Translator, hedef çalışma alanıdır.

Özel Translator Ayarları sayfasında, hedef çalışma alanı kimliği bulacaksınız:

1. Özel Translator Portalı'nda "Ayarını" sayfasına gidin.

2. Çalışma alanı kimliği temel bilgileri bölümünde bulabilirsiniz.

    ![Hedef çalışma alanı kimliği bulma](media/how-to/how-to-find-destination-ws-id.png)

3. Geçiş işlemi sırasında başvurmak için çalışma alanı kimliği, hedef tutun.

## <a name="migrate-a-project"></a>Bir projeyi geçir

Microsoft Translator Hub projelerinizi seçmeli olarak geçirmek istiyorsanız, bu özelliği sunar.

Bir projeyi geçirmek için:

1. Microsoft Translator Hub'ına oturum açın.

2. "Projeler" sayfasına gidin.

3. Uygun bir proje için "Geçiş" bağlantısına tıklayın.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-from-hub.png)

4. Taşıma bağlantısını bastığınız andan, olanak tanıyan bir form sunulur:
   * Özel Translator üzerinde aktarmak istediğiniz çalışma alanını belirtin
   * Tüm eğitimleri başarılı eğitimleri veya yalnızca dağıtılan eğitimleri ile Aktarım isteyip istemediğinizi belirtin. Varsayılan olarak tüm başarılı eğitimleri aktarılır.
   * Eğitim tamamlandığında dağıtılan, eğitim otomatik isteyip istemediğinizi belirtin. Varsayılan olarak, otomatik tamamlanmasından sonra dağıtılan eğitim olmayacaktır.

5. "İstek Gönder" e tıklayın.

## <a name="migrate-a-workspace"></a>Bir çalışma alanı geçirme

Tek bir projeye geçiş ek olarak, tüm projeleri bir çalışma alanında başarılı eğitimleri ile taşıyabilirsiniz. Bu her proje çalışma alanında geçiş bağlantısı basılmış gibi sorgulamanıza değerlendirilecek neden olur. Bu özellik, bunların tümünün aynı ayarlarla özel Translator'a geçiş yapmak istiyorsanız, birden çok projeli kullanıcılar için uygundur. Bir çalışma alanı geçişi Translator Hub Ayarları sayfasından başlatılabilir.

Bir çalışma alanı geçirmek için:

1. Microsoft Translator Hub'ına oturum açın.

2. "Ayarlar" sayfasına gidin.

3. "Ayarlar" sayfasında "Özel Translator geçiş çalışma alanında veri" tıklayın.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-workspace-from-hub.png)

4. Sonraki sayfada, bu iki seçeneklerinden birini seçin:

    a. Yalnızca eğitimleri dağıtılan: Bu seçeneğin belirlenmesi geçirme yalnızca dağıtılan sistemler ve ilgili belgeler.

    b. Tüm başarılı eğitimleri: Bu seçeneğin belirlenmesi, başarılı eğitimleri ve ilgili belgelerin geçirir.

    c. İçinde özel Translator, hedef çalışma alanı kimliği girin.

    ![Hub'ından geçirme](media/how-to/how-to-migrate-from-hub-screen.png)

5. İstek Gönder'e tıklayın.

## <a name="migration-history"></a>Geçiş geçmişi

Çalışma alanı istediniz / Hub'ından geçiş projesi, geçiş geçmişini özel Translator Ayarları sayfasında bulabilirsiniz.

Geçiş geçmişini görüntülemek için aşağıdaki adımları izleyin:

1. Özel Translator Portalı'nda "Ayarını" sayfasına gidin.

2. Geçiş geçmişini Ayarlar sayfasında geçiş geçmişini bölümüne tıklayın.

    ![Geçiş geçmişi](media/how-to/how-to-migration-history.png)

Geçiş geçmişi sayfası aşağıdaki istediğiniz her geçiş için Özet bilgiler görüntüler.

1. Tarafından geçirilen: Ad ve e-posta kullanıcının bu geçiş isteği gönderildi

2. Üzerinde geçişi: Tarih ve zaman damgası geçiş

3. Projeler: İstenen proje v/sn sayısı geçiş başarıyla geçirildi projeleri sayısı.

4. Eğitimleri: İstenen geçiş v/sn sayısı eğitimleri başarıyla geçirildi eğitimleri sayısı.

5. Belgeler: Belge v/sn sayısı geçiş başarıyla geçirildi istenen belge sayısı.

    ![Geçiş geçmişi ayrıntıları](media/how-to/how-to-migration-history-details.png)

Geçiş raporu projeleri, eğitimleri ve belgeleri hakkında daha ayrıntılı istiyorsanız ayrıntıları csv dosyası olarak dışarı aktarma seçeneğiniz vardır.

## <a name="implementation-notes"></a>Uygulama Notları
* Yalnızca özel çevirmende bulunmayan dil çiftlerine sahip sistemler, verilere erişmek veya özel çevirici aracılığıyla dağıtımı geri almak için kullanılabilir. Bu projeler, Projeler sayfasında "kullanılamaz" olarak işaretlenir. Özel çevirmenle yeni dil çiftlerini etkinleştirdiğimiz için, projeler eğecek ve dağıtılacak şekilde etkin hale gelir. 
* Bir proje hub'dan özel Translator'a geçiş Hub eğitimleri veya projeleri herhangi bir etkisi yoktur. Biz projeleri veya belgeler hub'dan bir geçiş sırasında silmeyin ve biz modelleri dağıtımını kaldırmayı değil.
* Yalnızca proje bir kez geçirmeye izin verilir. Bir geçiş projesi üzerinde yinelemek ihtiyacınız varsa lütfen bizimle iletişime geçin.
* Özel çevirici, Ingilizce veya Ingilizce için NMT dil çiftlerini destekler. [Desteklenen dillerin tüm listesini görüntüleyin](https://docs.microsoft.com/azure/cognitive-services/translator/language-support#customization). Hub temel modelleri gerektirmez ve bu nedenle birkaç bin dilleri destekler. Ancak, yalnızca belgeleri geçirmek eder ve tanımları proje bir desteklenmeyen dili çifti geçirebilirsiniz. Biz yeni modeli eğitmek mümkün olmayacaktır. Ayrıca, bu belge ve projeleri etkin değil olarak şu anda kullanılamaz olduğunu belirtmek için görüntülenir. Bu projeler ve/veya belgeler için ek destek, etkin ve trainable olur.
* Özel Translator tek dilli Kinsoku'ya eğitim verilerini şu anda desteklemiyor. Desteklenmeyen dil çiftleri gibi tek dilli Kinsoku'ya belgeleri geçirebilirsiniz, ancak tek dilli Kinsoku'ya veri desteklenen kadar bunlar devre dışı olarak göster.
* Özel Translator eğitmek için 10 k paralel cümleler gerektirir. Microsoft Hub daha küçük bir veri kümesini eğitme. Bu gereksinimi karşılamayan bir eğitim geçirilirse, bu işlem eğitilemez.

## <a name="custom-translator-versus-hub"></a>Özel Translator Hub karşılaştırması

Bu tabloda, Microsoft Translator Hub özel Translator arasındaki özellikler karşılaştırılmaktadır.

|   | Hub | Özel Çevirmen |
|:-----|:----:|:----:|
|Özelleştirme özelliği durumu   | Genel Erişilebilirlik  | Genel Erişilebilirlik |
| Metin çevirisi API'si sürümü  | V2    | V3  |
| SMT özelleştirme | Evet   | Hayır |
| NMT özelleştirme | Hayır    | Evet |
| Yeni birleşik konuşma Hizmetleri özelleştirme | Hayır    | Evet |
| İzleme Yok | Evet | Evet |

## <a name="new-languages"></a>Yeni diller

Microsoft Translator için yeni bir dil sistemi oluşturma konusunda çalışan bir topluluk veya kuruluşunuzda daha fazla bilgi edinmek için [custommt@microsoft.com](mailto:custommt@microsoft.com) ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir model eğitip](how-to-train-model.md).
- Dağıtılan özel çeviri modeliniz aracılığıyla kullanmaya başlama [Microsoft Translator Text API v3 sürümüne](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl).
