---
title: Mevcut bir kullanılabilirlik kümesine Azure VM 'lerinin eklenmesinin desteklenebilirliği | Microsoft Docs
description: Bu makalede, aynı Kullanılabilirlik kümesinde hangi VM serisine karıştırabilmeniz için bir desteklenebilirlik matrisi sunulmaktadır
services: virtual-machines-linux
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
ms.assetid: ''
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: cb0034f2b353284e94d6f1508541b31040a5b076
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028414"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Mevcut bir kullanılabilirlik kümesine Azure VM 'lerinin eklenmesinin desteklenebilirliği

Mevcut bir kullanılabilirlik kümesine yeni sanal makineler (VM 'Ler) eklerken zaman zaman sınırlamalar yaşayabilirsiniz. Aşağıdaki grafikte, aynı Kullanılabilirlik kümesinde karıştırabilmeniz gereken VM Serisi ayrıntıları verilmiştir.

Farklı türlerde VM 'leri karıştırmak için desteklenebilirlik matrisi aşağıda verilmiştir:

Dizi & kullanılabilirlik kümesi|İkinci VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|İlk VM|||||||
|A||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|Av2||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|D||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|Dv2||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|
|Dv3||TAMAM|TAMAM|TAMAM|TAMAM|TAMAM|

Tüm diğer seriler, belirli bir donanım gerektirdiğinden aynı Kullanılabilirlik kümesinde yer alamadı.

Adanmış RDMA arka uç ağı gereksinimi nedeniyle A8/A9 VM boyutu karıştırılamaz.
