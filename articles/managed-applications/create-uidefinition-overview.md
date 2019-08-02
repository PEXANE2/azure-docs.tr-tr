---
title: Azure yönetilen uygulamanın oluşturma deneyimi için Createuıdefiıniyon. JSON | Microsoft Docs
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
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 50bbaf740a67d3830df2d0447b9522153cb8c93c
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619088"
---
# <a name="createuidefitinionjson-for-azure-managed-applications-create-experience"></a>Azure yönetilen uygulamanın oluşturma deneyimi için Createuıdefiıniyon. JSON
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
      "outputs": { }
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

Kullanıcı arabirimi tanımınızı oluşturmak için bir JSON Düzenleyicisi kullanabilir ve bunu önizlemek için [UI tanımı korumalı](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) alanında test edebilirsiniz. Korumalı alan hakkında daha fazla bilgi için bkz. [Azure yönetilen uygulamalar için Portal arabiriminizi test](test-createuidefinition.md)etme.

## <a name="basics"></a>Temel
Temel bilgiler, Azure portal dosyayı ayrıştırdığında oluşturulan ilk adımdır. Portal, içinde `basics`belirtilen öğeleri görüntülemenin yanı sıra, kullanıcıların abonelik, kaynak grubu ve dağıtımın konumunu seçmesi için öğeleri çıkarır. Mümkün olduğunda, küme veya yönetici kimlik bilgileri gibi dağıtım çapındaki parametreleri sorgulayan öğeler bu adımda ilerlemelidir.

Bir öğenin davranışı kullanıcının aboneliğine, kaynak grubuna veya konuma bağımlıysa, bu öğe temel bilgiler bölümünde kullanılamaz. Örneğin, **Microsoft. COMPUTE. SizeSelector** , kullanılabilir boyutların listesini öğrenmek için kullanıcının aboneliğine ve konumuna bağımlıdır. Bu nedenle, **Microsoft. COMPUTE. SizeSelector** yalnızca adımlarda kullanılabilir. Genellikle, temel olarak yalnızca **Microsoft. Common** ad alanındaki öğeler kullanılabilir. Diğer ad alanlarında ( **Microsoft. COMPUTE. Credentials**gibi), kullanıcının bağlamına bağımlı olmayan bazı öğelere izin verilse de.

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

## <a name="functions"></a>İşlevler
Createuıdefinition, öğelerin girdileri ve çıkışları ve conditionals gibi özelliklerle çalışmak için [işlevler](create-uidefinition-functions.md) sağlar. Bu işlevler, Azure Resource Manager şablon işlevleri için hem söz dizimi hem de işlevselliğe benzer.

## <a name="next-steps"></a>Sonraki adımlar
Createuıdefinition. json dosyasının kendisi basit bir şemaya sahiptir. Gerçek derinliği, desteklenen tüm öğe ve işlevlerden gelir. Bu öğeler, daha ayrıntılı olarak açıklanmıştır:

- [Ög](create-uidefinition-elements.md)
- [İşlevler](create-uidefinition-functions.md)

Createuıdefinition için geçerli bir JSON şeması şurada bulunabilir: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Örnek bir kullanıcı arabirimi dosyası için bkz. [Createuıdefinition. JSON](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).
