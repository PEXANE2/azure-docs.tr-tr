---
title: Azure Backup aracısını yükseltme
description: Bu bilgiler, Azure Backup aracısını yükseltmeniz ve yükseltmenin nereye indirileceğini açıklar.
services: backup
cloud: ''
suite: ''
author: dcurwin
manager: carmonm
ms.service: backup
ms.tgt_pltfrm: <optional>
ms.devlang: <optional>
ms.topic: article
ms.date: 03/03/2020
ms.author: dacurwin
ms.openlocfilehash: bd298f758d6109b908db01dd2ae3b97e5e2f714a
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78673207"
---
## <a name="upgrade-the-mars-agent"></a>MARS aracısını yükseltme

2\.0.9083.0 altındaki Microsoft Azure Kurtarma Hizmetleri (MARS) aracısının sürümlerinin Azure Access Control hizmetine bağımlılığı vardır. MARS aracısına Azure Backup Aracısı da denir.

2018 ' de, Microsoft [Azure Access Control hizmetini kullanımdan kaldırılmıştır](../articles/active-directory/azuread-dev/active-directory-acs-migration.md). 19 Mart 2018 ' den itibaren, 2.0.9083.0 altındaki MARS aracısının tüm sürümleri yedekleme hatalarıyla karşılaşacaktır. Yedekleme başarısızlıklarını önlemek veya çözmek için [Mars aracınızı en son sürüme yükseltin](https://support.microsoft.com/help/4538314/update-for-azure-backup-for-microsoft-azure-recovery-services-agent). MARS Aracısı yükseltmesi gerektiren sunucuları belirlemek için [Microsoft Azure kurtarma hizmetleri (mars) aracısını yükseltme](../articles/backup/upgrade-mars-agent.md)bölümündeki adımları izleyin.

MARS Aracısı, dosya ve klasörleri ve sistem durumu verilerini Azure 'a yedeklemek için kullanılır. System Center DPM ve Azure Backup Sunucusu verileri Azure 'a yedeklemek için MARS aracısını kullanın.
