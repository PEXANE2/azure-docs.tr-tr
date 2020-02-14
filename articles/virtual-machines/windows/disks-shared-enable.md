---
title: Azure yönetilen diskler için Paylaşılan diskleri etkinleştir
description: Paylaşılan diskler (Önizleme) ile bir Azure yönetilen diski, bu dosyayı birden çok VM arasında paylaşabileceğiniz şekilde yapılandırma
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 7eb2f808269ec0f91725644d5afb82e12cbb885d
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202365"
---
# <a name="enable-shared-disk"></a>Paylaşılan diski etkinleştir

Bu makalede, Azure yönetilen diskler için paylaşılan diskler (Önizleme) özelliğinin nasıl etkinleştirileceği ele alınmaktadır. Azure paylaşılan diskler (Önizleme), birden çok sanal makineye (VM) aynı anda yönetilen bir disk iliştirmenizi sağlayan, Azure yönetilen disklere yönelik yeni bir özelliktir. Yönetilen bir diskin birden çok VM 'ye eklenmesi, yeni bir dağıtım veya mevcut kümelenmiş uygulamaları Azure 'a geçirmenize olanak sağlar. 

Paylaşılan diskler etkinleştirilmiş yönetilen diskler hakkında kavramsal bilgiler arıyorsanız [Azure paylaşılan diskler](disks-shared.md)' e bakın.
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]