---
title: Azure DevTest Labs'da paylaşılan bir resim galerisini yapılandırma | Microsoft Dokümanlar
description: Azure DevTest Labs'da paylaşılan bir resim galerisini nasıl yapılandırabilirsiniz öğrenin
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2019
ms.author: spelluru
ms.openlocfilehash: 9593d60f76802cd515ca85616bce028cf3aa0d49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77589326"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Azure DevTest Labs’de paylaşılan resim galerisi yapılandırma
DevTest Labs artık [Paylaşılan Resim Galerisi](../virtual-machines/windows/shared-image-galleries.md) özelliğini destekliyor. Laboratuvar kullanıcılarının laboratuvar kaynakları oluştururken paylaşılan bir konumdan görüntülere erişmesini sağlar. Ayrıca, özel olarak yönetilen VM görüntülerinizin etrafında yapı ve kuruluş oluşturmanıza da yardımcı olur. Paylaşılan Resim Galerisi özelliği destekler:

- Görüntülerin yönetilen küresel çoğaltma
- Daha kolay yönetim için görüntülerin sürümü ve gruplanması
- Kullanılabilirlik bölgelerini destekleyen bölgelerdeki Bölge Yedekli Depolama (ZRS) hesaplarıyla görüntülerinizi yüksek oranda kullanılabilir hale getirin. ZRS, zonal arızalara karşı daha iyi esneklik sağlar.
- Rol tabanlı erişim denetimini (RBAC) kullanarak abonelikler arasında ve hatta kiracılar arasında paylaşım.

Daha fazla bilgi için [Paylaşılan Resim Galerisi belgelerine](../virtual-machines/windows/shared-image-galleries.md)bakın. 
 
Korumanız gereken çok sayıda yönetilen görseliniz varsa ve bunları şirketinizde kullanılabilir hale getirmek istiyorsanız, görüntülerinizi güncellemeyi ve paylaşmayı kolaylaştıran bir depo olarak paylaşılan bir resim galerisikkullanabilirsiniz. Laboratuvar sahibi olarak, varolan paylaşılan bir resim galerisini laboratuvarınıza ekleyebilirsiniz. Bu galeri bağlandıktan sonra, laboratuvar kullanıcıları bu son resimlerden makineler oluşturabilir. Bu özelliğin önemli bir avantajı, DevTest Labs'ın artık görüntüleri laboratuvarlar arasında, abonelikler arasında ve bölgeler arasında paylaşma avantajından yararlanabilmesidir. 

