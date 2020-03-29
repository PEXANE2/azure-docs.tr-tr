---
title: Hizmet Veri Mes'le Azure kaynakları için yönetilen kimlikler
description: Bu makalede, Azure Hizmet Veri Hizmeti kuruluşlarıyla (kuyruklar, konular ve abonelikler) erişmek için yönetilen kimliklerin nasıl kullanılacağı açıklanmaktadır.
services: service-bus-messaging
documentationcenter: na
author: axisc
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2020
ms.author: aschhab
ms.openlocfilehash: 89de6bf80d14ec77fe6b1f98b6e1d15c6e573fbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756292"
---
# <a name="authenticate-a-managed-identity-with-azure-active-directory-to-access-azure-service-bus-resources"></a>Azure Hizmet Veri Yolu kaynaklarına erişmek için Azure Etkin Dizini ile yönetilen bir kimliğin kimliğini doğrulatın
[Azure kaynakları için yönetilen kimlikler,](../active-directory/managed-identities-azure-resources/overview.md) uygulama kodunuzu çalıştırdığınız dağıtımla ilişkili güvenli bir kimlik oluşturmanıza olanak tanıyan bir çapraz Azure özelliğidir. Daha sonra bu kimliği, uygulamanızın ihtiyaç duyduğu belirli Azure kaynaklarına erişmek için özel izinler veren erişim denetimi rolleriyle ilişkilendirebilirsiniz.

Azure platformu, yönetilen kimliklerle bu çalışma zamanı kimliğini yönetir. Erişim anahtarlarını uygulama kodunuzda veya yapılandırmanızda, kimliğin kendisi veya erişmeniz gereken kaynaklar için depolamanız ve korumanız gerekmez. Azure Kaynakları desteği için etkin yönetilen varlıklara sahip bir Sanal makinede çalışan bir Hizmet Veri Servisi istemci uygulamasının SAS kurallarını ve anahtarlarını veya başka erişim belirteçlerini işlemesi gerekmez. İstemci uygulamasının yalnızca Servis Veri Gönderi Mesajlaşma ad alanının bitiş noktası adresine ihtiyacı vardır. Uygulama bağlandığında, Servis Veri Servisi yönetilen varlığın bağlamını bu makalede daha sonra bir örnekte gösterilen bir işlemde istemciye bağlar. Yönetilen bir kimlikle ilişkilendirildikten sonra, Servis Veri Yolu istemciniz tüm yetkili işlemleri yapabilir. Yetkilendirme, yönetilen bir varlığı Niçin Hizmet Veri Günü rolleri ile ilişkilendirerek verilir. 

## <a name="overview"></a>Genel Bakış
Bir güvenlik sorumlusu (kullanıcı, grup veya uygulama) bir Hizmet Veri Servisi kuruluşuna erişmeye çalıştığında, isteğe izin verilmelidir. Azure AD ile kaynağa erişim iki adımlı bir işlemdir. 

 1. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2.0 belirteci döndürülür. Belirteç istemek için kaynak `https://servicebus.azure.net`adı.
 1. Ardından, belirteç, belirtilen kaynağa erişim yetkisi vermek üzere Servis Veri Servisi'ne yapılan isteğin bir parçası olarak geçirilir.

Kimlik doğrulama adımı, bir uygulama isteğinin çalışma zamanında bir OAuth 2.0 erişim belirteci içermesini gerektirir. Bir uygulama Azure VM, sanal makine ölçeği kümesi veya Azure İşlevi uygulaması gibi bir Azure varlığı içinde çalışıyorsa, kaynaklara erişmek için yönetilen bir kimliği kullanabilir. 

