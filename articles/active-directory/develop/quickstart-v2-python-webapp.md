---
title: Microsoft kimlik platformu Python web uygulamasına Microsoft ile oturum açma ekleme | Azure
description: OAuth2'yi kullanarak Python Web Uygulamasında Microsoft Oturum Açma'yı nasıl uygulayacağınızı öğrenin
services: active-directory
author: abhidnya13
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 09/25/2019
ms.author: abpati
ms.custom: aaddev
ms.openlocfilehash: 34f0fb57b4432a8153f2cbaa8cb60edbb9a6f494
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78271079"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-python-web-app"></a>Quickstart: Python web uygulamasına Microsoft ile oturum açma ekleme

Bu hızlı başlangıçta, Python web uygulamasını Microsoft kimlik platformuyla nasıl tümleştireceğinizi öğreneceksiniz. Uygulamanız bir kullanıcıda oturum açacak, Microsoft Graph API'yi aramak için bir erişim jetonu alacak ve Microsoft Graph API'ye istekte bulunacaktır.

Kılavuzu tamamladığınızda, uygulamanız Azure Active Directory kullanan herhangi bir şirket veya kuruluşun kişisel Microsoft hesaplarının (outlook.com, live.com ve diğerleri dahil) oturum açmalarını ve iş veya okul hesaplarını kabul eder. (Bkz. örnek bir resim için [nasıl çalışır?)](#how-the-sample-works)


## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için şunları yapmanız gerekir:

- [Python 2.7+](https://www.python.org/downloads/release/python-2713) veya [Python 3+](https://www.python.org/downloads/release/python-364/)
- [Flask](http://flask.pocoo.org/), [Flask-Session](https://pythonhosted.org/Flask-Session/), [istekleri](https://requests.kennethreitz.org/en/master/)
- [MSAL Piton](https://github.com/AzureAD/microsoft-authentication-library-for-python)

> [!div renderon="docs"]
>
> ## <a name="register-and-download-your-quickstart-app"></a>Hızlı başlangıç uygulamanızı kaydetme ve indirme
>
> Hızlı başlangıç uygulamanızı başlatmak için iki seçeneğiniz vardır: express (Seçenek 1) ve manuel (Seçenek 2)
>
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>1. Seçenek: Uygulamanızı otomatik olarak kaydedip yapılandırın ve ardından kod örneğinizi indirin
>
> 1. Azure portalına gidin [- Uygulama kayıtları.](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)
> 1. **Yeni kayıt**seçin.
> 1. Uygulamanız için bir ad girin ve **Kaydet**'i seçin.
> 1. Yeni uygulamanızı indirmek ve otomatik olarak yapılandırmak için yönergeleri izleyin.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>2. Seçenek: Uygulamanızı ve kod örneğinizi el ile kaydetme ve yapılandırma
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> Uygulamanızı kaydetmek ve uygulama kayıt bilgilerinizi çözümünüze el ile eklemek için şu adımları izleyin:
>
> 1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
> 1. Hesabınız size birden fazla Azure AD kiracısına erişim sunuyorsa sağ üst köşeden hesabınızı seçin ve portal oturumunuzu istediğiniz Azure AD kiracısına ayarlayın.
> 1. Geliştiriciler için Microsoft kimlik platformuna gidin [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) sayfası.
> 1. **Yeni kayıt**seçin.
> 1. Bir uygulama sayfası **kaydedin,** başvurunuzun kayıt bilgilerini girin:
>      - **Ad** bölümüne, örneğin `python-webapp`uygulama kullanıcılarına görüntülenecek anlamlı bir uygulama adı girin.
>      - **Desteklenen hesap türleri**altında, tüm kuruluş **dizininde ve kişisel Microsoft hesaplarında Hesapları**seçin.
>      - **Uri'yi Yeniden Yönlendirme** bölümünde, açılan listede **Web** platformünü seçin ve değeri `http://localhost:5000/getAToken`.'ye ayarlayın.
>      - **Kaydol**’u seçin. Uygulamaya **Genel Bakış** sayfasında, daha sonra kullanmak üzere **Uygulama (istemci) kimlik** değerine dikkat edin.
> 1. Sol **menüde, Sertifikalar & sırları** seçin ve Müşteri **Sırları** bölümünde Yeni müşteri **sırrı** tıklayın:
>
>      - Anahtar açıklaması yazın (örnek uygulama gizli).
>      - **1 yıl içinde**önemli bir süre seçin.
>      - **Ekle'ye**tıkladığınızda, anahtar değeri görüntülenir.
>      - Anahtarın değerini kopyalayın. Buna daha sonra ihtiyacınız olacak.
> 1. **API izinleri** bölümünü seçin
>
>      - İzin **ekle** düğmesini tıklatın ve ardından,
>      - **Microsoft API'leri** sekmesinin seçildiğinden emin olun
>      - Yaygın *olarak kullanılan Microsoft API'leri* bölümünde, **Microsoft Graph'ı** tıklatın
>      - Temsilci **verilen izinler** bölümünde, doğru izinlerin kontrol edildiğinden emin olun: **User.ReadBasic.All**. Gerekirse arama kutusunu kullanın.
>      - İzin **Ekle** düğmesini seçin
>
> [!div class="sxs-lookup" renderon="portal"]
>
> #### <a name="step-1-configure-your-application-in-azure-portal"></a>1. Adım: Uygulamanızı Azure portalında yapılandırma
>
> Bu hızlı başlatmanın işe yaraması için kod örneğinin çalışması için şunları yapmanız gerekir:
>
> 1. Yanıt URL'si '' olarak `http://localhost:5000/getAToken`ekleyin.
> 1. Bir Müşteri Sırrı oluşturun.
> 1. Microsoft Graph API'nin User.ReadBasic.All delege iznini ekleyin.
>
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Bu değişiklikleri benim için yapın]()
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Zaten yapılandırılmış](media/quickstart-v2-aspnet-webapp/green-check.png) Uygulamanız bu öznitelikle yapılandırılmış

#### <a name="step-2-download-your-project"></a>2. Adım: Projenizi indirme
> [!div renderon="docs"]
> [Kod Örneğini İndir](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!div class="sxs-lookup" renderon="portal"]
> Projeyi karşıdan yükleyin ve zip dosyasını kök klasörüne daha yakın yerel bir klasöre ayıklayın - örneğin, **C:\Azure-Örnekler**
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-python-webapp/archive/master.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-the-application"></a>Adım 3: Uygulamayı Yapılandır
> 
> 1. Zip dosyasını kök klasöre yakın bir yerel klasöre (örneğin **C:\Azure-Samples**) açın
> 1. Tümleşik bir geliştirme ortamı kullanıyorsanız, örneği favori IDE'nizde açın (isteğe bağlı).
> 1. Kök klasöründe bulunabilen **app_config.py** dosyasını açın ve aşağıdaki kod parçacığı yla değiştirin:
> 
> ```python
> CLIENT_ID = "Enter_the_Application_Id_here"
> CLIENT_SECRET = "Enter_the_Client_Secret_Here"
> AUTHORITY = "https://login.microsoftonline.com/Enter_the_Tenant_Name_Here"
> ```
> Konumlar:
>
> - `Enter_the_Application_Id_here` - Kaydettiğiniz uygulamanın Uygulama Kimliği değeridir.
> - `Enter_the_Client_Secret_Here`- Kayıtlı olduğunuz uygulama için **Sertifikalar & Sırlar'da** oluşturduğunuz **Müşteri Sırrıdır.**
> - `Enter_the_Tenant_Name_Here`- Kaydettiğiniz uygulamanın **Dizin (kiracı) kimlik** değeridir.

> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-run-the-code-sample"></a>Adım 3: Kod örneğini çalıştırma

> [!div renderon="docs"]
> #### <a name="step-4-run-the-code-sample"></a>Adım 4: Kod örneğini çalıştırma

1. MSAL Python kitaplığı, Flask çerçevesi, sunucu tarafı oturum yönetimi için Flask-Sessions ve pip kullanarak istekler aşağıdaki gibi yüklemeniz gerekir:

    ```Shell
    pip install -r requirements.txt
    ```

2. Kabuk veya komut satırından app.py çalıştırın:

    ```Shell
    python app.py
    ```
   > [!IMPORTANT]
   > Bu hızlı başlatma uygulaması, kendisini gizli istemci olarak tanımlamak için bir istemci sırrı kullanır. İstemci sırrı, güvenlik nedenleriyle proje dosyalarınıza düz metin olarak eklenmiştir, çünkü uygulamayı üretim uygulaması olarak düşünmeden önce istemci sırrı yerine bir sertifika kullanmanız önerilir. Sertifikanın nasıl kullanılacağı hakkında daha fazla bilgi için [şu talimatlara](https://docs.microsoft.com/azure/active-directory/develop/active-directory-certificate-credentials)bakın.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örnek nasıl çalışır?
![Bu hızlı başlatma tarafından oluşturulan örnek uygulamanın nasıl çalıştığını gösterir](media/quickstart-v2-python-webapp/python-quickstart.svg)

### <a name="getting-msal"></a>MSAL Alma
MSAL, kullanıcıları oturum ve microsoft kimlik platformu tarafından korunan bir API'ye erişmek için kullanılan belirteçleri istemek için kullanılan kitaplıktır.
Pip'i kullanarak uygulamanıza MSAL Python ekleyebilirsiniz.

```Shell
pip install msal
```

### <a name="msal-initialization"></a>MSAL başlatma
MSAL'ı kullanacakdosyanın üst bölümüne aşağıdaki kodu ekleyerek MSAL Python'a başvuruyu ekleyebilirsiniz:

```Python
import msal
```

## <a name="next-steps"></a>Sonraki adımlar

Oturum açan ve ardından web API'lerini çağıran web uygulamaları hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Senaryo: Kullanıcıları oturum açan Web uygulamaları](scenario-web-app-sign-user-overview.md)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
