---
title: Yönetilen kimliklerle kimlik doğrulama-Azure Logic Apps | Microsoft Docs
description: Oturum açmadan kimlik doğrulaması yapmak için, mantıksal uygulamanızın kimlik bilgileri veya gizli dizileri olmadan diğer Azure Active Directory (Azure AD) kiracılarındaki kaynaklara erişebilmesi için yönetilen bir kimlik (eski adıyla Yönetilen Hizmet Kimliği veya MSI) oluşturabilirsiniz.
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 03/29/2019
ms.openlocfilehash: b157db5032bd62ab443209f201b4ceded6e44cb5
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385559"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Azure Logic Apps yönetilen kimliklerle kimlik doğrulama ve kaynaklara erişme

Diğer Azure Active Directory (Azure AD) kiracılarındaki kaynaklara erişmek ve oturum açmadan kimliğinizi doğrulamak için, mantıksal uygulamanız kimlik bilgileri veya gizli dizileri yerine [yönetilen bir kimlik](../active-directory/managed-identities-azure-resources/overview.md) (eski adıyla YÖNETILEN HIZMET KIMLIĞI veya MSI) kullanabilir. Azure bu kimliği sizin için yönetir ve gizli dizileri sağlamanız veya döndürmenize gerek olmadığından kimlik bilgilerinizin güvenliğini sağlamaya yardımcı olur. Bu makalede, mantıksal uygulamanız için sistem tarafından atanan yönetilen bir kimlik ayarlama ve kullanma işlemlerinin nasıl yapılacağı gösterilir. Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları için Yönetilen kimlikler nelerdir?](../active-directory/managed-identities-azure-resources/overview.md)

> [!NOTE]
> Mantıksal uygulamanız, yönetilen kimlikleri yalnızca yönetilen kimlikleri destekleyen bağlayıcılarla kullanabilir. Şu anda yalnızca HTTP Bağlayıcısı yönetilen kimlikleri destekliyor.
>
> Şu anda her bir Azure aboneliğinde sistem tarafından atanan yönetilen kimliklerle en fazla 10 mantıksal uygulama iş akışı olabilir.

## <a name="prerequisites"></a>Önkoşullar

* Bir Azure aboneliği veya aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

