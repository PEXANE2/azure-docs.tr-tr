---
title: Azure Lab Services paylaşılan görüntü galerisini kullanma | Microsoft Docs
description: Bir laboratuvar hesabını paylaşılan görüntü Galerisi 'ni kullanmak üzere yapılandırmayı öğrenin, böylece Kullanıcı bir görüntüyü başkalarıyla paylaşabilir ve başka bir Kullanıcı, laboratuvarda bir şablon VM oluşturmak için görüntüyü kullanabilir.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 4dc001a5e4c95db608c38f0a7c00190fd1859998
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85445160"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>Azure Lab Services paylaşılan görüntü galerisini kullanma
Bu makalede, eğitimciler/Lab yöneticilerinin bir şablon sanal makine görüntüsünü [paylaşılan bir görüntü galerisine](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries) nasıl kaydedebilmesi, böylece diğerleri tarafından laboratuvarları oluşturmak için kullanılabilir. 

## <a name="scenarios"></a>Senaryolar
Bu özellik tarafından desteklenen birkaç senaryo şunlardır: 

- Laboratuvar hesabı Yöneticisi, laboratuvar hesabına paylaşılan bir görüntü Galerisi ekler ve bir görüntüyü bir laboratuvar bağlamı dışında paylaşılan görüntü galerisine yükler. Daha sonra laboratuvar oluşturucuları, Labs oluşturmak için paylaşılan görüntü galerisinden bu görüntüyü kullanabilir. 
- Laboratuvar hesabı Yöneticisi, laboratuvar hesabına paylaşılan bir görüntü Galerisi ekler. Laboratuvar Oluşturucu (eğitmen), kendi laboratuvarının özelleştirilmiş görüntüsünü paylaşılan görüntü galerisine kaydeder. Daha sonra, diğer laboratuvar oluşturucuları, Labs için bir şablon oluşturmak üzere paylaşılan görüntü galerisinden bu görüntüyü seçebilir. 

    Bir görüntü paylaşılan bir görüntü galerisine kaydedildiğinde, Azure Lab Services kaydedilen görüntüyü aynı [Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/)'da bulunan diğer bölgelere çoğaltır. Görüntünün aynı coğrafya içindeki diğer bölgelerde oluşturulan laboratuvarlarda kullanılabilir olmasını sağlar. Resimleri paylaşılan bir görüntü galerisine kaydetmek, çoğaltılan tüm görüntülerin maliyetini içeren ek bir maliyet doğurur. Bu maliyet Azure Lab Services kullanım maliyetinden ayrıdır. Paylaşılan görüntü Galerisi fiyatlandırması hakkında daha fazla bilgi için bkz. [paylaşılan görüntü Galerisi – faturalandırma]( https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#billing).
    
## <a name="prerequisites"></a>Ön koşullar
- [Azure PowerShell](../virtual-machines/windows/shared-images.md) veya [Azure CLI](../virtual-machines/linux/shared-images.md)kullanarak paylaşılan bir görüntü galerisi oluşturun.
- Paylaşılan görüntü galerisini laboratuvar hesabına eklemiş olabilirsiniz. Adım adım yönergeler için bkz. [paylaşılan görüntü Galerisi 'ni bağlama veya ayırma](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü galerisine görüntü kaydetme
Paylaşılan görüntü Galerisi eklendikten sonra, bir laboratuar hesabı Yöneticisi veya bir eğitimci, bir görüntüyü paylaşılan görüntü galerisine kaydedebilir, böylece diğer eğitimciler tarafından yeniden kullanılabilir. 

1. Laboratuvarın **şablon** sayfasında, araç çubuğunda **paylaşılan görüntü galerisine aktar** ' ı seçin.

    ![Görüntüyü Kaydet düğmesi](./media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-button.png)
2. **Paylaşılan görüntü galerisine aktar** iletişim kutusunda, **görüntü için bir ad**girin ve **dışarı aktar**' ı seçin. 

    ![Paylaşılan görüntü galerisine Aktar iletişim kutusu](./media/how-to-use-shared-image-gallery/export-to-shared-image-gallery-dialog.png)
3. Bu işlemin ilerleme durumunu **şablon** sayfasında görebilirsiniz. Bu işlem biraz zaman alabilir. 

    ![Dışarı aktarma devam ediyor](./media/how-to-use-shared-image-gallery/exporting-image-in-progress.png)
4. Dışarı aktarma işlemi başarılı olduğunda aşağıdaki iletiyi görürsünüz:

    ![Dışarı aktarma tamamlandı](./media/how-to-use-shared-image-gallery/exporting-image-completed.png)

    Görüntüyü paylaşılan görüntü galerisine kaydettikten sonra, başka bir laboratuvar oluştururken bu görüntüyü Galeriden kullanabilirsiniz. Ayrıca, bir laboratuvar bağlamı dışında paylaşılan görüntü galerisine bir görüntü yükleyebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan görüntü galerisine genel bakış](../virtual-machines/windows/shared-images.md). 

    > [!IMPORTANT]
    > Bir [laboratuvarın şablon görüntüsünü](how-to-use-shared-image-gallery.md#save-an-image-to-the-shared-image-gallery) paylaşılan bir görüntü galerisine Azure Lab Services kaydettiğinizde, görüntü galeriye **özelleştirilmiş bir görüntü**olarak yüklenir. [Özel görüntüler](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries#generalized-and-specialized-images) makineye özgü bilgileri ve Kullanıcı profillerini saklar. Genelleştirilmiş bir görüntüyü yine de Azure Lab Services dışında galeriye doğrudan yükleyebilirsiniz.    

## <a name="use-an-image-from-the-shared-image-gallery"></a>Paylaşılan görüntü galerisinden bir görüntü kullanma
Bir eğitimci, yeni laboratuvar oluşturma sırasında şablon için paylaşılan görüntü galerisinde bulunan özel bir görüntü seçebilir.

![Galerideki sanal makine görüntüsünü kullan](./media/how-to-use-shared-image-gallery/use-shared-image.png)

> [!NOTE]
> Azure Lab Services içinde hem **Genelleştirilmiş** hem de **özelleştirilmiş** görüntüleri temel alan bir şablon VM oluşturabilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [paylaşılan görüntü Galerisi](https://docs.microsoft.com/azure/virtual-machines/windows/shared-image-galleries).
