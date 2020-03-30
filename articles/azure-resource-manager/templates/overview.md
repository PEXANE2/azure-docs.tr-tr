---
title: Şablonlara genel bakış
description: Kaynakların dağıtımı için Azure Kaynak Yöneticisi şablonlarını kullanarak avantajları açıklar.
ms.topic: conceptual
ms.date: 03/25/2020
ms.openlocfilehash: 4570f5471ef6baf6f3f4a920be4d93c3f5a90438
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80258133"
---
# <a name="what-are-arm-templates"></a>ARM şablonları nelerdir?

Buluta taşınmasıyla, birçok ekip çevik geliştirme yöntemlerini benimsedi. Bu takımlar hızlı bir şekilde yinelerler. Çözümlerini sürekli olarak buluta dağıtmaları ve altyapılarının güvenilir bir durumda olduğunu bilmeleri gerekir. Altyapı yinelemeli sürecin bir parçası haline geldikçe, operasyonlar ve geliştirme arasındaki bölünme ortadan kalkmıştır. Ekiplerin altyapıyı ve uygulama kodunu birleşik bir işlem le yönetmesi gerekir.

Bu zorlukları aşmak için dağıtımları otomatikleştirebilir ve altyapı uygulamalarını kod olarak kullanabilirsiniz. Kodolarak, dağıtılması gereken altyapıyı tanımlarsınız. Altyapı kodu projenizin bir parçası olur. Uygulama kodu gibi, altyapı kodunu bir kaynak deposunda saklar ve sürüm haline etersiniz. Ekibinizdeki herhangi biri kodu çalıştırabilir ve benzer ortamları dağıtabilir.

Azure çözümleriniz için altyapıyı kod olarak uygulamak için Azure Kaynak Yöneticisi (ARM) şablonlarını kullanın. Şablon, projenizin altyapısını ve yapılandırmasını tanımlayan bir JavaScript Nesne Gösterimi (JSON) dosyasıdır. Şablon, oluşturmak için programlama komutları sırasını yazmak zorunda kalmadan dağıtmak istediğinizi belirtmenize olanak tanıyan bildirimsel sözdizimini kullanır. Şablonda, dağıtılacak kaynakları ve bu kaynakların özelliklerini belirtirsiniz.

## <a name="why-choose-arm-templates"></a>Neden ARM şablonlarını seçmelisiniz?

ARM şablonlarını ve diğer altyapılardan birini kod hizmeti olarak kullanmak arasında karar vermeye çalışıyorsanız, şablonları kullanmanın aşağıdaki avantajlarını göz önünde bulundurun:

* **Bildirimsel sözdizimi**: ARM şablonları, tüm Azure altyapısını bildirimsel olarak oluşturmanıza ve dağıtmanıza olanak tanır. Örneğin, yalnızca sanal makineleri değil, ağ altyapısını, depolama sistemlerini ve gereksinim duyabileceğiniz diğer kaynakları da dağıtabilirsiniz.

* **Tekrarlanabilir sonuçlar**: Altyapınızı geliştirme yaşam döngüsü boyunca sürekli olarak dağıtın ve kaynaklarınızın tutarlı bir şekilde dağıtıldıklarından emin olun. Şablonlar idempotenttir, bu da aynı şablonu birçok kez dağıtabileceğiniz ve aynı kaynak türlerini aynı durumda alabileceğiniz anlamına gelir. Güncelleştirmeleri temsil edecek çok sayıda ayrı şablon geliştirmek yerine, istenen durumu temsil eden bir şablon geliştirebilirsiniz.

* **Orkestrasyon**: Operasyon siparişinin karmaşıklığı hakkında endişelenmenize gerek yok. Kaynak Yöneticisi, birbirine bağlı kaynakların dağıtımını doğru sırada oluşturulabilmeleri için yönetir. Mümkün olduğunda Kaynak Yöneticisi, dağıtımlarınızın seri dağıtımlardan daha hızlı bitmesine izin için kaynakları paralel olarak dağıtır. Şablonu birden çok zorunlu komut yerine tek bir komut la dağıtMış sınız.

   ![Şablon dağıtım karşılaştırması](./media/overview/template-processing.png)

