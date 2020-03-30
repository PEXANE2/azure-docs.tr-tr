---
title: Microsoft kimlik platformu Java web uygulaması quickstart | Azure
description: OpenID Connect'i kullanarak Java Web Uygulamasında Microsoft Oturum Açma'yı nasıl uygulayacağınızı öğrenin
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/09/2019
ms.author: sagonzal
ms.custom: aaddev, scenarios:getting-started, languages:Java
ms.openlocfilehash: c2f3272beb463a16f9488139b6c3a45febae6493
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79529643"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>Quickstart: Java web uygulamasına Microsoft ile oturum açma ekleme

Bu hızlı başlangıçta, bir Java web uygulamasını Microsoft kimlik platformuyla nasıl tümleştireceğinizi öğreneceksiniz. Uygulamanız bir kullanıcıda oturum açacak, Microsoft Graph API'yi aramak için bir erişim jetonu alacak ve Microsoft Graph API'ye istekte bulunacaktır.

Bu hızlı başlangıcı tamamladığınızda, uygulamanız Azure Active Directory kullanan herhangi bir şirket veya kuruluşun kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) oturum açmalarını ve iş veya okul hesaplarını kabul eder. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için şunları yapmanız gerekir:

- [Java Geliştirme Kiti (JDK)](https://openjdk.java.net/) 8 veya daha büyük ve [Maven](https://maven.apache.org/).

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: express (Seçenek 1) veya manuel (Seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Azure portalına gidin [- Uygulama kayıtları.](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yeni uygulamanızı indirmek ve otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> Başvurunuzu kaydetmek ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için aşağıdaki adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
>
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](/azure/active-directory/develop/) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
>    - **Ad** bölümüne, örneğin `java-webapp`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>    - **Şimdilik URI'yi boş** bırak ve **Kaydol'u**seçin.
> 1. Genel **Bakış** sayfasında, **uygulamanın Uygulama (istemci) kimliğini** ve **Dizin (kiracı) kimlik** değerlerini bulun. Bu değerleri daha sonra kopyalayın.
> 1. Menüden **Kimlik Doğrulama'yı** seçin ve ardından aşağıdaki bilgileri ekleyin:
>    - **Redirect URI'** `https://localhost:8080/msal4jsample/secure/aad` de, ekle ve `https://localhost:8080/msal4jsample/graph/me`.
>    - **Kaydet'i**seçin.
> 1. Menüden **Sertifikalar & sırları** seçin ve Müşteri **sırları** bölümünde, Yeni **müşteri sırrı**tıklayın:
>
>    - Anahtar açıklaması yazın (örneğin uygulama sırrı).
>    - 1 yıl **içinde**önemli bir süre seçin.
>    - **Ekle'yi**seçtiğinizde anahtar değeri görüntülenir.
>    - Anahtarın değerini daha sonra kopyalayın. Bu anahtar değer yeniden görüntülenmez veya başka yollarla alınamaz, bu nedenle Azure portalından görünür görünmez kaydedin.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Adım 1: Uygulamanızı Azure portalında yapılandırın
>
> Bu hızlı başlatmanın işe yaraması için kod örneğinin çalışması için şunları yapmanız gerekir:
>
> 1. Yanıt URL'leri `https://localhost:8080/msal4jsample/secure/aad` ve `https://localhost:8080/msal4jsample/graph/me`.
> 1. Bir Müşteri Sırrı oluşturun.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-the-code-sample"></a>Adım 2: Kod örneğini indirin
> [!div renderon="docs"]
> [Kod Örneğini İndir](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Projeyi karşıdan yükleyin ve zip dosyasını kök klasörüne daha yakın yerel bir klasöre ayıklayın - örneğin, **C:\Azure-Örnekler**
> 
> https'yi localhost ile kullanmak için server.ssl.key özelliklerini doldurun. Kendi imzalı bir sertifika oluşturmak için anahtar aracı yardımcı programını (JRE'ye dahil) kullanın.
>
>  ```
>   Example:
>   keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password
>
>   server.ssl.key-store-type=PKCS12  
>   server.ssl.key-store=classpath:keystore.p12  
>   server.ssl.key-store-password=password  
>   server.ssl.key-alias=testCert
>   ```
>   Oluşturulan keystore dosyasını "kaynaklar" klasörüne koyun.
   
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-code-sample"></a>Adım 3: Kod örneğini yapılandırma
> 1. Zip dosyasını yerel bir klasöre çıkarın.
> 1. Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği favori IDE'nizde açın (isteğe bağlı).
> 1. Src/main/resources/klasöründe bulunabilen application.properties dosyasını açın ve *aad.clientId,* *aad.authority* ve *aad.secretKey* alanlarının değerini **Application Id**, Kiracı **Kimliği** ve **Müşteri Sırrı** ile aşağıdaki gibi değiştirin:
>
>    ```file
>    aad.clientId=Enter_the_Application_Id_here
>    aad.authority=https://login.microsoftonline.com/Enter_the_Tenant_Info_Here/
>    aad.secretKey=Enter_the_Client_Secret_Here
>    aad.redirectUriSignin=https://localhost:8080/msal4jsample/secure/aad
>    aad.redirectUriGraph=https://localhost:8080/msal4jsample/graph/me
>    aad.msGraphEndpointHost="https://graph.microsoft.com/"
>    ```
> Konumlar:
>
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
> - `Enter_the_Client_Secret_Here`- Kayıtlı olduğunuz uygulama için **Sertifikalar & Sırlar'da** oluşturduğunuz **Müşteri Sırrıdır.**
> - `Enter_the_Tenant_Info_Here`- Kaydettiğiniz uygulamanın **Dizin (kiracı) kimlik** değeridir.
> 1. https'yi localhost ile kullanmak için server.ssl.key özelliklerini doldurun. Kendi imzalı bir sertifika oluşturmak için anahtar aracı yardımcı programını (JRE'ye dahil) kullanın.
>
>  ```
>   Example:
>   keytool -genkeypair -alias testCert -keyalg RSA -storetype PKCS12 -keystore keystore.p12 -storepass password
>
>   server.ssl.key-store-type=PKCS12  
>   server.ssl.key-store=classpath:keystore.p12  
>   server.ssl.key-store-password=password  
>   server.ssl.key-alias=testCert
>   ```
>   Oluşturulan keystore dosyasını "kaynaklar" klasörüne koyun.


> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-run-the-code-sample"></a>Adım 3: Kod örneğini çalıştırma
> [!div renderon="docs"]
> #### <a name="step-4-run-the-code-sample"></a>Adım 4: Kod örneğini çalıştırma

Projeyi çalıştırmak için şunları yapabilirsiniz:

Gömülü yay önyükleme sunucusunu kullanarak doğrudan IDE'nizden çalıştırın veya [maven](https://maven.apache.org/plugins/maven-war-plugin/usage.html) kullanarak war dosyasına paketleyin ve [Apache Tomcat](http://tomcat.apache.org/)gibi bir J2EE kapsayıcı çözümüne dağıtın.

##### <a name="running-from-ide"></a>IDE'den çalıştırma

Web uygulamasını bir IDE'den çalıştırıyorsanız, çalıştır'ı tıklatın ve projenin ana sayfasına gidin. Bu örnek için, standart ana https://localhost:8080sayfa URL'si.

1. Ön sayfada, Azure Etkin Dizin'e yönlendirmek ve kullanıcıdan kimlik bilgilerini almak için **Giriş** düğmesini seçin.

1. Kullanıcının kimliği doğrulandıktan sonra, bu kullanıcı *https://localhost:8080/msal4jsample/secure/aad*' ya yönlendirilir. Artık oturum açMaktadırlar ve sayfada oturum açmış hesap la ilgili bilgiler gösterilecek. Örnek UI aşağıdaki düğmelere sahiptir:
    - *Oturum Aç*: Geçerli kullanıcıyı uygulamadan çıkarır ve ana sayfaya yönlendirir.
    - *Kullanıcı Bilgilerini Göster*: Microsoft Graph için bir belirteç edinir ve oturum açan kullanıcı hakkında temel bilgileri döndüren belirteci içeren bir istekle Microsoft Graph'ı çağırır.


   
> [!IMPORTANT]
> Bu hızlı başlatma uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci sırrı kullanır. İstemci sırrı proje dosyalarınıza düz metin olarak eklenmiştir, çünkü güvenlik nedenleriyle uygulamayı üretim uygulaması olarak düşünmeden önce istemci sırrı yerine bir sertifika kullanmanız önerilir. Sertifikanın nasıl kullanılacağı hakkında daha fazla bilgi [için, uygulama kimlik doğrulaması için Sertifika kimlik bilgilerine](https://docs.microsoft.com/azure/active-directory/develop/active-directory-certificate-credentials)bakın.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-java-webapp/java-quickstart.svg)

### <a name="getting-msal"></a>MSAL Alma

Java için MSAL (MSAL4J), kullanıcıları oturum ve microsoft kimlik Platformu tarafından korunan bir API'ye erişmek için kullanılan istek belirteçleri için kullanılan Java kitaplığıdır.

Uygulamanın pom.xml (Maven) veya build.gradle (Gradle) dosyasında aşağıdaki değişiklikleri yaparak bağımlılıklarınızı yönetmek için Maven veya Gradle'yi kullanarak uygulamanız için MSAL4J ekleyin.

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>1.0.0</version>
</dependency>
```

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '1.0.0'
```

### <a name="msal-initialization"></a>MSAL başlatma

MSAL4J'i kullanacakdosyanın üst bölümüne aşağıdaki kodu ekleyerek Java için MSAL'a bir başvuru ekleyin:

```Java
import com.microsoft.aad.msal4j.*;
```

## <a name="next-steps"></a>Sonraki Adımlar

İzinler ve izinler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [İzinler ve İzinler](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent)

Bu senaryonun auth akışı hakkında daha fazla bilgi için Oauth 2.0 yetkilendirme kodu akışına bakın:

> [!div class="nextstepaction"]
> [Yetki Kodu Oauth akışı](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-auth-code-flow)

Microsoft kimlik platformlarını geliştirmemize yardımcı olun. Kısa bir iki soruluk anketi tamamlayarak bize ne düşündüğünüzü söyleyin.

> [!div class="nextstepaction"]
> [Microsoft kimlik platformu anketi](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
