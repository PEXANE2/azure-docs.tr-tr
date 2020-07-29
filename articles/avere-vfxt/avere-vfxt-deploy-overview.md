---
title: Dağıtıma genel bakış-Azure için avere vFXT
description: Azure için avere vFXT dağıtmaya genel bakış
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 78140fea74272dff6056bebfbd44ed9d55b0e1db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76153692"
---
<!-- filename is linked to in the marketplace template, make sure it gets a redirect if we rename it -->

# <a name="avere-vfxt-for-azure---deployment-overview"></a>Azure için avere vFXT-dağıtıma genel bakış

Bu makale, Azure kümesi için avere vFXT 'nin çalışır duruma getirmek için gereken adımlara genel bir bakış sunar.

Azure Marketi 'nden vFXT kümesini oluşturmadan önce ve sonra birkaç görev gereklidir. Baştan sona işlemin bitmesine açık olması, gereken çabaların kapsamını belirlemenize yardımcı olur.

## <a name="deployment-steps"></a>Dağıtım adımları

[Sisteminizi planlamadan](avere-vfxt-deploy-plan.md)sonra, avere vFXT kümenizi oluşturmaya başlayabilirsiniz.

Azure Marketi 'ndeki bir Azure Resource Manager şablonu, gerekli bilgileri toplar ve tüm kümeyi otomatik olarak dağıtır.

VFXT kümesi çalışır duruma geçtikten sonra, kullanmadan önce bazı yapılandırma adımları devam eder. Yeni bir BLOB depolama kapsayıcısı oluşturduysanız, verilerinizi buna taşımak isteyeceksiniz. Bir NAS depolama sistemi kullanıyorsanız, küme oluşturulduktan sonra eklemeniz gerekir. İstemcileri kümeye bağlamak isteyeceksiniz.

Tüm adımlara genel bakış aşağıda verilmiştir.

1. Önkoşulları yapılandırma

   Bir VM oluşturmadan önce, avere vFXT projesi için yeni bir abonelik oluşturmanız, abonelik sahipliğini yapılandırmanız, kotaları denetlemeniz ve gerekirse bir artış istemeniz ve avere vFXT yazılımının kullanımı için koşulları kabul etmeniz gerekir. Ayrıntılı yönergeler için [avere vFXT oluşturmayı hazırlama](avere-vfxt-prereqs.md) konusunu okuyun.

1. Avere vFXT kümesi oluşturma

   Azure için avere vFXT 'yi oluşturmak için Azure Marketi 'ni kullanın. Şablon, gerekli bilgileri toplar ve son ürünü oluşturmak için betikleri yürütür.

   Küme oluşturma işlemi, Market şablonu tarafından gerçekleştirilen bu adımları içerir:

   * Gerekirse yeni ağ altyapısı ve kaynak grupları oluşturun
   * Küme denetleyicisi oluşturma

     Küme denetleyicisi, avere vFXT kümesiyle aynı sanal ağda bulunan ve kümeyi oluşturmak ve yönetmek için gerekli özel yazılıma sahip olan basit bir VM 'dir. Denetleyici, vFXT düğümlerini oluşturur ve kümeyi oluşturur ve ayrıca, yaşam süresi boyunca kümeyi yönetmek için bir komut satırı arabirimi sağlar.

     Dağıtım sırasında yeni bir sanal ağ veya alt ağ oluşturursanız, denetleyicinizin bir genel IP adresi olur. Bu, denetleyicinin sanal ağ dışından kümeye bağlanmak için bir geçiş ana bilgisayarı olarak kullanılabileceği anlamına gelir.

   * Küme düğümü VM 'lerini oluşturma

   * Kümeyi ayrı düğümlerden oluşturma

   * İsteğe bağlı olarak, yeni bir blob kapsayıcısı oluşturun ve küme için arka uç depolaması olarak yapılandırın

   Küme oluşturma [, vFXT kümesini dağıtma](avere-vfxt-deploy.md)bölümünde ayrıntılı olarak açıklanmıştır.

1. Kümeyi yapılandırma

   Kümenin ayarlarını özelleştirmek için avere vFXT yapılandırma arabirimine (avere Denetim Masası) bağlanın. Destek izleme için kabul edin ve donanım depolama veya ek blob kapsayıcıları kullanıyorsanız depolama sisteminizi ekleyin.

   * [vFXT kümesine erişme](avere-vfxt-cluster-gui.md)
   * [Desteği etkinleştir](avere-vfxt-enable-support.md)
   * [Depolamayı yapılandırma](avere-vfxt-add-storage.md) (gerekirse)

1. İstemcileri bağlama

   Yük Dengeleme ve istemci makinelerin kümeyi nasıl bağlayacağınızı öğrenmek için [avere vFXT kümesini bağlama](avere-vfxt-mount-clients.md) bölümündeki yönergeleri izleyin.

1. Veri Ekle (gerekirse)

   Avere vFXT, ölçeklenebilir bir çoklu istemci önbelleği olduğundan, verileri yeni bir arka uç depolama kapsayıcısına taşımanın en iyi yolu, çok istemci, çok iş parçacıklı bir dosya kopyalama stratejisidir.

   Çalışma kümesi verilerini yeni bir blob kapsayıcısına veya başka bir arka uç depolama sistemine taşımanız gerekiyorsa, [verileri vFXT kümesine taşıma](avere-vfxt-data-ingest.md)bölümündeki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Önkoşul görevlerini gerçekleştirmek için [avere vFXT oluşturmayı hazırlamaya](avere-vfxt-prereqs.md) devam edin.
