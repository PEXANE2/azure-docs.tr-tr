---
title: Azure Yedekleme Aracısını Yükseltin
description: Bu bilgiler, Azure Yedekleme Aracısını neden yükseltmeniz gerektiğini ve yükseltmeyi nereden karşıladığınızı açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78673207"
---
## <a name="upgrade-the-mars-agent"></a>MARS Aracısını Yükselt

2.0.9083.0 altındaki Microsoft Azure Kurtarma Hizmetleri (MARS) Aracısı sürümleri, Azure Erişim Denetimi hizmetine bağımlıdır. MARS Aracısı, Azure Yedekleme Aracısı olarak da adlandırılır.

2018 yılında Microsoft, [Azure Erişim Denetimi hizmetini devre altına verdi.](../articles/active-directory/azuread-dev/active-directory-acs-migration.md) 19 Mart 2018'den itibaren MARS Agent'ın 2.0.9083.0 altındaki tüm sürümlerinde yedekleme hataları yaşanacaktır. Yedekleme hatalarını önlemek veya gidermek [için MARS Aracınızı en son sürüme yükseltin.](https://support.microsoft.com/help/4538314/update-for-azure-backup-for-microsoft-azure-recovery-services-agent) MARS Aracısı yükseltmesi gerektiren sunucuları tanımlamak için [Microsoft Azure Kurtarma Hizmetlerini (MARS) aracısını yükseltme](../articles/backup/upgrade-mars-agent.md)adımlarını izleyin.

MARS Aracısı, dosya ve klasörleri yedeklemek için kullanılır ve sistem durumu verilerini Azure'a yedekler. System Center DPM ve Azure Yedekleme Sunucusu, verileri Azure'a yedeklemek için MARS Aracısını kullanır.
