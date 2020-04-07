---
title: 'Referans: Veri Bilimi Sanal Makine Görüntü Deprecation'
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesini etkileyen amortismanlarla ilgili ayrıntılar
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 04/03/2020
ms.topic: reference
ms.openlocfilehash: 6004ae0c1fd2fa25a07ab84776ab74789f6e7da9
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80754760"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Referans: DSVM Görüntülerinin Amortismanı

Aşağıda Azure Veri Bilimi Sanal Makine'de yaklaşan amortismanlarla başa çıkma önerilerini tartışıyoruz.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: Veri disklerini geçirme

Windows 2012 DSVM görüntüsünü 31 Aralık 2019'da desteklemeyi bırakacağız. Bir veri diskini mevcut Windows 2012 DSVM'nizden Windows 2016 DSVM'ye geçirmek için aşağıdaki adımları izleyin:

1. [Burada](./provision-vm.md#create-your-dsvm)gösterilen yönergeleri izleyerek yeni bir Windows 2016 DSVM oluşturun.
1. Bu yönergeleri kullanarak varolan veri disklerini Windows 2012 resminizden [ayırın.](../../virtual-machines/windows/detach-disk.md)
1. [Bu yönergeleri](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)kullanarak önceki adımdaki diski Windows 2016 resminize takın.

## <a name="centos"></a>CentOS

Yeni kullanıcılar en son Ubuntu veya Windows görüntülerini kullanmalıdır. CentOS, varolan çözüm şablonlarıyla kullanıma hazır olmaya devam edecektir.