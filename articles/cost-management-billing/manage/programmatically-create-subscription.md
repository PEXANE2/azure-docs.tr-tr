---
title: En son API’lerle program aracılığıyla Azure abonelikleri oluşturma
description: REST API, Azure CLI ve Azure PowerShell’in en son sürümlerini kullanarak program aracılığıyla Azure abonelikleri oluşturmayı öğrenin.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 10/29/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 7b44abbbf2e7592205d5d5c291ce99d381a283f7
ms.sourcegitcommit: 4f4a2b16ff3a76e5d39e3fcf295bca19cff43540
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043288"
---
# <a name="programmatically-create-azure-subscriptions-with-the-latest-apis"></a>En son API’lerle program aracılığıyla Azure abonelikleri oluşturma

Bu makale, en son API sürümlerini kullanarak program aracılığıyla Azure abonelikleri oluşturmanıza yardımcı olur. Hala eski önizleme sürümünü kullanıyorsanız [Önizleme API’leriyle program aracılığıyla Azure abonelikleri oluşturma](programmatically-create-subscription-preview.md) başlıklı makaleye bakın. 

Aşağıdaki sözleşme türlerine yönelik faturalama hesabına sahip Azure müşterileri program aracılığıyla abonelikler oluşturabilir:

- [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Microsoft Müşteri Sözleşmesi (MCA)](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/)
- [Microsoft İş Ortağı Sözleşmesi (MPA)](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement)

Bu makalede Azure Resource Manager'ı kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Program aracılığıyla Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft'tan veya yetkili satıcıdan aldığınız Azure hizmetlerinin tabi olduğu sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure Yasal Bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="create-subscriptions-for-an-ea-billing-account"></a>EA fatura hesabı için abonelik oluşturma

EA abonelikleri oluşturmak için aşağıdaki bölümlerdeki bilgilerden yararlanır.

### <a name="prerequisites"></a>Önkoşullar

Abonelik oluşturmak için Kayıt Hesabında Sahip rolünüzün olması gerekir. Rolü almanın iki yolu vardır:

* Kaydınızın Kuruluş Yöneticisi [sizi Hesap Sahibi yapabilir](https://ea.azure.com/helpdocs/addNewAccount) (oturum açma gereklidir) ve bu durumda Kayıt Hesabının Sahibi olursunuz.
* Kayıt Hesabının mevcut Sahibi [size erişim verebilir](grant-access-to-create-subscription.md). Benzer şekilde, EA aboneliğini oluştururken bir hizmet sorumlusu kullanmak için [söz konusu hizmet sorumlusuna abonelik oluşturabilme becerisi](grant-access-to-create-subscription.md) vermelisiniz.

### <a name="find-accounts-you-have-access-to"></a>Erişiminiz olan hesapları bulma

Siz Hesap Sahibi ile ilişkilendirilmiş bir Kayıt Hesabına eklendikten sonra, Azure hesabın kayıtla ilişkisini kullanarak abonelik ücretlerinin nereye faturalanacağını saptar. Hesap kapsamında oluşturulan tüm abonelikler hesabın içinde yer aldığı EA kaydına faturalanır. Abonelikleri oluşturmak için, aboneliğin sahibi olacak kayıt hesabı hakkındaki değerleri ve kullanıcı asıl adlarını geçirmeniz gerekir.

Aşağıdaki komutları çalıştırmak için Hesap Sahibinin *giriş dizininde* oturum açmalısınız; bu, varsayılan olarak aboneliklerin oluşturulduğu dizindir.

### <a name="rest"></a>[REST](#tab/rest-getEnrollments)

Erişiminiz olan tüm kayıt hesaplarını listeleme isteği:

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingaccounts/?api-version=2020-05-01
```

API yanıtında erişiminiz olan tüm kayıt hesapları listelenir:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/1234567",
      "name": "1234567",
      "properties": {
        "accountStatus": "Unknown",
        "accountType": "Enterprise",
        "agreementType": "EnterpriseAgreement",
        "soldTo": {
          "companyName": "Contoso",
          "country": "US "
        },
        "billingProfiles": {
          "hasMoreResults": false
        },
        "displayName": "Contoso",
        "enrollmentAccounts": [
          {
            "id": "/providers/Microsoft.Billing/billingAccounts/1234567/enrollmentAccounts/7654321",
            "name": "7654321",
            "type": "Microsoft.Billing/enrollmentAccounts",
            "properties": {
              "accountName": "Contoso",
              "accountOwnerEmail": "kenny@contoso.onmicrosoft.com",
              "costCenter": "Test",
              "isDevTest": false
            }
          }
        ],
        "hasReadAccess": false
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}

```

`enrollmentAccounts` hesaplarınızın birindeki `id` bilgisini not alın. Bu, abonelik oluşturma isteğinin başlatıldığı faturalama kapsamıdır. 

<!-- 
### [PowerShell](#tab/azure-powershell-getEnrollments)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.

-->


<!--
### [Azure CLI](#tab/azure-cli-getEnrollments)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

### <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli bir kayıt hesabı altında abonelik oluşturma

Aşağıdaki örnekte, önceki adımda seçilen kayıt hesabında *Dev Team Subscription* adlı bir abonelik oluşturulur. 

### <a name="rest"></a>[REST](#tab/rest-EA)

Abone oluşturma isteği/diğer adı oluşturmak için PUT API’yi çağırın.

```json
PUT  https://management.azure.com/providers/Microsoft.Subscription/aliases/sampleAlias?api-version=2020-09-01 
```

İstek gövdesinde, `enrollmentAccounts` hesaplarınızın birindeki `id` bilgisini `billingScope` olarak sağlayın.

```json 
{
  "properties": {
        "billingScope": "/providers/Microsoft.Billing/BillingAccounts/1234567/enrollmentAccounts/7654321",
        "DisplayName": "Dev Team Subscription", //Subscription Display Name
        "Workload": "Production"
  }
}
```

#### <a name="response"></a>Yanıt

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

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell-EA)

