---
title: En son API’lerle program aracılığıyla Azure Kurumsal Anlaşma abonelikleri oluşturma
description: REST API, Azure CLI ve Azure PowerShell’in en son sürümlerini kullanarak program aracılığıyla Azure Kurumsal Anlaşma abonelikleri oluşturmayı öğrenin.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 11/17/2020
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: 0cdd25b2937dd1fb2cc70ef7b1c5a9e9ddaef375
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96780629"
---
# <a name="programmatically-create-azure-enterprise-agreement-subscriptions-with-the-latest-apis"></a>En son API’lerle program aracılığıyla Azure Kurumsal Anlaşma abonelikleri oluşturma

Bu makale, en son API sürümlerini kullanarak program aracılığıyla Azure Kurumsal Anlaşma (EA) fatura hesabı için Azure EA abonelikleri oluşturmanıza yardımcı olur. Hala eski önizleme sürümünü kullanıyorsanız [Önizleme API’leriyle program aracılığıyla Azure abonelikleri oluşturma](programmatically-create-subscription-preview.md) başlıklı makaleye bakın. 

Bu makalede Azure Resource Manager'ı kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Program aracılığıyla Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft'tan veya yetkili satıcıdan aldığınız Azure hizmetlerinin tabi olduğu sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure Yasal Bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Abonelik oluşturmak için Kayıt Hesabında Sahip rolünüzün olması gerekir. Rolü almanın iki yolu vardır:

* Kaydınızın Kuruluş Yöneticisi [sizi Hesap Sahibi yapabilir](https://ea.azure.com/helpdocs/addNewAccount) (oturum açma gereklidir) ve bu durumda Kayıt Hesabının Sahibi olursunuz.
* Kayıt Hesabının mevcut Sahibi [size erişim verebilir](grant-access-to-create-subscription.md). Benzer şekilde, EA aboneliğini oluştururken bir hizmet sorumlusu kullanmak için [söz konusu hizmet sorumlusuna abonelik oluşturabilme becerisi](grant-access-to-create-subscription.md) vermelisiniz.

## <a name="find-accounts-you-have-access-to"></a>Erişiminiz olan hesapları bulma

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

Faturalama kapsamı değeri ile `id` aynı şeydir. Kayıt hesabınızın `id` değeri, abonelik isteğinin başlatıldığı faturalama kapsamıdır. ID değerini bilmek önemlidir çünkü bu değer daha sonra makalede abonelik oluşturmak için kullanacağınız gerekli bir parametredir.

<!-- 
### [PowerShell](#tab/azure-powershell-getEnrollments)

we're still working on enabling PowerShell SDK for billing APIs. Check back soon.

-->


<!--
### [Azure CLI](#tab/azure-cli-getEnrollments)

we're still working on enabling CLI SDK for billing APIs. Check back soon.
-->

---

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli bir kayıt hesabı altında abonelik oluşturma

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

`Workload` için izin verilen değerler `Production` ve `DevTest` değerleridir.

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

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal aboneliği oluşturma API'sinin sınırlamaları

- Bu API kullanılarak yalnızca Azure Kurumsal abonelikleri oluşturulabilir.
- Kayıt hesabı başına 2000 abonelik sınırı vardır. Hesap için bu sayıyı aşan abonelikler yalnızca Azure portalda oluşturulabilir. API aracılığıyla daha fazla abonelik oluşturmak için başka bir kayıt hesabı oluşturun. İptal edilen, silinen ve aktarılan aboneliklerin sayısı en fazla 2000 olabilir.
- Hesap Sahibi olmayan ama Azure RBAC yoluyla kayıt hesabına eklenmiş olan kullanıcılar Azure portalda abonelik oluşturamaz.
- İçinde aboneliğin oluşturulacağı kiracıyı seçemezsiniz. Abonelik her zaman Hesap Sahibinin giriş kiracısında oluşturulur. Aboneliği farklı bir kiracılığa taşımak için [abonelik kiracısını değiştirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md) konusuna bakın.


## <a name="next-steps"></a>Sonraki adımlar

* Artık aboneliği oluşturduğunuza göre, bu beceriyi diğer kullanıcılara ve hizmet sorumlularına verebilirsiniz. Daha fazla bilgi için bkz. [Azure Kurumsal abonelikleri oluşturma erişimi verme (önizlemi)](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok fazla sayıda aboneliği yönetme hakkında daha fazla bilgi için bkz. [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../governance/management-groups/overview.md).
