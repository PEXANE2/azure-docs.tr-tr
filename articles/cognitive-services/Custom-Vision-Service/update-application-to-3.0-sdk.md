---
title: Projenizi 3.0 API'ye güncelleştirme
titleSuffix: Azure Cognitive Services
description: Özel Görme projelerini API'nin önceki sürümünden 3.0 API'ye nasıl güncelleştireceğimiz öğrenin.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: areddish
ms.openlocfilehash: c134f30b124113a23df0e73cd1bbc8209e335183
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73647493"
---
# <a name="update-to-the-30-api"></a>3.0 API güncellemesi

Custom Vision artık Genel Kullanılabilirlik'e ulaştı ve bir API güncelleştirmesi yapıldı.
Bu güncelleştirme birkaç yeni özellik ve daha da önemlisi birkaç kesme değişikliği içerir:

* Tahmin API'si artık proje türüne göre ikiye ayrılmıştır.
* Vision AI Geliştirici Kiti (VAIDK) dışa aktarma seçeneği, belirli bir şekilde proje oluşturmayı gerektirir.
* Varsayılan yinelemeler, bir yayımlama / yayımlama dan başka bir deyişle kaldırılmıştır.

Bu kılavuz, yeni API sürümüyle çalışmak için projelerinizi nasıl güncelleştireceğinizgösterecektir. Değişikliklerin tam listesi için [Yayın notlarına](release-notes.md) bakın.

## <a name="use-the-updated-prediction-api"></a>Güncelleştirilmiş Tahmin API'sini kullanma

2.x API'ler hem görüntü sınıflandırıcıları hem de nesne dedektörü projeleri için aynı tahmin çağrısını kullandı. Her iki proje türü de PredictImage ve **PredictImageUrl** aramaları için kabul edilebilirdi. **PredictImageUrl** 3.0 ile başlayarak, proje türünü çağrıyla eşleştirmeniz için bu API'yi böldük:

* Görüntü sınıflandırma projeleri için öngörüler almak için Image ve **[ClassifyImageUrl'yi](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c14)** kullanın. **[ClassifyImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15)**
* Nesne algılama projeleri için öngörüler almak için **[DetectImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c19)** ve **[DetectImageUrl'yi](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c18)** kullanın.

## <a name="use-the-new-iteration-publishing-workflow"></a>Yeni yineleme yayımlama iş akışını kullanma

2.x API'ler, tahmin için kullanılacak yinelemeyi seçmek için varsayılan yinelemeyi veya belirli bir yineleme kimliğini kullanır. 3.0'dan başlayarak, ilk olarak eğitim API'sinden belirli bir ad altında bir yineleme yayınladığınız bir yayımlama akışını benimsedik. Daha sonra hangi yinelemenin kullanılacağını belirtmek için adı tahmin yöntemlerine geçirirsiniz.

> [!IMPORTANT]
> 3.0 API'ler varsayılan yineleme özelliğini kullanmaz. Eski API'leri amortismana katana kadar, bir yinelemeyi varsayılan olarak geçiş yapmak için 2.x API'leri kullanmaya devam edebilirsiniz. Bu API'ler bir süre boyunca korunur ve bir yinelemeyi varsayılan olarak işaretlemek için **[Güncelleştirme Yöntemini](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b818)** çağırabilirsiniz.

### <a name="publish-an-iteration"></a>Yineleme yayımlama

Bir yineleme eğitildikten sonra, **[PublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c82db28bf6a2b11a8247bbc)** yöntemini kullanarak tahmin için kullanılabilir hale getirebilirsiniz. Bir yineleme yayınlamak için, CustomVision web sitesinin ayarlar sayfasında bulunan tahmin kaynağı kimliğine ihtiyacınız vardır.

![Tahmin kaynağı kimliği özetlenen Özel Vizyon web sitesi ayarları sayfası.](./media/update-application-to-3.0-sdk/prediction-id.png)

> [!TIP]
> Ayrıca bu bilgileri Azure [Portalı'ndan](https://portal.azure.com) Özel Vizyon Tahmini kaynağına giderek ve **Özellikler'i**seçerek de alabilirsiniz.

Yinelemeniz yayımlandıktan sonra, uygulamalar tahmin API çağrısında adı belirterek bunu tahmin amacıyla kullanabilir. Bir yinelemeyi tahmin çağrıları için kullanılamaz hale getirmek için **[YayımlamaYı Yıkınma](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b81a)** API'sini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitim API referans belgeleri (REST)](https://go.microsoft.com/fwlink/?linkid=865446)
* [Tahmin API başvuru belgeleri (REST)](https://go.microsoft.com/fwlink/?linkid=865445)