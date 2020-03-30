---
title: Azure AD'de Anahtar Rollover'ı Imzalama
description: Bu makalede, Azure Etkin Dizini için imzalama anahtarı rollover en iyi uygulamaları tartışılır
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin
ms.custom: aaddev
ms.openlocfilehash: f3585cfa7ea6f0d8afc61e899f9641d415a2e354
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77161197"
---
# <a name="signing-key-rollover-in-azure-active-directory"></a>Azure Active Directory'de anahtar devrini imzalama
Bu makalede, güvenlik belirteçlerini imzalamak için Azure Etkin Dizini'nde (Azure AD) kullanılan ortak anahtarlar hakkında bilmeniz gerekenler açıklanmaktadır. Bu tuşların periyodik olarak devredilmesi ve acil bir durumda hemen devredilmesi önemlidir. Azure AD kullanan tüm uygulamalar, anahtar devriişlemini programlı bir şekilde işleyebilir veya periyodik olarak el ile rollover işlemi oluşturabilmeli. Tuşların nasıl çalıştığını, rollover'ın uygulamanıza etkisini nasıl değerlendirip uygulamanızı nasıl güncelleyeceğinizveya gerektiğinde anahtar devriişlemini işlemek için periyodik manuel rollover işlemini nasıl oluşturlayacağınızı anlamak için okumaya devam edin.

