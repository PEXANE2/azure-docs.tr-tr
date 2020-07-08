---
title: Bir müşteriyi Azure tarafından atanan temsilcinin kaynak yönetimine ekleme
description: Azure 'un Temsilcili kaynak yönetimine nasıl bir müşteri ekleneceğini ve bunların kendi kiracınız aracılığıyla kaynaklarına erişilmesine ve yönetilmesine izin vermeyi öğrenin.
ms.date: 05/26/2020
ms.topic: how-to
ms.openlocfilehash: 149398a822d5aa21335be4122e92c96800d94255
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920919"
---
# <a name="onboard-a-customer-to-azure-delegated-resource-management"></a>Bir müşteriyi Azure tarafından atanan temsilcinin kaynak yönetimine ekleme

Bu makalede, bir hizmet sağlayıcı olarak, bir müşteriyi Azure tarafından atanan kaynak yönetimine ekleme, temsilcili kaynakların (abonelikler ve/veya kaynak grupları) kendi Azure Active Directory (Azure AD) kiracınız aracılığıyla erişilebilir ve yönetilme olanağı sağlayan açıklanır. Burada hizmet sağlayıcılarına ve müşterilere başvuracağız. [birden çok kiracıyı yöneten kuruluşlar](../concepts/enterprise.md) , Azure açık thouse 'ı kurmak ve yönetim deneyimlerini birleştirmek için aynı süreci kullanabilir.

Birden çok müşteri için kaynakları yönetiyorsanız, bu işlemi yineleyebilirsiniz. Daha sonra, yetkili bir Kullanıcı kiracınızda oturum açtığında, bu kullanıcı, her müşteri kiracısında oturum açmaya gerek kalmadan yönetim işlemlerini gerçekleştirmek üzere müşteri kiracı kapsamları genelinde yetkilendiribiliyor olabilir.

Müşteri görevlendirmeleri genelinde etkileri izlemek ve tanıma almak için, Microsoft İş Ortağı Ağı (MPN) KIMLIĞINIZI eklendi aboneliklerinizin her birine erişimi olan en az bir kullanıcı hesabıyla ilişkilendirin. Bu ilişkilendirmeyi hizmet sağlayıcı kiracınızda gerçekleştirmeniz gerektiğini unutmayın. Basitlik için, kiracınızda MPN KIMLIĞINIZLE ilişkilendirilen bir hizmet sorumlusu hesabı oluşturmanızı ve BT okuyucuyu eklediğiniz her müşteri için erişim izni verilmesini öneririz. Daha fazla bilgi için bkz. [Azure hesaplarınıza bir iş ortağı kimliği bağlama](../../billing/billing-partner-admin-link-started.md). 

> [!NOTE]
> Müşteriler, Azure Marketi 'Nde yayımladığınız yönetilen bir hizmet teklifi (genel veya özel) satın alındığında da eklendi. Daha fazla bilgi için bkz. [yönetilen hizmet tekliflerini Azure Marketi 'Nde yayımlama](publish-managed-services-offers.md). Burada açıklanan ekleme işlemini, Azure Marketi 'Nde yayınlanan bir teklifle birlikte da kullanabilirsiniz.

Ekleme işlemi, eylemlerin hem hizmet sağlayıcının kiracısından hem de müşterinin kiracısından alınması gerekir. Bu adımların tümü bu makalede açıklanmıştır.

## <a name="gather-tenant-and-subscription-details"></a>Kiracı ve abonelik ayrıntılarını toplayın

Bir müşterinin kiracısını eklemek için etkin bir Azure aboneliğine sahip olması gerekir. Şunları bilmeniz gerekir:

- Hizmet sağlayıcısının kiracının kiracı KIMLIĞI (müşterinin kaynaklarını yönettiğiniz yer)
- Müşterinin kiracının kiracı KIMLIĞI (hizmet sağlayıcısı tarafından yönetilen kaynaklar olacaktır)
- Müşterinin kiracısındaki, hizmet sağlayıcısı tarafından yönetilecek (veya hizmet sağlayıcısı tarafından yönetilecek kaynak gruplarını içeren) her bir abonelik için abonelik kimlikleri.

> [!NOTE]
> Bir abonelik içinde yalnızca bir veya daha fazla kaynak grubu eklemek isteseniz bile, dağıtımın abonelik düzeyinde yapılması gerekir, bu nedenle abonelik KIMLIĞI gereklidir.