Yetkilendirme adımı, bir veya daha fazla RBAC rolünün güvenlik ilkesine atanmasını gerektirir. Azure Hizmet Veri Mes'ü, Hizmet Veri Servisi kaynakları için izin kümelerini kapsayan RBAC rolleri sağlar. Bir güvenlik ilkesine atanan roller, asıl ilkelerin sahip olacağı izinleri belirler. Azure Hizmet Veri Yolu'na RBAC rolleri atama hakkında daha fazla bilgi edinmek [için Azure Hizmet Veri Yolu için Yerleşik RBAC rolleri'ne](#built-in-rbac-roles-for-azure-service-bus)bakın. 

Hizmet Veri Tos'una istekte bulunan yerel uygulamalar ve web uygulamaları da Azure AD ile yetkilendirilebilir. Bu makalede, bir erişim jetonu istemek ve Hizmet Veri Servisi Veri Servisi kaynakları için istekleri ni yetkilendirmek için kullanmak nasıl gösterir. 


## <a name="assigning-rbac-roles-for-access-rights"></a>Erişim hakları için RBAC rolleri atama
Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Hizmet Veri Hizmetleri, Hizmet Veri Hizmeti varlıklarına erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar ve verilere erişmek için özel roller de tanımlayabilirsiniz.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna veya Hizmet Veri Servisi ad alanına kadar kapsama edilebilir. Azure AD güvenlik ilkesi, Azure kaynakları için bir kullanıcı, bir grup, bir uygulama hizmeti ilkesi veya yönetilen bir kimlik olabilir.

## <a name="built-in-rbac-roles-for-azure-service-bus"></a>Azure Hizmet Veri Servisi Veri Servisi için yerleşik RBAC rolleri
Azure Hizmet Veri Servisi için, Azure portalı ve Azure kaynak yönetimi API'si aracılığıyla ad alanlarının ve ilgili tüm kaynakların *yönetimi, rol tabanlı erişim denetimi* (RBAC) modeli kullanılarak zaten korunmaktadır. Azure, Bir Hizmet Veri Servisi ad alanına erişim yetkisi vermek için aşağıdaki yerleşik RBAC rollerini sağlar:

- [Azure Hizmet Veri Veri Stoymu Veri Sahibi](../role-based-access-control/built-in-roles.md#azure-service-bus-data-owner): Hizmet Veri İşlemi ad alanına ve varlıklarına (kuyruklar, konular, abonelikler ve filtreler) veri erişimi sağlar
- [Azure Hizmet Veri Veri Gönderen :](../role-based-access-control/built-in-roles.md#azure-service-bus-data-sender)Hizmet Veri Göndereci ad alanına ve varlıklarına gönderme erişimi sağlamak için bu rolü kullanın.
- [Azure Hizmet Veri Veri Alıcısı](../role-based-access-control/built-in-roles.md#azure-service-bus-data-receiver): Hizmet Veri Alimi ad alanına ve varlıklarına erişim sağlamak için bu rolü kullanın. 

## <a name="resource-scope"></a>Kaynak kapsamı 
Bir güvenlik ilkesine Bir RBAC rolü atamadan önce, güvenlik ilkesinin sahip olması gereken erişim kapsamını belirleyin. En iyi uygulamalar, yalnızca mümkün olan en dar kapsamı vermenin her zaman en iyisi olduğunu belirtir.

Aşağıdaki liste, en dar kapsamdan başlayarak Servis Veri Servisi kaynakları kaynaklarına erişimi kapsamdışı kullanabilirsiniz düzeyleri açıklar:

- **Sıra**, **konu**, veya **abonelik**: Rol ataması belirli Hizmet Veri Servisi varlığı için geçerlidir. Şu anda Azure portalı, abonelik düzeyinde Hizmet Veri Servisi RBAC rollerine kullanıcı/grup/yönetilen kimlik atamayı desteklemez. Aşağıda, Azure CLI komutunu kullanmanın bir örneği verilmiştir: Bir Hizmet Veri Yolu RBAC rolüne kimlik atamak için [az rol atama sı: az rol atama sı:](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create) 

    ```azurecli
    az role assignment create \
        --role $service_bus_role \
        --assignee $assignee_id \
        --scope /subscriptions/$subscription_id/resourceGroups/$resource_group/providers/Microsoft.ServiceBus/namespaces/$service_bus_namespace/topics/$service_bus_topic/subscriptions/$service_bus_subscription
    ```
- **Service Bus namespace**: Rol ataması, Hizmet Veri Tos'unun tüm topolojisini ad alanı altında ve ilişkili tüketici grubuna kapsar.
- **Kaynak grubu**: Rol ataması kaynak grubu altındaki tüm Servis Veri Mes'leri kaynakları için geçerlidir.
- **Abonelik**: Rol ataması, abonelikteki tüm kaynak gruplarından tüm Servis Veri Mes'leri kaynakları için geçerlidir.

> [!NOTE]
> RBAC rol atamalarının yayılması beş dakika kadar sürebilir. 

Yerleşik rollerin nasıl tanımlandığı hakkında daha fazla bilgi için [bkz.](../role-based-access-control/role-definitions.md#management-and-data-operations) Özel RBAC rolleri oluşturma hakkında bilgi [için](../role-based-access-control/custom-roles.md)bkz.

## <a name="enable-managed-identities-on-a-vm"></a>VM'de yönetilen kimlikleri etkinleştirme
VM'nizden Hizmet Veri Hizmeti Veri Aracı kaynaklarını yetkilendirmek için Azure Kaynakları için yönetilen kimlikleri kullanabilmeniz için önce VM'deki Azure Kaynakları için yönetilen kimlikleri etkinleştirmeniz gerekir. Azure Kaynakları için yönetilen kimlikleri nasıl etkinleştireceklerini öğrenmek için şu makalelerden birine bakın:

- [Azure portalında](../active-directory/managed-service-identity/qs-configure-portal-windows-vm.md)
- [Azure PowerShell](../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)
- [Azure CLI](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md)
- [Azure Resource Manager şablonu](../active-directory/managed-identities-azure-resources/qs-configure-template-windows-vm.md)
- [Azure Kaynak Yöneticisi istemci kitaplıkları](../active-directory/managed-identities-azure-resources/qs-configure-sdk-windows-vm.md)

## <a name="grant-permissions-to-a-managed-identity-in-azure-ad"></a>Azure AD'de yönetilen bir kimliğe izin verme
Uygulamanızdaki yönetilen bir kimlikten Servis Veri Yolu hizmetine bir istek yetkilendirmek için, önce bu yönetilen kimlik için rol tabanlı erişim denetimi (RBAC) ayarlarını yapılandırın. Azure Hizmet Veri Servisi, Hizmet Veri Servisi'nden gönderme ve okuma izinlerini kapsayan RBAC rollerini tanımlar. RBAC rolü yönetilen bir kimliğe atandığında, yönetilen kimliğe uygun kapsamda Hizmet Veri Yolu varlıklarına erişim izni verilir.

RBAC rolleri atama hakkında daha fazla bilgi için, [Hizmet Veri Servisi kaynaklarına erişmek için Azure Active Directory ile Kimlik Doğrulaması'na](authenticate-application.md#built-in-rbac-roles-for-azure-service-bus)bakın ve yetkilendirmek.

## <a name="use-service-bus-with-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimliklerle Hizmet Veri Servisi'ni kullanma
Yönetilen kimliklere sahip Hizmet Veri Yolu'nu kullanmak için, kimliği rolü ve uygun kapsamı atamanız gerekir. Bu bölümdeki yordam, yönetilen bir kimlik altında çalışan ve Hizmet Veri Yolu kaynaklarına erişen basit bir uygulama kullanır.

Burada [Azure Uygulama Hizmeti'nde](https://azure.microsoft.com/services/app-service/)barındırılan örnek bir web uygulaması kullanıyoruz. Bir web uygulaması oluşturmak için adım adım talimatlar için [Azure'da ASP.NET Core web uygulaması oluşturma](../app-service/app-service-web-get-started-dotnet.md)

Uygulama oluşturulduktan sonra aşağıdaki adımları izleyin: 

1. **Ayarlar'a** gidin ve **Kimlik'i**seçin. 
1. **Üzerinde**olmak için **Durumu** seçin. 
1. Ayarları kaydetmek için **Kaydet**’i seçin. 

    ![Bir web uygulaması için yönetilen kimlik](./media/service-bus-managed-service-identity/identity-web-app.png)

Bu ayarı etkinleştirdikten sonra, Azure Etkin Dizininizde (Azure AD) yeni bir hizmet kimliği oluşturulur ve Uygulama Hizmeti ana bilgisayarolarak yapılandırılır.

Şimdi, bu hizmet kimliğini Hizmet Veri Yolu kaynaklarınızda gerekli kapsamdaki bir role atayın.

### <a name="to-assign-rbac-roles-using-the-azure-portal"></a>Azure portalını kullanarak RBAC rollerini atamak için
Bir Hizmet Veri Servisi ad alanına rol atamak için Azure portalındaki ad alanına gidin. Kaynak için Erişim Denetimi (IAM) ayarlarını görüntüleyin ve rol atamalarını yönetmek için aşağıdaki yönergeleri izleyin:

> [!NOTE]
> Aşağıdaki adımlar, Hizmet Veri Yolu ad alanlarınıza bir hizmet kimliği rolü atar. Desteklenen diğer kapsamlarda (kaynak grubu ve abonelik) bir rol atamak için aynı adımları izleyebilirsiniz. 
> 
> Yoksa [Bir Hizmet Veri Servisi Mesajlaşma ad alanı oluşturun.](service-bus-create-namespace-portal.md) 

1. Azure portalında, Hizmet Veri Kurumu ad alanınıza gidin ve ad alanı için **Genel Bakış'ı** görüntüleyin. 
1. Hizmet Veri Aracı ad alanının erişim denetim ayarlarını görüntülemek için sol menüde **Erişim Denetimi 'ni (IAM)** seçin.
1.  Rol **atamaları** listesini görmek için Rol atamaları sekmesini seçin.
3.  Yeni bir rol eklemek için **Ekle'yi** seçin.
4.  Rol **ataması ekle** sayfasında, atamak istediğiniz Azure Hizmet Veri Yolunda'nı seçin. Ardından, rolü atamak için kaydolduğunuz hizmet kimliğini bulmak için arama yapın.
    
    ![Rol atama sayfası ekleme](./media/service-bus-managed-service-identity/add-role-assignment-page.png)
5.  **Kaydet'i**seçin. Rolü atadığınız kimlik, bu rolün altında listelenir. Örneğin, aşağıdaki resim, hizmet kimliğinin Azure Hizmet Verisi sahibine sahip olduğunu gösterir.
    
    ![Bir role atanan kimlik](./media/service-bus-managed-service-identity/role-assigned.png)

Rolü atadıktan sonra, web uygulaması tanımlanan kapsam altında Hizmet Veri Servisi varlıklarına erişebilir. 

### <a name="run-the-app"></a>Uygulamayı çalıştırma

Şimdi, oluşturduğunuz ASP.NET uygulamanın varsayılan sayfasını değiştirin. [Bu GitHub deposundan](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet)web uygulama kodunu kullanabilirsiniz.  

Default.aspx sayfası açılış sayfanızdır. Kod Default.aspx.cs dosyasında bulunabilir. Sonuç, birkaç giriş alanına sahip en az web uygulaması ve ileti göndermek veya almak için Servis Veri Servisi'ne bağlanan **gönder** ve **alma** düğmeleriyle elde edin.

[MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) nesnesinin nasıl baş harfe başlattığını unutmayın. Kod, Paylaşılan Erişim Belirteci (SAS) belirteç sağlayıcısını kullanmak yerine, `var msiTokenProvider = TokenProvider.CreateManagedIdentityTokenProvider();` çağrıyla birlikte yönetilen kimlik için bir belirteç sağlayıcısı oluşturur. Bu nedenle, korumak ve kullanmak için hiçbir sır vardır. Yönetilen kimlik bağlamının Servis Veri Yolu'na akışı ve yetkilendirme el sıkışması belirteç sağlayıcısı tarafından otomatik olarak işlenir. SAS kullanmaktan daha basit bir modeldir.

Bu değişiklikleri yaptıktan sonra uygulamayı yayımlayın ve çalıştırın. Visual Studio'da bir yayımlama profili indirip içe aktararak doğru yayımlama verilerini kolayca elde edebilirsiniz:

![Yayımlama profili alın](./media/service-bus-managed-service-identity/msi3.png)
 
İleti göndermek veya almak için ad alanının adını ve oluşturduğunuz varlığın adını girin. Ardından gönder **veya** **al'ı**tıklatın.


> [!NOTE]
> - Yönetilen kimlik yalnızca Azure ortamında, Uygulama hizmetlerinde, Azure VM'lerinde ve ölçek kümelerinde çalışır. .NET uygulamaları için, Service Bus NuGet paketi tarafından kullanılan Microsoft.Azure.Services.AppAuthentication kitaplığı, bu protokol üzerinde bir soyutlama sağlar ve yerel bir geliştirme deneyimini destekler. Bu kitaplık ayrıca Visual Studio, Azure CLI 2.0 veya Active Directory Integrated Authentication'daki kullanıcı hesabınızı kullanarak kodunuzu geliştirme makinenizde yerel olarak test etmenizi sağlar. Bu kitaplıkla yerel geliştirme seçenekleri hakkında daha fazla bilgi için [,.NET'i kullanarak Azure Key Vault'a hizmet-servis kimlik doğrulaması](../key-vault/service-to-service-authentication.md)bakın.  
> 
> - Şu anda yönetilen kimlikler App Service dağıtım yuvalarıyla çalışmıyor.

## <a name="next-steps"></a>Sonraki adımlar

Service Bus mesajlaşması hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
