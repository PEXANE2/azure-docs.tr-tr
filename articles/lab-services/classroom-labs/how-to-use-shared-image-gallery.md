---
title: Azure Laboratuvar Hizmetleri'nde paylaşılan resim galerisi kullanma | Microsoft Dokümanlar
description: Bir kullanıcının görüntüyü başka bir resimle paylaşabilmesi ve başka bir kullanıcının laboratuvarda bir şablon VM oluşturmak için görüntüyü kullanabilmesi için paylaşılan bir resim galerisini kullanacak şekilde laboratuvar hesabını nasıl yapılandırabileceğinizi öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2020
ms.author: spelluru
ms.openlocfilehash: c611ecdb5a2534f7368e533e3e19e6e3f96de57f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190458"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Laboratuvar Hizmetleri'nde paylaşılan resim galerisi kullanma
Bu makalede, öğretmenlerin/laboratuvar yöneticisinin şablon sanal makine görüntüsünü başkaları tarafından yeniden kullanılabilleri için nasıl kaydedebileceği gösterilmektedir. Bu görüntüler Azure [paylaşılan resim galerisine](../../virtual-machines/windows/shared-image-galleries.md)kaydedilir. İlk adım olarak, laboratuvar yöneticisi varolan paylaşılan bir resim galerisini laboratuvar hesabına bağlar. Paylaşılan resim galerisi bağlandıktan sonra, laboratuvar hesabında oluşturulan laboratuvarlar görüntüleri paylaşılan resim galerisine kaydedebilir. Diğer öğretmenler, sınıfları için bir şablon oluşturmak için paylaşılan resim galerisinden bu görüntüyü seçebilir. 

> [!NOTE]
> Şu anda Azure Laboratuvar Hizmetleri, paylaşılan bir resim galerisinde yalnızca **genelleştirilmiş** VM görüntülerine (özel leştirilmiş resimler değil) dayalı şablon VM'lerin oluşturulmasını destekler. 

## <a name="prerequisites"></a>Ön koşullar
- [Azure PowerShell](../../virtual-machines/windows/shared-images.md) veya [Azure CLI'yi](../../virtual-machines/linux/shared-images.md)kullanarak paylaşılan bir resim galerisi oluşturun.
- Paylaşılan resim galerisini laboratuvar hesabına iliştirdin. Adım adım yönergeler [için, paylaşılan resim galerisini nasıl ekleyecek veya ayıracağınız](how-to-attach-detach-shared-image-gallery.md)bölümüne bakın.


## <a name="save-an-image-to-the-shared-image-gallery"></a>Görüntüyü paylaşılan resim galerisine kaydetme
Paylaşılan bir resim galerisi bağlandıktan sonra, bir laboratuvar hesabı yöneticisi veya öğretmen, diğer öğretmenler tarafından yeniden kullanılabilmek için görüntüyü paylaşılan resim galerisine kaydedebilir. 

1. Laboratuvarın **Şablon** sayfasında, araç çubuğundaki **Paylaşılan Resim Galerisine Dışa Aktar'ı** seçin.

    ![Görüntüyü kaydet düğmesi](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. Paylaşılan **Resim Galerisi'ne Dışa** Aktar iletişim kutusunda, **görüntü için**bir ad girin ve sonra **Dışa Aktar'ı**seçin. 

    ![Paylaşılan Resim Galerisi iletişim kutusuna dışa aktarma](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Bu işlemin ilerlemesini **Şablon** sayfasında görebilirsiniz. Bu işlem bazen sürebilir. 

    ![İhracat devam ediyor](../media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. Dışa aktarma işlemi başarılı olduğunda, aşağıdaki iletiyi görürsünüz:

    ![Dışa aktarma tamamlandı](../media/how-to-use-shared-image-gallery/exporting-image-completed.png)

Ayrıca, bir görüntüyü laboratuvar bağlamı dışında paylaşılan resim galerisine de yükleyebilirsiniz. Daha fazla bilgi için Bkz. [Paylaşılan resim galerisine genel bakış.](../../virtual-machines/windows/shared-images.md) 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan resim galerisinden bir resim kullanma
Bir öğretmen/profesör, yeni laboratuvar oluşturma sırasında şablon için paylaşılan resim galerisinde bulunan özel bir resim seçebilir.

![Galeriden sanal makine görüntüsü kullanma](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan resim galerileri hakkında daha fazla bilgi için [paylaşılan resim galerisine](../../virtual-machines/windows/shared-image-galleries.md)bakın.
