---
title: Azure Veri Gezgini'nde Azure AD uygulaması oluşturma
description: Azure Veri Gezgini'nde bir Azure REKLAM uygulamasını nasıl oluşturabilirsiniz öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: herauch
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/01/2020
ms.openlocfilehash: 64e8637630675120fece1bbe1fa4a57d97f36eb5
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80550519"
---
# <a name="create-an-azure-active-directory-application-registration-in-azure-data-explorer"></a>Azure Veri Gezgini'nde Azure Etkin Dizin uygulama kaydı oluşturma

Azure Etkin Dizin (Azure AD) uygulama kimlik doğrulaması, katılımsız bir hizmet veya kullanıcı olmadan Azure Veri Gezgini'ne erişmesi gereken zamanlanmış akış gibi uygulamalar için kullanılır. Web uygulaması gibi bir uygulamayı kullanarak bir Azure Veri Gezgini veritabanına bağlanıyorsanız, hizmet temel kimlik doğrulamasını kullanarak kimlik doğrulaması yapmalısınız. Bu makalede, bir Azure AD hizmet sorumlusunun nasıl oluşturulup kaydedilen ve sonra bir Azure Veri Gezgini veritabanına erişmek için yetki vermek üzere nasıl yetki verilebilen ayrıntılı olarak açıklanmaktadır.

## <a name="create-azure-ad-application-registration"></a>Azure AD uygulama kaydı oluşturma

Azure AD uygulama kimlik doğrulaması, bir uygulamanın Oluşturulmasını ve Azure AD ile kaydedilmesini gerektirir. Uygulama kaydı bir Azure AD kiracısında oluşturulduğunda bir hizmet sorumlusu otomatik olarak oluşturulur. 

