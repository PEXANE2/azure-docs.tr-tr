---
title: İstemci sertifikalarını kullanarak yeniden bir hizmetin güvenliğini sağlama
titleSuffix: Azure AD B2C
description: İstemci sertifikalarını kullanarak Azure AD B2C özel REST API talep alışverlerinizin güvenliğini sağlama
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2017
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 98606443707c183a15a4bacd26646c3eac6c8a69
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78186922"
---
# <a name="secure-your-restful-service-by-using-client-certificates"></a>İstemci sertifikalarını kullanarak yeniden takip eden hizmetinizi güvenli hale getirin

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

İlgili bir makalede, Azure Active Directory B2C (Azure AD B2C) ile etkileşimde bulunan [bir yeniden çalışan hizmet oluşturursunuz](rest-api-claims-exchange-dotnet.md) .

Bu makalede, bir istemci sertifikası kullanarak Azure Web uygulamanıza erişimi nasıl kısıtlayacağınızı (yeniden deneme API) öğreneceksiniz. Bu mekanizmaya TLS karşılıklı kimlik doğrulaması veya *istemci sertifikası kimlik doğrulaması*adı verilir. Yalnızca Azure AD B2C gibi doğru sertifikalara sahip hizmetler hizmetinize erişebilir.

>[!NOTE]
>Ayrıca, [http temel kimlik doğrulamasını](secure-rest-api-dotnet-basic-auth.md)kullanarak, yeniden yaptığınız hizmeti güvenli hale getirebilirsiniz. Ancak, HTTP temel kimlik doğrulaması bir istemci sertifikası üzerinde daha az güvenli kabul edilir. Önerimiz, bu makalede açıklandığı gibi istemci sertifikası kimlik doğrulaması kullanarak yeniden yapılan hizmeti güvenli hale getirmeye yönelik.

Bu makalede nasıl yapılacağı açıklanır:
* Web uygulamanızı istemci sertifikası kimlik doğrulamasını kullanacak şekilde ayarlayın.
* Sertifikayı Azure AD B2C ilkesi anahtarlarına yükleyin.
* Özel ilkenizi istemci sertifikasını kullanacak şekilde yapılandırın.

## <a name="prerequisites"></a>Önkoşullar
* [REST API talep alışverişlerinde tümleştirme](rest-api-claims-exchange-dotnet.md) makalesindeki adımları uygulayın.
* Geçerli bir sertifika (özel anahtara sahip bir. pfx dosyası) alın.

## <a name="step-1-configure-a-web-app-for-client-certificate-authentication"></a>1\. Adım: istemci sertifikası kimlik doğrulaması için bir Web uygulaması yapılandırma
İstemci sertifikaları gerektirecek **Azure App Service** ayarlamak için, web uygulaması `clientCertEnabled` site ayarını *doğru*olarak ayarlayın. Bu değişikliği yapmak için, Azure portal web uygulaması sayfanızı açın. Sol gezinti bölmesinde **Ayarlar** ' ın altında **SSL ayarları**' nı seçin. **Istemci sertifikaları** bölümünde **gelen istemci sertifikası** seçeneğini açın.

