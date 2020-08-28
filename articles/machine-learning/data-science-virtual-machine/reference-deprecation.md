---
title: 'Başvuru: Veri Bilimi Sanal Makinesi Image retirements'
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi etkileyen retirements hakkındaki ayrıntılar
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 07/17/2020
ms.topic: reference
ms.openlocfilehash: d5f541dec14eebc944e4eac11dbe569b38cb277e
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89001629"
---
# <a name="reference-retirements-of-dsvm-images"></a>Başvuru: DSVM görüntülerinin retirements

Aşağıda, görüntülerin kullanımdan kaldırılması (kullanımdan kaldırma) ve Azure Veri Bilimi Sanal Makinesi yaklaşan retirements ile ilgilenme önerileri tartışıyoruz.

## <a name="why-we-retire-dsvm-images"></a>DSVM görüntülerini neden devre dışı bırakıyoruz

Veri Bilimi Sanal Makinesi Şu anda iki sürümü vardır:

* Ubuntu
* Windows Server

Bu sürümler için DSVM görüntüsü, belirli bir işletim sistemi sürümü (örneğin Ubuntu 18,04 LTS) üzerine kurulmuştur. Zaman içinde, işletim sistemi _sürümünün_ bakım penceresi sona ve/veya veri bilimi araçları artık eski işletim sistemi sürümlerini desteklemezler. En son işletim sistemleri ve veri bilimi araçları ile DSVM görüntüsünü güncel tutmak için, daha yeni işletim sistemi sürümlerini temel alan yeni bir DSVM görüntüsü yayınlarız.

## <a name="how-we-retire-dsvm-images"></a>DSVM görüntülerini devre dışı bırakma

Var olan DSVM kullanıcılarına yakında bir DSVM görüntüsü kullanımdan kaldırılması durumunda (e-posta aracılığıyla), bir _kullanımdan kaldırma duyurusu_ veriyoruz. Emeklilik duyurusu, _kullanımdan kaldırma tarihini_ ayrıntılandırır (Bu tarihten sonra, bu tarihten sonra) ve risk azaltma önerilerini (örneğin, daha yenı bır dsvm görüntüsüne yükseltme) ayrıntılandırır.

_Duyuru_ tarihinde Market 'teki görüntüyü gizleyecek ve şunları yapabilirsiniz:

1. Yeni kullanıcıların, kullanımdan kaldırılan bir DSVM görüntüsünü istemeden sağlaması engellenir.
2. Mevcut kullanıcılar, kullanımdan kaldırma tarihine kadar ARM dağıtımlarını kullanabilir.

Gizli görüntü, _kullanımdan kaldırma tarihine_ kadar işletim sistemi düzeltme ekleri alır, ancak veri bilimi araçları ve çerçeveleri için __güncelleştirmeleri almaz.__ Kullanımdan kaldırma _tarihinde_, görüntü ARM dağıtımları için kullanılamaz.

Aboneliğinizdeki tüm sağlanan DSVM yansımaları, kullanımdan kaldırma tarihinden sonra çalışmaya devam edecektir. Ancak, en son işletim sistemleri ve veri bilimi araçları 'nı kullanabilmeniz için en son DSVM görüntüsüne yükseltmeniz önerilir.

## <a name="impact"></a>Etki

Aboneliğinizde var olan DSVM sağlanmış görüntüleri, kullanımdan kaldırma tarihinden sonra çalışmaya devam edecektir. Ancak, kullanıcıların DSVM görüntüsünü Azure portal ya da ARM şablonunu kullanarak daha yeni bir sürüme yükseltmesini öneririz.

> [!WARNING]
> Sanal Makine Ölçek Kümeleri kullanılarak sağlanan kullanımdan kaldırılan DSVM görüntüleri, kullanımdan kaldırma tarihinden sonra ölçeklendirilemeyebilir.
>
> Yeni DSVM görüntü ayrıntıları ile güncelleştirilmemiş ARM şablonları, kullanımdan kaldırma tarihinden sonra dağıtılamaz.

