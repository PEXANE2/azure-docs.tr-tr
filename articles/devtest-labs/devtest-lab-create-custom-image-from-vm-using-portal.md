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
ms.date: 06/12/2020
ms.author: spelluru
ms.openlocfilehash: 3c11c8a4076f868e3f8896dac80147a485f166d3
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896238"
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

1. VM 'nin Yönetim bölmesinde, **işlemler**altında **özel görüntü oluştur** ' u seçin.

    :::image type="content" source="./media/devtest-lab-create-template/create-custom-image.png" alt-text="Özel görüntü menü öğesi oluştur":::
1. **Özel görüntü** bölmesinde, özel görüntünüz için bir ad ve açıklama girin. Bu bilgiler, bir VM oluşturduğunuzda temellerin listesinde görüntülenir. Özel görüntü, işletim sistemi diskini ve sanal makineye bağlı tüm veri disklerini içerir.

    :::image type="content" source="./media/devtest-lab-create-template/create-custom-image-blade.png" alt-text="Özel görüntü oluştur sayfası":::
1. Sysprep 'in VM 'de çalıştırılıp çalıştırılmadığını seçin. Sysprep sanal makinede çalıştırılmadıysa, özel görüntü oluşturulduğunda Sysprep 'in VM 'de çalıştırılmasını isteyip istemediğinizi belirtin.
1. Özel görüntüyü oluşturmak için bittiğinde **Tamam ' ı** seçin.

    Birkaç dakika sonra, özel görüntü oluşturulur ve laboratuvarın depolama hesabı içinde depolanır. Laboratuvar kullanıcısı yeni bir VM oluşturmak istediğinde, görüntü temel görüntüler listesinde kullanılabilir.

    :::image type="content" source="./media/devtest-lab-create-template/custom-image-available-as-base.png" alt-text="temel görüntüler listesinde kullanılabilen özel görüntü":::

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller mi?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızda VM ekleme](devtest-lab-add-vm.md)
