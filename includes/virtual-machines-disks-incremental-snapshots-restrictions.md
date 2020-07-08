---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/05/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 39a013f5f4b587137366147ade77f0be1b353c4c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82204470"
---
- Artımlı anlık görüntüler şu anda abonelikler arasında taşınamaz.
- Şu anda, belirli bir anda belirli bir anlık görüntü ailesinin en fazla beş anlık görüntüsüne sahip SAS URI 'Leri oluşturabilirsiniz.
- Bu diskin aboneliği dışındaki belirli bir disk için artımlı bir anlık görüntü oluşturamazsınız.
- Her beş dakikada bir disk başına en fazla yedi Artımlı anlık görüntü oluşturulabilir.
- Tek bir disk için toplam 200 Artımlı anlık görüntü oluşturulabilir.
- 4 TB 'lik sınırın üzerinde üst diskin boyutundaki değişiklikten önce ve sonra alınan anlık görüntüler arasındaki değişiklikleri alamaz. Yeniden boyutlandırma işleminden sonra oluşturulan anlık görüntünün tam kopyasını yeniden indirmeniz gerekir. Daha sonra, 4 TB 'lik sınırın üzerinde yeniden boyutlandırmayla oluşturulan anlık görüntüler arasındaki değişiklikleri alabilirsiniz. 
