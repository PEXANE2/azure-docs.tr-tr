---
title: Özel uç nokta oluşturma | Microsoft Docs
description: Bu makalede, Internet Çözümleyicisi kaynağınız ile ölçmek için özel bir uç noktanın nasıl yapılandırılacağını öğrenin.
services: internet-analyzer
author: mattcalder
ms.service: internet-analyzer
ms.topic: quickstart
ms.date: 10/16/2019
ms.author: mebeatty
ms.openlocfilehash: 6a781e767393f35a1ec22f195ba787e3018d8d1e
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76713090"
---
# <a name="measure-custom-endpoints-to-evaluate-in-your-internet-analyzer-tests"></a>Internet çözümleyici testlerinde değerlendirmek için özel uç noktaları ölçme 

Bu makalede, Internet çözümleyici testleriniz kapsamında ölçü için özel bir uç noktanın nasıl ayarlanacağı gösterilmektedir. Özel uç noktalar şirket içi iş yüklerini, diğer bulut sağlayıcılarında çalışan iş yüklerini ve özel Azure yapılandırmalarına karşı değerlendirmenize yardımcı olur.  Bir uç noktanın bir Azure kaynağı olması halinde, bir testte iki özel bitiş noktasını karşılaştırmak mümkündür. Internet çözümleyici hakkında daha fazla bilgi için bkz. [genel bakış](internet-analyzer-overview.md). 

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Bir Internet çözümleyici kaynağı ayarladığınızdan emin olun ve "özel uç nokta" seçeneğini belirleyin. Internet çözümleyici, Özel uç noktanıza Internet erişimi olduğunu varsayar. Daha fazla bilgi için bkz. [Internet çözümleyici kaynağı oluşturma](internet-analyzer-create-test-portal.md).


## <a name="create-custom-endpoint"></a>Özel uç nokta oluştur

1. Saydam bir tek piksellik test görüntüsünü [buraya](https://fpc.msedge.net/apc/trans.gif)indirin. Bu tek piksellik görüntü, istemci JavaScript 'in ölçü performansına göre getirileceği varlıktır.
2. Özel Web uygulamanızda, genel olarak erişilebilen bir yolda test görüntüsünü dağıtın. Yol, HTTPS üzerinden çalışmalıdır. 
3. Tam özel uç nokta URL 'sini (örn. https://contoso.com/test/trans.gif), test oluşturma sırasında özel uç nokta alanına kopyalayın.

## <a name="next-steps"></a>Sonraki adımlar

[Internet ÇÖZÜMLEYICISI SSS](internet-analyzer-faq.md) makalesini okuyun

