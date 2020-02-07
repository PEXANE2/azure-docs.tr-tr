---
title: Azure Izleyici ile Azure AD B2C izleme
titleSuffix: Azure AD B2C
description: Yetkilendirilmiş kaynak yönetimi 'ni kullanarak Azure Izleyici ile Azure AD B2C olaylarını günlüğe kaydetme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.author: marsma
ms.subservice: B2C
ms.date: 02/05/2020
ms.openlocfilehash: b701449e8cfb7a379522ee6ccb93f5569bd703d8
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77046010"
---
# <a name="monitor-azure-ad-b2c-with-azure-monitor"></a>Azure Izleyici ile Azure AD B2C izleme

Farklı izleme çözümlerine Azure Active Directory B2C (Azure AD B2C) oturum açma ve [Denetim](view-audit-logs.md) günlüklerini yönlendirmek Için Azure izleyici 'yi kullanın. Ortamınız hakkında bilgi edinmek için, uzun süreli kullanım veya üçüncü taraf güvenlik bilgileri ve olay yönetimi (SıEM) araçlarıyla bu günlükleri koruyabilirsiniz.

Günlük olaylarını şu şekilde yönlendirebilirsiniz:

* Bir Azure depolama hesabı.
* Azure Olay Hub 'ı (ve splunk ve sumo Logic Instances ile tümleştirin).
* Azure Log Analytics çalışma alanı (verileri analiz etmek, panolar oluşturmak ve belirli olaylara uyarı vermek için).

![Azure İzleyici](./media/azure-monitor/azure-monitor-flow.png)

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlayabilmeniz için, Azure PowerShell modülünü kullanarak bir Azure Resource Manager şablonu dağıtırsınız.

