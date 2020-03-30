---
title: 'Referans: Veri Bilimi Sanal Makine Görüntü Deprecation'
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesini etkileyen amortismanlarla ilgili ayrıntılar
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 10/14/2019
ms.topic: reference
ms.openlocfilehash: 71ce8519dca6fa71340b1c34fe3a6891ccf5753f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77525796"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referans: DSVM Görüntülerinin Amortismanı

Aşağıda Azure Veri Bilimi Sanal Makine'de yaklaşan amortismanlarla başa çıkma önerilerini tartışıyoruz.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Veri disklerini geçirme

Windows 2012 DSVM görüntüsünü 31 Aralık 2019'da desteklemeyi bırakacağız. Bir veri diskini mevcut Windows 2012 DSVM'nizden Windows 2016 DSVM'ye geçirmek için aşağıdaki adımları izleyin:

1. [Burada](./provision-vm.md#create-your-dsvm)gösterilen yönergeleri izleyerek yeni bir Windows 2016 DSVM oluşturun.
1. Bu yönergeleri kullanarak varolan veri disklerini Windows 2012 resminizden [ayırın.](../../virtual-machines/windows/detach-disk.md)
1. [Bu yönergeleri](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)kullanarak önceki adımdaki diski Windows 2016 resminize takın.
