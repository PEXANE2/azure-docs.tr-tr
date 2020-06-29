---
title: Program aracılığıyla Azure abonelikleri oluşturma
description: Programlı olarak ek Azure abonelikleri oluşturmayı öğrenin.
author: amberbhargava
ms.topic: conceptual
ms.date: 06/26/2020
ms.reviewer: andalmia
ms.author: banders
ms.openlocfilehash: f477def85db2d3def30f732dd5a7f8af49eda4b3
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85483763"
---
# <a name="programmatically-create-azure-subscriptions-preview"></a>Programlı olarak Azure abonelikleri oluşturma (Önizleme)

[Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)olan Azure müşterileri, [Microsoft MÜŞTERI Sözleşmesi (MCA)](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) veya [MICROSOFT iş ortağı Sözleşmesi (MPa)](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement) faturalandırma hesabı, abonelikleri programlı bir şekilde oluşturabilir. Bu makalede, Azure Resource Manager kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Bir Azure aboneliğini programlı bir şekilde oluşturduğunuzda, bu abonelik Microsoft 'tan veya yetkili bir satıcıdan Azure hizmetleri elde ettiğiniz sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]


## <a name="create-subscriptions-for-an-ea-billing-account"></a>EA faturalandırma hesabı için abonelikler oluşturma

EA abonelikleri oluşturmak için aşağıdaki bölümlerdeki bilgileri kullanın.

### <a name="prerequisites"></a>Ön koşullar

Abonelik oluşturmak için bir kayıt hesabında sahip rolüne sahip olmanız gerekir. Rolü almanın iki yolu vardır:

* Kaydınıza ait kuruluş yöneticisi, kayıt hesabının sahibini sağlayan [bir hesap sahibi](https://ea.azure.com/helpdocs/addNewAccount) (gerekli oturum açma) yapabilir.

* Kayıt hesabının mevcut sahibi [size erişim verebilir](grant-access-to-create-subscription.md). Benzer şekilde, bir EA aboneliği oluşturmak için hizmet sorumlusu kullanmak istiyorsanız, [Bu hizmet sorumlusuna abonelik oluşturma olanağı vermeniz](grant-access-to-create-subscription.md)gerekir.

### <a name="find-accounts-you-have-access-to"></a>Erişiminiz olan hesapları bulun

Hesap sahibiyle ilişkili bir kayıt hesabına eklendikten sonra Azure, abonelik ücretlerinin nerede faturalanabileceğini öğrenmek için hesap-kayıt ilişkisini kullanır. Hesap altında oluşturulan tüm abonelikler, hesabın bulunduğu EA kaydına faturalandırılır. Abonelik oluşturmak için, aboneliğe sahip kayıt hesabı ve Kullanıcı sorumluları hakkında değerleri geçirmeniz gerekir.

Aşağıdaki komutları çalıştırmak için, varsayılan olarak aboneliklerin oluşturulduğu dizin olan hesap sahibinin *giriş dizininde*oturum açmalısınız.

### <a name="rest"></a>[REST](#tab/rest)

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

`principalName`Aboneliklerinin faturalandırılması istediğiniz hesabı belirlemek için özelliğini kullanın. `name`Bu hesabın öğesini kopyalayın. Örneğin, kayıt hesabı altında abonelik oluşturmak isterseniz SignUpEngineering@contoso.com , ' yi kopyalamanız gerekir ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Bu tanımlayıcı, kayıt hesabının nesne KIMLIĞIDIR. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın `enrollmentAccountObjectId` .

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

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
`principalName`Aboneliklerinin faturalandırılması istediğiniz hesabı belirlemek için özelliğini kullanın. `ObjectId`Bu hesabın öğesini kopyalayın. Örneğin, kayıt hesabı altında abonelik oluşturmak isterseniz SignUpEngineering@contoso.com , ' yi kopyalamanız gerekir ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Bir sonraki adımda kullanabilmek için bu nesne KIMLIĞINI bir yere yapıştırın `enrollmentAccountObjectId` .

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

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

`principalName`Aboneliklerinin faturalandırılması istediğiniz hesabı belirlemek için özelliğini kullanın. `name`Bu hesabın öğesini kopyalayın. Örneğin, kayıt hesabı altında abonelik oluşturmak isterseniz SignUpEngineering@contoso.com , ' yi kopyalamanız gerekir ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Bu tanımlayıcı, kayıt hesabının nesne KIMLIĞIDIR. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın `enrollmentAccountObjectId` .

---

### <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli bir kayıt hesabı altında abonelik oluşturma

Aşağıdaki örnek, önceki adımda seçilen kayıt hesabında *dev takım aboneliği* adlı bir abonelik oluşturur. Abonelik teklifi *MS-AZR-0017P* (düzenli Microsoft kurumsal anlaşma). Ayrıca, isteğe bağlı olarak abonelik için RBAC sahipleri olarak iki kullanıcı da ekler.

### <a name="rest"></a>[REST](#tab/rest)

`<enrollmentAccountObjectId>` değerini, ilk adımda (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopyaladığınız `name` ile değiştirerek aşağıdaki istekte bulunun. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

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

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | No      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `offerType`   | Yes      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `owners`      | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |

Yanıtta, bir `subscriptionOperation` nesneyi izleme için geri alırsınız. Abonelik oluşturma işlemi tamamlandığında, `subscriptionOperation` nesne, `subscriptionLink` abonelik kimliği olan bir nesne döndürür.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak, ' yi çalıştırarak bu önizleme modülünü yüklemeniz gerekir `Install-Module Az.Subscription -AllowPrerelease` . Çalıştığından emin olmak için `-AllowPrerelease` PowerShellGet 'In [Get PowerShellGet modülünden](/powershell/scripting/gallery/installing-psget)yeni bir sürümünü yüklemelisiniz.

Aşağıdaki [New-AzSubscription](/powershell/module/az.subscription) komutunu çalıştırın, `<enrollmentAccountObjectId>` `ObjectId` ilk adımda toplanan ile değiştirin ( ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ). Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | No      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `OfferType`   | Yes      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `EnrollmentAccountObjectId`      | Yes       | Dize | Aboneliğin oluşturulduğu ve faturalandırılan kayıt hesabının nesne KIMLIĞI. Bu değer, alacağınız bir GUID 'dir `Get-AzEnrollmentAccount` . |
| `OwnerObjectId`      | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |
| `OwnerSignInName`    | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Yerine bu parametreyi kullanabilirsiniz `OwnerObjectId` .|
| `OwnerApplicationId` | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama KIMLIĞI. Yerine bu parametreyi kullanabilirsiniz `OwnerObjectId` . Bu parametre kullanılırken, hizmet sorumlusu [dizine okuma erişimine](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)sahip olmalıdır.|

Tüm parametrelerin tam listesini görmek için, bkz. [New-AzSubscription](/powershell/module/az.subscription).

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, ' yi çalıştırarak bu önizleme uzantısını yüklemeniz gerekir `az extension add --name subscription` .

[az account create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) `<enrollmentAccountObjectId>` `name` İlk adımda () kopyaladığınız ile değiştirerek, aşağıdaki az Account Create komutunu çalıştırın ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurecli-interactive
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | No      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `offer-type`   | Yes      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `enrollment-account-object-id`      | Yes       | Dize | Aboneliğin oluşturulduğu ve faturalandırılan kayıt hesabının nesne KIMLIĞI. Bu değer, alacağınız bir GUID 'dir `az billing enrollment-account list` . |
| `owner-object-id`      | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |
| `owner-upn`    | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Yerine bu parametreyi kullanabilirsiniz `owner-object-id` .|
| `owner-spn` | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama KIMLIĞI. Yerine bu parametreyi kullanabilirsiniz `owner-object-id` . Bu parametre kullanılırken, hizmet sorumlusu [dizine okuma erişimine](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)sahip olmalıdır.|

Tüm parametrelerin tam listesini görmek için, bkz. [az Account Create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

---

### <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal abonelik oluşturma API 'SI sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API kullanılarak oluşturulabilir.
- Kayıt hesabı başına 2000 aboneliklerin sınırı vardır. Bundan sonra, hesap için daha fazla abonelik yalnızca Azure portal oluşturulabilir. API aracılığıyla daha fazla abonelik oluşturmak istiyorsanız, başka bir kayıt hesabı oluşturun.
- Hesap sahipleri olmayan, ancak RBAC aracılığıyla bir kayıt hesabına eklenen kullanıcılar Azure portal abonelik oluşturamaz.
- İçinde oluşturulacak abonelik için kiracıyı seçemezsiniz. Abonelik, her zaman hesap sahibinin giriş kiracısında oluşturulur. Aboneliği farklı bir kiracıya taşımak için bkz. [abonelik kiracısını değiştirme](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).


## <a name="create-subscriptions-for-an-mca-account"></a>MCA hesabı için abonelikler oluşturma

### <a name="prerequisites"></a>Ön koşullar

Abonelik oluşturmak için bir fatura bölümünde veya bir faturalandırma hesabındaki sahip veya katkıda bulunan rolünde sahip, katkıda bulunan veya Azure abonelik Oluşturucu rolüne sahip olmanız gerekir. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](../../cost-management-billing/manage/understand-mca-roles.md#subscription-billing-roles-and-tasks).

Aşağıda gösterilen örnek REST API 'Lerini kullanır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

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
`displayName`Aboneliğini oluşturmak istediğiniz faturalandırma hesabını tanımlamak için özelliğini kullanın. Hesabın agreeementType, *Microsoftcustomeragreement*olduğundan emin olun. Hesabı kopyalayın `name` .  Örneğin, faturalandırma hesabı için bir abonelik oluşturmak istiyorsanız `Contoso` , ' yi kopyalamanız gerekir `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` . Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="find-invoice-sections-to-create-subscriptions"></a>Abonelik oluşturmak için fatura bölümlerini bulma

Aboneliğinizin ücretleri, bir faturalandırma profili faturasının bir bölümünde görüntülenir. Azure abonelikleri oluşturma izniniz olan fatura bölümlerinin ve faturalama profillerinin listesini almak için aşağıdaki API 'yi kullanın.

`<billingAccountName>` değerini, ilk adımda (```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) kopyaladığınız `name` ile değiştirerek aşağıdaki istekte bulunun.

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

`invoiceSectionDisplayName`Abonelik oluşturmak istediğiniz fatura bölümünü tanımlamak için özelliğini kullanın. `invoiceSectionId` `billingProfileId` `skuId` Fatura bölümü için ve birini kopyalayın. Örneğin, fatura bölümü için türünde bir abonelik oluşturmak istiyorsanız, ve ' yi `Microsoft Azure plan` `Development` kopyalayın `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX` `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-xxxx-xxx-xxx` `0001` . Bir sonraki adımda kullanabilmeniz için bu değerleri bir yere yapıştırın.

### <a name="create-a-subscription-for-an-invoice-section"></a>Fatura bölümü için abonelik oluşturma

Aşağıdaki örnek, *geliştirme* faturası bölümü Için *Microsoft Azure plan* türünde *dev takım aboneliği* adlı bir abonelik oluşturur. Abonelik, *contoso finans* 'nin faturalama profiline faturalandırılır ve faturasının *geliştirme* bölümünde görüntülenir.

`<invoiceSectionId>` `invoiceSectionId` İkinci adımdan () kopyalanmış ile değiştirerek aşağıdaki isteği yapın ```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX``` . `billingProfileId` `skuId` API 'nin istek parametrelerine ikinci adımdan kopyalamanýz gerekir. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

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

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Yes      | Dize | Aboneliğin görünen adı.|
| `billingProfileId`   | Yes      | Dize | Aboneliğin ücretleri için faturalandırılacak Faturalandırma profili KIMLIĞI.  |
| `skuId` | Yes      | Dize | Azure planının türünü belirleyen SKU KIMLIĞI. |
| `owners`      | No       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının veya hizmet sorumlusunun nesne KIMLIĞI.  |
| `costCenter` | No      | Dize | Abonelikle ilişkili maliyet merkezi. Kullanım CSV dosyasında görünür. |
| `managementGroupId` | No      | Dize | Aboneliğin ekleneceği yönetim grubunun KIMLIĞI. Yönetim gruplarının listesini almak için, bkz. [Yönetim grupları-LIST API](/rest/api/resources/managementgroups/list). API 'den bir yönetim grubunun KIMLIĞINI kullanın. |

Yanıtta, bir `subscriptionCreationResult` nesneyi izleme için geri alırsınız. Abonelik oluşturma işlemi tamamlandığında, `subscriptionCreationResult` nesne, `subscriptionLink` abonelik kimliği olan bir nesne döndürür.

## <a name="create-subscriptions-for-an-mpa-billing-account"></a>MPA faturalandırma hesabı için abonelikler oluşturma

### <a name="prerequisites"></a>Ön koşullar

Faturalandırma hesabınız için abonelik oluşturmak üzere kuruluşunuzun bulut çözümü sağlayıcısı hesabında bir genel yönetici veya yönetici aracı rolüne sahip olmanız gerekir. Daha fazla bilgi için bkz. [Iş Ortağı Merkezi-Kullanıcı rolleri ve Izinleri atama](https://docs.microsoft.com/partner-center/permissions-overview).

Aşağıda gösterilen örnek REST API 'Lerini kullanır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

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
`displayName`Aboneliğini oluşturmak istediğiniz faturalandırma hesabını tanımlamak için özelliğini kullanın. Hesabın agreeementType, *Microsoftpartneragreement*olduğundan emin olun. `name`Hesabı için öğesini kopyalayın. Örneğin, faturalandırma hesabı için bir abonelik oluşturmak istiyorsanız `Contoso` , ' yi kopyalamanız gerekir `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx` . Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="find-customers-that-have-azure-plans"></a>Azure planlarına sahip müşterileri bulun

`<billingAccountName>` `name` ```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx``` Azure abonelikleri oluşturabileceğiniz faturalandırma hesabındaki tüm müşterileri listelemek için, ilk adımla () kopyalama ile değiştirerek aşağıdaki isteği yapın.

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

`displayName`Abonelik oluşturmak istediğiniz müşteriyi belirlemek için özelliğini kullanın. Müşteriyi kopyalayın `id` . Örneğin, için bir abonelik oluşturmak istiyorsanız `Fabrikam toys` , öğesini kopyalamanız gerekir `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx` . Sonraki adımlarda bu değeri kullanmak için bir yere yapıştırın.

### <a name="optional-for-indirect-providers-get-the-resellers-for-a-customer"></a>Dolaylı sağlayıcılar için isteğe bağlı: bir müşterinin satıcılarını edinme

CSP iki katmanlı modelde dolaylı bir Sağlayıcıysanız, müşteriler için abonelikler oluştururken bir satıcı belirtebilirsiniz.

`<customerId>` `id` ```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` Bir müşterinin kullanabildiği tüm satıcıları listelemek için ikinci adımdan () kopyalanmış şekilde değiştirerek aşağıdaki isteği yapın.

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
`description`Abonelikle ilişkilendirilecek satıcısı tanımlamak için özelliğini kullanın. `resellerId`Satıcı için öğesini kopyalayın. Örneğin, ilişkilendirmek istiyorsanız `Wingtip` , öğesini kopyalamanız gerekir `3xxxxx` . Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="create-a-subscription-for-a-customer"></a>Müşteri için abonelik oluşturma

Aşağıdaki örnek, *fabrikam Toys* Için *dev takım aboneliği* adlı bir abonelik oluşturur ve *Wingtip* satıcıyı abonelikle ilişkilendirir. T

`<customerId>` `id` İkinci adımdan () kopyalanmış ile değiştirerek aşağıdaki isteği yapın ```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . API 'nin istek parametrelerine ikinci adımdan kopyalanmış olan isteğe bağlı *ResellerID* 'yi geçirin.

```json
POST https://management.azure.com<customerId>/providers/Microsoft.Subscription/createSubscription?api-version=2018-11-01-preview
```

```json
'{"displayName": "Dev Team subscription",
  "skuId": "0001",
  "resellerId": "<resellerId>",
}'
```

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Yes      | Dize | Aboneliğin görünen adı.|
| `skuId` | Yes      | Dize | Azure planının SKU KIMLIĞI. Microsoft Azure plan türünde abonelikler için *0001* kullanın |
| `resellerId`      | No       | Dize | Abonelikle ilişkilendirilecek satıcının MPN KIMLIĞI.  |

Yanıtta, bir `subscriptionCreationResult` nesneyi izleme için geri alırsınız. Abonelik oluşturma işlemi tamamlandığında, `subscriptionCreationResult` nesne, `subscriptionLink` abonelik kimliği olan bir nesne döndürür.

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak Kurumsal Anlaşma (EA) aboneliği oluşturma hakkında bir örnek için bkz. [GitHub üzerinde örnek kod](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Artık bir abonelik oluşturduğunuza göre, bu özelliği diğer kullanıcılara ve hizmet sorumlularına atayabilirsiniz. Daha fazla bilgi için bkz. [Azure Enterprise abonelikleri (Önizleme) oluşturmak için erişim verme](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](../../governance/management-groups/overview.md)
