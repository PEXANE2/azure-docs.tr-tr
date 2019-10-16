---
title: Program aracılığıyla Azure abonelikleri oluşturma | Microsoft Docs
description: Programlı olarak ek Azure abonelikleri oluşturmayı öğrenin.
services: azure-resource-manager
author: amberb
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/10/2019
ms.author: banders
ms.openlocfilehash: 5d977fd6ce74f9cabedd0553c5815fd64d4d09a7
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376019"
---
# <a name="programmatically-create-azure-subscriptions-preview"></a>Programlı olarak Azure abonelikleri oluşturma (Önizleme)

[Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)olan Azure müşterileri, [Microsoft MÜŞTERI Sözleşmesi (MCA)](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) veya [MICROSOFT iş ortağı Sözleşmesi (MPa)](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement) faturalandırma hesabı, abonelikleri programlı bir şekilde oluşturabilir. Bu makalede, Azure Resource Manager kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Bir Azure aboneliğini programlı bir şekilde oluşturduğunuzda, bu abonelik Microsoft 'tan veya yetkili bir satıcıdan Azure hizmetleri elde ettiğiniz sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-subscriptions-for-an-ea-billing-account"></a>EA faturalandırma hesabı için abonelikler oluşturma

### <a name="prerequisites"></a>Önkoşullar

Abonelik oluşturmak için bir kayıt hesabında sahip rolüne sahip olmanız gerekir. Rolü almanın iki yolu vardır:

* Kaydınıza ait kuruluş yöneticisi, kayıt hesabının sahibini sağlayan [bir hesap sahibi](https://ea.azure.com/helpdocs/addNewAccount) (gerekli oturum açma) yapabilir.

* Kayıt hesabının mevcut sahibi [size erişim verebilir](grant-access-to-create-subscription.md). Benzer şekilde, bir EA aboneliği oluşturmak için hizmet sorumlusu kullanmak istiyorsanız, [Bu hizmet sorumlusuna abonelik oluşturma olanağı vermeniz](grant-access-to-create-subscription.md)gerekir.

### <a name="find-accounts-you-have-access-to"></a>Erişiminiz olan hesapları bulun

Hesap sahibiyle ilişkili bir kayıt hesabına eklendikten sonra Azure, abonelik ücretlerinin nerede faturalanabileceğini öğrenmek için hesap-kayıt ilişkisini kullanır. Hesap altında oluşturulan tüm abonelikler, hesabın bulunduğu EA kaydına faturalandırılır. Abonelik oluşturmak için, aboneliğe sahip kayıt hesabı ve Kullanıcı sorumluları hakkında değerleri geçirmeniz gerekir. 

Aşağıdaki komutları çalıştırmak için, varsayılan olarak aboneliklerin oluşturulduğu dizin olan hesap sahibinin *giriş dizininde*oturum açmalısınız.

### <a name="resttabrest"></a>[REST](#tab/rest)

Erişiminiz olan tüm kayıt hesaplarını listeleme isteği:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

API yanıtı, erişiminiz olan tüm kayıt hesaplarını listeler:

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "SignUpEngineering@contoso.com"
      }
    },
    {
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "type": "Microsoft.Billing/enrollmentAccounts",
      "properties": {
        "principalName": "BillingPlatformTeam@contoso.com"
      }
    }
  ]
}
```

Aboneliklerinin faturalandırılacağını istediğiniz hesabı belirlemek için `principalName` özelliğini kullanın. Bu hesabın `name` ' ni kopyalayın. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelikler oluşturmak isterseniz, ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ' i kopyalamanız gerekir. Bu tanımlayıcı, kayıt hesabının nesne KIMLIĞIDIR. Sonraki adımda `enrollmentAccountObjectId` olarak kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

[Azure Cloud Shell](https://shell.azure.com/) açın ve PowerShell ' i seçin.

Erişiminiz olan tüm kayıt hesaplarını listelemek için [Get-Azkayıtları Mentaccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet 'ini kullanın.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure, erişim sahibi olduğunuz kayıt hesaplarının bir listesi ile yanıt verir:

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```
Aboneliklerinin faturalandırılacağını istediğiniz hesabı belirlemek için `principalName` özelliğini kullanın. Bu hesabın `ObjectId` ' ni kopyalayın. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelikler oluşturmak isterseniz, ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ' i kopyalamanız gerekir. Bir sonraki adımda `enrollmentAccountObjectId` olarak kullanabilmeniz için bu nesne KIMLIĞINI bir yere yapıştırın.