> [!NOTE]
> Paylaşılan Resim Galerisi hizmetiyle ilişkili maliyetler hakkında bilgi edinmek [için Paylaşılan Resim Galerisi için Faturalandırma'ya](../virtual-machines/windows/shared-image-galleries.md#billing)bakın.

## <a name="considerations"></a>Dikkat edilmesi gerekenler
- Aynı anda bir laboratuvara yalnızca bir paylaşılan resim galerisi ekleyebilirsiniz. Başka bir galeri eklemek isterseniz, varolan bir galeriyi ayırmanız ve başka bir galeri eklemeniz gerekir. 
- DevTest Labs şu anda yalnızca paylaşılan görüntü galerisi genelleştirilmiş görüntüleri destekler.
- DevTest Labs şu anda laboratuvar aracılığıyla galeriye resim yüklemeyi desteklememektedir. 
- DevTest Labs, paylaşılan bir resim galerisi görüntüsünü kullanarak sanal bir makine oluştururken her zaman bu resmin en son yayınlanan sürümünü kullanır. Ancak, bir görüntünün birden çok sürümü varsa, kullanıcı sanal makine oluşturma sırasında Gelişmiş ayarlar sekmesine giderek önceki sürümden bir makine oluşturmayı seçebilir.  
- DevTest Labs, paylaşılan görüntü galerisinin görüntüleri Laboratuarın bulunduğu bölgeye çoğaltmasını sağlamak için otomatik olarak en iyi girişimi yapsa da, bu her zaman mümkün değildir. Kullanıcıların bu görüntülerden VM oluşturma sorunları olmasını önlemek için, görüntülerin zaten laboratuvarın bölgesine çoğaltıldığından emin olun."

## <a name="use-azure-portal"></a>Azure portalı kullanma
1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol navigasyon menüsünden **Tüm Hizmetler'i** seçin.
1. Listeden **DevTest Labs**'i seçin.
1. Laboratuvarlar listesinden, **laboratuvarınızı**seçin.
1. Sol menüdeki **Ayarlar** bölümünde **Yapılandırma ve ilkeler'i** seçin.
1. Sol menüdeki Sanal makine tabanları altında **Paylaşılan Resim Galerileri'ni** seçin. **Virtual machine bases**

    ![Paylaşılan Resim Galerileri menüsü](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. **Ekle** düğmesine tıklayarak ve açılır açılır daki galerinizi seçerek mevcut paylaşılan bir resim galerisini laboratuvarınıza ekleyin.

    ![İliştir](./media/configure-shared-image-gallery/attach-options.png)
1. Bağlı galeriye gidin ve VM oluşturma için paylaşılan görüntüleri **etkinleştirmek veya devre dışı bırakabilmek** için galerinizi yapılandırın. Yapılandırmak için listeden bir resim galerisi seçin. 

    Varsayılan olarak, **sanal makine tabanları** **Evet**olarak ayarlanmış olarak kullanılacak tüm görüntülerin izin ver. Bu, yeni bir laboratuvar VM oluştururken ekli paylaşılan resim galerisinde bulunan tüm görüntülerin bir laboratuvar kullanıcısı tarafından kullanılabileceği anlamına gelir. Belirli görüntülere erişimin kısıtlanması gerekiyorsa, tüm görüntülerin sanal makine tabanı **No**olarak Hayır **olarak kullanılmasına izin ver'i** değiştirin ve VM oluştururken izin vermek istediğiniz görüntüleri seçin ve ardından **Kaydet** düğmesini seçin.

    ![Etkinleştirme veya devre dışı](./media/configure-shared-image-gallery/enable-disable.png)
1. Laboratuvar kullanıcıları daha sonra **+Ekle'ye** tıklayıp **temel sayfanızdaki** resmi bularak etkin görüntüleri kullanarak sanal bir makine oluşturabilirler.

    ![Laboratuvar kullanıcıları](./media/configure-shared-image-gallery/lab-users.png)
## <a name="use-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanma

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Laboratuarınıza paylaşılan bir resim galerisi ekleme
Paylaşılan bir resim galerisini laboratuarınıza eklemek için bir Azure Kaynak Yöneticisi şablonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi, bu şablonu Kaynak Yöneticisi şablonunuzun kaynaklar bölümünün altına eklemeniz gerekir:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "mylab",
    "location": "eastus",
    "resources": [
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"/subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/mySharedGalleryRg/providers/Microsoft.Compute/galleries/mySharedGallery",
            "allowAllImages": "Enabled"
        }
    }
    ]
}
```

Tam bir Kaynak Yöneticisi şablonu örneği için, herkese açık GitHub depomuzda bu Kaynak Yöneticisi şablon örneklerine bakın: [Laboratuvar oluştururken paylaşılan bir resim galerisini yapılandırın.](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured)

## <a name="use-api"></a>API'yi kullanma

### <a name="shared-image-galleries---create-or-update"></a>Paylaşılan resim galerileri - oluşturma veya güncelleme

```rest
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}?api-version= 2018-10-15-preview
Body: 
{
    "properties":{
        "galleryId": "[Shared Image Gallery resource Id]",
        "allowAllImages": "Enabled"
    }
}

```

### <a name="shared-image-galleries-images---list"></a>Paylaşılan Resim Galerileri Resimleri - Liste 

```rest
GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
```




## <a name="next-steps"></a>Sonraki adımlar
Ekli paylaşılan resim galerisinden bir görüntü kullanarak VM oluşturma yla ilgili aşağıdaki makalelere bakın: [Galeriden paylaşılan bir görüntüyü kullanarak VM oluşturun](add-vm-use-shared-image.md)
