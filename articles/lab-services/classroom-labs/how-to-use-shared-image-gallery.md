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
ms.date: 05/15/2020
ms.author: spelluru
ms.openlocfilehash: 8253b784a427ceb3842b6b4d585c52a2c30e0c4d
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83592432"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Lab Services paylaşılan görüntü galerisini kullanma
Bu makalede, eğitimciler/Lab yöneticilerinin bir şablon sanal makine görüntüsünü [paylaşılan bir görüntü galerisine](../../virtual-machines/windows/shared-image-galleries.md) nasıl kaydedebilmesi, böylece diğerleri tarafından laboratuvarları oluşturmak için kullanılabilir. 

## <a name="scenarios"></a>Senaryolar
Bu özellik tarafından desteklenen birkaç senaryo şunlardır: 

- Laboratuvar hesabı Yöneticisi, laboratuvar hesabına paylaşılan bir görüntü Galerisi ekler ve bir görüntüyü bir laboratuvar bağlamı dışında paylaşılan görüntü galerisine yükler. Daha sonra laboratuvar oluşturucuları, Labs oluşturmak için paylaşılan görüntü galerisinden bu görüntüyü kullanabilir. 
- Laboratuvar hesabı Yöneticisi, laboratuvar hesabına paylaşılan bir görüntü Galerisi ekler. Laboratuvar Oluşturucu (eğitmen), kendi laboratuvarının özelleştirilmiş görüntüsünü paylaşılan görüntü galerisine kaydeder. Daha sonra, diğer laboratuvar oluşturucuları, Labs için bir şablon oluşturmak üzere paylaşılan görüntü galerisinden bu görüntüyü seçebilir. 

    Bir görüntü paylaşılan bir görüntü galerisine kaydedildiğinde, Azure Lab Services kaydedilen görüntüyü aynı [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/)'da bulunan diğer bölgelere çoğaltır. Görüntünün aynı coğrafya içindeki diğer bölgelerde oluşturulan laboratuvarlarda kullanılabilir olmasını sağlar. Resimleri paylaşılan bir görüntü galerisine kaydetmek, çoğaltılan tüm görüntülerin maliyetini içeren ek bir maliyet doğurur. Bu maliyet Azure Lab Services kullanım maliyetinden ayrıdır. Paylaşılan görüntü Galerisi fiyatlandırması hakkında daha fazla bilgi için bkz. [paylaşılan görüntü Galerisi – faturalandırma]( https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#billing).

    > [!NOTE]
    > Azure Lab Services, paylaşılan görüntü galerisinde hem **Genelleştirilmiş** hem de **özelleştirilmiş** görüntüleri temel alan şablon VM 'lerinin oluşturulmasını destekler. 


## <a name="prerequisites"></a>Önkoşullar
- [Azure PowerShell](../../virtual-machines/windows/shared-images.md) veya [Azure CLI](../../virtual-machines/linux/shared-images.md)kullanarak paylaşılan bir görüntü galerisi oluşturun.
- Paylaşılan görüntü galerisini laboratuvar hesabına eklemiş olabilirsiniz. Adım adım yönergeler için bkz. [paylaşılan görüntü Galerisi 'ni bağlama veya ayırma](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü galerisine görüntü kaydetme
Paylaşılan görüntü Galerisi eklendikten sonra, bir laboratuar hesabı Yöneticisi veya bir eğitimci, bir görüntüyü paylaşılan görüntü galerisine kaydedebilir, böylece diğer eğitimciler tarafından yeniden kullanılabilir. 

1. Laboratuvarın **şablon** sayfasında, araç çubuğunda **paylaşılan görüntü galerisine aktar** ' ı seçin.

    ![Görüntüyü Kaydet düğmesi](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. **Paylaşılan görüntü galerisine aktar** iletişim kutusunda, **görüntü için bir ad**girin ve **dışarı aktar**' ı seçin. 

    ![Paylaşılan görüntü galerisine Aktar iletişim kutusu](../media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Bu işlemin ilerleme durumunu **şablon** sayfasında görebilirsiniz. Bu işlem biraz zaman alabilir. 

    ![Dışarı aktarma devam ediyor](../media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. Dışarı aktarma işlemi başarılı olduğunda aşağıdaki iletiyi görürsünüz:

    ![Dışarı aktarma tamamlandı](../media/how-to-use-shared-image-gallery/exporting-image-completed.png)

    > [!NOTE]
    > Görüntüyü paylaşılan görüntü galerisine kaydettikten sonra, başka bir laboratuvar oluştururken bu görüntüyü Galeriden kullanabilirsiniz. 
    > 
    > Ayrıca, bir laboratuvar bağlamı dışında paylaşılan görüntü galerisine bir görüntü yükleyebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan görüntü galerisine genel bakış](../../virtual-machines/windows/shared-images.md). 

    

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanma
Bir eğitimci, yeni laboratuvar oluşturma sırasında şablon için paylaşılan görüntü galerisinde bulunan özel bir görüntü seçebilir.

![Galerideki sanal makine görüntüsünü kullan](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md).
