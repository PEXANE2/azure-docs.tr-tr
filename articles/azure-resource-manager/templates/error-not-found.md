---
title: Kaynak bulunamadı hataları
description: Azure Resource Manager şablonuyla dağıtıldığında bir kaynak bulunamadığında hataların nasıl çözümleneceğini açıklar.
ms.topic: troubleshooting
ms.date: 01/21/2020
ms.openlocfilehash: c3e19af24fa7fb850eadf3deb346180476943241
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76310671"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Azure kaynakları için bulunamadı hatalarını çözümleme

Bu makalede, dağıtım sırasında bir kaynak bulunamadığında görebileceğiniz hatalar açıklanır.

## <a name="symptom"></a>Belirti

Şablonunuz çözülemeyen bir kaynağın adını içerdiğinde şuna benzer bir hata alırsınız:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

[Başvuru](template-functions-resource.md#reference) veya [ListKeys](template-functions-resource.md#listkeys) işlevlerini çözülemeyen bir kaynakla birlikte kullanırsanız, şu hatayı alırsınız:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Nedeni

Kaynak Yöneticisi bir kaynağın özelliklerini alması gerekiyor, ancak aboneliğinizde kaynağı tanımlayamıyor.

## <a name="solution-1---set-dependencies"></a>Çözüm 1-bağımlılıkları ayarlama

Şablonda eksik kaynağı dağıtmaya çalışıyorsanız, bir bağımlılık eklemeniz gerekip gerekmediğini kontrol edin. Kaynak Yöneticisi, mümkün olduğunda, paralel olarak kaynakları oluşturarak dağıtımı iyileştirir. Bir kaynak başka bir kaynaktan sonra dağıtılması gerekiyorsa, şablonunuzda **Bağımlıdson** öğesini kullanmanız gerekir. Örneğin, bir Web uygulaması dağıtıldığında App Service planının mevcut olması gerekir. Web uygulamasının App Service planına bağlı olduğunu belirtmediyse Kaynak Yöneticisi her iki kaynak de aynı anda oluşturulur. Yalnızca Web uygulamasında bir özellik ayarlamaya çalışırken mevcut olmadığından App Service planı kaynağının bulunamadığını belirten bir hata alırsınız. Web uygulamasındaki bağımlılığı ayarlayarak bu hatayı önleyebilirsiniz.

```json
{
  "type": "Microsoft.Web/sites",
  "apiVersion": "2015-08-01",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Ancak, gerekmeyen bağımlılıkları ayarlamayı önlemek isteyebilirsiniz. Gereksiz bağımlılıklara sahip olduğunuzda, birbirlerine bağımlı olmayan kaynakların paralel olarak dağıtılmasını önleyecek şekilde dağıtım süresini uzattınız. Ayrıca, dağıtımı engelleyen döngüsel bağımlılıklar da oluşturabilirsiniz. [Başvuru](template-functions-resource.md#reference) işlevi ve [liste *](template-functions-resource.md#list) işlevleri, başvurulan kaynak üzerinde bu kaynak aynı şablonda dağıtıldığında ve adına (kaynak kimliği değil) başvurduğu zaman bir örtük bağımlılık oluşturur. Bu nedenle, **Bağımlıdson** özelliğinde belirtilen bağımlılıklardan daha fazla bağımlılığı olabilir. [RESOURCEID](template-functions-resource.md#resourceid) işlevi örtük bir bağımlılık oluşturmaz veya kaynağın mevcut olduğunu doğrular. [Başvuru](template-functions-resource.md#reference) işlevi ve [liste *](template-functions-resource.md#list) IŞLEVLERI, kaynağa kaynak kimliği tarafından bahsedildiğinde örtülü bir bağımlılık oluşturmaz. Örtük bir bağımlılık oluşturmak için, aynı şablonda dağıtılan kaynağın adını geçirin.

Bağımlılık sorunlarını gördüğünüzde, kaynak dağıtımının sırası hakkında öngörü elde etmeniz gerekir. Dağıtım işlemlerinin sırasını görüntülemek için:

1. Kaynak grubunuz için dağıtım geçmişini seçin.

   ![Dağıtım geçmişini seçin](./media/error-not-found/select-deployment.png)

2. Geçmişten bir dağıtım seçin ve **Olaylar**' ı seçin.

   ![Dağıtım olaylarını seçin](./media/error-not-found/select-deployment-events.png)

3. Her kaynak için olay sırasını inceleyin. Her işlemin durumuna dikkat edin. Örneğin, aşağıdaki görüntüde paralel olarak dağıtılan üç depolama hesabı gösterilmektedir. Üç depolama hesabının aynı anda başlatıldığını unutmayın.

   ![paralel dağıtım](./media/error-not-found/deployment-events-parallel.png)

   Sonraki görüntüde paralel olarak dağıtılmayan üç depolama hesabı gösterilmektedir. İkinci depolama hesabı, ilk depolama hesabına bağlıdır ve üçüncü depolama hesabı ikinci depolama hesabına bağlıdır. İlk depolama hesabı, bir sonraki başlatılmadan önce başlatılır, kabul edilir ve tamamlanır.

   ![sıralı dağıtım](./media/error-not-found/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Çözüm 2-farklı kaynak grubundan kaynak al

Kaynak, dağıtılmasından farklı bir kaynak grubunda mevcut olduğunda, kaynağın tam adını almak için [RESOURCEID işlevini](template-functions-resource.md#resourceid) kullanın.

```json
"properties": {
  "name": "[parameters('siteName')]",
  "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Çözüm 3-başvuru işlevini denetle

[Başvuru](template-functions-resource.md#reference) işlevini içeren bir ifade arayın. Sağladığınız değerler, kaynağın aynı şablonda, kaynak grubunda ve abonelikte olup olmadığına göre farklılık gösterir. Senaryonuz için gerekli parametre değerlerini sağlayıp sağlamadığınızı iki kez denetleyin. Kaynak farklı bir kaynak grubsunda, tam kaynak KIMLIĞI sağlayın. Örneğin, başka bir kaynak grubundaki bir depolama hesabına başvurmak için şunu kullanın:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```

## <a name="solution-4---get-managed-identity-from-resource"></a>Çözüm 4-kaynaktan yönetilen kimliği al

[Yönetilen kimliği](../../active-directory/managed-identities-azure-resources/overview.md)dolaylı olarak oluşturan bir kaynak dağıtıyorsanız, yönetilen kimlik değerlerini almadan önce bu kaynak dağıtılana kadar beklemeniz gerekir. Yönetilen kimlik adını [başvuru](template-functions-resource.md#reference) işlevine geçirirseniz, kaynak ve kimlik dağıtılmadan önce başvuruyu çözümlemeye çalışır Kaynak Yöneticisi. Bunun yerine, kimliğin uygulandığı kaynağın adını geçirin. Bu yaklaşım, başvuru işlevini çözümlediği Kaynak Yöneticisi önce kaynak ve yönetilen kimliğin dağıtılmasını sağlar.

Başvuru işlevinde, yönetilen kimlik dahil tüm özellikleri almak için `Full` kullanın.

Örneğin, bir sanal makine ölçek kümesine uygulanan bir yönetilen kimliğin kiracı KIMLIĞINI almak için şunu kullanın:

```json
"tenantId": "[reference(concat('Microsoft.Compute/virtualMachineScaleSets/',  variables('vmNodeType0Name')), variables('vmssApiVersion'), 'Full').Identity.tenantId]"
```