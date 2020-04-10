---
title: Microsoft kimlik platformu Python daemon | Azure
description: Python işleminin nasıl erişim belirteci alabildiğini ve uygulamanın kendi kimliğini kullanarak Microsoft kimlik platformu bitiş noktası tarafından korunan bir API'yi nasıl arayabildiğini öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/22/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:Python
ms.openlocfilehash: ecbed58eabd2e835d8fa202916829d1da91210e7
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991067"
---
# <a name="quickstart-acquire-a-token-and-call-microsoft-graph-api-from-a-python-console-app-using-apps-identity"></a>Quickstart: Uygulamanın kimliğini kullanarak bir Python konsol uygulamasından bir belirteç edinin ve Microsoft Graph API'yi arayın

Bu hızlı başlangıçta, uygulamanın kimliğini kullanarak erişim belirteci alan bir Python uygulaması yazın ve ardından dizindeki [kullanıcıların listesini](https://docs.microsoft.com/graph/api/user-list) görüntülemek için Microsoft Graph API'sini çağırın. Bu senaryo, başsız, gözetimsiz iş veya windows hizmetinin kullanıcı kimliği yerine bir uygulama kimliğiyle çalışması gereken durumlar için yararlıdır.

> [!div renderon="docs"]
> ![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-netcore-daemon/netcore-daemon-intro.svg)

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için şunları yapmanız gerekir:

- [Python 2.7+](https://www.python.org/downloads/release/python-2713) veya [Python 3+](https://www.python.org/downloads/release/python-364/)
- [MSAL Piton](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme

> [!div renderon="docs" class="sxs-lookup"]
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (Aşağıdaki Seçenek 1) ve Manuel (Seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Yeni Azure [portalı - Uygulama kayıtları](https://portal.azure.com/?Microsoft_AAD_RegisteredApps=true#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/PythonDaemonQuickstartPage/sourceType/docs) bölmesine gidin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yönergeleri izleyerek yeni uygulamanızı yalnızca tek tıklamayla indirin ve otomatik olarak yapılandırın.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma

> [!div renderon="docs"]
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir başvuru sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin. 
> 1. **Ad** bölümüne, uygulama nın kullanıcılarına görüntülenecek anlamlı bir uygulama `Daemon-console`adı girin, örneğin , uygulamayı oluşturmak için **Kaydol'u** seçin.
> 1. Kaydedildikten sonra **Sertifikalar & Sırlar** menüsünü seçin.
> 1. **İstemci sırları**altında , seçin **+ Yeni istemci gizli**. Bir ad verin ve **Ekle'yi**seçin. Sırrı güvenli bir yerde kopyalayın. Kodunuzda kullanmak için ihtiyacınız olacaktır.
> 1. Şimdi, **API İzinleri** menüsünü seçin, **+ İzin düğmesi ekle'yi** seçin, **Microsoft Graph'ı**seçin.
> 1. **Uygulama izinlerini**seçin.
> 1. **Kullanıcı** düğümü altında **User.Read.All'ı**seçin ve **ardından İzin Ekle'yi** seçin

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="download-and-configure-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı indirin ve yapılandırın
> 
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
> Bu hızlı başlatmanın işe yaraması için bir istemci sırrı oluşturmanız ve Grafik API'nin **User.Read.All** uygulama iznini eklemeniz gerekir.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-netcore-daemon/green-check.png) Uygulamanız bu özniteliklerle yapılandırılmış.

#### <a name="step-2-download-your-python-project"></a>Adım 2: Python projenizi indirin

> [!div renderon="docs"]
> [Python daemon projesini indirin](https://github.com/Azure-Samples/ms-identity-python-daemon/archive/master.zip)

> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-daemon/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`


> [!div renderon="docs"]
> #### <a name="step-3-configure-your-python-project"></a>Adım 3: Python projenizi yapılandırın
> 
> 1. Zip dosyasını diskin köküne yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) ayıklayın.
> 1. Alt klasöre gidin **1-Call-MsGraph-WithSecret"**.
> 1. **Parametreleri.json'u** ve alanların `authority` `client_id`değerlerini ve `secret` aşağıdaki parçacıkları değiştirin:
>
>    ```json
>    "authority": "https://login.microsoftonline.com/Enter_the_Tenant_Id_Here",
>    "client_id": "Enter_the_Application_Id_Here",
>    "secret": "Enter_the_Client_Secret_Here"
>    ```
>    Konumlar:
>    - `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
>    - `Enter_the_Tenant_Id_Here`- Bu değeri **Kiracı Kimliği** veya **Kiracı adı** ile değiştirin (örneğin, contoso.microsoft.com)
>    - `Enter_the_Client_Secret_Here`- Bu değeri adım 1'de oluşturulan istemci sırrıyla değiştirin.
>
> > [!TIP]
> > **Uygulama (istemci) kimliği**, **Dizin (kiracı) Kimliği**değerlerini bulmak için, Azure portalındaki uygulamanın Genel **Bakış** sayfasına gidin. Yeni bir anahtar oluşturmak için **Sertifikalar & Sırlar** sayfasına gidin.
    
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-admin-consent"></a>Adım 3: Yönetici onayı

> [!div renderon="docs"]
> #### <a name="step-4-admin-consent"></a>Adım 4: Yönetici onayı

Bu noktada uygulamayı çalıştırmayı denerseniz, *HTTP 403 -* Yasak `Insufficient privileges to complete the operation`hata: . Bu hata, *yalnızca uygulama izni* için Yönetici onayı gerektirdiğinden olur: dizininizin genel yöneticisi nin uygulamanıza onay vermesi gerekir. Rolünüze bağlı olarak aşağıdaki seçeneklerden birini seçin:

##### <a name="global-tenant-administrator"></a>Genel kiracı yöneticisi

> [!div renderon="docs"]
> Genel bir kiracı yöneticiyseniz, Azure Portalı'nın Uygulama Kaydı'ndaki **(Önizleme) API İzinleri** sayfasına gidin ve **{Kiracı Adı} ({Kiracı Adı}** dizininizin adı için Hibe yöneticisi onayı seçin.

> [!div renderon="portal" class="sxs-lookup"]
> Genel bir yöneticiyseniz, **API İzinleri** sayfasına gidin **Enter_the_Tenant_Name_Here için Grant yönetici onayı** seçin
> > [!div id="apipermissionspage"]
> > [API İzinleri sayfasına gidin]()

##### <a name="standard-user"></a>Standart kullanıcı

Kiracınızın standart bir kullanıcısıysanız, genel bir yöneticiden uygulamanız için yönetici onayı vermesini istemeniz gerekir. Bunu yapmak için yöneticinize aşağıdaki URL'yi verin:

```url
https://login.microsoftonline.com/Enter_the_Tenant_Id_Here/adminconsent?client_id=Enter_the_Application_Id_Here
```

> [!div renderon="docs"]
>> Konumlar:
>> * `Enter_the_Tenant_Id_Here`- Bu değeri **Kiracı Kimliği** veya **Kiracı adı** ile değiştirin (örneğin, contoso.microsoft.com)
>> * `Enter_the_Application_Id_Here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-4-run-the-application"></a>Adım 4: Uygulamayı çalıştırın

> [!div renderon="docs"]
> #### <a name="step-5-run-the-application"></a>Adım 5: Uygulamayı çalıştırın

Bu örneğin bağımlılıklarını bir kez yüklemeniz gerekir

```console
pip install -r requirements.txt
```

Ardından, uygulamayı komut istemi veya konsol aracılığıyla çalıştırın:

```console
python confidential_client_secret_sample.py parameters.json
```

Konsolçıktısında Azure AD dizininizdeki kullanıcıların listesini temsil eden bazı Json parçasını görmeniz gerekir.

> [!IMPORTANT]
> Bu hızlı başlatma uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci sırrı kullanır. İstemci sırrı, güvenlik nedenleriyle proje dosyalarınıza düz metin olarak eklenmiştir, çünkü uygulamayı üretim uygulaması olarak düşünmeden önce istemci sırrı yerine bir sertifika kullanmanız önerilir. Sertifikanın nasıl kullanılacağı hakkında daha fazla bilgi için, [bu örnek](https://github.com/Azure-Samples/ms-identity-python-daemon/blob/master/2-Call-MsGraph-WithCertificate/README.md) için aynı GitHub deposunda, ancak ikinci klasörde **2-Call-MsGraph-WithCertificate**

## <a name="more-information"></a>Daha fazla bilgi

### <a name="msal-python"></a>MSAL Piton

[MSAL Python,](https://github.com/AzureAD/microsoft-authentication-library-for-python) kullanıcıları oturum ve microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteçleri oturum açmada kullanılan kitaplıktır. Açıklandığı gibi, bu hızlı başlatma, yetkin izinler yerine uygulamanın kendi kimliğini kullanarak belirteçleri ister. Bu durumda kullanılan kimlik doğrulama akışı *[istemci kimlik bilgileri oauth akışı](v2-oauth2-client-creds-grant-flow.md)* olarak bilinir. Daemon uygulamaları ile MSAL Python'un nasıl kullanılacağı hakkında daha fazla bilgi için [bu makaleye](scenario-daemon-overview.md)bakın.

 Aşağıdaki pip komutunu çalıştırarak MSAL Python'u yükleyebilirsiniz.

```powershell
pip install msal
```

### <a name="msal-initialization"></a>MSAL başlatma

Şu kodu ekleyerek MSAL başvurusunu ekleyebilirsiniz:

```Python
import msal
```

Sonra şu kodu kullanarak MSAL'yi başlatın:

```Python
app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"])
```

> | Konumlar: ||
> |---------|---------|
> | `config["secret"]` | Azure Portalı'ndaki uygulama için oluşturulan istemci sırrıdır. |
> | `config["client_id"]` | **Uygulama (istemci) Kimliği**, Azure portalda kayıtlı uygulamadır. Bu değeri Azure portalda uygulamanın **Genel bakış** sayfasında bulabilirsiniz. |
> | `config["authority"]`    | Kimlik doğrulaması yapılacak kullanıcı için STS uç noktası. Genellikle <https://login.microsoftonline.com/{tenant}> ,{kiracı} kiracınızın veya kiracı Kimliğinizin adı olan genel bulut için.|

Daha fazla bilgi için lütfen [başvuru belgelerine `ConfidentialClientApplication` ](https://msal-python.readthedocs.io/en/latest/#confidentialclientapplication) bakın

### <a name="requesting-tokens"></a>Belirteç isteme

Uygulamanın kimliğini kullanarak belirteç istemek `AcquireTokenForClient` için şu yöntemi kullanın:

```Python
result = None
result = app.acquire_token_silent(config["scope"], account=None)

if not result:
    logging.info("No suitable token exists in cache. Let's get a new one from AAD.")
    result = app.acquire_token_for_client(scopes=config["scope"])
```

> |Konumlar:| |
> |---------|---------|
> | `config["scope"]` | İstenilen kapsamları içerir. Gizli istemciler için bu biçim, `{Application ID URI}/.default` istenen kapsamların Azure Portalı'nda ayarlanan uygulama nesnesinde statik olarak tanımlanan kapsamlar `{Application ID URI}` olduğunu `https://graph.microsoft.com`belirtmek için benzer biçimi kullanmalıdır (Microsoft Graph için, işaret eder). Özel Web API'leri için, `{Application ID URI}` Azure Portal'ın Uygulama Kaydı'ndaki (Önizleme) **api açığa** çıkarma bölümü altında tanımlanır. |

Daha fazla bilgi için lütfen [başvuru belgelerine `AcquireTokenForClient` ](https://msal-python.readthedocs.io/en/latest/#msal.ConfidentialClientApplication.acquire_token_for_client) bakın

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daemon uygulamaları hakkında daha fazla bilgi edinmek için senaryo açılış sayfasına bakın

> [!div class="nextstepaction"]
> [Web API'leri çağıran Daemon uygulaması](scenario-daemon-overview.md)

Daemon uygulama öğreticisi için bkz:

> [!div class="nextstepaction"]
> [Daemon Python konsol eğitimi](https://github.com/Azure-Samples/ms-identity-python-daemon)

İzinler ve izinler hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [İzinler ve İzinler](v2-permissions-and-consent.md)

Bu senaryonun auth akışı hakkında daha fazla bilgi için Oauth 2.0 istemci kimlik bilgileri akışına bakın:

> [!div class="nextstepaction"]
> [İstemci kimlik bilgileri Oauth akışı](v2-oauth2-client-creds-grant-flow.md)
