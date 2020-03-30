---
title: Geçersiz şablon hataları
description: Azure Kaynak Yöneticisi şablonlarını dağıtırken geçersiz şablon hatalarının nasıl çözüleceğini açıklar.
ms.topic: troubleshooting
ms.date: 03/08/2018
ms.openlocfilehash: 65cd69d67933d117b51f37b587b276aec2bd635a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76154066"
---
# <a name="resolve-errors-for-invalid-template"></a>Geçersiz şablon hatalarını giderme

Bu makalede, geçersiz şablon hatalarınasıl çözüleceği açıklanmaktadır.

## <a name="symptom"></a>Belirti

Bir şablon dağıtırken, şunları belirten bir hata alırsınız:

```
Code=InvalidTemplate
Message=<varies>
```

Hata iletisi hata türüne bağlıdır.

## <a name="cause"></a>Nedeni

Bu hata birkaç farklı hata türünden kaynaklanabilir. Genellikle şablonda bir sözdizimi veya yapısal hata içerir.

<a id="syntax-error" />

## <a name="solution-1---syntax-error"></a>Çözüm 1 - sözdizimi hatası

Şablon başarısız doğrulamayı gösteren bir hata iletisi alırsanız, şablonunuzda sözdizimi sorununuz olabilir.

```
Code=InvalidTemplate
Message=Deployment template validation failed
```

Şablon ifadeleri karmaşık olabileceğinden, bu hatayı yapmak kolaydır. Örneğin, bir depolama hesabı için aşağıdaki ad ataması parantez, üç işlev, üç parantez kümesi, bir tek tırnak kümesi ve bir özellik içerir:

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
```

Eşleşen sözdizimini sağlamazsanız, şablon amacınızdan farklı bir değer üretir.

Bu tür bir hata aldığınızda, ifade sözdizimini dikkatle gözden geçirin. [Görsel Stüdyo](create-visual-studio-deployment-project.md) veya Visual Studio [Code](use-vs-code-to-create-template.md)gibi bir JSON editörü kullanmayı düşünün , sözdizimi hataları hakkında sizi uyarabilir.

<a id="incorrect-segment-lengths" />

## <a name="solution-2---incorrect-segment-lengths"></a>Çözüm 2 - yanlış segment uzunlukları

Kaynak adı doğru biçimde olmadığında başka bir geçersiz şablon hatası oluşur.

```
Code=InvalidTemplate
Message=Deployment template validation failed: 'The template resource {resource-name}'
for type {resource-type} has incorrect segment lengths.
```

Kök düzey kaynağının adında kaynak türünden daha az bir kesimi olmalıdır. Her segment bir eğik çizgi ile ayırt edilir. Aşağıdaki örnekte, tür iki kesimi vardır ve adı bir kesimi vardır, bu yüzden geçerli bir **ad.**

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "myHostingPlanName",
  ...
}
```

Ancak bir sonraki örnek, türle aynı sayıda segmente sahip olduğundan **geçerli bir ad değildir.**

```json
{
  "type": "Microsoft.Web/serverfarms",
  "name": "appPlan/myHostingPlanName",
  ...
}
```

Alt kaynaklar için, tür ve ad aynı sayıda segmente sahiptir. Bu kesim sayısı anlamlıdır, çünkü alt öğenin tam adı ve türü üst adı ve türü içerir. Bu nedenle, tam adı hala tam türünden daha az bir kesimi vardır.

```json
"resources": [
  {
    "type": "Microsoft.KeyVault/vaults",
    "name": "contosokeyvault",
    ...
    "resources": [
      {
        "type": "secrets",
        "name": "appPassword",
        ...
      }
    ]
  }
]
```

Kaynak sağlayıcılar arasında uygulanan Kaynak Yöneticisi türleri ile segmentleri doğru almak zor olabilir. Örneğin, bir web sitesine kaynak kilidi uygulamak dört segmentli bir tür gerektirir. Bu nedenle, ad üç bölümden biridir:

```json
{
  "type": "Microsoft.Web/sites/providers/locks",
  "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
  ...
}
```

<a id="parameter-not-valid" />

## <a name="solution-3---parameter-is-not-valid"></a>Çözüm 3 - parametre geçerli değil

İzin verilen değerlerden biri olmayan bir parametre değeri sağlarsanız, aşağıdaki hataya benzer bir ileti alırsınız:

