---
title: Portalı kullanarak Linux için paylaşılan Azure sanal makine görüntülerini oluşturma | Microsoft Docs
description: Azure portal kullanarak sanal makine görüntülerini oluşturma ve paylaşma hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/27/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: 8be4890f01ae2c0d893bb7c45f29c6f8178844f9
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70082124"
---
# <a name="create-a-shared-image-gallery-using-the-azure-portal"></a>Azure portal kullanarak paylaşılan bir görüntü galerisi oluşturun

[Paylaşılan görüntü Galerisi](shared-image-galleries.md) , kuruluşunuz genelinde özel görüntü paylaşımını basitleştirir. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamalar, uygulama yapılandırması ve diğer işletim sistemi yapılandırmalarının dağıtım görevlerinin önyüklemesi için kullanılabilir. 

Paylaşılan görüntü Galerisi, özel VM görüntülerinizi kuruluşunuzdaki diğer kişilerle, bir AAD kiracısı içinde veya bölgeler arasında paylaşmanızı sağlar. Hangi görüntüleri paylaşmak istediğinizi, içinde hangi bölgelerin kullanılabilir olmasını istediğinizi ve bunları ile paylaşmak istediğinizi seçin. Paylaşılan görüntüleri mantıksal olarak gruplandırabilmeniz için birden çok Galeri oluşturabilirsiniz. 

Galeri, tam rol tabanlı erişim denetimi (RBAC) sağlayan en üst düzey bir kaynaktır. Görüntülerin sürümü oluşturulabilir ve her görüntü sürümünü farklı bir Azure bölgesi kümesine çoğaltmayı tercih edebilirsiniz. Galeri yalnızca yönetilen görüntülerle birlikte kullanılabilir.

Paylaşılan görüntü Galerisi özelliğinin birden çok kaynak türü vardır. Bunları bu makalede kullanacağız veya oluşturacağız:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen görüntü** | Bu, tek başına kullanılabilen veya bir görüntü galerisinde **görüntü sürümü** oluşturmak için kullanılan temel bir görüntüdür. Yönetilen görüntüler Genelleştirilmiş VM 'lerden oluşturulur. Yönetilen görüntü, birden çok VM oluşturmak için kullanılabilen ve artık paylaşılan görüntü sürümleri oluşturmak için kullanılabilen özel bir VHD türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** , görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimlerin erişimi olduğunu kontrol edersiniz. |
| **Görüntü tanımı** | Görüntüler, bir galeri içinde tanımlanır ve bu görüntüyü dahili olarak kullanmaya yönelik gereksinimler hakkında bilgi taşır. Bu, görüntünün Windows veya Linux, sürüm notları ve en düşük ve en yüksek bellek gereksinimleri olduğunu içerir. Bu, bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** , galerı kullanılırken VM oluşturmak için kullandığınız şeydir. Ortamınız için gerektiğinde bir görüntünün birden fazla sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, bir sanal makine oluşturmak için bir **görüntü sürümü** kullandığınızda, sanal makine için yeni diskler oluşturmak üzere görüntü sürümü kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |


## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki örneği tamamlayabilmeniz için, mevcut bir yönetilen görüntünüz olmalıdır. Öğreticiyi izleyebilirsiniz [: Gerekirse bir Azure VM 'nin, Azure PowerShell](tutorial-custom-images.md) oluşturmak için özel bir görüntüsünü oluşturun. Yönetilen görüntü bir veri diski içeriyorsa, veri diski boyutu 1 TB 'den fazla olamaz.

Bu makalede çalışırken, kaynak grubu ve VM adlarını gerektiği yerde değiştirin.

 
[!INCLUDE [virtual-machines-common-shared-images-portal](../../../includes/virtual-machines-common-shared-images-portal.md)]

## <a name="create-vms-from-an-image"></a>Görüntüden VM oluşturma

Görüntü sürümü tamamlandıktan sonra bir veya daha fazla yeni VM oluşturabilirsiniz. 

> [!IMPORTANT]
> Başka bir Azure kiracısındaki görüntüden bir VM dağıtmak için portalını kullanamazsınız. Kiracılar arasında paylaşılan bir görüntüden VM oluşturmak için [Azure CLI](shared-images.md#create-a-vm) veya [PowerShell](../windows/shared-images.md#create-vms-from-an-image)kullanmanız gerekir.


Bu örnek, *Doğu ABD* veri merkezinde *Myresourcegroup* Içinde *myvmfromımage*adlı bir VM oluşturur.

1. Görüntü sürümünüzün sayfasında, sayfanın üst kısmındaki menüden **VM oluştur** ' u seçin.
1. **Kaynak grubu**Için **Yeni oluştur** ' u seçin ve ad için *myresourcegroup* yazın.
1. **Sanal makine adı**alanına *myvm*yazın.
1. **Bölge**için *Doğu ABD*' yi seçin.
1. **Kullanılabilirlik seçenekleri**için, varsayılan *altyapı yedekliliği gerekmez*' ı gerekli olarak bırakın.
1. Görüntü sürümü için sayfadan başladıysanız **görüntünün** değeri otomatik olarak doldurulmalıdır.
1. **Boyut**için kullanılabilir boyutlar LISTESINDEN bir VM boyutu seçin ve ardından "Seç" e tıklayın.
1. **Yönetici hesabı**altında **parola** veya **SSH ortak anahtarı**' nı seçin ve bilgilerinizi girin.
1. VM 'ye uzaktan erişime izin vermek istiyorsanız, **ortak gelen bağlantı noktaları**altında **Seçili bağlantı noktalarına izin ver** ' i seçin ve ardından açılır listeden **SSH (22)** öğesini seçin. VM 'ye uzaktan erişime izin vermek istemiyorsanız, **Genel gelen bağlantı noktaları**için **hiçbirini** seçilmemiş bırakın.
1. İşiniz bittiğinde sayfanın altındaki **gözden geçir + oluştur** düğmesini seçin.
1. VM doğrulamayı geçtikten sonra, dağıtımı başlatmak için sayfanın alt kısmındaki **Oluştur** ' u seçin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için sanal makinenin kaynak grubunu ve **Sil**’i seçip silinecek kaynak grubunun adını onaylayın.

Tek tek kaynakları silmek istiyorsanız, bunları ters sırada silmeniz gerekir. Örneğin, bir görüntü tanımını silmek için, bu görüntüden oluşturulan tüm görüntü sürümlerini silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, şablonlar kullanarak paylaşılan görüntü Galerisi kaynağı da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan görüntü galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan görüntü galerisinde görüntü tanımı oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Görüntü sürümünden VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [genel bakış](shared-image-galleries.md). Sorunlarla karşılaşırsanız bkz. [paylaşılan görüntü galerilerine sorun giderme](troubleshooting-shared-images.md).

