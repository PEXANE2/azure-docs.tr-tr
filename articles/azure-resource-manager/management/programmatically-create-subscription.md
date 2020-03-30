---
title: Azure aboneliklerini programlı bir şekilde oluşturun
description: Programlı olarak ek Azure abonelikleri oluşturmayı öğrenin.
author: amberbhargava
ms.topic: conceptual
ms.date: 03/17/2020
ms.reviewer: andalmia
ms.author: banders
ms.openlocfilehash: 33f1d154f438b917b79cd299e81c9078e2f2e81d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79460407"
---
# <a name="programmatically-create-azure-subscriptions-preview"></a>Azure aboneliklerini programlı bir şekilde oluşturun (önizleme)

Kurumsal Sözleşme [(EA)](https://azure.microsoft.com/pricing/enterprise-agreement/), [Microsoft Müşteri Sözleşmesi (MCA)](https://azure.microsoft.com/pricing/purchase-options/microsoft-customer-agreement/) veya [Microsoft İş Ortağı Sözleşmesi (MPA)](https://www.microsoft.com/licensing/news/introducing-microsoft-partner-agreement) fatura hesabı olan Azure müşterileri programlı abonelikler oluşturabilir. Bu makalede, Azure Kaynak Yöneticisi'ni kullanarak programlı abonelikoluşturmayı öğrenirsiniz.

Programlı bir Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft'tan veya yetkili bir satıcıdan Azure hizmetleri aldığınız sözleşmeye tabidir. Daha fazla bilgi için [Microsoft Azure Yasal Bilgileri'ne](https://azure.microsoft.com/support/legal/)bakın.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]


## <a name="create-subscriptions-for-an-ea-billing-account"></a>EA fatura hesabı için abonelik oluşturma

EA abonelikleri oluşturmak için aşağıdaki bölümlerdeki bilgileri kullanın.

### <a name="prerequisites"></a>Ön koşullar

Abonelik oluşturmak için Kayıt Hesabında Sahip rolünüz olmalıdır. Rolü almanın iki yolu vardır:

* Kaydınızın Kurumsal Yöneticisi [sizi Bir Hesap Sahibi (gerekli](https://ea.azure.com/helpdocs/addNewAccount) oturum açma) yapabilir ve bu da sizi Kayıt Hesabı'nın sahibi yapar.

* Kayıt Hesabı'nın varolan bir Sahibi [size erişim izni](grant-access-to-create-subscription.md)verebilir. Benzer şekilde, bir EA aboneliği oluşturmak için bir hizmet sorumlusu kullanmak istiyorsanız, [bu hizmet ilkesine abonelik oluşturma olanağı vermelisiniz.](grant-access-to-create-subscription.md)

### <a name="find-accounts-you-have-access-to"></a>Erişebildiğiniz hesapları bulun

Bir Hesap Sahibiyle ilişkili bir Kayıt Hesabına eklendikten sonra Azure, abonelik ücretlerini nerede faturalandıracağını belirlemek için hesap-kayıt ilişkisini kullanır. Hesap altında oluşturulan tüm abonelikler, hesabın içinde olduğu EA kaydına faturalandırılır. Abonelik oluşturmak için, aboneye sahip olmak için kayıt hesabı ve kullanıcı ilkeleri yle ilgili değerleri geçmeniz gerekir.

Aşağıdaki komutları çalıştırmak için, aboneliklerin varsayılan olarak oluşturulduğu dizini olan Hesap Sahibinin *ev dizininde*oturum açmanız gerekir.

### <a name="rest"></a>[Geri kalanı](#tab/rest)

Erişiminiz olan tüm kayıt hesaplarını listeleme isteği:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

API yanıtı, erişebildiğiniz tüm kayıt hesaplarını listeler:

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

Aboneliklerin `principalName` faturalandırılmasını istediğiniz hesabı tanımlamak için özelliği kullanın. O `name` hesabın kopyasını kopyala. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelik oluşturmak istiyorsanız, kopyalamak ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```istiyorum. Bu tanımlayıcı, kayıt hesabının nesne kimliğidir. Bir sonraki adımda kullanabilmeniz için bu değeri `enrollmentAccountObjectId`bir yere yapıştırın.

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

[Azure Bulut Shell'i](https://shell.azure.com/) açın ve PowerShell'i seçin.

Erişebildiğiniz tüm kayıt hesaplarını listelemek için [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet'ini kullanın.

```azurepowershell-interactive
Get-AzEnrollmentAccount
```

Azure, erişebildiğiniz kayıt hesaplarının bir listesiyle yanıt verir:

```azurepowershell
ObjectId                               | PrincipalName
747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
```
Aboneliklerin `principalName` faturalandırılmasını istediğiniz hesabı tanımlamak için özelliği kullanın. O `ObjectId` hesabın kopyasını kopyala. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelik oluşturmak istiyorsanız, kopyalamak ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```istiyorum. Bir sonraki adımda kullanabilmeniz için bu nesne kimliğini `enrollmentAccountObjectId`bir yere yapıştırın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Erişiminiz olan tüm kayıt hesaplarını listelemek için [az fatura kayıt hesabı listesini](https://aka.ms/EASubCreationPublicPreviewCLI) kullanın.

```azurecli-interactive
az billing enrollment-account list
```

Azure, erişebildiğiniz kayıt hesaplarının bir listesiyle yanıt verir:

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

Aboneliklerin `principalName` faturalandırılmasını istediğiniz hesabı tanımlamak için özelliği kullanın. O `name` hesabın kopyasını kopyala. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelik oluşturmak istiyorsanız, kopyalamak ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```istiyorum. Bu tanımlayıcı, kayıt hesabının nesne kimliğidir. Bir sonraki adımda kullanabilmeniz için bu değeri `enrollmentAccountObjectId`bir yere yapıştırın.

---

### <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli bir kayıt hesabı altında abonelik oluşturma

Aşağıdaki örnek, önceki adımda seçilen kayıt hesabında *Dev Team Aboneliği* adlı bir abonelik oluşturur. Abonelik teklifi *MS-AZR-0017P* 'dir (normal Microsoft Kurumsal Sözleşmesi). Ayrıca isteğe bağlı olarak abonelik için RBAC Sahipleri olarak iki kullanıcı ekler.

### <a name="rest"></a>[Geri kalanı](#tab/rest)

`<enrollmentAccountObjectId>` değerini, ilk adımda (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```) kopyaladığınız `name` ile değiştirerek aşağıdaki istekte bulunun. Sahipleri belirtmek istiyorsanız, [kullanıcı nesnesi İngilizcesi iAh'lerini nasıl alacağınızı](grant-access-to-create-subscription.md#userObjectId)öğrenin.

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
| `displayName` | Hayır      | Dize | Aboneliğin görüntü adı. Belirtilmemişse, "Microsoft Azure Enterprise" gibi teklifin adı ayarlanır.                                 |
| `offerType`   | Evet      | Dize | Abonelik teklifi. EA için iki seçenek [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/ test, [EA portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `owners`      | Hayır       | Dize | Oluşturulduğunda aboneliğe RBAC Sahibi olarak eklemek istediğiniz herhangi bir kullanıcının Nesne Kimliği.  |

Yanıtolarak, izleme için `subscriptionOperation` bir nesne geri alırsınız. Abonelik oluşturma tamamlandığında, `subscriptionOperation` nesne abonelik kimliği `subscriptionLink` olan bir nesne döndürecek.

### <a name="powershell"></a>[Powershell](#tab/azure-powershell)

İlk olarak, çalıştırarak `Install-Module Az.Subscription -AllowPrerelease`bu önizleme modüllerini yükleyin. Çalıştığından `-AllowPrerelease` emin olmak için PowerShellGet'ın yeni bir sürümünü [Get PowerShellGet Modülü'nden](/powershell/scripting/gallery/installing-psget)yükleyin.

Aşağıdaki [Yeni Abonelik](/powershell/module/az.subscription) komutunu çalıştırın, `ObjectId` ilk adımda toplananın```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```yerine `<enrollmentAccountObjectId>` (). Sahipleri belirtmek istiyorsanız, [kullanıcı nesnesi İngilizcesi iAh'lerini nasıl alacağınızı](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | Dize | Aboneliğin görüntü adı. Belirtilmemişse, "Microsoft Azure Enterprise" gibi teklifin adı ayarlanır.                                 |
| `OfferType`   | Evet      | Dize | Abonelik teklifi. EA için iki seçenek [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/ test, [EA portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `EnrollmentAccountObjectId`      | Evet       | Dize | Aboneliğin altında oluşturulduğu ve faturalandırıldığu kayıt hesabının Nesne Kimliği. Bu değer, aldığınız `Get-AzEnrollmentAccount`bir GUID'dir. |
| `OwnerObjectId`      | Hayır       | Dize | Oluşturulduğunda aboneliğe RBAC Sahibi olarak eklemek istediğiniz herhangi bir kullanıcının Nesne Kimliği.  |
| `OwnerSignInName`    | Hayır       | Dize | Oluşturulduğunda aboneye RBAC Sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Bu parametreyi ' `OwnerObjectId`yerine kullanabilirsiniz.|
| `OwnerApplicationId` | Hayır       | Dize | Oluşturulduğunda aboneliğe RBAC Sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama kimliği. Bu parametreyi ' `OwnerObjectId`yerine kullanabilirsiniz. Bu parametreyi kullanırken, hizmet [yöneticisinin dizine okuma erişimi](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)olması gerekir.|

Tüm parametrelerin tam listesini görmek için [Yeni Abonelik'e](/powershell/module/az.subscription)bakın.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, çalıştırarak `az extension add --name subscription`bu önizleme uzantısı yükleyin.

Az [hesabı oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) komutunu çalıştırın, `name` ilk adımda kopyalanan```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```ile değiştirin `<enrollmentAccountObjectId>` ( ). Sahipleri belirtmek istiyorsanız, [kullanıcı nesnesi İngilizcesi iAh'lerini nasıl alacağınızı](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurecli-interactive
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe Adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Hayır      | Dize | Aboneliğin görüntü adı. Belirtilmemişse, "Microsoft Azure Enterprise" gibi teklifin adı ayarlanır.                                 |
| `offer-type`   | Evet      | Dize | Abonelik teklifi. EA için iki seçenek [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-AZR-0148P](https://azure.microsoft.com/offers/ms-azr-0148p/) (dev/ test, [EA portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `enrollment-account-object-id`      | Evet       | Dize | Aboneliğin altında oluşturulduğu ve faturalandırıldığu kayıt hesabının Nesne Kimliği. Bu değer, aldığınız `az billing enrollment-account list`bir GUID'dir. |
| `owner-object-id`      | Hayır       | Dize | Oluşturulduğunda aboneliğe RBAC Sahibi olarak eklemek istediğiniz herhangi bir kullanıcının Nesne Kimliği.  |
| `owner-upn`    | Hayır       | Dize | Oluşturulduğunda aboneye RBAC Sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Bu parametreyi ' `owner-object-id`yerine kullanabilirsiniz.|
| `owner-spn` | Hayır       | Dize | Oluşturulduğunda aboneliğe RBAC Sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama kimliği. Bu parametreyi ' `owner-object-id`yerine kullanabilirsiniz. Bu parametreyi kullanırken, hizmet [yöneticisinin dizine okuma erişimi](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)olması gerekir.|

Tüm parametrelerin tam listesini görmek için [az hesap oluşturma](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create)bölümüne bakın.

---

### <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal abonelik oluşturma API sınırlamaları

- Bu API kullanılarak yalnızca Azure Kurumsal abonelikleri oluşturulabilir.
- Kayıt hesabı başına 500 abonelik sınırı vardır. Bundan sonra, hesap için daha fazla abonelik yalnızca Azure portalında oluşturulabilir. API üzerinden daha fazla abonelik oluşturmak istiyorsanız, başka bir kayıt hesabı oluşturun.
- Hesap Sahibi olmayan ancak RBAC aracılığıyla bir kayıt hesabına eklenen kullanıcılar Azure portalında abonelik oluşturamaz.
- Oluşturulacak abonelik için kiracıyı seçemezsiniz. Abonelik her zaman Hesap Sahibinin ev kiracı oluşturulur. Aboneliği farklı bir kiracıya taşımak [için, abonelik kiracısını değiştir'e](../../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)bakın.


## <a name="create-subscriptions-for-an-mca-account"></a>MCA hesabı için abonelik oluşturma

### <a name="prerequisites"></a>Ön koşullar

Abonelik oluşturmak için fatura bölümünde veya fatura profilinde veya fatura hesabında sahip, katılımcı veya Azure abonelik oluşturucu rolü olmalıdır. Daha fazla bilgi için bkz. [Abonelik faturalama rolleri ve görevleri](../../cost-management-billing/manage/understand-mca-roles.md#subscription-billing-roles-and-tasks).

Aşağıda gösterilen örnekte REST API'leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

### <a name="find-billing-accounts-that-you-have-access-to"></a>Erişebildiğiniz fatura hesaplarını bulma

Tüm fatura hesaplarını listelemek için aşağıdaki isteği yapın.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```
API yanıtı, erişebildiğiniz fatura hesaplarını listeler.

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
Abonelik `displayName` oluşturmak istediğiniz faturahesabını tanımlamak için özelliği kullanın. Emin olun, hesabın agreeementType *MicrosoftCustomerAgreement*olduğunu. Hesabın `name` kopyalanması.  Örneğin, `Contoso` fatura hesabı için bir abonelik oluşturmak istiyorsanız, kopyalamak `5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`istiyorum. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="find-invoice-sections-to-create-subscriptions"></a>Abonelik oluşturmak için fatura bölümlerini bulma

Aboneliğinizin ücretleri, fatura profilinin faturasının bir bölümünde görünür. Azure abonelikleri oluşturma iznine sahip fatura bölümleri nin ve fatura profillerinin listesini almak için aşağıdaki API'yi kullanın.

`<billingAccountName>` değerini, ilk adımda (```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```) kopyaladığınız `name` ile değiştirerek aşağıdaki istekte bulunun.

```json
POST https://management.azure.com/providers/Microsoft.Billing/billingAccounts/<billingAccountName>/listInvoiceSectionsWithCreateSubscriptionPermission?api-version=2019-10-01-preview
```
API yanıtı, abonelik oluşturmak için erişebildiğiniz tüm fatura bölümlerini ve bunların fatura profillerini listeler:

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

Abonelik `invoiceSectionDisplayName` oluşturmak istediğiniz fatura bölümünü tanımlamak için özelliği kullanın. Fatura `invoiceSectionId`bölümüne `billingProfileId` ve `skuId` fatura bölümünden birini kopyalayın. Örneğin, fatura bölümü `Microsoft Azure plan` için `Development` tür aboneliği oluşturmak istiyorsanız, `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX`, , `/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-xxxx-xxx-xxx` ve `0001`. Bir sonraki adımda kullanabilmeniz için bu değerleri bir yere yapıştırın.

### <a name="create-a-subscription-for-an-invoice-section"></a>Fatura bölümü için abonelik oluşturma

Aşağıdaki örnek, *Geliştirme* faturası bölümü için *Microsoft Azure Planı* türünde Dev *Team aboneliği* adlı bir abonelik oluşturur. Abonelik, *Contoso finansının* faturalandırma profiline faturalandırılır ve faturasının *Geliştirme* bölümünde görünür.

İkinci adımdan `<invoiceSectionId>` `invoiceSectionId` kopyalanan ile değiştirerek aşağıdaki isteği```/providers/Microsoft.Billing/billingAccounts/5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_2019-05-31/billingProfiles/PBFV-XXXX-XXX-XXX/invoiceSections/GJGR-XXXX-XXX-XXX```yapın ( ). API istek `billingProfileId` parametrelerinde `skuId` ikinci adımdan geçmeniz ve kopyalamanız gerekir. Sahipleri belirtmek istiyorsanız, [kullanıcı nesnesi İngilizcesi iAh'lerini nasıl alacağınızı](grant-access-to-create-subscription.md#userObjectId)öğrenin.

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
| `displayName` | Evet      | Dize | Aboneliğin görüntü adı.|
| `billingProfileId`   | Evet      | Dize | Aboneliğin ücretleri için faturalandırılacak faturalandırma profilinin kimliği.  |
| `skuId` | Evet      | Dize | Azure planının türünü belirleyen sku kimliği. |
| `owners`      | Hayır       | Dize | Oluşturulduğunda aboneliğe RBAC Sahibi olarak eklemek istediğiniz herhangi bir kullanıcı veya hizmet sorumlusunun Nesne Kimliği.  |
| `costCenter` | Hayır      | Dize | Abonelikle ilişkili maliyet merkezi. Bu kullanım csv dosyasında gösterir. |
| `managementGroupId` | Hayır      | Dize | Aboneliğin eklendiği yönetim grubunun kimliği. Yönetim gruplarının listesini almak için [Yönetim Grupları - Liste API'sine](/rest/api/resources/managementgroups/list)bakın. API'deki bir yönetim grubunun kimliğini kullanın. |

Yanıtolarak, izleme için `subscriptionCreationResult` bir nesne geri alırsınız. Abonelik oluşturma tamamlandığında, `subscriptionCreationResult` nesne abonelik kimliği `subscriptionLink` olan bir nesne döndürecek.

## <a name="create-subscriptions-for-an-mpa-billing-account"></a>MPA fatura hesabı için abonelik oluşturma

### <a name="prerequisites"></a>Ön koşullar

Fatura hesabınız için abonelik oluşturmak için kuruluşunuzun bulut çözümü sağlayıcısı hesabında Global Admin veya Yönetici Temsilcisi rolünün olması gerekir. Daha fazla bilgi için Bkz. [İş Ortağı Merkezi - Kullanıcılara roller ve izinler atayın.](https://docs.microsoft.com/partner-center/permissions-overview)

Aşağıda gösterilen örnekte REST API'leri kullanılmaktadır. Şu anda PowerShell ve Azure CLI desteklenmemektedir.

### <a name="find-the-billing-accounts-that-you-have-access-to"></a>Erişiminiz olan fatura hesaplarını bulma

Erişiminiz olan tüm fatura hesaplarını listelemek için aşağıdaki isteği yapın.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts?api-version=2019-10-01-preview
```
API yanıtı fatura hesaplarını listeler.

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
Abonelik `displayName` oluşturmak istediğiniz faturahesabını tanımlamak için özelliği kullanın. Emin olun, hesabın agreeementType *MicrosoftPartnerAgreement*olduğunu. Hesap `name` için kopyalayın. Örneğin, `Contoso` fatura hesabı için bir abonelik oluşturmak istiyorsanız, kopyalamak `99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx`istiyorum. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="find-customers-that-have-azure-plans"></a>Azure planları olan müşterileri bulma

Azure abonelikleri oluşturabileceğiniz `<billingAccountName>` fatura `name` hesabındaki tüm müşterileri```5e98e158-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx```listelemek için ilk adımdan kopyalanan ( ) ile değiştirerek aşağıdaki isteği yapın.

```json
GET https://management.azure.com/providers/Microsoft.Billing/billingAccounts/<billingAccountName>/customers?api-version=2019-10-01-preview
```
API yanıtı, fatura hesabındaki müşterileri Azure planlarıyla listeler. Bu müşteriler için abonelikler oluşturabilirsiniz.

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

Abonelikoluşturmak `displayName` istediğiniz müşteriyi tanımlamak için özelliği kullanın. Müşteri `id` için kopyalayın. Örneğin, bir abonelik oluşturmak `Fabrikam toys`istiyorsanız, kopyalamak `/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx`istiyorum. Sonraki adımlarda kullanmak için bu değeri bir yere yapıştırın.

### <a name="optional-for-indirect-providers-get-the-resellers-for-a-customer"></a>Dolaylı sağlayıcılar için isteğe bağlı: Bir müşteri için satıcıları alın

CSP iki katmanlı modelinde Dolaylı bir sağlayıcıysanız, müşteriler için abonelikler oluştururken bir satıcı belirtebilirsiniz.

Bir müşteri için kullanılabilen `id` tüm satıcıları listelemek```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```için ikinci adımdan kopyalanan ( ) ile değiştirerek `<customerId>` aşağıdaki isteği yapın.

```json
GET https://management.azure.com<customerId>?$expand=resellers&api-version=2019-10-01-preview
```
API yanıtı, müşteri için satıcıları listeler:

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
Abonelikle `description` ilişkilendirilecek satıcıyı tanımlamak için özelliği kullanın. Satıcı `resellerId` için kopyalayın. Örneğin, ilişkilendirmek `Wingtip`istiyorsanız, kopyalamak `3xxxxx`istiyorum. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın.

### <a name="create-a-subscription-for-a-customer"></a>Müşteri için abonelik oluşturma

Aşağıdaki örnek, *Fabrikam oyuncakları* için *Dev Team aboneliği* ve *Wingtip* satıcısını aboneye ortak olarak oluşturur. T

İkinci adımdan `<customerId>` `id` kopyalanan ile değiştirerek aşağıdaki isteği```/providers/Microsoft.Billing/billingAccounts/99a13315-xxxx-xxxx-xxxx-xxxxxxxxxxxx:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx_xxxx-xx-xx/customers/2281f543-xxxx-xxxx-xxxx-xxxxxxxxxxxx```yapın ( ). API istek parametrelerinde ikinci adımdan kopyalanan isteğe bağlı *satıcıId'i* geçirin.

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
| `displayName` | Evet      | Dize | Aboneliğin görüntü adı.|
| `skuId` | Evet      | Dize | Azure planının sku kimliği. Microsoft Azure Planı türü abonelikleri için *0001'i* kullanma |
| `resellerId`      | Hayır       | Dize | Abonelikle ilişkilendirilecek satıcının MPN kimliği.  |

Yanıtolarak, izleme için `subscriptionCreationResult` bir nesne geri alırsınız. Abonelik oluşturma tamamlandığında, `subscriptionCreationResult` nesne abonelik kimliği `subscriptionLink` olan bir nesne döndürecek.

## <a name="next-steps"></a>Sonraki adımlar

* .NET'i kullanarak Kurumsal Sözleşme (EA) aboneliği oluşturma örneği için [GitHub'daki örnek koda](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core)bakın.
* Artık bir abonelik oluşturduğunuza göre, bu yeteneği diğer kullanıcılara ve hizmet ilkelerine verebilirsiniz. Daha fazla bilgi için [Azure Kurumsal abonelikleri (önizleme) oluşturmak için Erişim Eki'ne](grant-access-to-create-subscription.md)bakın.
* Yönetim gruplarını kullanarak çok sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için [bkz.](../../governance/management-groups/overview.md)
