---
title: Azure Site Kurtarma'da VMware VM çoğaltma hedefini hazırlama
description: Bu makalede, Hedef Azure ortamınızın VMware VM çoğaltması için Azure'a nasıl hazırlanacağı açıklanmaktadır.
services: site-recovery
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 03/03/2019
ms.author: mayg
ms.openlocfilehash: c721234f2e1d806d51d31f3466e441bf8360f6b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73693175"
---
# <a name="prepare-the-target-environment-for-disaster-recovery-of-vmware-vms-or-physical-servers-to-azure"></a>Hedef ortamı, VMware VM'lerinin veya fiziksel sunucularının Azure'a olağanüstü kurtarma durumu için hazırlama

Bu makalede, VMware sanal makinelerini veya fiziksel sunucularını Azure'da çoğaltmaya başlamak için hedef Azure ortamınızı nasıl hazırlayacağınızı açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Makale varsayar:
- Kaynak makinelerinizi korumak için [Azure portalında](https://portal.azure.com "Azure portalında") bir Kurtarma Hizmetleri Kasası oluşturdunuz
- Kaynak [VMware sanal makinelerini](vmware-azure-set-up-source.md) veya [fiziksel sunucularını](physical-azure-set-up-source.md) Azure'a çoğaltmak için şirket içi ortamınızı kurabilirsiniz.

## <a name="prepare-target"></a>Hedefi hazırlama

Adım 1 tamamladıktan **sonra: Koruma hedefi seçin** ve **Adım 2: Kaynak hazırlayın**, **Adım 3 alınır: Hedef**

![Hedefi hazırlama](./media/vmware-azure-set-up-target/prepare-target-vmware-to-azure.png)

1. **Abonelik:** Açılan menüden, sanal makinelerinizi veya fiziksel sunucularınızı çoğaltmak istediğiniz Abonelik'i seçin.
2. **Dağıtım Modeli:** Dağıtım modelini seçin (Klasik veya Kaynak Yöneticisi)

Seçilen dağıtım modeline bağlı olarak, sanal makinenizin veya fiziksel sunucunuzun çoğaltılması ve başarısız olması için hedef abonelikte en az bir sanal ağınız olduğundan emin olmak için bir doğrulama çalıştırılır.

Doğrulamalar başarıyla tamamlandıktan sonra, bir sonraki adıma geçmek için Tamam'ı tıklatın.

Sanal ağınız yoksa, sayfanın üst kısmındaki + **Ağ** düğmesini tıklatarak bir ağ oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Çoğaltma ayarlarını yapılandırın.](vmware-azure-set-up-replication.md)
