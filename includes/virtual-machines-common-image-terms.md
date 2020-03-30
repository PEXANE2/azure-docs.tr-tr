---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 98d765e2f6909f00f8dfe76d06aef017aad67adf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71175003"
---
## <a name="terminology"></a>Terminoloji

Azure'daki Bir Market görüntüsü aşağıdaki özelliklere sahiptir:

* **Publisher**: Görüntüyü oluşturan kuruluş. Örnekler: Canonical, MicrosoftWindowsServer
* **Teklif**: Bir yayımcı tarafından oluşturulan ilgili resimler grubunun adı. Örnekler: UbuntuServer, WindowsServer
* **SKU**: Bir teklifin bir örneğinin, bir dağıtımın büyük bir sürümü gibi. Örnekler: 18.04-LTS, 2019-Datacenter
* **Sürüm**: Bir görüntüsünün sürüm numarası SKU. 

Bir VM'yi programlı olarak dağıtırken bir Market görüntüsünü tanımlamak için, bu değerleri tek tek parametre olarak ver. Bazı araçlar, bu değerleri birleştiren ve üst üste ayrılmış bir görüntü *URN'u*kabul :) karakter : *Publisher*:*Teklif*:*Sku*:*Sürüm*. BIR URL'de, sürüm numarasını görüntünün en son sürümünü seçen "en son" ile değiştirebilirsiniz. 

Görüntü yayımcısı ek lisans ve satın alma koşulları sağlıyorsa, bu koşulları kabul etmeli ve programatik dağıtımı etkinleştirmelisiniz. Ayrıca, bir VM'yi programlı olarak dağıtırken *satın alma planı* parametrelerini sağlamanız gerekir. Bkz. [Pazar yeri terimleriyle bir görüntü dağıtın.](#deploy-an-image-with-marketplace-terms)
