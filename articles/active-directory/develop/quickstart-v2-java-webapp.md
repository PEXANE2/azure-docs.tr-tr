---
title: Microsoft Identity platform Java Web uygulaması hızlı başlangıç | Mavisi
description: OpenID Connect kullanarak Java Web uygulamasında Microsoft oturum açma 'yı nasıl uygulayacağınızı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/11/2019
ms.author: sagonzal
ms.custom: aaddev
ms.openlocfilehash: 549b4457ee38504001e83c4b831cd321e1542068
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125471"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>Hızlı Başlangıç: Microsoft 'a Java Web uygulamasına oturum açma ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, bir Java Web uygulamasını Microsoft Identity platformu ile tümleştirmeyi öğreneceksiniz. Uygulamanız bir kullanıcıya oturum açacaktır, Microsoft Graph API 'sini çağırmak için bir erişim belirteci alır ve Microsoft Graph API 'sine bir istek yapar. 

Kılavuzu tamamladığınızda, uygulamanız kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) ve Azure Active Directory kullanan herhangi bir şirketten veya kuruluştan iş veya okul hesapları için oturum açma işlemlerini kabul eder.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-java-webapp/java-quickstart.svg)

> ## <a name="prerequisites"></a>Önkoşullar
> Bu örneği çalıştırmak için şunlar gerekir: 
> - Java Development Kit (JDK) 8 veya üzeri ve Maven.
> - Azure Active Directory (Azure AD) kiracısı. Azure AD kiracısı alma hakkında daha fazla bilgi için bkz [. Azure AD kiracısı alma.](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-create-new-tenant)
>
> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için kullanabileceğiniz iki seçenek vardır:
> * Çaba [Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin.](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * El ile: [Seçenek 2: Uygulamanızı ve kod örneğinizi kaydedin ve el ile yapılandırın.](#option-2-register-and-manually-configure-your-application-and-code-sample)
> 
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure portal uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2: Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
> 
>
> #### <a name="step-1-download-the-code-sample"></a>1\. adım: Kod örneğini indirin
> 
> - [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)
>
> #### <a name="step-2-open-applicationproperties"></a>2\. adım: Uygulamayı aç. Özellikler
>
> 1. ZIP dosyasını yerel bir klasöre ayıklayın.
> 1. Seçim Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği en sevdiğiniz IDE 'niz içinde açın.
> 1. *Application. Properties* dosyasını açın. Uygulamanızı bir sonraki adımda kaydettiğinizde `aad.clientId`, `aad.authority`ve `aad.secretKey` değerlerini eklersiniz.


> #### <a name="step-3-register-your-application"></a>3\. adım: Uygulamanızı kaydedin
> Uygulamanızı kaydetmek ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>    - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `java-webapp`.
>    - **Yeniden yönlendirme URI 'sini** Şu anda boş bırakın ve **Kaydet**' i seçin.
> 1. Uygulamanın **uygulama (istemci) kimliği** değerini bulun. *Application. Properties* dosyasındaki `Enter_the_Application_Id_here` değerini güncelleştirin.
> 1. Uygulamanın **Dizin (kiracı) kimliği** değerini bulun. *Application. Properties* dosyasındaki `Enter_the_Tenant_Info_Here` değerini güncelleştirin. 
> 1. **Kimlik doğrulama** menüsünü seçin ve ardından aşağıdaki bilgileri ekleyin:
>    - **Yeniden yönlendirme URI 'lerinde**, `http://localhost:8080/msal4jsamples/secure/aad` ve `https://localhost:8080/msal4jsamples/graph/users`ekleyin.
>    - **Kaydet**’i seçin.
> 1. Sol taraftaki menüde **sertifikalar & gizlilikler** ' ı seçin ve **istemci** gizli dizileri bölümünde **yeni istemci parolası** ' na tıklayın:
>     
>    - Bir anahtar açıklaması (örnek uygulama gizli anahtarı) yazın.
>    - **1 yılda**bir anahtar süresi seçin.
>    - **Ekle**' ye tıkladığınızda, anahtar değeri görüntülenecektir. 
>    - Anahtarın değerini kopyalayın. Daha önce indirdiğiniz *Application. Properties* dosyasını açın ve değerini `Enter_the_Client_Secret_Here` anahtar değeriyle güncelleştirin. 
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>1\. adım: Uygulamanızı Azure portal yapılandırma
> Bu hızlı başlangıçta çalışması için kod örneği için şunları yapmanız gerekir:
> 1. Yanıt URL 'lerini ve `http://localhost:8080/msal4jsamples/secure/aad` `https://localhost:8080/msal4jsamples/graph/users`olarak ekleyin.
> 1. Bir Istemci gizli dizisi oluşturun.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.
> 
> #### <a name="step-2-download-the-code-sample"></a>2\. adım: Kod örneğini indirin
> 
> - [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)
> 
> #### <a name="step-3-configure-the-code-sample"></a>3\. adım: Kod örneğini yapılandırma 
> 
> 1. ZIP dosyasını yerel bir klasöre ayıklayın.
> 1. Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği en sevdiğiniz IDE (isteğe bağlı) içinde açın.
> 1. *Src/Main/Resources/* konumunda bulunan **Application. Properties** dosyasını açın.
> 1. Uygulama özelliklerini değiştirin.
>   1. ' `aad.clientId` İ kaydettiğiniz uygulamanın **uygulama (istemci) kimliği** değeri `Enter_the_Application_Id_here` ile değerini bulun ve güncelleştirin. 
>   1. ' `aad.authority` In`Enter_the_Tenant_Name_Here` değerini, kaydettiğiniz uygulamanın **Dizin (kiracı) kimliği** değeriyle bulun ve güncelleştirin.
>   1. Kayıt `aad.secretKey` yaptığınız uygulama için gizli dizileri `Enter_the_Client_Secret_Here` **& Sertifikalar** ' da oluşturduğunuz **istemci gizli anahtarı** ile değerini bulun ve güncelleştirin.

#### <a name="step-4-run-the-code-sample"></a>4\. Adım: Kod örneğini çalıştırma
1. Kod örneğini çalıştırın ve bir tarayıcı açın ve adresine *http://localhost:8080* gidin.
1. Ön sayfa bir **oturum açma** düğmesi içerir. Azure Active Directory yeniden yönlendirmek için **oturum aç** düğmesine tıklayın. Kullanıcıdan kimlik bilgileri istenir.  
1. Azure Active Directory kimlik doğrulaması başarılı olduktan sonra, öğesine *http://localhost:8080/msal4jsamples/secure/aad* yeniden yönlendirilir. Bunlar, uygulamada resmi olarak oturum açırlar ve sayfada oturum açmış olan hesabın bilgileri gösterilmelidir. Ayrıca, için düğmeler de içerir: 
    - *Oturumu Kapat*: Geçerli kullanıcının uygulamadan oturumu açacaktır ve giriş sayfasına yönlendirilir.
    - *Kullanıcıları göster*: Microsoft Graph için bir belirteç alır ve sonra Kiracıdaki tüm kullanıcıları almak için isteğe ekli belirteçle Microsoft Graph çağırır.


## <a name="more-information"></a>Daha fazla bilgi

### <a name="getting-msal"></a>MSAL alma
MSAL4J, kullanıcıların oturum açması ve Microsoft Identity platformu tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri istemek için kullanılan bir kitaplıktır. Uygulamanızda Pod. xml veya Build. Gradle dosyasında aşağıdaki değişiklikleri yaparak bağımlılıklarınızı yönetmek için Maven veya Gradle kullanarak uygulamanıza MSAL4J ekleyebilirsiniz. 

```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>0.5.0-preview</version>
</dependency>
```

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '0.5.0-preview'
```


### <a name="msal-initialization"></a>Msal başlatma
Aşağıdaki kodu MSAL4J kullanacağınız dosyanın en üstüne ekleyerek MSAL4J başvurusunu ekleyebilirsiniz: 

```
import com.microsoft.aad.msal4j.*;
```

## <a name="next-steps"></a>Sonraki Adımlar

İzinler ve onay hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [İzinler ve onay](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent)

Bu senaryoya yönelik kimlik doğrulama akışı hakkında daha fazla bilgi edinmek için bkz. OAuth 2,0 yetkilendirme kodu akışı:

> [!div class="nextstepaction"]
> [Yetkilendirme kodu OAuth akışı](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow)

Microsoft Identity platformunu geliştirmemize yardımcı olun. Kısa bir iki sorulık anketi tamamlayarak düşüncelerinizi bize söyleyin.

> [!div class="nextstepaction"]
> [Microsoft Identity platform Anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