Modülün `New-AzSubscriptionAlias` cmdlet'ini içeren en son sürümünü yüklemek için `Install-Module Az.Subscription` çalıştırın. PowerShellGet'in yeni bir sürümünü yüklemek için [Get PowerShellGet Modülü](/powershell/scripting/gallery/installing-psget)'ne bakın.

`"/providers/Microsoft.Billing/BillingAccounts/1234567/enrollmentAccounts/7654321"` faturalama kapsamını kullanarak aşağıdaki [New-AzSubscriptionAlias](/powershell/module/az.subscription/new-azsubscription) komutunu çalıştırın. 

```azurepowershell-interactive
New-AzSubscriptionAlias -AliasName "sampleAlias" -SubscriptionName "Dev Team Subscription" -BillingScope "/providers/Microsoft.Billing/BillingAccounts/1234567/enrollmentAccounts/7654321" -Workload 'Production"
```

Komutun yanıtının bir parçası olarak subscriptionId bilgisini alırsınız.

```azurepowershell
{
  "id": "/providers/Microsoft.Subscription/aliases/sampleAlias",
  "name": "sampleAlias",
  "type": "Microsoft.Subscription/aliases",
  "properties": {
    "provisioningState": "Succeeded",
    "subscriptionId": "4921139b-ef1e-4370-a331-dd2229f4f510"
  }
}
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli-EA)

İlk olarak, `az extension add --name account` ve `az extension add --name alias` komutlarını çalıştırarak uzantıyı yükleyin.

Aşağıdaki [az account alias create](/cli/azure/ext/account/account/alias?view=azure-cli-latest#ext_account_az_account_alias_create&preserve-view=true) komutunu çalıştırıp `enrollmentAccounts` hesaplarınızdan birindeki `billing-scope` ve `id` bilgilerini sağlayın. 

```azurecli-interactive
az account alias create --name "sampleAlias" --billing-scope "/providers/Microsoft.Billing/billingAccounts/1234567/enrollmentAccounts/654321" --display-name "Dev Team Subscription" --workload "Production"
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

### <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal aboneliği oluşturma API'sinin sınırlamaları

- Bu API kullanılarak yalnızca Azure Kurumsal abonelikleri oluşturulabilir.
- Kayıt hesabı başına 2000 abonelik sınırı vardır. Hesap için bu sayıyı aşan abonelikler yalnızca Azure portalda oluşturulabilir. API aracılığıyla daha fazla abonelik oluşturmak için başka bir kayıt hesabı oluşturun. İptal edilen, silinen ve aktarılan aboneliklerin sayısı en fazla 2000 olabilir.
- Hesap Sahibi olmayan ama Azure RBAC yoluyla kayıt hesabına eklenmiş olan kullanıcılar Azure portalda abonelik oluşturamaz.
- İçinde aboneliğin oluşturulacağı kiracıyı seçemezsiniz. Abonelik her zaman Hesap Sahibinin giriş kiracısında oluşturulur. Aboneliği farklı bir kiracılığa taşımak için [abonelik kiracısını değiştirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) konusuna bakın.


## <a name="create-subscriptions-for-an-mca-account"></a>MCA hesabı için abonelik oluşturma

