---
title: VMware VM için Azure 'da bir işlem sunucusu ve Azure Site Recovery ile fiziksel sunucu yeniden çalışma ayarlama | Microsoft Docs
description: Bu makalede, Azure 'da Azure VM 'Leri VMware 'e yeniden çalışma için bir işlem sunucusunun nasıl ayarlanacağı açıklanır.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/21/2019
ms.author: ramamill
ms.openlocfilehash: 080edfc86848bb6c6579c177c72d3fbd3214a06a
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2019
ms.locfileid: "72968848"
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Azure 'da yeniden çalışma için bir işlem sunucusu ayarlayın

[Site Recovery](site-recovery-overview.md)kullanarak VMware VM 'lerini veya fiziksel sunucuları Azure 'a devretmek için, yeniden çalışır duruma geldiğinde şirket içi siteye geri dönebilirsiniz. Yeniden yük devretmek için Azure 'da Azure 'dan şirket içine çoğaltma işlemini işleyecek geçici bir işlem sunucusu ayarlamanız gerekir. Yeniden çalışma tamamlandıktan sonra bu VM 'yi silebilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

[Yeniden koruma ve yeniden](vmware-azure-reprotect.md) [çalışma](vmware-azure-failback.md) işlemi hakkında daha fazla bilgi edinin.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]


## <a name="deploy-a-process-server-in-azure"></a>Azure 'da bir işlem sunucusu dağıtma

1. Kasa > **Site Recovery altyapısı**>  > **yapılandırma sunucularını** **yönetmek** için yapılandırma sunucusunu seçin.
2. Sunucu sayfasında **+ işlem sunucusu** ' na tıklayın.
3. **İşlem sunucusu Ekle** sayfasında, Işlem sunucusunu Azure 'da dağıtmayı seçin.
4. Yük devretme için kullanılan abonelik, bir kaynak grubu, yük devretme için kullanılan Azure bölgesi ve Azure VM 'lerinin bulunduğu sanal ağ dahil olmak üzere Azure ayarlarını belirtin. Birden çok Azure ağı kullandıysanız, her birinde bir işlem sunucusuna ihtiyacınız vardır.

   ![İşlem sunucusu galeri öğesi Ekle](./media/vmware-azure-set-up-process-server-azure/add-ps-page-1.png)

4. **Sunucu adı**, **Kullanıcı adı**ve **parola**' da, işlem sunucusu için bir ad ve sunucuda yönetici izinleri atanacak kimlik bilgileri belirtin.
5. Sunucu VM diskleri için kullanılacak bir depolama hesabı, işlem sunucusu VM 'sinin bulunduğu alt ağ ve VM başlatıldığında atanacak sunucu IP adresi belirtin.
6. İşlem sunucusu VM 'sini dağıtmaya başlamak için **Tamam** düğmesine tıklayın. İşlem sunucusu Standard_A8_v2 SKU 'sunda dağıtılacak. Bu VM SKU 'sunun aboneliğiniz için kullanılabilir olduğundan emin olun.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>İşlem sunucusunu (Azure 'da çalışan) bir yapılandırma sunucusuna (Şirket içinde çalışan) kaydetme

İşlem sunucusu VM 'si çalışır duruma geçtikten sonra, bunu şirket içi yapılandırma sunucusuna aşağıdaki şekilde kaydetmeniz gerekir:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


