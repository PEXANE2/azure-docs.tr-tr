---
title: En son API’lerle Microsoft Müşteri Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturma
description: REST API, Azure CLI ve Azure PowerShell’in en son sürümlerini kullanarak Microsoft Müşteri Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturmayı öğrenin.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 1b5fbd5ee2b8db784f4e35a37fe144cbe2c0abeb
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94850990"
---
# <a name="programmatically-create-azure-subscriptions-for-a-microsoft-customer-agreement-with-the-latest-apis"></a>En son API’lerle Microsoft Müşteri Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturma

Bu makale, en son API sürümlerini kullanarak Microsoft Müşteri Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturmanıza yardımcı olur. Hala eski önizleme sürümünü kullanıyorsanız [Önizleme API’leriyle program aracılığıyla Azure abonelikleri oluşturma](programmatically-create-subscription-preview.md) başlıklı makaleye bakın. 

Bu makalede Azure Resource Manager'ı kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Program aracılığıyla Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft'tan veya yetkili satıcıdan aldığınız Azure hizmetlerinin tabi olduğu sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure Yasal Bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Abonelikleri oluşturmak için bir fatura bölümünde sahip, katkıda bulunan veya Azure abonelik oluşturucu rolünüz ya da faturalama profili veya faturalama hesabında sahip veya katkıda bulunan rolünüz olmalıdır. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](understand-mca-roles.md#subscription-billing-roles-and-tasks).

Bir Microsoft Müşteri Sözleşmesi hesabına erişiminizin olup olmadığını bilmiyorsanız bkz. [Microsoft Müşteri Sözleşmesi’ne erişimi denetleme](../understand/mca-overview.md#check-access-to-a-microsoft-customer-agreement).

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

Abonelikleri oluşturmak istediğiniz fatura hesabını belirlemek için `displayName` özelliğini kullanın. Hesabın agreementType ayarının *MicrosoftCustomerAgreement* olduğundan emin olun. Hesabın `name` değerini kopyalayın.  Örneğin, `Contoso` faturalama hesabı için bir abonelik oluşturmak amacıyla `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` değerini kopyalayın. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

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
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingProfiles)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="create-a-subscription-for-an-invoice-section"></a>Fatura bölümü için aboneliği oluşturma

Aşağıdaki örnekte, *Geliştirme* fatura bölümü için *Geliştirme Ekibi aboneliği* adlı bir abonelik oluşturulur. Abonelik *Contoso Faturalama Profiline* faturalanır ve faturanın *Geliştirme* bölümünde görüntülenir. Önceki adımda kopyalanan faturalama kapsamını kullanırsınız: `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

### <a name="rest"></a>[REST](#tab/rest-MCA)

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="request-body"></a>İstek gövdesi

```json
{
  "properties":
    {
        "billingScope": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx",
        "DisplayName": "Dev Team subscription",
        "Workload": "Production"
    }
}
```

### <a name="response"></a>Yanıt

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "subscriptionId": "b5bab918-e8a9-4c34-a2e2-ebc1b75b9d74",
    "provisioningState": "Accepted"
  }
}
```

İsteğin durumunu almak için aynı URL’de bir GET işlemi de yapabilirsiniz.

### <a name="request"></a>İstek

```json
GET https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="response"></a>Yanıt

```json
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "subscriptionId": "b5bab918-e8a9-4c34-a2e2-ebc1b75b9d74",
    "provisioningState": "Succeeded"
  }
}
```

Sürüyor durumu, `provisioningState` altında `Accepted` durumu olarak döndürülür.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-MCA)

Modülün `New-AzSubscriptionAlias` cmdlet'ini içeren en son sürümünü yüklemek için `Install-Module Az.Subscription` çalıştırın. PowerShellGet'in yeni bir sürümünü yüklemek için [Get PowerShellGet Modülü](/powershell/scripting/gallery/installing-psget)'ne bakın.

`"/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx"` faturalama kapsamını ve aşağıdaki [New-AzSubscriptionAlias](/powershell/module/az.subscription/new-azsubscription) komutunu çalıştırın. 

```azurepowershell-interactive
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx" -Workload 'Production"
```

Komutun yanıtının bir parçası olarak subscriptionId bilgisini alırsınız.

```azurepowershell
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  },
  "type": "Microsoft.Subscription/aliases"
}
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli-MCA)

İlk olarak, `az extension add --name account` ve `az extension add --name alias` komutlarını çalıştırarak uzantıyı yükleyin.

Aşağıdaki [az account alias create](/cli/azure/ext/account/account/alias?view=azure-cli-latest#ext_account_az_account_alias_create&preserve-view=true) komutunu çalıştırın.

```azurecli-interactive
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx" --display-name "Dev Team Subscription" --workload "Production"
```

Komutun yanıtının bir parçası olarak subscriptionId bilgisini alırsınız.

```azurecli
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  },
  "type": "Microsoft.Subscription/aliases"
}
```

---

## <a name="next-steps"></a>Sonraki adımlar

* Artık aboneliği oluşturduğunuza göre, bu beceriyi diğer kullanıcılara ve hizmet sorumlularına verebilirsiniz. Daha fazla bilgi için bkz. [Azure Kurumsal abonelikleri oluşturma erişimi verme (önizlemi)](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok fazla sayıda aboneliği yönetme hakkında daha fazla bilgi için bkz. [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../governance/management-groups/overview.md).