* **Yerleşik doğrulama**: Şablonunuz yalnızca doğrulamayı geçtikten sonra dağıtılır. Kaynak Yöneticisi, dağıtımın başarılı olacağından emin olmak için dağıtımı başlatmadan önce şablonu denetler. Dağıtımınızın yarı mamul durumda durma olasılığı daha düşüktür.

* **Modüler dosyalar**: Şablonlarınızı daha küçük, yeniden kullanılabilir bileşenlere ayırabilir ve dağıtım zamanında birbirine bağlayabilirsiniz. Ayrıca, bir şablonu başka bir şablonun içine sokabilirsiniz.

* **Herhangi bir Azure kaynağı oluşturma**: Yeni Azure hizmetlerini ve özelliklerini şablonlarda hemen kullanabilirsiniz. Bir kaynak sağlayıcısı yeni kaynaklar sunar tanıtmaz, bu kaynakları şablonlar aracılığıyla dağıtabilirsiniz. Yeni hizmetleri kullanmadan önce araçların veya modüllerin güncellenmesi için beklemeniz gerekmez.

* **İzlenen dağıtımlar**: Azure portalında dağıtım geçmişini inceleyebilir ve şablon dağıtımı hakkında bilgi alabilirsiniz. Dağıtılan şablonu, geçirilen parametre değerlerini ve çıktı değerlerini görebilirsiniz. Kod hizmetleri olarak diğer altyapı portalı üzerinden izlenmez.

   ![Dağıtım geçmişi](./media/overview/deployment-history.png)

* **Kod Olarak İlke**: [Azure İlkesi,](../../governance/policy/overview.md) yönetimi otomatikleştirmek için kod çerçevesi olarak bir ilkedir. Azure ilkeleri kullanıyorsanız, şablonlar aracılığıyla dağıtıldığında uyumlu olmayan kaynaklarda ilke düzeltme yapılır.

* **Dağıtım Planları**: Mevzuat ve uyumluluk standartlarını karşılamak için Microsoft tarafından sağlanan [Planlardan](../../governance/blueprints/overview.md) yararlanabilirsiniz. Bu planlar, çeşitli mimariler için önceden oluşturulmuş şablonlar içerir.

* **CI/CD tümleştirmesi**: Şablonları, hızlı ve güvenilir uygulama ve altyapı güncelleştirmeleri için sürüm ardışık alanlarınızı otomatikleştirebilen sürekli tümleştirme ve sürekli dağıtım (CI/CD) araçlarınıza entegre edebilirsiniz. Azure DevOps ve Kaynak Yöneticisi şablon görevi kullanarak, ARM şablon projeleri sürekli oluşturmak ve dağıtmak için Azure Ardışık Hatları'nı kullanabilirsiniz. Daha fazla bilgi edinmek için, boru hatları ve [Azure Boru Hatları ile Sürekli tümleştirme](template-tutorial-use-azure-pipelines.md)içeren VS [projesine](add-template-to-azure-pipelines.md) bakın.

* **Dışa aktarılabilir kod**: Kaynak grubunun geçerli durumunu dışa aktararak veya belirli bir dağıtım için kullanılan şablonu görüntüleyerek varolan bir kaynak grubu için şablon alabilirsiniz. [Dışarı aktarılan şablonu](export-template-portal.md) görüntülemek şablon söz dizimi hakkında bilgi edinmek için yararlı bir yoldur.

