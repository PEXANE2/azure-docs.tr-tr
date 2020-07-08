---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "71175003"
---
## <a name="terminology"></a>Terminoloji

Azure 'da Market görüntüsü aşağıdaki özniteliklere sahiptir:

* **Yayımcı**: görüntüyü oluşturan kuruluş. Örnekler: Canonical, MicrosoftWindowsServer
* **Teklif**: Yayımcı tarafından oluşturulan ilgili görüntü grubunun adı. Örnekler: UbuntuServer, WindowsServer
* **SKU**: bir dağıtımın ana sürümü gibi bir teklifin örneği. Örnekler: 18,04-LTS, 2019-Datacenter
* **Sürüm**: BIR görüntü SKU 'sunun sürüm numarası. 

Bir VM 'yi program aracılığıyla dağıtırken Market görüntüsünü belirlemek için, bu değerleri parametreler olarak tek tek sağlayın. Bazı araçlar, iki nokta (:) ile ayrılmış şekilde bu değerleri birleştiren bir resmi *urn*kabul eder. karakter: *Yayımcı*:*teklif*:*SKU*:*Sürüm*. Bir URN 'de, sürüm numarasını görüntünün en son sürümünü seçen "en son" ile değiştirebilirsiniz. 

Görüntü yayımcısı ek lisans ve satın alma koşulları sağlıyorsa, bu koşulları kabul etmeniz ve programlı dağıtımı etkinleştirmeniz gerekir. Ayrıca, bir VM 'yi programlı bir şekilde dağıttığınızda *satın alma planı* parametreleri sağlamanız gerekir. Bkz. [Market koşullarına sahip bir görüntü dağıtma](#deploy-an-image-with-marketplace-terms).
