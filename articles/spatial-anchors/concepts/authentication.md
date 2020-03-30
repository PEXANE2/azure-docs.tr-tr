---
title: Kimlik doğrulama ve yetkilendirme
description: Bir uygulamanın veya hizmetin Azure Uzamsal Çapaları'na kimlik doğrulaması yapmanın çeşitli yolları ve Azure Uzamsal Çapaları'na erişimi nizi sağlamak için sahip olduğunuz denetim düzeyleri hakkında bilgi edinin.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 05/28/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: df27a77c202951a6c789703f12712e75bd8b5906
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657004"
---
# <a name="authentication-and-authorization-to-azure-spatial-anchors"></a>Azure Uzamsal Çapalar için kimlik doğrulama ve yetkilendirme

Bu bölümde, uygulamanızdan veya web hizmetinizden Azure Uzamsal Çapalar'a kimlik doğrulaması yapabileceğiniz çeşitli yolları ve Uzamsal Çapa hesaplarınıza erişimi denetlemek için Azure Dizininde (Azure AD) Role Tabanlı Erişim Denetimi'ni kullanma yöntemlerini ele alacağız.

## <a name="overview"></a>Genel Bakış

![Azure Uzamsal Bağlantı Larına kimlik doğrulamasına genel bakış](./media/spatial-anchors-authentication-overview.png)

Belirli bir Azure Uzamsal Bağlantı Hesabına erişmek için, müşterilerin öncelikle Azure Karma Gerçeklik Güvenlik Belirteci Hizmeti'nden (STS) bir erişim belirteci edinmeleri gerekir. STS'den elde edilen jetonlar 24 saat boyunca yaşar ve hesapta yetkilendirme kararları vermek ve bu hesaba yalnızca yetkili müdürlerin erişebilmesini sağlamak için Uzamsal Çapa hizmetleri için bilgiler içerir.

Erişim belirteçleri hesap anahtarlarından veya Azure AD tarafından verilen belirteçlerden karşılığında elde edilebilir.

Hesap anahtarları, Azure Uzamsal Çapalar hizmetini kullanmaya hızla başlamanızı sağlar; ancak, uygulamanızı üretime dağıtmadan önce, Uygulamanızı Azure AD tabanlı kimlik doğrulamasını kullanmak üzere güncelleştirmeniz önerilir.

Azure AD kimlik doğrulama belirteçleri iki şekilde elde edilebilir:

- Kurumsal bir uygulama oluşturuyorsanız ve şirketiniz Azure AD'yi kimlik sistemi olarak kullanıyorsa, uygulamanızda kullanıcı tabanlı Azure AD kimlik doğrulamasını kullanabilir ve mevcut Azure AD güvenlik gruplarınızı kullanarak uzamsal bağlantı hesaplarınıza erişim izni verebilirsiniz veya doğrudan kuruluşunuzdaki kullanıcılara.
- Aksi takdirde, uygulamanızı destekleyen bir web hizmetinden Azure AD belirteçleri almanız önerilir. Destekleyici bir web hizmeti kullanmak, istemci uygulamanızda Azure Uzamsal Bağlantı Bilgileri'ne erişmek için kimlik bilgilerini gömmeyi önleyerek üretim uygulamaları için önerilen kimlik doğrulama yöntemidir.

## <a name="account-keys"></a>Hesap anahtarları

Azure Uzamsal Çapahesabınıza erişmek için hesap anahtarlarını kullanmak, başlamak için en basit yoldur. Hesap anahtarlarınızı Azure portalında bulabilirsiniz. Hesabınıza gidin ve "Anahtarlar" sekmesini seçin.

![Azure Uzamsal Bağlantı Larına kimlik doğrulamasına genel bakış](../../../includes/media/spatial-anchors-get-started-create-resource/view-account-key.png)


Her ikisi de Uzamsal Çapalar hesabına erişim için aynı anda geçerli olan iki anahtar kullanılabilir hale getirilir. Hesaba erişmek için kullandığınız anahtarı düzenli olarak güncelleştirmeniz önerilir; iki ayrı geçerli anahtara sahip olmak, bu güncelleştirmeleri kapalı kalma süresi olmadan etkinleştirmek; yalnızca birincil anahtarı ve ikincil anahtarı alternatif olarak güncelleştirmeniz gerekir.

