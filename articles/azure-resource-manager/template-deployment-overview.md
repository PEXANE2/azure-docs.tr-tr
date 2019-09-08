---
title: Azure Resource Manager şablonları
description: Kaynakların dağıtımı için Azure Resource Manager şablonlarının nasıl kullanılacağını açıklar.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/07/2019
ms.author: tomfitz
ms.openlocfilehash: 61e9bbabee969280c07521edb05d67ba68c0c58e
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70801995"
---
# <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları

Buluta geçiş sayesinde, birçok ekip çevik geliştirme yöntemlerini benimsemiştir. Bu takımlar hızla yineleyebilir. Çözümlerini buluta sürekli olarak dağıtmaları ve altyapısının güvenilir bir durumda olduğunu bilmeniz gerekir. Altyapı yinelemeli işlemin bir parçası haline geldiği için, işlemler ve geliştirme arasındaki bölme kayboldu. Ekiplerin, Birleşik bir işlem aracılığıyla altyapıyı ve uygulama kodunu yönetmesi gerekir.

Bu güçlükleri karşılamak için dağıtımları otomatik hale getirebilir ve kod olarak altyapı uygulamasını kullanabilirsiniz. Kodda, dağıtılması gereken altyapıyı tanımlarsınız. Altyapı kodu projenizin bir parçası haline gelir. Uygulama kodu gibi, altyapı kodunu bir kaynak deposunda depolar ve sürümü. Takımınızda herhangi biri kodu çalıştırabilir ve benzer ortamları dağıtabilir.

Altyapıyı Azure çözümleriniz için kod olarak uygulamak üzere Azure Resource Manager şablonlarını kullanın. Şablon, projeniz için altyapıyı ve yapılandırmayı tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Şablon bildirim temelli sözdizimini kullanır, bu, oluşturmak için programlama komutlarının dizisini yazmak zorunda kalmadan ne dağıtmayı istediğinizi belirtmenize olanak tanır. Şablonda, dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

## <a name="benefits-of-resource-manager-templates"></a>Kaynak Yöneticisi şablonlarının avantajları

Kaynak Yöneticisi şablonlar aşağıdaki avantajları sağlar:

* Bu kaynakları tek tek işlemek yerine, çözümünüz için tüm kaynakları bir grup olarak dağıtın, yönetin ve izleyin.

* Çözümünüzü geliştirme yaşam döngüsü boyunca sürekli olarak dağıtın ve kaynaklarınızın tutarlı bir durumda dağıtıldığından emin olmanız gerekir.

* Altyapınızı betikler yerine bildirim temelli şablonlar aracılığıyla yönetin.

Kaynak Yöneticisi şablonlarını veya diğer altyapılardan birini kod hizmetleri olarak kullanmaya karar vermeye çalışıyorsanız, aşağıdaki avantajlar şablonlarının bu hizmetler üzerinde olduğunu göz önünde bulundurun:

* Yeni Azure hizmetleri ve özellikleri şablonlarda hemen kullanılabilir. Kaynak sağlayıcısı yeni kaynakları kullanıma sunarak, bu kaynakları şablonlar aracılığıyla dağıtabilirsiniz. Kod hizmetleri olarak diğer altyapıyla, üçüncü tarafların yeni kaynaklar için arabirim uygulaması için beklemeniz gerekir.

* Şablon dağıtımları, birden çok kesinlik dışı komut yerine, şablonun tek bir gönderimi aracılığıyla işlenir. Kaynak Yöneticisi, birbirine bağlı kaynakların dağıtımını doğru sırada oluşturulacak şekilde düzenler. Şablonu ayrıştırır ve kaynaklar arasındaki başvurulara bağlı olarak dağıtım için doğru sırayı belirler.

   ![Şablon dağıtımı karşılaştırma](./media/template-deployment-overview/template-processing.png)

* Şablon dağıtımları Azure portal izlenir. Dağıtım geçmişini gözden geçirebilir ve şablon dağıtımı hakkında bilgi edinebilirsiniz. Dağıtılan şablonu, geçirilen parametre değerlerini ve tüm çıktı değerlerini görebilirsiniz. Kod hizmetleri olarak diğer altyapı Portal üzerinden izlenmez.

   ![Dağıtım geçmişi](./media/template-deployment-overview/deployment-history.png)

* Şablon dağıtımları, ön uçuş doğrulamasından geçer. Kaynak Yöneticisi dağıtımın başarılı olduğundan emin olmak için dağıtımı başlatmadan önce şablonu denetler. Dağıtımınız, yarı tamamlanmış bir durumda durmak daha az olabilir.

* [Azure ilkeleri](../governance/policy/overview.md)kullanıyorsanız, ilke düzeltme, şablonlar aracılığıyla dağıtıldığında uyumlu olmayan kaynaklar üzerinde yapılır.

* Microsoft, mevzuat ve uyumluluk standartlarını karşılamak için dağıtım [şemaları](../governance/blueprints/overview.md) sağlar. Bu planlar, çeşitli mimarilere yönelik önceden oluşturulmuş şablonlar içerir.

