---
title: Azure Monitor ile Azure AD B2C'yi izleyin
titleSuffix: Azure AD B2C
description: Temsilci kaynak yönetimini kullanarak Azure Monitor ile Azure AD B2C etkinliklerini nasıl günlüğe kaydedin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.author: mimart
ms.subservice: B2C
ms.date: 02/10/2020
ms.openlocfilehash: acba378badb41324b2124b84833407da920a0e00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190067"
---
# <a name="monitor-azure-ad-b2c-with-azure-monitor"></a>Azure Monitor ile Azure AD B2C'yi izleyin

Azure Active Directory B2C (Azure AD B2C) oturum açma ve [denetim](view-audit-logs.md) günlüklerini farklı izleme çözümlerine yönlendirmek için Azure Monitor'u kullanın. Uzun süreli kullanım için günlükleri saklayabilir veya ortamınız hakkında bilgi edinmek için üçüncü taraf güvenlik bilgileri ve olay yönetimi (SIEM) araçlarıyla entegre olabilirsiniz.

Günlük olaylarını şu şekilde yönlendirebilirsiniz:

* Bir Azure [depolama hesabı.](../storage/blobs/storage-blobs-introduction.md)
* Azure [olay hub'ı](../event-hubs/event-hubs-about.md) (ve Splunk ve Sumo Logic örneklerinizle tümleştirin).
* [Bir Günlük Analizi çalışma alanı](../azure-monitor/platform/resource-logs-collect-workspace.md) (verileri analiz etmek, panolar oluşturmak ve belirli olaylar hakkında uyarı için).

![Azure İzleyici](./media/azure-monitor/azure-monitor-flow.png)

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için, Azure PowerShell modüllerini kullanarak bir Azure Kaynak Yöneticisi şablonu dağıtırsınız.

* [Azure PowerShell modül](https://docs.microsoft.com/powershell/azure/install-az-ps) sürümü 6.13.1 veya üzeri

Azure [PowerShell](https://shell.azure.com)modülünün en son sürümünü içeren Azure Bulut Kabuğu'nu da kullanabilirsiniz.

## <a name="delegated-resource-management"></a>Temsilci kaynak yönetimi

Azure AD B2C, [Azure Active Directory denetiminde](../active-directory/reports-monitoring/overview-monitoring.md)nsağlar. Azure AD B2C kiracınızda Azure Etkin Dizini'nde *Tanılama ayarlarını* etkinleştirmek için [temsilci kaynak yönetimini](../lighthouse/concepts/azure-delegated-resource-management.md)kullanırsınız.

Azure AD B2C dizininizdeki **(Servis Sağlayıcı)** bir kullanıcıveya gruba, Azure aboneliğinizi **(Müşteri)** içeren kiracı içindeki Azure Monitör örneğini yapılandırması için yetki verirsiniz. Yetkilendirmeyi oluşturmak için, aboneliği içeren Azure AD kiracınıza bir [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml) şablonu dağıtAbilirsiniz. Aşağıdaki bölümler size süreç boyunca yol.

## <a name="create-or-choose-resource-group"></a>Kaynak grubu oluşturma veya seçme

Bu, Azure Monitor'dan veri almak için hedef Azure depolama hesabını, etkinlik merkezini veya Log Analytics çalışma alanını içeren kaynak grubudur. Azure Kaynak Yöneticisi şablonu dağıtırken kaynak grubu adını belirtirsiniz.

[Bir kaynak grubu oluşturun](../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups) veya Azure AD B2C kiracınızı içeren dizini *değil,* Azure Aboneliğinizi içeren Azure Etkin Dizin (Azure AD) kiracısında varolan bir grup seçin.

Bu *örnekte, Orta ABD* bölgesinde *azure-ad-b2c-monitor* adlı bir kaynak grubu kullanır.

## <a name="delegate-resource-management"></a>Temsilci kaynak yönetimi

Ardından, aşağıdaki bilgileri toplayın:

Azure AD B2C dizininizin **dizin kimliği** (kiracı kimliği olarak da bilinir).

1. *Kullanıcı yöneticisi* rolüne (veya daha yüksek bir kullanıcıya) sahip bir kullanıcı olarak [Azure portalında](https://portal.azure.com/) oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. **Azure Etkin Dizini'ni**seçin, **Özellikler'i**seçin.
1. **Dizin Kimliğini kaydedin.**

Azure AD B2C grubunun veya kullanıcının **nesne kimliği,** aboneliğinizi içeren dizinde daha önce oluşturduğunuz kaynak grubuna *Katkıda Bulunan'a* izin vermek ister.

Yönetimi kolaylaştırmak için, her rol için Azure AD kullanıcı *gruplarını* kullanmanızı ve izinleri doğrudan bu kullanıcıya atamak yerine tek tek kullanıcıları gruba eklemenize veya kaldırmanıza olanak tanır. Bu gözden geçirme de, bir kullanıcı ekleyin.

1. Azure portalında hala seçilen **Azure Etkin Dizini** ile **Kullanıcılar'ı**seçin ve ardından bir kullanıcı seçin.
1. Kullanıcının Nesne **Kimliğini**kaydedin.

### <a name="create-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu oluşturma

Azure AD kiracınızda **(Müşteri)** bulunan aylak, teklifiniz için aşağıdaki bilgileri içeren bir [Azure Kaynak Yöneticisi şablonu](../lighthouse/how-to/onboard-customer.md) oluşturun. Azure `mspOfferName` `mspOfferDescription` portalının [Hizmet sağlayıcıları sayfasında](../lighthouse/how-to/view-manage-service-providers.md) teklif ayrıntılarını görüntülediğinizde ve değerleri görünür.

| Alan   | Tanım |
|---------|------------|
| `mspOfferName`                     | Bu tanımı açıklayan bir ad. Örneğin, *Azure AD B2C Yönetilen Hizmetler.* Bu değer, teklifin başlığı olarak müşteriye görüntülenir. |
| `mspOfferDescription`              | Teklifinizin kısa bir açıklaması. Örneğin, *Azure AD B2C'de Azure Monitörünü etkinleştirir.*|
| `rgName`                           | Azure AD kiracınızda daha önce oluşturduğunuz kaynak grubunun adı. Örneğin, *azure-ad-b2c-monitor*. |
| `managedByTenantId`                | Azure AD B2C kiracınızın **Dizin Kimliği** (kiracı kimliği olarak da bilinir). |
| `authorizations.value.principalId` | B2C grubunun veya bu Azure aboneliğindeki kaynaklara erişimi olacak kullanıcının **Nesne Kimliği.** Bu gözden geçirme için, daha önce kaydettiğiniz kullanıcının Nesne Kimliğini belirtin. |

Azure Kaynak Yöneticisi şablonu ve parametre dosyalarını indirin:

- [rgDelegeKaynak Yönetimi.json](https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)
- [rgDelegeDResourceManagement.parameters.json](https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)

Ardından, parametreler dosyasını daha önce kaydettiğiniz değerlerle güncelleştirin. Aşağıdaki JSON snippet, Azure Kaynak Yöneticisi şablon parametreleri dosyasının bir örneğini gösterir. Bunun `authorizations.value.roleDefinitionId`için, *Katılımcı rolü*için [yerleşik](../role-based-access-control/built-in-roles.md) rol `b24988ac-6180-42a0-ab88-20f7382dd24c`değerini kullanın .

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "mspOfferName": {
            "value": "Azure AD B2C Managed Services"
        },
        "mspOfferDescription": {
            "value": "Enables Azure Monitor in Azure AD B2C"
        },
        "rgName": {
            "value": "azure-ad-b2c-monitor"
        },
        "managedByTenantId": {
            "value": "<Replace with DIRECTORY ID of Azure AD B2C tenant (tenant ID)>"
        },
        "authorizations": {
            "value": [
                {
                    "principalId": "<Replace with user's OBJECT ID>",
                    "principalIdDisplayName": "Azure AD B2C tenant administrator",
                    "roleDefinitionId": "b24988ac-6180-42a0-ab88-20f7382dd24c"
                }
            ]
        }
    }
}
```

### <a name="deploy-the-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını dağıtma

Parametreler dosyanızı güncelledikten sonra, Azure Kaynak Yöneticisi şablonunu abonelik düzeyinde bir dağıtım olarak Azure Kiracısına dağıtın. Bu abonelik düzeyinde bir dağıtım olduğundan, Azure portalında başlatılamaz. Azure PowerShell modüllerini veya Azure CLI'yi kullanarak dağıtabilirsiniz. Azure PowerShell yöntemi aşağıda gösterilmiştir.

[Connect-AzAccount'ı](/powershell/azure/authenticate-azureps)kullanarak aboneliğinizi içeren dizinde oturum açın. Doğru `-tenant` dizin kimlik doğrusına kimlik doğrulama zorlamak için bayrağı kullanın.

```PowerShell
Connect-AzAccount -tenant contoso.onmicrosoft.com
```

Geçerli hesabın Azure AD kiracıaltında erişebileceği abonelikleri listelemek için [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) cmdlet'ini kullanın. Azure AD B2C kiracınıza yansıtmak istediğiniz aboneliğin kimliğini kaydedin.

```PowerShell
Get-AzSubscription
```

Ardından, Azure AD B2C kiracısına yansıtmak istediğiniz aboneliğe geçin:

``` PowerShell
Select-AzSubscription <subscription ID>
```

Son olarak, daha önce indirdiğiniz ve güncelleştirdiğiniz Azure Kaynak Yöneticisi şablonu ve parametre dosyalarını dağıtın. Buna `Location`göre `TemplateFile`, `TemplateParameterFile` ve değerleri değiştirin.

```PowerShell
New-AzDeployment -Name "AzureADB2C" `
                 -Location "centralus" `
                 -TemplateFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.json" `
                 -TemplateParameterFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.parameters.json" `
                 -Verbose
```

Şablonun başarılı dağıtımı aşağıdakine benzer çıktı üretir (kısalık için kesilen çıktı):

```Console
PS /usr/csuser/clouddrive> New-AzDeployment -Name "AzureADB2C" `
>>                  -Location "centralus" `
>>                  -TemplateFile "rgDelegatedResourceManagement.json" `
>>                  -TemplateParameterFile "rgDelegatedResourceManagement.parameters.json" `
>>                  -Verbose
WARNING: Breaking changes in the cmdlet 'New-AzDeployment' :
WARNING:  - The cmdlet 'New-AzSubscriptionDeployment' is replacing this cmdlet.


WARNING: NOTE : Go to https://aka.ms/azps-changewarnings for steps to suppress this breaking change warning, and other information on breaking changes in Azure PowerShell.
VERBOSE: 7:25:14 PM - Template is valid.
VERBOSE: 7:25:15 PM - Create template deployment 'AzureADB2C'
VERBOSE: 7:25:15 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:42 PM - Resource Microsoft.ManagedServices/registrationDefinitions '44444444-4444-4444-4444-444444444444' provisioning status is succeeded
VERBOSE: 7:25:48 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:53 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is running
VERBOSE: 7:25:53 PM - Checking deployment status in 5 seconds
VERBOSE: 7:25:59 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is running
VERBOSE: 7:26:17 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:23 PM - Resource Microsoft.ManagedServices/registrationAssignments '11111111-1111-1111-1111-111111111111' provisioning status is succeeded
VERBOSE: 7:26:23 PM - Checking deployment status in 5 seconds
VERBOSE: 7:26:29 PM - Resource Microsoft.Resources/deployments 'rgAssignment' provisioning status is succeeded

DeploymentName          : AzureADB2C
Location                : centralus
ProvisioningState       : Succeeded
Timestamp               : 1/31/20 7:26:24 PM
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name                   Type                       Value
                          =====================  =========================  ==========
                          mspOfferName           String                     Azure AD B2C Managed Services
                          mspOfferDescription    String                     Enables Azure Monitor in Azure AD B2C
...
```

Şablonu dağıttıktan sonra kaynak projeksiyonunun tamamlanması birkaç dakika sürebilir. Aboneliği seçmek için bir sonraki bölüme geçmeden önce birkaç dakika (genellikle en fazla beş dakika) beklemeniz gerekebilir.

## <a name="select-your-subscription"></a>Aboneliğinizi seçme

Şablonu dağıttıktan ve kaynak projeksiyonunun tamamlanması için birkaç dakika bekledikten sonra aboneliğinizi Aşağıdaki adımlarla Azure AD B2C dizininize ilişkilendirin.

1. Şu anda oturum açmışsanız Azure portalından **çıkış kaydedin.** Bu ve aşağıdaki adım, portal oturumunda kimlik bilgilerinizi yenilemek için yapılır.
1. Azure AD B2C yönetim hesabınızla [Azure portalında](https://portal.azure.com) oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin.
1. Aboneliğinizi içeren dizini seçin.

    ![Dizini değiştirin](./media/azure-monitor/azure-monitor-portal-03-select-subscription.png)
1. Doğru dizini ve aboneliği seçtiğinizi doğrulayın. Bu örnekte, tüm dizinler ve abonelikler seçilir.

    ![Dizin & Abonelik filtresinde seçilen tüm dizinler](./media/azure-monitor/azure-monitor-portal-04-subscriptions-selected.png)

## <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma

Tanılama ayarları, bir kaynağın günlüklerinin ve ölçümlerinin nereye gönderilmesi gerektiğini tanımlar. Olası varış noktaları şunlardır:

- [Azure depolama hesabı](../azure-monitor/platform/resource-logs-collect-storage.md)
- [Olay hub çözümleri.](../azure-monitor/platform/resource-logs-stream-event-hubs.md)
- [Log Analytics çalışma alanı](../azure-monitor/platform/resource-logs-collect-workspace.md)

Henüz yapmadıysanız, [Azure Kaynak Yöneticisi şablonunda](#create-an-azure-resource-manager-template)belirttiğiniz kaynak grubunda seçtiğiniz hedef türünden bir örnek oluşturun.

### <a name="create-diagnostic-settings"></a>Tanılama ayarları oluşturma

Azure portalında [tanılama ayarları oluşturmaya](../active-directory/reports-monitoring/overview-monitoring.md) hazırsınız.

Azure AD B2C etkinlik günlükleri için izleme ayarlarını yapılandırmak için:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Portal araç çubuğundaki **Dizin + Abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. **Azure Etkin Dizini'ni** seçin
1. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.
1. Kaynakta varolan ayarlar varsa, önceden yapılandırılmış ayarların listesini görürsünüz. Yeni bir ayar eklemek için **tanı ayar ekle'yi** seçin veya varolan bir ayarı ayarlamak için **ayarını edin.** Her ayar, hedef türlerinin her birinden birden fazla olamaz...

    ![Azure portalında tanılama ayarları bölmesi](./media/azure-monitor/azure-monitor-portal-05-diagnostic-settings-pane-enabled.png)

1. Ayarınızda zaten bir ad yoksa bir ad verin.
1. Günlükleri göndermek için her hedef için kutuyu işaretleyin. Aşağıdaki tabloda açıklandığı gibi ayarlarını belirtmek için **Yapıya'yı** seçin.

    | Ayar | Açıklama |
    |:---|:---|
    | Bir depolama hesabına arşivle | Depolama hesabının adı. |
    | Bir olay hub'ına akış yap | Olay hub'ınoluşturulduğu ad alanı (bu günlükleri ilk kez akışınız saise) veya akış (bu günlük kategorisine zaten akış yapan kaynaklar varsa).
    | Log Analytics’e gönderme | Çalışma alanının adı. |

1. **Denetim Günlükleri** ve **SignInLogs**seçin.
1. **Kaydet'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Monitor'da tanılama ayarları ekleme ve yapılandırma hakkında daha fazla bilgi için [Bkz.](../azure-monitor/insights/monitor-azure-resource.md)

Azure AD günlüklerini bir etkinlik merkezine akış hakkında bilgi [için](../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)bkz.
