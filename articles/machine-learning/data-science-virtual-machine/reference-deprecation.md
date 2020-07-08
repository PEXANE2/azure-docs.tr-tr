---
title: 'Başvuru: Veri Bilimi Sanal Makinesi resim kullanımdan kaldırıldı'
titleSuffix: Azure Data Science Virtual Machine
description: Azure Veri Bilimi Sanal Makinesi etkileyen kullanım dışı bırakma ayrıntıları
author: lobrien
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: laobri
ms.date: 04/03/2020
ms.topic: reference
ms.openlocfilehash: 6004ae0c1fd2fa25a07ab84776ab74789f6e7da9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80754760"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Başvuru: DSVM görüntülerinin kullanımdan kaldırılması

Aşağıda, Azure Veri Bilimi Sanal Makinesi yaklaşan kullanım dışı bırakımlar ile ilgili öneriler ele alınmaktadır.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: veri disklerini geçirme

31 Aralık 2019 ' de Windows 2012 DSVM görüntüsünü desteklemeyi durduracağız. Mevcut Windows 2012 DSVM 'nizden bir veri diskini Windows 2016 DSVM 'ye geçirmek için aşağıdaki adımları uygulayın:

1. [Burada](./provision-vm.md#create-your-dsvm)gösterilen yönergeleri izleyerek yeni bir Windows 2016 dsvm oluşturun.
1. [Bu yönergeleri](../../virtual-machines/windows/detach-disk.md)kullanarak, Windows 2012 görüntüsünden mevcut veri disklerini ayırın.
1. [Bu yönergeleri](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)kullanarak diski önceki adımdan Windows 2016 yansımanıza ekleyin.

## <a name="centos"></a>CentOS

Yeni kullanıcıların en son Ubuntu veya Windows görüntülerini kullanması gerekir. CentOS, mevcut çözüm şablonlarıyla birlikte kullanılmak üzere kullanılabilir olmaya devam edecektir.