1. [Azure portalına](https://portal.azure.com) giriş yapın `Azure Active Directory` ve bıçağı açın

    ![Portal menüsünden Azure Active Directory'yi seçin](media/provision-azure-ad-app/create-app-reg-select-azure-active-directory.png)

1. Uygulama **kayıtları** bıçak seçin ve **Yeni kayıt** seçin

    ![Yeni bir uygulama kaydı başlatma](media/provision-azure-ad-app/create-app-reg-new-registration.png)

1. Aşağıdaki bilgileri doldurun: 

    * **Adı** 
    * **Desteklenen hesap türleri**
    * **URI Web'i** > **Web** Yönlendirme
        > [!IMPORTANT] 
        > Uygulama türü **Web**olmalıdır. URI isteğe bağlıdır ve bu durumda boş bırakılır.
    * **Kaydol'u** Seçin

    ![Yeni uygulama kaydını kaydedin](media/provision-azure-ad-app/create-app-reg-register-app.png)

1. Genel **Bakış** bıçağını seçin ve **Uygulama Kimliğini**kopyalayın.

    > [!NOTE]
    > Veritabanına erişmek için servis müdürünü yetkilendirmek için uygulama kimliğine ihtiyacınız vardır.

    ![Uygulama kayıt kimliğini kopyalama](media/provision-azure-ad-app/create-app-reg-copy-applicationid.png)

1. **Sertifikalar & sırları** bıçak, **Yeni istemci gizli** seçin

    ![İstemci sırrıoluşturmabaşlatın](media/provision-azure-ad-app/create-app-reg-new-client-secret.png)

    > [!TIP]
    > Bu makalede, uygulamanın kimlik bilgileri için bir istemci gizli kullanarak açıklanır.  Uygulamanızın kimliğini doğrulamak için x509 sertifikası da kullanabilirsiniz. **Sertifikayükle'yi** seçin ve sertifikanın ortak bölümünü yüklemek için yönergeleri izleyin.

1. Açıklama, son kullanma tarihi girin ve **Ekle'yi** seçin

    ![İstemci gizli parametrelerini girin](media/provision-azure-ad-app/create-app-reg-enter-client-secret-details.png)

1. Anahtar değerini kopyalayın.

    > [!NOTE]
    > Bu sayfadan çıktığınızda, anahtar değerine erişilemez.  İstemci kimlik bilgilerini veritabanına yapılandırmak için anahtara ihtiyacınız vardır.

    ![İstemci gizli anahtar değerini kopyalama](media/provision-azure-ad-app/create-app-reg-copy-client-secret.png)

Uygulamanız oluşturulur. Aşağıdaki programlı örnekte olduğu gibi yalnızca yetkili bir Azure Veri Gezgini kaynağına erişmeniz gerekiyorsa, bir sonraki bölümü atlayın. Devredilen izinler desteği [için, uygulama kaydı için yapılandırılan izinleri yapılandır'a](#configure-delegated-permissions-for-the-application-registration)bakın.

## <a name="configure-delegated-permissions-for-the-application-registration"></a>Uygulama kaydı için devredilen izinleri yapılandırma

Uygulamanızın arama kullanıcının kimlik bilgilerini kullanarak Azure Veri Gezgini'ne erişmesi gerekiyorsa, uygulama kaydınız için temsilcileştirilmiş izinleri yapılandırın. Örneğin, Azure Veri Gezgini'ne erişmek için bir web API'sı oluşturuyorsanız ve API'nizi *arayan* kullanıcının kimlik bilgilerini kullanarak kimlik doğrulaması yapmak istiyorsanız.  

1. **API izinleri** bıçak, **bir izin ekle'yi**seçin.
1. **Kuruluşumun kullandığı API'leri**seçin. Azure Veri **Gezgini'ni**arayın ve seçin.

    ![Azure Veri Gezgini API izni ekleme](media/provision-azure-ad-app/configure-delegated-add-api-permission.png)

1. **Temsilcilikli izinlerde,** **user_impersonation** kutusunu seçin ve **İzin Ekle**

    ![Kullanıcı kimliğe bürünme ile devredilen izinleri seçme](media/provision-azure-ad-app/configure-delegated-click-add-permissions.png)     

## <a name="grant-the-service-principal-access-to-an-azure-data-explorer-database"></a>Azure Veri Gezgini veritabanına hizmet temel erişimi verme

Hizmet temel uygulama kaydınız oluşturulduğuna göre, Azure Veri Gezgini veritabanınıza ilgili hizmet temel erişimini vermeniz gerekir. 

1. Web [UI'da](https://dataexplorer.azure.com/)veritabanınıza bağlanın ve bir sorgu sekmesi açın.

1. Aşağıdaki komutu yürütün:

    ```kusto
    .add database <DatabaseName> viewers ('<ApplicationId>') '<Notes>'
    ```

    Örnek:
    
    ```kusto
    .add database Logs viewers ('aadapp=f778b387-ba15-437f-a69e-ed9c9225278b') 'Azure Data Explorer App Registration'
    ```

    Son parametre, veritabanıyla ilişkili rolleri sorgularken not olarak görünen bir dizedir.
    
    > [!NOTE]
    > Uygulama kaydını oluşturduktan sonra, Azure Veri Gezgini'nin başvuruda bulunabildiği birkaç dakika gecikmeolabilir. Komutu çalıştırırken, uygulamanın bulunamaması yla ilgili bir hata alırsanız, bekleyin ve yeniden deneyin.

Daha fazla bilgi için [güvenlik rolleri yönetimi](/azure/kusto/management/security-roles) ve alma [izinlerine](/azure/kusto/api/netfx/kusto-ingest-client-permissions.md)bakın.  

## <a name="using-application-credentials-to-access-a-database"></a>Veritabanına erişmek için uygulama kimlik bilgilerini kullanma

[Azure Veri Gezgini istemci kitaplığını](/azure/kusto/api/netfx/about-kusto-data.md)kullanarak veritabanınıza programlı olarak erişmek için uygulama kimlik bilgilerini kullanın.

```C#
. . .
string applicationClientId = "<myClientID>";
string applicationKey = "<myApplicationKey>";
. . .
var kcsb = new KustoConnectionStringBuilder($"https://{clusterName}.kusto.windows.net/{databaseName}")
    .WithAadApplicationKeyAuthentication(
        applicationClientId,
        applicationKey,
        authority);
var client = KustoClientFactory.CreateCslQueryProvider(kcsb);
var queryResult = client.ExecuteQuery($"{query}");
```

   > [!NOTE]
   > Daha önce oluşturulan başvuru kimliğini ve başvuru kaydının anahtarını (hizmet sorumlusu) belirtin.

Daha fazla bilgi için Azure [Veri Gezgini erişimi için Azure AD ile kimlik doğrulaması'na](/azure/kusto/management/access-control/how-to-authenticate-with-aad) bakın ve [.NET Core web uygulamasıyla Azure Key Vault'u kullanın.](/azure/key-vault/tutorial-net-create-vault-azure-web-app#create-a-net-core-web-app)

## <a name="troubleshooting"></a>Sorun giderme

### <a name="invalid-resource-error"></a>Geçersiz kaynak hatası

Uygulamanız Azure Veri Gezgini erişimi için kullanıcıların veya uygulamaların kimliğini doğrulamak için kullanılıyorsa, Azure Veri Gezgini hizmet uygulaması için temsilci izinleri ayarlamanız gerekir. Uygulamanızın Azure Veri Gezgini erişimi için kullanıcıların veya uygulamaların kimliğini doğrulayabildiği bildirin. Bunu yapmamak, kimlik doğrulama denemesi yapıldığında aşağıdakilere benzer bir hataya neden olur:

`AADSTS650057: Invalid resource. The client has requested access to a resource which is not listed in the requested permissions in the client's application registration...`

[Azure Veri Gezgini hizmeti uygulaması için temsilci izinleri ayarlama](#configure-delegated-permissions-for-the-application-registration)yönergelerini izlemeniz gerekir.

### <a name="enable-user-consent-error"></a>Kullanıcı onayı hatasını etkinleştirme

Azure AD kiracı yöneticiniz, kiracı kullanıcıların uygulamalara izin vermesini engelleyen bir ilke yürürlüğe koyabilir. Bu durum, bir kullanıcı uygulamanızda oturum açmaya çalıştığında aşağıdakilere benzer bir hataya neden olur:

`AADSTS65001: The user or administrator has not consented to use the application with ID '<App ID>' named 'App Name'`

Kiracıdaki tüm kullanıcılar için onay vermek veya özel uygulamanız için kullanıcı onayı almak için Azure REKLAM yöneticinize başvurmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Desteklenen bağlantı dizeleri listesi için [Kusto bağlantı dizelerine](/azure/kusto/api/connection-strings/kusto.md) bakın.
