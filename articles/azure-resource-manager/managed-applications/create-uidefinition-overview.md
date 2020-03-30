---
title: Portal bölmesi için CreateUiDefinition.json dosyası
description: Azure portalı için kullanıcı arabirimi tanımlarının nasıl oluşturulacak olduğunu açıklar. Azure Yönetilen Uygulamaları tanımlarken kullanılır.
author: tfitzmac
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: tomfitz
ms.openlocfilehash: 2956c76f5bec353639b39228b982db21b6932deb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294902"
---
# <a name="createuidefinitionjson-for-azure-managed-applications-create-experience"></a>Azure tarafından yönetilen uygulamanın oluşturma deneyimi için CreateUiDefinition.json

Bu belge, Azure portalının yönetilen bir uygulama oluştururken kullanıcı arabirimini tanımlamak için kullandığı **createUiDefinition.json** dosyasının temel kavramlarını tanıtır.

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

CreateUiDefinition her zaman üç özellik içerir: 

* Işleyicisi
* version
* parametreler

İşleyici her zaman `Microsoft.Azure.CreateUIDef`olmalıdır ve en son `0.1.2-preview`desteklenen sürümü .

Parametreler özelliğinin şeması, belirtilen işleyici ve sürümün birleşimine bağlıdır. Yönetilen uygulamalar için desteklenen özellikler `basics`, `steps`, `outputs`ve . Temel ve adım özellikleri, Azure portalında görüntülenecek metin kutuları ve açılır bırakmalar gibi [öğeleri](create-uidefinition-elements.md) içerir. Çıktılar özelliği, belirtilen öğelerin çıktı değerlerini Azure Kaynak Yöneticisi dağıtım şablonunun parametreleri ile eşlemek için kullanılır.

Dahil `$schema` etmek önerilir, ancak isteğe bağlıdır. Belirtilirse, değer `version` `$schema` URI içindeki sürümle eşleşmelidir.

CreateUiDefinition oluşturmak için bir JSON düzenleyicisi kullanabilirsiniz sonra önizleme için [createUiDefinition Sandbox](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) test edin. Kum havuzu hakkında daha fazla bilgi için Azure [Yönetilen Uygulamalar için portal arabiriminizi test edin.](test-createuidefinition.md)

## <a name="basics"></a>Temel Bilgiler

Temel bilgiler, Azure portalı dosyayı parşattığında oluşturulan ilk adımdır. Belirtilen öğeleri görüntülemenin yanı `basics`sıra, portal kullanıcıların dağıtım için abonelik, kaynak grubu ve konumu seçmeleri için öğeler enjekte eder. Mümkün olduğunda, bir kümenin veya yönetici kimlik bilgilerinin adı gibi dağıtım genişliğindeki parametreleri sorgulayan öğeler bu adımda gitmeli.

## <a name="steps"></a>Adımlar

Adımlar özelliği, her biri bir veya daha fazla öğe içeren temel bilgilerden sonra görüntülenecek sıfır veya daha fazla ek adım içerebilir. Dağıtılan uygulamanın rolü veya katmanı başına adımlar eklemeyi düşünün. Örneğin, ana düğüm girişleri için bir adım ve bir kümedeki alt düğümler için bir adım ekleyin.

## <a name="outputs"></a>Çıkışlar

Azure portalı, `outputs` Azure Kaynak Yöneticisi `basics` `steps` dağıtım şablonunun parametrelerini ve öğelerini eşlemek için özelliği kullanır. Bu sözlüğün anahtarları şablon parametrelerinin adlarıdır ve değerler başvurulan öğelerden çıktı nesnelerinin özellikleridir.

Yönetilen uygulama kaynak adını ayarlamak için çıktıözelliğine adlandırılmış `applicationResourceName` bir değer eklemeniz gerekir. Bu değeri ayarlamazsanız, uygulama ad için bir GUID atar. Kullanıcı arabirimine kullanıcıdan bir ad isteyen bir textbox ekleyebilirsiniz.

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

Kullanılabilir konumları yalnızca dağıtılabilmek için kaynak türlerini destekleyen konumlara filtre lemek için, kaynak türlerinin bir dizisini sağlayın. Birden fazla kaynak türü sağlarsanız, yalnızca tüm kaynak türlerini destekleyen konumlar döndürülür. Bu özellik isteğe bağlıdır.

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

CreateUiDefinition, öğelerin girdi ve çıktılarıyla çalışmak için [işlevler](create-uidefinition-functions.md) ve koşullu özellikler sağlar. Bu işlevler hem sözdiziminde hem de işlevsellikte Azure Kaynak Yöneticisi şablon işlevlerine benzer.

## <a name="next-steps"></a>Sonraki adımlar

createUiDefinition.json dosyasının kendisi basit bir şema vardır. Bunun gerçek derinliği desteklenen tüm öğeleri ve işlevleri geliyor. Bu öğeler daha ayrıntılı olarak açıklanmıştır:

- [Öğeler](create-uidefinition-elements.md)
- [Işlev](create-uidefinition-functions.md)

CreateUiDefinition için geçerli bir JSON şeması `https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json`burada mevcuttur: .

Örnek bir kullanıcı arabirimi dosyası için [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/Managed%20Application%20Sample%20Packages/201-managed-app-using-existing-vnet/createUiDefinition.json)'a bakın.