### <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Erişiminiz olan tüm kayıt hesaplarını listelemek için [az faturalandırma kaydı-hesap listesi](https://aka.ms/EASubCreationPublicPreviewCLI) komutunu kullanın.

```azurecli-interactive 
az billing enrollment-account list
```

Azure, erişim sahibi olduğunuz kayıt hesaplarının bir listesi ile yanıt verir:

```json
[
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "SignUpEngineering@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  },
  {
    "id": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "name": "4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "principalName": "BillingPlatformTeam@contoso.com",
    "type": "Microsoft.Billing/enrollmentAccounts",
  }
]
```

Aboneliklerinin faturalandırılacağını istediğiniz hesabı belirlemek için `principalName` özelliğini kullanın. Bu hesabın `name` ' ni kopyalayın. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelikler oluşturmak isterseniz, ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ' i kopyalamanız gerekir. Bu tanımlayıcı, kayıt hesabının nesne KIMLIĞIDIR. Sonraki adımda `enrollmentAccountObjectId` olarak kullanabilmeniz için bu değeri bir yere yapıştırın.

---

### <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli bir kayıt hesabı altında abonelik oluşturma

Aşağıdaki örnek, önceki adımda seçilen kayıt hesabında *dev takım aboneliği* adlı bir abonelik oluşturur. Abonelik teklifi *MS-AZR-0017P* (düzenli Microsoft kurumsal anlaşma). Ayrıca, isteğe bağlı olarak abonelik için RBAC sahipleri olarak iki kullanıcı da ekler.

### <a name="resttabrest"></a>[REST](#tab/rest)

@No__t-0 ' ı ilk adımdan kopyalanmış `name` ile değiştirerek aşağıdaki isteği yapın (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```json
POST https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-03-01-preview

{
  "displayName": "Dev Team Subscription",
  "offerType": "MS-AZR-0017P",
  "owners": [
    {
      "objectId": "<userObjectId>"
    },
    {
      "objectId": "<servicePrincipalObjectId>"
    }
  ]
}
```

| Öğe adı  | Gereklidir | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `offerType`   | Yes      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `owners`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |

Yanıtta, izleme için `subscriptionOperation` nesnesini geri alırsınız. Abonelik oluşturma işlemi tamamlandığında `subscriptionOperation` nesnesi, abonelik KIMLIĞI olan bir `subscriptionLink` nesnesi döndürür.

### <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak, `Install-Module Az.Subscription -AllowPrerelease` ' ı çalıştırarak bu önizleme modülünü yüklemeniz gerekir. @No__t-0 çalıştığından emin olmak için, [Get PowerShellGet modülünden](/powershell/gallery/installing-psget)PowerShellGet ' in yeni bir sürümünü yüklemelisiniz.

@No__t-1 ' i ilk adımda toplanan `ObjectId` ile değiştirerek (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) [Yeni-AzSubscription](/powershell/module/az.subscription) komutunu çalıştırın. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Öğe adı  | Gereklidir | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `OfferType`   | Yes      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `EnrollmentAccountObjectId`      | Yes       | Dize | Aboneliğin oluşturulduğu ve faturalandırılan kayıt hesabının nesne KIMLIĞI. Bu değer `Get-AzEnrollmentAccount` ' dan aldığınız bir GUID 'dir. |
| `OwnerObjectId`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |
| `OwnerSignInName`    | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Bu parametreyi `OwnerObjectId` yerine kullanabilirsiniz.|
| `OwnerApplicationId` | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama KIMLIĞI. Bu parametreyi `OwnerObjectId` yerine kullanabilirsiniz. Bu parametre kullanılırken, hizmet sorumlusu [dizine okuma erişimine](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)sahip olmalıdır.| 

Tüm parametrelerin tam listesini görmek için, bkz. [New-AzSubscription](/powershell/module/az.subscription).

### <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, `az extension add --name subscription` çalıştırarak bu önizleme uzantısını yüklemeniz gerekir.

@No__t-1 ' i ilk adımda kopyaladığınız `name` ile değiştirerek (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```), aşağıdaki [az Account Create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) komutunu çalıştırın. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe adı  | Gereklidir | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `offer-type`   | Yes      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `enrollment-account-object-id`      | Yes       | Dize | Aboneliğin oluşturulduğu ve faturalandırılan kayıt hesabının nesne KIMLIĞI. Bu değer `az billing enrollment-account list` ' dan aldığınız bir GUID 'dir. |
| `owner-object-id`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |
| `owner-upn`    | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Bu parametreyi `owner-object-id` yerine kullanabilirsiniz.|
| `owner-spn` | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama KIMLIĞI. Bu parametreyi `owner-object-id` yerine kullanabilirsiniz. Bu parametre kullanılırken, hizmet sorumlusu [dizine okuma erişimine](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)sahip olmalıdır.| 

