---
title: Azure Site Kurtarma'da fiziksel sunucular için hedef ortamı ayarlama
description: Bu makalede, Azure Site Kurtarma kullanarak fiziksel sunucuların olağanüstü durum kurtarma için hedef Azure ortamının nasıl ayarlanır.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: ramamill
ms.openlocfilehash: 685f04b27cd315b04ef5c45e155e825eebe2747b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73953893"
---
# <a name="prepare-target-vmware-to-azure"></a>Hedef hazırlama (VMware to Azure)

Bu makalede, Windows veya Linux çalıştıran fiziksel sunucuları (x64) Azure'da çoğaltmaya başlamak için Azure ortamınızı nasıl hazırlayacağınızı açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Makale varsayar:
- Fiziksel sunucularınızı korumak için bir Kurtarma Hizmetleri Kasası oluşturdunuz. [Azure portalından](https://portal.azure.com "Azure portalında")Bir Kurtarma Hizmetleri Kasası oluşturabilirsiniz.
- Fiziksel [sunucuları](physical-azure-disaster-recovery.md) Azure'a çoğaltmak için şirket içi ortamınızı kurabilirsiniz.

## <a name="prepare-target"></a>Hedefi hazırlama

**Adım 1 tamamladıktan sonra:Koruma hedef seçin** ve Adım **2:Kaynak hazırlayın,** **Adım 3 alınır: Hedef**

![Hedefi hazırlama](./media/physical-azure-set-up-target/prepare-target-physical-to-azure.png)

1. **Abonelik:** Açılan menüden, fiziksel sunucularınızı çoğaltmak istediğiniz Abonelik'i seçin.
2. **Dağıtım Modeli:** Dağıtım modelini seçin (Klasik veya Kaynak Yöneticisi)

Seçilen dağıtım modeline göre, fiziksel sunucularınızı çoğaltmak ve başarısız olmak için hedef abonelikte en az bir uyumlu depolama hesabınız ve sanal ağınız olduğundan emin olmak için bir doğrulama çalıştırılır.

Doğrulamalar başarıyla tamamlandıktan sonra, bir sonraki adıma geçmek için Tamam'ı tıklatın.

Uyumlu bir Kaynak Yöneticisi depolama hesabınız veya sanal ağınız yoksa, sayfanın üst kısmındaki **+ Depolama Hesabı** veya + **Ağ** düğmelerini tıklatarak bir hesap oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırın.](vmware-azure-set-up-replication.md)