SDK, hesap anahtarlarıyla kimlik doğrulaması için yerleşik destek sağlar; cloudSession nesnenizde AccountKey özelliğini ayarlamanız yeterlidir.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccountKey = @"MyAccountKey";
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accountKey = @"MyAccountKey";
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accountKey = "MyAccountKey"
```

# <a name="java"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccountKey("MyAccountKey");
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccountKey(R"(MyAccountKey)");
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccountKey(LR"(MyAccountKey)");
```

---

Bu yapıldıktan sonra, SDK bir erişim belirteci için hesap anahtarının değişimini ve uygulamanız için gerekli belirteçlerin önbelleğe alınmasıişlemlerini ele alacaktır.

> [!WARNING]
> Hızlı biniş için hesap anahtarlarının kullanılması önerilir, ancak geliştirme/prototipleme sırasında yalnızca. Uygulamanızı gömülü bir hesap anahtarı kullanarak üretime göndermemeniz ve bunun yerine daha sonra listelenen kullanıcı tabanlı veya hizmet tabanlı Azure AD kimlik doğrulama yaklaşımlarını kullanmanız önerilir.

## <a name="azure-ad-user-authentication"></a>Azure AD kullanıcı kimlik doğrulaması

Azure Etkin Dizin kullanıcılarını hedefleyen uygulamalarda, önerilen yaklaşım, Kullanıcı için [MSAL kitaplığını](../../active-directory/develop/msal-overview.md)kullanarak elde edebilirsiniz bir Azure AD belirteci kullanmaktır. Bir [uygulama quickstart kayıt](../../active-directory/develop/quickstart-register-app.md)listelenen adımları izlemeniz gerekir , hangi içerir:

1. Azure portalında yapılandırma
    1.  Uygulamanızı Azure AD'de **Yerel uygulama**olarak kaydedin. Kayıt bölümünün bir parçası olarak, uygulamanızın çok kiracılı olup olmaması gerektiğini belirlemeniz ve uygulamanız için izin verilen yönlendirme URL'lerini sağlamanız gerekir.
        1.  **API izinleri** sekmesine geçiş
        2.  **İzin Ekle'yi** seçin
            1.  **Kuruluşumun kullandığı API'ler** altında **Karma Gerçeklik Kaynak Sağlayıcısı'nı** seçin
            2.  **Temsilci izinlerini** seçin
            3.  **mixedreality.signin** için kutuyu işaretleyin **mixedreality** altında
            4.  **İzin Ekle'yi** seçin
        3.  **Grant yönetici onayı** seçin
    2.  Uygulamanızı veya kullanıcılarınıza kaynağınıza erişim izni verebin:
        1.  Azure portalında Uzamsal Çapalar kaynağınıza gidin
        2.  **Access denetimine (IAM)** geçiş sekmesine geçiş
        3.  Hit **Ekle rol ataması**
            1.  [Rol seç](#role-based-access-control)
            2.  **Select** alanında, erişim atamak istediğiniz kullanıcının(lar), grup(lar) ve/veya uygulama(lar) adını girin.
            3.  **Kaydet**'e tıklayın.
2. Kodunuzda:
    1.  **Uygulama kimliğini** kullandığınızdan ve Kendi Azure AD uygulamanızın Uri'sini ADAL'daki **istemci kimliği** ve **RedirectUri** parametreleri olarak **yeniden yönlendirin**
    2.  Kiracı bilgilerini ayarlayın:
        1.  Başvurunuz **yalnızca Kuruluşumu**destekliyorsa, bu değeri **Kiracı Kimliğiniz** veya **Kiracı adınız** ile değiştirin (örneğin, contoso.microsoft.com)
        2.  Uygulamanız **hesapları herhangi bir kuruluş dizininde destekliyorsa,** bu değeri **Kuruluşlarla** değiştirin
        3.  Uygulamanız **Tüm Microsoft hesap kullanıcılarını**destekliyorsa, bu değeri **Ortak**
    3.  Belirteç isteğiniz üzerine, **kaynağı** " "https://sts.mixedreality.azure.comolarak ayarlayın. Bu "kaynak", Azure AD'ye uygulamanızın Azure Uzamsal Çapahizmeti için bir belirteç istediğini gösterir.

Bununla, uygulamanız MSAL'dan bir Azure AD belirteci alabilmeli; bu Azure AD belirtecisini bulut oturumu config nesnenizde **kimlik doğrulama Tokenolarak** ayarlayabilirsiniz.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
this.cloudSession.Configuration.AuthenticationToken = @"MyAuthenticationToken";
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.authenticationToken = @"MyAuthenticationToken";
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.authenticationToken = "MyAuthenticationToken"
```

# <a name="java"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAuthenticationToken("MyAuthenticationToken");
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AuthenticationToken(R"(MyAuthenticationToken)");
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AuthenticationToken(LR"(MyAuthenticationToken)");
```

---

## <a name="azure-ad-service-authentication"></a>Azure AD hizmeti kimlik doğrulaması

Azure Uzamsal Bağlantı larını üretime kazandıran uygulamaları dağıtmak için önerilen seçenek, kimlik doğrulama isteklerine aracılık edecek bir arka uç hizmetinden yararlanmaktır. Genel şema bu diyagramda açıklandığı gibi olmalıdır:

![Azure Uzamsal Bağlantı Larına kimlik doğrulamasına genel bakış](./media/spatial-anchors-aad-authentication.png)

Burada, uygulamanızın arka uç hizmetine kimlik doğrulamak için kendi mekanizmasını (örneğin: Microsoft hesabı, PlayFab, Facebook, Google Kimliği, özel kullanıcı adı/parola, vb.) kullandığı varsayılır. Kullanıcılarınızın arka uç hizmetinizde kimliği doğrulandıktan sonra, bu hizmet bir Azure AD belirteci alabilir, azure uzamsal bağlantı sunucuları için bir erişim belirteci yle değiştirebilir ve istemci uygulamanıza geri döndürebilir.

Azure AD erişim belirteci [MSAL kitaplığı](../../active-directory/develop/msal-overview.md)kullanılarak alınır. Bir [uygulama quickstart kayıt](../../active-directory/develop/quickstart-register-app.md)listelenen adımları izlemeniz gerekir , hangi içerir:

1.  Azure portalında yapılandırma:
    1.  Uygulamanızı Azure AD'ye kaydedin:
        1.  Azure portalında Azure **Etkin Dizin'e**gidin ve **uygulama kayıtlarını** seçin
        2.  **Yeni uygulama kaydı seçin**
        3.  Uygulamanızın adını girin, uygulama türü olarak **Web uygulamasını / API'yi** seçin ve hizmetinizin auth URL'sini girin. Sonra Oluştur tuşuna **bas.**
        4.  Bu uygulamada **Ayarlar'a**basın, ardından **Ayarlar** sekmesini seçin. Anahtarınızın adını girin, bir süre seçin ve **Kaydet'e**basın. Web hizmetinizin koduna eklemeniz gerektiğinden, o anda görüntülenen anahtar değerini kaydettiğinizden emin olun.
    2.  Uygulamanızı ve/veya kullanıcılarınıza kaynağınıza erişim izni verebin:
        1.  Azure portalında Uzamsal Çapalar kaynağınıza gidin
        2.  **Access denetimine (IAM)** geçiş sekmesine geçiş
        3.  Hit **Ekle rol ataması**
        1.  [Rol seç](#role-based-access-control)
        2.  **Select** alanında, oluşturduğunuz ve erişim atamak istediğiniz uygulamanın adını girin. Uygulamanızın kullanıcılarının Uzamsal Çapalar hesabına karşı farklı rollere sahip olmasını istiyorsanız, Azure AD'ye birden çok uygulama kaydetmeli ve her birine ayrı bir rol atamalısınız. Ardından, kullanıcılarınız için doğru rolü kullanmak için yetkilendirme mantığınızı uygulayın.
    3.  **Kaydet**'e tıklayın.
2.  Kodunuzda (not: GitHub'da yer alan hizmet örneğini kullanabilirsiniz):
    1.  Uygulama kimliğini, uygulama sırrını kullandığınızdan ve Uri'yi kendi Azure AD uygulamanızın ADAL'daki istemci kimliği, gizli ve RedirectUri parametreleri olarak yeniden yönlendirin
    2.  ADAL'daki yetki parametresinde kiracı kimliğini kendi AAAzure ADD kiracı kimliğinize ayarlama
    3.  Belirteç isteğiniz üzerine, **kaynağı** " "https://sts.mixedreality.azure.comolarak ayarlayın

Bununla birlikte, arka uç hizmetiniz bir Azure AD belirteci alabilir. Daha sonra istemciye geri dönecek bir MR belirteci için değiştirebilirsiniz. MR belirteci almak için Azure AD belirteci kullanmak, REST çağrısı yla yapılır. İşte örnek bir çağrı:

```
GET https://mrc-auth-prod.trafficmanager.net/Accounts/35d830cb-f062-4062-9792-d6316039df56/token HTTP/1.1
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni<truncated>FL8Hq5aaOqZQnJr1koaQ
Host: mrc-auth-prod.trafficmanager.net
Connection: Keep-Alive

HTTP/1.1 200 OK
Date: Sun, 24 Feb 2019 08:00:00 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 1153
Accept: application/json
MS-CV: 05JLqWeKFkWpbdY944yl7A.0
{"AccessToken":"eyJhbGciOiJSUzI1NiIsImtpZCI6IjI2MzYyMTk5ZTI2NjQxOGU4ZjE3MThlM2IyMThjZTIxIiwidHlwIjoiSldUIn0.eyJqdGkiOiJmMGFiNWIyMy0wMmUxLTQ1MTQtOWEzNC0xNzkzMTA1NTc4NzAiLCJjYWkiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJ0aWQiOiIwMDAwMDAwMC0wMDAwLTAwMDAtMDAwMC0wMDAwMDAwMDAwMDAiLCJhaWQiOiIzNWQ4MzBjYi1mMDYyLTQwNjItOTc5Mi1kNjMxNjAzOWRmNTYiLCJhYW8iOi0xLCJhcHIiOiJlYXN0dXMyIiwicmlkIjoiL3N1YnNjcmlwdGlvbnMvNzIzOTdlN2EtNzA4NC00ODJhLTg3MzktNjM5Y2RmNTMxNTI0L3Jlc291cmNlR3JvdXBzL3NhbXBsZV9yZXNvdXJjZV9ncm91cC9wcm92aWRlcnMvTWljcm9zb2Z0Lk1peGVkUmVhbGl0eS9TcGF0aWFsQW5jaG9yc0FjY291bnRzL2RlbW9fYWNjb3VudCIsIm5iZiI6MTU0NDU0NzkwMywiZXhwIjoxNTQ0NjM0MzAzLCJpYXQiOjE1NDQ1NDc5MDMsImlzcyI6Imh0dHBzOi8vbXJjLWF1dGgtcHJvZC50cmFmZmljbWFuYWdlci5uZXQvIiwiYXVkIjoiaHR0cHM6Ly9tcmMtYW5jaG9yLXByb2QudHJhZmZpY21hbmFnZXIubmV0LyJ9.BFdyCX9UJj0i4W3OudmNUiuaGgVrlPasNM-5VqXdNAExD8acFJnHdvSf6uLiVvPiQwY1atYyPbOnLYhEbIcxNX-YAfZ-xyxCKYb3g_dbxU2w8nX3zDz_X3XqLL8Uha-rkapKbnNgxq4GjM-EBMCill2Svluf9crDmO-SmJbxqIaWzLmlUufQMWg_r8JG7RLseK6ntUDRyDgkF4ex515l2RWqQx7cw874raKgUO4qlx0cpBAB8cRtGHC-3fA7rZPM7UQQpm-BC3suXqRgROTzrKqfn_g-qTW4jAKBIXYG7iDefV2rGMRgem06YH_bDnpkgUa1UgJRRTckkBuLkO2FvA"}
```

Yetkilendirme üstbilgisinin aşağıdaki gibi biçimlendiği yer:`Bearer <accoundId>:<accountKey>`

Ve yanıt düz metinde MR belirteci içerir.

Bu MR belirteci daha sonra istemciye iade edilir. İstemci uygulamanız daha sonra bulut oturumu config'inde erişim belirteci olarak ayarlayabilir.

# <a name="c"></a>[C #](#tab/csharp)

```csharp
this.cloudSession.Configuration.AccessToken = @"MyAccessToken";
```

# <a name="objc"></a>[ObjC](#tab/objc)

```objc
_cloudSession.configuration.accessToken = @"MyAccessToken";
```

# <a name="swift"></a>[Swift](#tab/swift)

```swift
_cloudSession!.configuration.accessToken = "MyAccessToken"
```

# <a name="java"></a>[Java](#tab/java)

```java
mCloudSession.getConfiguration().setAccessToken("MyAccessToken");
```

# <a name="c-ndk"></a>[C++ NDK](#tab/cpp)

```cpp
auto configuration = cloudSession_->Configuration();
configuration->AccessToken(R"(MyAccessToken)");
```

# <a name="c-winrt"></a>[C++ WinRT](#tab/cppwinrt)

```cpp
auto configuration = m_cloudSession.Configuration();
configuration.AccessToken(LR"(MyAccessToken)");
```

---

## <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

Hizmetinizin uygulamalarına, hizmetlerine veya Azure REKLAM kullanıcılarına verilen erişim düzeyini denetlemeye yardımcı olmak için, Azure Uzamsal Çapa hesaplarınıza gerektiği şekilde atamanız için aşağıdaki roller oluşturulmuştur:

- **Uzamsal Çapalar Hesap Sahibi**: bu role sahip uygulamalar veya kullanıcılar uzamsal çapalar oluşturabilir, onlar için sorgu lanabilir ve bunları silebilir. Hesap anahtarlarını kullanarak hesabınıza kimlik doğrulaması yaptığınızda, **Uzamsal Çapalar Hesap Sahibi** rolü kimlik doğrulaması yapılan anaparaya atanır.
- **Uzamsal Çapalar Hesap Katılımcısı**: bu role sahip uygulamalar veya kullanıcılar uzamsal çapalar oluşturabilir, onlar için sorgu yapamaz, ancak bunları silemez.
- **Uzamsal BağlantıLar Hesap Okuyucu**: bu role sahip uygulamalar veya kullanıcılar yalnızca uzamsal çapaları sorgulayamaz, ancak yenilerini oluşturamaz, varolanları silemez veya uzamsal çapalarda meta verileri güncelleştiremez. Bu genellikle bazı kullanıcıların ortamı nikür ettiği uygulamalar için kullanılırken, diğerleri yalnızca daha önce bu ortama yerleştirilen çapaları hatırlayabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure Uzamsal Çapalar ile ilk uygulamanızı oluşturun.

> [!div class="nextstepaction"]
> [Birlik (HoloLens)](../quickstarts/get-started-unity-hololens.md)

> [!div class="nextstepaction"]
> [Birlik (iOS)](../quickstarts/get-started-unity-ios.md)

> [!div class="nextstepaction"]
> [Birlik (Android)](../quickstarts/get-started-unity-android.md)

> [!div class="nextstepaction"]
> [iOS](../quickstarts/get-started-ios.md)

> [!div class="nextstepaction"]
> [Android](../quickstarts/get-started-android.md)

> [!div class="nextstepaction"]
> [HoloLens](../quickstarts/get-started-hololens.md)

> [!div class="nextstepaction"]
> [Xamarin (Android)](../quickstarts/get-started-xamarin-android.md)

> [!div class="nextstepaction"]
> [Xamarin (iOS)](../quickstarts/get-started-xamarin-ios.md)
