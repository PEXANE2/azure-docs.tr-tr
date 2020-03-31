---
title: Özel Bitiş Noktası Oluşturma | Microsoft Dokümanlar
description: Bu makalede, Internet Analyzer kaynağınızla ölçmek için özel bir bitiş noktasını nasıl yapılandırıştıracaksınız öğrenin.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: quickstart
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 6a781e767393f35a1ec22f195ba787e3018d8d1e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76713090"
---
# <a name="measure-custom-endpoints-to-evaluate-in-your-internet-analyzer-tests"></a>Internet Analyzer testlerinizde değerlendirmek için özel uç noktaları ölçün 

Bu makalede, Internet Analyzer testlerinizin bir parçası olarak ölçmek için özel bir bitiş noktası nasıl ayarlayınız gösteriş. Özel uç noktalar şirket içi iş yüklerinin, diğer bulut sağlayıcılarında çalışan iş yüklerinin ve özel Azure yapılandırmalarının değerlendirilmesine yardımcı olur.  Bir uç nokta Bir Azure kaynağıysa, bir testte iki özel uç noktayı karşılaştırmak mümkündür. Internet Analyzer hakkında daha fazla bilgi için [genel bakışa](internet-analyzer-overview.md)bakın. 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Bir Internet Analyzer kaynağı ayarladığınızdan ve "Özel Bitiş Noktası" seçeneğini seçtiğinizden emin olun. Internet Analyzer, özel bitiş noktanızın Internet'e erişilebilir olduğunu varsayar. Daha fazla bilgi için [bkz.](internet-analyzer-create-test-portal.md)


## <a name="create-custom-endpoint"></a>Özel Bitiş Noktası Oluşturma

1. [Burada](https://fpc.msedge.net/apc/trans.gif)saydam bir tek piksel test görüntüsü indirin. Bu tek piksellik görüntü, istemci JavaScript'in performansı ölçmek için getireceği varlıktır.
2. Özel web uygulamanızda, test görüntüsünü herkese açık bir yolda dağıtın. Yol HTTPS üzerinde çalışmalıdır. 
3. Tam özel uç nokta URL'sini https://contoso.com/test/trans.gif) (örn. test oluşturma sırasında özel bitiş noktası alanına kopyalayın).

## <a name="next-steps"></a>Sonraki adımlar

Internet [Analyzer SSS'yi](internet-analyzer-faq.md) okuyun

