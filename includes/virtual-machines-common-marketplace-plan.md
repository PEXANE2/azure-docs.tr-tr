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
ms.openlocfilehash: 8e0e549f88caf4a541642bab77faf54b5e536b29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71175002"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Pazar yeri terimleriyle görüntü dağıtma

Azure Marketi'ndeki bazı VM görüntülerinin, bunları programlı bir şekilde dağıtmadan önce kabul etmesi gereken ek lisans ve satın alma koşulları vardır.  

Böyle bir görüntüden bir VM dağıtmak için hem görüntünün koşullarını kabul etmeniz hem de programatik dağıtımı etkinleştirmeniz gerekir. Bunu abonelik başına yalnızca bir kez yapmanız gerekir. Daha sonra, görüntüden programlı bir VM dağıttığınızda *satınalma planı* parametrelerini de belirtmeniz gerekir.

Aşağıdaki bölümlernasıl yapılacağını gösterir:

* Market görüntüsünün ek lisans koşullarına sahip olup olmadığını öğrenin 
* Terimleri programlı olarak kabul edin
* Bir VM'yi programlı olarak dağıttığınızda satınalma planı parametrelerini sağlama

