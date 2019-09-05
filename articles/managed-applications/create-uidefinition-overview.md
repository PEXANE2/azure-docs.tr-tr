---
title: Azure yönetilen uygulamanın oluşturma deneyimi için Createuıdefinition. JSON | Microsoft Docs
description: Azure yönetilen uygulamalar için Kullanıcı arabirimi tanımlarının nasıl oluşturulacağını açıklar
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/06/2019
ms.author: tomfitz
ms.openlocfilehash: 013e861bb93d76454f2f0fd9c36259197dd671b9
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308655"
---
# <a name="createuidefinitionjson-for-azure-managed-applications-create-experience"></a>Azure yönetilen uygulamanın oluşturma deneyimi için Createuıdefinition. JSON

Bu belge, yönetilen bir uygulama oluştururken Kullanıcı arabirimini tanımlamak için Azure portal tarafından kullanılan **Createuıdefinition. JSON** dosyasının temel kavramlarını tanıtır.

Şablon aşağıdaki gibidir

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { },
      "resourceTypes": [ ]
   }
}
```

Createuıdefinition her zaman üç özellik içerir: 

* Iy
* version
* parameters

İşleyici her zaman `Microsoft.Azure.CreateUIDef`ve desteklenen en son `0.1.2-preview`sürüm olmalıdır.

Parameters özelliğinin şeması, belirtilen işleyicinin ve sürümün birleşimine bağlıdır. Yönetilen uygulamalar için desteklenen özellikler, `basics` `steps`ve `outputs`' dir. Temel bilgiler ve adımlar özellikleri, Azure portal görüntülenecek metin kutuları ve açılan [öğeler](create-uidefinition-elements.md) içerir. Çıktılar özelliği, belirtilen öğelerin çıkış değerlerini Azure Resource Manager dağıtım şablonunun parametreleriyle eşlemek için kullanılır.

Dahil `$schema` edilmesi önerilir, ancak isteğe bağlıdır. Belirtilmişse, değeri `version` `$schema` URI içindeki sürümle eşleşmelidir.

Createuıdefinition 'nizi oluşturmak için bir JSON Düzenleyicisi kullanabilir, ardından bunu önizlemek için [Createuıdefinition korumalı](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) alanında test edebilirsiniz. Korumalı alan hakkında daha fazla bilgi için bkz. [Azure yönetilen uygulamalar için Portal arabiriminizi test](test-createuidefinition.md)etme.

## <a name="basics"></a>Temel

Temel bilgiler, Azure portal dosyayı ayrıştırdığında oluşturulan ilk adımdır. Portal, içinde `basics`belirtilen öğeleri görüntülemenin yanı sıra, kullanıcıların abonelik, kaynak grubu ve dağıtımın konumunu seçmesi için öğeleri çıkarır. Mümkün olduğunda, küme veya yönetici kimlik bilgileri gibi dağıtım çapındaki parametreleri sorgulayan öğeler bu adımda ilerlemelidir.

## <a name="steps"></a>Adımlar

Steps özelliği, her biri bir veya daha fazla öğe içeren temel kavramlar sonrasında görüntülenecek sıfır veya daha fazla ek adım içerebilir. Dağıtılan uygulamanın rol veya katmanına göre adımları eklemeyi düşünün. Örneğin, ana düğüm girişleri için bir adım ve bir kümedeki çalışan düğümlerine yönelik bir adım ekleyin.

## <a name="outputs"></a>outputs

Azure Portal, `outputs` `basics` ve AzureResourceManagerdağıtımşablonununparametreleriyleöğelerieşlemekiçinözelliğinikullanır.`steps` Bu sözlüğün anahtarları, şablon parametrelerinin adlarıdır ve değerler başvurulan öğelerden çıkış nesnelerinin özellikleridir.

Yönetilen uygulama kaynak adını ayarlamak için, çıktılar özelliğinde adlı `applicationResourceName` bir değer eklemelisiniz. Bu değeri ayarlamazsanız, uygulama ad için bir GUID atar. Kullanıcı arabirimine, kullanıcıdan bir ad isteyen bir metin kutusu ekleyebilirsiniz.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="resource-types"></a>Kaynak türleri

Kullanılabilir konumları yalnızca dağıtılacak kaynak türlerini destekleyen konumlara göre filtrelemek için kaynak türlerinin bir dizisini sağlayın. Birden çok kaynak türü sağlarsanız, yalnızca tüm kaynak türlerini destekleyen konumlar döndürülür. Bu özellik isteğe bağlıdır.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Azure.CreateUIDef",
    "version": "0.1.2-preview",
    "parameters": {
      "resourceTypes": ["Microsoft.Compute/disks"],
      "basics": [
        ...
```  

## <a name="functions"></a>İşlevler

Createuıdefinition, öğelerin girdileri ve çıkışları ve conditionals gibi özelliklerle çalışmak için [işlevler](create-uidefinition-functions.md) sağlar. Bu işlevler, Azure Resource Manager şablon işlevleri için hem söz dizimi hem de işlevselliğe benzer.

## <a name="next-steps"></a>Sonraki adımlar

Createuıdefinition. json dosyasının kendisi basit bir şemaya sahiptir. Gerçek derinliği, desteklenen tüm öğe ve işlevlerden gelir. Bu öğeler, daha ayrıntılı olarak açıklanmıştır:

- [Ög](create-uidefinition-elements.md)
- [İşlevler](create-uidefinition-functions.md)

Createuıdefinition için geçerli bir JSON şeması şurada bulunabilir: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Örnek bir kullanıcı arabirimi dosyası için bkz. [Createuıdefinition. JSON](https://github.com/Azure/azure-managedapp-samples/blob/master/Managed%20Application%20Sample%20Packages/201-managed-app-using-existing-vnet/createUiDefinition.json).
