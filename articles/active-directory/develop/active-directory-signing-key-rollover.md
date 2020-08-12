---
title: Microsoft Identity platformunda imzalama anahtarı geçişi
description: Bu makalede Azure Active Directory için imzalama anahtarı aktarma en iyi yöntemleri açıklanmaktadır
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
ms.openlocfilehash: 42f100618ac6ce8769c4a7da67a5bd586794c63b
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88115603"
---
# <a name="signing-key-rollover-in-microsoft-identity-platform"></a>Microsoft Identity platformunda imzalama anahtarı geçişi
Bu makalede, güvenlik belirteçlerini imzalamak için Microsoft Identity platform tarafından kullanılan ortak anahtarlar hakkında bilmeniz gerekenler açıklanmaktadır. Bu anahtarların düzenli olarak bir süre içinde devredildiğini ve acil bir durumda bir acil durum için hemen geri alınabilir olduğunu unutmayın. Microsoft Identity platform kullanan tüm uygulamaların, anahtar geçişi işlemini programlı bir şekilde işleyebilmesi veya düzenli bir el ile geçiş işlemi kurabilmesi gerekir. Anahtarların nasıl çalıştığını, uygulamanıza yapılan geçişin etkisini nasıl değerlendirireceğini ve gerekirse anahtar rollover 'ı işlemek için düzenli el ile geçiş süreci oluşturmayı öğrenmek için okumaya devam edin.

