---
title: Yönetilen kimliklerle kimlik doğrulaması
description: Yönetilen bir kimlik kullanarak kimlik bilgileriyle veya gizli anahtarlarla oturum açmadan diğer Azure Active Directory kiracılardaki kaynaklara erişin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 02/10/2020
ms.openlocfilehash: 82710a66cdf7874c745070e49b2c7aff7bc8816d
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77117529"
---
# <a name="authenticate-access-to-azure-resources-by-using-managed-identities-in-azure-logic-apps"></a>Azure Logic Apps içindeki yönetilen kimlikleri kullanarak Azure kaynaklarına erişimi kimlik doğrulaması

Diğer Azure Active Directory (Azure AD) kiracılarındaki kaynaklara erişmek ve oturum açmadan kimliğinizi doğrulamak için, mantıksal uygulamanız kimlik bilgileri veya gizli dizileri yerine [yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md) (eski adıyla YÖNETILEN HIZMET KIMLIĞI veya MSI) kullanabilir. Azure bu kimliği sizin için yönetir ve gizli dizileri sağlamanız veya döndürmenize gerek olmadığından kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur.

Azure Logic Apps hem [*sistem tarafından atanan*](../active-directory/managed-identities-azure-resources/overview.md) hem de [*Kullanıcı tarafından atanan*](../active-directory/managed-identities-azure-resources/overview.md) yönetilen kimlikleri destekler. Mantıksal uygulamanız, sistem tarafından atanan kimlik veya *tek* bir kullanıcı tarafından atanan kimlik kullanabilir, bu da bir Logic Apps grubu genelinde paylaşabilir, ancak ikisini birden kullanamazsınız. Şu anda yalnızca [belirli yerleşik Tetikleyiciler ve eylemler](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) , yönetilmeyen bağlayıcılar veya bağlantılar değil, yönetilen kimlikleri destekler, örneğin:

* HTTP
* Azure İşlevleri
* Azure API Management
* Azure Uygulama Hizmetleri

Bu makalede mantıksal uygulamanız için her iki tür yönetilen kimliğin nasıl ayarlanacağı gösterilmektedir. Daha fazla bilgi için şu konulara bakın:

