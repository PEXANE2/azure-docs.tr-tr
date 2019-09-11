---
title: Azure Backup ile Azure Site Recovery kullanma desteği
description: Azure Site Recovery ve Azure Backup birlikte nasıl kullanılabileceğine ilişkin bir genel bakış sağlar.
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: sideeksh
ms.openlocfilehash: 2078aa33ca735b91b5923d94028a286427f107a6
ms.sourcegitcommit: aaa82f3797d548c324f375b5aad5d54cb03c7288
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70146867"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Azure Backup ile Site Recovery kullanma desteği

Bu makalede, [Azure Backup hizmetiyle](https://docs.microsoft.com/azure/backup/backup-overview)birlikte [Site Recovery hizmetini](site-recovery-overview.md) kullanma desteği özetlenmektedir.

**Eylem** | **Site Recovery desteği** | **Ayrıntılar**
--- | --- | ---
**Hizmetleri birlikte dağıtma** | Desteklenen | Hizmetler birlikte çalışabilir ve birlikte yapılandırılabilir.
**Dosya yedekleme/geri yükleme** | Desteklenen | Yedekleme ve çoğaltma bir VM için etkinleştirildiğinde ve yedeklemeler çekilirken, kaynak tarafı VM 'lerde veya VM grupları üzerinde dosya geri yükleme konusu yoktur. Çoğaltma, çoğaltma durumunda hiçbir değişiklik yapmadan her zamanki gibi devam eder.
**Disk yedekleme/geri yükleme** | Geçerli destek yok | Yedeklenen bir diski geri yüklerseniz, VM için çoğaltmayı yeniden devre dışı bırakıp yeniden etkinleştirmeniz gerekir.
**VM yedeklemesi/geri yükleme** | Geçerli destek yok | Bir VM 'yi veya VM grubunu yedekleyip geri yükledikten sonra VM için çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.  


