---
title: Yönetilen kimliklerle kimlik doğrulaması
description: Yönetilen bir kimlik kullanarak kimlik bilgileri veya sırlar ile oturum açmadan diğer Azure Etkin Dizin kiracılarının kaynaklarına erişin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: 82710a66cdf7874c745070e49b2c7aff7bc8816d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77117529"
---
# <a name="authenticate-access-to-azure-resources-by-using-managed-identities-in-azure-logic-apps"></a>Azure Logic Apps'ta yönetilen kimlikleri kullanarak Azure kaynaklarına erişimi kimlik doğrulama

Diğer Azure Etkin Dizin (Azure AD) kiracılarında kaynaklara erişmek ve oturum açmadan kimliğinizi doğrulamak için, mantık uygulamanız kimlik bilgileri veya sırlar yerine yönetilen bir [kimliği](../active-directory/managed-identities-azure-resources/overview.md) (eski yönetilen Hizmet Kimliği veya MSI) kullanabilir. Azure bu kimliği sizin için yönetir ve sırları sağlamak veya döndürmek zorunda olmadığınız için kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur.

Azure Logic Apps, hem [*sistem tarafından atanmış*](../active-directory/managed-identities-azure-resources/overview.md) hem de kullanıcı tarafından [*atanan*](../active-directory/managed-identities-azure-resources/overview.md) yönetilen kimlikleri destekler. Mantık uygulamanız, sistem tarafından atanan kimliği veya kullanıcı tarafından atanmış *tek* bir kimliği kullanabilir ve bu kimliği bir grup mantık uygulaması arasında paylaşabilirsiniz, ancak her ikisini birden kullanamaz. Şu anda, yalnızca [belirli yerleşik tetikleyiciler ve eylemler](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) yönetilen kimlikleri destekler, yönetilen bağlayıcıları veya bağlantıları desteklemez, örneğin:

* HTTP
* Azure İşlevleri
* Azure API Management
* Azure Uygulama Hizmetleri

Bu makalede, mantık uygulamanız için her iki türde yönetilen kimlik nasıl ayarlayınız gösterilmektedir. Daha fazla bilgi için şu konulara bakın:

* [Yönetilen kimlikleri destekleyen tetikleyiciler ve eylemler](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Giden aramalarda desteklenen kimlik doğrulama türleri](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Mantık uygulamaları için yönetilen kimlik sınırları](../logic-apps/logic-apps-limits-and-config.md#managed-identity)
* [Azure AD kimlik doğrulamasını yönetilen kimliklerle destekleyen Azure hizmetleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Hem yönetilen kimlik hem de erişime ihtiyacınız olan hedef Azure kaynağı aynı Azure aboneliğini kullanmalıdır.

* Azure kaynağına yönetilen bir kimlik erişimi sağlamak için, bu kimlik için hedef kaynağa bir rol eklemeniz gerekir. Roller eklemek için, ilgili Azure AD kiracısındaki kimliklere roller atayabilecek [Azure AD yöneticisi izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md) ihtiyacınız vardır.

* Erişmek istediğiniz hedef Azure kaynağı. Bu kaynağa, yönetilen kimlik için bir rol eklersiniz ve bu da mantık uygulamasının hedef kaynağa erişimi doğrulamasına yardımcı olur.

* [Tetikleyiciyi](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) kullanmak istediğiniz mantık uygulaması veya yönetilen kimlikleri destekleyen eylemler

## <a name="enable-managed-identity"></a>Yönetilen kimliği etkinleştirme

Kullanmak istediğiniz yönetilen kimliği ayarlamak için bu kimlik için bağlantıyı izleyin:

* [Sisteme atanmış kimlik](#system-assigned)
* [Kullanıcı tarafından atanan kimlik](#user-assigned)

<a name="system-assigned"></a>

### <a name="enable-system-assigned-identity"></a>Sistemle atanmış kimliği etkinleştirme

Kullanıcı tarafından atanan kimliklerin aksine, sistem tarafından atanan kimliği el ile oluşturmanız gerekmez. Mantık uygulamanız için sistem tarafından atanan kimliği ayarlamak için kullanabileceğiniz seçenekler şunlardır:

* [Azure portalında](#azure-portal-system-logic-app)
* [Azure Kaynak Yöneticisi şablonları](#template-system-logic-app)

<a name="azure-portal-system-logic-app"></a>

#### <a name="enable-system-assigned-identity-in-azure-portal"></a>Azure portalında sistem le atanmış kimliği etkinleştirme

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. Mantık uygulaması menüsünde Ayarlar **altında** **Kimlik'i**seçin.  > **Kaydet'te** > **Save** **atanan Sistemi**seçin. Azure onaylamanızı istediğinde **Evet'i**seçin.

   ![Sistemtarafından atanan kimliği etkinleştirme](./media/create-managed-service-identity/enable-system-assigned-identity.png)

   > [!NOTE]
   > Yalnızca tek bir yönetilen kimliğe sahip olabileceğiniz bir hata alırsanız, mantık uygulamanız zaten kullanıcı tarafından atanan kimlikle ilişkilendirilir. Sisteme atanan kimliği eklemeden önce, kullanıcıtarafından atanan kimliği mantık uygulamanızdan *kaldırmanız* gerekir.

   Mantık uygulamanız artık Azure Etkin Dizin'e kayıtlı ve nesne kimliğiyle temsil edilen sistem tarafından atanmış kimliği kullanabilir.

   ![Sistemle atanan kimlik için nesne kimliği](./media/create-managed-service-identity/object-id-system-assigned-identity.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Nesne Kimliği** | <*kimlik-kaynak-kimlik*> | Azure AD kiracısında, mantık uygulamanız için sistem tarafından atanan kimliği temsil eden Genel Olarak Benzersiz Tanımlayıcı (GUID) |
   ||||

1. Şimdi, bu konuda daha sonra [kaynağa bu kimlik erişimi veren adımları](#access-other-resources) izleyin.

<a name="template-system-logic-app"></a>

#### <a name="enable-system-assigned-identity-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonunda sistem le atanmış kimliği etkinleştirme

Mantıksal uygulamalar gibi Azure kaynaklarını oluşturmayı ve dağıtmayı otomatikleştirmek için [Azure Kaynak Yöneticisi şablonlarını](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)kullanabilirsiniz. Şablondaki mantık uygulamanız için sistem tarafından atanan yönetilen `identity` kimliği etkinleştirmek için, nesneyi ve `type` alt özelliği şablondaki mantık uygulamasının kaynak tanımına ekleyin:

```json
{
   "apiVersion": "2016-06-01",
   "type": "Microsoft.logic/workflows",
   "name": "[variables('logicappName')]",
   "location": "[resourceGroup().location]",
   "identity": {
      "type": "SystemAssigned"
   },
   "properties": {
      "definition": {
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
         "actions": {},
         "parameters": {},
         "triggers": {},
         "contentVersion": "1.0.0.0",
         "outputs": {}
   },
   "parameters": {},
   "dependsOn": []
}
```

Azure, mantıksal uygulama kaynak tanımınızı `identity` oluşturduğunda, nesne şu ek özellikleri alır:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Emlak (JSON) | Değer | Açıklama |
|-----------------|-------|-------------|
| `principalId` | <*müdür Kimliği*> | Azure AD kiracısında mantık uygulamanızı temsil eden yönetilen kimlik için hizmet ana nesnesinin Genel Olarak Benzersiz Tanımlayıcısı (GUID). Bu GUID bazen "nesne kimliği" `objectID`veya . |
| `tenantId` | <*Azure-AD-kiracı-Kimliği*> | Mantıksal uygulamanın artık üye olduğu Azure AD kiracısını temsil eden Genel Olarak Benzersiz Tanımlayıcı (GUID). Azure AD kiracısının içinde, hizmet sorumlusu mantık uygulaması örneğiyle aynı ada sahiptir. |
||||

<a name="user-assigned"></a>

### <a name="enable-user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği etkinleştirme

Mantık uygulamanız için kullanıcı tarafından atanmış yönetilen bir kimlik ayarlamak için, öncelikle bu kimliği ayrı bir bağımsız Azure kaynağı olarak oluşturmanız gerekir. Kullanabileceğiniz seçenekler şunlardır:

* [Azure portalında](#azure-portal-user-identity)
* [Azure Kaynak Yöneticisi şablonları](#template-user-identity)
* Azure PowerShell
  * [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
  * [Rol ataması ekle](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md)
* Azure CLI
  * [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)
  * [Rol ataması ekle](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md)
* Azure REST API
  * [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-rest.md)
  * [Rol ataması ekle](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-user-identity"></a>

#### <a name="create-user-assigned-identity-in-the-azure-portal"></a>Azure portalında kullanıcı tarafından atanmış kimlik oluşturma

1. Azure [portalında,](https://portal.azure.com)herhangi bir sayfadaki arama `managed identities`kutusuna **Yönetilen Kimlikler'i**girin ve seçin.

   !["Yönetilen Kimlikler"i bulun ve seçin](./media/create-managed-service-identity/find-select-managed-identities.png)

1. **Yönetilen Kimlikler**altında **Ekle'yi**seçin.

   ![Yeni yönetilen kimlik ekleme](./media/create-managed-service-identity/add-user-assigned-identity.png)

1. Yönetilen kimliğiniz hakkında bilgi sağlayın ve ardından örneğin **Oluştur'u**seçin:

   ![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/create-managed-service-identity/create-user-assigned-identity.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Kaynak Adı** | Evet | <*kullanıcı tarafından atanan-kimlik-adı*> | Kullanıcı tarafından atanan kimliğinize verecek ad. Bu örnekte "Fabrikam-kullanıcı tarafından atanan kimlik" kullanmıştır. |
   | **Abonelik** | Evet | <*Azure abonelik adı*> | Kullanılacak Azure aboneliğinin adı |
   | **Kaynak grubu** | Evet | <*Azure-kaynak grubu adı*> | Kullanılacak kaynak grubunun adı. Yeni bir grup oluşturun veya varolan bir grubu seçin. Bu örnek, "fabrikam-yönetilen-kimlikler-RG" adlı yeni bir grup oluşturur. |
   | **Konum** | Evet | <*Azure bölgesi*> | Kaynağınızla ilgili bilgilerin depolandığı Azure bölgesi. Bu örnekte "Batı ABD" kullanılıyor. |
   |||||

   Artık mantık uygulamanıza kullanıcı tarafından atanan kimliği ekleyebilirsiniz. Mantık uygulamanıza birden fazla kullanıcı tarafından atanmış kimlik ekemezsiniz.

1. Azure portalında Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı bulun ve açın.

1. Mantık uygulaması menüsünde, **Ayarlar**altında, **Kimlik'i**seçin ve sonra **Atanan** > **Kullanıcı'yı**seçin Ekle.

   ![Kullanıcı tarafından atanan yönetilen kimlik ekleme](./media/create-managed-service-identity/add-user-assigned-identity-logic-app.png)

1. Kullanıcı **tarafından atanan yönetilen kimlik** bölmesine **Abonelik** listesinden ekle, zaten seçilmemişse Azure aboneliğinizi seçin. Bu abonelikteki *tüm* yönetilen kimlikleri gösteren listeden, istediğiniz kullanıcı tarafından atanan kimliği bulun ve seçin. Listeye filtre uygulayacak şekilde, **Kullanıcı tarafından atanan yönetilen kimlikler** arama kutusuna kimlik veya kaynak grubunun adını girin. Bittiğinde **Ekle'yi**seçin.

   ![Kullanmak için kullanıcı tarafından atanan kimliği seçin](./media/create-managed-service-identity/select-user-assigned-identity.png)

   > [!NOTE]
   > Yalnızca tek bir yönetilen kimliğe sahip olabileceğiniz bir hata alırsanız, mantık uygulamanız zaten sistem tarafından atanan kimlikle ilişkilidir. Kullanıcı tarafından atanan kimliği eklemeden önce mantık uygulamanızda sistem tarafından atanan kimliği devre dışı bmelisiniz.

   Mantık uygulamanız artık kullanıcı tarafından atanan yönetilen kimlikle ilişkilendirilir.

   ![Kullanıcı tarafından atanan kimlikle ilişkilendirme](./media/create-managed-service-identity/added-user-assigned-identity.png)

1. Şimdi, bu konuda daha sonra [kaynağa bu kimlik erişimi veren adımları](#access-other-resources) izleyin.

<a name="template-user-identity"></a>

#### <a name="create-user-assigned-identity-in-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonunda kullanıcı tarafından atanmış kimlik oluşturma

Mantıksal uygulamalar gibi Azure kaynaklarını oluşturmayı ve dağıtmayı otomatikleştirmek [için, kimlik doğrulama için kullanıcıtarafından atanan kimlikleri](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)destekleyen [Azure Kaynak Yöneticisi şablonlarını](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)kullanabilirsiniz. Şablonunuzun `resources` bölümünde, mantık uygulamanızın kaynak tanımı şu öğeleri gerektirir:

* Özelliği `identity` ayarlanmış `type` bir nesne`UserAssigned`

* Kimliğin `userAssignedIdentities` kaynak kimliğini belirten bir alt nesne, bu nesne ve `principalId` `clientId` özelliklere sahip başka bir alt nesnedir

Bu örnek, http put isteği için bir mantıksal uygulama kaynak `identity` tanımı gösterir ve parametresiz bir nesne içerir. PUT isteğine ve sonraki GET işlemine `identity` verilen yanıtta da şu nesne var:

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {<template-parameters>},
   "resources": [
      {
         "apiVersion": "2016-06-01",
         "type": "Microsoft.logic/workflows",
         "name": "[variables('logicappName')]",
         "location": "[resourceGroup().location]",
         "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
               "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource-group-name>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<user-assigned-identity-name>": {
                  "principalId": "<principal-ID>",
                  "clientId": "<client-ID>"
               }
            }
         },
         "properties": {
            "definition": {<logic-app-workflow-definition>}
         },
         "parameters": {},
         "dependsOn": []
      },
   ],
   "outputs": {}
}
```

| Emlak (JSON) | Değer | Açıklama |
|-----------------|-------|-------------|
| `principalId` | <*müdür Kimliği*> | Azure AD kiracısında kullanıcı tarafından atanan yönetilen kimlik için Genel Olarak Benzersiz Tanımlayıcı (GUID) |
| `clientId` | <*istemci kimliği*> | Mantık uygulamanızın çalışma süresi sırasında aramalar için kullanılan yeni kimliği için Genel Olarak Benzersiz Tanımlayıcı (GUID) |
||||

Şablonunuz yönetilen kimliğin kaynak tanımını da içeriyorsa, nesneyi `identity` parametrenize ekleyebilirsiniz. Bu örnek, alt `userAssignedIdentities` nesnenin `userAssignedIdentity` şablonunuzun `variables` bölümünde tanımladığınız bir değişkene nasıl başvurullarını gösterir. Bu değişken, kullanıcı tarafından atanan kimliğiniz için kaynak kimliğine başvurur.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
   "contentVersion": "1.0.0.0",
   "parameters": {
      "Template_LogicAppName": {
         "type": "string"
      },
      "Template_UserAssignedIdentityName": {
         "type": "securestring"
      }
   },
   "variables": {
      "logicAppName": "[parameters(`Template_LogicAppName')]",
      "userAssignedIdentityName": "[parameters('Template_UserAssignedIdentityName')]"
   },
   "resources": [
      {
         "apiVersion": "2016-06-01",
         "type": "Microsoft.logic/workflows",
         "name": "[variables('logicAppName')]",
         "location": "[resourceGroup().location]",
         "identity": {
            "type": "UserAssigned",
            "userAssignedIdentities": {
               "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', variables('userAssignedIdentityName'))]": {}
            }
         },
         "properties": {
            "definition": {<logic-app-workflow-definition>}
         },
         "parameters": {},
         "dependsOn": [
            "[resourceId('Microsoft.ManagedIdentity/userAssignedIdentities/', variables('userAssignedIdentityName'))]"
         ]
      },
      {
         "apiVersion": "2018-11-30",
         "type": "Microsoft.ManagedIdentity/userAssignedIdentities",
         "name": "[parameters('Template_UserAssignedIdentityName')]",
         "location": "[resourceGroup().location]",
         "properties": {
            "tenantId": "<tenant-ID>",
            "principalId": "<principal-ID>",
            "clientId": "<client-ID>"
         }
      }
  ]
}
```

| Emlak (JSON) | Değer | Açıklama |
|-----------------|-------|-------------|
| `tenantId` | <*Azure-AD-kiracı-Kimliği*> | Kullanıcı tarafından atanan kimliğin artık üye olduğu Azure AD kiracısını temsil eden Genel Olarak Benzersiz Tanımlayıcı (GUID). Azure AD kiracısının içinde, hizmet sorumlusu kullanıcı tarafından atanan kimlik adı ile aynı ada sahiptir. |
| `principalId` | <*müdür Kimliği*> | Azure AD kiracısında kullanıcı tarafından atanan yönetilen kimlik için Genel Olarak Benzersiz Tanımlayıcı (GUID) |
| `clientId` | <*istemci kimliği*> | Mantık uygulamanızın çalışma süresi sırasında aramalar için kullanılan yeni kimliği için Genel Olarak Benzersiz Tanımlayıcı (GUID) |
||||

<a name="access-other-resources"></a>

## <a name="give-identity-access-to-resources"></a>Kaynaklara kimlik erişimi verme

Mantık uygulamanızın yönetilen kimliğini kimlik doğrulama için kullanmadan önce, kimliği kullanmayı planladığınız Azure kaynağında bu kimliğe erişim ayarlayın. Bu görevi tamamlamak için, hedef Azure kaynağındaki bu kimliğe uygun rolü atayın. Kullanabileceğiniz seçenekler şunlardır:

* [Azure portalında](#azure-portal-assign-access)
* [Azure Resource Manager şablonu](../role-based-access-control/role-assignments-template.md)
* Azure PowerShell ([Yeni-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment)) - Daha fazla bilgi için Azure [RBAC ve Azure PowerShell'i kullanarak rol ataması ekle'ye](../role-based-access-control/role-assignments-powershell.md)bakın.
* Azure CLI ([az rol ataması oluşturma](https://docs.microsoft.com/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create)) - Daha fazla bilgi için Azure [RBAC ve Azure CLI'yi kullanarak rol ataması ekle'ye](../role-based-access-control/role-assignments-cli.md)bakın.
* [Azure REST API](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-assign-access"></a>

### <a name="assign-access-in-the-azure-portal"></a>Azure portalında erişim atama

1. Azure [portalında,](https://portal.azure.com)yönetilen kimliğinizin erişimini istediğiniz Azure kaynağına gidin.

1. Kaynağın menüsünden, bu kaynak için geçerli rol atamalarını gözden geçirebileceğiniz **Access denetimi (IAM)** > **Rol atamaları'nı** seçin. Araç çubuğunda > **rol atama** **ekle'yi**seçin.

   !["Rol ataması ekle" > "Ekle"yi seçin](./media/create-managed-service-identity/add-role-to-resource.png)

   > [!TIP]
   > Rol **atama ekle** seçeneği devre dışı bırakılırsa, büyük olasılıkla izinlerin izyoktur. Kaynaklar için rolleri yönetmenize izinler hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Yönetici rol izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bakın.

1. **Rol Atama ekle**altında, kimliğinize hedef kaynağa gerekli erişimi sağlayan bir **Rol** seçin.

   Bu konunun örneği için, kimliğinizin [bir Azure Depolama kapsayıcısındaki blob'a erişebilecek bir role](../storage/common/storage-auth-aad.md#assign-rbac-roles-for-access-rights)ihtiyacı vardır.

   !["Depolama Blob Veri Katılımcısı" rolünü seçin](./media/create-managed-service-identity/select-role-for-identity.png)

1. Yönetilen kimliğiniz için aşağıdaki adımları izleyin:

   * **Sisteme atanmış kimlik**

     1. **Kutuya Erişim Atla'da** **Mantık Uygulaması'nı**seçin. **Abonelik** özelliği göründüğünde, kimliğinizle ilişkili Azure aboneliğini seçin.

        ![Sistemle atanan kimlik için erişimi seçin](./media/create-managed-service-identity/assign-access-system.png)

     1. **Seç** kutusunun altında, listeden mantık uygulamanızı seçin. Liste çok uzunsa, listeye filtre lemek için **Seç** kutusunu kullanın.

        ![Sistemle atanmış kimlik için mantık uygulamasını seçin](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

   * **Kullanıcı tarafından atanan kimlik**

     1. **Kutuya Access Atla'da,** **Kullanıcı tarafından atanan yönetilen kimliği**seçin. **Abonelik** özelliği göründüğünde, kimliğinizle ilişkili Azure aboneliğini seçin.

        ![Kullanıcı tarafından atanan kimlik için erişimi seçin](./media/create-managed-service-identity/assign-access-user.png)

     1. **Seç** kutusunun altında, listeden kimliğinizi seçin. Liste çok uzunsa, listeye filtre lemek için **Seç** kutusunu kullanın.

        ![Kullanıcı tarafından atanan kimliğinizi seçin](./media/create-managed-service-identity/add-permissions-select-user-assigned-identity.png)

1. İşiniz bittiğinde **Kaydet**'i seçin.

   Hedef kaynağın rol atamaları listesi artık seçilen yönetilen kimliği ve rolü gösterir. Bu örnek, bir mantık uygulaması için sistem tarafından atanan kimliği ve diğer mantık uygulamaları grubu için kullanıcı tarafından atanan kimliği nasıl kullanabileceğinizi gösterir.

   ![Hedef kaynağa yönetilen kimlikler ve roller eklendi](./media/create-managed-service-identity/added-roles-for-identities.png)

   Daha fazla bilgi için Azure [portalını kullanarak kaynağa yönetilen bir kimlik erişimi atayın.](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md)

1. Şimdi, yönetilen kimlikleri destekleyen bir tetikleyici veya eylemdeki [kimlikle erişimi doğrulamak için gereken adımları](#authenticate-access-with-identity) izleyin.

<a name="authenticate-access-with-identity"></a>

## <a name="authenticate-access-with-managed-identity"></a>Yönetilen kimlikle erişimi doğrulama

Mantık [uygulamanız için yönetilen kimliği etkinleştirdikten](#azure-portal-system-logic-app) ve [bu kimlik erişimini hedef kaynağa veya varlığa verdikten](#access-other-resources)sonra, bu kimliği [yönetilen kimlikleri destekleyen tetikleyicilerde ve eylemlerde](logic-apps-securing-a-logic-app.md#managed-identity-authentication)kullanabilirsiniz.

> [!IMPORTANT]
> Sistem tarafından atanan kimliği kullanmak istediğiniz bir Azure işleviniz varsa, önce [Azure işlevleri için kimlik doğrulamayı etkinleştirin.](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions)

Bu adımlar, yönetilen kimliğin Azure portalı üzerinden bir tetikleyici veya eylemle nasıl kullanılacağını gösterir. Bir tetikleyici veya eylemin temel JSON tanımında yönetilen kimliği belirtmek için Yönetilen [kimlik kimlik doğrulaması'na](../logic-apps/logic-apps-securing-a-logic-app.md#managed-identity-authentication)bakın.

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. Henüz yapmadıysanız, [yönetilen kimlikleri destekleyen tetikleyiciyi veya eylemi](logic-apps-securing-a-logic-app.md#managed-identity-authentication)ekleyin.

   Örneğin, HTTP tetikleyicisi veya eylemi, mantık uygulamanız için etkinleştirdiğiniz sistem tarafından atanmış kimliği kullanabilir. Genel olarak, HTTP tetikleyicisi veya eylemi, erişmek istediğiniz kaynağı veya varlığı belirtmek için bu özellikleri kullanır:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Yöntem** | Evet | Çalıştırmak istediğiniz işlem tarafından kullanılan HTTP yöntemi |
   | **URI** | Evet | Hedef Azure kaynağına veya varlığına erişmek için uç nokta URL'si. URI sözdizimi genellikle Azure kaynağı veya hizmeti için [kaynak kimliğini](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) içerir. |
   | **Üst Bilgiler** | Hayır | İçerik türü gibi giden isteğe eklemeniz gereken veya eklemek istediğiniz üstbilgi değerleri |
   | **Sorgular** | Hayır | Belirli bir işlemin parametresi veya çalıştırmak istediğiniz işlem için API sürümü gibi isteğe eklemeniz gereken veya eklemek istediğiniz sorgu parametreleri |
   | **Kimlik doğrulaması** | Evet | Hedef kaynağa veya varlığa erişimin doğruluğunu doğrulamak için kullanılacak kimlik doğrulama türü |
   ||||

   Belirli bir örnek olarak, [Snapshot Blob işlemini](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob) daha önce kimliğinize erişim ayarladığınız Azure Depolama hesabındaki bir blob üzerinde çalıştırmak istediğinizi varsayalım. Ancak, [Azure Blob Depolama bağlayıcısı](https://docs.microsoft.com/connectors/azureblob/) şu anda bu işlemi sunmuyor. Bunun yerine, bu işlemi [HTTP eylemini](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) veya başka bir [Blob Service REST API işlemini](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs)kullanarak çalıştırabilirsiniz.

   > [!IMPORTANT]
   > HTTP isteklerini ve yönetilen kimlikleri kullanarak güvenlik duvarlarının arkasındaki Azure depolama hesaplarına erişmek için, [güvenilen Microsoft hizmetlerine erişime izin veren istisna dışında](../connectors/connectors-create-api-azureblobstorage.md#access-trusted-service)depolama hesabınızı da ayarladığınızdan emin olun.

   [Snapshot Blob işlemini](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob)çalıştırmak için HTTP eylemi şu özellikleri belirtir:

   | Özellik | Gerekli | Örnek değer | Açıklama |
   |----------|----------|---------------|-------------|
   | **Yöntem** | Evet | `PUT`| Snapshot Blob işleminin kullandığı HTTP yöntemi |
   | **URI** | Evet | `https://{storage-account-name}.blob.core.windows.net/{blob-container-name}/{folder-name-if-any}/{blob-file-name-with-extension}` | Azure Global (genel) ortamında, bu sözdizimini kullanan bir Azure Blob Depolama dosyasının kaynak kimliği |
   | **Üst Bilgiler** | Evet, Azure Depolama için | `x-ms-blob-type` = `BlockBlob` <p>`x-ms-version` = `2019-02-02` | Azure `x-ms-blob-type` `x-ms-version` Depolama işlemleri için gereken üstbilgi değerleri. <p><p>**Önemli**: Azure Depolama için giden HTTP tetikleyici ve eylem `x-ms-version` isteklerinde üstbilgi, çalıştırmak istediğiniz işlem için özellik ve API sürümünü gerektirir. <p>Daha fazla bilgi için şu konulara bakın: <p><p>- [İstek başlıkları - Anlık Görüntü Blob](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob#request) <br>- [Azure Depolama hizmetleri için sürüm](https://docs.microsoft.com/rest/api/storageservices/versioning-for-the-azure-storage-services#specifying-service-versions-in-requests) |
   | **Sorgular** | Evet, bu operasyon için | `comp` = `snapshot` | Anlık Görüntü Blob işlemi için sorgu parametre adı ve değeri. |
   |||||

   Aşağıda, tüm bu özellik değerlerini gösteren örnek HTTP eylemi verilmiştir:

   ![Azure kaynağına erişmek için bir HTTP eylemi ekleme](./media/create-managed-service-identity/http-action-example.png)

1. Şimdi Http eylemine **Kimlik Doğrulama** özelliğini ekleyin. Yeni **parametre ekle** listesinden **Kimlik Doğrulama'yı**seçin.

   ![HTTP eylemine "Kimlik Doğrulama" özelliği ekleme](./media/create-managed-service-identity/add-authentication-property.png)

   > [!NOTE]
   > Tüm tetikleyiciler ve eylemler kimlik doğrulama türü eklemenize izin vermeyi desteklemez. Daha fazla bilgi için bkz: [Giden aramalara kimlik doğrulama ekle.](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

1. Kimlik **Doğrulama türü** listesinden **Yönetilen Kimlik'i**seçin.

   !["Kimlik Doğrulama" için "Yönetilen Kimlik" seçeneğini belirleyin](./media/create-managed-service-identity/select-managed-identity.png)

1. Yönetilen kimlik listesinden, senaryonuza göre kullanılabilir seçeneklerarasından seçim inizi alın.

   * Sistem tarafından atanan kimliği ayarlarsanız, zaten seçilmemişse **Sistem Atanmış Yönetilen Kimlik'i** seçin.

     !["Yönetilen Kimlik Atanan Sistem" seçeneğini belirleyin](./media/create-managed-service-identity/select-system-assigned-identity-for-action.png)

   * Kullanıcı tarafından atanmış bir kimlik ayarlarsanız, zaten seçilmemişse bu kimliği seçin.

     ![Kullanıcı tarafından atanan kimliği seçin](./media/create-managed-service-identity/select-user-assigned-identity-for-action.png)

   Bu örnek, **Yönetilen Kimlik Atanan Sistem**ile devam eder.

1. Bazı tetikleyicilerde ve eylemlerde, **Hedef Kitle** özelliği de hedef kaynak kimliğini ayarlamanız için görünür. Hedef **Kitle** özelliğini [hedef kaynak veya hizmet için kaynak kimliğine](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)ayarlayın. Aksi takdirde, varsayılan **olarak, Hedef Kitle** özelliği Azure Kaynak Yöneticisi'nin `https://management.azure.com/` kaynak kimliği olan kaynak kimliğini kullanır.

   > [!IMPORTANT]
   > Hedef kaynak kimliğinin, gerekli son daki kesikler de dahil olmak üzere Azure Active Directory 'nin (AD) beklediği değerle *tam olarak eşleştiğinden* emin olun. Örneğin, tüm Azure Blob Depolama hesaplarının kaynak kimliği, sondalı bir kesik gerektirir. Ancak, belirli bir depolama hesabının kaynak kimliği, sondalı bir kesik gerektirmez. Azure [AD'yi destekleyen Azure hizmetleri için kaynak adlarını](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)denetleyin.

   Bu örnek, kimlik `https://storage.azure.com/` doğrulaması için kullanılan erişim belirteçlerinin tüm depolama hesapları için geçerli olması için Hedef **Kitle** özelliğini belirler. Ancak, belirli bir depolama hesabı `https://fabrikamstorageaccount.blob.core.windows.net`için kök hizmet URL'sini de belirtebilirsiniz.

   ![Kaynak kimliğini hedeflemek için "Hedef Kitle" özelliğini ayarlama](./media/create-managed-service-identity/specify-audience-url-target-resource.png)

   Azure Depolama için Azure AD ile erişim yetkisi hakkında daha fazla bilgi için şu konulara bakın:

   * [Azure Etkin Dizin'i kullanarak Azure blob'larına ve kuyruklarına erişimi yetkilendirme](../storage/common/storage-auth-aad.md)
   * [Azure Etkin Dizini ile Azure Depolama'ya erişimi yetkilendirme](https://docs.microsoft.com/rest/api/storageservices/authorize-with-azure-active-directory#use-oauth-access-tokens-for-authentication)

1. Mantık uygulamasını istediğiniz şekilde oluşturmaya devam edin.

<a name="remove-identity"></a>

## <a name="disable-managed-identity"></a>Yönetilen kimliği devre dışı

Mantık uygulamanız için yönetilen bir kimliği kullanmayı durdurmak için şu seçeneklere sahipsiniz:

* [Azure portalında](#azure-portal-disable)
* [Azure Kaynak Yöneticisi şablonları](#template-disable)
* Azure PowerShell
  * [Rol atamayı kaldırma](../role-based-access-control/role-assignments-powershell.md)
  * [Kullanıcı tarafından atanan kimliği silme](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* Azure CLI
  * [Rol atamayı kaldırma](../role-based-access-control/role-assignments-cli.md)
  * [Kullanıcı tarafından atanan kimliği silme](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)
* Azure REST API
  * [Rol atamayı kaldırma](../role-based-access-control/role-assignments-rest.md)
  * [Kullanıcı tarafından atanan kimliği silme](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-rest.md)

Mantıksal uygulamanızı silerseniz, Azure yönetilen kimliği Azure AD'den otomatik olarak kaldırır.

<a name="azure-portal-disable"></a>

### <a name="disable-managed-identity-in-the-azure-portal"></a>Azure portalında yönetilen kimliği devre dışı

Azure portalında, öncelikle kimliğin hedef [kaynağınıza](#disable-identity-target-resource)erişimini kaldırın. Ardından, sistem tarafından atanan kimliği kapatın veya kullanıcı tarafından atanan kimliği [mantık uygulamanızdan](#disable-identity-logic-app)kaldırın.

<a name="disable-identity-target-resource"></a>

#### <a name="remove-identity-access-from-resources"></a>Kaynaklardan kimlik erişimini kaldırma

1. Azure [portalında,](https://portal.azure.com)yönetilen kimlik erişimini kaldırmak istediğiniz hedef Azure kaynağına gidin.

1. Hedef kaynağın menüsünden **Access denetimi (IAM)** seçeneğini belirleyin. Araç çubuğunun altında **Rol atamaları'nı**seçin.

1. Roller listesinde, kaldırmak istediğiniz yönetilen kimlikleri seçin. Araç çubuğunda **Kaldır'ı**seçin.

   > [!TIP]
   > **Kaldır** seçeneği devre dışı bırakılmışsa, büyük olasılıkla izinlerin yoktur. Kaynaklar için rolleri yönetmenize izinler hakkında daha fazla bilgi için [Azure Etkin Dizini'nde Yönetici rol izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bakın.

Yönetilen kimlik artık kaldırılır ve artık hedef kaynağa erişimi yoktur.

<a name="disable-identity-logic-app"></a>

#### <a name="disable-managed-identity-on-logic-app"></a>Mantık uygulamasında yönetilen kimliği devre dışı

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

1. Mantık uygulaması menüsünde, **Ayarlar**altında **Kimlik'i**seçin ve ardından kimliğiniz için aşağıdaki adımları izleyin:

   *  > **Kaydet'te** > **Save** **atanan Sistemi**seçin. Azure onaylamanızı istediğinde **Evet'i**seçin.

     ![Sistem tarafından atanan kimliği devre dışı](./media/create-managed-service-identity/disable-system-assigned-identity.png)

   * **Atanan Kullanıcı'yı** ve yönetilen kimliği seçin ve ardından **Kaldır'ı**seçin. Azure onaylamanızı istediğinde **Evet'i**seçin.

     ![Kullanıcı tarafından atanan kimliği kaldırma](./media/create-managed-service-identity/remove-user-assigned-identity.png)

Yönetilen kimlik artık mantık uygulamanızda devre dışı bırakılır.

<a name="template-disable"></a>

### <a name="disable-managed-identity-in-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonundaki yönetilen kimliği devre dışı

Bir Azure Kaynak Yöneticisi şablonu kullanarak mantık uygulamasının yönetilen `identity` kimliğini oluşturduysanız, nesnenin `type` alt özelliğini ' ye `None`ayarlayın. Sistem tarafından yönetilen kimlik için bu eylem, azure AD'deki ana kimliği de siler.

```json
"identity": {
   "type": "None"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps'ta güvenli erişim ve veri](../logic-apps/logic-apps-securing-a-logic-app.md)