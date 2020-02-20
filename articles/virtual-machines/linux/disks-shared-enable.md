---
title: Azure yönetilen diskler için Paylaşılan diskleri etkinleştir
description: Paylaşılan diskler (Önizleme) ile bir Azure yönetilen diski, bu dosyayı birden çok VM arasında paylaşabileceğiniz şekilde yapılandırma
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 4562567aea667dd7efadbcfb033e782ec9418e3f
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77472069"
---
# <a name="enable-shared-disk"></a>Paylaşılan diski etkinleştir

Bu makalede, Azure yönetilen diskler için paylaşılan diskler (Önizleme) özelliğinin nasıl etkinleştirileceği ele alınmaktadır. Azure paylaşılan diskler (Önizleme), birden çok sanal makineye (VM) aynı anda yönetilen bir disk iliştirmenizi sağlayan, Azure yönetilen disklere yönelik yeni bir özelliktir. Yönetilen bir diskin birden çok VM 'ye eklenmesi, yeni bir dağıtım veya mevcut kümelenmiş uygulamaları Azure 'a geçirmenize olanak sağlar. 

Paylaşılan diskler etkinleştirilmiş yönetilen diskler hakkında kavramsal bilgiler arıyorsanız [Azure paylaşılan diskler](disks-shared.md)' e bakın.
[!INCLUDE [virtual-machines-enable-shared-disk](../../../includes/virtual-machines-enable-shared-disk.md)]