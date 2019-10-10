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
ms.date: 09/25/2019
ms.author: abpati
ms.custom: aaddev
ms.openlocfilehash: 241935afa023162a35559cd3c46206efa7a7835f
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72240174"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Hızlı başlangıç: Microsoft 'a Python web uygulamasına oturum açma ekleme

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

Bu hızlı başlangıçta, bir Python web uygulamasını Microsoft Identity platformu ile tümleştirmeyi öğreneceksiniz. Uygulamanız bir kullanıcıya oturum açacaktır, Microsoft Graph API 'sini çağırmak için bir erişim belirteci alır ve Microsoft Graph API 'sine bir istek yapar.

Kılavuzu tamamladığınızda, uygulamanız kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) ve Azure Active Directory kullanan herhangi bir şirketten veya kuruluştan iş veya okul hesapları için oturum açma işlemlerini kabul eder.

![Bu hızlı başlangıç tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-python-webapp/python-quickstart.svg)

## <a name="prerequisites"></a>Prerequisites

Bu örneği çalıştırmak için şunlar gerekir:

- [Python 2.7 +](https://www.python.org/downloads/release/python-2713) veya [Python 3 +](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-oturum](https://pythonhosted.org/Flask-Session/), [istekler](https://requests.kennethreitz.org//en/master/)
- [MSAL Python](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div renderon="docs"]
>
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydedin ve indirin
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: Express (seçenek 1) ve el ile (seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1\. seçenek: uygulamanızı kaydedin ve otomatik olarak yapılandırın ve ardından kod örneğinizi indirin
>
> 1. [Azure portal uygulama kayıtları](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**' i seçin.
> 1. Yeni uygulamanızı indirip otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2\. seçenek: uygulamanızı ve kod örneğinizi kaydetme ve el ile yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1\. Adım: Uygulamanızı kaydetme
>
> Uygulamanızı kaydetmek ve uygulamanın kayıt bilgilerini çözümünüze el ile eklemek için aşağıdaki adımları izleyin:
>
> 1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst köşede hesabınızı seçin ve Portal oturumunuzu istenen Azure AD kiracısı olarak ayarlayın.
> 1. Geliştiriciler için Microsoft Identity platformu [uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfasına gidin.
> 1. **Yeni kayıt**seçeneğini belirleyin.
> 1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
>      - **Ad** bölümünde, uygulamanın kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `python-webapp`.
>      - **Desteklenen hesap türleri**altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar**' ı seçin.
>      - **Yeniden yönlendirme URI 'si** bölümünde, açılan listede, **Web** platformunu seçin ve ardından değeri `http://localhost:5000/getAToken` olarak ayarlayın.
>      - **Kaydol**' u seçin. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. Sol taraftaki menüde **sertifikalar & gizlilikler** ' ı seçin ve **istemci** gizli dizileri bölümünde **yeni istemci parolası** ' na tıklayın:
>
>      - Bir anahtar açıklaması (örnek uygulama gizli anahtarı) yazın.
>      - **1 yılda**bir anahtar süresi seçin.
>      - **Ekle**' ye tıkladığınızda, anahtar değeri görüntülenecektir.
>      - Anahtarın değerini kopyalayın. Daha sonra ihtiyacınız olacak.
>
> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1\. Adım: uygulamanızı Azure portal yapılandırma
>
> Bu hızlı başlangıçta çalışması için kod örneği için şunları yapmanız gerekir:
>
> 1. @No__t-0 olarak bir yanıt URL 'SI ekleyin.
> 1. Bir Istemci gizli dizisi oluşturun.
>
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişikliği benim için yap]()
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Önceden yapılandırılmış @ no__t-1, uygulamanız Bu öznitelikle yapılandırılmış

#### <a name="step-2-download-your-project"></a>2\. Adım: projenizi Indirin

[Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

#### <a name="step-3-configure-the-application"></a>3\. Adım: uygulamayı yapılandırma

1. ZIP dosyasını kök klasöre (örneğin, **C:\Azure-Samples** ) yakın bir yerel klasöre ayıklayın.
1. Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği en sevdiğiniz IDE (isteğe bağlı) içinde açın.
1. Kök klasörde bulunan **app_config. Kopyala** dosyasını açın ve aşağıdaki kod parçacığı ile değiştirin:

```python
CLIENT_SECRET = "Enter_the_Client_Secret_Here"
CLIENT_ID = "Enter_the_Application_Id_here"
```

> [!div renderon="docs"]
> Konum:
>
> - `Enter_the_Application_Id_here`-kaydettiğiniz uygulamanın uygulama kimliğidir.
> - `Enter_the_Client_Secret_Here`- **sertifikalarında** oluşturduğunuz ve kaydettiğiniz uygulamanın gizli dizileri & **gizli anahtar olan istemci sırrı** .

#### <a name="step-4-run-the-code-sample"></a>4\. Adım: kod örneğini çalıştırma

1. Sunucu tarafı oturum yönetimi için MSAL Python kitaplığını, Flask çerçevesini, Flask oturumlarını ve PIP 'yi şu şekilde kullanarak istekleri yüklemeniz gerekir:

   ```Shell
   pip install -r requirements.txt
   ```

2. App.py from Shell veya komut satırından Çalıştır:

   ```Shell
   python app.py
   ```

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcılara oturum açma ve sonra Web API 'Lerini çağıran Web Apps hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Senaryo: kullanıcılar oturum açtığında Web Apps](scenario-web-app-sign-user-overview.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
