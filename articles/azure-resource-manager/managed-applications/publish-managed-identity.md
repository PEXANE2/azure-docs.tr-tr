---
title: Yönetilen Kimlikle Yönetilen uygulama
description: Yönetilen Uygulamayı, varolan kaynaklara bağlanmak, Azure kaynaklarını yönetmek ve Etkinlik Günlüğü için operasyonel kimlik sağlamak için Yönetilen Kimlikle yapılandırın.
ms.topic: conceptual
ms.author: jobreen
author: jjbfour
ms.date: 05/13/2019
ms.openlocfilehash: dbf75262440474c5cb50a6d733ac7cba212b5f3f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75651663"
---
# <a name="azure-managed-application-with-managed-identity"></a>Yönetilen Kimlikle Azure Yönetilen Uygulama

> [!NOTE]
> Yönetilen Uygulamalar için Yönetilen Kimlik desteği şu anda önizlemededir. Yönetilen Kimlik kullanmak için lütfen 2018-09-01 önizleme api sürümünü kullanın.

Yönetilen Bir Kimlik içerecek şekilde Yönetilen Uygulamayı nasıl yapılandıracaksınız öğrenin. Yönetilen Kimlik, müşterinin Yönetilen Uygulama'ya varolan ek kaynaklara erişim izni vermesi için kullanılabilir. Kimlik Azure platformu tarafından yönetilir ve herhangi bir sırrı sağlamanızı veya döndürmenizi gerektirmez. Azure Etkin Dizini'nde (AAD) yönetilen kimlikler hakkında daha fazla şey için Azure [kaynakları için Yönetilen kimlikler'e](../../active-directory/managed-identities-azure-resources/overview.md)bakın.

Başvurunuza iki tür kimlik verilebilir:

- **Sistemle atanmış bir kimlik** uygulamanıza bağlıdır ve uygulamanız silinirse silinir. Bir uygulamayalnızca bir sistem tarafından atanmış kimliğe sahip olabilir.
- **Kullanıcı tarafından atanan kimlik,** uygulamanıza atanabilecek bağımsız bir Azure kaynağıdır. Bir uygulamanın birden çok kullanıcı tarafından atanmış kimliği olabilir.

## <a name="how-to-use-managed-identity"></a>Yönetilen Kimlik nasıl kullanılır?

Yönetilen Kimlik, Yönetilen Uygulamalar için birçok senaryosağlar. Çözülebilecek bazı yaygın senaryolar şunlardır:

- Varolan Azure kaynaklarına bağlı yönetilen bir uygulama dağıtma. Buna örnek olarak, Yönetilen Uygulama içinde varolan bir [ağ arabirimine](../../virtual-network/virtual-network-network-interface-vm.md)bağlı bir Azure sanal makinesi (VM) dağıtılıyor.
- Yönetilen Uygulama'ya ve yayımcıya **yönetilen kaynak grubunun**dışındaki Azure kaynaklarına erişim izni verilmesi.
- Azure'da Etkinlik Günlüğü için Yönetilen Uygulamalar ve diğer hizmetlerin operasyonel kimliğini sağlama.

## <a name="adding-managed-identity"></a>Yönetilen Kimlik Ekleme

Yönetilen Kimlikle Yönetilen Uygulama oluşturmak için Azure kaynağında ayarlanması gereken ek bir özellik gerekir. Aşağıdaki örnek bir **kimlik** özelliği ni gösterir:

```json
{
"identity": {
    "type": "SystemAssigned, UserAssigned",
    "userAssignedIdentities": {
        "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity": {}
    }
}
```

**Kimlikle**Yönetilen Uygulama oluşturmanın iki yaygın yolu vardır: [CreateUIDefinition.json](./create-uidefinition-overview.md) ve [Azure Kaynak Yöneticisi şablonları.](../templates/template-syntax.md) Basit tek oluşturma senaryoları için CreateUIDefinition, daha zengin bir deneyim sağladığından Yönetilen Kimlik'i etkinleştirmek için kullanılmalıdır. Ancak, otomatik veya birden çok Yönetilen Uygulama dağıtımı gerektiren gelişmiş veya karmaşık sistemlerle uğraşırken şablonlar kullanılabilir.

### <a name="using-createuidefinition"></a>CreateUiDefinition kullanma

Yönetilen Bir Uygulama [CreateUIDefinition.json](./create-uidefinition-overview.md)aracılığıyla Yönetilen Kimlik ile yapılandırılabilir. Çıktılar [bölümünde,](./create-uidefinition-overview.md#outputs)anahtar `managedIdentity` Yönetilen Uygulama şablonunun kimlik özelliğini geçersiz kılmak için kullanılabilir. Örnek körük, Yönetilen Uygulama'da **sistem tarafından atanmış** bir kimlik sağlar. Daha karmaşık kimlik nesneleri, tüketiciden girdi istemek için CreateUIDefinition öğeleri kullanılarak oluşturulabilir. Bu girişler, kullanıcı tarafından atanan **kimlikle**Yönetilen Uygulamalar'ı oluşturmak için kullanılabilir.

```json
"outputs": {
    "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
}
```

#### <a name="when-to-use-createuidefinition-for-managed-identity"></a>Yönetilen Kimlik için CreateUIDefinition ne zaman kullanılır?

Yönetilen Uygulamalarda Yönetilen Kimliği etkinleştirmek için CreateUIDefinition'ın ne zaman kullanılacağına ilişkin bazı öneriler aşağıda verilmiştir.

- Yönetilen Uygulama oluşturma, Azure portalı ndan veya pazartarafından geçer.
- Yönetilen Kimlik karmaşık tüketici girişi gerektirir.
- Yönetilen Uygulama'nın oluşturulmasında Yönetilen Kimlik gereklidir.

#### <a name="systemassigned-createuidefinition"></a>SystemAssigned CreateUIDefinition

Yönetilen Uygulama için SystemAssigned kimliğini etkinleştiren temel bir CreateUIDefinition.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
        ],
        "outputs": {
            "managedIdentity": "[parse('{\"Type\":\"SystemAssigned\"}')]"
        }
    }
}
```

#### <a name="userassigned-createuidefinition"></a>Kullanıcı Atanmış CreateUiDefinition

**Kullanıcı tarafından atanan kimlik** kaynağını girdi olarak alan ve Yönetilen Uygulama için UserAssigned kimliğini etkinleştiren temel bir CreateUIDefinition.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
  "handler": "Microsoft.Azure.CreateUIDef",
  "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "manageIdentity",
                "label": "Identity",
                "subLabel": {
                    "preValidation": "Manage Identities",
                    "postValidation": "Done"
                },
                "bladeTitle": "Identity",
                "elements": [
                    {
                        "name": "userAssignedText",
                        "type": "Microsoft.Common.TextBox",
                        "label": "User assigned managed identity",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('manageIdentity').userAssignedText),':{}}}'))]"
        }
    }
}
```

