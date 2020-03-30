---
title: Varolan bir kullanılabilirlik kümesine Azure VM'leri eklemenin desteklenebilirliği | Microsoft Dokümanlar
description: Bu makalede, aynı kullanılabilirlik kümesinde karıştırabileceğiniz VM serisi hakkında bir desteklenebilirlik matrisi sağlar
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
ms.service: virtual-machines
ms.topic: troubleshooting
ms.date: 06/15/2018
ms.author: delhan
ms.openlocfilehash: a9ca8f219bef787de04b51600209bfd3a24dd166
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77122928"
---
# <a name="supportability-of-adding-azure-vms-to-an-existing-availability-set"></a>Varolan kullanılabilirlik kümesine Azure VM'leri eklemenin desteklenebilirliği

Varolan bir kullanılabilirlik kümesine yeni sanal makineler (VM) eklediğinizde zaman zaman sınırlamalarla karşılaşabilirsiniz. Aynı kullanılabilirlik kümesinde karıştırabileceğiniz VM serisinin ayrıntılarını içeren aşağıdaki grafik ayrıntıları.

Farklı VM türlerini bir araya getirmek için desteklenebilirlik matrisi aşağıda verilmiştir:

Seri & Kullanılabilirlik Seti|İkinci VM|A|Av2|D|Dv2|Dv3|
|---|---|---|---|---|---|---|
|İlk VM|||||||
|A||Tamam|Tamam|Tamam|Tamam|Tamam|
|Av2||Tamam|Tamam|Tamam|Tamam|Tamam|
|D||Tamam|Tamam|Tamam|Tamam|Tamam|
|Dv2||Tamam|Tamam|Tamam|Tamam|Tamam|
|Dv3||Tamam|Tamam|Tamam|Tamam|Tamam|

Belirli bir donanım gerektirdiğinden, diğer tüm seriler aynı kullanılabilirlik kümesinde bulunamaz.

A8/A9 VM boyutu, özel RDMA arka uç ağındaki gereksinim nedeniyle karıştırılamaz.