Tüm parametrelerin tam listesini görmek için, bkz. [az Account Create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

---

### <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal abonelik oluşturma API 'SI sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API kullanılarak oluşturulabilir.
- Kayıt hesabı başına 200 aboneliklerin sınırı vardır. Bundan sonra, hesap için daha fazla abonelik yalnızca Azure portal oluşturulabilir. API aracılığıyla daha fazla abonelik oluşturmak istiyorsanız, başka bir kayıt hesabı oluşturun.
- Hesap sahipleri olmayan, ancak RBAC aracılığıyla bir kayıt hesabına eklenen kullanıcılar Azure portal abonelik oluşturamaz.
- İçinde oluşturulacak abonelik için kiracıyı seçemezsiniz. Abonelik, her zaman hesap sahibinin giriş kiracısında oluşturulur. Aboneliği farklı bir kiracıya taşımak için bkz. [abonelik kiracısını değiştirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).


## <a name="create-subscriptions-for-an-mca-account"></a>MCA hesabı için abonelikler oluşturma

### <a name="prerequisites"></a>Önkoşullar

Abonelik oluşturmak için bir fatura bölümünde veya bir faturalandırma hesabındaki sahip veya katkıda bulunan rolünde sahip, katkıda bulunan veya Azure abonelik Oluşturucu rolüne sahip olmanız gerekir. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](../billing/billing-understand-mca-roles.md#subscription-billing-roles-and-tasks).

Aşağıda gösterilen örnek REST API 'Lerini kullanır. Şu anda, PowerShell ve Azure CLı desteklenmez.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan faturalandırma hesaplarını bulun 

Tüm faturalandırma hesaplarını listelemek için aşağıdaki isteği yapın.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```
API yanıtı, erişiminiz olan faturalandırma hesaplarını listeler.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "displayName": "Contoso",
        "hasReadAccess": true,
        "organizationId": "41b29574-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "displayName": "Fabrikam",
        "hasReadAccess": true,
        "organizationId": "41b29574-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}

```
Abonelik oluşturmak istediğiniz faturalandırma hesabını belirlemek için `displayName` özelliğini kullanın. Hesabın agreeementType, *Microsoftcustomeragreement*olduğundan emin olun. Hesabın `name` ' nı kopyalayın.  Örneğin, `Contoso` faturalandırma hesabı için bir abonelik oluşturmak istiyorsanız, `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` ' i kopyalamanız gerekir. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="find-invoice-sections-to-create-subscriptions"></a>Abonelik oluşturmak için fatura bölümlerini bulma

Aboneliğinizin ücretleri, bir faturalandırma profili faturasının bir bölümünde görüntülenir. Azure abonelikleri oluşturma izniniz olan fatura bölümlerinin ve faturalama profillerinin listesini almak için aşağıdaki API 'yi kullanın.

@No__t-0 ' ı ilk adımdan kopyalanmış `name` ile değiştirerek aşağıdaki isteği yapın (```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```).

```json
POST https://management.azure.com/providers/Microsoft.Billing/billingAccounts/<billingAccountName>/listInvoiceSectionsWithCreateSubscriptionPermission?api-version=2019-10-01-preview
```
API yanıtı, abonelik oluşturma erişimi olan tüm fatura bölümlerini ve bunların faturalandırma profillerini listeler:

```json
{
    "value": [{
        "billingProfileDisplayName": "Contoso finance",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx",
        "enabledAzurePlans": [{
            "productId": "DZH318Z0BPS6",
            "skuId": "0001",
            "skuDescription": "Microsoft Azure Plan"
        }, {
            "productId": "DZH318Z0BPS6",
            "skuId": "0002",
            "skuDescription": "Microsoft Azure Plan for DevTest"
        }],
        "invoiceSectionDisplayName": "Development",
        "invoiceSectionId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx/invoiceSections/GJ77-xxxx-xxx-xxx"
    }, {
        "billingProfileDisplayName": "Contoso finance",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-xxxx-xxx-xxx",
        "enabledAzurePlans": [{
            "productId": "DZH318Z0BPS6",
            "skuId": "0001",
            "skuDescription": "Microsoft Azure Plan"
        }, {
            "productId": "DZH318Z0BPS6",
            "skuId": "0002",
            "skuDescription": "Microsoft Azure Plan for DevTest"
        }],
        "invoiceSectionDisplayName": "Testing",
        "invoiceSectionId": "/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX"
  }]
}
    
```

Abonelik oluşturmak istediğiniz fatura bölümünü belirlemek için `invoiceSectionDisplayName` özelliğini kullanın. @No__t-0, `billingProfileId` ' i ve fatura bölümü için `skuId` ' den birini kopyalayın. Örneğin, `Development` fatura bölümü için `Microsoft Azure plan` türünde bir abonelik oluşturmak istiyorsanız, `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX`, `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-xxxx-xxx-xxx` ve `0001` ' ü kopyalamanız gerekir. Bir sonraki adımda kullanabilmeniz için bu değerleri bir yere yapıştırın.

### <a name="create-a-subscription-for-an-invoice-section"></a>Fatura bölümü için abonelik oluşturma

Aşağıdaki örnek, *geliştirme* faturası bölümü Için *Microsoft Azure plan* türünde *dev takım aboneliği* adlı bir abonelik oluşturur. Abonelik, *contoso finans* 'nin faturalama profiline faturalandırılır ve faturasının *geliştirme* bölümünde görüntülenir. 

@No__t-0 ' ı ikinci adımdan kopyalanmış `invoiceSectionId` ile değiştirerek aşağıdaki isteği yapın (```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX```). @No__t-0 ' a ve `skuId` ' i, API 'nin istek parametrelerine ikinci adımdan kopyaladığınız şekilde geçirmeniz gerekir. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```json
POST https://management.azure.com<invoiceSectionId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview
```

```json
'{"displayName": "Dev Team subscription",
  "billingProfileId": "<billingProfileId>",
  "skuId": "<skuId>",
  "owners": [
      {
        "objectId": "<userObjectId>"
      },
      {
        "objectId": "<servicePrincipalObjectId>"
      }
    ],
  "costCenter": "35683",
  "managementGroupId": "/providers/Microsoft.Management/managementGroups/xxxxxxx",",
}'

```

| Öğe adı  | Gereklidir | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Yes      | Dize | Aboneliğin görünen adı.|
| `billingProfileId`   | Yes      | Dize | Aboneliğin ücretleri için faturalandırılacak Faturalandırma profili KIMLIĞI.  |
| `skuId` | Yes      | Dize | Azure planının türünü belirleyen SKU KIMLIĞI. |
| `owners`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının veya hizmet sorumlusunun nesne KIMLIĞI.  |
| `costCenter` | Hayır      | Dize | Abonelikle ilişkili maliyet merkezi. Kullanım CSV dosyasında görünür. |
| `managementGroupId` | Hayır      | Dize | Aboneliğin ekleneceği yönetim grubunun KIMLIĞI. Yönetim gruplarının listesini almak için, bkz. [Yönetim grupları-LIST API](https://docs.microsoft.com/rest/api/resources/managementgroups/list). API 'den bir yönetim grubunun KIMLIĞINI kullanın. |

Yanıtta, izleme için `subscriptionCreationResult` nesnesini geri alırsınız. Abonelik oluşturma işlemi tamamlandığında `subscriptionCreationResult` nesnesi, abonelik KIMLIĞI olan bir `subscriptionLink` nesnesi döndürür.

## <a name="create-subscriptions-for-an-mpa-billing-account"></a>MPA faturalandırma hesabı için abonelikler oluşturma

### <a name="prerequisites"></a>Önkoşullar

Faturalandırma hesabınız için abonelik oluşturmak üzere kuruluşunuzun bulut çözümü sağlayıcısı hesabında bir genel yönetici veya yönetici aracı rolüne sahip olmanız gerekir. Daha fazla bilgi için bkz. [Iş Ortağı Merkezi-Kullanıcı rolleri ve Izinleri atama](https://docs.microsoft.com/partner-center/permissions-overview).

Aşağıda gösterilen örnek REST API 'Lerini kullanır. Şu anda, PowerShell ve Azure CLı desteklenmez.

### <a name="find-the-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan faturalandırma hesaplarını bulun 

Erişiminiz olan tüm faturalandırma hesaplarını listelemek için aşağıdaki isteği yapın.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```
API yanıtı faturalandırma hesaplarını listeler.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftPartnerAgreement",
        "displayName": "Contoso",
        "hasReadAccess": true,
        "organizationId": "1d100e69-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "name": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx",
      "properties": {
        "accountId": "4f89e155-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "accountStatus": "Active",
        "accountType": "Enterprise",
        "agreementType": "MicrosoftCustomerAgreement",
        "displayName": "Fabrikam",
        "hasReadAccess": true,
        "organizationId": "1d100e69-xxxx-xxxx-xxxx-xxxxxxxxxxxxx_xxxx-xx-xx"
      },
      "type": "Microsoft.Billing/billingAccounts"
    }
  ]
}

```
Abonelik oluşturmak istediğiniz faturalandırma hesabını belirlemek için `displayName` özelliğini kullanın. Hesabın agreeementType, *Microsoftpartneragreement*olduğundan emin olun. Hesap için `name` ' yı kopyalayın. Örneğin, `Contoso` faturalandırma hesabı için bir abonelik oluşturmak istiyorsanız, `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` ' i kopyalamanız gerekir. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="find-customers-that-have-azure-plans"></a>Azure planlarına sahip müşterileri bulun

Azure abonelikleri oluşturabileceğiniz faturalandırma hesabındaki tüm müşterileri listelemek için, `<billingAccountName>` ' ı ilk adımdan kopyalanmış `name` ile değiştirerek aşağıdaki isteği yapın (```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```). 

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/<billingAccountName>/customers?api-version=2019-10-01-preview
```
API yanıtı, Azure planlarına sahip faturalandırma hesabındaki müşterileri listeler. Bu müşteriler için abonelikler oluşturabilirsiniz.

```json
{
  "value": [
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Contoso USD",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/JUT6-xxxx-xxxx-xxxx",
        "displayName": "Fabrikam toys"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    },
    {
      "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/97c3fac4-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "name": "97c3fac4-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "properties": {
        "billingProfileDisplayName": "Fabrikam sports",
        "billingProfileId": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/billingProfiles/JUT6-xxxx-xxxx-xxxx",
        "displayName": "Fabrikam bakery"
      },
      "type": "Microsoft.Billing/billingAccounts/customers"
    }]
}
    
```

Abonelik oluşturmak istediğiniz müşteriyi belirlemek için `displayName` özelliğini kullanın. Müşteri için `id` ' yı kopyalayın. Örneğin, `Fabrikam toys` için bir abonelik oluşturmak istiyorsanız, `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx` ' i kopyalamanız gerekir. Sonraki adımlarda bu değeri kullanmak için bir yere yapıştırın.

### <a name="optional-for-indirect-providers-get-the-resellers-for-a-customer"></a>Dolaylı sağlayıcılar için isteğe bağlı: bir müşterinin satıcılarını edinme

CSP iki katmanlı modelde dolaylı bir Sağlayıcıysanız, müşteriler için abonelikler oluştururken bir satıcı belirtebilirsiniz. 

Bir müşterinin kullanabildiği tüm satıcıları listelemek için `<customerId>` ' ı ikinci adımdan (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopyalanmış `id` ile değiştirerek aşağıdaki isteği yapın.

```json
GET https://management.azure.com<customerId>?$expand=resellers&api-version=2019-10-01-preview
```
API yanıtı, müşterinin satıcılarını listeler:

```json
{
  "value": [{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "2ed2c490-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "3xxxxx",
        "description": "Wingtip"
      }
    ]
  }
},
{
  "id": "/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/4ed2c793-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "name": "4ed2c793-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "type": "Microsoft.Billing/billingAccounts/customers",
  "properties": {
    "displayName": "Fabrikam toys",
    "resellers": [
      {
        "resellerId": "5xxxxx",
        "description": "Tailspin"
      }
    ]
  }
}]
}
```
Abonelikle ilişkilendirilecek Bayi tanımlamak için `description` özelliğini kullanın. Satıcı için `resellerId` ' yı kopyalayın. Örneğin, `Wingtip` ' ı ilişkilendirmek istiyorsanız, `3xxxxx` ' i kopyalamanız gerekir. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="create-a-subscription-for-a-customer"></a>Müşteri için abonelik oluşturma

Aşağıdaki örnek, *fabrikam Toys* Için *dev takım aboneliği* adlı bir abonelik oluşturur ve *Wingtip* satıcıyı abonelikle ilişkilendirir. T

@No__t-0 ' ı ilk adımdan kopyalanmış `id` ile değiştirerek aşağıdaki isteği yapın (```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). API 'nin istek parametrelerine ikinci adımdan kopyalanmış olan isteğe bağlı *ResellerID* 'yi geçirin. 

