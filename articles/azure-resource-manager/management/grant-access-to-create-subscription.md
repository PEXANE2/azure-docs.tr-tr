---
title: Azure Kurumsal abonelikleri oluşturmak için erişim izni verme
description: Bir kullanıcıya veya hizmet sorumlusuna Azure Kurumsal aboneliklerini programlı olarak oluşturma olanağını nasıl vereceğinizi öğrenin.
author: jureid
manager: jureid
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: jureid
ms.openlocfilehash: b77efd7e5cf7ff016605e0ba2e74cff9ea8dab89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75478883"
---
# <a name="grant-access-to-create-azure-enterprise-subscriptions-preview"></a>Azure Kurumsal abonelikleri oluşturmak için erişim izni verme (önizleme)

[Kurumsal Sözleşme 'de (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)bir Azure müşterisi olarak, başka bir kullanıcıya veya hizmet müdürüne hesabınıza faturalı abonelikler oluşturma izni verebilirsiniz. Bu makalede, abonelik oluşturma yeteneğini paylaşmak için [Rol Tabanlı Erişim Denetimi'ni (RBAC)](../../active-directory/role-based-access-control-configure.md) nasıl kullanacağınızı ve abonelik oluşturmalarını nasıl denetlersiniz öğrenirsiniz. Paylaşmak istediğiniz hesapta Sahip rolüne sahip olmalısınız.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="grant-access"></a>Erişim verme

