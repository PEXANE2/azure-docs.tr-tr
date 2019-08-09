---
title: Programlı olarak Azure Enterprise abonelikleri oluşturma | Microsoft Docs
description: Programlı olarak ek Azure Enterprise veya Enterprise dev/test abonelikleri oluşturmayı öğrenin.
services: azure-resource-manager
author: jureid
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/10/2019
ms.author: jureid
ms.openlocfilehash: d6ae863aed629f5f5b1497d5a6e0f8108f4703c8
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848706"
---
# <a name="programmatically-create-azure-enterprise-subscriptions-preview"></a>Programlı olarak Azure Enterprise abonelikleri oluşturma (Önizleme)

[Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)üzerinde bir Azure müşterisi olarak, ŞIRKET aracılığıyla EA (MS-azr-0017P) ve EA geliştirme/test (MS-azr-0148p) abonelikleri oluşturabilirsiniz. Bu makalede, Azure Resource Manager kullanarak program aracılığıyla abonelik oluşturmayı öğreneceksiniz.

Bu API 'den bir Azure aboneliği oluşturduğunuzda, bu abonelik Microsoft 'tan veya yetkili bir satıcıdan Microsoft Azure hizmetleri elde ettiğiniz sözleşmeye tabidir. Daha fazla bilgi için bkz. [Microsoft Azure yasal bilgiler](https://azure.microsoft.com/support/legal/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Abonelik oluşturmak istediğiniz kayıt hesabında bir sahip rolüne sahip olmanız gerekir. Bu rolleri almanın iki yolu vardır:

* Kayıt yöneticiniz, size kayıt hesabının sahibini sağlayan [bir hesap sahibi](https://ea.azure.com/helpdocs/addNewAccount) (gerekli oturum açma) yapabilir. Bir başlangıç aboneliğini el ile oluşturmak için aldığınız davet e-postasında yer alan yönergeleri izleyin. Bir sonraki adıma geçmeden önce hesap sahipliğini onaylayın ve bir başlangıç EA aboneliğini el ile oluşturun. Hesabı kayda eklemek yeterli değildir.

* Kayıt hesabının mevcut sahibi [size erişim verebilir](grant-access-to-create-subscription.md). Benzer şekilde, EA aboneliğini oluşturmak için bir hizmet sorumlusu kullanmak istiyorsanız, [Bu hizmet sorumlusuna abonelik oluşturma olanağı vermeniz](grant-access-to-create-subscription.md)gerekir.

## <a name="find-accounts-you-have-access-to"></a>Erişiminiz olan hesapları bulun

Bir Azure EA kaydına hesap sahibi olarak eklendikten sonra, Azure, abonelik ücretlerinin nerede faturalanabileceğini öğrenmek için hesap-kayıt ilişkisini kullanır. Hesap altında oluşturulan tüm abonelikler, hesabın bulunduğu EA kaydına doğru şekilde faturalandırılır. Abonelik oluşturmak için, aboneliğe sahip kayıt hesabı ve Kullanıcı sorumluları hakkında değerleri geçirmeniz gerekir. 

Aşağıdaki komutları çalıştırmak için, varsayılan olarak aboneliklerin oluşturulduğu dizin olan hesap sahibinin *giriş dizininde*oturum açmalısınız.

## <a name="resttabrest"></a>[REST](#tab/rest)

Erişiminiz olan tüm kayıt hesaplarını listeleme isteği:

```json
GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
```

Azure, erişiminiz olan tüm kayıt hesaplarının bir listesi ile yanıt verir:

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

Aboneliklerinin faturalandırılması istediğiniz hesabı belirlemek için özelliğinikullanın.`principalName` `name` Bu hesabın öğesini kopyalayın. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelik oluşturmak isterseniz, ' yi kopyalamanız ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```gerekir. Bu tanımlayıcı, kayıt hesabının nesne KIMLIĞIDIR. Bir sonraki adımda `enrollmentAccountObjectId`kullanabilmeniz için bu değeri bir yere yapıştırın.

## <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

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
Aboneliklerinin faturalandırılması istediğiniz hesabı belirlemek için özelliğinikullanın.`principalName` `ObjectId` Bu hesabın öğesini kopyalayın. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelik oluşturmak isterseniz, ' yi kopyalamanız ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```gerekir. Bir sonraki adımda kullanabilmek için `enrollmentAccountObjectId`Bu nesne kimliğini bir yere yapıştırın.

## <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

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

Aboneliklerinin faturalandırılması istediğiniz hesabı belirlemek için özelliğinikullanın.`principalName` `name` Bu hesabın öğesini kopyalayın. Örneğin, SignUpEngineering@contoso.com kayıt hesabı altında abonelik oluşturmak isterseniz, ' yi kopyalamanız ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```gerekir. Bu tanımlayıcı, kayıt hesabının nesne KIMLIĞIDIR. Bir sonraki adımda `enrollmentAccountObjectId`kullanabilmeniz için bu değeri bir yere yapıştırın.

---

## <a name="create-subscriptions-under-a-specific-enrollment-account"></a>Belirli bir kayıt hesabı altında abonelik oluşturma

Aşağıdaki örnek, önceki adımda seçilen kayıt hesabında *dev takım aboneliği* adlı bir abonelik oluşturur. Abonelik teklifi *MS-AZR-0017P* (düzenli Microsoft kurumsal anlaşma). Ayrıca, isteğe bağlı olarak abonelik için RBAC sahipleri olarak iki kullanıcı da ekler.

# <a name="resttabrest"></a>[REST](#tab/rest)

İlk adımdan ( `<enrollmentAccountObjectId>` `name` )```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```kopyalama ile değiştirerek aşağıdaki isteği yapın. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

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

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `displayName` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `offerType`   | Evet      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `owners`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |

Yanıtta, bir `subscriptionOperation` nesneyi izleme için geri alırsınız. Abonelik oluşturma işlemi tamamlandığında, `subscriptionOperation` nesne, abonelik kimliği olan bir `subscriptionLink` nesne döndürür.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

İlk olarak, ' yi çalıştırarak `Install-Module Az.Subscription -AllowPrerelease`bu önizleme modülünü yüklemeniz gerekir. Çalıştığından emin `-AllowPrerelease` olmak için PowerShellGet 'in [Get PowerShellGet modülünden](/powershell/gallery/installing-psget)yeni bir sürümünü yüklemelisiniz.

Aşağıdaki [New-azsubscription](/powershell/module/az.subscription) komutunu çalıştırın, ilk adımda `<enrollmentAccountObjectId>` `ObjectId` toplanan ile değiştirin (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurepowershell-interactive
New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `Name` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `OfferType`   | Evet      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `EnrollmentAccountObjectId`      | Evet       | Dize | Aboneliğin oluşturulduğu ve faturalandırılan kayıt hesabının nesne KIMLIĞI. Bu değer, alacağınız `Get-AzEnrollmentAccount`bir GUID 'dir. |
| `OwnerObjectId`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |
| `OwnerSignInName`    | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Yerine bu parametreyi kullanabilirsiniz `OwnerObjectId`.|
| `OwnerApplicationId` | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama KIMLIĞI. Yerine bu parametreyi kullanabilirsiniz `OwnerObjectId`. Bu parametre kullanılırken, hizmet sorumlusu [dizine okuma erişimine](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)sahip olmalıdır.| 

Tüm parametrelerin tam listesini görmek için, bkz. [New-AzSubscription](/powershell/module/az.subscription.preview).

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

İlk olarak, ' yi çalıştırarak `az extension add --name subscription`bu önizleme uzantısını yüklemeniz gerekir.

İlk adımda ( `<enrollmentAccountObjectId>` `name` [](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create) )kopyaladığınıziledeğiştirerek,aşağıdaki```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```az Account Create komutunu çalıştırın. Sahipleri belirtmek isterseniz, [Kullanıcı nesne kimliklerini nasıl alabileceğinizi](grant-access-to-create-subscription.md#userObjectId)öğrenin.

```azurecli-interactive 
az account create --offer-type "MS-AZR-0017P" --display-name "Dev Team Subscription" --enrollment-account-object-id "<enrollmentAccountObjectId>" --owner-object-id "<userObjectId>","<servicePrincipalObjectId>"
```

| Öğe adı  | Gerekli | Tür   | Açıklama                                                                                               |
|---------------|----------|--------|-----------------------------------------------------------------------------------------------------------|
| `display-name` | Hayır      | Dize | Aboneliğin görünen adı. Belirtilmemişse, "Microsoft Azure Kurumsal" gibi teklifin adına ayarlanır.                                 |
| `offer-type`   | Evet      | Dize | Abonelik teklifi. EA 'nın iki seçeneği [MS-AZR-0017P](https://azure.microsoft.com/pricing/enterprise-agreement/) (üretim kullanımı) ve [MS-azr-0148p](https://azure.microsoft.com/offers/ms-azr-0148p/) (GELIŞTIRME/test, [EA Portalı kullanılarak açık](https://ea.azure.com/helpdocs/DevOrTestOffer)olması gerekir).                |
| `enrollment-account-object-id`      | Evet       | Dize | Aboneliğin oluşturulduğu ve faturalandırılan kayıt hesabının nesne KIMLIĞI. Bu değer, alacağınız `az billing enrollment-account list`bir GUID 'dir. |
| `owner-object-id`      | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının nesne KIMLIĞI.  |
| `owner-upn`    | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir kullanıcının e-posta adresi. Yerine bu parametreyi kullanabilirsiniz `owner-object-id`.|
| `owner-spn` | Hayır       | Dize | Abonelikte RBAC sahibi olarak eklemek istediğiniz herhangi bir hizmet sorumlusunun uygulama KIMLIĞI. Yerine bu parametreyi kullanabilirsiniz `owner-object-id`. Bu parametre kullanılırken, hizmet sorumlusu [dizine okuma erişimine](/powershell/azure/active-directory/signing-in-service-principal?view=azureadps-2.0#give-the-service-principal-reader-access-to-the-current-tenant-get-azureaddirectoryrole)sahip olmalıdır.| 

Tüm parametrelerin tam listesini görmek için, bkz. [az Account Create](/cli/azure/ext/subscription/account?view=azure-cli-latest#-ext-subscription-az-account-create).

---

## <a name="limitations-of-azure-enterprise-subscription-creation-api"></a>Azure Kurumsal abonelik oluşturma API 'SI sınırlamaları

- Yalnızca Azure Enterprise abonelikleri bu API kullanılarak oluşturulabilir.
- Kayıt hesabı başına 200 aboneliklerin sınırı vardır. Bundan sonra hesap için daha fazla abonelik yalnızca hesap Merkezi aracılığıyla oluşturulabilir. API aracılığıyla daha fazla abonelik oluşturmak istiyorsanız, başka bir kayıt hesabı oluşturun.
- Hesap sahibi olmayan, ancak RBAC aracılığıyla bir kayıt hesabına eklenen kullanıcılar, hesap merkezini kullanarak abonelikler oluşturamaz.
- İçinde oluşturulacak abonelik için kiracıyı seçemezsiniz. Abonelik, her zaman hesap sahibinin giriş kiracısında oluşturulur. Aboneliği farklı bir kiracıya taşımak için bkz. [abonelik kiracısını değiştirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md).

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak abonelikler oluşturma hakkında bir örnek için bkz. [GitHub üzerinde örnek kod](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Artık bir abonelik oluşturduğunuza göre, bu özelliği diğer kullanıcılara ve hizmet sorumlularına atayabilirsiniz. Daha fazla bilgi için bkz. [Azure Enterprise abonelikleri (Önizleme) oluşturmak için erişim verme](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](management-groups-overview.md)
