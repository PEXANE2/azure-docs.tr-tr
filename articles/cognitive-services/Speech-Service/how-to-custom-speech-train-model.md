---
title: Özel Konuşma Tanıma konuşma hizmeti için model eğitme
titleSuffix: Azure Cognitive Services
description: Konuşmaya metin modeli eğitimi, Microsoft 'un temel modeli veya özel bir model için tanıma doğruluğunu iyileştirebilir. Model, insan etiketli döküm ve ilgili metin kullanılarak eğitilir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: erhopf
ms.openlocfilehash: bf9209e0c256412ccb06ea62a197046a7b012e00
ms.sourcegitcommit: d7fba095266e2fb5ad8776bffe97921a57832e23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84629016"
---
# <a name="train-a-model-for-custom-speech"></a>Özel Konuşma için modeli eğitme

Konuşmaya metin modeli eğitimi, Microsoft 'un temel modeli için tanıma doğruluğunu iyileştirebilir. Model, insan etiketli döküm ve ilgili metin kullanılarak eğitilir. Bu veri kümeleri, daha önce karşıya yüklenen ses verileriyle birlikte, konuşma-metin modelini iyileştirmek ve eğleştirmek için kullanılır.

## <a name="use-training-to-resolve-accuracy-issues"></a>Doğruluk sorunlarını gidermek için eğitim kullanma

Modelinize yönelik tanınma sorunlarıyla karşılaşdıysanız, insan etiketli döküm ve ek eğitim için ilgili verilerin kullanılması doğruluğu artırmaya yardımcı olabilir. Bu tabloyu kullanarak, sorununuzu gidermek için hangi veri kümesinin kullanılacağını saptayın:

| Kullanım örneği | Veri türü |
| -------- | --------- |
| Sektöre özgü sözlük ve dilbilgisinde tıp terminolojisi veya It jarggibi tanıma doğruluğunu geliştirir. | İlgili metin (cümleler/utterer) |
| Ürün adları veya kısaltmalar gibi standart olmayan telaffuz içeren bir sözcüğün veya bir terimin fonetik ve görüntülenen formunu tanımlayın. | İlgili metin (telaffuz) |
| Konuşma stillerinde, aksanların veya belirli arka plan noseslerdeki tanıma doğruluğunu geliştirir. | Ses + insan etiketli yazılı betikler |

> [!IMPORTANT]
> Bir veri kümesini karşıya yüklemediyseniz lütfen [verilerinizi hazırlayın ve test](how-to-custom-speech-test-data.md)edin. Bu belge, verileri karşıya yüklemeye yönelik yönergeler ve yüksek kaliteli veri kümeleri oluşturma yönergeleri sağlar.

## <a name="train-and-evaluate-a-model"></a>Modeli eğitme ve değerlendirme

Bir modeli eğiten ilk adım eğitim verilerini karşıya yüklemedir. İnsan etiketli onayları ve ilgili metinleri (dıklılık ve söylenişleri) hazırlamak için [verilerinizi hazırlama ve test](how-to-custom-speech-test-data.md) etme adımları için adım adım yönergeler kullanın. Eğitim verilerini karşıya yükledikten sonra modelinize eğitime başlamak için aşağıdaki yönergeleri izleyin:

1. [Özel konuşma tanıma portalında](https://speech.microsoft.com/customspeech)oturum açın.
2. **Konuşmayı metne > Özel Konuşma Tanıma > [proje adı] eğitim >**.
3. **Modeli eğitme**' ye tıklayın.
4. Ardından, eğitime bir **ad** ve **Açıklama**sağlayın.
5. **Senaryo ve taban çizgisi modeli** açılan menüsünde, etki alanına en uygun senaryoyu seçin. Hangi senaryonun seçeceğinizden emin değilseniz, **genel**' i seçin. Temel model, eğitimin başlangıç noktasıdır. En son model genellikle en iyi seçenektir.
6. **Eğitim verilerini seçin** sayfasında, eğitim için kullanmak istediğiniz bir veya daha fazla ses + insan etiketli döküm veri kümesi seçin.
7. Eğitim tamamlandıktan sonra, yeni eğitilen modelde doğruluk testi gerçekleştirmeyi tercih edebilirsiniz. Bu adım isteğe bağlıdır.
8. Özel modelinizi derlemek için **Oluştur** ' u seçin.

Eğitim tablosu, bu yeni oluşturulan modele karşılık gelen yeni bir giriş görüntüler. Tabloda Ayrıca durum görüntülenir: Işlem, başarılı, başarısız.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Eğitilen bir modelin doğruluğunu değerlendirin

Bu belgeleri kullanarak verileri inceleyebilir ve model doğruluğunu değerlendirebilirsiniz:

- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)

Doğruluk sınamasını yapmayı seçerseniz, modelin performansını gerçekçi bir şekilde anlamak için modelinizden farklı bir akustik veri kümesi seçmeniz önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Modelinizi dağıtın](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Ek kaynaklar

- [Verilerinizi hazırlayın ve test edin](how-to-custom-speech-test-data.md)
- [Verilerinizi inceleyin](how-to-custom-speech-inspect-data.md)
- [Verilerinizi değerlendirin](how-to-custom-speech-evaluate-data.md)
- [Modelinizi eğitme](how-to-custom-speech-train-model.md)
