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
ms.date: 8/29/2018
ms.author: dacurwin
ms.openlocfilehash: 227ec87081817b273b66684e31a0ebd0e11134a0
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78197138"
---
## <a name="upgrade-the-mars-agent"></a>MARS aracısını yükseltme

2\.0.9083.0 altındaki Microsoft Azure Kurtarma Hizmetleri (MARS) aracısının sürümlerinin Azure Access Control hizmetine bağımlılığı vardır. MARS aracısına Azure Backup Aracısı da denir.

2018 ' de, Microsoft [Azure Access Control hizmetini kullanımdan kaldırılmıştır](../articles/active-directory/azuread-dev/active-directory-acs-migration.md). 19 Mart 2018 ' den itibaren, 2.0.9083.0 altındaki MARS aracısının tüm sürümleri yedekleme hatalarıyla karşılaşacaktır. Yedekleme başarısızlıklarını önlemek veya çözmek için [Mars aracınızı en son sürüme yükseltin](https://go.microsoft.com/fwlink/?linkid=229525). MARS Aracısı yükseltmesi gerektiren sunucuları belirlemek için, [Mars aracılarını yükseltmek Için Yedekleme blogundan](https://blogs.technet.microsoft.com/srinathv/2018/01/17/updating-azure-backup-agents/)adımları izleyin.

MARS Aracısı, dosya ve klasörleri ve sistem durumu verilerini Azure 'a yedeklemek için kullanılır. System Center DPM ve Azure Backup Sunucusu verileri Azure 'a yedeklemek için MARS aracısını kullanın.
