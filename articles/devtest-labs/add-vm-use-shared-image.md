---
title: Azure DevTest Labs paylaşılan görüntü kullanarak VM ekleme | Microsoft Docs
description: Azure DevTest Labs eklenen paylaşılan görüntü galerisinden bir görüntü kullanarak sanal makine (VM) eklemeyi öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 9421a1e21be9446b0e59328bd9a3730b57655274
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85483865"
---
# <a name="add-a-vm-using-an-image-from-the-attached-shared-image-gallery"></a>Ekli paylaşılan görüntü galerisinden bir görüntü kullanarak VM ekleme
Azure DevTest Labs, laboratuvarınızda paylaşılan bir görüntü Galerisi eklemenize ve ardından galerideki görüntüleri laboratuvarda oluşturduğunuz sanal makinelerin temeli olarak kullanmanıza olanak tanır. Laboratuvarınızda paylaşılan görüntü Galerisi 'ni nasıl ekleyeceğinizi öğrenmek için bkz. [paylaşılan görüntü galerisini yapılandırma](configure-shared-image-gallery.md). Bu makalede, ekli paylaşılan görüntü galerisindeki bir görüntüyü temel olarak kullanarak laboratuvara nasıl bir VM ekleyebileceğiniz gösterilmektedir. 

## <a name="azure-portal"></a>Azure portal
Bu bölümde, ekli paylaşılan görüntü galerisindeki bir görüntüye göre laboratuvarınızı bir VM 'ye eklemek için Azure portal nasıl kullanacağınızı öğreneceksiniz. Bu bölüm, Azure portal kullanarak VM oluşturmaya yönelik ayrıntılı adım adım yönergeler sağlamaz. Bu ayrıntılar için bkz. [VM oluşturma-Azure Portal](devtest-lab-add-vm.md). Bu yalnızca, ekli paylaşılan görüntü galerisinden bir görüntü seçtiğiniz ve bir görüntünün kullanmak istediğiniz bir sürümünü seçebileceğiniz adımları vurgular. 

Laboratuvarınızda bir VM eklerken, ekli paylaşılan görüntü galerisinden temel görüntü olarak bir görüntü seçebilirsiniz: 

![Taban için paylaşılan bir görüntü seçin](./media/add-vm-use-shared-image/select-shared-image-for-base.png)

Ardından, **Laboratuvar kaynağı oluştur** sayfasının **Gelişmiş ayarlar** sekmesinde, temel görüntü olarak kullanmak istediğiniz görüntünün sürümünü seçebilirsiniz:

![Görüntü sürümü seçin](./media/add-vm-use-shared-image/select-version-shared-image.png)

VM oluşturulduktan sonra görüntünün farklı bir sürümünü kullanarak geçiş yapabilirsiniz. 

## <a name="resource-manager-template"></a>Resource Manager şablonu
Paylaşılan görüntü Galerisi görüntüsünü kullanarak bir sanal makine oluşturmak için Azure Resource Manager şablonu kullanıyorsanız, **Özellikler** bölümünde **parça kimliği** için bir değer belirtin. Aşağıdaki örneğe bakın: 

```json
"resources": [
{
    ...
    "properties": {
         "sharedImageId": "/subscriptions/111111111-1111-1111-1111-111111111111/resourcegroups/mydtlrg/providers/microsoft.devtestlab/labs/mydtllab/sharedgalleries/spsig/sharedimages/myimagefromgallery",
        "sharedImageVersion": "1.0.1",
        ...
    }
}
],
```

Kaynak Yöneticisi Şablon örneği için bkz. GitHub deponuzdaki [paylaşılan görüntü Galerisi görüntüsünü kullanarak sanal makine oluşturma](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage) . 

## <a name="rest-api"></a>REST API

1. İlk olarak, paylaşılan görüntü galerisinde görüntünün KIMLIĞINI almanız gerekir. Bir yol, ekli paylaşılan görüntü galerisindeki tüm görüntüleri aşağıdaki GET komutunu kullanarak listeetmenin bir yoludur. 

    ```rest
    GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
    ```
2. Önceki çağrısından aldığınız paylaşılan görüntünün KIMLIĞINI geçirerek sanal makinelerde PUT yöntemini çağırın `properties.SharedImageId` .

## <a name="next-steps"></a>Sonraki adımlar
Bir laboratuvara paylaşılan görüntü Galerisi ekleyip bunu yapılandırma hakkında bilgi edinmek için bkz. [paylaşılan görüntü galerisini yapılandırma](configure-shared-image-gallery.md).