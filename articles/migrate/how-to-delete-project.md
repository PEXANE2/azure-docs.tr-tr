---
title: Azure Geçişi projesini silme
description: Azure geçişi projesi oluşturmayı ve değerlendirme/geçiş aracının nasıl ekleneceğini açıklar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 10/22/2019
ms.author: raynew
ms.openlocfilehash: 55842d36cddb2a7851ff5bd7002c20e9873158f5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "73512736"
---
# <a name="delete-an-azure-migrate-project"></a>Azure Geçişi projesini silme

Bu makalede bir [Azure geçişi](migrate-overview.md) projesinin nasıl silineceği açıklanır.


## <a name="before-you-start"></a>Başlamadan önce

Bir projeyi silmeden önce:

- Bir projeyi sildiğinizde, proje ve bulunan makine meta verileri silinir.
- Bağımlılık analizi için sunucu değerlendirmesi aracına bir Log Analytics çalışma alanı eklediyseniz, çalışma alanını silmek istediğinize karar verin. 
    - Çalışma alanı otomatik olarak silinmez. El ile silin.
    - Silmeden önce bir çalışma alanının ne için kullanıldığını doğrulayın. Aynı Log Analytics çalışma alanı birden çok senaryo için kullanılabilir.
    - Projeyi silmeden önce, **OMS çalışma alanı**altında **Azure geçişi-sunucuları** > **Azure geçişi-sunucu değerlendirmesi**' ndeki çalışma alanına bir bağlantı bulabilirsiniz.
    - Bir projeyi sildikten sonra bir çalışma alanını silmek için ilgili kaynak grubundaki çalışma alanını bulun ve [Bu yönergeleri](../azure-monitor/platform/delete-workspace.md)izleyin.


## <a name="delete-a-project"></a>Projeyi silme


1. Azure portal, projenin oluşturulduğu kaynak grubunu açın.
2. Kaynak grubu sayfasında **gizli türleri göster**' i seçin.
3. Projeyi ve silmek istediğiniz ilişkili kaynakları seçin.
    - Azure geçişi projelerinin kaynak türü **Microsoft. Migrate/migrateprojects**.
    - Bir sonraki bölümde, bir Azure geçişi projesinde bulma, değerlendirme ve geçiş için oluşturulan kaynakları gözden geçirin.
    - Kaynak grubu yalnızca Azure geçişi projesini içeriyorsa, tüm kaynak grubunu silebilirsiniz.
    - Azure geçişi 'nin önceki sürümünden bir projeyi silmek istiyorsanız, adımlar aynıdır. Bu projelerin kaynak türü **geçiş projem**.


## <a name="created-resources"></a>Oluşturulan kaynaklar

Bu tablolar, bir Azure geçişi projesinde bulma, değerlendirme ve geçiş için oluşturulan kaynakları özetler.

> [!NOTE]
> Anahtar kasasını, güvenlik anahtarları içerebileceğinden dikkatli bir şekilde silin.

### <a name="vmwarephysical-server"></a>VMware/fiziksel sunucu

**Kaynak** | **Tür**
--- | ---
"Appliancename" kV | Key Vault
"Appliancename" sitesi | Microsoft. OffAzure/VMwareSites
ProjectName | Microsoft. Migrate/migrateprojects
"ProjectName" projesi | Microsoft. Migrate/assessmentProjects
"ProjectName" rskasa | Kurtarma Hizmetleri kasası
"ProjectName"-Migratekasa-* | Kurtarma Hizmetleri kasası
migrateappligwsa* | Depolama hesabı
migrateappzasa * | Depolama hesabı
migrateapplicsa* | Depolama hesabı
migrateapplikv * | Key Vault
migrateapplisbns16041 | Service Bus Ad Alanı

### <a name="hyper-v-vm"></a>Hyper-V VM 

**Kaynak** | **Tür**
--- | ---
ProjectName | Microsoft. Migrate/migrateprojects
"ProjectName" projesi | Microsoft. Migrate/assessmentProjects
HyperV * kV | Key Vault
HyperV * sitesi | Microsoft. OffAzure/HyperVSites
"ProjectName"-Migratekasa-* | Kurtarma Hizmetleri kasası


## <a name="next-steps"></a>Sonraki adımlar

Ek [değerlendirme](how-to-assess.md) ve [geçiş](how-to-migrate.md) araçları eklemeyi öğrenin. 
