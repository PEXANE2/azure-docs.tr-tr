---
title: Azure Resource Manager şablonunu kullanarak Azure aboneliklerini program aracılığıyla oluşturma
description: Azure Resource Manager şablonunu kullanarak Azure aboneliklerini program aracılığıyla oluşturmayı öğrenin.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 92dd129444800dc44e4418fb12d2e4df4aebc02d
ms.sourcegitcommit: e7179fa4708c3af01f9246b5c99ab87a6f0df11c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/30/2020
ms.locfileid: "97826145"
---
# <a name="programmatically-create-azure-subscriptions-with-an-azure-resource-manager-template"></a>Azure Resource Manager şablonunu kullanarak Azure aboneliklerini program aracılığıyla oluşturma

Bu makale, bir Azure Resource Manager şablonu (ARM şablonu) kullanarak Azure aboneliklerini program aracılığıyla oluşturmanıza yardımcı olur. Aşağıdaki sözleşme türlerine yönelik faturalama hesabına sahip Azure müşterileri ARM şablonunu kullanarak abonelikler oluşturabilir:

- Kurumsal Anlaşma (EA)
    - Daha fazla bilgi için bkz. [Azure Kurumsal REST API’leri](ea-portal-rest-apis.md)
- Microsoft Müşteri Sözleşmesi (MCA)
    - [Azure Faturalandırma REST API’si](/rest/api/billing)
    - [Azure Abonelik API’leri](/rest/api/subscription)
- Microsoft İş Ortağı Sözleşmesi (MPA)
    - [Azure Faturalandırma REST API’si](/rest/api/billing)
    - [Azure Abonelik API’leri](/rest/api/subscription)

Şablon kullanarak program aracılığıyla Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft'tan veya yetkili satıcıdan aldığınız Azure hizmetlerinin tabi olduğu sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure Yasal Bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="create-subscriptions-using-arm-templates"></a>ARM şablonlarını kullanarak abonelik oluşturma

Üretim/test dağıtım işlemlerinizi otomatikleştirmenizi sağlayan Azure Resource Manager şablonuyla (ARM şablonu) abonelikler oluşturabilirsiniz. Aşağıdaki örnekte, bir Azure aboneliği ve Azure kaynak grubu oluşturmak için ARM şablonu kullanırsınız.

## <a name="prerequisites"></a>Ön koşullar

Abonelik oluşturmak için aşağıdaki rollerden birine sahip olmanız gerekir: 

- Fatura Bölümündeki Azure Abonelik Sahibi
- Fatura Bölümündeki Azure Abonelik Katkıda Bulunanı
- Fatura Bölümündeki Azure Abonelik Oluşturucusu rolü
- Faturalama Profili veya Faturalama Hesabındaki Azure Abonelik Sahibi
- Faturalama Profili veya Faturalama Hesabındaki Azure Abonelik Katkıda Bulunanı Rolü

Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](understand-mca-roles.md#subscription-billing-roles-and-tasks).

Ayrıca, ARM şablon dağıtımı yaptığınızdan kök nesnesinde yazma izinlerine sahip olmanız gerekir. Bir yönetim grubu kapsamında ARM dağıtımı oluşturmak için, yönetim grubunda yazma izinleriniz olmalıdır. Eylem sadece bir ARM dağıtımı oluşturmaktır. Abonelik oluşturulursa yalnızca ARM şablonunda belirtilen yönetim grubunda oluşturulur.

Aşağıdaki örneklerde REST API’leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

## <a name="find-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan fatura hesaplarını bulma

Tüm faturalama hesaplarını listelemek için aşağıdaki isteği gönderin.

### <a name="rest"></a>[REST](#tab/rest-getBillingAccounts)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01
```

API yanıtında erişiminiz olan fatura hesapları listelenir.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "hasReadAccess": false
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}
```

Abonelikleri oluşturmak istediğiniz fatura hesabını belirlemek için `displayName` özelliğini kullanın. Hesabın agreementType ayarının *MicrosoftCustomerAgreement* olduğundan emin olun. Hesabın `name` değerini kopyalayın. Örneğin, `Contoso` faturalama hesabı için bir abonelik oluşturmak amacıyla `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` değerini kopyalayın. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

<!--
### [PowerShell](#tab/azure-powershell-getBillingAccounts)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingAccounts)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="find-billing-profiles--invoice-sections-to-create-subscriptions"></a>Abonelik oluşturmak için faturalama profillerini ve fatura bölümlerini bulma

Aboneliğinizin ücretleri, faturalama profilinin faturasının bir bölümünde gösterilir. Azure aboneliklerini oluşturma izniniz olan faturalama profillerinin ve fatura bölümlerinin listesini almak için aşağıdaki API’yi kullanın.

İlk olarak, erişiminiz olan faturalama hesabındaki faturalama profillerinin listesini alırsınız.

### <a name="rest"></a>[REST](#tab/rest-getBillingProfiles)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingprofiles/?api-version=2020-05-01
```

API yanıtında, abonelik oluşturma erişiminiz olan tüm faturalama profilleri listelenir:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx",
      "name": "AW4F-xxxx-xxx-xxx",
      "properties": {
        "billingRelationshipType": "Direct",
        "billTo": {
          "addressLine1": "One Microsoft Way",
          "city": "Redmond",
          "companyName": "Contoso",
          "country": "US",
          "email": "kenny@contoso.com",
          "phoneNumber": "425xxxxxxx",
          "postalCode": "98052",
          "region": "WA"
        },
        "currency": "USD",
        "displayName": "Contoso Billing Profile",
        "enabledAzurePlans": [
          {
            "skuId": "0002",
            "skuDescription": "Microsoft Azure Plan for DevTest"
          },
          {
            "skuId": "0001",
            "skuDescription": "Microsoft Azure Plan"
          }
        ],
        "hasReadAccess": true,
        "invoiceDay": 5,
        "invoiceEmailOptIn": false,
        "invoiceSections": {
          "hasMoreResults": false
        },
        "poNumber": "001",
        "spendingLimit": "Off",
        "status": "Active",
        "systemId": "AW4F-xxxx-xxx-xxx",
        "targetClouds": []
      },
      "type": "Microsoft.Billing/billingAccounts/billingProfiles"
    }
  ]
}
```

 Daha sonra, faturalama profilinin altındaki fatura bölümlerini belirlemek için `id` değerini kopyalayın. Örneğin, `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx` değerini kopyalayın ve aşağıdaki API’yi çağırın.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoicesections?api-version=2020-05-01
```

### <a name="response"></a>Yanıt

```json
{
  "totalCount": 1,
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx",
      "name": "SH3V-xxxx-xxx-xxx",
      "properties": {
        "displayName": "Development",
        "state": "Active",
        "systemId": "SH3V-xxxx-xxx-xxx"
      },
      "type": "Microsoft.Billing/billingAccounts/billingProfiles/invoiceSections"
    }
  ]
}
```

Abonelikleri oluşturmak istediğiniz fatura bölümünü belirlemek için `id` özelliğini kullanın. Dizenin tamamını kopyalayın. Örneğin, `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