Yukarıdaki CreateUIDefinition.json, bir tüketicinin kullanıcı tarafından **atanan kimlik** Azure kaynak kimliğini girebilen bir textbox'ı olan bir kullanıcı oluşturma deneyimi oluşturur. Oluşturulan deneyim gibi görünecek:

![Örnek kullanıcı tarafından atanan kimlik CreateUIDefinition](./media/publish-managed-identity/user-assigned-identity.png)

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

> [!NOTE]
> Azure portalından geçen müşteriler için Pazar Yeri Yönetilen Uygulama şablonları otomatik olarak oluşturulur.
> Bu senaryolar için `managedIdentity` CreateUIDefinition'daki çıktı anahtarı etkin kimlik için kullanılmalıdır.

Yönetilen Kimlik, Azure Kaynak Yöneticisi şablonları aracılığıyla da etkinleştirilebilir. Örnek körük, Yönetilen Uygulama'da **sistem tarafından atanmış** bir kimlik sağlar. Girişleri sağlamak için Azure Kaynak Yöneticisi şablon parametreleri kullanılarak daha karmaşık kimlik nesneleri oluşturulabilir. Bu girişler, kullanıcı tarafından atanan **kimlikle**Yönetilen Uygulamalar'ı oluşturmak için kullanılabilir.

#### <a name="when-to-use-azure-resource-manager-templates-for-managed-identity"></a>Yönetilen Kimlik için Azure Kaynak Yöneticisi şablonları ne zaman kullanılır?

Yönetilen Uygulamalarda Yönetilen Kimliği etkinleştirmek için Azure Kaynak Yöneticisi şablonlarının ne zaman kullanılacağına ilişkin bazı öneriler aşağıda verilmiştir.

- Yönetilen Uygulamalar bir şablona dayalı olarak programlanabilir.
- Yönetilen Uygulama'yı sağlamak için Yönetilen Kimlik için özel rol atamaları gereklidir.
- Yönetilen Uygulama'nın Azure portalına ve pazar yeri oluşturma akışına ihtiyacı yoktur.

#### <a name="systemassigned-template"></a>SystemAssigned şablonu

**Sistem tarafından atanmış** kimliğe sahip yönetilen bir uygulama dağıtan temel bir Azure Kaynak Yöneticisi şablonu.