```json
POST https://management.azure.com<customerId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview
```

```json
'{"displayName": "Dev Team subscription",
  "skuId": "0001",
  "resellerId": "<resellerId>",
}'
```

| Öğe adı  | Gereklidir | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Yes      | Dize | Aboneliğin görünen adı.|
| `skuId` | Yes      | Dize | Azure planının SKU KIMLIĞI. Microsoft Azure plan türünde abonelikler için *0001* kullanın |
| `resellerId`      | Hayır       | Dize | Abonelikle ilişkilendirilecek satıcının MPN KIMLIĞI.  |

Yanıtta, izleme için `subscriptionCreationResult` nesnesini geri alırsınız. Abonelik oluşturma işlemi tamamlandığında `subscriptionCreationResult` nesnesi, abonelik KIMLIĞI olan bir `subscriptionLink` nesnesi döndürür.

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak Kurumsal Anlaşma (EA) aboneliği oluşturma hakkında bir örnek için bkz. [GitHub üzerinde örnek kod](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Artık bir abonelik oluşturduğunuza göre, bu özelliği diğer kullanıcılara ve hizmet sorumlularına atayabilirsiniz. Daha fazla bilgi için bkz. [Azure Enterprise abonelikleri (Önizleme) oluşturmak için erişim verme](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](management-groups-overview.md)
