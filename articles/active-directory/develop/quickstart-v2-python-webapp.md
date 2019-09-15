---
title: Microsoft Identity platform Python web uygulaması hızlı başlangıç | Mavisi
description: OAuth2 kullanarak bir Python web uygulamasında Microsoft oturum açma uygulamasını nasıl uygulayacağınızı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: abhidnya13
editor: ''
ms.assetid: 9551f0b5-04f2-44d7-87b5-756409180fe9
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/11/2019
ms.author: abpati
ms.custom: aaddev
ms.openlocfilehash: 682582c8c695550f7dfdfcc079e1d0bf04828180
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997379"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Hızlı Başlangıç: Microsoft 'a Python web uygulamasına oturum açma ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, bir Python web uygulamasını Microsoft Identity platformu ile tümleştirmeyi öğreneceksiniz. Uygulamanız bir kullanıcıya oturum açacaktır, Microsoft Graph API 'sini çağırmak için bir erişim belirteci alır ve Microsoft Graph API 'sine bir istek yapar.

Kılavuzu tamamladığınızda, uygulamanız kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) ve Azure Active Directory kullanan herhangi bir şirketten veya kuruluştan iş veya okul hesapları için oturum açma işlemlerini kabul eder.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-python-webapp/python-quickstart.svg)

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için şunlar gerekir:

- [Python 2.7 +](https://www.python.org/downloads/release/python-2713) veya [Python 3 +](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-oturum](https:/pythonhosted.org/Flask-Session/), [istekler](https://2.python-requests.org/en/master/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python) 
- Azure Active Directory (Azure AD) kiracısı. Azure AD kiracısı alma hakkında daha fazla bilgi için bkz [. Azure AD kiracısı alma.](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

> [!div renderon="docs"]
>
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (seçenek 1) ve el ile (seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Seçenek 1: Uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure portal uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Seçenek 2: Uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. adım: Uygulamanızı kaydedin
>
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
>      - **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `python-webapp`.
>      - **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
>      - **Yeniden yönlendirme URI 'si** bölümünde, açılan listede, **Web** platformunu seçin ve değerini olarak `http://localhost:5000/getAToken`ayarlayın.
>      - **Kaydol**’u seçin. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. Sol taraftaki menüde **sertifikalar & gizlilikler** ' ı seçin ve **istemci** gizli dizileri bölümünde **yeni istemci parolası** ' na tıklayın:
>
>      - Bir anahtar açıklaması (örnek uygulama gizli anahtarı) yazın.
>      - **1 yılda**bir anahtar süresi seçin.
>      - **Ekle**' ye tıkladığınızda, anahtar değeri görüntülenecektir.
>      - Anahtarın değerini kopyalayın. Buna daha sonra ihtiyacınız olacak.
>
> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1\. adım: Uygulamanızı Azure portal yapılandırma
>
> Bu hızlı başlangıçta çalışması için kod örneği için şunları yapmanız gerekir:
>
> 1. Olarak `http://localhost:5000/getAToken`bir yanıt URL 'si ekleyin.
> 1. Bir Istemci gizli dizisi oluşturun.
>
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu özellikle yapılandırıldı

#### <a name="step-2-download-your-project"></a>2\. adım: Projenizi indirin

[Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

#### <a name="step-3-configure-the-application"></a>3\. adım: Uygulamayı yapılandırma

1. Zip dosyasını kök klasöre yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) açın
1. Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği en sevdiğiniz IDE (isteğe bağlı) içinde açın.
1. Kök klasörde bulunan **app_config. Kopyala** dosyasını açın ve aşağıdaki kod parçacığı ile değiştirin:

```python
AUTHORITY = "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here"
CLIENT_ID = "Enter_the_Application_Id_here"
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
SCOPE = ["https://graph.microsoft.com/User.Read"]
REDIRECT_URI = "http://localhost:5000/getAToken"
```

> [!div renderon="docs"]
> Konumlar:
>
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
> - `Enter_the_Tenant_Info_Here` - Aşağıdaki seçeneklerden biridir:
>   - Uygulamanız **yalnızca Kuruluşumu**destekliyorsa, bu değeri **Kiracı kimliği** veya **kiracı adıyla** değiştirin (örneğin, contoso.onmicrosoft.com)
>   - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
>   - Uygulamanız **Tüm Microsoft hesabı kullanıcıları** yaklaşımını destekliyorsa bu değeri `common` ile değiştirin
> - `Enter_the_Client_Secret_Here`-Sertifikalar 'da oluşturduğunuz **Istemci gizli anahtarı** , kaydettiğiniz uygulamanın **gizli dizileri &** .

#### <a name="step-4-run-the-code-sample"></a>4\. Adım: Kod örneğini çalıştırma

- Sunucu tarafı oturum yönetimi için MSAL Python kitaplığını, Flask çerçevesini, Flask oturumlarını ve PIP 'yi şu şekilde kullanarak istekleri yüklemeniz gerekir:

```Shell
pip install msal
pip install flask
pip install Flask-Session
pip install requests
```

- Flask için ortam değişkeni zaten ayarlandıysa: App.py from Shell veya komut satırından Çalıştır:

```Shell
python app.py
```

- Flask için ortam değişkeni ayarlanmamışsa:

    1. Aşağıdaki komutları, proje dizinine giderek Shell veya komut satırına yazın:

```Shell
export FLASK_APP=app.py
export FLASK_DEBUG=1
flask run
```

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
