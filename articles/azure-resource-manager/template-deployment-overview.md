---
title: Azure Resource Manager şablonları
description: Kaynakların dağıtımı için Azure Resource Manager şablonlarının nasıl kullanılacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: tomfitz
ms.openlocfilehash: 6d0d162f0f6f3024f6b4b63b8df1df9fd413afc8
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965304"
---
# <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Buluta geçiş sayesinde, birçok ekip çevik geliştirme yöntemlerini benimsemiştir. Bu takımlar hızla yineleyebilir. Çözümlerini buluta sürekli olarak dağıtmaları ve altyapısının güvenilir bir durumda olduğunu bilmeniz gerekir. Altyapı yinelemeli işlemin bir parçası haline geldiği için, işlemler ve geliştirme arasındaki bölme kayboldu. Ekiplerin, Birleşik bir işlem aracılığıyla altyapıyı ve uygulama kodunu yönetmesi gerekir.

Bu güçlükleri karşılamak için dağıtımları otomatik hale getirebilir ve kod olarak altyapı uygulamasını kullanabilirsiniz. Kodda, dağıtılması gereken altyapıyı tanımlarsınız. Altyapı kodu projenizin bir parçası haline gelir. Uygulama kodu gibi, altyapı kodunu bir kaynak deposunda depolar ve sürümü. Takımınızda herhangi biri kodu çalıştırabilir ve benzer ortamları dağıtabilir.

Altyapıyı Azure çözümleriniz için kod olarak uygulamak üzere Azure Resource Manager şablonlarını kullanın. Şablon, projeniz için altyapıyı ve yapılandırmayı tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Şablon bildirim temelli sözdizimini kullanır, bu, oluşturmak için programlama komutlarının dizisini yazmak zorunda kalmadan ne dağıtmayı istediğinizi belirtmenize olanak tanır. Şablonda, dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

## <a name="why-choose-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarını neden seçmeliyim?

Kaynak Yöneticisi şablonları ve diğer altyapılardan birini kod hizmetleri olarak kullanmaya karar vermeye çalışıyorsanız, şablonları kullanmanın aşağıdaki avantajlarını göz önünde bulundurun:

* **Bildirime dayalı sözdizimi**: Kaynak Yöneticisi şablonlar, tüm Azure altyapısını bildirimli olarak oluşturmanıza ve dağıtmanıza olanak tanır. Örneğin, yalnızca sanal makineler, aynı zamanda ağ altyapısı, depolama sistemleri ve ihtiyacınız olabilecek diğer kaynaklar için dağıtım yapabilirsiniz.

* **Tekrarlanabilir sonuçlar**: Altyapınızı geliştirme yaşam döngüsü boyunca sürekli olarak dağıtın ve kaynaklarınızın tutarlı bir şekilde dağıtılmasının rahatlığını yapın. Şablonlar, aynı şablonu birçok kez dağıtabileceğiniz ve aynı durumda aynı kaynak türlerini alabileceği anlamına gelen ıdempotent. Güncelleştirmeleri temsil etmek için çok sayıda ayrı şablon geliştirmek yerine istenen durumu temsil eden bir şablon geliştirebilirsiniz.

* **Düzenleme**: Sıralama işlemlerinin karmaşıklıkları hakkında endişelenmeniz gerekmez. Kaynak Yöneticisi, birbirine bağlı kaynakların dağıtımını doğru sırada oluşturulacak şekilde düzenler. Mümkün olduğunda, dağıtımlarınızın seri dağıtımlardan daha hızlı tamamlanabilmesi için Kaynak Yöneticisi Kaynakları paralel olarak dağıtır. Şablonu, birden çok zorunlu komut yerine bir komut aracılığıyla dağıtırsınız.

   ![Şablon dağıtımı karşılaştırma](./media/template-deployment-overview/template-processing.png)

* **Yerleşik doğrulama**: Şablonunuz yalnızca doğrulama işlemi yapıldıktan sonra dağıtılır. Kaynak Yöneticisi dağıtımın başarılı olduğundan emin olmak için dağıtımı başlatmadan önce şablonu denetler. Dağıtımınız, yarı tamamlanmış bir durumda durmak daha az olabilir.

* **Modüler dosyalar**: Şablonlarınızı daha küçük, yeniden kullanılabilir bileşenlere bölebilir ve dağıtım zamanında birbirine bağlayabilirsiniz. Ayrıca, bir şablonu başka bir şablon içinde iç içe geçirebilirsiniz.

