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
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "71175002"
---
## <a name="deploy-an-image-with-marketplace-terms"></a>Market koşullarına sahip bir görüntü dağıtma

Azure Marketi 'ndeki bazı sanal makine görüntülerinin, programlama yoluyla dağıtmadan önce kabul etmeniz gereken ek lisans ve satın alma koşulları vardır.  

Bu tür bir görüntüden bir VM dağıtmak için, hem görüntünün koşullarını kabul etmeniz hem de programlı dağıtımı etkinleştirmeniz gerekir. Bunu her abonelik için yalnızca bir kez yapmanız gerekir. Bundan sonra, görüntüden program aracılığıyla bir VM dağıttığınızda, *satın alma planı* parametreleri de belirtmeniz gerekir.

Aşağıdaki bölümlerde aşağıdakilerin nasıl yapılacağı gösterilmektedir:

* Market görüntüsünün ek lisans koşullarına sahip olup olmadığını öğrenin 
* Koşulları programlama yoluyla kabul etme
* Bir VM 'yi program aracılığıyla dağıtırken satın alma planı parametreleri sağlama