## <a name="overview-of-signing-keys-in-microsoft-identity-platform"></a>Microsoft Identity platformunda imzalama anahtarlarına genel bakış
Microsoft Identity platform, kendisini kullanan uygulamalar arasında güven sağlamak için endüstri standartları üzerinde oluşturulmuş ortak anahtar şifrelemeyi kullanır. Pratik koşullarda, bu işlem aşağıdaki şekilde işe yarar: Microsoft Identity platform ortak ve özel anahtar çiftinden oluşan bir imzalama anahtarı kullanır. Bir kullanıcı kimlik doğrulaması için Microsoft Identity platform kullanan bir uygulamada oturum açtığında, Microsoft Identity platform Kullanıcı hakkında bilgi içeren bir güvenlik belirteci oluşturur. Bu belirteç, uygulamaya geri gönderilmeden önce özel anahtarı kullanılarak Microsoft Identity platformu tarafından imzalanır. Belirtecin geçerli olduğunu ve Microsoft Identity platformundan geldiğini doğrulamak için, uygulamanın, kiracının [OpenID Connect bulgu belgesi](https://openid.net/specs/openid-connect-discovery-1_0.html) veya SAML/WS-beslikli [Federasyon meta veri belgesinde](../azuread-dev/azure-ad-federation-metadata.md)bulunan Microsoft Identity platform tarafından kullanıma sunulan ortak anahtarı kullanarak belirtecin imzasını doğrulaması gerekir.

Güvenlik nedeniyle, Microsoft Identity platformunun imzalama anahtarı düzenli olarak kaydedilir ve acil durumda bir acil durum durumunda hemen üzerinden alınabilir. Microsoft Identity platformu ile tümleştirilen herhangi bir uygulama, ne sıklıkta gerçekleşebileceğini önemli bir şekilde bir anahtar geçişi olayını işleyecek şekilde hazırlanmalıdır. Yoksa ve uygulamanız bir belirteçte imzayı doğrulamak için bir süre sonu anahtarını kullanmayı denerse, oturum açma isteği başarısız olur.

OpenID Connect bulgu belgesinde ve Federasyon meta verileri belgesinde her zaman bir geçerli anahtar mevcuttur. Uygulamanız, belgede belirtilen anahtarlardan herhangi birini kullanmaya hazırlanmalıdır, çünkü bir anahtar yakında alınabilir, diğeri de bunun yerini alır ve bu şekilde devam eder.

## <a name="how-to-assess-if-your-application-will-be-affected-and-what-to-do-about-it"></a>Uygulamanızın etkilenip etkilendiğine ve bunun ne yapacaklarınızın nasıl değerlendirilecektir
Uygulamanızın anahtar rollover 'ı nasıl işleyeceği, uygulamanın türü veya kimlik protokolünün ve kitaplığın kullanıldığı değişkenlere bağlıdır. Aşağıdaki bölümler, en yaygın uygulama türlerinin anahtar rollover tarafından etkilenip etkilenmediğini değerlendirir ve uygulamanın otomatik geçişi desteklemek veya anahtarı el ile güncelleştirmek için nasıl güncellenebileceğine ilişkin yönergeler sağlar.

* [Kaynaklara erişen yerel istemci uygulamaları](#nativeclient)
* [Kaynaklara erişen web uygulamaları/API 'Ler](#webclient)
* [Kaynakları koruyan ve Azure Uygulama Hizmetleri kullanılarak oluşturulan Web uygulamaları/API 'Leri](#appservices)
* [.NET OWIN OpenID Connect, WS-beslenir veya Windowsazureactivedirectoryyataerauthentication ara yazılımı kullanılarak kaynakları koruyan Web uygulamaları/API 'Leri](#owin)
* [.NET Core OpenID Connect veya Jwtyataerauthentication ara yazılımı kullanılarak kaynakları koruyan Web uygulamaları/API 'Leri](#owincore)
* [Node.js Passport-Azure-ad modülü kullanarak kaynakları koruyan Web uygulamaları/API 'Leri](#passport)
* [Kaynakları koruyan ve Visual Studio 2015 veya üzeri ile oluşturulan Web uygulamaları/API 'Leri](#vs2015)
* [Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web uygulamaları](#vs2013)
* Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web API 'Leri
* [Kaynakları koruyan ve Visual Studio 2012 ile oluşturulan Web uygulamaları](#vs2012)
* [Windows Identity Foundation kullanarak kaynakları koruyan ve Visual Studio 2010, 2008 o ile oluşturulan Web uygulamaları](#vs2010)
* [Diğer kitaplıkları kullanan veya desteklenen protokollerden herhangi birini uygulayan Web uygulamaları/API 'Leri, kaynakları koruyan](#other)

Bu kılavuz için geçerli **değildir** :

* Azure AD Uygulama Galerisi 'nden (özel dahil) eklenen uygulamalar, imzalama anahtarlarının dikkate aldığı ayrı bir kılavuza sahiptir. [Daha fazla bilgi.](../manage-apps/manage-certificates-for-federated-single-sign-on.md)
* Uygulama proxy 'si aracılığıyla yayımlanan şirket içi uygulamalar, imzalama anahtarları hakkında endişelenmenize gerek kalmaz.

### <a name="native-client-applications-accessing-resources"></a><a name="nativeclient"></a>Kaynaklara erişen yerel istemci uygulamaları
Yalnızca kaynaklara erişen uygulamalar (ör. Microsoft Graph, Keykasası, Outlook API ve diğer Microsoft API 'Leri) genellikle yalnızca bir belirteç alıp kaynak sahibine geçiş yapın. Hiçbir kaynağı korumadıklarından, belirteci incelemeyin ve bu nedenle düzgün bir şekilde imzalandığından emin olmak zorunda değildir.

Masaüstü veya mobil tarafından bu kategoriye giren ve bu nedenle geçiş tarafından etkilenmeyenler yerel istemci uygulamaları.

### <a name="web-applications--apis-accessing-resources"></a><a name="webclient"></a>Kaynaklara erişen web uygulamaları/API 'Ler
Yalnızca kaynaklara erişen uygulamalar (ör. Microsoft Graph, Keykasası, Outlook API ve diğer Microsoft API 'Leri) genellikle yalnızca bir belirteç alıp kaynak sahibine geçiş yapın. Hiçbir kaynağı korumadıklarından, belirteci incelemeyin ve bu nedenle düzgün bir şekilde imzalandığından emin olmak zorunda değildir.

Yalnızca uygulama akışını kullanan Web uygulamaları ve Web API 'Leri (istemci kimlik bilgileri/istemci sertifikası), bu kategoriye girer ve bu nedenle geçiş tarafından etkilenmemektedir.

### <a name="web-applications--apis-protecting-resources-and-built-using-azure-app-services"></a><a name="appservices"></a>Kaynakları koruyan ve Azure Uygulama Hizmetleri kullanılarak oluşturulan Web uygulamaları/API 'Leri
Azure Uygulama Hizmetleri ' kimlik doğrulama/yetkilendirme (EasyAuth) işlevselliğinde, anahtar geçişi otomatik olarak işlemek için gerekli mantık zaten var.

### <a name="web-applications--apis-protecting-resources-using-net-owin-openid-connect-ws-fed-or-windowsazureactivedirectorybearerauthentication-middleware"></a><a name="owin"></a>.NET OWIN OpenID Connect, WS-beslenir veya Windowsazureactivedirectoryyataerauthentication ara yazılımı kullanılarak kaynakları koruyan Web uygulamaları/API 'Leri
Uygulamanız .NET OWIN OpenID Connect, WS-beslenir veya Windowsazureactivedirectoryyataerauthentication ara yazılımı kullanıyorsa, anahtar geçişi otomatik olarak işlemek için gerekli mantık zaten vardır.

Uygulamanızın Startup.cs veya Startup.Auth.cs ' de aşağıdaki kod parçacıklarını arayarak uygulamanızın bunlardan herhangi birini kullandığını doğrulayabilirsiniz.

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

### <a name="web-applications--apis-protecting-resources-using-net-core-openid-connect-or--jwtbearerauthentication-middleware"></a><a name="owincore"></a>.NET Core OpenID Connect veya Jwtyataerauthentication ara yazılımı kullanılarak kaynakları koruyan Web uygulamaları/API 'Leri
Uygulamanız .NET Core OWIN OpenID Connect veya Jwtyataerauthentication ara yazılımı kullanıyorsa, anahtar geçişi otomatik olarak işlemek için gerekli mantık zaten vardır.

Uygulamanızın Startup.cs veya Startup.Auth.cs ' de aşağıdaki kod parçacıklarını arayarak uygulamanızın bunlardan herhangi birini kullandığını doğrulayabilirsiniz.

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

### <a name="web-applications--apis-protecting-resources-using-nodejs-passport-azure-ad-module"></a><a name="passport"></a>Node.js Passport-Azure-ad modülü kullanarak kaynakları koruyan Web uygulamaları/API 'Leri
Uygulamanız Node.js Passport-ad modülünü kullanıyorsa, anahtar geçişi otomatik olarak işlemek için gerekli mantık zaten vardır.

Uygulamanızın Passport-ad ' i, uygulamanızın app.js aşağıdaki kod parçacığını arayarak doğrulayabilirsiniz

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
    //...
));
```

### <a name="web-applications--apis-protecting-resources-and-created-with-visual-studio-2015-or-later"></a><a name="vs2015"></a>Kaynakları koruyan ve Visual Studio 2015 veya üzeri ile oluşturulan Web uygulamaları/API 'Leri
Uygulamanız, Visual Studio 2015 veya sonraki sürümlerde bir Web uygulaması şablonu kullanılarak oluşturulduysa ve **değişiklik kimlik doğrulaması** menüsünden **Iş veya okul hesapları** ' nı seçtiyseniz, anahtar geçişi otomatik olarak işlemek için gerekli mantık zaten vardır. OWıN OpenID Connect ara yazılım ' na gömülü olan bu mantık, OpenID Connect bulgu belgesinden anahtarları alır ve önbelleğe alır ve bunları düzenli aralıklarla yeniler.

Çözümünüze el ile kimlik doğrulaması eklediyseniz, uygulamanız gerekli anahtar geçiş mantığına sahip olmayabilir. Kendiniz yazmanız veya [diğer kitaplıkları kullanarak Web uygulamalarında/API 'lerde bulunan adımları izlemeniz veya desteklenen protokollerden herhangi birini el ile](#other)uygulamanız gerekecektir.

### <a name="web-applications-protecting-resources-and-created-with-visual-studio-2013"></a><a name="vs2013"></a>Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web uygulamaları
Uygulamanız Visual Studio 2013 bir Web uygulaması şablonu kullanılarak oluşturulmuşsa ve **kimlik doğrulama** menüsünden **Kurumsal hesaplar** ' ı seçtiyseniz, anahtar geçişi otomatik olarak işlemek için gerekli mantık zaten vardır. Bu mantık, kuruluşunuzun benzersiz tanımlayıcısını ve imza anahtarı bilgilerini projeyle ilişkili iki veritabanı tablosu halinde depolar. Veritabanının bağlantı dizesini projenin Web.config dosyasında bulabilirsiniz.

Çözümünüze el ile kimlik doğrulaması eklediyseniz, uygulamanız gerekli anahtar geçiş mantığına sahip olmayabilir. Kendiniz yazmanız veya [diğer kitaplıkları kullanarak Web uygulamalarında/API 'lerde bulunan veya desteklenen protokollerden herhangi birini elle uygulayan](#other)adımları izlemeniz gerekecektir.

Aşağıdaki adımlar, mantığın uygulamanızda düzgün çalıştığını doğrulamanıza yardımcı olur.

1. Visual Studio 2013, çözümü açın ve sağ penceredeki **Sunucu Gezgini** sekmesine tıklayın.
2. **Veri bağlantıları**, **DefaultConnection**ve ardından **Tablolar**' ı genişletin. **Issuingauthoritykeys** tablosunu bulun, sağ tıklayın ve ardından **tablo verilerini göster**' e tıklayın.
3. **Issuingauthoritykeys** tablosunda, anahtar için parmak izi değerine karşılık gelen en az bir satır olacaktır. Tablodaki satırları silin.
4. **Kiracılar** tablosuna sağ tıklayın ve ardından **tablo verilerini göster**' e tıklayın.
5. **Kiracılar** tablosunda, benzersiz bir dizin kiracı tanımlayıcısına karşılık gelen en az bir satır olacaktır. Tablodaki satırları silin. Hem **kiracılar** tablosu hem de **ıssuingauthoritykeys** tablosundaki satırları silmezseniz, çalışma zamanında bir hata alırsınız.
6. Uygulamayı derleyin ve çalıştırın. Hesabınızda oturum açtıktan sonra, uygulamayı durdurabilirsiniz.
7. **Sunucu Gezgini** dönün ve **ıssuingauthoritykeys** ve **kiracılar** tablosundaki değerlere bakın. Federasyon meta veri belgesinden uygun bilgilerle otomatik olarak yeniden doldurulduğuna dikkat edin.

### <a name="web-apis-protecting-resources-and-created-with-visual-studio-2013"></a><a name="vs2013"></a>Kaynakları koruyan ve Visual Studio 2013 ile oluşturulan Web API 'Leri
Visual Studio 2013 ' de Web API şablonu kullanarak bir Web API uygulaması oluşturduysanız ve sonra **değişiklik kimlik doğrulaması** menüsünden **Kurumsal hesaplar** ' ı seçtiyseniz, uygulamanızda gerekli mantığa sahipsiniz demektir.

Kimlik doğrulamasını el ile yapılandırdıysanız, kendi anahtar bilgilerini otomatik olarak güncelleştirmek üzere Web API 'nizi nasıl yapılandıracağınızı öğrenmek için aşağıdaki yönergeleri izleyin.

Aşağıdaki kod parçacığı, Federasyon meta veri belgesinden en son anahtarları nasıl alınacağını ve sonra belirteci doğrulamak için [JWT belirteci işleyicisini](/previous-versions/dotnet/framework/security/json-web-token-handler) nasıl kullanacağınızı gösterir. Kod parçacığı, bir veritabanı, yapılandırma dosyası veya başka bir yerde olup olmadığı gibi, Microsoft Identity platform 'dan gelecek belirteçleri doğrulamak için anahtarı kalıcı hale getiren kendi önbelleğe alma mekanizmanızı kullanacağınızı varsayar.

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
Uygulamanız Visual Studio 2012 ' de oluşturulduysa, büyük olasılıkla uygulamanızı yapılandırmak için kimlik ve erişim aracını kullanmışsınızdır. Bu, [doğrulama verenin ad kayıt defteri 'ni (VINR)](/previous-versions/dotnet/framework/security/validating-issuer-name-registry)de kullanıyor olabilirsiniz. HAVR, güvenilir kimlik sağlayıcıları (Microsoft Identity Platform) ve bunlar tarafından verilen belirteçleri doğrulamak için kullanılan anahtarlarla ilgili bilgilerin korunmasından sorumludur. VINR Ayrıca dizininizle ilişkili en son Federasyon meta veri belgesini indirerek, yapılandırmanın en son belgeyle güncel olup olmadığını kontrol ederek ve uygulamayı yeni anahtarı gerektiği gibi kullanacak şekilde güncelleştirerek bir Web.config dosyasında depolanan anahtar bilgilerini otomatik olarak güncelleştirmeyi kolaylaştırır.

Uygulamanızı Microsoft tarafından sağlanan kod örneklerinden veya İzlenecek yol belgelerinden birini kullanarak oluşturduysanız, anahtar aktarma mantığı projenize zaten dahil edilmiştir. Aşağıdaki kodun projenizde zaten var olduğunu fark edeceksiniz. Uygulamanızda zaten bu mantık yoksa, eklemek ve düzgün çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. **Çözüm Gezgini**, uygun proje için **System. IdentityModel** derlemesine bir başvuru ekleyin.
2. **Global.asax.cs** dosyasını açın ve aşağıdaki using yönergelerini ekleyin:
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
4. **Global.asax.cs** içinde gösterildiği gibi, **Application_Start ()** yönteminde **refreshvalidationsettings ()** yöntemini çağırın:
   ```
   protected void Application_Start()
   {
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
   }
   ```

Bu adımları izledikten sonra, uygulamanızın Web.config en son anahtarlar dahil olmak üzere federasyon meta veri belgesinden en son bilgilerle güncelleştirilir. Bu güncelleştirme, uygulama havuzunuzun IIS 'de her geri dönüştürüldüğünde oluşur; Varsayılan olarak IIS, uygulamaları her 29 saatte bir geri dönüştürmek üzere ayarlanmıştır.

Anahtar aktarma mantığının çalıştığını doğrulamak için aşağıdaki adımları izleyin.

1. Uygulamanızın yukarıdaki kodu kullandığını doğruladıktan sonra, **Web.config** dosyasını açın ve **\<issuerNameRegistry>** özellikle aşağıdaki birkaç satırı arayarak bloğa gidin:
   ```
   <issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
   ```
2. **\<add thumbprint="">** Ayarında, herhangi bir karakteri farklı bir karakterle değiştirerek parmak izi değerini değiştirin. **Web.config** dosyasını kaydedin.
3. Uygulamayı derleyin ve çalıştırın. Oturum açma işlemini tamamlayabilirseniz, uygulamanız dizininizin Federasyon meta veri belgesinden gerekli bilgileri indirerek anahtarı başarıyla güncelliyor. Oturum açarken sorun yaşıyorsanız, [Microsoft Identity platform makalesini kullanarak Web uygulamanıza oturum açma ekleme](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) veya aşağıdaki kod örneğini indirme ve İnceleme ' yi okuyarak uygulamanızdaki değişikliklerin doğru olduğundan emin olun: [Azure Active Directory Için çok kiracılı bulut uygulaması](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b).

### <a name="web-applications-protecting-resources-and-created-with-visual-studio-2008-or-2010-and-windows-identity-foundation-wif-v10-for-net-35"></a><a name="vs2010"></a>.NET 3,5 için kaynakları koruyan ve Visual Studio 2008 ya da 2010 ve Windows Identity Foundation (WıF) v 1.0 ile oluşturulan Web uygulamaları
WıF v 1.0 üzerinde bir uygulama oluşturduysanız, yeni bir anahtar kullanmak için uygulamanızın yapılandırmasını otomatik olarak yenilemek üzere bir sağlanmayan mekanizma yoktur.

* *En kolay yol* WıF SDK ' da bulunan FedUtil aracı 'nı kullanın. Bu, en son meta veri belgesini alabilir ve yapılandırmanızı güncelleştirebilir.
* Uygulamanızı, sistem ad alanında yer alan en yeni WıF sürümünü içeren .NET 4,5 ' a güncelleştirin. Daha sonra, uygulama yapılandırmasının otomatik güncelleştirmelerini gerçekleştirmek için [doğrulama verenin adı kayıt defterini (VINR)](/previous-versions/dotnet/framework/security/validating-issuer-name-registry) kullanabilirsiniz.
* Bu kılavuz belgesinin sonundaki yönergelere göre el ile geçiş gerçekleştirin.

Yapılandırmanızı güncelleştirmek için FedUtil kullanma yönergeleri:

1. Visual Studio 2008 veya 2010 için geliştirme makinenizde WıF v 1.0 SDK 'nın yüklü olduğunu doğrulayın. Henüz yüklemediyseniz, [buradan indirebilirsiniz](https://www.microsoft.com/en-us/download/details.aspx?id=4451) .
2. Visual Studio 'da çözümü açın ve ardından geçerli projeye sağ tıklayıp **Federasyon meta verilerini Güncelleştir**' i seçin. Bu seçenek kullanılamıyorsa, FedUtil ve/veya WıF v 1.0 SDK yüklü değildir.
3. Komut isteminde, Federasyon meta verilerinizi güncelleştirmeye başlamak için **Güncelleştir** ' i seçin. Uygulamanın barındırıldığı sunucu ortamına erişiminiz varsa, isteğe bağlı olarak FedUtil 'nin [Otomatik meta veri güncelleştirme zamanlayıcısını](/previous-versions/windows-identity-foundation/ee517272(v=msdn.10))kullanabilirsiniz.
4. Güncelleştirme işlemini gerçekleştirmek için **son** ' a tıklayın.

### <a name="web-applications--apis-protecting-resources-using-any-other-libraries-or-manually-implementing-any-of-the-supported-protocols"></a><a name="other"></a>Diğer kitaplıkları kullanan veya desteklenen protokollerden herhangi birini uygulayan Web uygulamaları/API 'Leri, kaynakları koruyan
Başka bir kitaplığı kullanıyorsanız veya desteklenen protokollerden herhangi birini el ile uyguladıysanız, anahtarın OpenID Connect bulgu belgesinden veya Federasyon meta veri belgesinden alındığından emin olmak için kitaplığı veya uygulamanızı gözden geçirmeniz gerekir. Bunu kontrol etmenin bir yolu, OpenID bulgu belgesine veya Federasyon meta veri belgesine yapılan çağrılar için kodunuzda veya kitaplığın kodunda bir arama yapmak.

Bu anahtar, uygulamanızda bir yere veya sabit kodlanmış olarak depolanırsa, bu kılavuz belgesinin sonundaki yönergelere göre el ile bir rollover gerçekleştirerek, anahtarı el ile alabilir ve uygun şekilde güncelleştirebilirsiniz. Microsoft Identity platform 'ın rollover temposunda 'i artırdığı veya bir acil durum bant dışı geçişe sahip olması durumunda, gelecekteki kesintiler ve yük devretme adımlarını önlemek için **uygulamanızı geliştirmektir** .

## <a name="how-to-test-your-application-to-determine-if-it-will-be-affected"></a>Uygulamanızı, etkilenip etkilenmediğine yönelik test etme
Komut dosyalarını indirerek ve [Bu GitHub deposundaki](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) yönergeleri izleyerek uygulamanızın otomatik anahtar geçişi 'ni destekleyip desteklemediğini doğrulayabilirsiniz.

## <a name="how-to-perform-a-manual-rollover-if-your-application-does-not-support-automatic-rollover"></a>Uygulamanız otomatik geçişi desteklemiyorsa el ile geçiş gerçekleştirme
Uygulamanız otomatik **geçişi desteklemiyorsa,** Microsoft kimlik platformunun imzalama anahtarlarını düzenli olarak izleyen ve el ile geçiş yapan bir işlem oluşturmanız gerekir. [Bu GitHub deposu](https://github.com/AzureAD/azure-activedirectory-powershell-tokenkey) bunun nasıl yapılacağı hakkında betikler ve yönergeler içerir.