* Sistem tarafından atanan yönetilen kimliği kullanmak istediğiniz mantıksal uygulama. Mantıksal uygulamanız yoksa, bkz. [ilk mantıksal uygulama iş akışınızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="enable-identity"></a>

## <a name="enable-managed-identity"></a>Yönetilen kimliği etkinleştir

Sistem tarafından atanan Yönetilen kimlikler için, bu kimliği el ile oluşturmanız gerekmez. Mantıksal uygulamanız için sistem tarafından atanan bir yönetilen kimlik ayarlamak için şu yolları kullanabilirsiniz: 

* [Azure portal](#azure-portal) 
* [Azure Resource Manager şablonları](#template) 
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md) 

<a name="azure-portal"></a>

### <a name="azure-portal"></a>Azure portal

Mantıksal uygulamanız için Azure portal aracılığıyla sistem tarafından atanan yönetilen bir kimliği etkinleştirmek üzere mantıksal uygulamanızın kimlik ayarlarındaki sistem tarafından **atanan** ayarını açın.

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde, **Ayarlar**altında **kimlik**' i seçin. 

1. **Sistem tarafından atanan** > **durum**altında **Açık**' ı seçin. Ardından,**Evet** **Kaydet** > ' i seçin.

   ![Yönetilen kimlik ayarını aç](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

   Mantıksal uygulamanızda artık Azure Active Directory kayıtlı sistem tarafından atanan bir yönetilen kimlik vardır:

   ![Nesne KIMLIĞI için GUID 'Ler](./media/create-managed-service-identity/object-id.png)

   | Özellik | Value | Açıklama | 
   |----------|-------|-------------| 
   | **Nesne KIMLIĞI** | <*kimlik-kaynak KIMLIĞI*> | Bir Azure AD kiracısında mantıksal uygulamanız için sistem tarafından atanan yönetilen kimliği temsil eden bir genel benzersiz tanımlayıcı (GUID) | 
   ||| 

<a name="template"></a>

### <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu

Logic Apps gibi Azure kaynaklarını oluşturma ve dağıtmaya otomatik hale getirmek istediğinizde [Azure Resource Manager şablonları](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md)kullanabilirsiniz. Mantıksal uygulamanız için bir şablon aracılığıyla sistem tarafından atanan bir yönetilen kimlik oluşturmak için, dağıtım şablonunuzda mantıksal `"identity"` uygulama iş `"type"` akışı tanımınıza öğe ve özellik ekleyin: 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Örneğin:

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

Azure mantıksal uygulamanızı oluşturduğunda, bu mantıksal uygulamanın iş akışı tanımı şu ek özellikleri içerir:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Özellik | Value | Açıklama | 
|----------|-------|-------------|
| **PrincipalId** | <*sorumlu KIMLIĞI*> | Azure AD kiracısında mantıksal uygulamayı temsil eden ve bazen bir "nesne KIMLIĞI" veya`objectID` | 
| **tenantId** | <*Azure-AD-kiracı KIMLIĞI*> | Mantıksal uygulamanın artık bir üye olduğu Azure AD kiracısını temsil eden bir genel benzersiz tanımlayıcı (GUID). Azure AD kiracısı içinde hizmet sorumlusu, mantıksal uygulama örneğiyle aynı ada sahiptir. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Yönetilen kimliğe sahip kaynaklara erişin

Mantıksal uygulamanız için sistem tarafından atanan bir yönetilen kimlik oluşturduktan sonra, [Bu kimliğe diğer Azure kaynaklarına erişim izni verebilirsiniz](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Bu kimliği, diğer [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md)gibi kimlik doğrulaması için de kullanabilirsiniz. 

> [!NOTE]
> Hem sistem tarafından atanan yönetilen kimlik hem de erişim atamak istediğiniz kaynak aynı Azure aboneliğine sahip olmalıdır.

### <a name="assign-access-to-managed-identity"></a>Yönetilen kimliğe erişim atama

Mantıksal uygulamanızın sistem tarafından atanan yönetilen kimliği için başka bir Azure kaynağına erişim vermek üzere aşağıdaki adımları izleyin:

1. Azure portal, yönetilen Kimliğiniz için erişim atamak istediğiniz Azure kaynağına gidin. 

1. Kaynak menüsünden **erişim denetimi (IAM)** seçeneğini belirleyin. Araç çubuğunda,**Rol Ekle ataması** **Ekle** > ' yi seçin.

   ![Rol ataması ekle](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. **Rol ataması Ekle**' nin altında, kimlik Için istediğiniz **rolü** seçin. 

1. **Erişim ata** özelliği ' nde, henüz seçili değilse, **Azure AD Kullanıcı, Grup veya hizmet sorumlusu**' nı seçin.

1. Mantıksal uygulamanızın adının ilk karakteriyle başlayarak **Seç** kutusunda mantıksal uygulamanızın adını girin. Mantıksal uygulamanız göründüğünde mantıksal uygulamayı seçin.

   ![Yönetilen kimlikle mantıksal uygulama seçin](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. İşiniz bittiğinde **Kaydet**’i seçin.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Mantıksal uygulamada yönetilen kimlikle kimlik doğrulama

Mantıksal uygulamanızı sistem tarafından atanan bir yönetilen kimlikle ayarladıktan ve bu kimlik için istediğiniz kaynağa erişim izni verdikten sonra, kimlik doğrulaması için artık bu kimliği kullanabilirsiniz. Örneğin, mantıksal uygulamanızın bir HTTP isteği veya bu kaynağa çağrı gönderebilmesi için bir HTTP eylemi kullanabilirsiniz. 

1. Mantıksal uygulamanızda **http** eylemini ekleyin.

1. Çağırmak istediğiniz kaynak için istek **yöntemi** ve **URI** konumu gibi bu eylem için gerekli ayrıntıları sağlayın.

   Örneğin, [Azure AD 'yi destekleyen bu Azure hizmetlerinden biriyle](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)Azure Active Directory (Azure AD) kimlik doğrulamasını kullandığınızı varsayalım. 
   **URI** kutusuna söz konusu Azure hizmeti için uç nokta URL 'sini girin. 
   Bu nedenle, Azure Resource Manager kullanıyorsanız **URI** özelliğine şu değeri girin:

   `https://management.azure.com/subscriptions/<Azure-subscription-ID>?api-version=2016-06-01`

1. HTTP eyleminde, **Gelişmiş seçenekleri göster**' i seçin.

1. **Kimlik doğrulama** listesinden **yönetilen kimlik**' i seçin. Bu kimlik doğrulamasını seçtikten sonra, **hedef kitle** özelliği varsayılan kaynak kimliği değeriyle görüntülenir:

   !["Yönetilen kimlik" i seçin](./media/create-managed-service-identity/select-managed-service-identity.png)

   > [!IMPORTANT]
   > 
   > **Hedef kitle** ÖZELLIĞINDE kaynak kimliği değeri, tüm gerekli eğik çizgiler de dahil olmak üzere Azure AD 'nin beklediği şekilde tam olarak eşleşmelidir. 
   > Bu kaynak KIMLIĞI değerlerini, Azure [ad 'yi destekleyen Azure hizmetlerini açıklayan bu tabloda](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)bulabilirsiniz. 
   > Örneğin, Azure Resoruce Manager kaynak KIMLIĞI kullanıyorsanız, URI 'nin sonundaki eğik çizgiye sahip olduğundan emin olun.

1. Mantıksal uygulamayı istediğiniz şekilde oluşturmaya devam edin.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Yönetilen kimliği kaldır

Mantıksal uygulamanızda sistem tarafından atanan bir yönetilen kimliği devre dışı bırakmak için Azure portal, Azure Resource Manager dağıtım şablonları veya Azure PowerShell aracılığıyla kimliği nasıl ayarladığınızdan benzer adımları izleyebilirsiniz.

Mantıksal uygulamanızı sildiğinizde Azure, mantıksal uygulamanızın sistem tarafından atanan kimliğini Azure AD 'den otomatik olarak kaldırır.

### <a name="azure-portal"></a>Azure portal

Mantıksal uygulamanıza yönelik sistem tarafından atanan yönetilen bir kimliği Azure portal kaldırmak için, mantıksal uygulamanızın kimlik ayarlarındaki sistem tarafından **atanan** ayarını kapatın.

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulama menüsünde, **Ayarlar**altında **kimlik**' i seçin. 

1. **Sistem tarafından atanan** > **durum**altında **kapalı**' yı seçin. Ardından,**Evet** **Kaydet** > ' i seçin.

   ![Yönetilen kimlik ayarını kapat](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Dağıtım şablonu

Mantıksal uygulamanın sistem tarafından atanan yönetilen kimliğini bir Azure Resource Manager dağıtım şablonuyla oluşturduysanız, `"identity"` `"type"` öğesinin özelliğini olarak `"None"`ayarlayın. Bu eylem ayrıca Azure AD 'den asıl KIMLIĞI de siler.

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.