Bu KIMLIK değerleri zaten yoksa, bunları aşağıdaki yollarla alabilirsiniz. Dağıtımınızda bu tam değerleri kullandığınızdan emin olun ve bunları kullanın.

### <a name="azure-portal"></a>Azure portal

Kiracı KIMLIĞINIZ, Azure portal sağ üst tarafındaki hesap adınızın üzerine gelerek veya **Dizin Değiştir**' i seçerek görülebilir. Kiracı KIMLIĞINIZI seçmek ve kopyalamak için Portal içinden "Azure Active Directory" araması yapın, ardından **Özellikler** ' i seçin ve **dizin kimliği** alanında gösterilen değeri kopyalayın. Müşterinin kiracısında bir aboneliğin KIMLIĞINI bulmak için, "abonelikler" araması yapın ve ardından uygun abonelik KIMLIĞINI seçin.

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
> Burada açıklanan işlemi kullanarak abonelik (veya bir abonelik içindeki bir veya daha fazla kaynak grubu) ekleme sırasında, **Microsoft. ManagedServices** kaynak sağlayıcısı bu abonelik için kaydedilir.

## <a name="define-roles-and-permissions"></a>Rolleri ve izinleri tanımlama

Hizmet sağlayıcı olarak, farklı kapsamlar için farklı erişim gerektiren tek bir müşteri için birden çok görev gerçekleştirmek isteyebilirsiniz. Kiracınızdaki kullanıcılara [rol tabanlı erişim denetimi (RBAC) yerleşik rolleri](../../role-based-access-control/built-in-roles.md) atamak için gereken sayıda Yetkilendirme tanımlayabilirsiniz.

Yönetimi kolaylaştırmak için, her rol için Azure AD Kullanıcı grupları kullanmanızı öneririz. böylece, izinleri doğrudan bu kullanıcıya atamak yerine gruba bireysel kullanıcı ekleyebilir veya kaldırabilirsiniz. Ayrıca, bir hizmet sorumlusuna roller atamak isteyebilirsiniz. Kullanıcıların yalnızca işlerini tamamlaması için gerekli izinlere sahip olması için en az ayrıcalık ilkesini izlediğinizden emin olun. Desteklenen roller hakkında öneriler ve bilgiler için bkz. [Azure açık bir senaryolarda kiracılar, kullanıcılar ve roller](../concepts/tenants-users-roles.md).

