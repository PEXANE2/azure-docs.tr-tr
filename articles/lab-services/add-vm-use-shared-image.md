---
title: Azure DevTest Labs'da paylaşılan bir görüntü kullanarak VM ekleme | Microsoft Dokümanlar
description: Azure DevTest Labs'daki ekli paylaşılan resim galerisinden bir resmi kullanarak sanal makine (VM) eklemeyi öğrenin
services: devtest-lab,virtual-machines
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
ms.date: 08/02/2019
ms.author: spelluru
ms.openlocfilehash: c13b2c3d00a0ab0af0f7785feae645d907ffdaa3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68775597"
---
# <a name="add-a-vm-using-an-image-from-the-attached-shared-image-gallery"></a>Ekli paylaşılan resim galerisinden bir görüntü kullanarak VM ekleme
Azure DevTest Labs, paylaşılan bir resim galerisini laboratuarınıza eklemenize ve galerideki görüntüleri laboratuvarda oluşturduğunuz VM'ler için üs olarak kullanmanıza olanak tanır. Paylaşılan bir resim galerisini laboratuarınıza nasıl ekleyeceklerinizi öğrenmek için [paylaşılan resim galerisini Yapıla'ya](configure-shared-image-gallery.md)bakın. Bu makalede, bağlı paylaşılan resim galerisinden bir görüntüyü temel olarak kullanarak laboratuvarınıza nasıl VM ekleyeceğiniz gösterilmektedir. 

## <a name="azure-portal"></a>Azure portalında
Bu bölümde, ekli paylaşılan resim galerisinden alınan bir resme dayanarak laboratuvarınıza VM eklemek için Azure portalını nasıl kullanacağınızı öğrenirsiniz. Bu bölümde, Azure portalını kullanarak bir VM oluşturmak için ayrıntılı adım adım yönergeler sağlamaz. Bu ayrıntılar için [vm - Azure portalı oluştur'a](devtest-lab-add-vm.md)bakın. Yalnızca ekli paylaşılan resim galerisinden bir resim seçtiğiniz ve kullanmak istediğiniz resmin bir sürümünü seçtiğiniz adımları vurgular. 

Laboratuvarınıza VM eklerken, temel resim olarak ekli paylaşılan resim galerisinden bir resim seçebilirsiniz: 

![Taban için paylaşılan bir resim seçin](./media/add-vm-use-shared-image/select-shared-image-for-base.png)

Ardından, **Laboratuvar kaynak** sayfasının **Gelişmiş ayarlar** sekmesinde, temel resim olarak kullanmak istediğiniz resmin sürümünü seçebilirsiniz:

![Resim sürümünü seçin](./media/add-vm-use-shared-image/select-version-shared-image.png)

VM oluşturulduktan sonra görüntünün farklı bir sürümünü kullanmaya geçebilirsiniz. 

## <a name="resource-manager-template"></a>Resource Manager şablonu
Paylaşılan resim galerisi görüntüsünü kullanarak sanal bir makine oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanıyorsanız, **Özellikler** bölümünde **paylaşılanImageId** için bir değer belirtin. Aşağıdaki örneğe bakın: 

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

Tam bir Kaynak Yöneticisi şablonu örneği için [bkz.](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage) 

## <a name="rest-api"></a>REST API

1. İlk olarak, paylaşılan resim galerisinde görüntünün kimliğini almanız gerekir. Bir yol aşağıdaki GET komutunu kullanarak ekli paylaşılan resim galerisindeki tüm görüntüleri listelemektir. 

    ```rest
    GET  https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DevTestLab/labs/{labName}/sharedgalleries/{name}/sharedimages?api-version= 2018-10-15-preview
    ```
2. Bir önceki çağrıdan aldığınız paylaşılan görüntünün kimliğini sanal makinelere geçirerek `properties.SharedImageId`PUT yöntemini çağırın.

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan bir resim galerisini bir laboratuvara nasıl ekleyip yapılandırışla nasıl yapılandırılabildiğini öğrenmek için [paylaşılan resim galerisini Yapılandır'a](configure-shared-image-gallery.md)bakın.