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
ms.openlocfilehash: bdb2b31ee3abb30e6e7339f641b778ff5dc522b0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81403803"
---
# <a name="what-is-custom-vision"></a>Özel Görüntü İşleme nedir?

[!INCLUDE [TLS 1.2 enforcement](../../../includes/cognitive-services-tls-announcement.md)]

Azure Özel Görüntü İşleme, kendi resim sınıflandırıcılarınızı oluşturmanıza, dağıtmanıza ve geliştirmenize olanak sağlayan bir bilişsel hizmettir. Görüntü sınıflandırıcı, görsel özelliklerine göre resimlere etiketler _(sınıfları_temsil eden) uygulayan bir AI hizmetidir. Computer [Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) hizmetinin aksine, Custom Vision uygulanacak etiketleri belirtmenize olanak tanır.

## <a name="what-it-does"></a>Ne yapar?

Custom Vision hizmeti, resimlere etiketler uygulamak için bir makine öğrenme algoritması kullanır. Geliştirici olarak siz, söz konusu özellik ve özellikleri olmayan görüntü gruplarını göndermeniz gerekir. Teslim sırasında görüntüleri kendiniz etiketlersiniz. Sonra algoritma bu verilere trenler ve aynı görüntüler üzerinde kendini test ederek kendi doğruluğunu hesaplar. Algoritma eğitildikten sonra, yeni görüntüleri uygulamanızın gereksinimlerine göre sınıflandırmak için test edebilir, yeniden eğitebilir ve sonunda kullanabilirsiniz. İsterseniz modeli çevrimdışı kullanmak üzere dışarı aktarabilirsiniz.

### <a name="classification-and-object-detection"></a>Sınıflandırma ve nesne algılama

Özel Görüntü İşleme hizmeti iki bölüme ayrılabilir. **Görüntü sınıflandırması** bir görüntüye bir veya daha fazla etiket uygular. **Nesne algılama** benzer, ancak aynı zamanda uygulanan etiket(ler) bulunabilir görüntüdeki koordinatları döndürür.

### <a name="optimization"></a>İyileştirme

Özel Görme hizmeti, görüntüler arasındaki büyük farklılıkları hızlı bir şekilde tanıyabilmek için optimize edilsin, böylece modelinizi az miktarda veriyle prototiplemeye başlayabilirsiniz. Etiket başına 50 görüntü genellikle iyi bir başlangıç. Ancak, hizmet görüntülerdeki ince farklılıkları algılamak için uygun değildir (örneğin, kalite güvence senaryolarında küçük çatlaklar veya çentikler algılama).

Ayrıca, belirli konu materyalleri,&mdash;yer işaretleri veya perakende öğeler gibi görüntüler için optimize edilmiş Özel Vizyon algoritmasının çeşitli çeşitleri arasından seçim yapabilirsiniz. Daha fazla bilgi için [bir sınıflandırıcı oluştur](getting-started-build-a-classifier.md) kılavuzuna bakın.

## <a name="what-it-includes"></a>Neleri içerir

Özel Görüntü İşleme Hizmeti, yerel SDK'lara ek olarak [Özel Görüntü İşleme giriş sayfası](https://customvision.ai/) üzerinden web tabanlı arabirim aracılığıyla da sunulmaktadır. Bir modeli arabirim üzerinden oluşturabilir, sınayabilir ve eğitebilir veya her ikisini de birlikte kullanabilirsiniz.

![Chrome tarayıcı penceresinde Özel Görüntü İşleme giriş sayfası](media/browser-home.png)

## <a name="data-privacy-and-security"></a>Veri gizliliği ve güvenliği

Tüm Bilişsel Hizmetlerde olduğu gibi, Custom Vision hizmetini kullanan geliştiriciler microsoft'un müşteri verilerine ilişkin ilkelerinden haberdar olmalıdır. Daha fazla bilgi edinmek için Microsoft Güven Merkezi'ndeki [Bilişsel Hizmetler sayfasına](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Web'de Özel Görme'yi kullanmaya başlamak için [bir sınıflandırıcı oluştur](getting-started-build-a-classifier.md) kılavuzunu izleyin veya kodda temel bir senaryo uygulamak için bir [Resim sınıflandırma öğreticisini](csharp-tutorial.md) tamamlayın.
