---
title: Dağıtıma genel bakış - Azure için Avere vFXT
description: Azure için Avere vFXT dağıtmaya genel bakış
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 78140fea74272dff6056bebfbd44ed9d55b0e1db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76153692"
---
<!-- filename is linked to in the marketplace template, make sure it gets a redirect if we rename it -->

# <a name="avere-vfxt-for-azure---deployment-overview"></a>Azure için Avere vFXT - dağıtıma genel bakış

Bu makalede, Azure kümesi için Avere vFXT'nin çalışır hale getirilmesi için gereken adımlara genel bir bakış getirilir.

Azure Marketi'nden vFXT kümesini oluşturmadan önce ve sonra birkaç görev gerekir. Başlangıç-bitiş işlemi hakkında net bir anlayışa sahip olmak, gereken çabayı kapsamanıza yardımcı olacaktır.

## <a name="deployment-steps"></a>Dağıtım adımları

[Sisteminizi planladıktan](avere-vfxt-deploy-plan.md)sonra, Avere vFXT kümenizi oluşturmaya başlayabilirsiniz.

Azure Marketi'ndeki bir Azure Kaynak Yöneticisi şablonu gerekli bilgileri toplar ve tüm kümeyi otomatik olarak dağıtır.

vFXT kümesi çalışmaya devam ettikten sonra, kullanmadan önce hala bazı yapılandırma adımları vardır. Yeni bir Blob depolama kapsayıcısı oluşturduysanız, verilerinizi bu kapsayıcıya taşımak isteyebilirsiniz. Bir NAS depolama sistemi kullanıyorsanız, küme oluşturulduktan sonra eklemeniz gerekir. İstemcileri kümeye bağlamak isteyeceksiniz.

Aşağıda tüm adımların genel görünümü vereb ve vealar yer almaktadır.

1. Ön koşulları yapılandırma

   VM oluşturmadan önce, Avere vFXT projesi için yeni bir abonelik oluşturmanız, abonelik sahipliğini yapılandırmanız, kotaları denetlemeniz ve gerekirse artış istemeniz ve Avere vFXT yazılımını kullanma koşullarını kabul etmeniz gerekir. Ayrıntılı talimatlar [için Avere vFXT'yi oluşturmak](avere-vfxt-prereqs.md) için Hazırla'yı okuyun.

1. Avere vFXT kümesini oluşturma

   Azure kümesi için Avere vFXT oluşturmak için Azure Marketi'ni kullanın. Şablon gerekli bilgileri toplar ve son ürünü oluşturmak için komut dosyalarını yürütür.

   Küme oluşturma, tüm pazar şablonu tarafından yapılan bu adımları içerir:

   * Gerekirse yeni ağ altyapısı ve kaynak grupları oluşturun
   * Küme denetleyicisi oluşturma

     Küme denetleyicisi, Avere vFXT kümesiyle aynı sanal ağda bulunan ve kümeoluşturmak ve yönetmek için gereken özel yazılıma sahip basit bir VM'dir. Denetleyici vFXT düğümlerini oluşturur ve kümeyi oluşturur ve aynı zamanda kümenin ömrü boyunca yönetmek için bir komut satırı arabirimi sağlar.

     Dağıtım sırasında yeni bir sanal ağ veya alt ağ oluşturursanız, denetleyicinizin ortak bir IP adresi olur. Bu, denetleyicinin sanal ağ dışından kümeye bağlanmak için bir atlama ana bilgisayar olarak hizmet edebileceği anlamına gelir.

   * Küme düğümü VM'leri oluşturma

   * Tek tek düğümlerden küme oluşturma

   * İsteğe bağlı olarak, yeni bir Blob kapsayıcısı oluşturun ve küme için arka uç depolama alanı olarak yapılandırır

   Küme oluşturma [vFXT kümesini dağıt'ta](avere-vfxt-deploy.md)ayrıntılı olarak açıklanmıştır.

1. Kümeyi yapılandırma

   Kümeayarlarını özelleştirmek için Avere vFXT yapılandırma arabirimine (Avere Control Panel) bağlanın. Destek izlemeyi tercih edin ve donanım depolama alanı veya ek Blob kapsayıcıları kullanıyorsanız depolama sisteminizi ekleyin.

   * [vFXT kümesine erişme](avere-vfxt-cluster-gui.md)
   * [Desteği etkinleştirme](avere-vfxt-enable-support.md)
   * [Depolamayı yapılandırın](avere-vfxt-add-storage.md) (gerekirse)

1. İstemcileri bağlama

   Yük dengeleme ve istemci makinelerin kümeye nasıl monte etmesi gerektiği hakkında bilgi edinmek için [Mount the Avere vFXT kümesindeki](avere-vfxt-mount-clients.md) yönergeleri izleyin.

1. Veri ekleme (gerekirse)

   Avere vFXT ölçeklenebilir çok istemcili bir önbellek olduğundan, verileri yeni bir arka uç depolama kapsayıcısına taşımanın en iyi yolu çok istemcili, çok iş parçacığı dosya kopyalama stratejisidir.

   Çalışma kümesi verilerini yeni bir Blob kapsayıcısına veya diğer arka uç depolama sistemine taşımanız gerekiyorsa, [verileri vFXT kümesine taşıma yönergelerini](avere-vfxt-data-ingest.md)izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Ön koşul görevlerini tamamlamak [için Avere vFXT'yi oluşturmaya devam](avere-vfxt-prereqs.md) edin.