## <a name="idempotent"></a>Idempotent

Idempotent yalnızca aynı işlemleri birçok kez çalıştırabilmeniz ve aynı sonucu elde etmeniz anlamına gelir. Kaynak Yöneticisi şablonu dağıtmak, ıdempotent. Aynı şablonu birçok kez dağıtabilir ve aynı durumda aynı kaynak türlerini alabilirsiniz. Bu kavram, bir şablonu var olan bir kaynak grubuna yeniden dağıtmanıza veya bir şablonu yeni bir kaynak grubuna dağıtmanıza bakılmaksızın tutarlı sonuçlar elde ettiğiniz anlamına geldiğinden, bu kavram önemlidir.

Bir kaynak grubuna üç kaynak dağıttığınızı ve sonra dördüncü kaynak eklemeniz gerektiğine karar verelim. Yalnızca yeni kaynağı içeren yeni bir şablon oluşturmak yerine varolan şablonunuza dördüncü kaynağı ekleyebilirsiniz. Yeni şablonu, zaten üç kaynağı bulunan kaynak grubuna dağıttığınızda, Kaynak Yöneticisi gerçekleştirilecek eylemleri belirler.

Kaynak grubunda kaynak varsa ve istek özelliklerde güncelleştirme içermiyorsa, hiçbir işlem yapılmaz. Kaynak varsa ancak Özellikler değiştiyse, mevcut kaynak güncellenir. Kaynak yoksa, yeni kaynak oluşturulur.

Dağıtım bittiğinde kaynakların her zaman beklenen durumda olduğunu güvendiniz.

## <a name="template-file"></a>Şablon dosyası

Şablonunuzda, JSON özelliklerini genişleten [şablon ifadeleri](template-expressions.md) yazabilirsiniz. Bu ifadeler Kaynak Yöneticisi tarafından sunulan [işlevleri](resource-group-template-functions.md) kullanır.

Şablon aşağıdaki bölümlere sahiptir:

* [Parametreler](template-parameters.md) -dağıtım sırasında, aynı şablonun farklı ortamlarla kullanılmasına izin veren değerler sağlar.

* [Değişkenler](template-variables.md) -şablonlarınız için yeniden kullanılan değerleri tanımlayın. Parametre değerlerinden oluşturulabilirler.

* [Kullanıcı tanımlı işlevler](template-user-defined-functions.md) -şablonunuzu basitleştiren özelleştirilmiş işlevler oluşturun.

* [Kaynaklar](resource-group-authoring-templates.md#resources) -dağıtılacak kaynakları belirtin.

* [Çıktılar](template-outputs.md) -dağıtılan kaynaklardan değer döndürür.

## <a name="template-features"></a>Şablon Özellikleri

Kaynak Yöneticisi, kaynakların doğru sırada oluşturulmasını sağlamak için bağımlılıkları analiz eder. Çoğu bağımlılık örtük olarak belirlenir. Ancak, bir kaynağın başka bir kaynaktan önce dağıtıldığından emin olmak için açıkça bir bağımlılık ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Resource Manager’da bağımlılıkları tanımlama](resource-group-define-dependencies.md).

Şablonunuza bir kaynak ekleyebilir ve isteğe bağlı olarak dağıtabilirsiniz. Genellikle, kaynağın dağıtılması gerekip gerekmediğini belirten bir parametre değeri geçirin. Daha fazla bilgi için [Kaynak Yöneticisi şablonlarda koşullu dağıtım](conditional-resource-deployment.md)konusuna bakın.

Şablonunuzda çok sayıda JSON blok blokları yerine, bir kopya öğesi kullanarak bir değişken, özellik veya kaynağın birden fazla örneğini belirtebilirsiniz. Daha fazla bilgi için [Azure Resource Manager şablonlarda kaynak, özellik veya değişken yinelemesi](resource-group-create-multiple.md)bölümüne bakın.

## <a name="export-templates"></a>Şablonları dışarı aktarma

Kaynak grubunun geçerli durumunu dışarı aktararak veya belirli bir dağıtım için kullanılan şablonu görüntüleyerek, var olan bir kaynak grubu için bir şablon alabilirsiniz. [Dışarı aktarılan şablonu](export-template-portal.md) görüntülemek şablon söz dizimi hakkında bilgi edinmek için yararlı bir yoldur.

Portaldan bir çözüm oluşturduğunuzda çözüm otomatik olarak bir dağıtım şablonu içerir. Bir şablonla başlayacağınız ve bu şablonu size özel ihtiyaçlara göre özelleştirebileceğiniz için yeni bir şablon oluşturmanız gerekmez. Bir örnek için bkz [. hızlı başlangıç: Azure portal](./resource-manager-quickstart-create-templates-use-the-portal.md)kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın.

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
* Şablon geliştirmeye başlamak için bkz. [Azure Resource Manager şablonları oluşturmak için Visual Studio Code kullanma](resource-manager-tools-vs-code.md).
* Yönetim özellikleri de dahil olmak üzere Kaynak Yöneticisi hizmetine giriş için bkz. [Azure Resource Manager genel bakış](resource-group-overview.md).