MCA abonelikleri oluşturmak için aşağıdaki bölümlerdeki bilgilerden yararlanır.

### <a name="prerequisites"></a>Ön koşullar

Abonelikleri oluşturmak için bir fatura bölümünde sahip, katkıda bulunan veya Azure abonelik oluşturucu rolünüz ya da faturalama profili veya faturalama hesabında sahip veya katkıda bulunan rolünüz olmalıdır. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](understand-mca-roles.md#subscription-billing-roles-and-tasks).

Aşağıdaki örneklerde REST API’leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan fatura hesaplarını bulma

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

### <a name="find-billing-profiles--invoice-sections-to-create-subscriptions"></a>Abonelik oluşturmak için faturalama profillerini ve fatura bölümlerini bulma

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

### <a name="create-a-subscription-for-an-invoice-section"></a>Fatura bölümü için aboneliği oluşturma

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

## <a name="create-subscriptions-for-an-mpa-billing-account"></a>MPA fatura hesabı için abonelik oluşturma

MPA abonelikleri oluşturmak için aşağıdaki bölümlerdeki bilgilerden yararlanır.

### <a name="prerequisites"></a>Ön koşullar

Fatura hesabınıza abonelik oluşturmak için kuruluşunuzun bulut çözüm sağlayıcısı hesabında Genel Yönetici veya Yönetici Aracısı rolünüz olmalıdır. Daha fazla bilgi için bkz. [İş Ortağı Merkezi - Kullanıcı rollerini ve izinleri atama](/partner-center/permissions-overview).

Aşağıdaki örneklerde REST API’leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

### <a name="find-the-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan fatura hesaplarını bulma

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

### <a name="find-customers-that-have-azure-plans"></a>Azure planları olan müşterileri bulma

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

### <a name="optional-for-indirect-providers-get-the-resellers-for-a-customer"></a>Dolaylı sağlayıcılar için isteğe bağlı: Müşterinin kurumsal bayilerini alma

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

### <a name="create-a-subscription-for-a-customer"></a>Müşteriden abonelik oluşturma

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

## <a name="create-subscriptions-using-arm-templates"></a>ARM şablonlarını kullanarak abonelik oluşturma

Üretim/test dağıtım işlemlerinizi otomatikleştirmenizi sağlayan Azure Resource Manager şablonunuzda (ARM şablonu) abonelikler oluşturabilirsiniz. Aşağıdaki örnekte, bir Azure aboneliği ve Azure kaynak grubu oluşturmak için ARM şablonu kullanırsınız.

### <a name="prerequisites"></a>Ön koşullar

Abonelikleri oluşturmak için bir fatura bölümünde sahip, katkıda bulunan veya Azure abonelik oluşturucu rolünüz ya da faturalama profili veya faturalama hesabında sahip veya katkıda bulunan rolünüz olmalıdır. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](understand-mca-roles.md#subscription-billing-roles-and-tasks).

Ayrıca, ARM şablon dağıtımı yaptığınızdan kök nesnesinde yazma izinlerine sahip olmanız gerekir. Bu nedenle, bir yönetim grubu kapsamında ARM dağıtımı oluşturuyorsanız yönetim grubunda yazma izinlerine sahip olmanız gerekir. Eylemin yalnızca ARM dağıtımı oluşturmaya yaradığını, bir abonelik oluşturulduysa bunun yalnızca ARM şablonunda belirtilen yönetim grubunda oluşturulduğunu unutmayın.

Aşağıdaki örneklerde REST API’leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan fatura hesaplarını bulma

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

### <a name="find-billing-profiles--invoice-sections-to-create-subscriptions"></a>Abonelik oluşturmak için faturalama profillerini ve fatura bölümlerini bulma

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

### <a name="create-a-subscription-and-resource-group-with-a-template"></a>Şablonla bir abonelik ve kaynak grubu oluşturma

Aşağıdaki ARM şablonunda, *Geliştirme* fatura bölümü için *Geliştirme Ekibi aboneliği* adlı bir abonelik oluşturulur. Abonelik *Contoso Faturalama Profiline* faturalanır ve faturanın *Geliştirme* bölümünde görüntülenir. Önceki adımda kopyalanan faturalama kapsamını kullanırsınız: `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/AW4F-xxxx-xxx-xxx/invoiceSections/SH3V-xxxx-xxx-xxx`. 

#### <a name="request"></a>İstek

```rest
PUT https://management.azure.com/providers/Microsoft.Resources/deployments/sampleTemplate?api-version=2019-10-01
```

#### <a name="request-body"></a>İstek Gövdesi

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

#### <a name="response"></a>Yanıt

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

#### <a name="response"></a>Yanıt

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
