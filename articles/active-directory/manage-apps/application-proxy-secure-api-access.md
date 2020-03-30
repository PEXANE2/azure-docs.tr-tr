---
title: Azure AD Application Proxy ile şirket içi API'lere erişin
description: Azure Active Directory'nin Uygulama Proxy'si, yerel uygulamaların şirket içinde veya bulut VM'lerde barındırdığınız API'lere ve iş mantığına güvenli bir şekilde erişmesine olanak tanır.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: mimart
ms.reviewer: japere
ms.openlocfilehash: ecd5d8bae22d67f8d9f5b99d5c94eecf54a4a1f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77166001"
---
# <a name="secure-access-to-on-premises-apis-with-azure-ad-application-proxy"></a>Azure AD Application Proxy ile şirket içi API'lere güvenli erişim

Şirket içinde çalışan veya buluttaki sanal makinelerde barındırılan iş mantığı API'leri olabilir. Verileri kullanmak veya kullanıcı etkileşimi sağlamak için ana bilgisayardaki Android, iOS, Mac veya Windows uygulamalarınızın API uç noktalarıyla etkileşimde olması gerekir. Azure AD Application Proxy ve [Azure Etkin Dizin Kimlik Doğrulama Kitaplıkları (ADAL),](/azure/active-directory/develop/active-directory-authentication-libraries) yerel uygulamalarınızın şirket içi API'lerinize güvenli bir şekilde erişmesini sağlar. Azure Active Directory Application Proxy, güvenlik duvarı bağlantı noktalarını açmaktan ve uygulama katmanında kimlik doğrulama ve yetkilendirmeyi denetlemekten daha hızlı ve daha güvenli bir çözümdür. 

Bu makale, yerel uygulamaların erişebileceği bir web API hizmetini barındırmak için bir Azure AD Application Proxy çözümü kurmanıza yardımcı olabilir. 

## <a name="overview"></a>Genel Bakış

Aşağıdaki diyagram, şirket içi API'leri yayımlamanın geleneksel bir yolunu gösterir. Bu yaklaşım, gelen bağlantı noktaları 80 ve 443 açılmasını gerektirir.

![Geleneksel API erişimi](./media/application-proxy-secure-api-access/overview-publish-api-open-ports.png)

Aşağıdaki diyagram, gelen bağlantı noktalarını açmadan API'leri güvenli bir şekilde yayımlamak için Azure AD Application Proxy'yi nasıl kullanabileceğinizi gösterir:

![Azure AD Application Proxy API erişimi](./media/application-proxy-secure-api-access/overview-publish-api-app-proxy.png)

Azure AD Uygulama Proxy'si çözümün belkemiğini oluşturur, API erişimi için genel bir bitiş noktası olarak çalışır ve kimlik doğrulama ve yetkilendirme sağlar. [ADAL](/azure/active-directory/develop/active-directory-authentication-libraries) kitaplıklarını kullanarak API'lerinize çok çeşitli platformlardan erişebilirsiniz. 