* [Yönetilen kimlikleri destekleyen Tetikleyiciler ve eylemler](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Giden aramalarda desteklenen kimlik doğrulama türleri](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Mantıksal uygulamalar için yönetilen kimlik sınırları](../logic-apps/logic-apps-limits-and-config.md#managed-identity)
* [Yönetilen kimliklerle Azure AD kimlik doğrulamasını destekleyen Azure hizmetleri](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). Hem yönetilen kimlik hem de erişmeniz gereken hedef Azure kaynağının aynı Azure aboneliğini kullanması gerekir.

* Azure kaynağına bir yönetilen kimlik erişimi sağlamak için, bu kimliğin hedef kaynağına bir rol eklemeniz gerekir. Rol eklemek için, ilgili Azure AD kiracısındaki kimliklere roller atayabilecek [Azure ad Yöneticisi izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md) sahip olmanız gerekir.

* Erişmek istediğiniz hedef Azure kaynağı. Bu kaynakta, mantıksal uygulamanın hedef kaynağa erişim kimlik doğrulamasını sağlayan yönetilen kimlik için bir rol ekleyeceksiniz.

* [Yönetilen kimlikleri destekleyen tetikleyiciyi veya eylemleri](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound) kullanmak istediğiniz mantıksal uygulama

## <a name="enable-managed-identity"></a>Yönetilen kimliği etkinleştir

Kullanmak istediğiniz yönetilen kimliği ayarlamak için bu kimliğin bağlantısını izleyin:

* [Sistem tarafından atanan kimlik](#system-assigned)
* [Kullanıcı tarafından atanan kimlik](#user-assigned)

<a name="system-assigned"></a>

### <a name="enable-system-assigned-identity"></a>Sistem tarafından atanan kimliği etkinleştir

Kullanıcı tarafından atanan kimliklerin aksine, sistem tarafından atanan kimliği el ile oluşturmanız gerekmez. Mantıksal uygulamanız için sistem tarafından atanan kimliği ayarlamak için kullanabileceğiniz seçenekler şunlardır:

* [Azure Portal](#azure-portal-system-logic-app)
* [Azure Resource Manager şablonları](#template-system-logic-app)

<a name="azure-portal-system-logic-app"></a>

#### <a name="enable-system-assigned-identity-in-azure-portal"></a>Azure portal 'de sistem tarafından atanan kimliği etkinleştirme

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde, **Ayarlar**altında **kimlik**' i seçin.  > **kaydederken**sistem tarafından **atanan** > seçin. Azure 'un onaylamanızı isterse, **Evet**' i seçin.

   ![Sistem tarafından atanan kimliği etkinleştirme](./media/create-managed-service-identity/enable-system-assigned-identity.png)

   > [!NOTE]
   > Yalnızca tek bir yönetilen kimliğiniz olduğunu belirten bir hata alırsanız, mantıksal uygulamanız zaten Kullanıcı tarafından atanan kimlikle ilişkilendirilir. Sistem tarafından atanan kimliği ekleyebilmeniz için önce mantıksal uygulamanızdan Kullanıcı tarafından atanan kimliği *kaldırmanız* gerekir.

   Mantıksal uygulamanız artık Azure Active Directory kayıtlı olan ve bir nesne KIMLIĞIYLE temsil edilen sistem tarafından atanan kimliği kullanabilir.

   ![Sistem tarafından atanan kimliğin nesne KIMLIĞI](./media/create-managed-service-identity/object-id-system-assigned-identity.png)

   | Özellik | Değer | Açıklama |
   |----------|-------|-------------|
   | **Nesne KIMLIĞI** | <*Identity-kaynak kimliği*> | Bir Azure AD kiracısında mantıksal uygulamanız için sistem tarafından atanan kimliği temsil eden bir genel benzersiz tanımlayıcı (GUID) |
   ||||

1. Şimdi bu konunun ilerleyen kısımlarında bu [kimlik için kaynağa erişim sağlayan adımları](#access-other-resources) izleyin.

<a name="template-system-logic-app"></a>

#### <a name="enable-system-assigned-identity-in-azure-resource-manager-template"></a>Azure Resource Manager şablonunda sistem tarafından atanan kimliği etkinleştirme

Logic Apps gibi Azure kaynaklarını oluşturma ve dağıtmaya otomatik hale getirmek için [Azure Resource Manager şablonları](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)kullanabilirsiniz. Şablondaki mantıksal uygulamanız için sistem tarafından atanan yönetilen kimliği etkinleştirmek üzere, `identity` nesnesini ve `type` alt özelliğini şablondaki mantıksal uygulamanın kaynak tanımına ekleyin, örneğin:

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

Azure mantıksal uygulama kaynak tanımınızı oluşturduğunda, `identity` nesnesi bu ek özellikleri alır:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Özellik (JSON) | Değer | Açıklama |
|-----------------|-------|-------------|
| `principalId` | <*asıl kimliği*> | Azure AD kiracısında mantıksal uygulamanızı temsil eden yönetilen kimliğin hizmet sorumlusu nesnesinin genel benzersiz tanıtıcısı (GUID). Bu GUID bazen bir "nesne KIMLIĞI" veya `objectID`olarak görünür. |
| `tenantId` | <*Azure-AD-KIRACı kimliği*> | Mantıksal uygulamanın artık bir üyesi olduğu Azure AD kiracısını temsil eden genel benzersiz tanımlayıcı (GUID). Azure AD kiracısı içinde hizmet sorumlusu, mantıksal uygulama örneğiyle aynı ada sahiptir. |
||||

<a name="user-assigned"></a>

### <a name="enable-user-assigned-identity"></a>Kullanıcı tarafından atanan kimliği etkinleştir

Mantıksal uygulamanız için Kullanıcı tarafından atanan bir yönetilen kimlik ayarlamak için, önce bu kimliği ayrı bir tek başına Azure kaynağı olarak oluşturmanız gerekir. Kullanabileceğiniz seçenekler şunlardır:

* [Azure Portal](#azure-portal-user-identity)
* [Azure Resource Manager şablonları](#template-user-identity)
* Azure PowerShell
  * [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
  * [Rol ataması Ekle](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md)
* Azure CLI
  * [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)
  * [Rol ataması Ekle](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md)
* Azure REST API
  * [Kullanıcı tarafından atanan kimlik oluşturma](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-rest.md)
  * [Rol ataması Ekle](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-user-identity"></a>

#### <a name="create-user-assigned-identity-in-the-azure-portal"></a>Azure portal Kullanıcı tarafından atanan kimlik oluşturma

1. [Azure Portal](https://portal.azure.com), herhangi bir sayfadaki arama kutusuna `managed identities`girin ve **Yönetilen kimlikler**' i seçin.

   !["Yönetilen kimlikler" i bulun ve seçin](./media/create-managed-service-identity/find-select-managed-identities.png)

1. **Yönetilen kimlikler**altında **Ekle**' yi seçin.

   ![Yeni yönetilen kimlik Ekle](./media/create-managed-service-identity/add-user-assigned-identity.png)

1. Yönetilen Kimliğiniz hakkında bilgi sağlayın ve ardından **Oluştur**' u seçin, örneğin:

   ![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/create-managed-service-identity/create-user-assigned-identity.png)

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Kaynak Adı** | Yes | <*Kullanıcı tarafından atanan kimlik-adı*> | Kullanıcı tarafından atanan kimliğinize verilecek ad. Bu örnek, "Fabrikam-User-atanan-Identity" kullanır. |
   | **Abonelik** | Yes | <*Azure-subscription-name*> | Kullanılacak Azure aboneliğinin adı |
   | **Kaynak grubu** | Yes | <*Azure-Resource-Group-name*> | Kullanılacak kaynak grubunun adı. Yeni bir grup oluşturun veya mevcut bir grubu seçin. Bu örnek, "Fabrikam-Managed-dentities-RG" adlı yeni bir grup oluşturur. |
   | **Konum** | Yes | *Azure-region*> < | Kaynağınız hakkındaki bilgilerin depolanacağı Azure bölgesi. Bu örnek, "Batı ABD" kullanır. |
   |||||

   Artık Kullanıcı tarafından atanan kimliği mantıksal uygulamanıza ekleyebilirsiniz. Mantıksal uygulamanıza birden fazla kullanıcı tarafından atanan kimlik ekleyemezsiniz.

1. Azure portal mantıksal uygulama tasarımcısında mantıksal uygulamanızı bulun ve açın.

1. Mantıksal uygulama menüsünde, **Ayarlar**altında **kimlik**' i seçin ve ardından **Kullanıcı tarafından atanan** > **Ekle**' yi seçin.

   ![Kullanıcı tarafından atanan yönetilen kimlik Ekle](./media/create-managed-service-identity/add-user-assigned-identity-logic-app.png)

1. **Kullanıcı tarafından atanan yönetilen kimlik Ekle** bölmesinde, **abonelik** listesinden, henüz seçili değilse Azure aboneliğinizi seçin. Bu abonelikteki *Tüm* yönetilen kimlikleri gösteren listede, istediğiniz kullanıcı tarafından atanan kimliği bulun ve seçin. Listeyi filtrelemek için, **Kullanıcı tarafından atanan Yönetilen kimlikler** arama kutusuna kimlik veya kaynak grubunun adını girin. İşiniz bittiğinde **Ekle**' yi seçin.

   ![Kullanılacak kullanıcı tarafından atanan kimliği seçin](./media/create-managed-service-identity/select-user-assigned-identity.png)

   > [!NOTE]
   > Yalnızca tek bir yönetilen kimliğiniz olduğunu belirten bir hata alırsanız, mantıksal uygulamanız zaten sistem tarafından atanan kimlikle ilişkilendirilmiş olur. Kullanıcı tarafından atanan kimliği ekleyebilmeniz için önce mantıksal uygulamanızda sistem tarafından atanan kimliği devre dışı bırakmanız gerekir.

   Mantıksal uygulamanız artık Kullanıcı tarafından atanan yönetilen kimlikle ilişkilendirilir.

   ![Kullanıcı tarafından atanan kimlikle ilişkilendirme](./media/create-managed-service-identity/added-user-assigned-identity.png)

1. Şimdi bu konunun ilerleyen kısımlarında bu [kimlik için kaynağa erişim sağlayan adımları](#access-other-resources) izleyin.

<a name="template-user-identity"></a>

#### <a name="create-user-assigned-identity-in-an-azure-resource-manager-template"></a>Azure Resource Manager şablonunda Kullanıcı tarafından atanan kimlik oluşturma

Logic Apps gibi Azure kaynaklarını oluşturma ve dağıtmaya otomatik hale getirmek için, [kimlik doğrulaması için Kullanıcı tarafından atanan kimlikleri](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)destekleyen [Azure Resource Manager şablonları](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)kullanabilirsiniz. Şablonunuzun `resources` bölümünde mantıksal uygulamanızın kaynak tanımı şu öğeleri gerektirir:

* `type` özelliği `UserAssigned` olarak ayarlanan `identity` nesnesi

* `principalId` ve `clientId` özelliklerine sahip başka bir alt nesne olan kimliğin kaynak KIMLIĞINI belirten bir alt `userAssignedIdentities` nesnesi

Bu örnek, bir HTTP PUT isteği için mantıksal uygulama kaynak tanımını gösterir ve parametreli olmayan `identity` bir nesne içerir. PUT isteğinin ve sonraki GET işleminin yanıtı da bu `identity` nesnesine sahiptir:

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

| Özellik (JSON) | Değer | Açıklama |
|-----------------|-------|-------------|
| `principalId` | <*asıl kimliği*> | Azure AD kiracısında Kullanıcı tarafından atanan yönetilen kimliğin genel benzersiz tanıtıcısı (GUID) |
| `clientId` | <*ISTEMCI kimliği*> | Mantıksal uygulamanızın çalışma zamanı sırasında çağrılar için kullanılan yeni kimliği için bir genel benzersiz tanımlayıcı (GUID) |
||||

Şablonunuz yönetilen kimliğin kaynak tanımını da içeriyorsa, `identity` nesnesini parametreleştirebilirsiniz. Bu örnek, alt `userAssignedIdentities` nesnesinin, şablonunuzun `variables` bölümünde tanımladığınız bir `userAssignedIdentity` değişkenine nasıl başvurmuş olduğunu gösterir. Bu değişken, Kullanıcı tarafından atanan kimliğinizin kaynak KIMLIĞINE başvurur.

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

| Özellik (JSON) | Değer | Açıklama |
|-----------------|-------|-------------|
| `tenantId` | <*Azure-AD-KIRACı kimliği*> | Kullanıcı tarafından atanan kimliğin artık bir üye olduğu Azure AD kiracısını temsil eden genel benzersiz tanımlayıcı (GUID). Azure AD kiracısı içinde hizmet sorumlusu, Kullanıcı tarafından atanan kimlik adıyla aynı ada sahiptir. |
| `principalId` | <*asıl kimliği*> | Azure AD kiracısında Kullanıcı tarafından atanan yönetilen kimliğin genel benzersiz tanıtıcısı (GUID) |
| `clientId` | <*ISTEMCI kimliği*> | Mantıksal uygulamanızın çalışma zamanı sırasında çağrılar için kullanılan yeni kimliği için bir genel benzersiz tanımlayıcı (GUID) |
||||

<a name="access-other-resources"></a>

## <a name="give-identity-access-to-resources"></a>Kaynaklara kimlik erişimi verme

Kimlik doğrulaması için mantıksal uygulamanızın yönetilen kimliğini kullanabilmeniz için, kimliği kullanmayı planladığınız Azure kaynağında bu kimlik için erişim ayarlayın. Bu görevi gerçekleştirmek için, hedef Azure kaynağında ilgili kimliğe uygun rolü atayın. Kullanabileceğiniz seçenekler şunlardır:

* [Azure Portal](#azure-portal-assign-access)
* [Azure Resource Manager şablonu](../role-based-access-control/role-assignments-template.md)
* Azure PowerShell ([New-Azroleatama](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment))-daha fazla bilgi için bkz. [Azure RBAC ve Azure PowerShell kullanarak rol ataması ekleme](../role-based-access-control/role-assignments-powershell.md).
* Azure CLı ([az rol atama oluşturma](https://docs.microsoft.com/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create))-daha fazla bilgi için bkz. [Azure RBAC ve Azure CLI kullanarak rol ataması ekleme](../role-based-access-control/role-assignments-cli.md).
* [Azure REST API](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-assign-access"></a>

### <a name="assign-access-in-the-azure-portal"></a>Azure portal erişim atama

1. [Azure Portal](https://portal.azure.com), yönetilen kimliğinizin erişimini istediğiniz Azure kaynağına gidin.

1. Kaynak menüsünde, bu kaynak için geçerli rol atamalarını gözden geçirebileceğiniz **rol atamalarını** > **ERIŞIM denetimi (IAM)** seçeneğini belirleyin. Araç çubuğunda **ekle** > **rol ataması Ekle**' yi seçin.

   !["Rol ataması Ekle" > "Ekle" yi seçin](./media/create-managed-service-identity/add-role-to-resource.png)

   > [!TIP]
   > **Rol ataması Ekle** seçeneği devre dışıysa, büyük olasılıkla izinlerinizin olması gerekmez. Kaynakları kaynaklar için yönetmenizi sağlayan izinler hakkında daha fazla bilgi için, [Azure Active Directory Içindeki yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bölümüne bakın.

1. **Rol ataması Ekle**' nin altında, kimliğinize hedef kaynağa gereken erişimi veren bir **rol** seçin.

   Bu konunun örneği için, kimliğiniz bir [Azure depolama kapsayıcısındaki bloba erişebilen bir role](../storage/common/storage-auth-aad.md#assign-rbac-roles-for-access-rights)ihtiyaç duyuyor.

   !["Depolama Blobu verileri katılımcısı" rolünü seçin](./media/create-managed-service-identity/select-role-for-identity.png)

1. Yönetilen Kimliğiniz için şu adımları izleyin:

   * **Sistem tarafından atanan kimlik**

     1. **Erişim ata** kutusunda **mantıksal uygulama**' yı seçin. **Abonelik** özelliği göründüğünde, kimliğiniz Ile ilişkili Azure aboneliğini seçin.

        ![Sistem tarafından atanan kimlik için erişimi seçin](./media/create-managed-service-identity/assign-access-system.png)

     1. **Seçim** kutusu altında, listeden mantıksal uygulamanızı seçin. Liste çok uzunsa, listeyi filtrelemek için **seçim** kutusunu kullanın.

        ![Sistem tarafından atanan kimlik için mantıksal uygulama seçin](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

   * **Kullanıcı tarafından atanan kimlik**

     1. **Erişim ata** kutusunda, **Kullanıcı tarafından atanan yönetilen kimlik**' i seçin. **Abonelik** özelliği göründüğünde, kimliğiniz Ile ilişkili Azure aboneliğini seçin.

        ![Kullanıcı tarafından atanan kimlik için erişimi seçin](./media/create-managed-service-identity/assign-access-user.png)

     1. **Seçim** kutusu altında, listeden kimliğinizi seçin. Liste çok uzunsa, listeyi filtrelemek için **seçim** kutusunu kullanın.

        ![Kullanıcı tarafından atanan kimliğinizi seçin](./media/create-managed-service-identity/add-permissions-select-user-assigned-identity.png)

1. İşiniz bittiğinde **Kaydet**'i seçin.

   Hedef kaynağın rol atamaları listesi artık seçili yönetilen kimliği ve rolü gösterir. Bu örnek, bir mantıksal uygulama için sistem tarafından atanan kimliği ve bir grup diğer Logic Apps için Kullanıcı tarafından atanan kimliği nasıl kullanabileceğinizi gösterir.

   ![Hedef kaynağa Yönetilen kimlikler ve roller eklendi](./media/create-managed-service-identity/added-roles-for-identities.png)

   Daha fazla bilgi için [Azure Portal kullanarak bir kaynağa yönetilen bir kimlik erişimi atayın](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md).

1. Şimdi, yönetilen kimlikleri destekleyen bir tetikleyici veya eylemde [kimlik doğrulamak için adımları](#authenticate-access-with-identity) izleyin.

<a name="authenticate-access-with-identity"></a>

## <a name="authenticate-access-with-managed-identity"></a>Yönetilen kimlikle erişimin kimliğini doğrulama

[Mantıksal uygulamanız için yönetilen kimliği etkinleştirdikten](#azure-portal-system-logic-app) ve [Bu kimliğe hedef kaynağa veya varlığa erişim izni](#access-other-resources)verdikten sonra, bu kimliği [yönetilen kimlikleri destekleyen Tetikleyiciler ve eylemler](logic-apps-securing-a-logic-app.md#managed-identity-authentication)' de kullanabilirsiniz.

> [!IMPORTANT]
> Sistem tarafından atanan kimliği kullanmak istediğiniz bir Azure işleviniz varsa, önce [Azure işlevleri için kimlik doğrulamasını etkinleştirin](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

Bu adımlarda, Azure portal aracılığıyla yönetilen kimliğin bir tetikleyici veya eylemle nasıl kullanılacağı gösterilmektedir. Bir tetikleyici veya eylemin temel alınan JSON tanımında yönetilen kimliği belirtmek için bkz. [yönetilen kimlik kimlik doğrulaması](../logic-apps/logic-apps-securing-a-logic-app.md#managed-identity-authentication).

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Henüz yapmadıysanız, [yönetilen kimlikleri destekleyen tetikleyiciyi veya eylemi](logic-apps-securing-a-logic-app.md#managed-identity-authentication)ekleyin.

   Örneğin, HTTP tetikleyicisi veya eylemi mantıksal uygulamanız için etkinleştirdiğiniz sistem tarafından atanan kimliği kullanabilir. Genel olarak, HTTP tetikleyicisi veya eylemi, erişmek istediğiniz kaynağı veya varlığı belirtmek için bu özellikleri kullanır:

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Yöntem** | Yes | Çalıştırmak istediğiniz işlem tarafından kullanılan HTTP yöntemi |
   | **KULLANıLMAMıŞSA** | Yes | Hedef Azure kaynağına veya varlığına erişmek için uç nokta URL 'SI. URI sözdizimi genellikle Azure kaynağı veya hizmeti için [kaynak kimliğini](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) içerir. |
   | **Üst Bilgiler** | Hayır | İçerik türü gibi giden isteğe dahil etmek veya istediğiniz tüm üst bilgi değerleri |
   | **Sorgular** | Hayır | Belirli bir işlemin parametresi ya da çalıştırmak istediğiniz işlem için API sürümü gibi isteğe dahil etmek istediğiniz veya isteğe dahil etmek istediğiniz sorgu parametreleri |
   | **Kimlik doğrulaması** | Yes | Hedef kaynağa veya varlığa erişimi doğrulamak için kullanılacak kimlik doğrulaması türü |
   ||||

   Belirli bir örnek olarak, [anlık görüntü blobu işlemini](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob) , Azure depolama hesabındaki bir blob üzerinde daha önce Kimliğiniz için daha önce ayarladığınız bir blob üzerinde çalıştırmak istediğinizi varsayalım. Ancak, [Azure Blob depolama Bağlayıcısı](https://docs.microsoft.com/connectors/azureblob/) Şu anda bu işlemi sunmaz. Bunun yerine, [http eylemini](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) veya başka bir [BLOB hizmeti REST API işlemini](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs)kullanarak bu işlemi çalıştırabilirsiniz.

   > [!IMPORTANT]
   > HTTP isteklerini ve yönetilen kimlikleri kullanarak güvenlik duvarlarının arkasındaki Azure depolama hesaplarına erişmek için, depolama hesabınızı [Güvenilen Microsoft hizmetlerinin erişimine izin veren özel durumla](../connectors/connectors-create-api-azureblobstorage.md#access-trusted-service)ayarladığınızdan emin olun.

   [Anlık görüntü blobu işlemini](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob)ÇALıŞTıRMAK için http eylemi şu özellikleri belirtir:

   | Özellik | Gerekli | Örnek değer | Açıklama |
   |----------|----------|---------------|-------------|
   | **Yöntem** | Yes | `PUT`| Anlık görüntü blobu işleminin kullandığı HTTP yöntemi |
   | **KULLANıLMAMıŞSA** | Yes | `https://{storage-account-name}.blob.core.windows.net/{blob-container-name}/{folder-name-if-any}/{blob-file-name-with-extension}` | Bu söz dizimini kullanan Azure genel (genel) ortamındaki bir Azure Blob depolama dosyasının kaynak KIMLIĞI |
   | **Üst Bilgiler** | Evet, Azure depolama için | `x-ms-blob-type` = `BlockBlob` <p>`x-ms-version` = `2019-02-02` | Azure depolama işlemleri için gereken `x-ms-blob-type` ve `x-ms-version` üst bilgi değerleri. <p><p>**Önemli**: giden http tetikleyicisinde ve Azure depolama için eylem isteklerinde, üst bilgi, çalıştırmak istediğiniz işlem için `x-ms-version` ÖZELLIĞINI ve API sürümünü gerektirir. <p>Daha fazla bilgi için şu konulara bakın: <p><p>- [istek üstbilgileri-anlık görüntü blobu](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob#request) <br>[Azure depolama hizmetleri için - sürümü oluşturma](https://docs.microsoft.com/rest/api/storageservices/versioning-for-the-azure-storage-services#specifying-service-versions-in-requests) |
   | **Sorgular** | Evet, bu işlem için | `comp` = `snapshot` | Anlık görüntü blobu işleminin sorgu parametresi adı ve değeri. |
   |||||

   Bu özellik değerlerinin tümünü gösteren örnek HTTP eylemi aşağıda verilmiştir:

   ![Azure kaynağına erişmek için HTTP eylemi ekleme](./media/create-managed-service-identity/http-action-example.png)

1. Şimdi HTTP eylemine **kimlik doğrulama** özelliğini ekleyin. **Yeni parametre Ekle** listesinden **kimlik doğrulaması**' nı seçin.

   ![HTTP eylemine "Authentication" özelliği Ekle](./media/create-managed-service-identity/add-authentication-property.png)

   > [!NOTE]
   > Tüm tetikleyiciler ve eylemler desteği, kimlik doğrulama türü eklemenize izin vermez. Daha fazla bilgi için bkz. [giden çağrılara kimlik doğrulama ekleme](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound).

1. **Kimlik doğrulama türü** listesinden **yönetilen kimlik**' i seçin.

   !["Kimlik doğrulaması" için "yönetilen kimlik" i seçin](./media/create-managed-service-identity/select-managed-identity.png)

1. Yönetilen kimlik listesinden, senaryonuza bağlı olarak kullanılabilir seçenekler arasından seçim yapın.

   * Sistem tarafından atanan kimliği ayarlarsanız, zaten seçili değilse, **sistem tarafından atanan yönetilen kimlik** ' i seçin.

     !["Sistem tarafından atanmış yönetilen kimlik" i seçin](./media/create-managed-service-identity/select-system-assigned-identity-for-action.png)

   * Kullanıcı tarafından atanan bir kimlik ayarlarsanız, zaten seçili değilse bu kimliği seçin.

     ![Kullanıcı tarafından atanan kimliği seçin](./media/create-managed-service-identity/select-user-assigned-identity-for-action.png)

   Bu örnek, **sistem tarafından atanan yönetilen kimlik**ile devam eder.

1. Bazı Tetikleyiciler ve eylemlerde, hedef kaynak KIMLIĞINI ayarlamanız için **izleyici** özelliği de görüntülenir. [Hedef kaynak veya hizmetin kaynak kimliği için hedef](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) **kitle** özelliğini ayarlayın. Aksi takdirde, varsayılan olarak, **hedef kitle** özelliği, Azure Resource Manager kaynak kimliği olan `https://management.azure.com/` kaynak kimliğini kullanır.

   > [!IMPORTANT]
   > Hedef kaynak KIMLIĞININ, tüm gerekli eğik çizgiler de dahil olmak üzere, Azure Active Directory (AD) beklediği değerle *tam olarak eşleştiğinden* emin olun. Örneğin, tüm Azure Blob depolama hesaplarının kaynak KIMLIĞI, sonunda eğik çizgi gerektirir. Ancak, belirli bir depolama hesabının kaynak KIMLIĞI, sonunda eğik çizgi gerektirmez. [Azure AD 'yi destekleyen Azure hizmetleri için kaynak kimliklerini](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)denetleyin.

   Bu örnek, kimlik doğrulaması için kullanılan erişim belirteçlerinin tüm depolama hesapları için geçerli olması için **hedef kitle** özelliğini `https://storage.azure.com/` olarak ayarlar. Ancak, belirli bir depolama hesabı için `https://fabrikamstorageaccount.blob.core.windows.net`kök hizmeti URL 'sini de belirtebilirsiniz.

   !["Hedef kitle" özelliğini hedef kaynak KIMLIĞI olarak ayarla](./media/create-managed-service-identity/specify-audience-url-target-resource.png)

   Azure depolama için Azure AD 'ye erişimi yetkilendirme hakkında daha fazla bilgi için şu konulara bakın:

   * [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](../storage/common/storage-auth-aad.md)
   * [Azure Active Directory ile Azure depolama 'ya erişim yetkisi verme](https://docs.microsoft.com/rest/api/storageservices/authorize-with-azure-active-directory#use-oauth-access-tokens-for-authentication)

1. Mantıksal uygulamayı istediğiniz şekilde oluşturmaya devam edin.

<a name="remove-identity"></a>

## <a name="disable-managed-identity"></a>Yönetilen kimliği devre dışı bırak

Mantıksal uygulamanız için yönetilen bir kimlik kullanmayı durdurmak için şu seçeneklere sahipsiniz:

* [Azure Portal](#azure-portal-disable)
* [Azure Resource Manager şablonları](#template-disable)
* Azure PowerShell
  * [Rol atamasını Kaldır](../role-based-access-control/role-assignments-powershell.md)
  * [Kullanıcı tarafından atanan kimliği Sil](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* Azure CLI
  * [Rol atamasını Kaldır](../role-based-access-control/role-assignments-cli.md)
  * [Kullanıcı tarafından atanan kimliği Sil](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)
* Azure REST API
  * [Rol atamasını Kaldır](../role-based-access-control/role-assignments-rest.md)
  * [Kullanıcı tarafından atanan kimliği Sil](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-rest.md)

Mantıksal uygulamanızı silerseniz Azure, yönetilen kimliği Azure AD 'den otomatik olarak kaldırır.

<a name="azure-portal-disable"></a>

### <a name="disable-managed-identity-in-the-azure-portal"></a>Azure portal yönetilen kimliği devre dışı bırakma

Azure portal, öncelikle kimliğin [hedef kaynağına](#disable-identity-target-resource)erişimini kaldırın. Sonra, sistem tarafından atanan kimliği kapatın veya [mantıksal uygulamanızdan](#disable-identity-logic-app)Kullanıcı tarafından atanan kimliği kaldırın.

<a name="disable-identity-target-resource"></a>

#### <a name="remove-identity-access-from-resources"></a>Kaynaklardan kimlik erişimini kaldırma

1. [Azure Portal](https://portal.azure.com), yönetilen kimlik için erişimi kaldırmak Istediğiniz hedef Azure kaynağına gidin.

1. Hedef kaynağın menüsünde, **erişim denetimi (IAM)** seçeneğini belirleyin. Araç çubuğu altında **rol atamaları**' nı seçin.

1. Roller listesinde, kaldırmak istediğiniz yönetilen kimlikleri seçin. Araç çubuğunda **Kaldır**' ı seçin.

   > [!TIP]
   > **Kaldır** seçeneği devre dışıysa, büyük olasılıkla izinlerinizin olması gerekmez. Kaynakları kaynaklar için yönetmenizi sağlayan izinler hakkında daha fazla bilgi için, [Azure Active Directory Içindeki yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bölümüne bakın.

Yönetilen kimlik artık kaldırılır ve artık hedef kaynağa erişemez.

<a name="disable-identity-logic-app"></a>

#### <a name="disable-managed-identity-on-logic-app"></a>Mantıksal uygulamada yönetilen kimliği devre dışı bırak

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde, **Ayarlar**' ın altında **kimlik**' i seçin ve ardından kimliğinize ilişkin adımları izleyin:

   *  > **kaydederken**sistem tarafından **atanan** > seçin. Azure 'un onaylamanızı isterse, **Evet**' i seçin.

     ![Sistem tarafından atanan kimliği devre dışı bırak](./media/create-managed-service-identity/disable-system-assigned-identity.png)

   * **Kullanıcı atandı** ve yönetilen kimlik ' i seçin ve ardından **Kaldır**' ı seçin. Azure 'un onaylamanızı isterse, **Evet**' i seçin.

     ![Kullanıcı tarafından atanan kimliği kaldırma](./media/create-managed-service-identity/remove-user-assigned-identity.png)

Yönetilen kimlik artık mantıksal uygulamanızda devre dışı bırakıldı.

<a name="template-disable"></a>

### <a name="disable-managed-identity-in-azure-resource-manager-template"></a>Azure Resource Manager şablonunda yönetilen kimliği devre dışı bırak

Mantıksal uygulamanın yönetilen kimliğini bir Azure Resource Manager şablonu kullanarak oluşturduysanız, `identity` nesnesinin `type` alt özelliğini `None`olarak ayarlayın. Sistem tarafından yönetilen kimlik için bu eylem ayrıca Azure AD 'den asıl KIMLIĞI de siler.

```json
"identity": {
   "type": "None"
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps 'da güvenli erişim ve veriler](../logic-apps/logic-apps-securing-a-logic-app.md)