---
title: Özel Görüntü İşleme nedir?
titleSuffix: Azure Cognitive Services
description: Özel Görüntü İşleme Hizmeti'ni kullanarak Azure bulutunda özel görüntü sınıflandırıcıları oluşturmayı öğrenin.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: overview
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: 79ecb801e1b4d0fa96ca7ae06223fc231cbf12e6
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82129883"
---
# <a name="what-is-custom-vision"></a>Özel Görüntü İşleme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Özel Görüntü İşleme, kendi resim sınıflandırıcılarınızı oluşturmanıza, dağıtmanıza ve geliştirmenize olanak sağlayan bir bilişsel hizmettir. Görüntü Sınıflandırıcısı, görsel özelliklerine göre resimlere (sınıfları temsil eden _sınıflar_) uygulanan bir AI hizmetidir. [Görüntü işleme](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) hizmetinin aksine, özel görüntü işleme uygulanacak etiketleri belirtmenize olanak tanır.

## <a name="what-it-does"></a>Ne yapar?

Özel Görüntü İşleme Hizmeti, resimlere etiket uygulamak için bir makine öğrenimi algoritması kullanır. Geliştirici, söz konusu özelliklere sahip olan ve özellik olmayan görüntü gruplarını göndermesi gerekir. Görüntüleri gönderme sırasında kendiniz etiketlendirin. Daha sonra algoritma bu verilere yönelik olarak bir test yapar ve kendisini aynı görüntülerle test ederek kendi doğruluğunu hesaplar. Algoritma eğitilirken, bunu test edebilir, yeniden eğitebilir ve en sonunda, yeni görüntüleri uygulamanızın gereksinimlerine göre sınıflandırmak için kullanabilirsiniz. İsterseniz modeli çevrimdışı kullanmak üzere dışarı aktarabilirsiniz.

### <a name="classification-and-object-detection"></a>Sınıflandırma ve nesne algılama

Özel Görüntü İşleme hizmeti iki bölüme ayrılabilir. **Görüntü sınıflandırması** bir görüntüye bir veya daha fazla etiket uygular. **Nesne algılama** benzerdir, ancak uygulanan etiketlerin bulunduğu görüntüdeki koordinatları de döndürür.

### <a name="optimization"></a>İyileştirme

Özel Görüntü İşleme Hizmeti, görüntüler arasındaki önemli farklılıkları hızlı bir şekilde tanımak için en iyi duruma getirilmiştir, bu sayede modelinize kısa bir veri miktarıyla prototip başlatabilirsiniz. Her etiket için 50 görüntü genellikle iyi bir başlangıç. Ancak, görüntüde hafif farklılıkları tespit etmek için en uygun değildir (örneğin, kalite güvencesi senaryolarındaki küçük bir yandan veya en yüzlerini algılama).

Ayrıca, belirli konu malzemelere&mdash;örnek olarak en iyi duruma getirilmiş özel görüntü işleme algoritmasından farklı, örneğin, yer işaretleri veya perakende öğeleri arasından seçim yapabilirsiniz. Daha fazla bilgi için bkz. [sınıflandırıcı oluşturma](getting-started-build-a-classifier.md) Kılavuzu.

## <a name="what-it-includes"></a>Neleri içerir

Özel Görüntü İşleme Hizmeti, yerel SDK'lara ek olarak [Özel Görüntü İşleme giriş sayfası](https://customvision.ai/) üzerinden web tabanlı arabirim aracılığıyla da sunulmaktadır. Bir modeli bir arabirim aracılığıyla oluşturabilir, test edebilir ve eğitebilirsiniz ya da ikisini birlikte kullanabilirsiniz.

![Chrome tarayıcı penceresinde Özel Görüntü İşleme giriş sayfası](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm bilişsel hizmetlerde olduğu gibi, Özel Görüntü İşleme hizmetini kullanan geliştiriciler Microsoft 'un müşteri verileri ilkelerine göre farkında olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi ' nde bilişsel [Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Web üzerinde Özel Görüntü İşleme kullanmaya başlamak için [sınıflandırıcının derleme](getting-started-build-a-classifier.md) kılavuzunu izleyin veya kodda temel bir senaryoyu uygulamak Için bir [görüntü sınıflandırması öğreticisini](quickstarts/image-classification.md) tamamlayın.