```
Code=InvalidTemplate;
Message=Deployment template validation failed: 'The provided value {parameter value}
for the template parameter {parameter name} is not valid. The parameter value is not
part of the allowed values
```

Şablonda izin verilen değerleri iki kez denetleyin ve dağıtım sırasında bir tane sağlayın. İzin verilen parametre değerleri hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi şablonlarının Parametreler bölümüne](template-syntax.md#parameters)bakın.

<a id="too-many-resource-groups" />

## <a name="solution-4---too-many-target-resource-groups"></a>Çözüm 4 - Çok fazla hedef kaynak grubu

Tek bir dağıtımda beşten fazla hedef kaynak grubu belirtirseniz, bu hatayı alırsınız. Dağıtımınızdaki kaynak gruplarının sayısını birleştirmeyi veya şablonlardan bazılarını ayrı dağıtımlar olarak dağıtmayı düşünün. Daha fazla bilgi için bkz: [Azure kaynaklarını birden fazla abonelik veya kaynak grubuna dağıtın.](cross-resource-group-deployment.md)

<a id="circular-dependency" />

## <a name="solution-5---circular-dependency-detected"></a>Çözüm 5 - dairesel bağımlılık algılandı

Kaynaklar dağıtımın başlatılmasını engelleyecek şekilde birbirine bağımlı olduğunda bu hatayı alırsınız. Karşılıklı bağımlılıkların birleşimi, iki veya daha fazla kaynağın bekleyen diğer kaynakları beklemesini sağlar. Örneğin, kaynak1 kaynak3bağlıdır, kaynak2 kaynak1 bağlıdır ve kaynak3 kaynakbağlıdır2. Genellikle gereksiz bağımlılıkları kaldırarak bu sorunu çözebilirsiniz.

Dairesel bağımlılığı çözmek için:

1. Şablonunuzda, dairesel bağımlılıkta tanımlanan kaynağı bulun.
2. Bu kaynak için, bağlı **özelliği** ve hangi kaynaklara bağlı olduğunu görmek için **başvuru** işlevinin herhangi bir kullanımlarını inceleyin.
3. Hangi kaynaklara bağımlı olduklarını görmek için bu kaynakları inceleyin. Özgün kaynağa bağlı bir kaynak fark edene kadar bağımlılıkları izleyin.
5. Dairesel bağımlılıkla ilgili kaynaklar için, gerekli olmayan bağımlılıkları belirlemek için **bağlı olan** özelliğin tüm kullanımlarını dikkatle inceleyin. Bu bağımlılıkları kaldırın. Bir bağımlılık gerektiğinden emin değilseniz, bunu kaldırmayı deneyin.
6. Şablonu yeniden dağıtın.

**Bağlı olan** özellikteki değerleri kaldırmak, şablonu dağıttığınızda hatalara neden olabilir. Bir hata alırsanız, bağımlılığını şablona geri ekleyin.

Bu yaklaşım dairesel bağımlılığı çözmüyorsa, dağıtım mantığınızın bir kısmını alt kaynaklara (uzantılar veya yapılandırma ayarları gibi) taşımayı düşünün. Bu alt kaynakları, döngüsel bağımlılıkta yer alan kaynaklardan sonra dağıtacak şekilde yapılandırın. Örneğin, iki sanal makine dağıtıyorsanız, ancak her birinin diğerine atıfta bulunan özellikleri ayarlamanız gerektiğini varsayalım. Bunları aşağıdaki sırada dağıtabilirsiniz:

1. vm1
2. vm2
3. VM1'in uzatılması vm1 ve vm2'ye bağlıdır. Uzantı vm2'den aldığı değerleri vm1 olarak ayarlar.
4. VM2'nin uzatılması vm1 ve vm2'ye bağlıdır. Uzantı vm1'den aldığı değerleri vm2'de ayarlar.

Aynı yaklaşım App Service uygulamaları için de geçerlidir. Yapılandırma değerlerini uygulama kaynağının alt kaynağına taşımayı düşünün. İki web uygulamasınızı aşağıdaki sırayla dağıtabilirsiniz:

1. webapp1
2. webapp2
3. webapp1 için config webapp1 ve webapp2 bağlıdır. Webapp2'den gelen değerlerle uygulama ayarları içerir.
4. webapp2 için config webapp1 ve webapp2 bağlıdır. Webapp1'den gelen değerlerle uygulama ayarları içerir.
