---
title: Özel Konuşma Tanıma modelini eğitme ve dağıtma-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Bu makalede Özel Konuşma Tanıma modellerini eğitme ve dağıtmayı öğreneceksiniz. Konuşmaya metin modeli eğitimi, Microsoft 'un temel modeli veya özel bir model için tanıma doğruluğunu iyileştirebilir. Model, insan etiketli döküm ve ilgili metin kullanılarak eğitilir.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/11/2020
ms.author: trbye
ms.openlocfilehash: 34c0703ee7c335ca904a21bcce6ed44abc6dc13f
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94555796"
---
# <a name="train-and-deploy-a-custom-speech-model"></a>Özel Konuşma Tanıma modeli eğitme ve dağıtma

Bu makalede Özel Konuşma Tanıma modellerini eğitme ve dağıtmayı öğreneceksiniz. Konuşmaya metin modeli eğitimi, Microsoft 'un temel modeli için tanıma doğruluğunu iyileştirebilir. Model, insan etiketli döküm ve ilgili metin kullanılarak eğitilir. Bu veri kümeleri, daha önce karşıya yüklenen ses verileriyle birlikte, konuşma-metin modelini iyileştirmek ve eğleştirmek için kullanılır.

## <a name="use-training-to-resolve-accuracy-issues"></a>Doğruluk sorunlarını gidermek için eğitim kullanma

Bir temel modelde tanınma sorunlarıyla karşılaşırsanız, özel bir modeli eğtirecek şekilde, insan etiketli döküm ve ilgili verilerin kullanılması doğruluğu artırmaya yardımcı olabilir. Bu tabloyu kullanarak, sorununuzu gidermek için hangi veri kümesinin kullanılacağını saptayın:

| Kullanım örneği | Veri türü |
| -------- | --------- |
| Sektöre özgü sözlük ve dilbilgisinde tıp terminolojisi veya It jarggibi tanıma doğruluğunu geliştirir. | İlgili metin (cümleler/utterer) |
| Ürün adları veya kısaltmalar gibi standart olmayan telaffuz içeren bir sözcüğün veya bir terimin fonetik ve görüntülenen formunu tanımlayın. | İlgili metin (telaffuz) |
| Konuşma stillerinde, aksanların veya belirli arka plan noseslerdeki tanıma doğruluğunu geliştirir. | Ses + insan etiketli yazılı betikler |

## <a name="train-and-evaluate-a-model"></a>Modeli eğitme ve değerlendirme

Bir modeli eğiten ilk adım eğitim verilerini karşıya yüklemedir. İnsan etiketli onayları ve ilgili metinleri (dıklılık ve söylenişleri) hazırlamak için [verilerinizi hazırlama ve test](how-to-custom-speech-test-data.md) etme adımları için adım adım yönergeler kullanın. Eğitim verilerini karşıya yükledikten sonra modelinize eğitime başlamak için aşağıdaki yönergeleri izleyin:

1. [Özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech)oturum açın.
2. **Konuşmayı metne > Özel Konuşma Tanıma > [proje adı] eğitim >**.
3. **Modeli eğitme** ' ye tıklayın.
4. Ardından, eğitime bir **ad** ve **Açıklama** sağlayın.
5. **Senaryo ve taban çizgisi modeli** açılan menüsünde, etki alanına en uygun senaryoyu seçin. Hangi senaryonun seçeceğinizden emin değilseniz, **genel** ' i seçin. Temel model, eğitimin başlangıç noktasıdır. En son model genellikle en iyi seçenektir.
6. **Eğitim verilerini seçin** sayfasında, eğitim için kullanmak istediğiniz bir veya daha fazla ses + insan etiketli döküm veri kümesi seçin.
7. Eğitim tamamlandıktan sonra, yeni eğitilen modelde doğruluk testi gerçekleştirmeyi tercih edebilirsiniz. Bu adım isteğe bağlıdır.
8. Özel modelinizi derlemek için **Oluştur** ' u seçin.

Eğitim tablosu, bu yeni oluşturulan modele karşılık gelen yeni bir giriş görüntüler. Tabloda Ayrıca durum görüntülenir: Işlem, başarılı, başarısız.

Özel Konuşma Tanıma modeli doğruluğunu değerlendirmek ve iyileştirmek için bkz. [nasıl yapılır](how-to-custom-speech-evaluate-data.md) . Doğruluk sınamasını yapmayı seçerseniz, modelin performansını gerçekçi bir şekilde anlamak için modelinizden farklı bir akustik veri kümesi seçmeniz önemlidir.

## <a name="deploy-a-custom-model"></a>Özel bir modeli dağıtma

Verileri karşıya yükledikten ve inceledikten, doğruluğu değerlendirdikten ve özel bir model eğitilene kadar, uygulamalarınız, araçlarınızla ve ürünlerinizle kullanmak üzere özel bir uç nokta dağıtabilirsiniz. 

Yeni bir özel uç nokta oluşturmak için [özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech) oturum açın ve sayfanın üst kısmındaki özel konuşma tanıma menüsünden **dağıtım** ' yi seçin. İlk kez çalıştırıldıysanız tabloda listelenen bitiş noktaları olmadığını fark edeceksiniz. Bir uç nokta oluşturduktan sonra, dağıtılan her bitiş noktasını izlemek için bu sayfayı kullanırsınız.

Sonra, **uç nokta Ekle** ' yi seçin ve özel uç noktanız Için bir **ad** ve **Açıklama** girin. Ardından, bu uç nokta ile ilişkilendirmek istediğiniz özel modeli seçin. Bu sayfadan günlüğe kaydetmeyi de etkinleştirebilirsiniz. Günlüğe kaydetme, uç nokta trafiğini izlemenizi sağlar. Devre dışı bırakılırsa trafik depolanmaz.

![Model dağıtma](./media/custom-speech/custom-speech-deploy-model.png)

> [!NOTE]
> Kullanım koşullarını ve fiyatlandırma ayrıntılarını kabul etmek zorunda kalmazsınız.

Sonra **Oluştur** ' u seçin. Bu eylem sizi **dağıtım** sayfasına döndürür. Tablo artık özel uç noktanıza karşılık gelen bir giriş içerir. Uç noktanın durumu geçerli durumunu gösterir. Özel modellerinizi kullanarak yeni bir uç noktanın örneklendirilecek 30 dakikaya kadar zaman alabilir. Dağıtımın durumu **tamamlandığında** , uç nokta kullanıma kullanılır.

Uç noktanız dağıtıldıktan sonra, uç nokta adı bir bağlantı olarak görünür. Uç nokta anahtarı, uç nokta URL 'SI ve örnek kod gibi uç noktanıza özgü bilgileri göstermek için bağlantıya tıklayın.

## <a name="view-logging-data"></a>Günlük verilerini görüntüleme

Günlüğe kaydetme verileri, **uç nokta > ayrıntıları** altında indirilebilir.
> [!NOTE]
>Günlüğe kaydetme verileri Microsoft 'un sahip olduğu depolamada 30 gün boyunca kullanılabilir ve daha sonra kaldırılacaktır. Müşterinin sahip olduğu bir depolama hesabının bilişsel hizmetler aboneliğine bağlı olması durumunda günlüğe kaydetme verileri otomatik olarak silinmez.

## <a name="next-steps"></a>Sonraki adımlar

* [Özel modelinizi nasıl kullanacağınızı](how-to-specify-source-language.md)öğrenin.

## <a name="additional-resources"></a>Ek kaynaklar

- [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