Azure AD Uygulama Proxy kimlik doğrulaması ve yetkilendirmesi Azure AD'nin üzerine kurulduğundan, Yalnızca güvenilen aygıtların Application Proxy aracılığıyla yayınlanan API'lere erişebilmesini sağlamak için Azure AD Koşullu Erişimi kullanabilirsiniz. Masaüstü bilgisayarlar için Azure AD Join veya Azure AD Karma Birleştirme'yi ve aygıtlar için Intune Yönetilen'i kullanın. Ayrıca Azure Çok Faktörlü Kimlik Doğrulama gibi Azure Active Directory Premium özelliklerinden ve [Azure Kimlik Koruması'nın](/azure/active-directory/active-directory-identityprotection)makine öğrenimi destekli güvenliğinden de yararlanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu izbiyi takip etmek için şunları yapmanız gerekir:

- Uygulamalar oluşturabilen ve kaydedebilen bir hesapla azure dizinine yönetici erişimi
- Örnek web API ve yerel istemci uygulamaları[https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp](https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp) 

## <a name="publish-the-api-through-application-proxy"></a>UYGULAMA Proxy'si aracılığıyla API'yi yayımla

Application Proxy aracılığıyla intranetinizin dışında bir API yayınlamak için, web uygulamalarını yayımlamak için aynı deseni izlersiniz. Daha fazla bilgi için [Bkz. Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleyin.](application-proxy-add-on-premises-application.md)

Application Proxy aracılığıyla SecretAPI web API yayınlamak için:

1. Örnek SecretAPI projesini yerel bilgisayarınızda veya intranetinizde ASP.NET bir web uygulaması olarak oluşturun ve yayınlayın. Web uygulamasına yerel olarak erişebildiğinizden emin olun. 
   
1. Azure [portalında](https://portal.azure.com) **Azure Etkin Dizin'i**seçin. Ardından **Kurumsal uygulamaları**seçin.
   
1. Kurumsal uygulamaların üst kısmında **- Tüm uygulamalar** sayfasında, Yeni **uygulama**seçin.
   
1. **Uygulama** ekle **sayfasında, şirket içi uygulamaları**seçin. **Kendi şirket içi uygulama sayfanızı ekle** görüntülenir.
   
1. Yüklü bir Uygulama Proxy Bağlayıcısı yoksa, yüklemeniz istenir. Konektörü indirmek ve yüklemek için **Uygulama Proxy Bağlayıcısını İndir'i** seçin. 
   
1. Uygulama Proxy Bağlayıcısını yükledikten sonra kendi **şirket içi uygulama sayfanıza ekle:**
   
   1. **Adın**yanında *SecretAPI*girin.
      
   1. **Dahili Url'nin**yanında, api'ye intranetinizin içinden erişmek için kullandığınız URL'yi girin.
      
   1. Ön **Kimlik Doğrulama'nın** **Azure Etkin Dizini**olarak ayarlandıklarına emin olun. 
      
   1. Sayfanın üst kısmında **Ekle'yi** seçin ve uygulamanın oluşturulmasını bekleyin.
   
   ![API uygulaması ekleme](./media/application-proxy-secure-api-access/3-add-api-app.png)
   
1. Kurumsal **uygulamalarda - Tüm uygulamalar** sayfasında **SecretAPI** uygulamasını seçin. 
   
1. **SecretAPI - Genel Bakış** sayfasında, sol gezintideki **Özellikler'i** seçin.
   
1. API'lerin **MyApps** panelinde son kullanıcılar tarafından kullanılabileceğini istemediğiniz için, **Kullanıcılar** tarafından Özellikler sayfasının alt kısmında **Kisiler'** i ayarlayın ve ardından **Kaydet'i**seçin. **No**
   
   ![Kullanıcılar tarafından görülemez](./media/application-proxy-secure-api-access/5-not-visible-to-users.png)
   
Web API'nizi Azure AD Application Proxy aracılığıyla yayımladınız. Şimdi, uygulamaya erişebilen kullanıcıları ekleyin. 

1. **SecretAPI - Genel Bakış** sayfasında, sol navigasyondaki **Kullanıcıları ve grupları** seçin.
   
1. Kullanıcılar **ve gruplar** sayfasında **Kullanıcı Ekle'yi**seçin.  
   
1. Atama **Ekle** **sayfasında, Kullanıcılar ve gruplar**seçin. 
   
1. Kullanıcılar **ve gruplar** sayfasında, en azından kendiniz de dahil olmak üzere uygulamaya erişebilecek kullanıcıları arayın ve seçin. Tüm kullanıcıları seçtikten sonra **Seç'i**seçin. 
   
   ![Kullanıcıyı seçme ve atama](./media/application-proxy-secure-api-access/7-select-admin-user.png)
   
1. **Atama Ekle** sayfasında, **Atama'yı**seçin. 

> [!NOTE]
> Tümleşik Windows Kimlik Doğrulaması kullanan [API'ler ek adımlar](/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd)gerektirebilir.

## <a name="register-the-native-app-and-grant-access-to-the-api"></a>Yerel uygulamayı kaydedin ve API'ye erişim izni ver

Yerel uygulamalar, belirli bir platformda veya cihazda kullanılmak üzere geliştirilmiş programlardır. Yerel uygulamanız bir API'ye bağlanıp erişemeden önce, uygulamayı Azure AD'ye kaydetmeniz gerekir. Aşağıdaki adımlar, yerel bir uygulamayı nasıl kaydedin ve Uygulama Proxy aracılığıyla yayınladığınız web API'sine nasıl erişirebildiğini gösterir.

AppProxyNativeAppSample yerel uygulamasını kaydetmek için:

1. Azure Active Directory **Overview** sayfasında, **Uygulama kayıtlarını**seçin ve **Uygulama kayıtları** bölmesinin üst kısmında Yeni **kayıt'ı**seçin.
   
1. Bir başvuru sayfasını **kaydedin:**
   
   1. **Adı**altında, *AppProxyNativeAppSample*girin. 
      
   1. **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin. 
      
   1. **Url'yi Yeniden Yönlendirme**altında, açılır ve **Ortak istemci (mobil & masaüstü)** seçin ve sonra *https girin:\//appproxynativeapp*. 
      
   1. **Kaydol'u**seçin ve uygulamanın başarıyla kaydolmasını bekleyin. 
      
      ![Yeni uygulama kaydı](./media/application-proxy-secure-api-access/8-create-reg-ga.png)
   
AppProxyNativeAppSample uygulamasını Azure Active Directory'ye kaydettiniz. Yerel uygulamanızın SecretAPI web API'sine erişmesini sağlamak için:

1. Azure Active Directory **Overview** > **App Registrations** sayfasında **AppProxyNativeAppSample** uygulamasını seçin. 
   
1. **AppProxyNativeAppSample** sayfasında, sol navigasyonda **API izinlerini** seçin. 
   
1. **API izinleri** sayfasında **izin ekle'yi**seçin.
   
1. İlk **İstek API izinleri** sayfasında, **kuruluşumun kullandığı API'leri** seçin ve ardından **SecretAPI'yi**arayın ve seçin. 
   
1. Sonraki **İstek API izinleri** sayfasında, **user_impersonation**yanındaki onay kutusunu seçin ve ardından **İzin Ekle'yi**seçin. 
   
    ![Bir API seçin](./media/application-proxy-secure-api-access/10-secretapi-added.png)
   
1. **API izinleri** sayfasında, diğer kullanıcıların uygulamayı tek tek kabul etmesini önlemek **için Contoso için Grant admin onayı** seçebilirsiniz. 

## <a name="configure-the-native-app-code"></a>Yerel uygulama kodunu yapılandırma

Son adım, yerel uygulamayı yapılandırmaktır. NativeClient örnek uygulamasındaki *Form1.cs* dosyasından aşağıdaki parçacık, ADAL kitaplığı'nın API çağrısını istemek için belirteci edinmesine ve bunu uygulama üstbilgisine taşıyıcı olarak iliştirilmesine neden olur. 
   
   ```csharp
       AuthenticationResult result = null;
       HttpClient httpClient = new HttpClient();
       authContext = new AuthenticationContext(authority);
       result = await authContext.AcquireTokenAsync(todoListResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
       
       // Append the token as bearer in the request header.
       httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
       
       // Call the API.
       HttpResponseMessage response = await httpClient.GetAsync(todoListBaseAddress + "/api/values/4");
   
       // MessageBox.Show(response.RequestMessage.ToString());
       string s = await response.Content.ReadAsStringAsync();
       MessageBox.Show(s);
   ```
   
Azure Active Directory'ye bağlanacak şekilde yerel uygulamayı yapılandırmak ve API App Proxy'yi aramak için, NativeClient örnek uygulamasının *App.config* dosyasındaki yer tutucu değerlerini Azure AD'deki değerlerle güncelleştirin: 

- Dizini **(kiracı) kimliğini** alana `<add key="ida:Tenant" value="" />` yapıştırın. Bu değeri (GUID) uygulamalarınızın genel **bakış** sayfasından bulabilir ve kopyalayabilirsiniz. 
  
- AppProxyNativeAppSample Uygulamasını **(istemci) kimliğini** alana `<add key="ida:ClientId" value="" />` yapıştırın. AppProxyNativeAppSample **Genel Bakış** sayfasından bu değeri (GUID) bulabilir ve kopyalayabilirsiniz.
  
- AppProxyNativeAppSample Alanında **URI** `<add key="ida:RedirectUri" value="" />` Yönlendirme yapıştırın. AppProxyNativeAppSample **Kimlik Doğrulama** sayfasından bu değeri (URI) bulabilir ve kopyalayabilirsiniz. 
  
- SecretAPI Uygulama **Kimliği** URI'yi `<add key="todo:TodoListResourceId" value="" />` alana yapıştırın. Bu değeri (URI) SecretAPI Expose bir **API** sayfasından bulabilir ve kopyalayabilirsiniz.
  
- SecretAPI Giriş **Sayfası** URL'sini `<add key="todo:TodoListBaseAddress" value="" />` alana yapıştırın. Bu değeri (URL) SecretAPI **Markalama** sayfasından bulabilir ve kopyalayabilirsiniz.

Parametreleri yapılandırıldıktan sonra yerel uygulamayı oluşturun ve çalıştırın. **Oturum Aç** düğmesini seçtiğinizde, uygulama oturum açmanızı sağlar ve ardından SecretAPI'ye başarıyla bağdalı olduğunu doğrulamak için bir başarı ekranı görüntüler.

![Başarılı](./media/application-proxy-secure-api-access/success.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure Active Directory'de Application Proxy aracılığıyla uzaktan erişim için şirket içi uygulama ekleme](application-proxy-add-on-premises-application.md)
- [Hızlı başlatma: Web API'lerine erişmek için istemci uygulamasını yapılandırma](../develop/quickstart-configure-app-access-web-apis.md)
- [Yerel istemci uygulamalarının proxy uygulamalarıyla etkileşime geçmesi nasıl etkinleştirilir?](application-proxy-configure-native-client-application.md)
