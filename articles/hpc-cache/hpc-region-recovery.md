---
title: Azure HPC Cache (Önizleme) ile bölgesel artıklık ve yük devretme kurtarma
description: Azure HPC Cache ile olağanüstü durum kurtarma için yük devretme özellikleri sağlama teknikleri
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/07/2019
ms.author: rohogue
ms.openlocfilehash: ca771ceed433c6eb0ed26bba6fcb1bf11db211f0
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72031623"
---
# <a name="use-multiple-caches-for-regional-failover-recovery"></a>Bölgesel yük devretme kurtarması için birden çok önbellek kullanma

Her Azure HPC Cache örneği belirli bir abonelik içinde ve tek bir bölgede çalışır. Bu, bölgenin tam bir kesinti olması durumunda önbellek iş akışınızın kesintiye uğramaması anlamına gelir.

Bu makalede, önbellek yük devretmesi için ikinci bir bölge kullanarak iş kesintisini azaltmaya yönelik bir strateji açıklanır.

Anahtar, birden çok bölgeden erişilebilen arka uç depolamayı kullanıyor. Bu depolama alanı, uygun DNS desteğiyle şirket içi bir NAS sistemi ya da önbellekten farklı bir bölgede bulunan Azure Blob depolama alanı olabilir.

İş akışınız birincil bölgenize devam ettikçe, veriler bölge dışında uzun vadeli depolamaya kaydedilir. Önbellek bölgesi kullanılamaz hale gelirse, ikincil bölgede yinelenen bir Azure HPC önbellek örneği oluşturabilir, aynı depolamaya bağlanabilir ve yeni önbellekten çalışmayı sürdürebilirsiniz.

## <a name="planning-for-regional-failover"></a>Bölgesel yük devretme planlaması

Olası yük devretme için hazırlanan bir önbellek ayarlamak için şu adımları izleyin:

1. Arka uç depolama alanınızı ikinci bir bölgede erişilebilir olduğundan emin olun.
1. Birincil önbellek örneğini oluşturmayı planlarken, ikinci bölgede bu kurulum işlemini çoğaltmaya de hazırlanmalısınız. Şu öğeleri ekle:

   1. Sanal ağ ve alt ağ yapısı
   1. Önbellek kapasitesi
   1. Depolama hedefi ayrıntıları, adları ve ad alanı yolları
   1. Önbellek ile aynı bölgede yer alıyorsa istemci makineler hakkındaki ayrıntılar
   1. Önbellek istemcileri tarafından kullanılmak üzere bağlama komutu

   > [!NOTE]
   > Azure HPC önbelleği, bir [Azure Resource Manager şablonu](../azure-resource-manager/template-deployment-overview.md) aracılığıyla veya doğrudan API 'sine erişerek programlı bir şekilde oluşturulabilir. Ayrıntılar için Azure HPC önbellek ekibine başvurun.

## <a name="failover-example"></a>Yük devretme örneği

Örnek olarak, Azure 'un Doğu ABD bölgesindeki Azure HPC önbelleğinizi bulmak istediğinizi düşünün. Bu, şirket içi veri merkezinizde depolanan verilere erişir.

Batı ABD 2 bölgesindeki bir önbelleği yük devretme yedeklemesi olarak kullanabilirsiniz.

Doğu ABD ' de önbellek oluştururken, Batı ABD 2 dağıtım için ikinci bir önbellek hazırlayın. Bu hazırlığı otomatikleştirmek için komut dosyası veya şablon kullanabilirsiniz.

Doğu ABD bölge genelinde hata durumunda Batı ABD 2 bölgesinde hazırladığınız önbelleği oluşturun.

Önbellek oluşturulduktan sonra, aynı şirket içi veri depolarına işaret eden depolama hedefleri ekleyin ve eski önbelleğin depolama hedefleri ile aynı toplanmış ad alanı yollarını kullanın.

Özgün istemciler etkileniyorsa, yeni önbellek ile kullanmak üzere Batı ABD 2 bölgede yeni istemciler oluşturun.

İstemcilerin bölge kesintisinden etkilenmemesi durumunda bile, tüm istemcilerin yeni önbelleği takmasına gerek olacaktır. Yeni önbelleğin eskileri 'den farklı bağlama adresleri vardır.

## <a name="learn-more"></a>Daha fazla bilgi

Azure Uygulama Mimarisi Kılavuzu, [bölge genelinde hizmet kesintisini nasıl kurtarabileceğiniz](<https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region>)hakkında daha fazla bilgi içerir.
<!-- this should be an internal link instead of a URL but I can't find the tree  -->
