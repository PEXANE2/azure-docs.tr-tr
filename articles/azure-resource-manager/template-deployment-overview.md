---
title: Azure Resource Manager şablonları
description: Kaynakların dağıtımı için Azure Resource Manager şablonlarının nasıl kullanılacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: tomfitz
ms.openlocfilehash: 95c127b3a7c9c47c96b292066bf1597a02896806
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70166530"
---
# <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Azure Resource Manager, Azure 'a dağıtmak istediğinizi tanımlayan şablonlar oluşturabilirsiniz. Şablon, Azure çözümünüzün altyapısını ve yapılandırmasını içeren bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Bir şablon kullanarak çözümünü yaşam döngüsü boyunca defalarca dağıtabilir ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olabilirsiniz.

Şablon bildirim temelli sözdizimini kullanır, bu, oluşturmak için programlama komutlarının dizisini yazmak zorunda kalmadan ne dağıtmayı istediğinizi belirtmenize olanak tanır. Şablonda, dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

## <a name="benefits-of-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarının avantajları

Kaynak Yöneticisi şablonlar çeşitli avantajlar sağlar:

* Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

* Çözümünüzü geliştirme yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

* Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

* Doğru sırayla dağıtılmalarını sağlamak için kaynaklarınız arasındaki bağımlılıkları tanımlayabilirsiniz.

Çözümleriniz üzerinde çalışırken aşağıdaki önerilerden yararlanarak Resource Manager’dan tam anlamıyla yararlanabilirsiniz.

* Altyapınızı kesinlik temelli komutlar yerine Resource Manager şablonlarındaki bildirim temelli söz dizimini kullanarak tanımlayabilir ve dağıtabilirsiniz.

* Şablondaki tüm dağıtım ve yapılandırma adımlarını tanımlayın. Çözümünüzü ayarlarken el ile yapılan hiçbir adım olmaması gerekir.

* Kaynaklarınızı yönetirken, örneğin bir uygulamayı veya makineyi başlatırken ya da durdururken, kesinlik temelli komutlar çalıştırın.

* [Azure Resource Manager şablonu en iyi uygulamalarını](template-best-practices.md)izleyin.

## <a name="template-deployment-process"></a>Şablon dağıtımı işlemi

Bir şablonu dağıttığınızda Kaynak Yöneticisi şablonu ayrıştırır ve sözdizimini REST API işlemlerine dönüştürür. Bu işlemleri uygun kaynak sağlayıcılarına gönderir. Örneğin, Resource Manager aşağıdaki kaynak tanımına sahip bir şablonu aldığında:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Tanımı aşağıdaki REST API işlemine dönüştürerek Microsoft.Storage kaynak sağlayıcısına gönderir:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage"
}
```

## <a name="template-structure"></a>Şablon yapısı

Şablonları ve kaynak gruplarını tanımlama şekli tamamen size ve çözümünüzü yönetme biçiminize bağlıdır. Örneğin, üç katmanlı uygulamanızı tek bir şablondan tek bir kaynak grubuna dağıtabilirsiniz.

![üç katmanlı şablon](./media/resource-group-overview/3-tier-template.png)

Ancak, bütün altyapınızı tek bir şablonda tanımlamak zorunda değilsiniz. Genellikle, en uygun seçenek dağıtım gereksinimlerinizi hedeflenen, amaca yönelik bir dizi şablona bölüştürmektir. Bu şablonları farklı çözümler için kolayca yeniden kullanabilirsiniz. Belirli bir çözümü dağıtmak için gerekli tüm şablonların bağlandığı bir ana şablon oluşturun. Aşağıdaki görüntüde üç katmanlı çözümün iç içe üç şablon içeren ana şablon aracılığıyla nasıl dağıtıldığı gösterilmektedir.

![iç içe geçmiş şablon](./media/resource-group-overview/nested-tiers-template.png)

Katmanlarınızın farklı yaşam döngülerine sahip olacağını düşünüyorsanız, üç katmanı farklı kaynak gruplarına dağıtabilirsiniz. Kaynaklar diğer kaynak gruplarındaki kaynaklara bağlanmaya devam edebilir.

![katman şablonu](./media/resource-group-overview/tier-templates.png)

İç içe geçmiş şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).

Azure Resource Manager kaynakların doğru sırada oluşturulmasını sağlamak için bağımlılıkları analiz eder. Bir kaynak başka bir kaynaktaki değere bağımlıysa (diskler için depolama hesabına ihtiyaç duyan bir sanal makine gibi) bir bağımlılık ayarlayın. Daha fazla bilgi için bkz. [Azure Resource Manager’da bağımlılıkları tanımlama](resource-group-define-dependencies.md).

Ayrıca, şablonu altyapıda yapılan güncelleştirmeler için kullanabilirsiniz. Örneğin, çözümünüze bir kaynak ekleyebilir ve zaten dağıtılmış kaynaklar için yapılandırma kuralları ekleyebilirsiniz. Şablon zaten var olan bir kaynağı tanımlıyorsa, Kaynak Yöneticisi yeni bir kaynak oluşturmak yerine var olan kaynağı güncelleştirir.

Resource Manager, kurulumun içermediği belirli bir yazılımı yükleme gibi ek işlemlere ihtiyaç duymanız halinde size uzantılar sunar. Zaten DSC, Chef veya Puppet gibi bir yapılandırma yönetim hizmeti kullanıyorsanız, uzantıları kullanarak bu hizmetle çalışmaya devam edebilirsiniz. Sanal makine uzantıları hakkında daha fazla bilgi için bkz. [Sanal makine uzantıları ve özellikleri hakkında](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Portaldan bir çözüm oluşturduğunuzda çözüm otomatik olarak bir dağıtım şablonu içerir. Bir şablonla başlayacağınız ve bu şablonu size özel ihtiyaçlara göre özelleştirebileceğiniz için yeni bir şablon oluşturmanız gerekmez. Bir örnek için bkz [. hızlı başlangıç: Azure portal](./resource-manager-quickstart-create-templates-use-the-portal.md)kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın. Ayrıca kaynak grubunun mevcut durumunu dışarı aktararak veya belirli bir dağıtım için kullanılan şablonu görüntüleyerek mevcut kaynak grubu için bir şablon elde edebilirsiniz. [Dışarı aktarılan şablonu](./manage-resource-groups-portal.md#export-resource-groups-to-templates) görüntülemek şablon söz dizimi hakkında bilgi edinmek için yararlı bir yoldur.

Son olarak, uygulamanızın kaynak kodunun bir parçası haline gelir. Bunu kaynak kod deponuza dahil edebilir ve uygulamanız geliştikçe buna göre güncelleştirebilirsiniz. Visual Studio üzerinden şablonu düzenleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Şablon dosyaları hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).

Şablonunuzu tanımladıktan sonra kaynakları Azure'a dağıtmak için hazır olursunuz. Kaynakları dağıtmak için bkz.:

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](resource-group-template-deploy-cli.md)
* [Kaynakları Resource Manager şablonları ve Azure portalı ile dağıtma](resource-group-template-deploy-portal.md)
* [Kaynakları Resource Manager şablonları ve Resource Manager REST API’si ile dağıtma](resource-group-template-deploy-rest.md)

