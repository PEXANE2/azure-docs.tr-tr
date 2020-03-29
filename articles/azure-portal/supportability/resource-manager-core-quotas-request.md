---
title: Azure Kaynak Yöneticisi vCPU kotası artış istekleri
description: Azure Kaynak Yöneticisi vCPU kotası artış istekleri
author: sowmyavenkat86
ms.author: svenkat
ms.date: 01/27/2020
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: cdbf7364a275eb246615f398044456645a96d1a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76843676"
---
# <a name="quota-increase-requests"></a>Kota artışı istekleri

Sanal makineler ve sanal makine ölçek kümeleri için Kaynak Yöneticisi vCPU kotaları, her bölgedeki her abonelik için iki katmanda uygulanır.

Birinci katman, tüm VM serilerinde Toplam Bölgesel vCPUs sınırıdır. İkinci katman, D serisi vCPUs'lar gibi VM serisi vCPUs sınırıdır. Her zaman yeni bir sanal makine dağıtılsa, bu VM serisi için yeni ve varolan vCPUs kullanımının toplamı, o vm serisi için onaylanan VCPU kotasını aşmamalıdır. Ayrıca, tüm VM serilerinde dağıtılan toplam yeni ve varolan vCPU sayısı, abonelik için onaylanan Toplam Bölgesel vCPU kotasını aşmamalıdır. Bu kotalardan biri aşılırsa, VM dağıtımına izin verilmez.
Azure portalından VM serisi için vCPUs kota sınırının artırılmasını talep edebilirsiniz. VM serisi kotasındaki artış, Toplam Bölgesel vCPUs sınırını otomatik olarak aynı miktarda artırır.

Yeni bir abonelik oluşturulduğunda, varsayılan Toplam Bölgesel vCPUs'lar tüm bireysel VM serileri için varsayılan vCPU kotalarının toplamına eşit olmayabilir. Bu gerçek, dağıtmak istediğiniz her VM serisi için yeterli kotaya sahip bir abonelikle sonuçlanabilir. Tüm dağıtımlar için Toplam Bölgesel vCPUs için yeterli kota eksikliği olabilir. Bu durumda, Toplam Bölgesel vCPUs sınırını açıkça artırmak için bir istek göndermeniz gerekir. Toplam Bölgesel vCPUs sınırı, bölge için tüm VM serilerinde onaylanan kotanın toplamını aşamaz.

Kotalar hakkında daha fazla bilgi için [Sanal makine vCPU kotaları](../../virtual-machines/windows/quotas.md) ve [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md)abakın.

