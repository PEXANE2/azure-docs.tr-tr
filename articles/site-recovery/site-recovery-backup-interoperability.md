---
title: Azure Yedekleme ile Azure Site Kurtarma'yı kullanma desteği
description: Azure Site Kurtarma ve Azure Yedekleme'nin birlikte nasıl kullanılabileceğine genel bir bakış sağlar.
author: sideeksh
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: sideeksh
ms.openlocfilehash: d8c896bc655339fbd7d51f738fb629450e8c12ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72376210"
---
# <a name="support-for-using-site-recovery-with-azure-backup"></a>Azure Yedekleme ile Site Kurtarma'yı kullanma desteği

Bu makalede, [Azure Yedekleme hizmeti](https://docs.microsoft.com/azure/backup/backup-overview)ile birlikte Site Kurtarma [hizmetini](site-recovery-overview.md) kullanma desteği özetlenmiştir.

**Eylem** | **Site Recovery desteği** | **Şey**
--- | --- | ---
**Hizmetleri birlikte dağıtma** | Destekleniyor | Hizmetler birlikte çalışabilir ve birlikte yapılandırılabilir.
**Dosya yedekleme/geri yükleme** | Destekleniyor | VM için yedekleme ve çoğaltma etkinleştirildiğinde ve yedeklemeler alındığında, kaynak tarafındaki VM'lerde veya VM grubundaki dosyaları geri almakta bir sorun yoktur. Çoğaltma, çoğaltma durumunda hiçbir değişiklik olmadan her zamanki gibi devam ediyor.
**Disk geri yükleme** | Geçerli destek yok | Yedeklenmiş bir diski geri yüklerseniz, VM için çoğaltmayı yeniden devre dışı bırakıp yeniden etkinleştirmeniz gerekir.
**VM geri yükleme** | Geçerli destek yok | Bir VM veya VM grubu geri yükseniz, VM için çoğaltmayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.  


