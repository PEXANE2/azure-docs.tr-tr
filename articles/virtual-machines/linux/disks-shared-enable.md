---
title: Azure yönetilen diskler için Paylaşılan diskleri etkinleştir
description: Paylaşılan diskler (Önizleme) ile bir Azure yönetilen diski, bu dosyayı birden çok VM arasında paylaşabileceğiniz şekilde yapılandırma
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 06/03/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 3b949905f311b1a8878aa691e32abc3f568e1e6b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84657296"
---
# <a name="enable-shared-disk"></a>Paylaşılan diski etkinleştirme

Bu makalede, Azure yönetilen diskler için paylaşılan diskler (Önizleme) özelliğinin nasıl etkinleştirileceği ele alınmaktadır. Azure paylaşılan diskler (Önizleme), birden çok sanal makineye (VM) aynı anda yönetilen bir disk iliştirmenizi sağlayan, Azure yönetilen disklere yönelik yeni bir özelliktir. Yönetilen bir diskin birden çok VM 'ye eklenmesi, yeni bir dağıtım veya mevcut kümelenmiş uygulamaları Azure 'a geçirmenize olanak sağlar. 

Paylaşılan diskler etkinleştirilmiş yönetilen diskler hakkında kavramsal bilgiler arıyorsanız [Azure paylaşılan diskler](disks-shared.md)' e bakın.
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]