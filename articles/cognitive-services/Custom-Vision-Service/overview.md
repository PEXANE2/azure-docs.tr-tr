---
title: Özel Görüntü İşleme nedir?
titleSuffix: Azure Cognitive Services
description: Azure Özel Görüntü İşleme hizmetini kullanarak Azure bulutundaki özel AI görüntüsü sınıflandırıcıları ve nesne algılayıcıları oluşturma hakkında bilgi edinin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 09/14/2020
ms.author: pafarley
ms.custom: cog-serv-seo-aug-2020
keywords: görüntü tanıma, görüntü tanımlayıcı, görüntü tanıma uygulaması, Özel Vizyon
ms.openlocfilehash: 47227a60b1ed45499afdb42fbc062abc5ae51ff9
ms.sourcegitcommit: 80b9c8ef63cc75b226db5513ad81368b8ab28a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90605597"
---
# <a name="what-is-custom-vision"></a>Özel Görüntü İşleme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Özel Görüntü İşleme, kendi görüntü tanımlayıcılarınızı oluşturmanıza, dağıtmanıza ve iyileştirmenize olanak tanıyan bir görüntü tanıma hizmetidir. Görüntü tanımlayıcısı, görsel özelliklerine göre resimlere (sınıfları veya nesneleri temsil eden) etiketleri uygular. [Görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) hizmetinden farklı olarak, özel görüntü işleme, etiketleri belirleyip özel modeller eğitmenize olanak tanır.

## <a name="what-it-does"></a>Ne yapar?

Özel Görüntü İşleme Hizmeti görüntüleri çözümlemek için bir makine öğrenimi algoritması kullanır. Geliştirici, söz konusu özelliklere sahip ve söz konusu özellikleri olmayan görüntü gruplarını gönderir. Görüntüleri gönderme sırasında kendiniz etiketlendirin. Ardından, algoritma bu verilere yönelik bir test yapar ve kendisini aynı görüntülerle test ederek kendi doğruluğunu hesaplar. Algoritma eğitilirken, bunları test edebilir, yeniden eğitebilir ve sonunda yeni görüntüleri sınıflandırmak için görüntü tanıma uygulamanızda kullanabilirsiniz. İsterseniz modeli çevrimdışı kullanmak üzere dışarı aktarabilirsiniz.

### <a name="classification-and-object-detection"></a>Sınıflandırma ve nesne algılama

Özel Görüntü İşleme hizmeti iki bölüme ayrılabilir. **Görüntü sınıflandırması** bir görüntüye bir veya daha fazla etiket uygular. **Nesne algılama** benzerdir, ancak uygulanan etiketlerin bulunduğu görüntüdeki koordinatları de döndürür.

### <a name="optimization"></a>İyileştirme

Özel Görüntü İşleme Hizmeti, görüntüler arasındaki önemli farklılıkları hızlı bir şekilde tanımak için en iyi duruma getirilmiştir, bu sayede modelinize kısa bir veri miktarıyla prototip başlatabilirsiniz. Her etiket için 50 görüntü genellikle iyi bir başlangıç. Ancak, görüntüde hafif farklılıkları tespit etmek için en uygun değildir (örneğin, kalite güvencesi senaryolarındaki küçük bir yandan veya en yüzlerini algılama).

Ayrıca, belirli konu malzemelere örnek olarak en iyi duruma getirilmiş Özel Görüntü İşleme algoritmasından farklı, &mdash; Örneğin, yer işaretleri veya perakende öğeleri arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) veya [nesne algılayıcısı oluşturma](get-started-build-detector.md) Kılavuzu.

## <a name="what-it-includes"></a>Neleri içerir

Özel Görüntü İşleme Hizmeti, yerel SDK 'ların yanı sıra [özel görüntü işleme web sitesinde](https://customvision.ai/)Web tabanlı bir arabirim aracılığıyla kullanılabilir. Bir modeli bir arabirim aracılığıyla oluşturabilir, test edebilir ve eğitebilirsiniz ya da ikisini birlikte kullanabilirsiniz.

![Chrome tarayıcı penceresinde Özel Görüntü İşleme Web sitesi](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, Özel Görüntü İşleme hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine göre farkında olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Web portalında Özel Görüntü İşleme kullanmaya başlamak için [sınıflandırıcının derleme](getting-started-build-a-classifier.md) kılavuzunu izleyin veya temel senaryoları kodda uygulamak için bir [istemci kitaplığı hızlı](quickstarts/image-classification.md) başlangıcını tamamlayın.
