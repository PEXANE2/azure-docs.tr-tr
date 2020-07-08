---
title: Etkileşimli olmayan oturum açma ile tek sayfalı bir uygulamanın güvenliğini sağlama
titleSuffix: Azure Maps
description: Etkileşimli olmayan Azure AD rol tabanlı erişim denetimi ve Azure Maps web SDK ile tek sayfalı bir uygulama yapılandırma.
author: philmea
ms.author: philmea
ms.date: 06/12/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: d33fbdf2e95faaa82c13b4fc0d52ea3f188c3f86
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84988682"
---
# <a name="how-to-secure-a-single-page-application-with-non-interactive-sign-in"></a>Etkileşimli olmayan oturum açma ile tek sayfalı bir uygulamanın güvenliğini sağlama

Aşağıdaki kılavuz, Kullanıcı Azure AD 'de oturum açarken Azure Maps uygulamalarına erişim belirteci sağlamak için Azure Active Directory (Azure AD) kullanan bir uygulamayla ilgilidir. Bu akış, yalnızca tek sayfalı Web uygulaması tarafından erişilebilmesi için güvenli hale getirilmesi gereken bir Web hizmetinin barındırılmasını gerektirir. Azure AD kimlik doğrulamasını gerçekleştirebilen birden çok uygulama vardır. Bu kılavuz, erişim belirteçleri almak için ürün, Azure Işlevinden yararlanır.

[!INCLUDE [authentication details](./includes/view-authentication-details.md)]

> [!Tip]
> Azure haritalar, Kullanıcı oturum açma/etkileşimli akışlarından erişim belirteçlerini destekleyebilir. Etkileşimli akışlar, erişim iptali ve gizli dizi yönetiminin daha kısıtlı bir kapsamını etkinleştirir.

## <a name="create-azure-function"></a>Azure İşlevi oluşturma

Azure AD 'den kimlik doğrulamasından sorumlu bir güvenli Web hizmeti uygulaması oluşturun. 

1. Azure portal bir işlev oluşturun. Daha fazla bilgi için bkz. [Azure Işlevi oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function).

2. Azure işlevindeki CORS ilkesini tek sayfalı Web uygulaması tarafından erişilebilir olacak şekilde yapılandırın. Bu, tarayıcı istemcilerinin Web uygulamanızın izin verilen kaynaklarına güvenmemesini sağlar. Bkz. [CORS Işlevselliği ekleme](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-rest-api#add-cors-functionality).

3. Azure AD 'de kimlik doğrulaması yapmak için bir hizmet sorumlusu oluşturulmasını etkinleştirmek üzere Azure işlevinde [sistem tarafından atanan bir kimlik ekleyin](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet#add-a-system-assigned-identity) .  

4. Azure haritalar hesabına sistem tarafından atanan kimlik için rol tabanlı erişim izni verin. Ayrıntılar için bkz. [rol tabanlı erişim verme](#grant-role-based-access) .

5. Desteklenen mekanizmalardan veya REST protokolden biriyle sistem tarafından atanan kimlik kullanarak Azure Maps erişim belirteçlerini almak için Azure işlevi için kod yazın. Bkz. [Azure kaynakları için belirteçleri alma](https://docs.microsoft.com/azure/app-service/overview-managed-identity?tabs=dotnet#add-a-system-assigned-identity)

    Örnek REST protokol örneği:

    ```http
    GET /MSI/token?resource=https://atlas.microsoft.com/&api-version=2019-08-01 HTTP/1.1
    Host: localhost:4141
    ```

    Örnek yanıt:

    ```http
    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "access_token": "eyJ0eXAi…",
        "expires_on": "1586984735",
        "resource": "https://atlas.microsoft.com/",
        "token_type": "Bearer",
        "client_id": "..."
    }
    ```

6. Azure işlevi HttpTrigger için güvenliği yapılandırma

   * [İşlev erişim anahtarı oluşturma](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#authorization-keys)
   * Üretimde Azure işlevi için [GÜVENLI HTTP uç noktası](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook-trigger?tabs=csharp#secure-an-http-endpoint-in-production) .
   
7. Web uygulaması Azure Maps web SDK 'sını yapılandırın. 

    ```javascript
    //URL to custom endpoint to fetch Access token
    var url = 'https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>';

    var map = new atlas.Map('myMap', {
                center: [-122.33, 47.6],
                zoom: 12,
                language: 'en-US',
                view: "Auto",
            authOptions: {
                authType: "anonymous",
                clientId: "<insert>", // azure map account client id
                getToken: function(resolve, reject, map) {
                    fetch(url).then(function(response) {
                        return response.text();
                    }).then(function(token) {
                        resolve(token);
                    });
                }
            }
        });

        // use the following events to debug, you can remove them at any time.
        map.events.add("tokenacquired", function () {
            console.log("token acquired");
        });
        map.events.add("error", function (err) {
            console.log(JSON.stringify(err.error));
        });
    ```

## <a name="grant-role-based-access"></a>Rol tabanlı erişim verme

Sistem tarafından atanan kimliği bir veya daha fazla Azure rol tanımına atayarak *rol tabanlı erişim denetimi* (RBAC) verirsiniz. Azure haritalar için kullanılabilen RBAC rol tanımlarını görüntülemek için **erişim denetimi (IAM)** sayfasına gidin. **Roller**' i seçin ve ardından *Azure Maps*ile başlayan roller için arama yapın.

1. **Azure haritalar hesabınıza**gidin. **Erişim denetimi (IAM)**  >  **rol atamasını**seçin.

    > [!div class="mx-imgBorder"]
    > ![RBAC verme](./media/how-to-manage-authentication/how-to-grant-rbac.png)

2. **Rol atamaları** sekmesinde, **rol**altında **Azure Maps veri okuyucusu** veya **Azure haritalar veri katılımcısı**gibi yerleşik bir Azure Maps rol tanımı seçin. **Erişim ata**altında **işlev uygulaması**' yi seçin. Asıl ada göre ' yi seçin. Sonra **Kaydet**'i seçin.

   * [Rol atamaları ekleme veya kaldırma](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)ile ilgili ayrıntılara bakın.

> [!WARNING]
> Azure Maps yerleşik rol tanımları birçok Azure haritalar REST API 'Lerine çok büyük bir yetkilendirme erişimi sağlar. API 'Lerin en düşük düzeyde erişimini kısıtlamak için bkz. [özel rol tanımı oluşturma ve sistem tarafından atanan kimliği](https://docs.microsoft.com/azure/role-based-access-control/custom-roles) özel rol tanımına atama. Bu, uygulamanın Azure Maps 'e erişmesi için gereken en düşük ayrıcalığa olanak sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Tek sayfalı uygulama senaryosuna daha fazla anlama:
> [!div class="nextstepaction"]
> [Tek sayfalı uygulama](https://docs.microsoft.com/azure/active-directory/develop/scenario-spa-overview)

Azure haritalar hesabınız için API kullanım ölçümlerini bulun:
> [!div class="nextstepaction"]
> [Kullanım ölçümlerini görüntüleme](how-to-view-api-usage.md)

Azure AD 'nin Azure Maps ile nasıl tümleştirileceğini gösteren diğer örnekleri göz atın:
> [!div class="nextstepaction"]
> [Azure haritalar örnekleri](https://github.com/Azure-Samples/Azure-Maps-AzureAD-Samples/tree/master/src/ClientGrant)