```json
"resources": [
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "SystemAssigned"
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

### <a name="userassigned-template"></a>UserAssigned şablonu

Yönetilen Uygulamayı kullanıcı tarafından atanmış bir **kimliğe**göre dağıtan temel bir Azure Kaynak Yöneticisi şablonu.

```json
"resources": [
    {
      "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
      "name": "[parameters('managedIdentityName')]",
      "apiVersion": "2018-11-30",
      "location": "[parameters('location')]"
    },
    {
        "type": "Microsoft.Solutions/applications",
        "name": "[parameters('applicationName')]",
        "apiVersion": "2018-09-01-preview",
        "location": "[parameters('location')]",
        "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
                "[resourceID('Microsoft.ManagedIdentity/userAssignedIdentities/',parameters('managedIdentityName'))]": {}
            }
        },
        "properties": {
            "ManagedResourceGroupId": "[parameters('managedByResourceGroupId')]",
            "parameters": { }
        }
    }
]
```

## <a name="granting-access-to-azure-resources"></a>Azure kaynaklarına erişim verme

Yönetilen Uygulama'ya bir kimlik verildikten sonra, varolan azure kaynaklarına erişim izni verilebilir. Bu işlem, Azure portalındaki Access denetimi (IAM) arabirimi üzerinden yapılabilir. Yönetilen Uygulama'nın adı veya **kullanıcı tarafından atanan kimlik,** rol ataması eklemek için aranabilir.

![Yönetilen Uygulama için rol ataması ekleme](./media/publish-managed-identity/identity-role-assignment.png)

## <a name="linking-existing-azure-resources"></a>Varolan Azure kaynaklarını bağlama

> [!NOTE]
> Yönetilen Uygulama'yı dağıtmadan önce **kullanıcı tarafından atanan bir kimliğin** [yapılandırılması](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) gerekir. Buna ek olarak, Yönetilen Uygulamalar bağlantılı kaynak dağıtımı yalnızca **pazar türü** için desteklenir.

Yönetilen Kimlik, dağıtımı sırasında varolan kaynaklara erişim gerektiren yönetilen bir uygulama dağıtmak için de kullanılabilir. Yönetilen Uygulama müşteri tarafından sağlandığında, **ana Template** dağıtımına ek yetkilendirmesağlamak için kullanıcı tarafından **atanan kimlikler** eklenebilir.

### <a name="authoring-the-createuidefinition-with-a-linked-resource"></a>CreateUIDefinition'ı bağlantılı bir kaynakla yazma

Yönetilen Uygulama'nın dağıtımını varolan kaynaklara bağlarken, hem varolan Azure kaynağı hem de kullanıcı **tarafından atanan** bir kimlik ve bu kaynaktaki geçerli rol ataması sağlanmalıdır.

 İki giriş gerektiren bir örnek CreateUIDefinition: bir ağ arabirimi kaynak kimliği ve kullanıcı tarafından atanan kimlik kaynağı kimliği.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "managedApplicationSetting",
                "label": "Managed Application Settings",
                "subLabel": {
                    "preValidation": "Managed Application Settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Managed Application Settings",
                "elements": [
                    {
                        "name": "networkInterfaceId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "network interface resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Network/networkInterfaces/existingnetworkinterface",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.Network/networkInterfaces/networkinterface1",
                        "visible": true
                    },
                    {
                        "name": "userAssignedId",
                        "type": "Microsoft.Common.TextBox",
                        "label": "user assigned identity resource id",
                        "defaultValue": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.ManagedIdentity/userassignedidentites/myuserassignedidentity",
                        "toolTip": "Must represent the identity as an Azure Resource Manager resource identifer format ex. /subscriptions/sub1/resourcegroups/myGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/identity1",
                        "visible": true
                    }
                ]
            }
        ],
        "outputs": {
            "existingNetworkInterfaceId": "[steps('managedApplicationSetting').networkInterfaceId]",
            "managedIdentity": "[parse(concat('{\"Type\":\"UserAssigned\",\"UserAssignedIdentities\":{',string(steps('managedApplicationSetting').userAssignedId),':{}}}'))]"
        }
    }
}
```

Bu CreateUIDefinition.json iki alanı olan bir oluşturma kullanıcı deneyimi oluşturur. İlk alan, yönetilen uygulama dağıtımına bağlanan kaynak için kullanıcının Azure kaynak kimliğine girmesini sağlar. İkincisi, bir tüketicinin bağlı Azure kaynağına erişimi olan kullanıcı tarafından **atanan kimlik** Azure kaynak kimliğini girmesidir. Oluşturulan deneyim gibi görünecek:

![Örnek CreateUIDefinition iki girişi ile: bir ağ arabirimi kaynak kimliği ve bir kullanıcı kimlik kaynak kimliği atanmış](./media/publish-managed-identity/network-interface-cuid.png)

### <a name="authoring-the-maintemplate-with-a-linked-resource"></a>MainTemplate'i bağlantılı bir kaynakla yazma

CreateUIDefinition güncelleştirmeye ek olarak, ana şablon da bağlı kaynak kimliği geçti kabul etmek için güncelleştirilmesi gerekir. Ana şablon, yeni bir parametre ekleyerek yeni çıktıyı kabul etmek üzere güncellenebilir. `managedIdentity` Çıktı, oluşturulan Yönetilen Uygulama şablonundaki değeri geçersiz kustun, ana şablona geçirilmez ve parametreler bölümüne dahil edilmemelidir.

Ağ profilini CreateUIDefinition tarafından sağlanan varolan bir ağ arabirimine ayarlayan örnek bir ana şablon.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "existingNetworkInterfaceId": { "type": "string" }
    },
    "variables": {
    },
    "resources": [
        {
            "apiVersion": "2016-04-30-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "myLinkedResourceVM",
            "location": "[resourceGroup().location]",
            "properties": {
                …,
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[parameters('existingNetworkInterfaceId')]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="consuming-the-managed-application-with-a-linked-resource"></a>Yönetilen Uygulamayı bağlantılı bir kaynakla tüketme

Yönetilen Uygulama paketi oluşturulduktan sonra Yönetilen Uygulama Azure portalı üzerinden tüketilebilir. Tüketilmeden önce, birkaç ön koşul adımı vardır.

- Gerekli bağlantılı Azure kaynağının bir örneği oluşturulmalıdır.
- **Kullanıcı tarafından atanan kimlik** oluşturulmalı ve bağlı kaynağa rol atamaları [verilmelidir.](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
- Varolan bağlantılı kaynak kimliği ve **kullanıcı tarafından atanan kimlik** kimliği CreateUIDefinition'a sağlanır.

## <a name="accessing-the-managed-identity-token"></a>Yönetilen Kimlik belirtecine erişme

Yönetilen Uygulama'nın belirteci artık yayıncı `listTokens` kiracıdan api üzerinden erişilebilir. Örnek bir istek şu şekilde görünebilir:

``` HTTP
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}/listTokens?api-version=2018-09-01-preview HTTP/1.1

{
    "authorizationAudience": "https://management.azure.com/",
    "userAssignedIdentities": [
        "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.ManagedIdentity/userAssignedIdentities/{userAssignedIdentityName}"
    ]
}
```

İstek Gövde Parametreleri:

Parametre | Gerekli | Açıklama
---|---|---
yetkilendirmeHedef | *hayır* | Hedef kaynağın Uygulama Kimliği URI. Ayrıca verilen `aud` belirteç (hedef kitle) iddiasıdır. Varsayılan değer "https://management.azure.com/"
kullanıcıAssignedIdentities | *hayır* | Bir belirteç almak için kullanıcı tarafından atanan yönetilen kimliklerin listesi. Belirtilmemişse, `listTokens` sistem tarafından atanan yönetilen kimlik için belirteci döndürecek.


Örnek bir yanıt şu şekilde görünebilir:

``` HTTP
HTTP/1.1 200 OK
Content-Type: application/json

{
    "value": [
        {
            "access_token": "eyJ0eXAi…",
            "expires_in": "2…",
            "expires_on": "1557…",
            "not_before": "1557…",
            "authorizationAudience": "https://management.azure.com/",
            "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroup}/providers/Microsoft.Solutions/applications/{applicationName}",
            "token_type": "Bearer"
        }
    ]
}
```

Yanıt, özelliğin `value` altında bir dizi belirteç içerir:

Parametre | Açıklama
---|---
access_token | İstenen erişim jetonu.
expires_in | Erişim belirteci nin saniye sayısı geçerli olacaktır.
expires_on | Erişim belirteci süresi dolduğunda zaman aşımı. Bu, çağa ait saniye sayısı olarak temsil edilir.
not_before | Erişim belirteci etkin olduğunda zaman alanı. Bu, çağa ait saniye sayısı olarak temsil edilir.
yetkilendirmeHedef | Erişim `aud` belirteci (hedef kitle) için istek edildi. `listTokens` Bu, istekte sağlananla aynıdır.
resourceId | Verilen belirteç için Azure kaynak kimliği. Bu, yönetilen uygulama kimliği veya kullanıcı tarafından atanan kimlik kimliğidir.
token_type | Belirtecin türü.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yönetilen Bir Uygulama'yı Özel Sağlayıcıyla yapılandırma](../custom-providers/overview.md)