* [Azure PowerShell Module](https://docs.microsoft.com/powershell/azure/install-az-ps) sürümü 6.13.1 veya üzeri

Azure PowerShell modülünün en son sürümünü içeren [Azure Cloud Shell](https://shell.azure.com)de kullanabilirsiniz.

## <a name="delegated-resource-management"></a>Atanan kaynak yönetimi

Azure AD B2C [Azure Active Directory izlemeyi](../active-directory/reports-monitoring/overview-monitoring.md)kullanır. Azure AD B2C kiracınızda Azure Active Directory *tanılama ayarlarını* etkinleştirmek için, [Temsilcili kaynak yönetimi](../lighthouse/concepts/azure-delegated-resource-management.md)kullanırsınız.

Azure aboneliğinizi ( **Müşteri**) içeren kiracı Içinde Azure izleyici örneğini yapılandırmak için Azure AD B2C dizininizde ( **hizmet sağlayıcısı**) bir kullanıcıyı yetkilendirirsiniz. Yetkilendirmeyi oluşturmak için aboneliği içeren Azure AD kiracınıza bir [Azure Resource Manager](../azure-resource-manager/index.yml) şablonu dağıtırsınız. Aşağıdaki bölümler süreç boyunca size yol gösterir.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure aboneliğinizi içeren Azure Active Directory (Azure AD) kiracısında (Azure AD B2C kiracınızı içeren dizin*değil* ), [bir kaynak grubu oluşturun](../azure-resource-manager/management/manage-resource-groups-portal.md#create-resource-groups). Aşağıdaki değerleri kullanın:

* **Abonelik**: Azure aboneliğinizi seçin.
* **Kaynak grubu**: kaynak grubu için ad girin. Örneğin, *Azure-AD-B2C-Monitor*.
* **Bölge**: bir Azure konumu seçin. Örneğin, *Orta ABD*.

## <a name="delegate-resource-management"></a>Kaynak yönetimini devretmek

Ardından, aşağıdaki bilgileri toplayın:

Azure AD B2C dizininizin **DIZIN kimliği** (Kiracı kimliği olarak da bilinir).

1. Kullanıcı *Yöneticisi* rolüne (veya üzeri) sahip bir kullanıcı olarak [Azure Portal](https://portal.azure.com/) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. **Azure Active Directory**seçin, **Özellikler**' i seçin.
1. **DIZIN kimliğini**kaydedin.

Aboneliğinizi içeren dizinde daha önce oluşturduğunuz kaynak grubuna *katkıda* bulunan izin vermek istediğiniz Azure AD B2C grubunun veya kullanıcının **nesne kimliği** .

Yönetimi kolaylaştırmak için, her rol için Azure AD Kullanıcı *grupları* kullanmanızı öneririz. böylece, izinleri doğrudan bu kullanıcıya atamak yerine gruba bireysel kullanıcı ekleyebilir veya kaldırabilirsiniz. Bu kılavuzda bir Kullanıcı eklersiniz.

1. **Azure Active Directory** Azure Portal hala seçili durumdayken **Kullanıcılar**' ı seçin ve ardından bir kullanıcı seçin.
1. Kullanıcının **nesne kimliğini**kaydedin.

### <a name="create-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu oluşturma

Azure AD kiracınızı ( **Müşteri**) eklemek için aşağıdaki bilgilerle teklifiniz için bir [Azure Resource Manager şablonu](../lighthouse/how-to/onboard-customer.md) oluşturun. `mspOfferName` ve `mspOfferDescription` değerleri, Azure portal [hizmet sağlayıcıları sayfasında](../lighthouse/how-to/view-manage-service-providers.md) teklif ayrıntılarını görüntülediğinizde görülebilir.

| Alan   | Tanım |
|---------|------------|
| `mspOfferName`                     | Bu tanımı açıklayan bir ad. Örneğin, *yönetilen hizmetler Azure AD B2C*. Bu değer, müşteriye teklifin başlığı olarak gösterilir. |
| `mspOfferDescription`              | Teklifinizin kısa bir açıklaması. Örneğin, *Azure AD B2C 'de Azure Izleyicisini etkinleştirilir*.|
| `rgName`                           | Azure AD kiracınızda daha önce oluşturduğunuz kaynak grubunun adı. Örneğin, *Azure-AD-B2C-Monitor*. |
| `managedByTenantId`                | Azure AD B2C kiracınızın **DIZIN kimliği** (Kiracı kimliği olarak da bilinir). |
| `authorizations.value.principalId` | Bu Azure aboneliğindeki kaynaklara erişimi olacak B2C grubunun veya kullanıcının **nesne kimliği** . Bu izlenecek yol için, daha önce kaydettiğiniz kullanıcının nesne KIMLIĞINI belirtin. |

Azure Resource Manager şablonu ve parametre dosyalarını indirin:

- [rgDelegatedResourceManagement. JSON](https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.json)
- [rgDelegatedResourceManagement. Parameters. JSON](https://raw.githubusercontent.com/Azure/Azure-Lighthouse-samples/master/Azure-Delegated-Resource-Management/templates/rg-delegated-resource-management/rgDelegatedResourceManagement.parameters.json)

Daha sonra, Parameters dosyasını daha önce kaydettiğiniz değerlerle güncelleştirin. Aşağıdaki JSON kod parçacığında bir Azure Resource Manager şablon parametreleri dosyası örneği gösterilmektedir. `authorizations.value.roleDefinitionId`için, *katkıda bulunan rolü*için [yerleşik rol](../role-based-access-control/built-in-roles.md) değerini kullanın, `b24988ac-6180-42a0-ab88-20f7382dd24c`.

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

### <a name="deploy-the-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını dağıtma

Parametreleri dosyanızı güncelleştirdikten sonra, Azure Resource Manager şablonunu abonelik düzeyinde bir dağıtım olarak Azure kiracısına dağıtın. Bu, abonelik düzeyinde bir dağıtım olduğundan Azure portal başlatılamaz. Azure PowerShell modülünü veya Azure CLı 'yi kullanarak dağıtabilirsiniz. Azure PowerShell yöntemi aşağıda gösterilmiştir.

[Connect-AzAccount](/powershell/azure/authenticate-azureps)' ı kullanarak aboneliğinizi içeren dizinde oturum açın. Kimlik doğrulamasını doğru dizine zorlamak için `-tenant` bayrağını kullanın.

```PowerShell
Connect-AzAccount -tenant contoso.onmicrosoft.com
```

Geçerli hesabın Azure AD kiracısı altına erişebileceği abonelikleri listelemek için [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) cmdlet 'ini kullanın. Azure AD B2C kiracınıza proje eklemek istediğiniz aboneliğin KIMLIĞINI kaydedin.

```PowerShell
Get-AzSubscription
```

Sonra, Azure AD B2C kiracısına proje eklemek istediğiniz aboneliğe geçin:

``` PowerShell
Select-AzSubscription <subscription ID>
```

Son olarak, daha önce indirdiğiniz ve güncelleştirdiğiniz Azure Resource Manager şablonu ve parametre dosyalarını dağıtın. `Location`, `TemplateFile`ve `TemplateParameterFile` değerlerini uygun şekilde değiştirin.

```PowerShell
New-AzDeployment -Name "AzureADB2C" `
                 -Location "centralus" `
                 -TemplateFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.json" `
                 -TemplateParameterFile "C:\Users\azureuser\Documents\rgDelegatedResourceManagement.parameters.json" `
                 -Verbose
```

Şablonun başarıyla dağıtılması aşağıdakine benzer bir çıktı üretir (breçekimi için çıkış kesilir):

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

Şablonu dağıttıktan sonra, kaynak projeksiyonunun tamamlanması birkaç dakika sürebilir. Aboneliği seçmek için bir sonraki bölüme geçmeden önce birkaç dakika beklemeniz gerekebilir (genellikle beşten fazla olmayan).

## <a name="select-your-subscription"></a>Aboneliğinizi seçme

Şablonu dağıttıktan ve kaynak projeksiyonunun tamamlanmasını birkaç dakika beklemişseniz, aboneliğinizi aşağıdaki adımlarla Azure AD B2C dizininizle ilişkilendirin.

1. Şu anda oturum açtıysanız Azure portal **oturumunuzu kapatın** . Bu ve aşağıdaki adım, Portal oturumunda kimlik bilgilerinizi yenilemek için yapılır.
1. [Azure portal](https://portal.azure.com) Azure AD B2C Yönetici hesabınızla oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin.
1. Aboneliğinizi içeren dizini seçin.

    ![Dizini Değiştir](./media/azure-monitor/azure-monitor-portal-03-select-subscription.png)
1. Doğru dizin ve aboneliği seçtiğinizden emin olun. Bu örnekte, tüm dizinler ve abonelikler seçilidir.

    ![Dizin & Abonelik filtresi 'nde seçilen tüm dizinler](./media/azure-monitor/azure-monitor-portal-04-subscriptions-selected.png)

## <a name="configure-diagnostic-settings"></a>Tanılama ayarlarını yapılandırma

Kaynak yönetimine yetki verdikten ve aboneliğinizi seçtikten sonra, Azure portal [Tanılama ayarları oluşturmaya](../active-directory/reports-monitoring/overview-monitoring.md) hazırsınız demektir.

Azure AD B2C etkinlik günlüklerinin izleme ayarlarını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.
1. **Azure Active Directory** seçin
1. **İzleme**altında **Tanılama ayarları**' nı seçin.
1. **+ Tanılama ayarı Ekle**' yi seçin.

    ![Azure portal Tanılama ayarları bölmesi](./media/azure-monitor/azure-monitor-portal-05-diagnostic-settings-pane-enabled.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Izleyici 'de tanılama ayarlarını ekleme ve yapılandırma hakkında daha fazla bilgi için Azure Izleyici belgelerindeki Bu öğreticiye bakın:

[Öğretici: Azure kaynağından kaynak günlüklerini toplayın ve çözümleyin](/azure-monitor/learn/tutorial-resource-logs.md)
