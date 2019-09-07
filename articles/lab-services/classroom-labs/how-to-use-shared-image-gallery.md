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
ms.date: 09/05/2019
ms.author: spelluru
ms.openlocfilehash: 65cc2d9ac2b96822f2c1b740f3180ba1d9eaf98c
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390010"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Lab Services paylaşılan görüntü galerisini kullanma
Bu makalede, öğretmenler/laboratuvar yöneticisinin başkaları tarafından yeniden kullanılabilmesi için bir şablon sanal makine görüntüsünü nasıl kaydedecağı gösterilmektedir. Bu görüntüler bir Azure [paylaşılan görüntü galerisine](../../virtual-machines/windows/shared-image-galleries.md)kaydedilir. İlk adım olarak, Laboratuvar Yöneticisi laboratuvar hesabına var olan bir paylaşılan görüntü Galerisi ekler. Paylaşılan görüntü Galerisi eklendikten sonra, laboratuvar hesabında oluşturulan laboratuvarlar paylaşılan görüntü galerisine görüntü kaydedebilir. Diğer öğretmenler bu görüntüyü, sınıfları için bir şablon oluşturmak üzere paylaşılan görüntü galerisinden seçebilir. 

## <a name="prerequisites"></a>Önkoşullar
- [Azure PowerShell](../../virtual-machines/windows/shared-images.md) veya [Azure CLI](../../virtual-machines/linux/shared-images.md)kullanarak paylaşılan bir görüntü galerisi oluşturun.
- Paylaşılan görüntü galerisini laboratuvar hesabına eklemiş olabilirsiniz. Adım adım yönergeler için bkz. [paylaşılan görüntü Galerisi 'ni bağlama veya ayırma](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü galerisine görüntü kaydetme
Paylaşılan görüntü Galerisi eklendikten sonra, bir laboratuvar hesabı Yöneticisi veya öğretmen, bir görüntüyü paylaşılan görüntü galerisine kaydedebilir, böylece diğer öğretmenler tarafından yeniden kullanılabilir. 

1. Laboratuvarın giriş sayfasında, **şablon** bölümündeki kutucukta **görüntüyü kaydet** ' i seçin.

    ![Görüntüyü Kaydet düğmesi](../media/how-to-use-shared-image-gallery/save-image-button.png)
2.  **Sanal makine görüntüsünü kaydet** penceresinde, görüntü için bir ad girin ve **Kaydet**' i seçin. 

    ![Sanal makine görüntüsünü Kaydet penceresi](../media/how-to-use-shared-image-gallery/save-virtual-machine-image.png)
3. Laboratuvar kutucuğunun durumunu kontrol edin. 

    ![Görüntü kaydetme işleminin durumu](../media/how-to-use-shared-image-gallery/save-image-status.png)
4. İşlemin başarılı olduğunu doğrulayın.

    ![Görüntü kaydetme işlemi başarılı](../media/how-to-use-shared-image-gallery/save-image-successful.png)

Ayrıca, bir laboratuvar bağlamı dışında paylaşılan görüntü galerisine bir görüntü yükleyebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan görüntü galerisine genel bakış](../../virtual-machines/windows/shared-images.md). 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanma
Öğretmen/Mesleme, yeni laboratuvar oluşturma sırasında şablon için paylaşılan görüntü galerisinde bulunan özel bir görüntü seçebilir.

![Galerideki sanal makine görüntüsünü kullan](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md).