>[!NOTE]
>Azure App Service planınızın standart veya daha büyük olduğundan emin olun. Daha fazla bilgi için bkz. [Azure App Service planlar ayrıntılı genel bakış](https://docs.microsoft.com/azure/app-service/overview-hosting-plans).

>[!NOTE]
>**Clientcertenabled** özelliğini ayarlama hakkında daha fazla bilgi için bkz. [Web Apps için TLS karşılıklı kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

## <a name="step-2-upload-your-certificate-to-azure-ad-b2c-policy-keys"></a>2\. Adım: sertifikanızı Azure AD B2C ilke anahtarlarına yükleme
`clientCertEnabled` *true*olarak ayarladıktan sonra, RESTATION API 'niz ile iletişim istemci sertifikası gerektirir. İstemci sertifikasını Azure AD B2C kiracınızda almak, karşıya yüklemek ve depolamak için aşağıdakileri yapın:
1. Azure AD B2C kiracınızda **B2C ayarları** > **kimlik deneyimi çerçevesi**' ni seçin.

2. Kiracınızda kullanılabilir olan anahtarları görüntülemek için, **Ilke anahtarları**' nı seçin.

3. **Add (Ekle)** seçeneğini belirleyin.
    **Anahtar oluştur** penceresi açılır.

4. **Seçenekler** kutusunda **karşıya yükle**' yi seçin.

5. **Ad** kutusuna **B2cRestClientCertificate**yazın.
    Ön ek *B2C_1A_* otomatik olarak eklenir.

6. **Karşıya dosya yükleme** kutusunda, sertifikanın. pfx dosyasını özel bir anahtarla seçin.

7. **Parola** kutusuna sertifikanın parolasını yazın.

    ![Azure portal anahtar oluştur sayfasında ilke anahtarını karşıya yükle](./media/secure-rest-api-dotnet-certificate-auth/rest-api-netfw-secure-client-cert-upload.png)

7. **Oluştur**’u seçin.

8. Kiracınızda kullanılabilen anahtarları görüntülemek için `B2C_1A_B2cRestClientCertificate` anahtarı oluşturduğunuzdan emin olun ve **Ilke anahtarları**' nı seçin.

## <a name="step-3-change-the-technical-profile"></a>3\. Adım: teknik profili değiştirme
Özel ilkenizde istemci sertifikası kimlik doğrulamasını desteklemek için aşağıdakileri yaparak teknik profili değiştirin:

1. Çalışma dizininizde, *TrustFrameworkExtensions. xml* uzantısı ilke dosyasını açın.

2. `Id="REST-API-SignUp"`içeren `<TechnicalProfile>` düğümünü arayın.

3. `<Metadata>` öğesini bulun.

4. *AuthenticationType* aşağıdaki şekilde *ClientCertificate*olarak değiştirin:

    ```xml
    <Item Key="AuthenticationType">ClientCertificate</Item>
    ```

5. Kapanış `<Metadata>` öğesinden hemen sonra aşağıdaki XML kod parçacığını ekleyin:

    ```xml
    <CryptographicKeys>
        <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
    </CryptographicKeys>
    ```

    Kod parçacığını ekledikten sonra teknik profiliniz aşağıdaki XML kodu gibi görünmelidir:

    ![ClientCertificate Authentication XML öğelerini ayarla](./media/secure-rest-api-dotnet-certificate-auth/rest-api-netfw-secure-client-cert-tech-profile.png)

## <a name="step-4-upload-the-policy-to-your-tenant"></a>4\. Adım: ilkeyi kiracınıza yükleme

1. [Azure Portal](https://portal.azure.com), Portal araç çubuğunda **Dizin + abonelik** simgesini seçin ve ardından Azure AD B2C kiracınızı içeren dizini seçin.

1. Azure portal, araması yapın ve **Azure AD B2C**seçin.

1. **Kimlik deneyimi çerçevesini**seçin.

1. **Tüm ilkeler**' i seçin.

1. **Ilkeyi karşıya yükle**' yi seçin.

1. Varsa **Ilkenin üzerine yaz** onay kutusunu seçin.

1. *TrustFrameworkExtensions. xml* dosyasını karşıya yükleyin ve ardından doğrulamayı geçirdiğinizden emin olun.

## <a name="step-5-test-the-custom-policy-by-using-run-now"></a>5\. Adım: Şimdi Çalıştır 'ı kullanarak özel ilkeyi test etme
1. **Azure AD B2C ayarları**' nı açın ve ardından **kimlik deneyimi çerçevesi**' ni seçin.

    >[!NOTE]
    >Şimdi Çalıştır, kiracıya en az bir uygulamanın önceden bağlı olmasını gerektirir. Uygulamaları nasıl kaydedeceğinizi öğrenmek için Azure AD B2C [Başlarken](tutorial-create-tenant.md) makalesine veya [uygulama kaydı](tutorial-register-applications.md) makalesine bakın.

2. Karşıya yüklediğiniz bağlı olan taraf (RP) özel ilkesini **B2C_1A_signup_signin**açın ve **Şimdi Çalıştır**' ı seçin.

3. **Verilen ad** kutusuna **Test** yazarak işlemi test edin.
    Azure AD B2C pencerenin üst kısmında bir hata mesajı görüntüler.

    ![Verilen ad metin kutusu vurgulanmış ve giriş doğrulama hatası gösteriliyor](./media/secure-rest-api-dotnet-certificate-auth/rest-api-netfw-test.png)

4. **Verilen ad** kutusuna bir ad yazın ("test" dışında).
    Azure AD B2C kullanıcıyı imzalar ve sonra uygulamanıza bir bağlılık programı numarası gönderir. Bu JWT örneğinde bulunan sayıyı göz önünde edin:

   ```
   {
     "typ": "JWT",
     "alg": "RS256",
     "kid": "X5eXk4xyojNFum1kl2Ytv8dlNP4-c57dO6QGTVBwaNk"
   }.{
     "exp": 1507125903,
     "nbf": 1507122303,
     "ver": "1.0",
     "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
     "aud": "e1d2612f-c2bc-4599-8e7b-d874eaca1ee1",
     "acr": "b2c_1a_signup_signin",
     "nonce": "defaultNonce",
     "iat": 1507122303,
     "auth_time": 1507122303,
     "loyaltyNumber": "290",
     "given_name": "Emily",
     "emails": ["B2cdemo@outlook.com"]
   }
   ```

   >[!NOTE]
   >Hata iletisini alırsanız, *ad geçerli değil, lütfen geçerli bir ad sağlayın*. bu, Azure AD B2C istemci sertifikasını sunarken yeniden elde ettiğiniz hizmeti başarıyla çağırdı. Sonraki adım sertifikayı doğrulamaktır.

## <a name="step-6-add-certificate-validation"></a>6\. Adım: sertifika doğrulama ekleme
Azure AD B2C, yeniden yayımlanan hizmetinize gönderdiği istemci sertifikası, sertifikanın mevcut olup olmadığını kontrol etmek dışında Azure App Service platformu tarafından doğrulama yapmaz. Sertifikayı doğrulamak, Web uygulamasının sorumluluğundadır.

Bu bölümde, kimlik doğrulama amacıyla sertifika özelliklerini doğrulayan örnek ASP.NET kodu eklersiniz.

> [!NOTE]
>İstemci sertifikası kimlik doğrulaması için Azure App Service yapılandırma hakkında daha fazla bilgi için bkz. [Web Apps IÇIN TLS karşılıklı kimlik doğrulamasını yapılandırma](https://docs.microsoft.com/azure/app-service-web/app-service-web-configure-tls-mutual-auth).

### <a name="61-add-application-settings-to-your-projects-webconfig-file"></a>6,1 projenizin Web. config dosyasına uygulama ayarları ekleme
Daha önce oluşturduğunuz Visual Studio projesinde, `appSettings` öğeden sonra aşağıdaki uygulama ayarlarını *Web. config* dosyasına ekleyin:

```XML
<add key="ClientCertificate:Subject" value="CN=Subject name" />
<add key="ClientCertificate:Issuer" value="CN=Issuer name" />
<add key="ClientCertificate:Thumbprint" value="Certificate thumbprint" />
```

Sertifikanın **Konu adı**, **verenin adı**ve **sertifika parmak izi** değerlerini sertifika değerlerinizle değiştirin.

### <a name="62-add-the-isvalidclientcertificate-function"></a>6,2 IsValidClientCertificate işlevini ekleme
*Controllers\ıdentitycontroller.cs* dosyasını açın ve ardından `Identity` denetleyicisi sınıfına aşağıdaki işlevi ekleyin:

```csharp
private bool IsValidClientCertificate()
{
    string ClientCertificateSubject = ConfigurationManager.AppSettings["ClientCertificate:Subject"];
    string ClientCertificateIssuer = ConfigurationManager.AppSettings["ClientCertificate:Issuer"];
    string ClientCertificateThumbprint = ConfigurationManager.AppSettings["ClientCertificate:Thumbprint"];

    X509Certificate2 clientCertInRequest = RequestContext.ClientCertificate;
    if (clientCertInRequest == null)
    {
        Trace.WriteLine("Certificate is NULL");
        return false;
    }

    // Basic verification
    if (clientCertInRequest.Verify() == false)
    {
        Trace.TraceError("Basic verification failed");
        return false;
    }

    // 1. Check the time validity of the certificate
    if (DateTime.Compare(DateTime.Now, clientCertInRequest.NotBefore) < 0 ||
        DateTime.Compare(DateTime.Now, clientCertInRequest.NotAfter) > 0)
    {
        Trace.TraceError($"NotBefore '{clientCertInRequest.NotBefore}' or NotAfter '{clientCertInRequest.NotAfter}' not valid");
        return false;
    }

    // 2. Check the subject name of the certificate
    bool foundSubject = false;
    string[] certSubjectData = clientCertInRequest.Subject.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certSubjectData)
    {
        if (String.Compare(s.Trim(), ClientCertificateSubject) == 0)
        {
            foundSubject = true;
            break;
        }
    }

    if (!foundSubject)
    {
        Trace.TraceError($"Subject name '{clientCertInRequest.Subject}' is not valid");
        return false;
    }

    // 3. Check the issuer name of the certificate
    bool foundIssuerCN = false;
    string[] certIssuerData = clientCertInRequest.Issuer.Split(new char[] { ',' }, StringSplitOptions.RemoveEmptyEntries);
    foreach (string s in certIssuerData)
    {
        if (String.Compare(s.Trim(), ClientCertificateIssuer) == 0)
        {
            foundIssuerCN = true;
            break;
        }
    }

    if (!foundIssuerCN)
    {
        Trace.TraceError($"Issuer '{clientCertInRequest.Issuer}' is not valid");
        return false;
    }

    // 4. Check the thumbprint of the certificate
    if (String.Compare(clientCertInRequest.Thumbprint.Trim().ToUpper(), ClientCertificateThumbprint) != 0)
    {
        Trace.TraceError($"Thumbprint '{clientCertInRequest.Thumbprint.Trim().ToUpper()}' is not valid");
        return false;
    }

    // 5. If you also want to test whether the certificate chains to a trusted root authority, you can uncomment the following code:
    //
    //X509Chain certChain = new X509Chain();
    //certChain.Build(certificate);
    //bool isValidCertChain = true;
    //foreach (X509ChainElement chElement in certChain.ChainElements)
    //{
    //    if (!chElement.Certificate.Verify())
    //    {
    //        isValidCertChain = false;
    //        break;
    //    }
    //}
    //if (!isValidCertChain) return false;
    return true;
}
```

Önceki örnek kodda, sertifikayı yalnızca aşağıdaki koşulların tümü karşılanırsa geçerli olarak kabul ediyoruz:
* Sertifikanın süresi dolmadı ve sunucudaki geçerli zaman için etkin.
* Sertifikanın `Subject` adı `ClientCertificate:Subject` uygulama ayarı değerine eşittir.
* Sertifikanın `Issuer` adı `ClientCertificate:Issuer` uygulama ayarı değerine eşittir.
* Sertifikanın `thumbprint`, `ClientCertificate:Thumbprint` uygulama ayarı değerine eşittir.

>[!IMPORTANT]
>Hizmetinizin duyarlılığına bağlı olarak, daha fazla doğrulama eklemeniz gerekebilir. Örneğin, sertifikanın güvenilir bir kök yetkilisi, verenin kuruluş adı doğrulama vb. ile zincirinin yapılıp yapılmayacağını test etmeniz gerekebilir.

### <a name="63-call-the-isvalidclientcertificate-function"></a>6,3 IsValidClientCertificate işlevini çağırın
*Controllers\ıdentitycontroller.cs* dosyasını açın ve ardından `SignUp()` işlevinin başlangıcında aşağıdaki kod parçacığını ekleyin:

```csharp
if (IsValidClientCertificate() == false)
{
    return Content(HttpStatusCode.Conflict, new B2CResponseContent("Your client certificate is not valid", HttpStatusCode.Conflict));
}
```

Kod parçacığını ekledikten sonra, `Identity` denetleyicinizin aşağıdaki kod gibi görünmesi gerekir:

![Sertifika doğrulama kodu ekle](./media/secure-rest-api-dotnet-certificate-auth/rest-api-netfw-secure-client-code.png)

## <a name="step-7-publish-your-project-to-azure-and-test-it"></a>7\. Adım: projenizi Azure 'da yayımlayın ve test edin
1. **Çözüm Gezgini**, **contoso. AADB2C. API** projesine sağ tıklayın ve ardından **Yayımla**' yı seçin.

2. Sertifika doğrulaması ile özel ilkenizi yeniden test edin ve "6. adımı" tekrarlayın. İlkeyi çalıştırmayı deneyin ve doğrulama ekledikten sonra her şeyin çalıştığından emin olun.

3. *Web. config* dosyanızda `ClientCertificate:Subject` değerini **geçersiz**olarak değiştirin. İlkeyi yeniden çalıştırın ve bir hata mesajı görmeniz gerekir.

4. Değeri tekrar **geçerli**olarak değiştirin ve ilkenin REST API çağırada emin olun.

Bu adımda sorun gidermeniz gerekiyorsa, bkz. [Application Insights kullanarak günlükleri toplama](troubleshoot-with-application-insights.md).

## <a name="optional-download-the-complete-policy-files-and-code"></a>Seçim Tüm ilke dosyalarını ve kodunu indirin
* [Özel ilkeleri kullanmaya başlama talimatlarını](custom-policy-get-started.md) tamamladıktan sonra, kendi özel ilke dosyalarınızı kullanarak senaryonuzu oluşturmanızı öneririz. Başvurunuz için [örnek ilke dosyaları](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw-secure-cert)sağladık.
* [Başvuru Için örnek Visual Studio çözümünden](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-rest-api-netfw/Contoso.AADB2C.API)tüm kodu indirebilirsiniz.
