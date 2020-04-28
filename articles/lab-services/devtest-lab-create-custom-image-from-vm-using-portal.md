---
title: Bir VM 'den Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs
description: Azure portal kullanarak sağlanan bir VM 'den Azure DevTest Labs özel görüntü oluşturmayı öğrenin
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60868508"
---
# <a name="create-a-custom-image-from-a-vm"></a>VM’den özel görüntü oluşturma

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Sağlanan bir VM 'den özel bir görüntü oluşturabilir ve bu özel görüntüyü daha sonra aynı VM 'Ler oluşturmak için kullanabilirsiniz. Aşağıdaki adımlarda, bir VM 'den nasıl özel bir görüntü oluşturacağınız gösterilmektedir:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.

1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  

1. Laboratuvarın ana bölmesinde **sanal makinelerim**' ı seçin.
 
1. **Sanal makinelerim** bölmesinde, özel görüntüyü oluşturmak istediğiniz VM 'yi seçin.

1. VM 'nin Yönetim bölmesinde **özel görüntü (VHD) oluştur**' u seçin.

    ![Özel görüntü menü öğesi oluştur](./media/devtest-lab-create-template/create-custom-image.png)

1. **Özel görüntü** bölmesinde, özel görüntünüz için bir ad ve açıklama girin. Bu bilgiler, bir VM oluşturduğunuzda temellerin listesinde görüntülenir. Özel görüntü, işletim sistemi diskini ve sanal makineye bağlı tüm veri disklerini içerir.

    ![Özel görüntü bölmesi oluştur](./media/devtest-lab-create-template/create-custom-image-blade.png)

1. Sysprep 'in VM 'de çalıştırılıp çalıştırılmadığını seçin. Sysprep sanal makinede çalıştırılmadıysa, özel görüntü oluşturulduğunda Sysprep 'in VM 'de çalıştırılmasını isteyip istemediğinizi belirtin.

1. Özel görüntüyü oluşturmak için bittiğinde **Tamam ' ı** seçin.

Birkaç dakika sonra, özel görüntü oluşturulur ve laboratuvarın depolama hesabı içinde depolanır. Laboratuvar kullanıcısı yeni bir VM oluşturmak istediğinde, görüntü temel görüntüler listesinde kullanılabilir.

![Temel görüntüler listesinde kullanılabilen özel görüntü](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller mi?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızda VM ekleme](devtest-lab-add-vm.md)