[Bir kayıt hesabı altında abonelik oluşturmak](programmatically-create-subscription.md)için, kullanıcıların bu hesapta [RBAC Sahibi rolüne](../../role-based-access-control/built-in-roles.md#owner) sahip olması gerekir. Bir kullanıcıya veya bir kullanıcı grubuna kayıt hesabında RBAC Sahibi rolü şu adımları izleyerek verebilirsiniz:

1. Erişim izni vermek istediğiniz kayıt hesabının nesne kimliğini alma

    Başkalarına bir kayıt hesabında RBAC Sahibi rolünü vermek için, hesabın Hesap Sahibi veya RBAC Sahibi olmalısınız.

    # <a name="rest"></a>[Geri kalanı](#tab/rest)

    Erişiminiz olan tüm kayıt hesaplarını listeleme isteği:

    ```json
    GET https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts?api-version=2018-03-01-preview
    ```

    Azure, erişebildiğiniz tüm kayıt hesaplarının bir listesiyle yanıt verir:

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

    RBAC `principalName` Sahibine erişim izni vermek istediğiniz hesabı tanımlamak için özelliği kullanın. O `name` hesabın kopyasını kopyala. Örneğin, RBAC Sahibine SignUpEngineering@contoso.com kayıt hesabına erişim izni vermek istiyorsanız, kopyalarsınız. ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` Bu, kayıt hesabının nesne kimliğidir. Bir sonraki adımda kullanabilmeniz için bu değeri `enrollmentAccountObjectId`bir yere yapıştırın.

    # <a name="powershell"></a>[Powershell](#tab/azure-powershell)

    Erişebildiğiniz tüm kayıt hesaplarını listelemek için [Get-AzEnrollmentAccount](/powershell/module/az.billing/get-azenrollmentaccount) cmdlet'ini kullanın. [Azure Bulut Uyp'ı](https://shell.azure.com/)açmak için **deneyin'i** seçin. Kodu yapıştırmak için kabuk pencerelerini ve yapıştır'ı **seç'i**sağ tıklatın.

    ```azurepowershell-interactive
    Get-AzEnrollmentAccount
    ```

    Azure, erişebildiğiniz kayıt hesaplarının bir listesiyle yanıt verir:

    ```azurepowershell
    ObjectId                               | PrincipalName
    747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | SignUpEngineering@contoso.com
    4cd2fcf6-xxxx-xxxx-xxxx-xxxxxxxxxxxx   | BillingPlatformTeam@contoso.com
    ```

    RBAC `principalName` Sahibine erişim izni vermek istediğiniz hesabı tanımlamak için özelliği kullanın. O `ObjectId` hesabın kopyasını kopyala. Örneğin, RBAC Sahibine SignUpEngineering@contoso.com kayıt hesabına erişim izni vermek istiyorsanız, kopyalarsınız. ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` Bir sonraki adımda kullanabilmeniz için bu nesne kimliğini `enrollmentAccountObjectId`bir yere yapıştırın.

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

    Erişiminiz olan tüm kayıt hesaplarını listelemek için [az fatura kayıt hesabı listesini](https://aka.ms/EASubCreationPublicPreviewCLI) kullanın. [Azure Bulut Uyp'ı](https://shell.azure.com/)açmak için **deneyin'i** seçin. Kodu yapıştırmak için kabuk pencerelerini ve yapıştır'ı **seç'i**sağ tıklatın.

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

    ---

    RBAC `principalName` Sahibine erişim izni vermek istediğiniz hesabı tanımlamak için özelliği kullanın. O `name` hesabın kopyasını kopyala. Örneğin, RBAC Sahibine SignUpEngineering@contoso.com kayıt hesabına erişim izni vermek istiyorsanız, kopyalarsınız. ```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` Bu, kayıt hesabının nesne kimliğidir. Bir sonraki adımda kullanabilmeniz için bu değeri `enrollmentAccountObjectId`bir yere yapıştırın.

1. <a id="userObjectId"></a>RBAC Sahibi rolü vermek istediğiniz kullanıcının veya grubun nesne kimliğini

    1. Azure portalında Azure **Etkin Dizini'nde**arama yapın.
    1. Kullanıcıerişimi vermek istiyorsanız, soldaki menüdeki **Kullanıcılar'ı** tıklatın. Bir gruba erişim izni vermek istiyorsanız, **Gruplar'ı**tıklatın.
    1. RBAC Sahibi rolü vermek istediğiniz Kullanıcı veya Grubu seçin.
    1. Bir Kullanıcı seçtiyseniz, profil sayfasında nesne kimliğini bulursunuz. Bir Grup seçtiyseniz, nesne kimliği Genel Bakış sayfasında olacaktır. Metin kutusunun sağındaki simgeyi tıklatarak **ObjectID'yi** kopyalayın. Bir sonraki adımda kullanabilmeniz için bunu `userObjectId`bir yere yapıştırın.

1. Kullanıcıya veya kayıt hesabında RBAC Sahibi rolünü gruplandırma

    İlk iki adımda topladığınız değerleri kullanarak, kullanıcıya rbac sahibi rolünü veya gruplandırmayı kayıt hesabında ver.

    # <a name="rest"></a>[Geri kalanı](#tab/rest-2)

    İlk adımda kopyalanan ```<enrollmentAccountObjectId>``` ile `name` değiştirerek aşağıdaki komutu```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```çalıştırın ( ). İkinci ```<userObjectId>``` adımdan kopyaladığınız nesne kimliğiyle değiştirin.

    ```json
    PUT  https://management.azure.com/providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>/providers/Microsoft.Authorization/roleAssignments/<roleAssignmentGuid>?api-version=2015-07-01

    {
      "properties": {
        "roleDefinitionId": "/providers/Microsoft.Billing/enrollmentAccounts/providers/Microsoft.Authorization/roleDefinitions/<ownerRoleDefinitionId>",
        "principalId": "<userObjectId>"
      }
    }
    ```

    Kayıt hesabı kapsamında Sahip rolü başarıyla atandığında, Azure rol ataması bilgileriyle yanıt verir:

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

    # <a name="powershell"></a>[Powershell](#tab/azure-powershell-2)

    Aşağıdaki [Yeni-AzRoleAssignment](../../active-directory/role-based-access-control-manage-access-powershell.md) komutunu çalıştırın, `ObjectId` ilk adımda toplananla değiştirin```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx``` ```<enrollmentAccountObjectId>``` ( ). İkinci ```<userObjectId>``` adımda toplanan nesne kimliğiyle değiştirin.

    ```azurepowershell-interactive
    New-AzRoleAssignment -RoleDefinitionName Owner -ObjectId <userObjectId> -Scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli-2)

    Aşağıdaki [az rol ataması komutu oluşturma,](../../active-directory/role-based-access-control-manage-access-azure-cli.md) ilk adımda kopyalanan ile değiştirerek ```<enrollmentAccountObjectId>``` `name` (```747ddfe5-xxxx-xxxx-xxxx-xxxxxxxxxxxx```). İkinci ```<userObjectId>``` adımda toplanan nesne kimliğiyle değiştirin.

    ```azurecli-interactive
    az role assignment create --role Owner --assignee-object-id <userObjectId> --scope /providers/Microsoft.Billing/enrollmentAccounts/<enrollmentAccountObjectId>
    ```

    Bir kullanıcı kayıt hesabınız için RBAC Sahibi olduğunda, bu hesabın altında [programlı olarak abonelikler oluşturabilir.](programmatically-create-subscription.md) Devredilen bir kullanıcı tarafından oluşturulan bir abonelik hala Hizmet Yöneticisi olarak orijinal Hesap Sahibine sahiptir, ancak varsayılan olarak varsayılan olarak devredilen kullanıcıyı RBAC Sahibi olarak da kullanmıştır.

    ---

## <a name="audit-who-created-subscriptions-using-activity-logs"></a>Etkinlik günlüklerini kullanarak abonelikleri kimlerin oluşturduğunu denetleme

Bu API aracılığıyla oluşturulan abonelikleri izlemek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs)kullanın. Abonelik oluşturmayı izlemek için şu anda PowerShell, CLI veya Azure portalını kullanmak mümkün değil.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../../active-directory/role-based-access-control-tenant-admin-access.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın.
1. Denetim kullanıcısı olarak, abonelik oluşturma etkinliklerini görmek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) arayın. Örnek:

    ```
    GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Subscription'"
    ```

Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* Artık kullanıcı veya hizmet sorumlusu abonelik oluşturma iznine sahip olduğuna göre, bu kimliği [Azure Kurumsal aboneliklerini programlı bir şekilde oluşturmak](programmatically-create-subscription.md)için kullanabilirsiniz.
* .NET'i kullanarak abonelik oluşturma örneği için [GitHub'daki örnek koda](https://github.com/Azure-Samples/create-azure-subscription-dotnet-core)bakın.
* Azure Kaynak Yöneticisi ve API'leri hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi'ne genel bakış'a](overview.md)bakın.
* Yönetim gruplarını kullanarak çok sayıda aboneliği yönetme hakkında daha fazla bilgi edinmek için [bkz.](../../governance/management-groups/overview.md)
* Abonelik yönetimi konusunda büyük kuruluşlar için kapsamlı bir en iyi uygulama kılavuzunu görmek için Azure [kurumsal iskelesi - önceden yazılmış abonelik yönetimi](/azure/architecture/cloud-adoption-guide/subscription-governance)
