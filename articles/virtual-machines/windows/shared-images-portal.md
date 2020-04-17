---
title: Portalı kullanarak Azure Paylaşılan Resim Galerisi oluşturma
description: Sanal makine görüntüleri oluşturmak ve paylaşmak için Azure portalLarını nasıl kullanacağınızı öğrenin.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/06/2019
ms.author: cynthn
ms.custom: ''
ms.openlocfilehash: 83cdae95d43884647e257cbf1808222a542a212e
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458105"
---
# <a name="create-an-azure-shared-image-gallery-using-the-portal"></a>Portalı kullanarak Azure Paylaşılan Resim Galerisi oluşturma

[Paylaşılan Resim Galerisi,](shared-image-galleries.md) kuruluşunuz genelinde özel görüntü paylaşımını kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, önceden yükleme uygulamaları, uygulama yapılandırmaları ve diğer işletim sistemi yapılandırmaları gibi dağıtım görevlerini önyükleme de kullanılabilir. 

Paylaşılan Resim Galerisi, özel VM görüntülerinizi kuruluşunuzdaki, bölgeler içinde veya diğer bölgeler arasında, bir AAD kiracısı içinde başkalarıyla paylaşmanıza olanak tanır. Hangi resimleri paylaşmak istediğinizi, bunları hangi bölgelerde kullanılabilir hale getirmek istediğinizi ve bunları kiminle paylaşmak istediğinizi seçin. Paylaşılan görüntüleri mantıksal olarak gruplandırmak için birden çok galeri oluşturabilirsiniz. 

Galeri, tam rol tabanlı erişim denetimi (RBAC) sağlayan üst düzey bir kaynaktır. Görüntüler sürülebilir ve her resim sürümünü farklı bir Azure bölgesi kümesinde çoğaltmayı seçebilirsiniz. Galeri yalnızca Yönetilen Görüntüler ile çalışır.

Paylaşılan Resim Galerisi özelliğinin birden çok kaynak türü vardır. Bu makalede bunları kullanacak veya inşa edeceğiz:

