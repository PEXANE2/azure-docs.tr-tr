---
title: Özel Konuşma Tanıma modelini eğitme ve dağıtma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Özel Konuşma Tanıma modellerini eğitme ve dağıtmayı öğrenin. Konuşmayı metne dönüştürme eğitimi, Microsoft Baseline model veya özel bir model için tanıma doğruluğunu iyileştirebilir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 41fdb3d2e69ae39dbe80f21a953fd9fdaa6d1127
ms.sourcegitcommit: 9514d24118135b6f753d8fc312f4b702a2957780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2021
ms.locfileid: "97968475"
---
# <a name="train-and-deploy-a-custom-speech-model"></a>Özel Konuşma Tanıma modeli eğitme ve dağıtma

Bu makalede Özel Konuşma Tanıma modellerini eğitme ve dağıtmayı öğreneceksiniz. Konuşmaya metin modeli eğitimi, Microsoft Baseline model için tanıma doğruluğunu iyileştirebilir. Bir modeli eğitebilmeniz için insan etiketli döküm ve ilgili metin kullanırsınız. Bu veri kümeleri, daha önce karşıya yüklenen ses verileriyle birlikte, konuşma-metin modelini iyileştirmek ve eğleştirmek için kullanılır.

## <a name="use-training-to-resolve-accuracy-problems"></a>Doğruluk sorunlarını gidermek için eğitim kullanma

Bir temel modelle ilgili tanıma sorunlarıyla karşılaşırsanız, özel bir modeli eğitmek ve doğruluğu artırmaya yardımcı olmak için insan etiketli döküm dosyalarını ve ilgili verileri kullanabilirsiniz. Sorunlarınızı gidermek için hangi veri kümesinin kullanılacağını öğrenmek için bu tabloyu kullanın:

| Kullanım örneği | Veri türü |
| -------- | --------- |
| Tıbbi Terminoloji veya It jarggibi sektöre özgü sözlük ve dilbilgisinde tanıma doğruluğunu geliştirme | İlgili metin (cümleler/utterer) |
| Ürün adları veya kısaltmalar gibi standart olmayan telaffuz içeren bir sözcüğün veya bir terimin fonetik ve görüntülenen formunu tanımlayın | İlgili metin (telaffuz) |
| Konuşma stillerinde, aksanların veya belirli arka plan noseslerdeki tanıma doğruluğunu geliştirme | Ses + insan etiketli yazılı betikler |

## <a name="train-and-evaluate-a-model"></a>Modeli eğitme ve değerlendirme

Bir modeli eğiten ilk adım eğitim verilerini karşıya yüklemedir. Bkz. insanlar etiketli onayları ve ilgili metinleri (utde ve söylenişleri) hazırlamak için [verilerinizi hazırlama ve test](./how-to-custom-speech-test-and-train.md) etme adımları. Eğitim verilerini karşıya yükledikten sonra modelinize eğitime başlamak için aşağıdaki yönergeleri izleyin:

1. [Özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech)oturum açın. Bir modeli ses + insan etiketli döküm veri kümeleri ile eğmayı planlıyorsanız, eğitim için [adanmış donanıma sahip bir bölgede](custom-speech-overview.md#set-up-your-azure-account) konuşma aboneliği seçin.
2. **Konuşmayı metne**  >  **özel konuşma tanıma**  >  **[proje adı]**  >  **eğitimi**' ne gidin.
3. **Modeli eğitme**' yi seçin.
4. Eğitimle bir **ad** ve **Açıklama** sağlayın.
5. **Senaryo ve temel model** listesinde, etki alanına en uygun senaryoyu seçin. Hangi senaryonun seçeceğiniz konusunda emin değilseniz, **genel**' i seçin. Temel model, eğitimin başlangıç noktasıdır. En son model genellikle en iyi seçenektir.
6. **Eğitim verilerini seçin** sayfasında, eğitim için kullanmak istediğiniz bir veya daha fazla ilgili metin veri kümesi veya ses + insan etiketli döküm veri kümeleri seçin. Yeni bir modeli eğitedığınızda ilgili metinle başlayın; Sesli eğitim ve insan etiketli döküm çok daha uzun sürebilir ( [birkaç güne](how-to-custom-speech-evaluate-data.md#improve-model-recognition)kadar).
7. Eğitim tamamlandıktan sonra, yeni eğitilen modelde doğruluk testi yapabilirsiniz. Bu adım isteğe bağlıdır.
8. Özel modelinizi derlemek için **Oluştur** ' u seçin.

**Eğitim** tablosu yeni modele karşılık gelen yeni bir giriş görüntüler. Tabloda Ayrıca durum görüntülenir: **işlem**, **başarılı** veya **başarısız**.

Özel Konuşma Tanıma modeli doğruluğunu değerlendirmek ve iyileştirmek için bkz. [nasıl yapılır](how-to-custom-speech-evaluate-data.md) . Doğruluk sınamasını tercih ederseniz, modelin performansını gerçekçi bir şekilde anlamak için modelinizle birlikte kullandığınız bir akustik veri kümesini seçmeniz önemlidir.

> [!NOTE]
> Hem temel modeller hem de özel modeller yalnızca belirli bir tarihe kadar kullanılabilir (bkz. [model yaşam döngüsü](custom-speech-overview.md#model-lifecycle)). Konuşma Studio, bu tarihi her model ve uç nokta için **süre sonu** sütununda gösterir. Bu tarih isteği bir uç noktaya veya toplu iş dökümden sonra başarısız olabilir veya temel modele geri dönemez.
>
> Doğruluk geliştirmelerinden faydalanmak ve modelinizin süresinin dolmasını önlemek için en son temel modeli kullanarak modelinize yeniden eğitme.

## <a name="deploy-a-custom-model"></a>Özel bir modeli dağıtma

Verileri yükleyip İnceleme, doğruluğu değerlendirin ve özel bir model eğdikten sonra, uygulamalarınız, araçları ve ürünleriniz ile kullanmak üzere özel bir uç nokta dağıtabilirsiniz. 

Özel bir uç nokta oluşturmak için [özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech)oturum açın. Sayfanın üst kısmındaki **özel konuşma tanıma** menüsünde **dağıtım** ' ı seçin. İlk kez çalıştırıldıysanız tabloda listelenen bitiş noktaları olmadığını fark edeceksiniz. Uç nokta oluşturduktan sonra, dağıtılan her bitiş noktasını izlemek için bu sayfayı kullanırsınız.

Sonra, **uç nokta Ekle** ' yi seçin ve özel uç noktanız Için bir **ad** ve **Açıklama** girin. Ardından uç noktayla ilişkilendirmek istediğiniz özel modeli seçin.  Bu sayfadan günlüğe kaydetmeyi de etkinleştirebilirsiniz. Günlüğe kaydetme, uç nokta trafiğini izlemenizi sağlar. Günlüğe kaydetme devre dışıysa, trafik depolanmaz.

![Yeni uç nokta sayfasını gösteren ekran görüntüsü.](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Kullanım koşullarını ve fiyatlandırma ayrıntılarını kabul etmek zorunda kalmazsınız.

Sonra **Oluştur**' u seçin. Bu eylem sizi **dağıtım** sayfasına döndürür. Tablo artık özel uç noktanıza karşılık gelen bir giriş içerir. Uç noktanın durumu geçerli durumunu gösterir. Özel modellerinizi kullanarak yeni bir uç noktanın örneklendirilecek 30 dakikaya kadar zaman alabilir. Dağıtımın durumu **tamamlandığında**, uç nokta kullanıma kullanılır.

Uç noktanız dağıtıldıktan sonra, uç nokta adı bir bağlantı olarak görünür. Uç nokta anahtarı, uç nokta URL 'SI ve örnek kod gibi uç noktanıza özgü bilgileri görmek için bağlantıyı seçin. Zaman aşımı tarihini bir yere göz atın ve kesintisiz hizmeti sağlamak için bitiş noktasının modelini bu tarihten önce güncelleştirin.

## <a name="view-logging-data"></a>Günlük verilerini görüntüleme

**Dağıtım** altında uç noktanın sayfasına giderseniz, günlüğe kaydetme verileri dışarı aktarma için kullanılabilir.
> [!NOTE]
>Günlüğe kaydetme verileri, Microsoft 'a ait depolamada 30 gün boyunca kullanılabilir. Daha sonra kaldırılacaktır. Müşterinin sahip olduğu bir depolama hesabı bilişsel hizmetler aboneliğine bağlanmışsa, günlüğe kaydetme verileri otomatik olarak silinmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel modelinizi nasıl kullanacağınızı öğrenin](how-to-specify-source-language.md)

## <a name="additional-resources"></a>Ek kaynaklar

- [Verilerinizi hazırlayın ve test edin](./how-to-custom-speech-test-and-train.md)
- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
