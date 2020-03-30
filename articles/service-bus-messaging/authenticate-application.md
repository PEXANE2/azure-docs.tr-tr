---
title: Azure Hizmet Veri Hizmeti Veri Hizmeti varlıklarına erişmek için uygulamanın kimliğini doğrulama
description: Bu makalede, Azure Hizmet Veri Hizmeti veri hizmeti varlıklarına erişmek için Azure Etkin Dizini ile bir uygulamanın kimlik doğrulaması hakkında bilgi verilmektedir (kuyruklar, konular, vb.)
services: service-bus-messaging
ms.service: event-hubs
documentationcenter: ''
author: axisc
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: aschhab
ms.openlocfilehash: 6a78e4d81921fae8dcb325e9d72df1eee7b99a3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79259298"
---
# <a name="authenticate-and-authorize-an-application-with-azure-active-directory-to-access-azure-service-bus-entities"></a>Azure Hizmet Veri Hizmeti Veri Hizmeti varlıklarına erişmek için Azure Active Directory ile bir uygulamanın kimliğini doğrulama ve yetkilendirme
Azure Servis Veri Servisi, Hizmet Veri Hizmeti kuruluşlarına (kuyruklar, konular, abonelikler veya filtreler) istekleri yetkilendirmek için Azure Active Directory (Azure AD) kullanma yı destekler. Azure AD ile, kullanıcı, grup veya uygulama hizmeti ilkesi olabilecek bir güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz. Roller ve rol atamaları hakkında daha fazla bilgi edinmek için farklı [rolleri anlama 'ya](../role-based-access-control/overview.md)bakın.

## <a name="overview"></a>Genel Bakış
Bir güvenlik sorumlusu (kullanıcı, grup veya uygulama) bir Hizmet Veri Servisi kuruluşuna erişmeye çalıştığında, isteğe izin verilmelidir. Azure AD ile kaynağa erişim iki adımlı bir işlemdir. 

 1. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2.0 belirteci döndürülür. Belirteç istemek için kaynak `https://servicebus.azure.net`adı.
 1. Ardından, belirteç, belirtilen kaynağa erişim yetkisi vermek üzere Servis Veri Servisi'ne yapılan isteğin bir parçası olarak geçirilir.

Kimlik doğrulama adımı, bir uygulama isteğinin çalışma zamanında bir OAuth 2.0 erişim belirteci içermesini gerektirir. Bir uygulama Azure VM, sanal makine ölçeği kümesi veya Azure İşlevi uygulaması gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için yönetilen bir kimliği kullanabilir. Yönetilen bir kimliktarafından Hizmet Veri Yolu hizmetine yönelik isteklerin nasıl doğrudoğruyapılacağını öğrenmek [için, Azure Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle Azure Hizmet Veri Yolu kaynaklarına kimlik doğrulama](service-bus-managed-service-identity.md)erişimi ne res.a.'e bakın. 

Yetkilendirme adımı, bir veya daha fazla RBAC rolünün güvenlik ilkesine atanmasını gerektirir. Azure Hizmet Veri Mes'ü, Hizmet Veri Servisi kaynakları için izin kümelerini kapsayan RBAC rolleri sağlar. Bir güvenlik ilkesine atanan roller, asıl ilkelerin sahip olacağı izinleri belirler. Azure Hizmet Veri Yolu'na RBAC rolleri atama hakkında daha fazla bilgi edinmek [için Azure Hizmet Veri Yolu için Yerleşik RBAC rolleri'ne](#built-in-rbac-roles-for-azure-service-bus)bakın. 

Hizmet Veri Tos'una istekte bulunan yerel uygulamalar ve web uygulamaları da Azure AD ile yetkilendirilebilir. Bu makalede, bir erişim jetonu istemek ve Hizmet Veri Servisi Veri Servisi kaynakları için istekleri ni yetkilendirmek için kullanmak nasıl gösterir. 


## <a name="assigning-rbac-roles-for-access-rights"></a>Erişim hakları için RBAC rolleri atama
Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Hizmet Veri Hizmetleri, Hizmet Veri Hizmeti varlıklarına erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar ve verilere erişmek için özel roller de tanımlayabilirsiniz.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna veya Hizmet Veri Servisi ad alanına kadar kapsama edilebilir. Azure AD güvenlik ilkesi, [Azure kaynakları için](../active-directory/managed-identities-azure-resources/overview.md)bir kullanıcı, bir grup, bir uygulama hizmeti ilkesi veya yönetilen bir kimlik olabilir.

## <a name="built-in-rbac-roles-for-azure-service-bus"></a>Azure Hizmet Veri Servisi Veri Servisi için yerleşik RBAC rolleri
Azure Hizmet Veri Servisi için, Azure portalı ve Azure kaynak yönetimi API'si aracılığıyla ad alanlarının ve ilgili tüm kaynakların *yönetimi, rol tabanlı erişim denetimi* (RBAC) modeli kullanılarak zaten korunmaktadır. Azure, Bir Hizmet Veri Servisi ad alanına erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Azure Hizmet Veri Veri Stoymu Veri Sahibi](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner): Hizmet Veri İşlemi ad alanına ve varlıklarına (kuyruklar, konular, abonelikler ve filtreler) veri erişimi sağlar
- [Azure Hizmet Veri Veri Gönderen :](../role-based-access-control/built-in-roles.md#azure-service-bus-data-sender)Hizmet Veri Göndereci ad alanına ve varlıklarına gönderme erişimi sağlamak için bu rolü kullanın.
- [Azure Hizmet Veri Veri Alıcısı](../role-based-access-control/built-in-roles.md#azure-service-bus-data-receiver): Hizmet Veri Alimi ad alanına ve varlıklarına erişim sağlamak için bu rolü kullanın. 

## <a name="resource-scope"></a>Kaynak kapsamı 
Bir güvenlik ilkesine Bir RBAC rolü atamadan önce, güvenlik ilkesinin sahip olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, yalnızca mümkün olan en dar kapsamı vermenin her zaman en iyisi olduğunu belirtir.

Aşağıdaki liste, en dar kapsamdan başlayarak Servis Veri Servisi kaynakları kaynaklarına erişimi kapsamdışı kullanabilirsiniz düzeyleri açıklar:

- **Sıra**, **konu**, veya **abonelik**: Rol ataması belirli Hizmet Veri Servisi varlığı için geçerlidir. Şu anda Azure portalı, abonelik düzeyinde Hizmet Veri Servisi RBAC rollerine kullanıcı/grup/yönetilen kimlik atamayı desteklemez. 
- **Service Bus namespace**: Rol ataması, Hizmet Veri Tos'unun tüm topolojisini ad alanı altında ve ilişkili tüketici grubuna kapsar.
- **Kaynak grubu**: Rol ataması kaynak grubu altındaki tüm Servis Veri Mes'leri kaynakları için geçerlidir.
- **Abonelik**: Rol ataması, abonelikteki tüm kaynak gruplarından tüm Servis Veri Mes'leri kaynakları için geçerlidir.

> [!NOTE]
> RBAC rol atamalarının yayılması beş dakika kadar sürebilir. 

Yerleşik rollerin nasıl tanımlandığı hakkında daha fazla bilgi için [bkz.](../role-based-access-control/role-definitions.md#management-and-data-operations) Özel RBAC rolleri oluşturma hakkında bilgi [için](../role-based-access-control/custom-roles.md)bkz.


## <a name="assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC rollerini atama  
RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme hakkında daha fazla bilgi edinmek için [bu makaleye](..//role-based-access-control/role-assignments-portal.md)bakın. 

Bir rol ataması için uygun kapsamı belirledikten sonra, Azure portalındaki kaynağa gidin. Kaynak için erişim denetimi (IAM) ayarlarını görüntüleyin ve rol atamalarını yönetmek için aşağıdaki yönergeleri izleyin:

> [!NOTE]
> Aşağıda açıklanan adımlar, Hizmet Veri Günü ad alanınıza bir rol atar. Desteklenen diğer kapsamlara (kaynak grubu, abonelik, vb.) rol atamak için aynı adımları izleyebilirsiniz.

1. Azure [portalında,](https://portal.azure.com/)Hizmet Veri Kurumu ad alanınıza gidin. Ad alanının erişim denetim ayarlarını görüntülemek için sol menüde **Erişim Denetimi 'ni (IAM)** seçin. Bir Hizmet Veri Yolunda ad alanı oluşturmanız gerekiyorsa, bu makaledeki yönergeleri izleyin: [Hizmet Veri Servisi İletisi ad alanı oluşturun.](service-bus-create-namespace-portal.md)

    ![Sol menüde Erişim Denetimi'ni seçin](./media/authenticate-application/select-access-control-menu.png)
1. Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin. Araç çubuğundaki **Ekle** düğmesini seçin ve ardından **rol ataması ekle'yi**seçin. 

    ![Araç çubuğuna düğme ekleme](./media/authenticate-application/role-assignments-add-button.png)
1. Rol **atamaekle** sayfasında aşağıdaki adımları yapın:
    1. Atamak istediğiniz **Servis Veri Servisi rolünü** seçin. 
    1. Rolü atamak istediğiniz **güvenlik ilkesini** (kullanıcı, grup, hizmet sorumlusu) bulmak için arama yapın.
    1. Rol atamasını kaydetmek için **Kaydet'i** seçin. 

        ![Kullanıcıya rol atama](./media/authenticate-application/assign-role-to-user.png)
    4. Rolü atadığınız kimlik, bu rolün altında listelenir. Örneğin, aşağıdaki resim Azure kullanıcılarının Azure Hizmet Veri Veri Sebunu sahibi rolünde olduğunu gösterir. 
        
        ![Listedeki kullanıcı](./media/authenticate-application/user-in-list.png)

Bir kaynak grubuna veya aboneye kapsamlı bir rol atamak için benzer adımları izleyebilirsiniz. Rolü ve kapsamını tanımladıktan sonra, bu davranışı [GitHub'daki örneklerle](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl)sınayabilirsiniz.


## <a name="authenticate-from-an-application"></a>Uygulamadan kimlik doğrulaması yapma
Hizmet Veri Servisi ile Azure AD kullanmanın önemli bir avantajı, kimlik bilgilerinizin artık kodunuzda depolanmasına gerek kalmamasıdır. Bunun yerine, Microsoft kimlik platformundan Bir OAuth 2.0 erişim jetonu isteyebilirsiniz. Azure AD, uygulamayı çalıştıran güvenlik ilkesinin (kullanıcı, grup veya hizmet sorumlusu) kimliğini doğrular. Kimlik doğrulama başarılı olursa, Azure AD erişim belirtecisini uygulamaya döndürür ve uygulama daha sonra istekleri Azure Hizmet Veri Servisi'ne yetkilendirmek için erişim belirteci'ni kullanabilir.

Aşağıdaki bölümler, Microsoft kimlik platformu 2.0 ile kimlik doğrulama için yerel uygulamanızı veya web uygulamanızı nasıl yapılandırabileceğinizi gösterir. Microsoft kimlik platformu 2.0 hakkında daha fazla bilgi için [Microsoft kimlik platformuna (v2.0) genel bakış](../active-directory/develop/v2-overview.md)bakın.

OAuth 2.0 kod hibe akışına genel bir bakış için, [OAuth 2.0 kod hibe akışını kullanarak Azure Active Directory web uygulamalarına erişimi yetkilendirme](../active-directory/develop/v2-oauth2-auth-code-flow.md)bölümüne bakın.

### <a name="register-your-application-with-an-azure-ad-tenant"></a>Uygulamanızı azure AD kiracısıyla kaydedin
Hizmet Veri Hizmeti Veri Hizmeti varlıklarını yetkilendirmek için Azure AD'yi kullanmanın ilk adımı, istemci uygulamanızı [Azure portalından](https://portal.azure.com/)bir Azure AD kiracısına kaydettirmektir. İstemci başvurunuzu kaydettirdiğinizde, uygulama yla ilgili bilgileri AD'ye salarsınız. Azure AD daha sonra, uygulamanızı Azure AD çalışma süresiyle ilişkilendirmek için kullanabileceğiniz bir istemci kimliği (uygulama kimliği olarak da adlandırılır) sağlar. İstemci kimliği hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesnelerine](../active-directory/develop/app-objects-and-service-principals.md)bakın. 

Aşağıdaki resimler, bir web uygulamasını kaydetme adımlarını gösterir:

![Bir uygulamayı kaydetme](./media/authenticate-application/app-registrations-register.png)

> [!Note]
> Başvurunuzu yerel bir uygulama olarak kaydettirürseniz, Yönlendirme URI için geçerli bir URI belirtebilirsiniz. Yerel uygulamalar için bu değerin gerçek bir URL olması gerekmez. Web uygulamaları için, yeniden yönlendirme URI geçerli bir URI olmalıdır, çünkü belirteçleri sağlanan URL belirtir.

Başvurunuzu kaydettikten sonra **Ayarlar**altında **Uygulama (istemci) kimliğini** görürsünüz:

![Kayıtlı başvurunun başvuru kimliği](./media/authenticate-application/application-id.png)

Bir uygulamayı Azure AD'ye kaydetme hakkında daha fazla bilgi [için](../active-directory/develop/quickstart-v2-register-an-app.md)bkz.

> [!IMPORTANT]
> TenantId ve **ApplicationId'e** dikkat edin. **ApplicationId** Uygulamayı çalıştırmak için bu değerlere ihtiyacınız olacaktır.

### <a name="create-a-client-secret"></a>İstemci sırrı oluşturma   
Uygulama, bir belirteç talep ederken kimliğini kanıtlamak için bir istemci sırrı gerekir. İstemci sırrını eklemek için aşağıdaki adımları izleyin.

1. Sayfada zaten değilseniz Azure portalındaki uygulama kaydınıza gidin.
1. Sol menüde **Sertifikalar & sırları** seçin.
1. **İstemci sırları**altında, yeni bir sır oluşturmak için **Yeni istemci sırrı** nı seçin.

    ![Yeni istemci gizli - düğmesi](./media/authenticate-application/new-client-secret-button.png)
1. Gizli için bir açıklama sağlayın ve istenen son kullanma aralığını seçin ve sonra **Ekle'yi**seçin.

    ![İstemci gizli sayfa ekleme](./media/authenticate-application/add-client-secret-page.png)
1. Yeni sırrın değerini hemen güvenli bir konuma kopyalayın. Dolgu değeri size yalnızca bir kez görüntülenir.

    ![Gizli anahtar](./media/authenticate-application/client-secret.png)

### <a name="permissions-for-the-service-bus-api"></a>Servis Veri Servisi API için izinler
Uygulamanız bir konsol uygulamasıysa, yerel bir uygulama kaydetmeniz ve **microsoft.servicebus** için API izinleri **eklemeniz** gerekir. Yerel uygulamaların, tanımlayıcı olarak hizmet veren Azure AD'de **yeniden yönlendirme-URI'ye** de ihtiyacı vardır; URI'nin bir ağ hedefi olması gerekmez. Örnek `https://servicebus.microsoft.com` kod zaten uri kullanır, çünkü bu örnek için kullanın.

### <a name="client-libraries-for-token-acquisition"></a>Belirteç edinimi için istemci kitaplıkları  
Başvurunuzu kaydettikten ve Azure Hizmet Veri Tos'unda veri gönderme/alma izni verdikten sonra, bir güvenlik ilkesinin kimliğini doğrulamak ve OAuth 2.0 jetonunu satın almak için başvurunuza kod ekleyebilirsiniz. Belirteci doğrulamak ve elde etmek için, Microsoft [kimlik doğrulama kitaplıklarından](../active-directory/develop/reference-v2-libraries.md) birini veya OpenID veya Connect 1.0'ı destekleyen başka bir açık kaynak kitaplığını kullanabilirsiniz. Uygulamanız daha sonra Azure Hizmet Veri Servisi'ne karşı bir isteği yetkilendirmek için erişim jetonunu kullanabilir.

Belirteçleri satın almanın desteklendiği senaryoların listesi için ,NET GitHub deposu [için Microsoft Kimlik Doğrulama Kitaplığı'nın (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) [Senaryolar](https://aka.ms/msal-net-scenarios) bölümüne bakın.

## <a name="sample-on-github"></a>GitHub'da örnek
GitHub'da aşağıdaki örneğe bakın: [Hizmet Veri Servisi için rol temel erişim denetimi.](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl) 

**İnteraktif Kullanıcı Girişi** seçeneğini değil, **İstemci Gizli Giriş** seçeneğini kullanın. İstemci gizli seçeneğini kullandığınızda, açılır pencere görmezsiniz. Uygulama, kimlik doğrulaması için kiracı kimliğini ve uygulama kimliğini kullanır. 

### <a name="run-the-sample"></a>Örneği çalıştırma

Örneği çalıştırmadan önce **app.config** dosyasını edin ve senaryonuza bağlı olarak aşağıdaki değerleri ayarlayın:

- `tenantId`: **TenantId** değerine ayarlayın.
- `clientId`: **ApplicationId** değerine ayarlayın.
- `clientSecret`: İstemci sırrını kullanarak oturum kurmak istiyorsanız, bunu Azure AD'de oluşturun. Ayrıca, yerel bir uygulama yerine bir web uygulaması veya API kullanın. Ayrıca, uygulamayı daha önce oluşturduğunuz ad alanına **Access Control (IAM)** altına ekleyin.
- `serviceBusNamespaceFQDN`: Yeni oluşturduğunuz Hizmet Veri Günü ad alanınızın tam DNS adına ayarlayın; örneğin, `example.servicebus.windows.net`.
- `queueName`: Oluşturduğunuz sıranın adını ayarlayın.
- Önceki adımlarda uygulamanızda belirttiğiniz URI yönlendirme.

Konsol uygulamasını çalıştırdığınızda, bir senaryo seçmeniz istenir. Numarasını yazarak ve ENTER tuşuna basarak **Etkileşimli Kullanıcı Girişi'ni** seçin. Uygulama bir giriş penceresi görüntüler, Hizmet Veri Yolu'na erişmek için izninizi ister ve ardından giriş kimliğini kullanarak gönderme/alma senaryosunu çalıştırmak için hizmeti kullanır.


## <a name="next-steps"></a>Sonraki adımlar
- RBAC hakkında daha fazla bilgi edinmek için [rol tabanlı erişim denetimi (RBAC) nedir?](../role-based-access-control/overview.md)
- Azure PowerShell, Azure CLI veya REST API ile RBAC rol atamalarının nasıl atayılmayı ve yöneteceğimizi öğrenmek için aşağıdaki makalelere bakın:
    - [Azure PowerShell ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-powershell.md)  
    - [Azure CLI ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-cli.md)
    - [REST API ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-rest.md)
    - [Azure Kaynak Yöneticisi Şablonları ile rol tabanlı erişim denetimini (RBAC) yönetme](../role-based-access-control/role-assignments-template.md)

Service Bus mesajlaşma hizmeti hakkında daha fazla bilgi edinmek için aşağıdaki konu başlıklarına bakın.

- [Servis Veri Otobüsü RBAC örnekleri](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/RoleBasedAccessControl)
- [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
- [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
- [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