| Kaynak | Açıklama|
|----------|------------|
| **Yönetilen görüntü** | Tek başına kullanılabilen veya resim galerisinde **görüntü sürümü** oluşturmak için kullanılabilen temel bir resim. Yönetilen görüntüler [genelleştirilmiş](shared-image-galleries.md#generalized-and-specialized-images) VM'lerden oluşturulur. Yönetilen görüntü, birden çok VM yapmak için kullanılabilen ve şimdi paylaşılan görüntü sürümleri oluşturmak için kullanılabilen özel bir VHD türüdür. |
| **Anlık Görüntü** | Bir **görüntü sürümü**yapmak için kullanılabilecek bir VHD kopyası. Anlık görüntüler, özel leştirilmiş bir görüntü sürümü oluşturmak için tek başına veya veri disklerinin anlık görüntüleriyle kullanılan [özel](shared-image-galleries.md#generalized-and-specialized-images) bir VM'den (genelleştirilmemiş bir) alınabilir.
| **Resim galerisi** | Azure Marketi gibi, **resim galerisi** de görüntüleri yönetmek ve paylaşmak için bir depodur, ancak kimin erişimi olduğunu siz denetlersiniz. |
| **Resim tanımı** | Görüntüler bir galeri içinde tanımlanır ve görüntü ve kuruluşunuz içinde kullanmak için gereksinimleri hakkında bilgi taşır. Görüntünün genelleştirilmiş veya özelleştirilmiş olup olmadığı, işletim sistemi, minimum ve maksimum bellek gereksinimleri ve sürüm notları gibi bilgileri ekleyebilirsiniz. Bu bir görüntü türünün tanımıdır. |
| **Görüntü sürümü** | **Resim sürümü,** galeri kullanırken VM oluşturmak için kullandığınız sürümdür. Ortamınız için gerektiği gibi görüntünün birden çok sürümüne sahip olabilirsiniz. Yönetilen bir görüntü gibi, VM oluşturmak için bir **görüntü sürümü** kullandığınızda, görüntü sürümü VM için yeni diskler oluşturmak için kullanılır. Görüntü sürümleri birden çok kez kullanılabilir. |

<br>


> [!IMPORTANT]
> Özelleştirilmiş görüntüler şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
>
> **Bilinen önizleme sınırlamaları** VM'ler yalnızca portal veya API kullanılarak özel görüntülerden oluşturulabilir. Önizleme için CLI veya PowerShell desteği yoktur.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki örneği tamamlamak için, genelleştirilmiş bir VM'nin varolan yönetilen görüntüsüne veya özel leştirilmiş bir VM'nin anlık görüntüsüne sahip olmalısınız. Öğretici'yi takip edebilirsiniz: Yönetilen bir görüntü oluşturmak için [Azure PowerShell ile Azure VM'nin özel](tutorial-custom-images.md) bir görüntüsünü oluşturabilir veya özel bir VM için [anlık görüntü oluşturun.](snapshot-copy-managed-disk.md) Hem yönetilen görüntüler hem de anlık görüntüler için veri diskboyutu 1 TB'den fazla olamaz.

Bu makale üzerinde çalışırken, gerektiğinde kaynak grubunu ve VM adlarını değiştirin.


[!INCLUDE [virtual-machines-common-shared-images-portal](../../../includes/virtual-machines-common-shared-images-portal.md)]
 
## <a name="create-vms"></a>VM oluşturma

Artık bir veya daha fazla yeni VM oluşturabilirsiniz. Bu örnek, *Doğu ABD* veri merkezinde *myVM*adlı bir VM , *myResourceGroup*, oluşturur.

1. Resim tanımına gidin. Kullanılabilir tüm resim tanımlarını göstermek için kaynak filtresini kullanabilirsiniz.
1. Resim tanımınız için sayfada, sayfanın üst kısmındaki menüden **VM Oluştur'u** seçin.
1. **Kaynak grubu**için **yeni oluştur'u** seçin ve ad için *myResourceGroup* yazın'ı seçin.
1. **Sanal makine adına**, *myVM*yazın.
1. **Bölge**için *Doğu ABD'yi*seçin.
1. **Kullanılabilirlik seçenekleri**için, *hiçbir altyapı artıklığı gerekli*varsayılan bırakın.
1. Resim tanımı için sayfadan başladıysanız, **Resim** değeri otomatik olarak `latest` görüntü sürümüyle doldurulur.
1. **Boyut**için, kullanılabilir boyutlar listesinden bir VM boyutu seçin ve ardından **Seç'i**seçin.
1. **Yönetici hesabı**altında, görüntü genelleştirilmişse, *azureuser* ve parola gibi bir kullanıcı adı sağlamanız gerekir. Parola en az 12 karakter uzunluğunda olmalı ve [tanımlanmış karmaşıklık gereksinimlerini](faq.md#what-are-the-password-requirements-when-creating-a-vm) karşılamalıdır. Resminiz özelleştirilmişse, kaynak VM'nin kullanıcı adı ve şifresi kullanıldığından, kullanıcı adı ve parola alanları soluklanır.
1. **Genel gelen bağlantı noktaları**altında VM'ye uzaktan erişime izin vermek istiyorsanız, **seçili bağlantı noktalarına izin ver'i** seçin ve ardından açılır bağlantıdan **RDP'yi (3389)** seçin. VM'ye uzaktan erişime izin vermek istemiyorsanız, Ortak **gelen bağlantı noktaları**için **Hiçbiri'ı** seçili bırakın.
1. İşiniz bittiğinde, sayfanın altındaki **Gözden Geçir + oluştur** düğmesini seçin.
1. VM doğrulamayı geçtikten sonra, dağıtımı başlatmak için sayfanın en altında **Oluştur'u** seçin.


## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, sanal makineyi ve tüm ilişkili kaynakları silebilirsiniz. Bunu yapmak için, sanal makinenin kaynak grubunu ve **Sil**’i seçin, ardından silinecek kaynak grubunun adını onaylayın.

Tek tek kaynakları silmek istiyorsanız, bunları ters sırada silmeniz gerekir. Örneğin, bir resim tanımını silmek için, bu resimden oluşturulan tüm resim sürümlerini silmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Şablonları kullanarak Paylaşılan Resim Galerisi kaynağını da oluşturabilirsiniz. Kullanılabilir birkaç Azure Quickstart Şablonu vardır: 

- [Paylaşılan Resim Galerisi Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan Resim Galerisinde Resim Tanımı Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan Resim Galerisinde Resim Sürümü Oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resim Sürümünden VM Oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan Resim Galerileri hakkında daha fazla bilgi için [Genel Bakış'a](shared-image-galleries.md)bakın. Sorunlarla karşılaştıysanız, [paylaşılan görüntü galerilerini sorun giderme](troubleshooting-shared-images.md)konusuna bakın.

