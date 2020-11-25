---
title: En son API’lerle Microsoft İş Ortağı Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturma
description: REST API, Azure CLI ve Azure PowerShell’in en son sürümlerini kullanarak Microsoft İş Ortağı Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturmayı öğrenin.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: ab5e51186fba7fc0dc8264026d314c32ce81d73f
ms.sourcegitcommit: 0a9df8ec14ab332d939b49f7b72dea217c8b3e1e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2020
ms.locfileid: "94850989"
---
# <a name="programmatically-create-azure-subscriptions-for-a-microsoft-partner-agreement-with-the-latest-apis"></a>En son API’lerle Microsoft İş Ortağı Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturma

Bu makale, en son API sürümlerini kullanarak Microsoft İş Ortağı Sözleşmesi için program aracılığıyla Azure abonelikleri oluşturmanıza yardımcı olur. Hala eski önizleme sürümünü kullanıyorsanız [Önizleme API’leriyle program aracılığıyla Azure abonelikleri oluşturma](programmatically-create-subscription-preview.md) başlıklı makaleye bakın. 

Bu makalede Azure Resource Manager'ı kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Program aracılığıyla Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft'tan veya yetkili satıcıdan aldığınız Azure hizmetlerinin tabi olduğu sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure Yasal Bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Fatura hesabınıza abonelik oluşturmak için kuruluşunuzun bulut çözüm sağlayıcısı hesabında Genel Yönetici veya Yönetici Aracısı rolünüz olmalıdır. Daha fazla bilgi için bkz. [İş Ortağı Merkezi - Kullanıcı rollerini ve izinleri atama](/partner-center/permissions-overview).

