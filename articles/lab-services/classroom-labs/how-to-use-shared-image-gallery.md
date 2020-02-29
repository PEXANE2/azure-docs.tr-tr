---
title: Azure Lab Services paylaşılan görüntü galerisini kullanma | Microsoft Docs
description: Bir laboratuvar hesabını paylaşılan görüntü Galerisi 'ni kullanmak üzere yapılandırmayı öğrenin, böylece Kullanıcı bir görüntüyü başkalarıyla paylaşabilir ve başka bir Kullanıcı, laboratuvarda bir şablon VM oluşturmak için görüntüyü kullanabilir.
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
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78190458"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Lab Services paylaşılan görüntü galerisini kullanma
Bu makalede, öğretmenler/laboratuvar yöneticisinin başkaları tarafından yeniden kullanılabilmesi için bir şablon sanal makine görüntüsünü nasıl kaydedecağı gösterilmektedir. Bu görüntüler bir Azure [paylaşılan görüntü galerisine](../../virtual-machines/windows/shared-image-galleries.md)kaydedilir. İlk adım olarak, Laboratuvar Yöneticisi laboratuvar hesabına var olan bir paylaşılan görüntü Galerisi ekler. Paylaşılan görüntü Galerisi eklendikten sonra, laboratuvar hesabında oluşturulan laboratuvarlar paylaşılan görüntü galerisine görüntü kaydedebilir. Diğer öğretmenler bu görüntüyü, sınıfları için bir şablon oluşturmak üzere paylaşılan görüntü galerisinden seçebilir. 

> [!NOTE]
> Şu anda, Azure Lab Services paylaşılan görüntü galerisinde yalnızca **Genelleştirilmiş** VM görüntülerine (özelleştirilmiş görüntüler değil) bağlı olarak şablon VM 'lerinin oluşturulmasını destekler. 

## <a name="prerequisites"></a>Önkoşullar
- [Azure PowerShell](../../virtual-machines/windows/shared-images.md) veya [Azure CLI](../../virtual-machines/linux/shared-images.md)kullanarak paylaşılan bir görüntü galerisi oluşturun.
- Paylaşılan görüntü galerisini laboratuvar hesabına eklemiş olabilirsiniz. Adım adım yönergeler için bkz. [paylaşılan görüntü Galerisi 'ni bağlama veya ayırma](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü galerisine görüntü kaydetme
Paylaşılan görüntü Galerisi eklendikten sonra, bir laboratuvar hesabı Yöneticisi veya öğretmen, bir görüntüyü paylaşılan görüntü galerisine kaydedebilir, böylece diğer öğretmenler tarafından yeniden kullanılabilir. 

1. Laboratuvarın **şablon** sayfasında, araç çubuğunda **paylaşılan görüntü galerisine aktar** ' ı seçin.

    ![Görüntüyü Kaydet düğmesi](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. **Paylaşılan görüntü galerisine aktar** iletişim kutusunda, **görüntü için bir ad**girin ve **dışarı aktar**' ı seçin. 

    ![Paylaşılan görüntü galerisine Aktar iletişim kutusu](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Bu işlemin ilerleme durumunu **şablon** sayfasında görebilirsiniz. Bu işlem biraz zaman alabilir. 

    ![Dışarı aktarma devam ediyor](../media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. Dışarı aktarma işlemi başarılı olduğunda aşağıdaki iletiyi görürsünüz:

    ![Dışarı aktarma tamamlandı](../media/how-to-use-shared-image-gallery/exporting-image-completed.png)

Ayrıca, bir laboratuvar bağlamı dışında paylaşılan görüntü galerisine bir görüntü yükleyebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan görüntü galerisine genel bakış](../../virtual-machines/windows/shared-images.md). 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanma
Öğretmen/Mesleme, yeni laboratuvar oluşturma sırasında şablon için paylaşılan görüntü galerisinde bulunan özel bir görüntü seçebilir.

![Galerideki sanal makine görüntüsünü kullan](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md).
