---
title: Azure Etkinlik Hub'ları kaynaklarına erişmek için uygulamanın kimliğini doğrulama
description: Bu makalede, Azure Etkinlik Hub'ları kaynaklarına erişmek için Azure Etkin Dizini ile bir uygulamanın kimlik doğrulaması hakkında bilgi verilmektedir
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: spelluru
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: spelluru
ms.openlocfilehash: 4cef49f138b96848b8e59cb5b2d0b185d4568aa9
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520992"
---
# <a name="authenticate-an-application-with-azure-active-directory-to-access-event-hubs-resources"></a>Etkinlik Hub'ları kaynaklarına erişmek için Azure Etkin Dizini ile uygulamanın kimliğini doğrula
Microsoft Azure, Azure Etkin Dizini'ni (Azure AD) temel alan kaynaklar ve uygulamalar için tümleşik erişim denetimi yönetimi sağlar. Azure Etkinlik Hub'ları ile Azure AD kullanmanın önemli bir avantajı, kimlik bilgilerinizi artık kodda depolamanız gerekmemese de. Bunun yerine, Microsoft Identity platformundan Bir OAuth 2.0 erişim jetonu isteyebilirsiniz. Bir belirteç istemek için `https://eventhubs.azure.net/` kaynak adı (Kafka istemcileri için, `https://<namespace>.servicebus.windows.net`bir belirteç istemek için kaynak). Azure AD, uygulamayı çalıştıran güvenlik ilkesinin (kullanıcı, grup veya hizmet sorumlusu) kimliğini doğrular. Kimlik doğrulama başarılı olursa, Azure AD uygulamaya bir erişim belirteci döndürür ve uygulama daha sonra isteği Azure Olay Hub'ları kaynaklarına yetkilendirmek için erişim belirteci'ni kullanabilir.

Bir rol bir Azure REKLAM güvenlik ilkesine atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna, Olay Hub'larına veya altındaki herhangi bir kaynağa kadar kapsama edilebilir. Azure AD güvenliği, Azure kaynakları için bir kullanıcıya, bir gruba, bir uygulama hizmeti ilkesine veya yönetilen bir [kimliğe](../active-directory/managed-identities-azure-resources/overview.md)roller atayabilir. 

