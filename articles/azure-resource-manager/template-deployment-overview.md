---
title: Azure Resource Manager şablonları
description: Kaynakların dağıtımı için Azure Resource Manager şablonlarının nasıl kullanılacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 4f273a04322246a2b4112c0b5b8a062732bab555
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390749"
---
# <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Buluta geçiş sayesinde, birçok ekip çevik geliştirme yöntemlerini benimsemiştir. Bu takımlar hızla yineleyebilir. Çözümlerini buluta tekrar tekrar ve güvenilir bir şekilde dağıtmaları gerekir. Takımların altyapıyı ve kaynak kodu tek bir işlemde yönetmesi gerektiği için işlemler ve geliştirme arasındaki bölme kayboldu.

Bu güçlüklere yönelik bir çözüm, dağıtımı otomatikleştirecek ve kod olarak altyapı uygulamasını kullanmaktır. Hangi işlemin dağıtılması gerektiğini tanımlamak ve kodu Kaynak kodla aynı işlemle yönetmek için kodu kullanırsınız. Altyapıyı bir kaynak deposunda ve sürümüne bir kod olarak depoladığınızda.

Azure Resource Manager, altyapıyı Kaynak Yöneticisi şablonları aracılığıyla kod olarak uygulamanıza olanak sağlar. Şablon, Azure çözümünüz için altyapıyı ve yapılandırmayı içeren bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Şablon bildirim temelli sözdizimini kullanır, bu, oluşturmak için programlama komutlarının dizisini yazmak zorunda kalmadan ne dağıtmayı istediğinizi belirtmenize olanak tanır. Şablonda, dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

## <a name="benefits-of-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarının avantajları

Kaynak Yöneticisi şablonlar çeşitli avantajlar sağlar. Şunları yapabilirsiniz:

* Bu kaynakları tek tek işlemek yerine, çözümünüz için tüm kaynakları bir grup olarak dağıtın, yönetin ve izleyin.

* Çözümünüzü geliştirme yaşam döngüsü boyunca sürekli olarak dağıtın ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olmanız gerekir.

* Altyapınızı betikler yerine bildirim temelli şablonlar aracılığıyla yönetin.

* Doğru sırada dağıtılabilmesi için kaynaklar arasındaki bağımlılıkları tanımlayın.

* Diğer taraflar için gereken ara iş bulunmadığından, yeni sürümlerden ve hizmetlerden hemen yararlanın.

## <a name="template-file"></a>Şablon dosyası

Şablonunuzda, JSON özelliklerini genişleten [şablon ifadeleri](template-expressions.md) yazabilirsiniz. Bu ifadeler Kaynak Yöneticisi tarafından sunulan [işlevleri](resource-group-template-functions.md) kullanır.

Şablon aşağıdaki bölümlere sahiptir:

* [Parametreler](template-parameters.md) -dağıtım sırasında, aynı şablonun farklı ortamlarla kullanılmasına izin veren değerler sağlar.

* [Değişkenler](template-variables.md) -şablonlarınız için kullanılan değerleri tanımlayın.

* [Kullanıcı tanımlı işlevler](template-user-defined-functions.md) -şablonunuzu basitleştiren özelleştirilmiş işlevler oluşturun.