Bir Microsoft İş Ortağı Sözleşmesi hesabına erişiminizin olup olmadığını bilmiyorsanız bkz. [Microsoft İş Ortağı Sözleşmesi’ne erişimi denetleme](mpa-request-ownership.md#check-access-to-a-microsoft-partner-agreement).

Aşağıdaki örneklerde REST API’leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

## <a name="find-the-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan fatura hesaplarını bulma

Erişiminiz olan tüm fatura hesaplarını listelemek için aşağıdaki isteği gönderin.

### <a name="rest"></a>[REST](#tab/rest-getBillingAccount-MPA)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01
```

API yanıtında fatura hesapları listelenir.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountStatus": "Active",
        "accountType": "Partner",
        "agreementType": "MicrosoftPartnerAgreement",
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "hasReadAccess": true
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}
```

Abonelikleri oluşturmak istediğiniz fatura hesabını belirlemek için `displayName` özelliğini kullanın. Hesabın agreementType ayarının *MicrosoftPartnerAgreement* olduğundan emin olun. Hesap için `name` değerini kopyalayın. Örneğin `Contoso` faturalama hesabı için bir abonelik oluşturmak amacıyla `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` değerini kopyalayın. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

<!--
### [PowerShell](#tab/azure-powershell-getBillingAccounts-MPA)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getBillingAccounts-MPA)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="find-customers-that-have-azure-plans"></a>Azure planları olan müşterileri bulma

Azure abonelikleri oluşturabileceğiniz fatura hesabındaki tüm müşterileri listelemek için ilk adımda (```99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) kopyaladığınız `name` ile aşağıdaki isteği gönderin.

### <a name="rest"></a>[REST](#tab/rest-getCustomers)

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers?api-version=2020-05-01
```

API yanıtında, fatura hesabında Azure planları olan müşteriler listelenir. Bu müşteriler için abonelik oluşturabilirsiniz.

```json
{
  "totalCount": 2,
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Fabrikam toys Billing Profile",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
        "displayName": "Fabrikam toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "acba85c9-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Contoso toys Billing Profile",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
        "displayName": "Contoso toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    }
  ]
}

```

Abonelikleri oluşturmak istediğiniz müşteriyi belirlemek için `displayName` özelliğini kullanın. Müşteri için `id` değerini kopyalayın. Örneğin, `Fabrikam toys` için bir abonelik oluşturmak amacıyla `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/7d15644f-xxxx-xxxx-xxxx-xxxxxxxxxxxx` değerini kopyalayın. Sonraki adımlarda kullanabilmeniz için bu değeri yapıştırın.

<!--
### [PowerShell](#tab/azure-powershell-getCustomers)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getCustomers)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="get-the-resellers-for-a-customer"></a>Müşterinin kurumsal bayilerini alma

Bu bölüm yalnızca Dolaylı sağlayıcılar için isteğe bağlıdır.

CSP iki katmanlı modelinde bir Dolaylı sağlayıcıysanız, müşteriler için abonelik oluştururken bir kurumsal bayi belirtebilirsiniz.

Müşterinin kullanılabilir tüm kurumsal bayilerini listelemek için, ikinci adımda (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopyaladığınız `id` ile aşağıdaki isteği gönderin.

### <a name="rest"></a>[REST](#tab/rest-getIndirectResellers)

```json
GET "https://management.azure.com/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx?$expand=resellers&api-version=2020-05-01"
```
API yanıtında müşterinin kurumsal bayileri listelenir:

```json
{
  "value": [{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "billingProfileDisplayName": "Fabrikam toys Billing Profile",
    "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/YL4M-xxxx-xxx-xxx",
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "3xxxxx",
        "description": "Wingtip"
      }
    ]
  }
}]
}
```

Abonelikle ilişkili kurumsal bayiyi belirlemek için `description` özelliğini kullanın. Kurumsal bayi için `resellerId` değerini kopyalayın. Örneğin, `Wingtip` adlı bayiyi ilişkilendirmek için `3xxxxx` değerini kopyalayın. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

<!--
### [PowerShell](#tab/azure-powershell-getIndirectResellers)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.
-->

<!--
### [Azure CLI](#tab/azure-cli-getIndirectResellers)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="create-a-subscription-for-a-customer"></a>Müşteriden abonelik oluşturma

Aşağıdaki örnekte *Fabrikam toys* için *Dev Team subscription* adlı bir abonelik oluşturulur ve *Wingtip* kurumsal bayisi abonelikle ilişkilendirilir. Önceki adımda kopyalanan faturalama kapsamını kullanırsınız: `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx`. 

### <a name="rest"></a>[REST](#tab/rest-MPA)

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01
```

### <a name="request-body"></a>İstek gövdesi

```json
{
  "properties":
    {
        "billingScope": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
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

İkinci adımda kopyaladığınız isteğe bağlı *resellerId* değerini API’nin istek gövdesine geçirin.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-MPA)

Modülün `New-AzSubscriptionAlias` cmdlet'ini içeren en son sürümünü yüklemek için `Install-Module Az.Subscription` çalıştırın. PowerShellGet'in yeni bir sürümünü yüklemek için [Get PowerShellGet Modülü](/powershell/scripting/gallery/installing-psget)'ne bakın.

`"/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` faturalama kapsamını kullanarak aşağıdaki [New-AzSubscriptionAlias](/powershell/module/az.subscription/new-azsubscription) komutunu çalıştırın. 

```azurepowershell-interactive
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -Workload 'Production"
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

`New-AzSubscriptionAlias` çağrısındaki ikinci adımda kopyaladığınız isteğe bağlı *resellerId* değerini geçirin.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli-MPA)

İlk olarak, `az extension add --name account` ve `az extension add --name alias` komutlarını çalıştırarak uzantıyı yükleyin.

Aşağıdaki [az account alias create](/cli/azure/ext/account/account/alias?view=azure-cli-latest#ext_account_az_account_alias_create&preserve-view=true) komutunu çalıştırın. 

```azurecli-interactive
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx" --display-name "Dev Team Subscription" --workload "Production"
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

`az account alias create` çağrısındaki ikinci adımda kopyaladığınız isteğe bağlı *resellerId* değerini geçirin.

---

## <a name="next-steps"></a>Sonraki adımlar

* Artık aboneliği oluşturduğunuza göre, bu beceriyi diğer kullanıcılara ve hizmet sorumlularına verebilirsiniz. Daha fazla bilgi için bkz. [Azure Kurumsal abonelikleri oluşturma erişimi verme (önizlemi)](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok fazla sayıda aboneliği yönetme hakkında daha fazla bilgi için bkz. [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../governance/management-groups/overview.md).