* **Herhangi bir Azure kaynağı oluşturun**: Şablonlarda yeni Azure hizmetlerini ve özelliklerini hemen kullanabilirsiniz. Kaynak sağlayıcısı yeni kaynakları kullanıma sunarak, bu kaynakları şablonlar aracılığıyla dağıtabilirsiniz. Yeni hizmetleri kullanmadan önce araçların veya modüllerin güncelleştirilmesini beklemeniz gerekmez.

* **İzlenen dağıtımlar**: Azure portal, dağıtım geçmişini gözden geçirebilir ve şablon dağıtımı hakkında bilgi alabilirsiniz. Dağıtılan şablonu, geçirilen parametre değerlerini ve tüm çıktı değerlerini görebilirsiniz. Kod hizmetleri olarak diğer altyapı Portal üzerinden izlenmez.

   ![Dağıtım geçmişi](./media/template-deployment-overview/deployment-history.png)

* **Kod olarak ilke**: [Azure ilkesi](../governance/policy/overview.md) , idare otomatikleştiren kod çerçevesi olarak bir ilkedir. Azure ilkeleri kullanıyorsanız, ilke düzeltme, şablonlar aracılığıyla dağıtıldığında uyumlu olmayan kaynaklar üzerinde yapılır.

* **Dağıtım planları**: Microsoft tarafından, mevzuat ve uyumluluk standartlarını karşılamak [için sunulan planların](../governance/blueprints/overview.md) avantajlarından yararlanabilirsiniz. Bu planlar, çeşitli mimarilere yönelik önceden oluşturulmuş şablonlar içerir.

* **Dışarı aktarılabilir kod**: Kaynak grubunun geçerli durumunu dışarı aktararak veya belirli bir dağıtım için kullanılan şablonu görüntüleyerek, var olan bir kaynak grubu için bir şablon alabilirsiniz. [Dışarı aktarılan şablonu](export-template-portal.md) görüntülemek şablon söz dizimi hakkında bilgi edinmek için yararlı bir yoldur.

* **Yazma araçları**: Şablonları [Visual Studio Code](resource-manager-tools-vs-code.md) ve şablon aracı uzantısıyla yazabilirsiniz. IntelliSense, sözdizimi vurgulama, çevrimiçi Yardım ve diğer birçok dil işlevini edinirsiniz.

## <a name="template-file"></a>Şablon dosyası

Şablonunuzda, JSON özelliklerini genişleten [şablon ifadeleri](template-expressions.md) yazabilirsiniz. Bu ifadeler Kaynak Yöneticisi tarafından sunulan [işlevleri](resource-group-template-functions.md) kullanır.

Şablon aşağıdaki bölümlere sahiptir:

* [Parametreler](template-parameters.md) -dağıtım sırasında, aynı şablonun farklı ortamlarla kullanılmasına izin veren değerler sağlar.

* [Değişkenler](template-variables.md) -şablonlarınız için yeniden kullanılan değerleri tanımlayın. Parametre değerlerinden oluşturulabilirler.

* [Kullanıcı tanımlı işlevler](template-user-defined-functions.md) -şablonunuzu basitleştiren özelleştirilmiş işlevler oluşturun.

* [Kaynaklar](resource-group-authoring-templates.md#resources) -dağıtılacak kaynakları belirtin.

* [Çıktılar](template-outputs.md) -dağıtılan kaynaklardan değer döndürür.

## <a name="template-deployment-process"></a>Şablon dağıtımı işlemi

Bir şablonu dağıtırken Kaynak Yöneticisi şablonu REST API işlemlerine dönüştürür. Örneğin, Resource Manager aşağıdaki kaynak tanımına sahip bir şablonu aldığında:

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
* Bir kaynağın başka bir kaynaktan önce dağıtılması için bağımlılıkları açıkça ayarlamak için, bkz. [Azure Resource Manager şablonlarda bağımlılıkları tanımlama](resource-group-define-dependencies.md).
* Şablonunuza bir kaynak ekleyebilir ve isteğe bağlı olarak dağıtabilirsiniz. Daha fazla bilgi için [Kaynak Yöneticisi şablonlarda koşullu dağıtım](conditional-resource-deployment.md)konusuna bakın.
* Şablonunuzda çok sayıda JSON yineleme bloğu yerine, bir değişken, özellik veya kaynak örneği belirtebilirsiniz. Daha fazla bilgi için [Azure Resource Manager şablonlarda kaynak, özellik veya değişken yinelemesi](resource-group-create-multiple.md)bölümüne bakın.
* Şablonları dışarı aktarma hakkında bilgi edinmek için [bkz. hızlı başlangıç: Azure portal](./resource-manager-quickstart-create-templates-use-the-portal.md)kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın.
