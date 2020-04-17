---
title: Azure HPC Önbelleği ile bölgesel artıklık ve başarısız kurtarma
description: Azure HPC Önbelleği ile olağanüstü durum kurtarma için başarısız yetenekler sağlama teknikleri
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: 21074ae6bc4959da031bc7065cd7d0639ec2a14f
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537279"
---
# <a name="use-multiple-caches-for-regional-failover-recovery"></a>Bölgesel başarısız kurtarma için birden çok önbellek kullanma

Her Azure HPC Önbelleği örneği belirli bir abonelik içinde ve tek bir bölgede çalışır. Bu, bölge tam bir kesinti varsa önbellek iş akışının kesintiye uğrayabileceği anlamına gelir.

Bu makalede, önbellek kesintisi için ikinci bir bölge kullanarak iş kesintisi riskini azaltmak için bir strateji açıklanmaktadır.

Anahtar, birden çok bölgeden erişilebilen arka uç depolama alanını kullanmaktır. Bu depolama, uygun DNS desteğine sahip şirket içi bir NAS sistemi veya önbellekten farklı bir bölgede bulunan Azure Blob depolama alanı olabilir.

İş akışınız birincil bölgenizde ilerledikçe, veriler bölge dışındaki uzun süreli depolama alanına kaydedilir. Önbellek bölgesi kullanılamıyorsa, ikincil bir bölgede yinelenen bir Azure HPC Önbelleği örneği oluşturabilir, aynı depolama alanına bağlanabilir ve yeni önbellekten çalışmaya devam edebilirsiniz.

## <a name="planning-for-regional-failover"></a>Bölgesel başarısız için planlama

Olası başarısızlık için hazırlanmış bir önbellek ayarlamak için aşağıdaki adımları izleyin:

1. Arka uç depolamanızın ikinci bir bölgede erişilebilir olduğundan emin olun.
1. Birincil önbellek örneğini oluşturmayı planlarken, bu kurulum işlemini ikinci bölgede çoğaltmaya da hazırlanmanız gerekir. Bu öğeleri ekleyin:

   1. Sanal ağ ve alt ağ yapısı
   1. Önbellek kapasitesi
   1. Depolama hedef ayrıntıları, adlar ve ad alanı yolları
   1. Önbellekle aynı bölgede bulunuyorlarsa, istemci makineleri yle ilgili ayrıntılar
   1. Önbellek istemcileri tarafından kullanılmak üzere montaj komutu

   > [!NOTE]
   > Azure HPC Önbelleği, bir [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/templates/overview.md) aracılığıyla veya API'sine doğrudan erişerek programlı olarak oluşturulabilir. Ayrıntılar için Azure HPC Önbellek ekibine başvurun.

## <a name="failover-example"></a>Failover örneği

Örnek olarak, Azure HPC Önbelleğinizi Azure'un Doğu ABD bölgesinde bulmak istediğinizi düşünün. Şirket içi veri merkezinizde depolanan verilere erişecektir.

Batı ABD 2 bölgesindeki bir önbelleği başarısız yedekleme olarak kullanabilirsiniz.

Doğu ABD'de önbellek oluştururken, Batı ABD 2'de dağıtım için ikinci bir önbellek hazırlayın. Bu hazırlığı otomatikleştirmek için komut dosyası oluşturma yı veya şablonları kullanabilirsiniz.

Doğu ABD'de bölge çapında bir hata olması durumunda, Batı ABD 2 bölgesinde hazırladığınız önbelleği oluşturun.

Önbellek oluşturulduktan sonra, aynı şirket içi veri depolarını gösteren depolama hedefleri ekleyin ve eski önbelleğin depolama hedefleriyle aynı toplu ad alanı yollarını kullanın.

Özgün istemciler etkilenirse, yeni önbellekle kullanılmak üzere Batı ABD 2 bölgesinde yeni istemciler oluşturun.

İstemciler bölge kesintisinden etkilenmese bile, tüm istemcilerin yeni önbelleği takması gerekir. Yeni önbellek, eskisinden farklı montaj adreslerine sahiptir.

## <a name="learn-more"></a>Daha fazla bilgi edinin

Azure uygulama mimarisi kılavuzu, bölge [çapındaki bir hizmet kesintisinden](<https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region>)nasıl kurtarılalabilen hakkında daha fazla bilgi içerir.
