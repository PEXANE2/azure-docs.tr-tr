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
ms.openlocfilehash: 8e0e549f88caf4a541642bab77faf54b5e536b29
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "71175002"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Market koşullarına sahip bir görüntü dağıtma

Azure Marketi 'ndeki bazı sanal makine görüntülerinin, programlama yoluyla dağıtmadan önce kabul etmeniz gereken ek lisans ve satın alma koşulları vardır.  

Bu tür bir görüntüden bir VM dağıtmak için, hem görüntünün koşullarını kabul etmeniz hem de programlı dağıtımı etkinleştirmeniz gerekir. Bunu her abonelik için yalnızca bir kez yapmanız gerekir. Bundan sonra, görüntüden program aracılığıyla bir VM dağıttığınızda, *satın alma planı* parametreleri de belirtmeniz gerekir.

Aşağıdaki bölümlerde aşağıdakilerin nasıl yapılacağı gösterilmektedir:

* Market görüntüsünün ek lisans koşullarına sahip olup olmadığını öğrenin 
* Koşulları programlama yoluyla kabul etme
* Bir VM 'yi program aracılığıyla dağıtırken satın alma planı parametreleri sağlama

