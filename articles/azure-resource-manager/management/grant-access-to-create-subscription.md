---
title: Azure Kurumsal abonelikleri oluşturma erişimi verme
description: Bir kullanıcıya veya hizmet sorumlusuna, programlı olarak Azure Kurumsal abonelikleri oluşturma yeteneği verme hakkında bilgi edinin.
author: jureid
manager: jureid
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: jureid
ms.openlocfilehash: aef9c6781c87ff4e84e46de711308319755e4630
ms.sourcegitcommit: 1a0dfa54116aa036af86bd95dcf322307cfb3f83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88042080"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure Kurumsal abonelikleri oluşturma erişimi verme (Önizleme)

[Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)üzerinde bir Azure müşterisi olarak, hesabınıza faturalandırılan abonelikler oluşturmak için başka bir kullanıcıya veya hizmet sorumlusu izni verebilirsiniz. Bu makalede, [Azure rol tabanlı erişim denetimi 'ni (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanarak abonelikler oluşturma ve abonelik oluşturma özelliklerini denetleme hakkında bilgi edineceksiniz. Paylaşmak istediğiniz hesapta sahip rolüne sahip olmanız gerekir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="grant-access"></a>Erişim verme

[Bir kayıt hesabı altında abonelik oluşturmak](programmatically-create-subscription.md)için, kullanıcıların bu hesapta [RBAC sahibi rolüne](../../role-based-access-control/built-in-roles.md#owner) sahip olmaları gerekir. Aşağıdaki adımları izleyerek bir kullanıcıya veya kullanıcı grubuna bir kayıt hesabında RBAC sahibi rolü verebilirsiniz:

1. Erişim vermek istediğiniz kayıt hesabının nesne KIMLIĞINI alın

    Diğerlerine bir kayıt hesabında RBAC sahibi rolü vermek için hesabın sahibi veya hesabın RBAC sahibi olmanız gerekir.

    # <a name="rest"></a>[REST](#tab/rest)

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

    `principalName`RBAC sahibine erişim vermek istediğiniz hesabı belirlemek için özelliğini kullanın. `name`Bu hesabın öğesini kopyalayın. Örneğin, kayıt hesabına RBAC sahibi erişimi vermek isterseniz SignUpEngineering@contoso.com , ' yi kopyalamanız gerekir ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Bu, kayıt hesabının nesne KIMLIĞIDIR. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın `enrollmentAccountObjectId` .

    # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

    Erişiminiz olan tüm kayıt hesaplarını listelemek için [Get-Azkayıtları Mentaccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet 'ini kullanın. [Azure Cloud Shell](https://shell.azure.com/)açmak için **deneyin** ' i seçin. Kodu yapıştırmak için kabuk pencerelerini sağ tıklatın ve **Yapıştır**' ı seçin.

    ```azurepowershell-interactive
    Get-AzEnrollmentAccount
    ```

    Azure, erişim sahibi olduğunuz kayıt hesaplarının bir listesi ile yanıt verir:

    ```azurepowershell
    ObjectId                               | PrincipalName
    747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
    4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
    ```

    `principalName`RBAC sahibine erişim vermek istediğiniz hesabı belirlemek için özelliğini kullanın. `ObjectId`Bu hesabın öğesini kopyalayın. Örneğin, kayıt hesabına RBAC sahibi erişimi vermek isterseniz SignUpEngineering@contoso.com , ' yi kopyalamanız gerekir ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Bir sonraki adımda kullanabilmek için bu nesne KIMLIĞINI bir yere yapıştırın `enrollmentAccountObjectId` .

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

    Erişiminiz olan tüm kayıt hesaplarını listelemek için [az faturalandırma kaydı-hesap listesi](https://aka.ms/EASubCreationPublicPreviewCLI) komutunu kullanın. [Azure Cloud Shell](https://shell.azure.com/)açmak için **deneyin** ' i seçin. Kodu yapıştırmak için kabuk pencerelerini sağ tıklatın ve **Yapıştır**' ı seçin.

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

    ---

    `principalName`RBAC sahibine erişim vermek istediğiniz hesabı belirlemek için özelliğini kullanın. `name`Bu hesabın öğesini kopyalayın. Örneğin, kayıt hesabına RBAC sahibi erişimi vermek isterseniz SignUpEngineering@contoso.com , ' yi kopyalamanız gerekir ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . Bu, kayıt hesabının nesne KIMLIĞIDIR. Bir sonraki adımda kullanabilmeniz için bu değeri bir yere yapıştırın `enrollmentAccountObjectId` .

1. <a id="userObjectId"></a>RBAC sahibi rolüne vermek istediğiniz kullanıcının veya grubun nesne KIMLIĞINI al

    1. Azure portal, **Azure Active Directory**arama yapın.
    1. Bir kullanıcı erişimi vermek istiyorsanız, sol taraftaki menüden **Kullanıcılar** ' a tıklayın. Bir gruba erişim vermek istiyorsanız, **gruplar**' a tıklayın.
    1. RBAC sahibi rolüne vermek istediğiniz kullanıcıyı veya grubu seçin.
    1. Bir Kullanıcı seçtiyseniz, profil sayfasında nesne KIMLIĞINI bulacaksınız. Bir grup seçtiyseniz, nesne KIMLIĞI Genel Bakış sayfasında olur. Metin kutusunun sağ tarafındaki simgeye tıklayarak **ObjectID** 'yi kopyalayın. Bunu bir sonraki adımda kullanabilmeniz için bir yere yapıştırın `userObjectId` .

1. Kullanıcıya veya gruba RBAC sahibi rolünü kayıt hesabında verme

    İlk iki adımda topladığınız değerleri kullanarak, kullanıcıya, kayıt hesabında RBAC sahibi rolünü verin veya gruba sahip olursunuz.

    # <a name="rest"></a>[REST](#tab/rest-2)

    ```<enrollmentAccountObjectId>``` `name` İlk adımda () kopyaladığınız ile değiştirerek aşağıdaki komutu çalıştırın ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` . ```<userObjectId>```İkinci adımdan kopyaladığınız nesne kimliğiyle değiştirin.

    ```json
    PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

    {
      "properties": {
        "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
        "principalId": "<userObjectId>"
      }
    }
    ```

    Sahip rolü kayıt hesabı kapsamında başarıyla atandığında, Azure, rol atamasının bilgileriyle yanıt verir:

    ```json
    {
      "properties": {
        "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
        "principalId": "<userObjectId>",
        "scope": "/providers/Microsoft.Billing/enrollmentAccounts/747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "createdOn": "2018-03-05T08:36:26.4014813Z",
        "updatedOn": "2018-03-05T08:36:26.4014813Z",
        "createdBy": "<assignerObjectId>",
        "updatedBy": "<assignerObjectId>"
      },
      "id": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "<roleAssignmentGuid>"
    }
    ```

    # <a name="powershell"></a>[PowerShell](#tab/azure-powershell-2)

    İlk adımda toplanan ile değiştirerek aşağıdaki [New-Azroleatama](../../role-based-access-control/role-assignments-powershell.md) komutunu çalıştırın ```<enrollmentAccountObjectId>``` `ObjectId` ( ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ). ```<userObjectId>```İkinci adımda toplanan nesne kimliğiyle değiştirin.

    ```azurepowershell-interactive
    New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli-2)

    Aşağıdaki [az role atama Create](../../role-based-access-control/role-assignments-cli.md) komutunu çalıştırın, ```<enrollmentAccountObjectId>``` `name` ilk adımda kopyaladığınız ile değiştirin ( ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ). ```<userObjectId>```İkinci adımda toplanan nesne kimliğiyle değiştirin.

    ```azurecli-interactive
    az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    Bir kullanıcı kayıt hesabınız için RBAC sahibi olduktan sonra, bu kullanıcılar [Program aracılığıyla abonelik oluşturabilir](programmatically-create-subscription.md) . Temsilci Kullanıcı tarafından oluşturulan bir abonelik hala hizmet yöneticisi olarak özgün hesap sahibine sahiptir, ancak aynı zamanda temsilci Kullanıcı varsayılan olarak bir RBAC sahibi olarak da bulunur.

    ---

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Etkinlik günlüklerini kullanarak kimin abonelik oluşturduğunu denetleme

Bu API aracılığıyla oluşturulan abonelikleri izlemek için [kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs)'sini kullanın. Abonelik oluşturmayı izlemek için şu anda PowerShell, CLı veya Azure portal kullanmak mümkün değildir.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../../role-based-access-control/elevate-access-global-admin.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın. Bu erişim [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü, [izleme katılımcısı](../../role-based-access-control/built-in-roles.md#monitoring-contributor) rolü veya [özel bir rol](../../role-based-access-control/custom-roles.md)tarafından kullanılabilir.
1. Denetim kullanıcısı olarak, abonelik oluşturma etkinliklerini görmek için [kiracı etkinlik günlüğü API](/rest/api/monitor/tenantactivitylogs) 'sini çağırın. Örnek:

    ```
    GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'"
    ```

Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* Artık Kullanıcı veya hizmet sorumlusunun bir abonelik oluşturma izni olduğuna göre, bu kimliği kullanarak [Azure Enterprise abonelikleri 'ni programlı](programmatically-create-subscription.md)bir şekilde oluşturabilirsiniz.
* .NET kullanarak abonelikler oluşturma hakkında bir örnek için bkz. [GitHub üzerinde örnek kod](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core).
* Azure Resource Manager ve API 'Leri hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager genel bakış](overview.md).
* Yönetim gruplarını kullanarak çok sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için bkz. [Azure Yönetim gruplarıyla kaynaklarınızı düzenleme](../../governance/management-groups/overview.md)
* Abonelik idaresinde büyük kuruluşlar için kapsamlı en iyi yöntem kılavuzunu görmek için bkz. [Azure Kurumsal yapı iskelesi-açık abonelik](/azure/architecture/cloud-adoption-guide/subscription-governance) İdaresi
