---
title: Mevcut bir kullanılabilirlik kümesine Azure VM 'lerinin eklenmesinin desteklenebilirliği | Microsoft Docs
description: Bu makalede, aynı Kullanılabilirlik kümesinde hangi VM serisine karıştırabilmeniz için bir desteklenebilirlik matrisi sunulmaktadır
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: a9ca8f219bef787de04b51600209bfd3a24dd166
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77122928"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Mevcut bir kullanılabilirlik kümesine Azure VM 'lerinin eklenmesinin desteklenebilirliği

Mevcut bir kullanılabilirlik kümesine yeni sanal makineler (VM 'Ler) eklerken zaman zaman sınırlamalar yaşayabilirsiniz. Aşağıdaki grafikte, aynı Kullanılabilirlik kümesinde karıştırabilmeniz gereken VM Serisi ayrıntıları verilmiştir.

Farklı türlerde VM 'leri karıştırmak için desteklenebilirlik matrisi aşağıda verilmiştir:

Dizi & kullanılabilirlik kümesi|İkinci VM|A|AV2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|İlk VM|||||||
|A||Tamam|Tamam|Tamam|Tamam|Tamam|
|AV2||Tamam|Tamam|Tamam|Tamam|Tamam|
|D||Tamam|Tamam|Tamam|Tamam|Tamam|
|Dv2||Tamam|Tamam|Tamam|Tamam|Tamam|
|Dv3||Tamam|Tamam|Tamam|Tamam|Tamam|

Tüm diğer seriler, belirli bir donanım gerektirdiğinden aynı Kullanılabilirlik kümesinde yer alamadı.

Adanmış RDMA arka uç ağı gereksinimi nedeniyle A8/A9 VM boyutu karıştırılamaz.
