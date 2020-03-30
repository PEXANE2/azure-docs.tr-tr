---
title: Azure Site Kurtarma'da işlem sunucusu VMware/fiziksel hata geri ödeme ayarlama
description: Bu makalede, Azure VM'leri VMware'e geri vermek için Azure'da bir işlem sunucusunun nasıl kurulup kuruldestekleyeceği açıklanmaktadır.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/21/2019
ms.author: ramamill
ms.openlocfilehash: 16214fa07b14c8db2f32fa2c69739b7fa3dc1907
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74083962"
---
# <a name="set-up-a-process-server-in-azure-for-failback"></a>Azure’da yeniden çalışma için işlem sunucusu ayarlama

[Site Kurtarma'yı](site-recovery-overview.md)kullanarak VMware VM'leri veya fiziksel sunucuları Azure'da başarısız olduktan sonra, yeniden çalışır durumdayken bunları şirket içi siteye geri getirebilirsiniz. Geri dönmekte başarısız olmak için, Azure'dan şirket başına çoğaltma işlemini işlemek için Azure'da geçici bir işlem sunucusu ayarlamanız gerekir. Failback tamamlandıktan sonra bu VM silebilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce

[Yeniden koruma](vmware-azure-reprotect.md) ve [geri alma](vmware-azure-failback.md) işlemi hakkında daha fazla bilgi edinin.

[!INCLUDE [site-recovery-vmware-process-server-prerequ](../../includes/site-recovery-vmware-azure-process-server-prereq.md)]


## <a name="deploy-a-process-server-in-azure"></a>Azure'da bir işlem sunucusu dağıtma

1. Site Kurtarma **Altyapısı**> **>** > **Configuration Servers'ın**kasasında yapılandırma sunucusunu seçin.
2. Sunucu sayfasında + **İşlem sunucusuna** tıklayın
3. **İşlem sunucu** ekle sayfasında ve işlem sunucusunu Azure'da dağıtmak için seçin.
4. Başarısız olmak için kullanılan abonelik, kaynak grubu, başarısız olmak için kullanılan Azure bölgesi ve Azure Sanal M'lerinin bulunduğu sanal ağ dahil olmak üzere Azure ayarlarını belirtin. Birden çok Azure ağı kullandıysanız, her birinde bir işlem sunucusuna ihtiyacınız var.

   ![İşlem sunucusu galeri öğesi ekleme](./media/vmware-azure-set-up-process-server-azure/add-ps-page-1.png)

4. **Sunucu adı**, **Kullanıcı adı**ve **Parola**, işlem sunucusu için bir ad ve sunucuda Yönetici izinleri atanacak kimlik bilgileri belirtin.
5. Sunucu VM diskleri için kullanılacak bir depolama hesabı, işlem sunucusu VM'nin bulunacağı alt ağ ve VM başlatıldığında atanacak sunucu IP adresi belirtin.
6. İşlem sunucusu VM'yi dağıtmaya başlamak için **Tamam** düğmesini tıklatın. İşlem sunucusu Standard_A8_v2 SKU'da dağıtılacaktır. Bu VM SKU'nun aboneliğiniz için kullanılabilir olduğundan emin olun.

>

## <a name="registering-the-process-server-running-in-azure-to-a-configuration-server-running-on-premises"></a>İşlem sunucusunu (Azure'da çalışan) yapılandırma sunucusuna kaydetme (şirket içinde çalışma)

İşlem sunucusu VM çalışmaya başladıktan sonra, aşağıdaki gibi şirket içi yapılandırma sunucusuna kaydetmeniz gerekir:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]