* [Kaynaklar](resource-group-authoring-templates.md#resources) -dağıtılacak kaynakları belirtin.

* [Çıktılar](template-outputs.md) -dağıtılan kaynaklardan değer döndürür.

## <a name="dependencies"></a>Bağımlılıkları

Azure Resource Manager kaynakların doğru sırada oluşturulmasını sağlamak için bağımlılıkları analiz eder. Bir kaynak başka bir kaynaktaki değere bağımlıysa (diskler için depolama hesabına ihtiyaç duyan bir sanal makine gibi) bir bağımlılık ayarlayın. Daha fazla bilgi için bkz. [Azure Resource Manager’da bağımlılıkları tanımlama](resource-group-define-dependencies.md).

## <a name="conditional-deployment"></a>Koşullu dağıtım

Şablonunuza bir kaynak ekleyebilir ve isteğe bağlı olarak dağıtabilirsiniz. Genellikle, kaynağın dağıtılması gerekip gerekmediğini belirten bir parametre değeri geçirin. Daha fazla bilgi için [Kaynak Yöneticisi şablonlarda koşullu dağıtım](conditional-resource-deployment.md)konusuna bakın.

## <a name="create-multiple-instances"></a>Birden çok örnek oluşturma

Şablonunuzda çok sayıda JSON blok blokları yerine, bir kopya öğesi kullanarak bir değişken, özellik veya kaynağın birden fazla örneğini belirtebilirsiniz. Daha fazla bilgi için [Azure Resource Manager şablonlarda kaynak, özellik veya değişken yinelemesi](resource-group-create-multiple.md)bölümüne bakın.

## <a name="export-templates"></a>Şablonları dışarı aktarma

Kaynak grubunun geçerli durumunu dışarı aktararak veya belirli bir dağıtım için kullanılan şablonu görüntüleyerek, var olan bir kaynak grubu için bir şablon alabilirsiniz. [Dışarı aktarılan şablonu](export-template-portal.md) görüntülemek şablon söz dizimi hakkında bilgi edinmek için yararlı bir yoldur.

Portaldan bir çözüm oluşturduğunuzda çözüm otomatik olarak bir dağıtım şablonu içerir. Bir şablonla başlayacağınız ve bu şablonu size özel ihtiyaçlara göre özelleştirebileceğiniz için yeni bir şablon oluşturmanız gerekmez. Bir örnek için bkz [. hızlı başlangıç: Azure portal](./resource-manager-quickstart-create-templates-use-the-portal.md)kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın.

## <a name="template-deployment-process"></a>Şablon dağıtımı işlemi

Bir şablonu dağıttığınızda Kaynak Yöneticisi şablonu ayrıştırır ve sözdizimini REST API işlemlerine dönüştürür. Örneğin, Resource Manager aşağıdaki kaynak tanımına sahip bir şablonu aldığında:

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

Kaynak grupta zaten varsa ve istek özelliklerde güncelleştirme içermiyorsa, hiçbir işlem yapılmaz. Kaynak varsa ancak Özellikler değiştiyse, mevcut kaynak güncellenir. Kaynak yoksa, yeni kaynak oluşturulur. Bu yaklaşım, bir şablonu yeniden dağıtmanız ve kaynakların tutarlı bir durumda kaldığını bilmeniz için güvenli hale getirir.

## <a name="template-design"></a>Şablon tasarımı

Şablonları ve kaynak gruplarını tanımlama şekli tamamen size ve çözümünüzü yönetme biçiminize bağlıdır. Örneğin, üç katmanlı uygulamanızı tek bir şablondan tek bir kaynak grubuna dağıtabilirsiniz.

![üç katmanlı şablon](./media/template-deployment-overview/3-tier-template.png)

Ancak, bütün altyapınızı tek bir şablonda tanımlamak zorunda değilsiniz. Genellikle, en uygun seçenek dağıtım gereksinimlerinizi hedeflenen, amaca yönelik bir dizi şablona bölüştürmektir. Bu şablonları farklı çözümler için kolayca yeniden kullanabilirsiniz. Belirli bir çözümü dağıtmak için gerekli tüm şablonların bağlandığı bir ana şablon oluşturun. Aşağıdaki görüntüde üç katmanlı çözümün iç içe üç şablon içeren ana şablon aracılığıyla nasıl dağıtıldığı gösterilmektedir.

![iç içe geçmiş şablon](./media/template-deployment-overview/nested-tiers-template.png)

Katmanlarınızın farklı yaşam döngülerine sahip olacağını düşünüyorsanız, üç katmanı farklı kaynak gruplarına dağıtabilirsiniz. Kaynaklar diğer kaynak gruplarındaki kaynaklara bağlanmaya devam edebilir.

![katman şablonu](./media/template-deployment-overview/tier-templates.png)

İç içe geçmiş şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md).

## <a name="next-steps"></a>Sonraki adımlar

* Şablon dosyalarındaki özellikler hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonlarının yapısını ve sözdizimini anlayın](resource-group-authoring-templates.md).
* Şablon geliştirmeye başlamak için bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](resource-manager-tools-vs-code.md).


