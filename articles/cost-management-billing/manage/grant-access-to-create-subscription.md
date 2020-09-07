---
title: Azure Kurumsal abonelikleri oluşturma erişimi verme
description: Kullanıcıya veya hizmet sorumlusuna program aracılığıyla Azure Kurumsal abonelikleri oluşturma becerisi vermeyi öğrenin.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.reviewer: amberb
ms.topic: conceptual
ms.date: 08/26/2020
ms.author: banders
ms.openlocfilehash: b154d723e82d02ea864459ef65eb5c05c14ae336
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943200"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure Kurumsal abonelikleri oluşturma erişimi verme (önizleme)

Azure'ın bir [Kurumsal Anlaşma (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) müşterisi olarak başka bir kullanıcıya veya hizmet sorumlusuna sizin hesabınıza faturalanacak abonelikler oluşturma izni verebilirsiniz. Bu makalede [Azure rol tabanlı erişim denetimini (Azure RBAC)](../../role-based-access-control/role-assignments-portal.md) kullanarak abonelik oluşturma becerisini paylaşmayı ve abonelik oluşturma işlemlerini izlemeyi öğreneceksiniz. Paylaşmak istediğiniz hesapta Sahip rolünüz olmalıdır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="grant-access"></a>Erişim verme

[Kayıt hesabı altında abonelikler oluşturmak](programmatically-create-subscription.md) için kullanıcıların söz konusu hesapta [RBAC Sahip rolü](../../role-based-access-control/built-in-roles.md#owner) olmalıdır. Aşağıdaki adımları izleyerek kullanıcıya veya kullanıcı grubuna bir kayıt hesabında RBAC Sahibi rolü verebilirsiniz:

1. Erişim vermek istediğiniz kayıt hesabının nesne kimliğini alma

    Başkalarına kayıt hesabında RBAC Sahibi rolü vermek için hesabın Hesap Sahibi veya bir RBAC Sahibi olmalısınız.

    # <a name="rest"></a>[REST](#tab/rest)

    Erişiminiz olan tüm kayıt hesaplarını listeleme isteği:

    ```json
    GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
    ```

    Azure erişiminiz olan tüm kayıt hesaplarını listeleyerek yanıt verir:

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

    `principalName` özelliğini kullanarak RBAC Sahibi erişimi vermek istediğiniz hesabı belirleyin. Bu hesabın `name` değerini kopyalayın. Örneğin SignUpEngineering@contoso.com kayıt hesabına RBAC Sahibi erişimi vermek istiyorsanız ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` değerini kopyalayın. Bu, kayıt hesabının nesne kimliğidir. Bir sonraki adımda `enrollmentAccountObjectId` olarak kullanabilmeniz için bu değeri bir yere yapıştırın.

    # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

    [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet'ini kullanarak erişiminiz olan tüm kayıt hesaplarını listeleyin. **Deneyin**'i seçerek [Azure Cloud Shell](https://shell.azure.com/)'i açın. Kodu yapıştırmak için Shell penceresine sağ tıklayın ve **Yapıştır**'ı seçin.

    ```azurepowershell-interactive
    Get-AzEnrollmentAccount
    ```

    Azure erişiminiz olan kayıt hesaplarını listeleyerek yanıt verir:

    ```azurepowershell
    ObjectId                               | PrincipalName
    747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
    4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
    ```

    `principalName` özelliğini kullanarak RBAC Sahibi erişimi vermek istediğiniz hesabı belirleyin. Bu hesabın `ObjectId` değerini kopyalayın. Örneğin SignUpEngineering@contoso.com kayıt hesabına RBAC Sahibi erişimi vermek istiyorsanız ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` değerini kopyalayın. Bir sonraki adımda `enrollmentAccountObjectId` olarak kullanabilmeniz için bu nesne kimliğini bir yere yapıştırın.

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

    Erişiminiz olan tüm kayıt hesaplarını listelemek için [az billing enrollment-account list](https://aka.ms/EASubCreationPublicPreviewCLI) komutunu kullanın. **Deneyin**'i seçerek [Azure Cloud Shell](https://shell.azure.com/)'i açın. Kodu yapıştırmak için Shell penceresine sağ tıklayın ve **Yapıştır**'ı seçin.

    ```azurecli-interactive
    az billing enrollment-account list
    ```

    Azure erişiminiz olan kayıt hesaplarını listeleyerek yanıt verir:

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

    `principalName` özelliğini kullanarak RBAC Sahibi erişimi vermek istediğiniz hesabı belirleyin. Bu hesabın `name` değerini kopyalayın. Örneğin SignUpEngineering@contoso.com kayıt hesabına RBAC Sahibi erişimi vermek istiyorsanız ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` değerini kopyalayın. Bu, kayıt hesabının nesne kimliğidir. Bir sonraki adımda `enrollmentAccountObjectId` olarak kullanabilmeniz için bu değeri bir yere yapıştırın.

1. <a id="userObjectId"></a>RBAC Sahibi rolünü vermek istediğiniz kullanıcının veya grubun nesne kimliğini alma

    1. Azure portalda **Azure Active Directory** araması yapın.
    1. Bir kullanıcıya erişim vermek istiyorsanız sol taraftaki menüde **Kullanıcılar**'ı seçin. Bir gruba erişim vermek için **Gruplar**'ı seçin.
    1. RBAC Sahibi rolünü vermek istediğiniz Kullanıcıyı veya Grubu seçin.
    1. Kullanıcı seçtiyseniz, Profil sayfasında nesne kimliğini bulabilirsiniz. Grup seçtiyseniz, nesne kimliği Genel Bakış sayfasında olur. Metin kutusunun sağ tarafındaki simgeyi seçerek **ObjectID** değerini kopyalayın. Bir sonraki adımda `userObjectId` olarak kullanabilmek için bu değeri bir yere yapıştırın.

1. Kullanıcıya veya gruba kayıt hesabında RBAC Sahibi rolü verme

    İlk iki adımda topladığınız değerleri kullanarak kullanıcıya veya gruba kayıt hesabında RBAC Sahibi rolü verin.

    # <a name="rest"></a>[REST](#tab/rest-2)

    Aşağıdaki komutu çalıştırın; ```<enrollmentAccountObjectId>``` değerini ilk adımda kopyaladığınız `name` ile değiştirin (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). ```<userObjectId>``` değerini ikinci adımda kopyaladığınız nesne kimliğiyle değiştirin.

    ```json
    PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

    {
      "properties": {
        "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
        "principalId": "<userObjectId>"
      }
    }
    ```

    Kayıt hesabı bağlamında Sahip rolü başarıyla atandığında, Azure rol atamasının bilgileriyle yanıt verir:

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

    Aşağıdaki [New-AzRoleAssignment](../../role-based-access-control/role-assignments-powershell.md) komutunu çalıştırın; ```<enrollmentAccountObjectId>``` değerini ilk adımda topladığınız `ObjectId` ile değiştirin (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). ```<userObjectId>``` değerini ikinci adımda toplanan nesne kimliğiyle değiştirin.

    ```azurepowershell-interactive
    New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli-2)

    Aşağıdaki [az role assignment create](../../role-based-access-control/role-assignments-cli.md) komutunu çalıştırın; ```<enrollmentAccountObjectId>``` değerini ilk adımda kopyaladığınız `name` ile değiştirin (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). ```<userObjectId>``` değerini ikinci adımda toplanan nesne kimliğiyle değiştirin.

    ```azurecli-interactive
    az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    Bir kullanıcı kayıt hesabınızın RBAC Sahibi olduktan sonra bu hesap altında [program aracılığıyla abonelikler oluşturabilir](programmatically-create-subscription.md). Temsilci kullanıcı tarafından oluşturulan aboneliğin de Hizmet Yöneticisi olarak özgün Hesap Sahibi vardır ama aynı zamanda varsayılan olarak temsilci kullanıcı da RBAC Sahibi olur.

    ---

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Etkinlik günlüklerini kullanarak abonelikleri kimin oluşturduğunu denetleme

Bu API yoluyla oluşturulan abonelikleri izlemek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) kullanın. Şu anda PowerShell, CLI veya Azure portalı kullanarak abonelik oluşturma işlemini izlemek mümkün değildir.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../../role-based-access-control/elevate-access-global-admin.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın. Bu erişim [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolünde, [İzleme katkıda bulunanı](../../role-based-access-control/built-in-roles.md#monitoring-contributor) rolü veya [özel bir rolde](../../role-based-access-control/custom-roles.md) kullanılabilir.
1. Denetleyen kullanıcı olarak, abonelik oluşturma etkinliklerini görmek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) çağırın. Örnek:

    ```
    GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'"
    ```

Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* Artık kullanıcının veya hizmet sorumlusunun abonelik oluşturma izni olduğuna göre, bu kimliği kullanarak [program aracılığıyla Azure Kurumsal abonelikleri oluşturabilirsiniz](programmatically-create-subscription.md).
* .NET kullanarak abonelik oluşturma örneği için [GitHub'da örnek koda](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core) bakın.
* Azure Resource Manager ve onun API'leri hakkında daha fazla bilgi edinmek için bkz [Azure Resource Manager’a genel bakış](../../azure-resource-manager/management/overview.md).
* Yönetim gruplarını kullanarak çok fazla sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için bkz. [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../governance/management-groups/overview.md)
* Büyük kuruluşlara yönelik abonelik idaresiyle ilgili kapsamlı bir en iyi yöntemler kılavuzu için bkz. [Azure kurumsal iskelesi - açıklayıcı abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance)