<!--
### [PowerShell](#tab/azure-powershell-getBillingProfiles)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.

### [Azure CLI](#tab/azure-cli-getBillingProfiles)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="create-a-subscription-and-resource-group-with-a-template"></a>Şablonla bir abonelik ve kaynak grubu oluşturma

Aşağıdaki ARM şablonunda, *Geliştirme* fatura bölümü için *Geliştirme Ekibi aboneliği* adlı bir abonelik oluşturulur. Abonelik *Contoso Faturalama Profiline* faturalanır ve faturanın *Geliştirme* bölümünde görüntülenir. Önceki adımda kopyalanan faturalama kapsamını kullanırsınız: `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

### <a name="request"></a>İstek

```rest
PUT https://management.azure.com/providers/Microsoft.Resources/deployments/sampleTemplate?api-version=2019-10-01
```

### <a name="request-body"></a>İstek gövdesi

```json
{
  "properties":
    {
      "location": "westus",
      "properties": {
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {
            "uniqueAliasName": "sampleAlias"
          },
          "resources": [
            {
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2019-10-01",
              "name": "sampleTemplate",
              "location": "westus",
              "properties": {
                "expressionEvaluationOptions": {
                  "scope": "inner"
                },
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                  "contentVersion": "1.0.0.0",
                  "variables": {
                    "uniqueAliasName": "sampleAlias"
                  },
                  "resources": [
                    {
                      "name": "[variables('uniqueAliasName')]",
                      "type": "Microsoft.Subscription/aliases",
                      "apiVersion": "2020-09-01",
                      "properties": {
                        "workLoad": "Production",
                        "displayName": "Dev Team subscription",
                        "billingScope": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx"
                      },
                      "dependsOn": [],
                      "tags": {}
                    }
                  ],
                  "outputs": {
                    "subscriptionId": {
                      "type": "string",
                      "value": "[replace(reference(variables('uniqueAliasName')).subscriptionId, 'invalidrandom/', '')]"
                    }
                  }
                }
              }
            },
            {
              "name": "sampleOuterResource",
              "type": "Microsoft.Resources/deployments",
              "apiVersion": "2019-10-01",
              "location": "westus",
              "properties": {
                "expressionEvaluationOptions": {
                  "scope": "inner"
                },
                "mode": "Incremental",
                "parameters": {
                  "subscriptionId": {
                    "value": "[reference('sampleTemplate').outputs.subscriptionId.value]"
                  }
                },
                "template": {
                  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
              "contentVersion": "1.0.0.0",
                  "parameters": {
                    "subscriptionId": {
                      "type": "string"
                    }
                  },
                  "variables": {},
                  "resources": [
                    {
                      "name": "sampleInnerResource",
                      "type": "Microsoft.Resources/deployments",
                      "subscriptionId": "[parameters('subscriptionId')]",
                      "apiVersion": "2019-10-01",
                      "location": "westus",
                      "properties": {
                        "expressionEvaluationOptions": {
                          "scope": "inner"
                        },
                        "mode": "Incremental",
                        "parameters": {},
                        "template": {
                          "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                          "contentVersion": "1.0.0.0",
                          "parameters": {},
                          "variables": {},
                          "resources": [
                            {
                              "type": "Microsoft.Resources/resourceGroups",
                              "apiVersion": "2020-05-01",
                              "location": "[deployment().location]",
                              "name": "sampleRG",
                              "properties": {},
                              "tags": {}
                            }
                          ],
                          "outputs": {}
                        }
                      }
                    }
                  ],
                  "outputs": {}
                }
              }
            }
          ],
          "outputs": {
            "messageFromLinkedTemplate": {
              "type": "string",
              "value": "[reference('sampleTemplate').outputs.subscriptionId.value]"
            }
          }
        },
        "mode": "Incremental"
      }
    }
}
```

### <a name="response"></a>Yanıt

```json
{
  "id": "/providers/Microsoft.Resources/deployments/sampleTemplate",
  "name": "sampleTemplate",
  "type": "Microsoft.Resources/deployments",
  "location": "westus",
  "properties": {
    "templateHash": "16005880870587497948",
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted",
    "timestamp": "2020-10-07T19:06:34.110811Z",
    "duration": "PT0.1345459S",
    "correlationId": "2b57ddf6-7e27-42cb-90b4-90eeccd11a28",
    "providers": [
      {
        "namespace": "Microsoft.Resources",
        "resourceTypes": [
          {
            "resourceType": "deployments",
            "locations": [
              "westus"
            ]
          }
        ]
      }
    ],
    "dependencies": [
      {
        "dependsOn": [
          {
            "id": "/providers/Microsoft.Resources/deployments/sampleTemplate",
            "resourceType": "Microsoft.Resources/deployments",
            "resourceName": "anuragTemplate1"
          }
        ],
        "id": "/providers/Microsoft.Resources/deployments/sampleOuterResource",
        "resourceType": "Microsoft.Resources/deployments",
        "resourceName": "sampleOuterResource"
      }
    ]
  }
}
```

İlerleme durumunu izlemek için dağıtımın durumunu alabilirsiniz.

```json
GET https://management.azure.com/providers/Microsoft.Resources/deployments/sampleTemplate?api-version=2019-10-01
```

### <a name="response"></a>Yanıt

```json
{
  "id": "/providers/Microsoft.Resources/deployments/sampleDeployment5",
  "name": "sampleDeployment5",
  "type": "Microsoft.Resources/deployments",
  "location": "westus",
  "properties": {
    "templateHash": "16005880870587497948",
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Succeeded",
    "timestamp": "2020-10-07T19:07:20.8007311Z",
    "duration": "PT46.824466S",
    "correlationId": "2b57ddf6-7e27-42cb-90b4-90eeccd11a28",
    "providers": [
      {
        "namespace": "Microsoft.Resources",
        "resourceTypes": [
          {
            "resourceType": "deployments",
            "locations": [
              "westus"
            ]
          }
        ]
      }
    ],
    "dependencies": [
      {
        "dependsOn": [
          {
            "id": "/providers/Microsoft.Resources/deployments/sampleTemplate",
            "resourceType": "Microsoft.Resources/deployments",
            "resourceName": "sampleTemplate"
          }
        ],
        "id": "/providers/Microsoft.Resources/deployments/sampleOuterResource",
        "resourceType": "Microsoft.Resources/deployments",
        "resourceName": "sampleOuterResource"
      }
    ],
    "outputs": {
      "messageFromLinkedTemplate": {
        "type": "String",
        "value": "16edf959-11fd-48bb-9a46-85190963ead9"
      }
    },
    "outputResources": [
      {
        "id": "/providers/Microsoft.Subscription/aliases/sampleAlias"
      },
      {
        "id": "/subscriptions/16edf959-11fd-48bb-9a46-85190963ead9/resourceGroups/sampleRG"
      }
    ]
  }
}
```

Yukarıdaki örnekte, oluşturulan aboneliğin `16edf959-11fd-48bb-9a46-85190963ead9`, oluşturulan kaynak grubunun ise `sampleRG` olduğunu görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Artık aboneliği oluşturduğunuza göre, bu beceriyi diğer kullanıcılara ve hizmet sorumlularına verebilirsiniz. Daha fazla bilgi için bkz. [Azure Kurumsal abonelikleri oluşturma erişimi verme (önizlemi)](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok fazla sayıda aboneliği yönetme hakkında daha fazla bilgi için bkz. [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../governance/management-groups/overview.md).
