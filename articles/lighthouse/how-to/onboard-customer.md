---
title: Bir müşteriyi Azure tarafından atanan temsilcinin kaynak yönetimine ekleme
description: Bir müşteriyi Azure'a devredilen kaynak yönetimine nasıl bağlayıp kaynaklarına kendi kiracınız aracılığıyla erişilmesine ve yönetilmesine izin vermeyi öğrenin.
ms.date: 04/16/2020
ms.topic: conceptual
ms.openlocfilehash: 22c96d43f3d5f284c2cba995eb33f5f8cd238659
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81481680"
---
# <a name="onboard-a-customer-to-azure-delegated-resource-management"></a>Bir müşteriyi Azure tarafından atanan temsilcinin kaynak yönetimine ekleme

Bu makalede, bir hizmet sağlayıcısı olarak, bir müşteriye Azure temsilcili kaynak yönetimine nasıl binebileceğinizi ve devredilen kaynaklarına (abonelikler ve/veya kaynak grupları) kendi Azure Etkin Dizin (Azure AD) kiracınız aracılığıyla erişilmesine ve yönetilmesine izin verebilirsiniz. Burada hizmet sağlayıcılara ve müşterilere atıfta bulunsak da, [birden çok kiracıyı yöneten işletmeler](../concepts/enterprise.md) yönetim deneyimlerini pekiştirmek için aynı işlemi kullanabilir.

Birden çok müşteri için kaynak yönetiyorsanız, bu işlemi yineleyebilirsiniz. Daha sonra, yetkili bir kullanıcı kiracınıza oturum açtığınızda, bu kullanıcı her bir müşteri kiracısında oturum açmadan yönetim işlemlerini gerçekleştirmek için müşteri kira kapsamında yetkilendirilebilir.

Müşteri etkileşimleri üzerindeki etkinizi izlemek ve tanınmak için Microsoft İş Ortağı Ağı (MPN) kimliğinizi, yerleşik aboneliklerinizin her birine erişimi olan en az bir kullanıcı hesabıyla ilişkilendirin. Bu ilişkilendirmeyi hizmet sağlayıcıkiracınızda gerçekleştirmeniz gerektiğini unutmayın. Basitlik için, kiracınızda MPN Kimliğinizle ilişkili bir hizmet ana hesabı oluşturmanızı ve ona içinde bulunan her müşteriye Okuyucu erişimi vermenizi öneririz. Daha fazla bilgi için [bkz.](../../billing/billing-partner-admin-link-started.md) 

> [!NOTE]
> Müşteriler, Azure Marketi'nde yayınladığınız yönetilen bir hizmet teklifini (herkese açık veya özel) satın aldıklarında da uçakta bulunabilir. Daha fazla bilgi için Azure [Marketi'ne Yönetilen Hizmetleri Yayımla teklifine](publish-managed-services-offers.md)bakın. Azure Marketi'nde yayınlanan bir teklifle birlikte burada açıklanan onboarding işlemini de kullanabilirsiniz.

Onboarding işlemi, hem hizmet sağlayıcısının kiracısı hem de müşterinin kiracısından yapılacak eylemleri gerektirir. Bu adımların tümü bu makalede açıklanmıştır.

## <a name="gather-tenant-and-subscription-details"></a>Kiracı ve abonelik ayrıntılarını toplama

Bir müşterinin kiracısına binebilmek için etkin bir Azure aboneliği olması gerekir. Aşağıdakileri bilmeniz gerekir:

- Servis sağlayıcısının kiracısının kiracı kimliği (müşterinin kaynaklarını yöneteceğiniz yer)
- Müşterinin kiracısının kiracı kimliği (hizmet sağlayıcı tarafından yönetilen kaynaklara sahip olacak)
- Müşterinin kiracısında, hizmet sağlayıcısı tarafından yönetilecek (veya servis sağlayıcısı tarafından yönetilecek kaynak grubu(lar) içeren her abonelik için abonelik tayini.

> [!NOTE]
> Bir abonelik içinde yalnızca bir veya daha fazla kaynak grubuna dahil olmak isteseniz bile, dağıtımın abonelik düzeyinde yapılması gerekir, bu nedenle abonelik kimliğine ihtiyacınız olacaktır.

Bu kimlik değerlerini zaten bilmiyorsanız, bunları aşağıdaki yollardan biriyle alabilirsiniz. Dağıtımınızda bu değerleri kullandığınızdan emin olun ve kullanın.

### <a name="azure-portal"></a>Azure portal

Kiracı kimliğiniz, Azure portalının sağ üst tarafında hesap adınızın üzerinde gezinerek veya **Switch dizinini**seçerek görülebilir. Kiracı kimliğinizi seçmek ve kopyalamak için portal içinden "Azure Etkin Dizini"ni arayın, ardından **Özellikler'i** seçin ve **Dizin Kimliği** alanında gösterilen değeri kopyalayın. Müşteri kiracısında bir abonelik kimliğini bulmak için "Abonelikler"i arayın ve ardından uygun abonelik kimliğini seçin.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Select-AzSubscription <subscriptionId>
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account set --subscription <subscriptionId/name>
az account show
```

> [!NOTE]
> Burada açıklanan işlemi kullanarak bir aboneye (veya bir abonelik içindeki bir veya daha fazla kaynak grubuna) bindiğinizde, **Microsoft.ManagedServices** kaynak sağlayıcısı bu abonelik için kaydedilir.

## <a name="define-roles-and-permissions"></a>Rolleri ve izinleri tanımlama

Bir hizmet sağlayıcısı olarak, farklı kapsamlar için farklı erişim gerektiren tek bir müşteri için birden çok görev gerçekleştirmek isteyebilirsiniz. Kiracınızdaki kullanıcılara [rol tabanlı erişim denetimi (RBAC) yerleşik roller](../../role-based-access-control/built-in-roles.md) atamak için gereken kadar yetkilendirme tanımlayabilirsiniz.

Yönetimi kolaylaştırmak için, her rol için Azure AD kullanıcı gruplarını kullanmanızı ve izinleri doğrudan bu kullanıcıya atamak yerine tek tek kullanıcıları gruba eklemenize veya kaldırmanıza olanak tanır. Ayrıca, bir hizmet müdürüne roller atamak isteyebilirsiniz. Kullanıcıların yalnızca işlerini tamamlamak için gereken izinlere sahip olmaları için en az ayrıcalık ilkesine uymayı unutmayın. Desteklenen rollerle ilgili öneriler ve bilgiler için Azure [Deniz Feneri senaryolarında Kiracılar, kullanıcılar ve roller](../concepts/tenants-users-roles.md)ebakını görün.

> [!IMPORTANT]
> Bir Azure REKLAM grubu için izin eklemek için **Grup türü** **Office 365** **değil, Güvenlik** olmalıdır. Grup oluşturulduğunda bu seçenek seçilir. Daha fazla bilgi için [bkz.](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)

Yetkilendirmeleri tanımlamak için, erişim vermek istediğiniz hizmet sağlayıcı kiracıdaki her kullanıcı, kullanıcı grubu veya hizmet sorumlusu için kimlik değerlerini bilmeniz gerekir. Ayrıca, atamak istediğiniz her yerleşik rol için rol tanımı kimliğine de ihtiyacınız olur. Zaten yoksa, servis sağlayıcı kiracı içinden aşağıdaki komutları çalıştırarak bunları alabilirsiniz.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
(Get-AzADGroup -DisplayName '<yourGroupName>').id

# To retrieve the objectId for an Azure AD user
(Get-AzADUser -UserPrincipalName '<yourUPN>').id

# To retrieve the objectId for an SPN
(Get-AzADApplication -DisplayName '<appDisplayName>').objectId

# To retrieve role definition IDs
(Get-AzRoleDefinition -Name '<roleName>').id
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# To retrieve the objectId for an Azure AD group
az ad group list --query "[?displayName == '<yourGroupName>'].objectId" --output tsv

# To retrieve the objectId for an Azure AD user
az ad user show --id "<yourUPN>" --query "objectId" --output tsv

# To retrieve the objectId for an SPN
az ad sp list --query "[?displayName == '<spDisplayName>'].objectId" --output tsv

# To retrieve role definition IDs
az role definition list --name "<roleName>" | grep name
```
> [!TIP]
> Kiracınızdaki kullanıcıların gerekirse daha sonra [temsilciliğe erişimi kaldırabilmesi için,](#remove-access-to-a-delegation) bir müşteriye binerken [Yönetilen Hizmetler Kaydı Atama Silme Rolü'nü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) atamanızı öneririz. Bu rol atanmazsa, devredilen kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.

## <a name="create-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu oluşturma

Müşterinizi işe almak için, teklifiniz için aşağıdaki bilgileri içeren bir [Azure Kaynak Yöneticisi](../../azure-resource-manager/index.yml) şablonu oluşturmanız gerekir. **MspOfferName** ve **mspOfferDescription** değerleri, Azure portalının [Hizmet sağlayıcıları sayfasında](view-manage-service-providers.md) teklif ayrıntılarını görüntülerken müşteri tarafından görülebilir.

|Alan  |Tanım  |
|---------|---------|
|**mspOfferName**     |Bu tanımı açıklayan bir ad. Bu değer, teklifin başlığı olarak müşteriye görüntülenir.         |
|**mspOfferDescription**     |Teklifinizin kısa bir açıklaması (örneğin, "Contoso VM yönetim teklifi").      |
|**yönetilen ByTenantId**     |Kiracı kimliğiniz.          |
|**Yetki**     |Kiracınızdan gelen kullanıcılar/gruplar/SPN'ler **için,** her biri müşterinizin yetkilendirmenin amacını anlamasına yardımcı olacak bir **principalIdDisplayName** ile ilgili temel IdDisplayName değerleri ve erişim düzeyini belirtmek için yerleşik **roleDefinitionId** değerine eşlenir.      |

Onboarding işlemi, bir Azure Kaynak Yöneticisi şablonu [(örneklerirepo'muzda](https://github.com/Azure/Azure-Lighthouse-samples/)sağlanan) ve yapılandırmanızla eşleşecek ve yetkilendirmelerinizi tanımlamak için değiştirdiğiniz ilgili parametreler dosyası gerektirir.

Seçtiğiniz şablon, bir abonelik içinde tüm bir abonelik, kaynak grubu veya birden çok kaynak grubuna binip binmediğinize bağlıdır. Ayrıca, bu şekilde aboneliklerini (ler) yerleşik olarak yayınlamayı tercih ederseniz, Azure Marketi'nde yayınladığınız yönetilen bir hizmet teklifi satın alan müşteriler için kullanılabilecek bir şablon da sunuyoruz.

|Bu gemide için  |Bu Azure Kaynak Yöneticisi şablonundan yararlanın  |Ve bu parametre dosyayı değiştirin |
|---------|---------|---------|
|Abonelik   |[delegeKaynak Yönetimi.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management/delegatedResourceManagement.json)  |[delegeKaynak Yönetimi.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Kaynak grubu   |[rgDelegeKaynak Yönetimi.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[rgDelegeDResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Abonelik içindeki birden çok kaynak grubu   |[multipleRgDelegeKaynak Yönetimi.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[multipleRgDelegeDResourceManagement.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|Abonelik (Azure Marketi'nde yayınlanan bir teklifi kullanırken)   |[pazarDelegeKaynakYönetimi.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[pazarDelegeKaynakYönetimi.parameters.json](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!IMPORTANT]
> Burada açıklanan işlem, aynı müşteri kiracısında yerleşik abonelikler olsa bile, gemide bulunan her abonelik için ayrı bir abonelik düzeyinde dağıtım gerektirir. Aynı müşteri kiracısında farklı abonelikler içinde birden çok kaynak grubuna biniyorsanız, ayrı dağıtımlar da gereklidir. Ancak, tek bir abonelik içinde birden çok kaynak grubuna binme tek bir abonelik düzeyinde dağıtım yapılabilir.
>
> Aynı aboneye (veya abonelik içindeki kaynak gruplarına) birden çok teklifin uygulanması için ayrı dağıtımlar da gereklidir. Uygulanan her teklif farklı bir **mspOfferName**kullanmalıdır.

Aşağıdaki örnek, bir abonelik için kullanılabilecek değiştirilmiş bir **temsilciKaynak Yönetimi.parameters.json** dosyasını gösterir. Kaynak grubu parametre dosyaları [(rg-delegated-resource-management](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/rg-delegated-resource-management) klasöründe bulunan) benzerdir, ancak gemide bulunacak belirli kaynak grubunu(lar) tanımlamak için bir **rgName** parametresi de içerir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "mspOfferDescription": {
            "value": "Fabrikam Managed Services - Interstellar"
        },
        "managedByTenantId": {
            "value": "df4602a3-920c-435f-98c4-49ff031b9ef6"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "0019bcfb-6d35-48c1-a491-a701cf73b419",
                    "principalIdDisplayName": "Tier 1 Support",
                    "roleDefinitionId": "36243c78-bf99-498c-9df9-86d9f8d28608"
                },
                {
                    "principalId": "0afd8497-7bff-4873-a7ff-b19a6b7b332c",
                    "principalIdDisplayName": "Tier 2 Support",
                    "roleDefinitionId": "acdd72a7-3385-48ef-bd42-f606fba81ae7"
                },
                {
                    "principalId": "9fe47fff-5655-4779-b726-2cf02b07c7c7",
                    "principalIdDisplayName": "Service Automation Account",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                },
                {
                    "principalId": "3kl47fff-5655-4779-b726-2cf02b05c7c4",
                    "principalIdDisplayName": "Policy Automation Account",
                    "roleDefinitionId": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
                    "delegatedRoleDefinitionIds": [
                        "b24988ac-6180-42a0-ab88-20f7382dd24c",
                        "92aaf0da-9dab-42b6-94a3-d43ce8d16293"
                    ]
                }
            ]
        }
    }
}
```

Yukarıdaki örnekte son yetkilendirme Kullanıcı Erişim Yöneticisi rolü (18d7d88d-d35e-4fb5-a5c3-7773c20a72d9) ile bir **principalId** ekler. Bu rolü atarken, **devredilen RoleDefinitionIds** özelliğini ve bir veya daha fazla yerleşik rolü eklemeniz gerekir. Bu yetkilendirmede oluşturulan kullanıcı, [düzeltilebilen ilkeleri dağıtmak](deploy-policy-remediation.md)için gereken bu yerleşik rolleri müşteri kiracısında [yönetilen kimliklere](../../active-directory/managed-identities-azure-resources/overview.md) atayabilecektir. Normalde Kullanıcı Erişim Yöneticisi rolüyle ilişkili başka hiçbir izin bu kullanıcı için geçerli olmayacaktır.

## <a name="deploy-the-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını dağıtma

Parametre dosyanızı güncelledikten sonra, müşterinin kiracısındaki bir kullanıcının Azure Kaynak Yöneticisi şablonunu kiracısında abonelik düzeyi dağıtım olarak dağıtması gerekir. Azure temsilcikaynak yönetimine (veya yerleşik olmasını istediğiniz kaynak gruplarını içeren her abonelik için) yerleşik olarak yapmak istediğiniz her abonelik için ayrı bir dağıtım gerekir.

Bu abonelik düzeyinde bir dağıtım olduğundan, Azure portalında başlatılamaz. Dağıtım, aşağıda gösterildiği gibi PowerShell veya Azure CLI kullanılarak yapılabilir.

> [!IMPORTANT]
> Bu abonelik düzeyi dağıtımı, aboneliğin yerleşik olması (veya gemide bulunan kaynak gruplarını içeren) için [Sahibi yerleşik rolüne](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) sahip müşterinin kiracısında konuk olmayan bir hesap tarafından yapılmalıdır. Aboneliği devratabilecek tüm kullanıcıları görmek için, müşterinin kiracısındaki bir kullanıcı Azure portalındaki aboneliği seçebilir, **Access denetimini (IAM)** açabilir ve [Sahibi rolü olan tüm kullanıcıları görüntüleyebilir.](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription)

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
New-AzSubscriptionDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateFile <pathToTemplateFile> `
                 -TemplateParameterFile <pathToParameterFile> `
                 -Verbose

# Deploy Azure Resource Manager template that is located externally
New-AzSubscriptionDeployment -Name <deploymentName> `
                 -Location <AzureRegion> `
                 -TemplateUri <templateUri> `
                 -TemplateParameterUri <parameterUri> `
                 -Verbose
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Deploy Azure Resource Manager template using template and parameter file locally
az deployment create --name <deploymentName> \
                     --location <AzureRegion> \
                     --template-file <pathToTemplateFile> \
                     --parameters <parameters/parameterFile> \
                     --verbose

# Deploy external Azure Resource Manager template, with local parameter file
az deployment create --name <deploymentName> \
                     --location <AzureRegion> \
                     --template-uri <templateUri> \
                     --parameters <parameterFile> \
                     --verbose
```

## <a name="confirm-successful-onboarding"></a>Başarılı onboarding onaylayın

Bir müşteri aboneliği Azure temsilcili kaynak yönetimine başarıyla bindiğinde, hizmet sağlayıcısının kiracısındaki kullanıcılar aboneliği ve kaynaklarını görebilir (yukarıdaki işlem yoluyla bunlara tek tek veya uygun izinlere sahip bir Azure AD grubunun üyesi olarak erişebiliyorlarsa). Bunu onaylamak için aboneliğin aşağıdaki yollardan birinde göründüğünden emin olun.  

### <a name="azure-portal"></a>Azure portal

Hizmet sağlayıcısının kiracısında:

1. [Müşterilerim sayfasına](view-manage-customers.md)gidin.
2. **Müşteri**Seçin.
3. Kaynak Yöneticisi şablonunda sağladığınız teklif adı ile abonelik(ler)i görebileceğinizi onaylayın.

> [!IMPORTANT]
> [Müşterilerimdeki](view-manage-customers.md)temsilci aboneliğini görebilmek için, hizmet sağlayıcısının kiracısındaki kullanıcılara, abonelik Azure temsilci kaynak yönetimi için yerleşik olduğunda [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü (veya Reader erişimi içeren başka bir yerleşik rol) verilmiş olmalıdır.

Müşterinin kiracısında:

1. [Hizmet sağlayıcıları sayfasına](view-manage-service-providers.md)gidin.
2. Servis **sağlayıcının sunduğu teklifi**seçin.
3. Kaynak Yöneticisi şablonunda sağladığınız teklif adı ile abonelik(ler)i görebileceğinizi onaylayın.

> [!NOTE]
> Güncelleştirmelerin Azure portalına yansıtılmadan önce dağıtımınız tamamlandıktan sonra birkaç dakika sürebilir.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

Get-AzContext
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

az account list
```

## <a name="remove-access-to-a-delegation"></a>Temsilciye erişimi kaldırma

Varsayılan olarak, müşterinin kiracısında uygun izinlere sahip olan kullanıcılar, hizmet sağlayıcısının Azure portalının [Hizmet sağlayıcıları sayfasındaki](view-manage-service-providers.md#add-or-remove-service-provider-offers) temsilci kaynaklarına erişimini kaldırabilir. Bunu yaptıkları zaman, hizmet sağlayıcısının kiracısındaki hiçbir kullanıcı daha önce devredilen kaynaklara erişemez.

[Yönetilen Hizmetler Kaydı Atama Silme Rolü'ne](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) sahip kullanıcılara Azure temsilcikaynak yönetimi için bir müşteriye binerken bindiyseniz, bu kullanıcılar de delegasyonu kaldırabilecektir.

Aşağıdaki örnekte, **parametre** dosyasına dahil edilebilen Yönetilen Hizmetler Kayıt Atama Silme Rolü veren bir atama gösterilmektedir:

```json
    "authorizations": [ 
        { 
            "principalId": "cfa7496e-a619-4a14-a740-85c5ad2063bb", 
            "principalIdDisplayName": "MSP Operators", 
            "roleDefinitionId": "91c1777a-f3dc-4fae-b103-61d183457e46" 
        } 
    ] 
```

Bu izne sahip bir kullanıcı bir temsilciliği aşağıdaki yollardan biriyle kaldırabilir.

### <a name="azure-portal"></a>Azure portal

1. [Müşterilerim sayfasına](view-manage-customers.md)gidin.
2. **Heyetleri**seçin.
3. Kaldırmak istediğiniz delegasyonu bulun ve ardından satırında görünen çöp kutusu simgesini seçin.

### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
# Log in first with Connect-AzAccount if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory 

Login-AzAccount

# Select the subscription that is delegated - or contains the delegated resource group(s)

Select-AzSubscription -SubscriptionName "<subscriptionName>"

# Get the registration assignment

Get-AzManagedServicesAssignment -Scope "/subscriptions/{delegatedSubscriptionId}"

# Delete the registration assignment

Remove-AzManagedServicesAssignment -ResourceId "/subscriptions/{delegatedSubscriptionId}/providers/Microsoft.ManagedServices/registrationAssignments/{assignmentGuid}"
```

### <a name="azure-cli"></a>Azure CLI

```azurecli-interactive
# Log in first with az login if you're not using Cloud Shell

# Sign in as a user from the managing tenant directory

az login

# Select the subscription that is delegated – or contains the delegated resource group(s)

az account set -s <subscriptionId/name>

# List registration assignments

az managedservices assignment list

# Delete the registration assignment

az managedservices assignment delete --assignment <id or full resourceId>
```

## <a name="next-steps"></a>Sonraki adımlar

- Kiracılar [arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure portalındaki **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin.](view-manage-customers.md)
