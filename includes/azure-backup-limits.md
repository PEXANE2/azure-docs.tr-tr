---
title: include dosyası
description: include dosyası
services: backup
author: dcurwin
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: dacurwin
ms.custom: include file
ms.openlocfilehash: ca6c57b139b195b94dc7171336cfcc34d18b1872
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74828762"
---
Azure Backup için aşağıdaki sınırlar geçerlidir.

| **Sınır** | **Varsayılan** |
| --- | --- |
| Bir kasada kaydedilebilirler sunucu veya makineler. | Windows Server/Windows Istemci/System Center Data Protection Manager: 50. <br/><br/> IaaS VM 'Leri: 1.000.  |
| Kasa depolama alanındaki bir veri kaynağının boyutu. |en fazla 54.400 GB. Sınır IaaS VM yedeklemesi için uygulanmaz. |
| Azure aboneliğindeki yedekleme kasaları. |Bölge başına 500 kasa. |
| Günlük yedeklemeleri zamanlayın. |Windows Server/Client: üç gün.<br/> System Center DPM: Iki gün. <br/> IaaS VM 'Leri: günde bir kez.  |
| Yedekleme için bir Azure VM 'ye eklenen veri diskleri. | 16 |
| Yedekleme için Azure VM 'ye bağlı tek bir veri diski.| 32 TB|
