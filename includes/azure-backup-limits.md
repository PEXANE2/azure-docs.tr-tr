---
title: include dosyası
description: include dosyası
services: backup
author: rayne-wiselman
ms.service: backup
ms.topic: include
ms.date: 12/07/2018
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 9e9c09c1825f5c8383a708e8bd343146396f878e
ms.sourcegitcommit: 23389df08a9f4cab1f3bb0f474c0e5ba31923f12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2019
ms.locfileid: "67188771"
---
Azure Backup için aşağıdaki sınırlar geçerlidir.

| **Sınırı** | **Varsayılan** |
| --- | --- |
| Bir kasada kaydedilebilirler sunucu veya makineler. | Windows Server/Windows Istemcisi/System Center Data Protection Manager: kullanır. <br/><br/> IaaS VM 'Leri: 1.000.  |
| Kasa depolama alanındaki bir veri kaynağının boyutu. |en fazla 54.400 GB. Sınır IaaS VM yedeklemesi için uygulanmaz. |
| Azure aboneliğindeki yedekleme kasaları. |Bölge başına 500 kasa. |
| Günlük yedeklemeleri zamanlayın. |Windows Server/Client: Üç gün.<br/> System Center DPM: İki gün. <br/> IaaS VM 'Leri: Günde bir kez.  |
| Yedekleme için bir Azure VM 'ye eklenen veri diskleri. | 16 |
| Yedekleme için Azure VM 'ye bağlı tek bir veri diski.| 4\.095 GB|