> [!NOTE]
> Rol tanımı, izinlerden oluşan bir koleksiyondur. Rol tabanlı erişim denetimi (RBAC), bu izinlerin rol ataması yoluyla nasıl zorlanır denetler. Rol ataması üç öğeden oluşur: güvenlik sorumlusu, rol tanımı ve kapsam. Daha fazla bilgi için farklı [rolleri anlama 'ya](../role-based-access-control/overview.md)bakın.

## <a name="built-in-roles-for-azure-event-hubs"></a>Azure Etkinlik Hub'ları için yerleşik roller
Azure, Azure AD ve OAuth kullanarak Etkinlik Hub'ları verilerine erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Azure Etkinlik Hub'ları Veri Sahibi](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-owner): Olay Hub'ları kaynaklarına tam erişim sağlamak için bu rolü kullanın.
- [Azure Olay Hub'ları Veri Gönderen](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-sender): Olay Hub'ları kaynaklarına gönderme erişimi sağlamak için bu rolü kullanın.
- [Azure Etkinlik Hub'ları Veri Alıcısı](../role-based-access-control/built-in-roles.md#azure-event-hubs-data-receiver): Olay Hub'ları kaynaklarına erişim sağlamak için bu rolü kullanın.   

> [!IMPORTANT]
> Önizleme sürümümüz, Sahibi veya Katılımcı rolüne Olay Hub'ları veri erişim ayrıcalıkları eklenmesini destekledi. Ancak, Sahibi ve Katılımcı rolü için veri erişim ayrıcalıkları artık onurlandırılan değildir. Sahibi veya Katılımcı rolünü kullanıyorsanız, Azure Etkinlik Hub'ları Veri Sahibi rolünü kullanmaya geçin.

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC rollerini atama  
RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme hakkında daha fazla bilgi edinmek için [bu makaleye](..//role-based-access-control/role-assignments-portal.md)bakın. 

Bir rol ataması için uygun kapsamı belirledikten sonra, Azure portalındaki kaynağa gidin. Kaynak için erişim denetimi (IAM) ayarlarını görüntüleyin ve rol atamalarını yönetmek için aşağıdaki yönergeleri izleyin:

> [!NOTE]
> Aşağıda açıklanan adımlar, Olay Hub'ları ad alanları altında etkinlik merkezinize bir rol atar, ancak herhangi bir Olay Hub'ları kaynağına kapsamlı bir rol atamak için aynı adımları izleyebilirsiniz.

1. Azure [portalında,](https://portal.azure.com/)Etkinlik Hub'ları ad alanınıza gidin.
2. Genel **Bakış** sayfasında, rol atamak istediğiniz etkinlik merkezini seçin.

    ![Etkinlik merkezinizi seçin](./media/authenticate-application/select-event-hub.png)
1. Etkinlik merkezinin erişim denetim ayarlarını görüntülemek için **Erişim Denetimi'ni (IAM)** seçin. 
1. Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin. Araç çubuğundaki **Ekle** düğmesini seçin ve ardından **rol ataması ekle'yi**seçin. 

    ![Araç çubuğuna düğme ekleme](./media/authenticate-application/role-assignments-add-button.png)
1. Rol **atamaekle** sayfasında aşağıdaki adımları yapın:
    1. Atamak istediğiniz **Olay Hub'ları rolünü** seçin. 
    1. Rolü atamak istediğiniz **güvenlik ilkesini** (kullanıcı, grup, hizmet sorumlusu) bulmak için arama yapın.
    1. Rol atamasını kaydetmek için **Kaydet'i** seçin. 

        ![Kullanıcıya rol atama](./media/authenticate-application/assign-role-to-user.png)
    4. Rolü atadığınız kimlik, bu rolün altında listelenir. Örneğin, aşağıdaki resim Azure kullanıcılarının Azure Etkinlik Hub'ları Veri Sahibi rolünde olduğunu gösterir. 
        
        ![Listedeki kullanıcı](./media/authenticate-application/user-in-list.png)

Olay Hub'ları ad alanına, kaynak grubuna veya aboneye kapsamlı bir rol atamak için benzer adımları izleyebilirsiniz. Rolü ve kapsamını tanımladıktan sonra, bu davranışı [bu GitHub konumundaki](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac)örneklerle sınayabilirsiniz.


## <a name="authenticate-from-an-application"></a>Uygulamadan kimlik doğrulaması yapma
Azure AD'yi Etkinlik Hub'larıyla kullanmanın önemli bir avantajı, kimlik bilgilerinizin artık kodunda depolanmasına gerek kalmamasıdır. Bunun yerine, Microsoft kimlik platformundan Bir OAuth 2.0 erişim jetonu isteyebilirsiniz. Azure AD, uygulamayı çalıştıran güvenlik ilkesinin (kullanıcı, grup veya hizmet sorumlusu) kimliğini doğrular. Kimlik doğrulama başarılı olursa, Azure AD erişim belirtecisini uygulamaya döndürür ve uygulama daha sonra istekleri Azure Olay Hub'larına yetkilendirmek için erişim belirteci'ni kullanabilir.

Aşağıdaki bölümler, Microsoft kimlik platformu 2.0 ile kimlik doğrulama için yerel uygulamanızı veya web uygulamanızı nasıl yapılandırabileceğinizi gösterir. Microsoft kimlik platformu 2.0 hakkında daha fazla bilgi için [Microsoft kimlik platformuna (v2.0) genel bakış](../active-directory/develop/v2-overview.md)bakın.

OAuth 2.0 kod hibe akışına genel bir bakış için, [OAuth 2.0 kod hibe akışını kullanarak Azure Active Directory web uygulamalarına erişimi yetkilendirme](../active-directory/develop/v2-oauth2-auth-code-flow.md)bölümüne bakın.

### <a name="register-your-application-with-an-azure-ad-tenant"></a>Uygulamanızı azure AD kiracısıyla kaydedin
Etkinlik Hub'ları kaynaklarını yetkilendirmek için Azure AD'yi kullanmanın ilk adımı, istemci uygulamanızı [Azure portalından](https://portal.azure.com/)bir Azure AD kiracısına kaydetmektir. İstemci başvurunuzu kaydettirdiğinizde, uygulama yla ilgili bilgileri AD'ye salarsınız. Azure AD daha sonra, uygulamanızı Azure AD çalışma süresiyle ilişkilendirmek için kullanabileceğiniz bir istemci kimliği (uygulama kimliği olarak da adlandırılır) sağlar. İstemci kimliği hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesnelerine](../active-directory/develop/app-objects-and-service-principals.md)bakın. 

Aşağıdaki resimler, bir web uygulamasını kaydetme adımlarını gösterir:

![Bir uygulamayı kaydetme](./media/authenticate-application/app-registrations-register.png)

> [!Note]
> Başvurunuzu yerel bir uygulama olarak kaydettirürseniz, Yönlendirme URI için geçerli bir URI belirtebilirsiniz. Yerel uygulamalar için bu değerin gerçek bir URL olması gerekmez. Web uygulamaları için, yeniden yönlendirme URI geçerli bir URI olmalıdır, çünkü belirteçleri sağlanan URL belirtir.

Başvurunuzu kaydettikten sonra **Ayarlar**altında **Uygulama (istemci) kimliğini** görürsünüz:

![Kayıtlı başvurunun başvuru kimliği](./media/authenticate-application/application-id.png)

Bir uygulamayı Azure AD'ye kaydetme hakkında daha fazla bilgi [için](../active-directory/develop/quickstart-v2-register-an-app.md)bkz.


### <a name="create-a-client-secret"></a>İstemci sırrı oluşturma   
Uygulama, bir belirteç talep ederken kimliğini kanıtlamak için bir istemci sırrı gerekir. İstemci sırrını eklemek için aşağıdaki adımları izleyin.

1. Azure portalındaki uygulama kaydınıza gidin.
1. **Sertifikalar & sırları** ayarını seçin.
1. **İstemci sırları**altında, yeni bir sır oluşturmak için **Yeni istemci sırrı** nı seçin.
1. Gizli için bir açıklama sağlayın ve istenen son kullanma aralığını seçin.
1. Yeni sırrın değerini hemen güvenli bir konuma kopyalayın. Dolgu değeri size yalnızca bir kez görüntülenir.

    ![Gizli anahtar](./media/authenticate-application/client-secret.png)


### <a name="client-libraries-for-token-acquisition"></a>Belirteç edinimi için istemci kitaplıkları  
Uygulamanızı kaydettikten ve Azure Etkinlik Hub'larında veri gönderme/alma izni verdikten sonra, bir güvenlik ilkesinin kimliğini doğrulamak ve OAuth 2.0 jetonunu satın almak için başvurunuza kod ekleyebilirsiniz. Belirteci doğrulamak ve elde etmek için, Microsoft [kimlik doğrulama kitaplıklarından](../active-directory/develop/reference-v2-libraries.md) birini veya OpenID veya Connect 1.0'ı destekleyen başka bir açık kaynak kitaplığını kullanabilirsiniz. Uygulamanız daha sonra, Azure Etkinlik Hub'larına karşı bir isteği yetkilendirmek için erişim jetonunu kullanabilir.

Belirteçleri satın almanın desteklendiği senaryoların listesi için ,NET GitHub deposu [için Microsoft Kimlik Doğrulama Kitaplığı'nın (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) [Senaryolar](https://aka.ms/msal-net-scenarios) bölümüne bakın.

## <a name="samples"></a>Örnekler
- [Microsoft.Azure.EventHubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/Rbac). 
    
    Bu örnekler eski **Microsoft.Azure.EventHubs** kitaplığını kullanır, ancak en son **Azure.Messaging.EventHubs** kitaplığını kullanarak kolayca güncelleştirebilirsiniz. Örneği eski kitaplığı kullanmaktan yenisine taşımak [için Microsoft.Azure.EventHub'larından Azure.Messaging.EventHubs'a geçiş kılavuzuna](https://github.com/Azure/azure-sdk-for-net/blob/master/sdk/eventhub/Azure.Messaging.EventHubs/MigrationGuide.md)bakın.
- [Azure.Messaging.EventHubs örnekleri](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Azure.Messaging.EventHubs/ManagedIdentityWebApp)

    Bu örnek en son **Azure.Messaging.EventHubs** kitaplığını kullanacak şekilde güncelleştirildi.

## <a name="next-steps"></a>Sonraki adımlar
- RBAC hakkında daha fazla bilgi edinmek için [rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
- Azure PowerShell, Azure CLI veya REST API ile RBAC rol atamalarının nasıl atayılmayı ve yöneteceğimizi öğrenmek için aşağıdaki makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-powershell.md)  
    - [Azure CLI ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-rest.md)
    - [Azure Kaynak Yöneticisi Şablonları ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-template.md)

Aşağıdaki ilgili makalelere bakın:
- [Etkinlik Hub'ları Kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın](authenticate-managed-identity.md)
- [Paylaşılan Erişim İmzalarını kullanarak Azure Etkinlik Hub'larına istekleri nidoğrulatın](authenticate-shared-access-signature.md)
- [Azure Active Directory'yi kullanarak Etkinlik Hub kaynaklarına erişimi yetkilendirme](authorize-access-azure-active-directory.md)
- [Paylaşılan Erişim İmzalarını kullanarak Etkinlik Hubkaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md)

