---
title: Bir VM'den Azure DevTest Labs özel görüntü oluşturma | Microsoft Dokümanlar
description: Azure portalını kullanarak sağlanan bir VM'den Azure DevTest Labs'da nasıl özel bir görüntü oluşturabilirsiniz öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 07f3b60b9218f74bb3a778daa27f31687c4538b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60868508"
---
# <a name="create-a-custom-image-from-a-vm"></a>VM’den özel görüntü oluşturma

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Sağlanan bir VM'den özel bir görüntü oluşturabilir ve daha sonra aynı VM'leri oluşturmak için bu özel görüntüyü kullanabilirsiniz. Aşağıdaki adımlar, VM'den özel bir görüntünün nasıl oluşturulabildiğini gösterir:

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.

1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  

1. Laboratuvarın ana bölmesinde **sanal makinelerimi**seçin.
 
1. Sanal **makinelerim** bölmesinde, özel görüntüyü oluşturmak istediğiniz VM'yi seçin.

1. VM'nin yönetim bölmesine **özel görüntü oluştur (VHD)** seçeneğini belirleyin.

    ![Özel resim menüsü öğesi oluşturma](./media/devtest-lab-create-template/create-custom-image.png)

1. Özel **resim** bölmesine, özel resminiz için bir ad ve açıklama girin. Bu bilgiler, bir VM oluşturduğunuzda üsler listesinde görüntülenir. Özel görüntü işletim sistemi diski ve sanal makineye bağlı tüm veri diskleri içerir.

    ![Özel görüntü bölmesi oluşturma](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Sysprep'in VM'de çalıştırılıp çalıştırılmayacağını seçin. Sysprep VM'de çalıştırılmadıysa, özel görüntü oluşturulduğunda Sysprep'in VM'de çalıştırılıp çalıştırılmayacağını belirtin.

1. Özel görüntüyü oluşturmak için tamamlandığında **Tamam'ı** seçin.

Birkaç dakika sonra, özel görüntü oluşturulur ve laboratuvarın depolama hesabında depolanır. Bir laboratuvar kullanıcısı yeni bir VM oluşturmak istediğinde, görüntü temel görüntüler listesinde kullanılabilir.

![Temel görüntüler listesinde bulunan özel resim](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında Özel Görüntüler Kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınıza VM ekleme](devtest-lab-add-vm.md)
