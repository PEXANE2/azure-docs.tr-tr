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
ms.openlocfilehash: ac56e2e69cf62c8afae73e0a7616e7e7f80c0493
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88816329"
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

## <a name="mitigating-upcoming-retirements"></a>Yaklaşan retirements

Bu bölümde yaklaşan retirements için risk azaltma tartışıyoruz.

### <a name="upgrade-windows-2016-dsvm"></a>Windows 2016 DSVM 'YI yükseltme

Mevcut Windows 2016 DSVM 'nizden bir veri diskini Windows 2019 DSVM 'ye geçirmek için aşağıdaki adımları uygulayın:

1. [Burada](./provision-vm.md#create-your-dsvm)gösterilen yönergeleri izleyerek yeni bir Windows 2019 dsvm oluşturun.
1. [Bu yönergeleri](../../virtual-machines/windows/detach-disk.md)kullanarak, Windows 2016 görüntüsünden mevcut veri disklerini ayırın.
1. [Bu yönergeleri](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)kullanarak diski önceki adımdan Windows 2019 yansımanıza ekleyin.

### <a name="upgrade-ubuntu-1604-dsvm"></a>Ubuntu 16,04 DSVM 'YI yükseltme

Mevcut Ubuntu 16,04 DSVMs 'yi [ubuntu 18,04 DSVM sürümüne](./dsvm-ubuntu-intro.md)yükseltmenizi öneririz.
