---
title: Azure Geçişi projesini silme
description: Azure Geçiş projesi nin nasıl oluşturulup bir değerlendirme/geçiş aracı ekleyeceğiniz açıklanır.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/22/2019
ms.author: raynew
ms.openlocfilehash: 55842d36cddb2a7851ff5bd7002c20e9873158f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73512736"
---
# <a name="delete-an-azure-migrate-project"></a>Azure Geçişi projesini silme

Bu makalede, bir [Azure Geçiş](migrate-overview.md) projesinin nasıl silinir.


## <a name="before-you-start"></a>Başlamadan önce

Bir projeyi silmeden önce:

- Bir projeyi sildiğinizde, proje ve keşfedilen makine meta verileri silinir.
- Bağımlılık çözümlemesi için Sunucu Değerlendirmesi aracına bir Log Analytics çalışma alanı iliştirdiyseniz, çalışma alanını silmek isteyip istemediğiniz konusunda karar verin. 
    - Çalışma alanı otomatik olarak silinmez. El ile silin.
    - Silmeden önce çalışma alanının ne için kullanıldığını doğrulayın. Aynı Log Analytics çalışma alanı birden çok senaryo için kullanılabilir.
    - Projeyi silmeden önce, **OMS Çalışma Alanı**altında **Azure Geçir - Sunucular** > **Azure Geçir - Sunucu Değerlendirmesi**'nde çalışma alanına bir bağlantı bulabilirsiniz.
    - Projeyi sildikten sonra bir çalışma alanını silmek için, ilgili kaynak grubundaki çalışma alanını bulun ve [bu yönergeleri](../azure-monitor/platform/delete-workspace.md)izleyin.


## <a name="delete-a-project"></a>Projeyi silme


1. Azure portalında, projenin oluşturulduğu kaynak grubunu açın.
2. Kaynak grubu sayfasında, **gizli türleri göster'i**seçin.
3. Projeyi ve silmek istediğiniz ilişkili kaynakları seçin.
    - Azure Geçir projeleri için kaynak türü **Microsoft.Migrate/geçir projeleridir.**
    - Sonraki bölümde, bir Azure Geçiş projesinde keşif, değerlendirme ve geçiş için oluşturulan kaynakları gözden geçirin.
    - Kaynak grubu yalnızca Azure Geçiş projesini içeriyorsa, tüm kaynak grubunu silebilirsiniz.
    - Bir projeyi Azure Geçiş'in önceki sürümünden silmek istiyorsanız, adımlar aynıdır. Bu projelerin kaynak türü **Geçiş projesidir.**


## <a name="created-resources"></a>Oluşturulan kaynaklar

Bu tablolar, bir Azure Geçiş projesinde keşif, değerlendirme ve geçiş için oluşturulan kaynakları özetler.

> [!NOTE]
> Güvenlik anahtarları içerebileceğinden anahtar kasasını dikkatli bir şekilde silin.

### <a name="vmwarephysical-server"></a>VMware/fiziksel sunucu

**Kaynak** | **Tür**
--- | ---
"Cihaz adı"kv | Key Vault
"Cihaz adı" sitesi | Microsoft.OffAzure/VMwareSiteleri
"Proje Adı" | Microsoft.Migrate/migrate projects
"ProjectName" projesi | Microsoft.Migrate/assessmentProjects
"ProjectName"rsvault | Kurtarma Hizmetleri kasası
"Proje Adı"-MigrateVault-* | Kurtarma Hizmetleri kasası
migrateappligwsa* | Depolama hesabı
migrateapplilsa* | Depolama hesabı
migrateapplicsa* | Depolama hesabı
migrateapplikv* | Key Vault
göçapplisbns16041 | Service Bus Ad Alanı

### <a name="hyper-v-vm"></a>Hyper-V VM 

**Kaynak** | **Tür**
--- | ---
"Proje Adı" | Microsoft.Migrate/migrate projects
"ProjectName" projesi | Microsoft.Migrate/assessmentProjects
HyperV*kv | Key Vault
HyperV*Sitesi | Microsoft.OffAzure/HyperVSites
"Proje Adı"-MigrateVault-* | Kurtarma Hizmetleri kasası


## <a name="next-steps"></a>Sonraki adımlar

Ek [değerlendirme](how-to-assess.md) ve [geçiş](how-to-migrate.md) araçları eklemeyi öğrenin. 
