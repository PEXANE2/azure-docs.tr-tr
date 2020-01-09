---
title: 'Başvuru: DSVM görüntüsü kullanımdan kaldırıldı'
description: Azure Veri Bilimi Sanal Makinesi (DSVM) etkileyen kullanım dışı bırakma ayrıntıları
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 10/14/2019
ms.topic: reference
ms.openlocfilehash: 252f9c66034dbadaf7a2e9e6f78665c26d414deb
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75456279"
---
# <a name="reference-deprecation-of-dsvm-images"></a>Başvuru: DSVM görüntülerinin kullanımdan kaldırılması

Aşağıda, Azure Veri Bilimi Sanal Makinesi yaklaşan kullanım dışı bırakımlar ile ilgili öneriler ele alınmaktadır.

## <a name="windows-2012-migrating-data-disks"></a>Windows 2012: veri disklerini geçirme

31 Aralık 2019 ' de Windows 2012 DSVM görüntüsünü desteklemeyi durduracağız. Mevcut Windows 2012 DSVM 'nizden bir veri diskini Windows 2016 DSVM 'ye geçirmek için aşağıdaki adımları uygulayın:

1. [Burada](./provision-vm.md#create-your-dsvm)gösterilen yönergeleri izleyerek yeni bir Windows 2016 dsvm oluşturun.
1. [Bu yönergeleri](../../virtual-machines/windows/detach-disk.md)kullanarak, Windows 2012 görüntüsünden mevcut veri disklerini ayırın.
1. [Bu yönergeleri](../../virtual-machines/windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm)kullanarak diski önceki adımdan Windows 2016 yansımanıza ekleyin.