> [!IMPORTANT]
> Bir Azure AD grubu için izinler eklemek üzere, **Grup türü** **güvenlik** olmalıdır ve **Office 365**' i değil. Grup oluşturulduğunda bu seçenek seçilidir. Daha fazla bilgi için bkz. [temel Grup oluşturma ve Azure Active Directory kullanarak üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Yetkilendirmeleri tanımlamak için, erişim vermek istediğiniz hizmet sağlayıcı kiracısında her bir Kullanıcı, Kullanıcı grubu veya hizmet sorumlusu için KIMLIK değerlerini bilmeniz gerekir. Ayrıca, atamak istediğiniz her bir yerleşik rol için rol tanımı KIMLIĞI gerekir. Daha önceden sahip değilseniz, hizmet sağlayıcı kiracısı içinden aşağıdaki komutları çalıştırarak bunları alabilirsiniz.

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
> Bir müşteriyi eklerken [yönetilen hizmetler kayıt ataması silme rolünü](../../role-based-access-control/built-in-roles.md#managed-services-registration-assignment-delete-role) atamanız önerilir, böylece kiracınızdaki kullanıcılar gerekirse daha sonra [temsilciye erişimi kaldırabilirler](remove-delegation.md) . Bu rol atanmamışsa, atanan kaynaklar yalnızca müşterinin kiracısındaki bir kullanıcı tarafından kaldırılabilir.

## <a name="create-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu oluşturma

Müşterinize eklemek için aşağıdaki bilgilerle teklifiniz için bir [Azure Resource Manager](../../azure-resource-manager/index.yml) şablonu oluşturmanız gerekir. Azure portal [hizmet sağlayıcıları sayfasında](view-manage-service-providers.md) teklif ayrıntılarını görüntülerken, **mspoffername** ve **mspofferdescription** değerleri müşteriye görünür olacaktır.

|Alan  |Tanım  |
|---------|---------|
|**mspOfferName**     |Bu tanımı açıklayan bir ad. Bu değer, müşteriye teklifin başlığı olarak gösterilir.         |
|**mspOfferDescription**     |Teklifinizin kısa bir açıklaması (örneğin, "contoso VM yönetimi teklifi").      |
|**Managedbytenantıd**     |Kiracı KIMLIĞINIZ.          |
|**yetkilendirmeleri**     |Kiracınızdaki kullanıcılar/gruplar/SPN 'Ler için **PrincipalId** değerleri, müşterinizin yetkilendirmesinin amacını anlamasına **yardımcı olmak ve** erişim düzeyini belirtmek Için yerleşik bir **roledefinitionıd** değeri ile eşleştirilir.      |

Ekleme işlemi, bir Azure Resource Manager şablonu ( [örnek](https://github.com/Azure/Azure-Lighthouse-samples/)depolarımızda sağlanan) ve yapılandırmanızla eşleşecek şekilde değiştirdiğiniz karşılık gelen bir parametre dosyası gerektirir ve yetkilendirmeleri tanımlar.

Seçtiğiniz şablon, bir aboneliğin tüm aboneliğini, kaynak grubunu veya birden çok kaynak grubunu mı, yoksa bir abonelik içinde mi sundığınıza bağlıdır. Ayrıca, aboneliklerini bu şekilde eklemek isterseniz, Azure Marketi 'Nde yayımladığınız yönetilen hizmet teklifini satın alan müşteriler için kullanılabilecek bir şablon sunuyoruz.

|Bunu eklemek için  |Bu Azure Resource Manager şablonunu kullan  |Ve bu parametre dosyasını Değiştir |
|---------|---------|---------|
|Abonelik   |[ÜzerindedelegatedResourceManagement.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management/delegatedResourceManagement.json)  |[ÜzerindedelegatedResourceManagement.parameters.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/delegated-resource-management/delegatedResourceManagement.parameters.json)    |
|Kaynak grubu   |[ÜzerindergDelegatedResourceManagement.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)  |[ÜzerindergDelegatedResourceManagement.parameters.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)    |
|Abonelik içindeki birden fazla kaynak grubu   |[ÜzerindemultipleRgDelegatedResourceManagement.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.json)  |[ÜzerindemultipleRgDelegatedResourceManagement.parameters.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/rg-delegated-resource-management/multipleRgDelegatedResourceManagement.parameters.json)    |
|Abonelik (Azure Marketi 'Nde yayınlanan bir teklifi kullanırken)   |[ÜzerindemarketplaceDelegatedResourceManagement.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.json)  |[ÜzerindemarketplaceDelegatedResourceManagement.parameters.js](https://github.com/Azure/Azure-Lighthouse-samples/blob/master/templates/marketplace-delegated-resource-management/marketplaceDelegatedResourceManagement.parameters.json)    |

> [!IMPORTANT]
> Burada açıklanan işlem, aynı müşteri kiracısına abonelik ekleme olsanız bile, her abonelik için eklendi olan ayrı bir abonelik düzeyinde dağıtım gerektirir. Aynı müşteri kiracısında farklı abonelikler içinde birden fazla kaynak grubu eklediyseniz ayrı dağıtımlar da gereklidir. Ancak, tek bir abonelik içinde birden fazla kaynak grubu ekleme, tek bir abonelik düzeyinde dağıtımda yapılabilir.
>
> Aynı aboneliğe (veya bir abonelik içindeki kaynak gruplarına) uygulanan birden çok teklif için ayrı dağıtımlar da gerekir. Uygulanan her teklifin farklı bir **Mspoffername**kullanması gerekir.

Aşağıdaki örnek, bir aboneliği eklemek için kullanılabilecek **delegatedResourceManagement.parameters.js** değiştirilmiş bir dosya gösterir. Kaynak grubu parametre dosyaları ( [RG-Temsilcili-kaynak-yönetim](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/rg-delegated-resource-management) klasöründe bulunur) benzerdir, ancak aynı zamanda eklendi olacak belirli kaynak gruplarını belirlemek Için bir **RgName** parametresi de içerir.

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

Yukarıdaki örnekteki en son yetkilendirme, Kullanıcı erişimi yönetici rolü (18d7d88d-d35e-4fb5-a5c3-7773c20a72d9) ile bir **PrincipalId** ekliyor. Bu rolü atarken, **Delegatedrotadefinitionıds** özelliğini ve bir veya daha fazla yerleşik rolü de eklemeniz gerekir. Bu yetkilendirmede oluşturulan kullanıcı, bu yerleşik rolleri, [düzeltilen ilkeleri dağıtmak](deploy-policy-remediation.md)için gerekli olan müşteri kiracısında [yönetilen kimliklere](../../active-directory/managed-identities-azure-resources/overview.md) atayabilecektir.  Kullanıcı da destek olayları oluşturabilir.  Normalde Kullanıcı erişimi Yöneticisi rolüyle ilişkili başka hiçbir izin bu kullanıcı için uygulanacaktır.

## <a name="deploy-the-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını dağıtma

Parametre dosyanızı güncelleştirdikten sonra, müşterinin kiracısındaki bir kullanıcının kiracı içinde Azure Resource Manager şablonunu abonelik düzeyinde bir dağıtım olarak dağıtması gerekir. Azure 'un Temsilcili kaynak yönetimine eklemek istediğiniz her abonelik için ayrı bir dağıtım gerekir (veya eklemek istediğiniz kaynak gruplarını içeren her abonelik için).

Bu, abonelik düzeyinde bir dağıtım olduğundan Azure portal başlatılamaz. Dağıtım, aşağıda gösterildiği gibi PowerShell veya Azure CLı kullanılarak yapılabilir.

> [!IMPORTANT]
> Bu abonelik düzeyi dağıtım, eklendi olan abonelik için [sahip yerleşik rolüne](../../role-based-access-control/built-in-roles.md#owner) sahip olan (veya eklendi olan kaynak gruplarını içeren) müşterinin kiracısında Konuk olmayan bir hesap tarafından yapılmalıdır. Aboneliği temsil edebilen tüm kullanıcıları görmek için, müşterinin kiracısındaki bir Kullanıcı Azure portal aboneliği seçebilir, **erişim denetimini (IAM)** açabilir ve [sahip rolüne sahip tüm kullanıcıları görüntüleyebilir](../../role-based-access-control/role-assignments-list-portal.md#list-owners-of-a-subscription).
>
> Abonelik, [bulut çözümü sağlayıcısı (CSP) programı](../concepts/cloud-solution-provider.md)aracılığıyla oluşturulduysa, hizmet sağlayıcı kiracınızda [yönetici Aracısı](/partner-center/permissions-overview#manage-commercial-transactions-in-partner-center-azure-ad-and-csp-roles) rolüne sahip olan tüm kullanıcılar dağıtımı gerçekleştirebilir.

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

## <a name="confirm-successful-onboarding"></a>Başarılı ekleme Onayla

Bir müşteri aboneliği, Azure tarafından atanan kaynak yönetimine başarıyla eklendi, hizmet sağlayıcının kiracısındaki kullanıcılar aboneliği ve kaynaklarını görebilir (tek tek veya bir Azure AD grubunun bir üyesi olarak, uygun izinlerle birlikte bu gruba erişim verildiyse). Bunu onaylamak için, aboneliğin aşağıdaki yollarla göründüğünden emin olun.  

### <a name="azure-portal"></a>Azure portal

Hizmet sağlayıcısının kiracısında:

1. [Müşterilerimiz sayfasına](view-manage-customers.md)gidin.
2. **Müşteriler**' i seçin.
3. Kaynak Yöneticisi şablonunda verdiğiniz teklif adı ile abonelik (ler) i görmek istediğinizi onaylayın.

> [!IMPORTANT]
> [Müşterilerimde](view-manage-customers.md)Temsilcili abonelik görmek için, hizmet sağlayıcının kiracısındaki kullanıcılara, abonelik Azure tarafından atanan kaynak yönetimi için eklendi olduğunda [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü (veya okuyucu erişimi içeren başka bir yerleşik rol) verilmiş olması gerekir.

Müşterinin kiracısında:

1. [Hizmet sağlayıcıları sayfasına](view-manage-service-providers.md)gidin.
2. **Hizmet sağlayıcısı tekliflerini**seçin.
3. Kaynak Yöneticisi şablonunda verdiğiniz teklif adı ile abonelik (ler) i görmek istediğinizi onaylayın.

> [!NOTE]
> Güncelleştirmeler Azure portal yansıtılmadan önce dağıtımınızın tamamlanması birkaç dakika sürebilir.

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

## <a name="next-steps"></a>Sonraki adımlar

- [Çapraz kiracı yönetim deneyimleri](../concepts/cross-tenant-management-experience.md)hakkında bilgi edinin.
- Azure portal **müşterilerime** giderek [müşterileri görüntüleyin ve yönetin](view-manage-customers.md) .
- Daha önce eklendi [bir temsilciye erişimi kaldırmayı](remove-delegation.md) öğrenin.
