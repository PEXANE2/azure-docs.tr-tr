---
title: Bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma | Microsoft Dokümanlar
description: Azure portalını kullanarak bir VHD dosyasından Azure DevTest Labs'da nasıl özel bir görüntü oluşturabilirsiniz öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: b795bc61-7c28-40e6-82fc-96d629ee0568
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 853c138c8cf73b41b0cebb6c1d349865e18eab6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61296167"
---
# <a name="create-a-custom-image-from-a-vhd-file"></a>VHD dosyasından özel görüntü oluşturma

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, Azure portalını kullanarak bir VHD dosyasından özel bir görüntü oluşturmanıza yol açar:

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.

1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  

1. Laboratuvarın ana bölmesine Yapılandırma **ve ilkeler'i**seçin. 

1. Yapılandırma **ve ilkeler** bölmesine **özel görüntüler**seçin.

1. Özel **resimler** bölmesine **+Ekle'yi**seçin.

    ![Özel resim ekle](./media/devtest-lab-create-template/add-custom-image.png)

1. Özel görüntünün adını girin. Bu ad, VM oluşturulurken temel görüntüler listesinde görüntülenir.

1. Özel resmin açıklamasını girin. Bu açıklama, VM oluşturulurken temel görüntüler listesinde görüntülenir.

1. **İşletim sistemi türü için** **Windows** veya **Linux'u**seçin.

    - **Windows'u**seçerseniz, *sysprep'in* makinede çalıştırılıp çalıştırılmadığını onay kutusu aracılığıyla belirtin. 
    - **Linux'u**seçerseniz, onay kutusu aracılığıyla *makinede deprovision* çalıştırılıp çalıştırılmadığını belirtin. 

1. Açılan menüden bir **VHD** seçin. Bu, yeni özel görüntü oluşturmak için kullanılacak VHD'dir. Gerekirse **PowerShell kullanarak bir VHD yüklemeyi**seçin.

1. Ayrıca, özel görüntü oluşturmak için kullanılan resim lisanslı bir resim (Microsoft tarafından yayımlanmış) değilse, bir plan adı, plan teklifi ve plan yayımcı girebilirsiniz.

   - **Plan adı:** Bu özel görüntünün oluşturulduğu Market görüntüsünün (SKU) adını girin 
   - **Teklif planı:** Bu özel görüntünün oluşturulduğu Market görüntüsünün ürün (teklif) girin 
   - **Plan yayımcı:** Bu özel görüntünün oluşturulduğu Market görüntüsünün yayımcısını girin

   > [!NOTE]
   > Özel bir görüntü oluşturmak için kullandığınız resim lisanslı bir görüntü **değilse,** bu alanlar boştur ve isterseniz doldurulabilir. Görüntü lisanslı bir **görüntüyse,** alanlar plan bilgileriyle otomatik olarak doldurulur. Bu durumda bunları değiştirmeye çalışırsanız, bir uyarı iletisi görüntülenir.
   >
   >

1. Özel görüntüyü oluşturmak için **Tamam'ı** seçin.

Birkaç dakika sonra, özel görüntü oluşturulur ve laboratuvarın depolama hesabında depolanır. Bir laboratuvar kullanıcısı yeni bir VM oluşturmak istediğinde, görüntü temel görüntüler listesinde kullanılabilir.

![Temel görüntüler listesinde bulunan özel resim](./media/devtest-lab-create-template/custom-image-available-as-base.png)


[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntüler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında Özel Görüntüler Kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınıza VM ekleme](./devtest-lab-add-vm.md)
