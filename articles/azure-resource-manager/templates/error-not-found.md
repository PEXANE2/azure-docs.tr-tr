---
title: Kaynak hataları bulunamadı
description: Azure Kaynak Yöneticisi şablonuyla dağıtılırken kaynak bulunamadığında hataların nasıl çözüleceğini açıklar.
ms.topic: troubleshooting
ms.date: 01/21/2020
ms.openlocfilehash: b6f433118092e46f734d4b65040dd97c2fcb58d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76773256"
---
# <a name="resolve-not-found-errors-for-azure-resources"></a>Azure kaynakları için bulunamadı hataları çözme

Bu makalede, dağıtım sırasında bir kaynak bulunamadığında görebileceğiniz hataları açıklanmaktadır.

## <a name="symptom"></a>Belirti

Şablonunuz çözülemeyen bir kaynağın adını içeriyorsa, aşağıdakilere benzer bir hata alırsınız:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

[Çözümlenemez](template-functions-resource.md#reference) bir kaynakile başvuru veya [listKeys](template-functions-resource.md#listkeys) işlevlerini kullanırsanız, aşağıdaki hatayı alırsınız:

```
Code=ResourceNotFound;
Message=The Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

## <a name="cause"></a>Nedeni

Kaynak Yöneticisi'nin bir kaynak için özellikleri alması gerekir, ancak aboneliğinizdeki kaynağı tanımlayamaz.

## <a name="solution-1---set-dependencies"></a>Çözüm 1 - küme bağımlılıkları

Şablonda eksik kaynağı dağıtmaya çalışıyorsanız, bağımlılık eklemeniz gerekip gerekmediğini denetleyin. Kaynak Yöneticisi, mümkün olduğunda paralel olarak kaynak oluşturarak dağıtımı en iyi duruma getirmektedir. Bir kaynağın başka bir kaynaktan sonra dağıtılması gerekiyorsa, şablonunuzdaki **Bağlı Öğe** öğesini kullanmanız gerekir. Örneğin, bir web uygulaması dağıtılırken, Uygulama Hizmeti planının bulunması gerekir. Web uygulamasının Uygulama Hizmeti planına bağlı olduğunu belirtmediysen, Kaynak Yöneticisi her iki kaynağı da aynı anda oluşturur. Web uygulamasında bir özellik ayarlamaya çalışırken henüz var olmadığından, Uygulama Hizmeti planı kaynağının bulunamadığını belirten bir hata alırsınız. Web uygulamasındaki bağımlılığı ayarlayarak bu hatayı önlersiniz.

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

Ancak, gerekli olmayan bağımlılıkları ayarlamaktan kaçınmak istersiniz. Gereksiz bağımlılıklarınız olduğunda, birbirine bağımlı olmayan kaynakların paralel olarak dağıtılmasını engelleyerek dağıtım süresini uzatırsınız. Ayrıca, dağıtımı engelleyen dairesel bağımlılıklar oluşturabilirsiniz. [Başvuru](template-functions-resource.md#reference) işlevi ve [liste*](template-functions-resource.md#list) işlevleri, kaynak aynı şablonda dağıtıldığında ve adı (kaynak kimliği yle değil) başvurulduğunda, başvurulan kaynağa örtülü bir bağımlılık oluşturur. Bu nedenle, bağlı **özelliği** belirtilen bağımlılıklar daha fazla bağımlılık olabilir. [ResourceId](template-functions-resource.md#resourceid) işlevi örtük bir bağımlılık oluşturmaz veya kaynağın var olduğunu doğrulamaz. Kaynak kaynağı kimliğitarafından atıfta [bulunulduğunda, başvuru](template-functions-resource.md#reference) işlevi ve [liste*](template-functions-resource.md#list) işlevleri örtük bir bağımlılık oluşturmaz. Örtük bağımlılık oluşturmak için, aynı şablonda dağıtılan kaynağın adını geçirin.

Bağımlılık sorunlarını gördüğünüzde, kaynak dağıtım sırası hakkında bilgi edinmeniz gerekir. Dağıtım işlemlerinin sırasını görüntülemek için:

1. Kaynak grubunuzun dağıtım geçmişini seçin.

   ![dağıtım geçmişini seçin](./media/error-not-found/select-deployment.png)

2. Geçmişten bir dağıtım seçin ve **Olaylar'ı**seçin.

   ![dağıtım olaylarını seçme](./media/error-not-found/select-deployment-events.png)

3. Her kaynak için olayların sırasını inceleyin. Her operasyonun durumuna dikkat edin. Örneğin, aşağıdaki resim paralel olarak dağıtılan üç depolama hesabı gösterir. Üç depolama hesabının aynı anda başlatıldığuna dikkat edin.

   ![paralel dağıtım](./media/error-not-found/deployment-events-parallel.png)

   Sonraki resimde, paralel olarak dağıtılmamaüç depolama hesabı gösterilmektedir. İkinci depolama hesabı ilk depolama hesabına, üçüncü depolama hesabı ise ikinci depolama hesabına bağlıdır. İlk depolama hesabı başlatılır, kabul edilir ve sonraki başlatılmadan önce tamamlanır.

   ![sıralı dağıtım](./media/error-not-found/deployment-events-sequence.png)

## <a name="solution-2---get-resource-from-different-resource-group"></a>Çözüm 2 - farklı kaynak grubundan kaynak almak

Kaynak, dağıtılan kaynaktan farklı bir kaynak grubunda varsa, kaynağın tam nitelikli adını almak için [resourceId işlevini](template-functions-resource.md#resourceid) kullanın.

```json
"properties": {
  "name": "[parameters('siteName')]",
  "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

## <a name="solution-3---check-reference-function"></a>Çözüm 3 - kontrol referans fonksiyonu

[Başvuru](template-functions-resource.md#reference) işlevini içeren bir ifade arayın. Sağladığınız değerler, kaynağın aynı şablonda, kaynak grubunda ve abonelikte olup olmadığına bağlı olarak değişir. Senaryonuz için gerekli parametre değerlerini sağladığınızı iki kez denetleyin. Kaynak farklı bir kaynak grubundaysa, tam kaynak kimliğini sağlayın. Örneğin, başka bir kaynak grubundaki bir depolama hesabına başvurmak için şunları kullanın:

```json
"[reference(resourceId('exampleResourceGroup', 'Microsoft.Storage/storageAccounts', 'myStorage'), '2017-06-01')]"
```

## <a name="solution-4---get-managed-identity-from-resource"></a>Çözüm 4 - kaynaktan yönetilen kimlik almak

Yönetilen bir kimlik oluşturan bir kaynağı dağıtıyorsanız, yönetilen [kimlikteki](../../active-directory/managed-identities-azure-resources/overview.md)değerleri almadan önce bu kaynağın dağıtılmasını beklemeniz gerekir. Yönetilen kimlik adını [başvuru](template-functions-resource.md#reference) işlevine geçirirseniz, Kaynak Yöneticisi kaynak ve kimlik dağıtılmadan önce başvuruyu çözmeye çalışır. Bunun yerine, kimliğin uygulandığı kaynağın adını geçirin. Bu yaklaşım, Kaynak Yöneticisi başvuru işlevini çözmeden önce kaynağın ve yönetilen kimliğin dağıtılmasını sağlar.

Başvuru işlevinde, `Full` yönetilen kimlik de dahil olmak üzere tüm özellikleri almak için kullanın.

Örneğin, sanal makine ölçeği kümesine uygulanan yönetilen bir kimlik için kiracı kimliğini almak için şunları kullanın:

```json
"tenantId": "[reference(resourceId('Microsoft.Compute/virtualMachineScaleSets',  variables('vmNodeType0Name')), variables('vmssApiVersion'), 'Full').Identity.tenantId]"
```