* **Yazma araçları**: Visual Studio [Code](use-vs-code-to-create-template.md) ve şablon araç uzantısı ile şablonyazabilirsiniz. Intellisense, sözdizimi vurgulama, satır içi yardım ve diğer birçok dil işlevleri olsun. Visual Studio koduna ek olarak [Visual Studio'yı](create-visual-studio-deployment-project.md)da kullanabilirsiniz.

## <a name="template-file"></a>Şablon dosyası

Şablonunuzda JSON'un özelliklerini genişleten [şablon ifadeleri](template-expressions.md) yazabilirsiniz. Bu ifadeler Kaynak Yöneticisi tarafından sağlanan [işlevleri](template-functions.md) kullanır.

Şablonaşağıdaki bölümlere sahiptir:

* [Parametreler](template-parameters.md) - Dağıtım sırasında aynı şablonun farklı ortamlarda kullanılmasına izin veren değerler sağlayın.

* [Değişkenler](template-variables.md) - Şablonlarınızda yeniden kullanılan değerleri tanımlayın. Parametre değerlerinden oluşturulabilirler.

* [Kullanıcı tanımlı işlevler](template-user-defined-functions.md) - Şablonunuzu basitleştiren özelleştirilmiş işlevler oluşturun.

* [Kaynaklar](template-syntax.md#resources) - Dağıtılacak kaynakları belirtin.

* [Çıktılar](template-outputs.md) - Dağıtılan kaynaklardan gelen değerleri döndürün.

## <a name="template-deployment-process"></a>Şablon dağıtım işlemi

Bir şablon dağıttığınızda, Kaynak Yöneticisi şablonu REST API işlemlerine dönüştürür. Örneğin, Resource Manager aşağıdaki kaynak tanımına sahip bir şablonu aldığında:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2016-01-01",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
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
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage",
  "properties": {}
}
```

## <a name="template-design"></a>Şablon tasarımı

Şablonları ve kaynak gruplarını tanımlama şekli tamamen size ve çözümünüzü yönetme biçiminize bağlıdır. Örneğin, üç katmanlı uygulamanızı tek bir şablondan tek bir kaynak grubuna dağıtabilirsiniz.

![üç katmanlı şablon](./media/overview/3-tier-template.png)

Ancak, bütün altyapınızı tek bir şablonda tanımlamak zorunda değilsiniz. Genellikle, en uygun seçenek dağıtım gereksinimlerinizi hedeflenen, amaca yönelik bir dizi şablona bölüştürmektir. Bu şablonları farklı çözümler için kolayca yeniden kullanabilirsiniz. Belirli bir çözümü dağıtmak için gerekli tüm şablonların bağlandığı bir ana şablon oluşturun. Aşağıdaki görüntüde üç katmanlı çözümün iç içe üç şablon içeren ana şablon aracılığıyla nasıl dağıtıldığı gösterilmektedir.

![iç içe geçmiş şablon](./media/overview/nested-tiers-template.png)

Katmanlarınızın farklı yaşam döngülerine sahip olacağını düşünüyorsanız, üç katmanı farklı kaynak gruplarına dağıtabilirsiniz. Kaynaklar diğer kaynak gruplarındaki kaynaklara bağlanmaya devam edebilir.

![katman şablonu](./media/overview/tier-templates.png)

İç içe geçmiş şablonlar hakkında daha fazla bilgi için bkz. [Azure Resource Manager ile bağlı şablonları kullanma](linked-templates.md).

## <a name="next-steps"></a>Sonraki adımlar

* Şablon oluşturma sürecinde size yol gösteren adım adım bir öğretici için [Bkz. Öğretici: İlk ARM şablonunuzu oluşturun ve dağıtın.](template-tutorial-create-first-template.md)
* Şablon dosyalarındaki özellikler hakkında bilgi için [bkz.](template-syntax.md)
* Şablonları dışa aktarma hakkında bilgi edinmek için [Bkz. Quickstart: Azure portalını kullanarak ARM şablonları oluşturun ve dağıtın.](quickstart-create-templates-use-the-portal.md)
