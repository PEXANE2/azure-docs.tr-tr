---
title: Azure yönetilen diskler için paylaşılan diskleri etkinleştirme
description: Azure yönetilen bir diski paylaşılan disklerle (önizleme) yapılandırarak birden fazla VM arasında paylaşabilirsiniz
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: e7ada64a50d6ace6ea4d34db87e0501d0311071d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471727"
---
# <a name="enable-shared-disk"></a>Paylaşılan diski etkinleştirme

Bu makalede, Azure yönetilen diskler için paylaşılan diskler (önizleme) özelliğinin nasıl etkinleştirilen nasıl etkinleştirilen kapsar. Azure paylaşılan diskleri (önizleme), yönetilen bir diski aynı anda birden çok sanal makineye (VM) eklemenize olanak tanıyan Azure yönetilen diskler için yeni bir özelliktir. Yönetilen bir diski birden çok VM'ye eklemek, yeni kümelenmiş uygulamaları dağıtabilir veya varolan kümelenmiş uygulamaları Azure'a geçirmenize olanak tanır. 

Paylaşılan diskler etkinleştirilmiş yönetilen diskler hakkında kavramsal bilgiler arıyorsanız, [Azure paylaşılan disklerine](disks-shared.md)bakın.
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]