## <a name="overview-of-signing-keys-in-azure-ad"></a>Azure AD'de imzalama tuşlarına genel bakış
Azure AD, kendisi yle kullanan uygulamalar arasında güven oluşturmak için endüstri standartları üzerine kurulu ortak anahtar şifrelemesini kullanır. Pratik açıdan bu, aşağıdaki şekilde çalışır: Azure AD, ortak ve özel anahtar çiftinden oluşan bir imzalama anahtarı kullanır. Bir kullanıcı kimlik doğrulaması için Azure AD kullanan bir uygulamayı imzaladığında, Azure AD kullanıcı hakkında bilgi içeren bir güvenlik belirteci oluşturur. Bu belirteç, uygulamaya geri gönderilmeden önce özel anahtarı kullanılarak Azure AD tarafından imzalanır. Belirteci geçerli olduğunu doğrulamak ve Azure AD kaynaklı olduğunu doğrulamak için, uygulamanın kiracının [OpenID Connect bulma belgesinde](https://openid.net/specs/openid-connect-discovery-1_0.html) veya SAML/WS-Fed [federasyon meta veri belgesinde](../azuread-dev/azure-ad-federation-metadata.md)yer alan Azure AD tarafından açığa çıkarılan ortak anahtarı kullanarak belirteci imzasını doğrulaması gerekir.

Güvenlik amacıyla, Azure AD'nin imzalama anahtarı anahtar rulolarını periyodik olarak kaydeder ve acil bir durumda hemen devredilebilir. Azure AD ile tümlebilen herhangi bir uygulama, ne sıklıkta olursa olsun önemli bir rollover olayını işlemek üzere hazır lanmalıdır. Yoksa ve uygulamanız bir belirteçteki imzayı doğrulamak için süresi dolmuş bir anahtarı kullanmaya çalışırsa, oturum açma isteği başarısız olur.

OpenID Connect bulma belgesinde ve federasyon meta veri belgesinde her zaman birden fazla geçerli anahtar vardır. Başvurunuz belgede belirtilen anahtarlardan herhangi birini kullanmaya hazır olmalıdır, çünkü bir anahtar yakında yuvarlanabilir, diğeri onun yerine olabilir ve saire.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Uygulamanızın etkilenip etkilenmeyeceğini ve bu konuda ne yapılması gerektiğini nasıl değerlendireceğiniz
Uygulamanızın anahtar devrini nasıl işlediği, uygulama türü veya kimlik protokolü ve kitaplık gibi değişkenlere bağlıdır. Aşağıdaki bölümlerde en yaygın uygulama türlerinin anahtar devrinden etkilenip etkilenmediğini değerlendirir ve otomatik rollover'ı desteklemek veya anahtarı el ile güncelleştirmek için uygulamanın nasıl güncelleştirileceklerine ilişkin kılavuzluk sağlar.

* [Kaynaklara erişen yerel istemci uygulamaları](#nativeclient)
* [Kaynaklara erişen Web uygulamaları / API'ler](#webclient)
* [Web uygulamaları / API'ler kaynakları korur ve Azure Uygulama Hizmetleri kullanılarak oluşturulmuş](#appservices)
* [.NET OWIN OpenID Connect, WS-Fed veya WindowsAzureActiveDirectoryBearerAuthentication aracını kullanarak kaynakları koruyan Web uygulamaları / API'ler](#owin)
* [.NET Core OpenID Connect veya JwtBearerAuthentication aracını kullanarak kaynakları koruyan Web uygulamaları / API'ler](#owincore)
* [Node.js passport-azure-reklam modüllerini kullanarak kaynakları koruyan web uygulamaları / API'ler](#passport)
* [Kaynakları koruyan ve Visual Studio 2015 veya sonrası ile oluşturulan Web uygulamaları / API'leri](#vs2015)
* [Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web uygulamaları](#vs2013)
* Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web API'leri
* [Kaynakları koruyan ve Visual Studio 2012 ile oluşturulan Web uygulamaları](#vs2012)
* [Web uygulamaları kaynakları korumak ve Visual Studio 2010, 2008 o Windows Identity Foundation kullanarak oluşturulan](#vs2010)
* [Web uygulamaları / API'ler kaynakları diğer kitaplıkları kullanarak veya desteklenen protokollerden herhangi birini el ile kullanarak korur](#other)

Bu kılavuz şu süreyle geçerli **değildir:**

* Azure AD Uygulama Galerisi'nden eklenen uygulamalar (Özel dahil) imzalama tuşları ile ilgili ayrı bir yönergeye sahiptir. [Daha fazla bilgi.](../manage-apps/manage-certificates-for-federated-single-sign-on.md)
* Uygulama proxy'si aracılığıyla yayınlanan şirket içi uygulamalar, anahtar imzalama konusunda endişelenmenize gerek yoktur.

### <a name="native-client-applications-accessing-resources"></a><a name="nativeclient"></a>Kaynaklara erişen yerel istemci uygulamaları
Yalnızca kaynaklara erişen uygulamalar (örn. Microsoft Graph, KeyVault, Outlook API ve diğer Microsoft API'leri) genellikle yalnızca bir belirteç edinin ve kaynak sahibine iletir. Herhangi bir kaynağı korumadıkları göz önüne alındığında, belirteci incelemezler ve bu nedenle doğru şekilde imzalandığından emin olmaları gerekmez.

Yerel istemci uygulamaları, ister masaüstü ister mobil olsun, bu kategoriye girer ve bu nedenle rollover'dan etkilenmez.

### <a name="web-applications--apis-accessing-resources"></a><a name="webclient"></a>Kaynaklara erişen Web uygulamaları / API'ler
Yalnızca kaynaklara erişen uygulamalar (örn. Microsoft Graph, KeyVault, Outlook API ve diğer Microsoft API'leri) genellikle yalnızca bir belirteç edinin ve kaynak sahibine iletir. Herhangi bir kaynağı korumadıkları göz önüne alındığında, belirteci incelemezler ve bu nedenle doğru şekilde imzalandığından emin olmaları gerekmez.

Yalnızca uygulama akışını (istemci kimlik bilgileri / istemci sertifikası) kullanan web uygulamaları ve web API'leri bu kategoriye girer ve böylece rollover'dan etkilenmez.

### <a name="web-applications--apis-protecting-resources-and-built-using-azure-app-services"></a><a name="appservices"></a>Web uygulamaları / API'ler kaynakları korur ve Azure Uygulama Hizmetleri kullanılarak oluşturulmuş
Azure Uygulama Hizmetleri'nin Kimlik Doğrulama / Yetkilendirme (EasyAuth) işlevi, anahtar devrini otomatik olarak işlemek için gerekli mantığa sahiptir.

### <a name="web-applications--apis-protecting-resources-using-net-owin-openid-connect-ws-fed-or-windowsazureactivedirectorybearerauthentication-middleware"></a><a name="owin"></a>.NET OWIN OpenID Connect, WS-Fed veya WindowsAzureActiveDirectoryBearerAuthentication aracını kullanarak kaynakları koruyan Web uygulamaları / API'ler
Uygulamanız .NET OWIN OpenID Connect, WS-Fed veya WindowsAzureActiveDirectoryBearerAuthentication aracıkullanıyorsa, anahtar devrini otomatik olarak işlemek için gerekli mantığa sahiptir.

Uygulamanızın Startup.cs veya Startup.Auth.cs aşağıdaki parçacıklardan herhangi birini arayarak uygulamanızın bunlardan herhangi birini kullandığını doğrulayabilirsiniz

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
     // ...
     });
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
     new WindowsAzureActiveDirectoryBearerAuthenticationOptions
     {
     // ...
     });
```

### <a name="web-applications--apis-protecting-resources-using-net-core-openid-connect-or--jwtbearerauthentication-middleware"></a><a name="owincore"></a>.NET Core OpenID Connect veya JwtBearerAuthentication aracını kullanarak kaynakları koruyan Web uygulamaları / API'ler
Uygulamanız .NET Core OWIN OpenID Connect veya JwtBearerAuthentication aracıkullanıyorsa, anahtar devrini otomatik olarak işlemek için gerekli mantığa sahiptir.

Uygulamanızın Startup.cs veya Startup.Auth.cs aşağıdaki parçacıklardan herhangi birini arayarak uygulamanızın bunlardan herhangi birini kullandığını doğrulayabilirsiniz

```
app.UseOpenIdConnectAuthentication(
     new OpenIdConnectAuthenticationOptions
     {
         // ...
     });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
     // ...
     });
```

### <a name="web-applications--apis-protecting-resources-using-nodejs-passport-azure-ad-module"></a><a name="passport"></a>Node.js passport-azure-reklam modüllerini kullanarak kaynakları koruyan web uygulamaları / API'ler
Başvurunuz Node.js passport-ad modüllerini kullanıyorsa, anahtar devrini otomatik olarak işlemek için gerekli mantığa sahiptir.

Uygulamanızın app.js'sinde aşağıdaki snippet'i arayarak başvuru pasaportu reklamınızı onaylayabilirsiniz.

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="web-applications--apis-protecting-resources-and-created-with-visual-studio-2015-or-later"></a><a name="vs2015"></a>Kaynakları koruyan ve Visual Studio 2015 veya sonrası ile oluşturulan Web uygulamaları / API'leri
Uygulamanız Visual Studio 2015 veya sonraki bir web uygulama şablonu kullanılarak oluşturulmuşsa ve **Kimlik Doğruluğunu Değiştir** menüsünden **İş Veya Okul Hesapları'nı** seçtiyseniz, anahtar devrini otomatik olarak işlemek için gerekli mantığa sahiptir. OWIN OpenID Connect ara yazılımına katışta bulunan bu mantık, anahtarları OpenID Connect bulma belgesinden alır ve önbelleğe alır ve önbelleğe alır ve düzenli aralıklarla yeniler.

Çözümünüzü el ile kimlik doğrulamasına eklediyseniz, uygulamanız gerekli anahtar devrilme mantığına sahip olmayabilir. Bunu kendiniz yazmanız veya [web uygulamalarındaki / API'lerde diğer kitaplıkları kullanarak veya desteklenen protokollerden herhangi birini el ile uygulamanız](#other)gerekir.

### <a name="web-applications-protecting-resources-and-created-with-visual-studio-2013"></a><a name="vs2013"></a>Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web uygulamaları
Uygulamanız Visual Studio 2013'te bir web uygulama şablonu kullanılarak oluşturulmuşsa ve **Kimlik Doğrulamasını Değiştir** menüsünden **Kuruluş Hesapları'nı** seçtiyseniz, anahtar devrini otomatik olarak işlemek için gerekli mantığa sahiptir. Bu mantık, kuruluşunuzun benzersiz tanımlayıcısını ve imzalama anahtar bilgilerini projeyle ilişkili iki veritabanı tablolarında saklar. Veritabanının bağlantı dizesini projenin Web.config dosyasında bulabilirsiniz.

Çözümünüzü el ile kimlik doğrulamasına eklediyseniz, uygulamanız gerekli anahtar devrilme mantığına sahip olmayabilir. Bunu kendiniz yazmanız veya [web uygulamalarındaki / API'lerde diğer kitaplıkları kullanarak veya desteklenen protokollerden herhangi birini el ile uygulamanız gerekir.](#other)

Aşağıdaki adımlar, mantığın uygulamanızda düzgün çalıştığını doğrulamanıza yardımcı olur.

1. Visual Studio 2013'te çözümü açın ve ardından sağ penceredeki **Server Explorer** sekmesine tıklayın.
2. **Veri Bağlantılarını**Genişlet , **Varsayılan Bağlantı**ve ardından **Tablolar**. Veren **AuthorityKeys** tablosunu bulun, sağ tıklatın ve ardından **Tablo Verilerini Göster'i**tıklatın.
3. **VermeAuthorityKeys** tablosunda, anahtarın parmak izi değerine karşılık gelen en az bir satır olacaktır. Tablodaki satırları silin.
4. **Kiracılar** tablosuna sağ tıklayın ve ardından **Tablo Verilerini Göster'i**tıklatın.
5. **Kiracı** tablosunda, benzersiz bir dizin kiracı tanımlayıcısına karşılık gelen en az bir satır olacaktır. Tablodaki satırları silin. **Hem Kiracı** tablosundaki hem de **VermeAuthorityKeys** tablosundaki satırları silmezseniz, çalışma zamanında bir hata alırsınız.
6. Uygulamayı derleyin ve çalıştırın. Hesabınızda oturum açtıktan sonra uygulamayı durdurabilirsiniz.
7. **Sunucu Gezgini'ne** dönün ve **Veren AuthorityKeys** ve **Kiracılar** tablosundaki değerlere bakın. Federasyon meta veri belgesinden gelen uygun bilgilerle otomatik olarak yeniden doldurulduklarını fark edeceksiniz.

### <a name="web-apis-protecting-resources-and-created-with-visual-studio-2013"></a><a name="vs2013"></a>Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web API'leri
Visual Studio 2013'te Web API şablonunu kullanarak bir web API uygulaması oluşturduysanız ve ardından **Kimlik Doğruluğunu Değiştir** menüsünden **Kurumsal Hesapları** seçtiyseniz, uygulamanızda zaten gerekli mantığa sahipsiniz.

Kimlik doğrulamasını el ile yapılandırmışsanız, web API'nizi temel bilgilerini otomatik olarak güncelleştirmek üzere nasıl yapılandıracağınıöğrenmek için aşağıdaki yönergeleri izleyin.

Aşağıdaki kod snippet federasyon meta veri belgesinden en son anahtarları almak ve sonra belirteci doğrulamak için [JWT Belirteç İşleyicisi](https://msdn.microsoft.com/library/dn205065.aspx) kullanın nasıl gösterir. Kod snippet, ister veritabanında, ister yapılandırma dosyasında veya başka bir yerde olsun, Azure AD'den gelecek belirteçleri doğrulamak için anahtarı kalıcı olarak kullanmak için kendi önbelleğe alma mekanizmasınızı kullanacağınızı varsayar.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="web-applications-protecting-resources-and-created-with-visual-studio-2012"></a><a name="vs2012"></a>Kaynakları koruyan ve Visual Studio 2012 ile oluşturulan Web uygulamaları
Uygulamanız Visual Studio 2012'de oluşturulmuşsa, uygulamanızı yapılandırmak için büyük olasılıkla Kimlik ve Erişim Aracı'nı kullanmışolabilirsiniz. Ayrıca, [Doğrulama Veren Ad Kayıt Defteri (VINR)](https://msdn.microsoft.com/library/dn205067.aspx)kullanıyorsanız da muhtemeldir. VINR, güvenilen kimlik sağlayıcıları (Azure AD) ve bunlar tarafından verilen belirteçleri doğrulamak için kullanılan anahtarlar hakkında bilgi sağlamakla yükümlüdür. VINR ayrıca, dizininizde ilişkili en son federasyon meta veri belgesini indirerek, yapılandırmanın en son belgeyle güncel olup olmadığını kontrol ederek Web.config dosyasında depolanan önemli bilgileri otomatik olarak güncelleştirmeyi ve gerektiğinde yeni anahtarı kullanmak için uygulamayı güncelleştirin.

Uygulamanızı Microsoft tarafından sağlanan kod örneklerinden veya gözden geçireç belgelerinden birini kullanarak oluşturduysanız, anahtar devri mantığı zaten projenize dahildir. Aşağıdaki kodun projenizde zaten mevcut olduğunu fark edeceksiniz. Uygulamanız zaten bu mantığa sahip değilse, eklemek ve doğru çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. **Solution**Explorer'da, uygun proje için **System.IdentityModel** derlemesine bir başvuru ekleyin.
2. **Global.asax.cs** dosyasını açın ve yönergeleri kullanarak aşağıdaki leri ekleyin:
   ```
   using System.Configuration;
   using System.IdentityModel.Tokens;
   ```
3. **Global.asax.cs** dosyasına aşağıdaki yöntemi ekleyin:
   ```
   protected void RefreshValidationSettings()
   {
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
   }
   ```
4. **Global.asax.cs'da** **Application_Start()** yönteminde **RefreshValidationSettings()** yöntemini çağırın:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Bu adımları izledikten sonra, uygulamanızın Web.config'i en son anahtarlar da dahil olmak üzere federasyon meta veri belgesindeki en son bilgilerle güncelleştirilir. Bu güncelleştirme, uygulama havuzunuz IIS'de her geri dönüşüm de oluşacaktır; varsayılan olarak IIS uygulamaları her 29 saatte bir geri dönüştürmek için ayarlanır.

Anahtar devriman mantığının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. Uygulamanızın yukarıdaki kodu kullandığını doğruladıktan sonra, **Web.config** dosyasını açın ve özellikle aşağıdaki birkaç satırı ** \<arayarak, verenNameRegistry>** bloğuna gidin:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. Thumbprint=">ayarında, herhangi bir karakteri farklı bir karakterle değiştirerek parmak izi değerini değiştirin. ** \<** **Web.config** dosyasını kaydedin.
3. Uygulamayı oluşturun ve çalıştırın. Oturum açma işlemini tamamlayabilirseniz, başvurunuz dizinizin federasyon meta veri belgesinden gerekli bilgileri indirerek anahtarı başarıyla günceller. Oturum açma sorunları yaşıyorsanız, Azure AD makalesini kullanarak [Web Uygulamanıza Oturum Açma'yı](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) okuyarak veya aşağıdaki kod örneğini indirip inceleyerek uygulamanızdaki değişikliklerin doğru olduğundan emin olun: [Azure Etkin Diziniçin Çok Kiracılı Bulut Uygulaması.](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b)

### <a name="web-applications-protecting-resources-and-created-with-visual-studio-2008-or-2010-and-windows-identity-foundation-wif-v10-for-net-35"></a><a name="vs2010"></a>Web uygulamaları kaynakları koruyan ve Visual Studio 2008 veya 2010 ve Windows Identity Foundation (WIF) v1.0 ile oluşturulan .NET 3.5 için
WIF v1.0'da bir uygulama oluşturmuşsanız, yeni bir anahtar kullanmak için uygulamanızın yapılandırmasını otomatik olarak yenilemek için sağlanan bir mekanizma yoktur.

* *En kolay yol* WIF SDK'da yer alan ve en son meta veri belgesini alıp yapılandırmanızı güncelleştirebilen FedUtil aracını kullanın.
* Uygulamanızı, Sistem ad alanında bulunan WIF'in en yeni sürümünü içeren .NET 4.5 ile güncelleyin. Daha sonra, uygulamanın yapılandırmasının otomatik güncelleştirmelerini gerçekleştirmek için [Doğrulama İhraççı Adı Kayıt Defteri'ni (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) kullanabilirsiniz.
* Bu kılavuz belgenin sonundaki talimatlara göre el ile rollover gerçekleştirin.

Yapılandırmanızı güncellemek için FedUtil'i kullanma yönergeleri:

1. Visual Studio 2008 veya 2010 için geliştirme makinenizde WIF v1.0 SDK yüklü olduğunu doğrulayın. Henüz yüklemediyseniz [buradan indirebilirsiniz.](https://www.microsoft.com/en-us/download/details.aspx?id=4451)
2. Visual Studio'da çözümü açın ve ardından ilgili projeyi sağ tıklatın ve **Federasyon meta verilerini güncelleştir'i**seçin. Bu seçenek kullanılamıyorsa, FedUtil ve/veya WIF v1.0 SDK yüklenmedi.
3. Federasyon meta verilerinizi güncelleştirmeye başlamak için komut isteminden **Güncelleştirme'yi** seçin. Uygulamanın barındırıldığı sunucu ortamına erişiminiz varsa, isteğe bağlı olarak FedUtil'in [otomatik meta veri güncelleştirme zamanlayıcısını](https://msdn.microsoft.com/library/ee517272.aspx)kullanabilirsiniz.
4. Güncelleştirme işlemini tamamlamak için **Bitir'i** tıklatın.

### <a name="web-applications--apis-protecting-resources-using-any-other-libraries-or-manually-implementing-any-of-the-supported-protocols"></a><a name="other"></a>Web uygulamaları / API'ler kaynakları diğer kitaplıkları kullanarak veya desteklenen protokollerden herhangi birini el ile kullanarak korur
Başka bir kitaplık kullanıyorsanız veya desteklenen protokollerden herhangi birini el ile uyguladıysanız, anahtarın OpenID Connect bulma belgesinden veya federasyon meta verilerinden alındığından emin olmak için kitaplığı veya uygulamanızı gözden geçirmeniz gerekir Belge. Bunu denetlemenin bir yolu, OpenID bulma belgesine veya federasyon meta veri belgesine yapılan aramalar için kodunuzda veya kitaplığın kodunda arama yapmaktır.

Bu anahtar bir yerde depolanıyorsa veya uygulamanızda kodlanmışsa, bu kılavuz belgenin sonundaki talimatlara göre el ile rollover gerçekleştirerek anahtarı el ile alabilir ve buna göre güncelleyebilirsiniz. Azure AD'nin rollover'ını artırması veya acil bir bant dışı rollover'ı olması durumunda gelecekteki aksaklıkları ve genel yükü önlemek için bu makalede ana hatlarını kullanarak otomatik rollover'ı desteklemek için **uygulamanızı geliştirmeniz önemle tavsiye edilir.**

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Etkilenip etkilenmeyeceğini belirlemek için uygulamanız nasıl test edilebilir?
Uygulamanızın komut dosyalarını indirerek ve [bu GitHub deposundaki](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) yönergeleri izleyerek otomatik anahtar devrini destekleyip desteklemediğini doğrulayabilirsiniz.

## <a name="how-to-perform-a-manual-rollover-if-your-application-does-not-support-automatic-rollover"></a>Uygulamanız otomatik rollover'ı desteklemiyorsa manuel rollover nasıl gerçekleştirilir?
Uygulamanız otomatik rollover'ı **desteklemiyorsa,** Azure AD'nin imzalama anahtarlarını düzenli aralıklarla izleyen ve buna göre manuel rollover gerçekleştiren bir işlem oluşturmanız gerekir. [Bu GitHub deposu,](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) bunun nasıl yapılacağını anlatan komut dosyaları ve yönergeler içerir.

