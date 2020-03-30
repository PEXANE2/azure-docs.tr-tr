---
title: Azure Active Directory B2C ve Azure API Yönetimi'ni kullanarak SPA arka ucunu OAuth 2.0 ile koruyun.
description: Bir JavaScript SPA'dan çağrılmak üzere Azure Active Directory B2C, Azure API Yönetimi ve Easy Auth'u kullanarak Bir API'yi OAuth 2.0 ile koruyun.
services: api-management, azure-ad-b2c, app-service
documentationcenter: ''
author: WillEastbury
manager: alberts
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2020
ms.author: wieastbu
ms.custom: fasttrack-new
ms.openlocfilehash: 55acea360de11c5fcc699d65daf92cf24dfd691d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475485"
---
# <a name="protect-spa-backend-with-oauth-20-azure-active-directory-b2c-and-azure-api-management"></a>OAuth 2.0, Azure Active Directory B2C ve Azure API Yönetimi ile SPA arka ucunu koruyun

Bu senaryo, bir API'yi korumak için Azure API Yönetimi örneğini nasıl yapılandırabileceğinizi gösterir.
EasyAuth kullanarak Bir Azure İşlevlerini arka ucunu güvence altına almak için API Yönetimi ile birlikte Azure AD B2C ile OAuth 2.0 protokolünü kullanacağız.

## <a name="aims"></a>Amaçları
Azure İşlevler ve Azure AD B2C ile API Yönetimi'nin basitleştirilmiş bir senaryoda nasıl kullanılabileceğini göreceğiz. Azure AD B2C'li kullanıcılarda işaretleyen api çağıran bir JavaScript (JS) uygulaması oluşturursunuz. Ardından, Arka Uç API'sini korumak için API Management'ın doğrulama-jwt ilkesi özelliklerini kullanırsınız.

Derinlemesine savunma için, daha sonra arka uç API içinde tekrar belirteç doğrulamak için EasyAuth kullanın.

## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki adımları izlemek için şunları yapmış olmalısınız:
* Ön uç JS Tek Sayfa uygulamasına ev sahipliği yapmak için bir Azure (StorageV2) Genel Amaçlı V2 Depolama Hesabı
* Azure API Yönetimi örneği 
* Çağrılan API'yi barındırmak için boş bir Azure İşlevi uygulaması (Windows Tüketim Planı'nda V2 .NET Core çalışma süresini çalıştırıyor)
* Aboneleğe bağlı bir Azure AD B2C kiracısı 

Uygulamada aynı bölgedeki kaynakları üretim iş yüklerinde kullanmanıza rağmen, bu nasıl yapılan makale için dağıtım bölgesi önemli değildir.

## <a name="overview"></a>Genel Bakış
Burada kullanılan bileşenlerin bir örneği ve bu işlem tamamlandıktan sonra aralarındaki akış.
![Kullanılan ve akışta olan bileşenler](../api-management/media/howto-protect-backend-frontend-azure-ad-b2c/image-arch.png "Kullanılan ve akışta olan bileşenler")

Adımların hızlı bir özeti aşağıda verebilirsiniz:

1. Kapsamları olan Azure AD B2C Arama (Frontend, API Yönetimi) ve API Uygulamaları oluşturun ve API Erişimi verir
1. Kullanıcıların Azure AD B2C ile oturum açmalarına olanak sağlamak için oturum açma veya oturum açma ilkeleri oluşturun 
1. Geliştirici Konsolunda OAuth2 kullanıcı yetkilendirmesini etkinleştirmek için API Yönetimini yeni Azure AD B2C İstemci işleri ve anahtarlarıyla yapım
1. İşlev API'sini oluşturma
1. İşlev API'sini easyAuth'u yeni Azure AD B2C İstemci Kimliği ve Anahtarları ile etkinleştirecek şekilde yapılandırın ve APIM VIP'ye kilitleyin 
1. API Yönetiminde API Tanımını Oluşturma
1. API Yönetimi API yapılandırması için Oauth2'yi ayarlama
1. **CORS** ilkesini ayarlayın ve gelen her istek için OAuth belirteci doğrulamak için **doğrulama-jwt** ilkesini ekleyin
1. API'yi tüketmek için arama uygulaması oluşturma
1. JS SPA Örneğini Yükleyin
1. Örnek JS İstemci Uygulamasını yeni Azure AD B2C İstemci Kimliği ve anahtarlarıyla yapılandırın 
1. İstemci Uygulamasını Test Edin

## <a name="configure-azure-ad-b2c"></a>Azure AD B2C'yi yapılandırma 
Portaldaki Azure AD B2C bıçağını açın ve aşağıdaki adımları yapın.
1. **Uygulamalar** sekmesini seçin 
1. 'Ekle' düğmesini tıklatın ve aşağıdaki amaçlar için üç uygulama oluşturun
   * Frontend Müşterisi.
   * Arka uç işlevi API'si.
   * [İsteğe bağlı] API Yönetimi geliştirici portalı (tüketim katmanında Azure API Yönetimi'ni çalıştırdığınız sürece, daha sonra bu senaryohakkında daha fazla şey bulabilirsiniz).
1. Tüm 3 uygulama için WebApp / Web API'yi ayarlayın ve yalnızca Frontend İstemci için 'Örtülü akışa izin verin'i evet olarak ayarlayın.
1. Şimdi App ID URI'yi ayarlayın, oluşturulan hizmetle ilgili benzersiz ve alakalı bir şey seçin.
1. Şimdilik yanıt urlleri için yer tutucular kullanın, https://localhostbu url'leri daha sonra güncelleştireceğiz.
1. Yukarıdaki üç uygulamanın her biri için 'Oluştur'u tıklatın ve ardından yukarıdaki üç uygulamanın her biri için 2-5 adımlarını tekrarlayın ve üç uygulama için daha sonra kullanmak üzere AppID URI, ad ve Uygulama Kimliği'ni kaydedin.
1. UYGULAMA listesinden API Yönetimi Geliştirici Portalı Uygulamasını açın ve *Anahtarlar* sekmesini seçin (Genel altında) ardından auth tuşu oluşturmak için 'Anahtar Oluştur'u tıklatın
1. Kaydet'i tıklattıktan sonra, anahtarı daha sonra kullanmak için güvenli bir yere kaydedin - bu yerin bu anahtarı görüntülemek ve kopyalamak için tek şans olduğunu unutmayın.
1. Şimdi *Yayımlanmış Kapsamlar* Sekmesini seçin (API Erişimi altında)
1. İşlev API'niz için bir kapsam oluşturun ve adlandırın ve Kapsam ve doldurulan Tam Kapsam Değerini kaydedin ve ardından 'Kaydet'i tıklatın.
   > [!NOTE]
   > Azure AD B2C kapsamları, API'nizde, diğer uygulamaların uygulamalarından API erişim bıçağı aracılığıyla erişim isteyebileceği etkin izinlerdir ve etkili bir şekilde, çağrılan API'niz için uygulama izinleri oluşturdunuz.
1. Diğer iki uygulamayı açın ve ardından *API Access* sekmesinin altına bakın.
1. Onlara arka uç API kapsamına ve zaten orada olan varsayılan kapsamına erişim izni ver ("Kullanıcının profiline erişin").
1. Auth tuşu oluşturmak ve bu anahtarları daha sonra güvenli bir yere kaydetmek için 'General' altındaki *Keys* sekmesini seçerek her biri için bir anahtar oluşturun.

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>"Kaydol veya Kaydol" kullanıcı akışı oluşturma
1. Azure AD B2C Blade'in köküne (Veya 'Genel Bakış'a' dön 
1. Ardından "Kullanıcı Akışları (İlkeler)" seçeneğini belirleyin ve "Yeni kullanıcı akışı"na tıklayın
1. 'Kaydolun ve oturum açın' kullanıcı akış türünü seçin
1. İlkeye bir ad verin ve daha sonra kaydedin.
1. Ardından 'Kimlik sağlayıcıları' altında 'Kullanıcı Kimliği kaydolun' seçeneğini işaretleyin (bu durum 'E-posta yla kaydol' olarak dalanabilir) ve Tamam'ı tıklatın. 
1. 'Kullanıcı Öznitelikleri ve talepleri' altında 'Daha Fazlasını Göster...' seçeneğini tıklayın. ardından, kullanıcılarınızın girmesini istediğiniz ve belirteciolarak iade edilen talep seçeneklerini seçin. Toplamak ve geri dönmek için en az 'Görüntüadı' ve 'E-posta Adresi'ni işaretleyin ve 'Tamam'ı tıklatın ve ardından 'Oluştur'u tıklatın.
1. Listede oluşturduğunuz ilkeyi seçin ve ardından 'Kullanıcı akışını çalıştır' düğmesini tıklatın.
1. Bu eylem çalıştırılabilen kullanıcı akış bıçağını açar, ön uç uygulamasını seçer ve ardından açılan b2clogin.com etki alanının adresini 'Etki alanı seçin' için açılır alan adı altında kaydeder.
1. 'İyi bilinen openid yapılandırma bitiş noktasını' açmak için üstteki bağlantıyı tıklatın ve authorization_endpoint ve token_endpoint değerlerinin yanı sıra bağlantının değerini iyi bilinen openid yapılandırma bitiş noktası olarak kaydedin.

   > [!NOTE]
   > B2C İlkeleri, farklı veri bileşenlerini yakalayabilmek ve kullanıcıları farklı şekillerde oturum açabilmek için Azure AD B2C giriş uç noktalarını ortaya çıkarmanızı sağlar. Bu durumda, iyi bilinen bir yapılandırma bitiş noktasını ortaya çıkaran bir kayıt veya oturum açma bitiş noktasını yapılandırdık, özellikle oluşturulan politikamız URL'de p= parametresi tarafından tanımlandı.
   > 
   > Bu yapıldıktan sonra , artık birden fazla uygulama içine kullanıcıları imzalayacak işlevsel bir İş tüketici kimlik platformu var. 
   > İsterseniz burada 'Kullanıcı akışını çalıştır' düğmesini tıklatabilir (kaydolma veya oturum açma işleminden geçmek için) ve uygulamada ne yapacağını anlayabilir, ancak uygulama henüz dağıtılmadı gibi sonunda yeniden yönlendirme adımı başarısız olacaktır.

## <a name="build-the-function-api"></a>İşlev API'sini oluşturun
1. Aboneliğinizdeki öğeleri yeniden yapılandırabilmemiz için Azure portalındaki standart Azure AD kiracınıza geri dön 
1. Azure portalının İşlev Uygulamaları bıçağına gidin, boş işlev uygulamanızı açın ve hızlı başlangıç yoluyla yeni bir Portal Içi 'Webhook + API' işlevi oluşturun.
1. Örnek kodu aşağıdan Run.csx'e görünen varolan kodun üzerine yapıştırın.

   ```csharp
   
   using System.Net;
   using Microsoft.AspNetCore.Mvc;
   using Microsoft.Extensions.Primitives;
   
   public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
   {
      log.LogInformation("C# HTTP trigger function processed a request.");
      
      return (ActionResult)new OkObjectResult($"Hello World, time and date are {DateTime.Now.ToString()}");
   }
   
   ```

   > [!NOTE]
   > Az önce yapıştırdığınız c# komut dosyası işlev kodu, bir satırı işlev günlüklerine kaydeder ve bazı dinamik verilerle (tarih ve saat) "Hello World" metnini döndürür.

3. Sol bıçaktan "Tümleştir"i seçin ve bölmenin sağ üst köşesindeki "Gelişmiş Düzenleyici"yi seçin.
4. Aşağıdaki örnek kodu varolan json'un üzerine yapıştırın.

   ```json
   {
      "bindings": [
       {
        "authLevel": "function",
        "name": "req",
        "type": "httpTrigger",
        "direction": "in",
        "methods": [
           "get"
        ],
        "route": "hello"
       },
       {
         "name": "$return",
         "type": "http",
         "direction": "out"
       }
     ]
   }
   ```

5. HttpTrigger1 sekmesine geri dön, 'İşlev URL'sini Al'ı tıklatın ve ardından görünen URL'yi kopyalayın.

   > [!NOTE]
   > Az önce oluşturduğunuz ciltler, Fonksiyonlar'a anonim http GET isteklerini kopyaladığınız URL'ye yanıtlamalarını söylemeniz yeterlidir. (https://yourfunctionappname.azurewebsites.net/api/hello?code=secretkey) Şimdi çok basit bir yük döndürme yeteneğine sahip bir ölçeklenebilir sunucusuz https API var.
   > Artık yukarıdaki URL'yi kullanarak bu API'yi bir web tarayıcısından çağırmayı test edebilir, URL'nin ?code=secret bölümünü de silebilir ve Azure Fonksiyonları'nın 401 hata döndüreceğini kanıtlayabilirsiniz.

## <a name="configure-and-secure-the-function-api"></a>İşlev API'sini yapılandırma ve güvenli hale getirmesi
1. İşlev uygulamasındaki iki ekstra alanın (Auth ve Network Restrictions) yapılandırılması gerekir.
1. Öncelikle Kimlik Doğrulama / Yetkilendirme'yi yapılandıralım, bu nedenle genel bakış &lt;sayfasını&gt; göstermek için işlev uygulamasının adını (Z işlevleri simgesinin yanında) tıklayın.
1. Sonraki 'Platform özellikleri' sekmesini seçin ve 'Kimlik Doğrulama / Yetkilendirme'yi seçin.
1. Uygulama Hizmeti Kimlik Doğrulaması özelliğini açın.
1. 'Kimlik Doğrulama Sağlayıcıları' altında 'Azure Etkin Dizini'ni seçin ve Yönetim Modu anahtarından 'Gelişmiş' seçeneğini belirleyin.
1. Arka uç işlevi API'sinin uygulama kimliğini (Azure AD B2C'den 'İstemci Kimliği' kutusuna) yapıştırın
1. İyi bilinen açık kimlik yapılandırma bitiş noktasını kaydokuldan yapıştırın veya ilkeyi Veren URL kutusuna kaydedin (bu yapılandırmayı daha önce kaydetmiştik).
1. Tamam'ı seçin.
1. İstek kimlik doğrulaması olmadığında Eylem'i "Azure Etkin Diziniyle Oturum Aç"a ayarlayın ve ardından Kaydet'i tıklatın.

   > [!NOTE]
   > Şimdi İşlev API'niz dağıtıldı ve doğru anahtar sağlandığı takdirde 401 yanıt atmalı ve geçerli bir istek sunulduğunda verileri döndürmelidir.
   > EasyAuth'ta, kimliği doğrulanmamış istekleri işlemek için 'Azure AD ile Giriş' seçeneğini yapılandırarak ayrıntılı savunma güvenliği eklediniz. EasyAuth 401 Yetkili Olmayan yanıt yerine AAD'ye 302 yönlendirmesi yapacağından, bunun Backend İşlev Uygulaması ve Frontend SPA arasındaki yetkisiz istek davranışını değiştireceğini unutmayın, bunu daha sonra API Yönetimi'ni kullanarak düzelteceğiz.
   > Geçerli bir anahtarınız ve OAuth2 jetonu varsa, geçerli bir anahtarınız ve OAuth2 jetonu varsa, herkes bunu her yerden arayabilir - ideal olarak tüm istekleri API Yönetimi üzerinden gelmeye zorlamak istiyoruz.
   > API Yönetimi tüketim katmanını kullanıyorsanız, bu katman için özel statik BIR IP olmadığı için VIP tarafından bu kilitlemeyi gerçekleştiremeceksiniz, paylaşılan gizli işlev anahtarı aracılığıyla API çağrılarınızı kilitleme yöntemine güvenmeniz gerekir , bu nedenle 11-13 adımları mümkün olmayacaktır.

1. 'Kimlik Doğrulama / Yetkilendirme' bıçağını kapatın 
1. 'Ağ' seçeneğini seçin ve ardından 'Erişim Kısıtlamaları'nı seçin
1. Ardından, izin verilen işlev uygulaması IP'lerini API Yönetimi örneği VIP'ye kilitleyin. Bu VIP, portalın API yönetimi - genel bakış bölümünde gösterilir.
1. İşlevportalı ile etkileşime devam etmek ve aşağıdaki isteğe bağlı adımları gerçekleştirmek istiyorsanız, burada da kendi genel IP adresinizi veya CIDR aralığını eklemeniz gerekir.
1. Listede izin girişi olduğunda, Azure diğer tüm adresleri engellemek için örtülü bir reddet kuralı ekler. 

IP kısıtlamaları paneline CIDR biçimlendirilmiş adres blokları eklemeniz gerekir. API Management VIP gibi tek bir adres eklemeniz gerektiğinde xx.xx.xx.xx biçiminde eklemeniz gerekir.

   > [!NOTE]
   > Artık İşlev API'niz API yönetimi veya adresiniz dışında herhangi bir yerden çağrılabilir olmamalıdır.
   
## <a name="import-the-function-app-definition"></a>İşlev uygulaması tanımını alma
1. *API Management bıçağını*açın, ardından *örneğini*açın.
1. Örneğinizin API Yönetimi bölümünden API Blade'i seçin.
1. 'Yeni API Ekle' bölmesinden 'İşlev Uygulaması'nı seçin ve açılan pencerenin üst kısmından 'Tam' seçeneğini belirleyin.
1. Gözat'ı tıklatın, içinde API barındırdığınız işlev uygulamasını seçin ve seç'e tıklayın.
1. API'ye API Yönetimi'nin dahili kullanımı için bir ad ve açıklama verin ve 'sınırsız' Ürüne ekleyin.
1. Daha sonra kullanmak üzere temel URL'yi kaydettiğinizden ve ardından oluştur'u tıklattığınızdan emin olun.

## <a name="configure-oauth2-for-api-management"></a>API Yönetimi için Oauth2'yi yapılandırın

1. Ardından, Güvenlik Sekmesinden Oauth 2.0 bıçağını seçin ve 'Ekle'yi tıklatın
1. Eklenen Oauth Endpoint için *Ekran Adı* ve *Açıklama* için değerler verin (bu değerler bir sonraki adımda Oauth2 bitiş noktası olarak gösterecektir).
1. Bu değer kullanılmayabileceğinden, Müşteri kayıt sayfası URL'sindeki herhangi bir değeri girebilirsiniz.
1. Örtülü *Auth* Grant türünü kontrol edin ve Yetkilendirme kodu hibe türünü işaretli bırakın.
1. *Yetkilendirme* ve *Token* uç noktası alanlarına taşıyın ve daha önce bilinen yapılandırma xml belgesinden yakaladığınız değerleri girin.
1. Azure AD B2C App kaydından Backend Function API istemci kimliğiyle 'kaynak' adı verilen *ek gövde parametresi* aşağı kaydırın ve doldurulun
1. 'İstemci kimlik bilgileri'ni seçin, Müşteri Kimliğini Geliştirici konsol uygulamasının uygulama kimliğine ayarlayın - tüketim API Yönetimi modelini kullanıyorsanız bu adımı atlayın.
1. İstemci Sırrını daha önce kaydettiğiniz anahtara ayarlayın - tüketim API Yönetimi modelini kullanıyorsanız bu adımı atlayın.
1. Son olarak, daha sonra kullanmak üzere API Management'ın auth kod hibesinin redirect_uri kaydedin.

## <a name="set-up-oauth2-for-your-api"></a>API'niz için Oauth2'yi ayarlayın
1. API'niz portalın sol tarafında 'Tüm API'ler' bölümünün altında görünür, üzerine tıklayarak API'nizi açar.
1. 'Ayarlar' Sekmesini seçin.
1. Kullanıcı yetkilendirme radyo düğmesinden "Oauth 2.0" seçeneğini seçerek ayarlarınızı güncelleyin.
1. Daha önce tanımladığınız Oauth sunucusunu seçin.
1. 'Kapsamı geçersiz kılma' onay kutusunu işaretleyin ve daha önce arka uç API çağrısı için kaydettiğiniz kapsamı girin.

   > [!NOTE]
   > Artık, istekleri yetkilendirmek için Azure AD B2C'den jetonlara nasıl erişebileceğimizi bilen ve Oauth2 Azure Active Directory B2C yapılandırmamızı anlayan bir API Yönetimi örneğimiz var.

## <a name="set-up-the-cors-and-validate-jwt-policies"></a>**CORS'i** ayarlayın ve **doğrulayın-jwt ilkelerini**

> Kullanılan APIM katmanına bakılmaksızın aşağıdaki bölümler izlenmelidir. 

1. Tasarım sekmesine geri dön ve "Tüm API'ler"i seçin ve ardından ilke düzenleyicisini göstermek için kod görünümü düğmesini tıklatın.
1. Gelen bölümü edin ve aşağıdaki gibi okunsın şekilde aşağıdaki xml yapıştırın.

   ```xml
   <inbound>
      <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
         <openid-config url="https://tenant.b2clogin.com/tenant.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=B2C_1_MyDefaultPolicy" />
         <required-claims>
            <claim name="aud">
               <value>your-backend-api-application-client-id</value>
            </claim>
         </required-claims>
      </validate-jwt>
      <cors>
         <allowed-origins>
            <origin>*</origin>
         </allowed-origins>
         <allowed-methods>
           <method>GET</method>
         </allowed-methods>
         <allowed-headers>
            <header>*</header>
         </allowed-headers>
         <expose-headers>
           <header>*</header>
         </expose-headers>
       </cors>
   </inbound>
   ```
1. Kaydolma veya oturum açma ilkesi için tanınmış Azure AD B2C bitiş noktanıza uyacak şekilde openid-config url'sini edin.
1. Arka uç API uygulaması için istemci kimliği olarak da bilinen geçerli uygulama kimliğiyle eşleşecek şekilde talep değerini edin ve kaydedin.

   > [!NOTE]
   > Şimdi API yönetimi JS SPA uygulamalarına çapraz menşe isteklerine yanıt verebiliyor ve isteği Fonksiyon API'sine iletmeden önce geçirilen JWT auth belirtecinin daraltma, fiyat sınırlama ve ön doğrulamasını gerçekleştirecek.

   > [!NOTE]
   > Aşağıdaki bölüm isteğe bağlıdır ve geliştirici portalını desteklemeyen **Tüketim** katmanı için geçerli değildir.
   > Geliştirici portalını kullanmak istemiyorsanız veya Tüketim katmanını kullandığınızdan beri kullanamıyorsanız, lütfen bu adımı atlayın ve doğrudan ["API'yi tüketmek için JavaScript SPA'yı oluşturun"](#build-the-javascript-spa-to-consume-the-api)'a atlayın.

## <a name="optional-configure-the-developer-portal"></a>[İsteğe bağlı] Geliştirici portalını yapılandırma

1. Azure AD B2C bıçağını açın ve Geliştirici Portalı için uygulama kaydına gidin
1. DAHA önce API Yönetimi'nde auth kodu hibesinin redirect_uri yapılandırdığınızda not aldığınıza 'Url'yi Yanıtla' girişini ayarlayın.

   Şimdi OAuth 2.0 kullanıcı yetkilendirmesi `Echo API`etkinleştirildiğinde, Geliştirici Konsolu API'yi aramadan önce kullanıcı için bir erişim belirteci elde eder.

1. Geliştirici `Echo API` portalının altındaki herhangi bir operasyona göz atın ve sizi Geliştirici Konsolu'na getirmek için **Deneyin'i** seçin.
1. Az önce eklediğiniz yetkilendirme sunucusuna karşılık gelen **Yetkilendirme** bölümünde yeni bir öğe not edin.
1. Yetkilendirme açılır listesinden **Yetkilendirme kodunu** seçin ve Azure AD kiracısında oturum açmanız istenir. Hesapla zaten oturum açtıysanız, sizden istenmeyebilir.
1. Başarılı oturum açmadan `Authorization: Bearer` sonra, Base64'te kodlanmış Azure AD B2C'den bir erişim belirteciyle isteğe bir üstbilgi eklenir. 
1. **Gönder'i** seçin ve API'yi başarıyla arayabilirsiniz.

   > [!NOTE]
   > Artık API yönetimi, geliştirici portalının API'nizi test etmesi için belirteçler edinebilir ve bunun tanımını anlayabilir ve geliştirme portalında uygun test sayfasını işletebilir.

1. API Yönetimi portalının genel bakış açısından, API yöneticisi olarak oturum açabilmek için 'Geliştirici Portalı'nı tıklatın.
1. Burada, siz ve API'nizin diğer seçili tüketicileri bunları bir konsoldan test edebilir ve arayabilirsiniz.
1. 'Ürünler'i seçin, ardından 'Sınırsız'ı seçin, ardından daha önce oluşturduğumuz API'yi seçin ve 'TRY IT'e tıklayın
1. API abonelik anahtarının hide'sını boşaltın ve daha sonra gereksinim duyduğunuz istek url'si ile birlikte güvenli bir yere kopyalayın.
1. Ayrıca Oauth auth açılan, Örtük seçin ve burada bir pop-up ile doğrulamak zorunda klabilirsiniz.
1. 'Gönder'i tıklatın ve her şey yolundaysa, İşlev Uygulamanız 200 OK mesajı ve bazı JSON ile API yönetimi aracılığıyla bir merhaba mesajıyla yanıt vermelidir.

   > [!NOTE]
   > Tebrikler, artık Bir API'yi yayımlamak, güvenli hale getirmek ve tüketmek için birlikte çalışan Azure AD B2C, API Yönetimi ve Azure Fonksiyonları'na sahipsiniz. API aslında iki kez bu yöntemi kullanarak güvenli olduğunu fark etmiş olabilirsiniz, bir kez API Yönetimi Ocp-Abonelik-Anahtar Üstbilgi ile, ve bir kez Yetkilendirme ile: Bearer JWT.
   > Bu örnek bir JavaScript Tek Sayfa Uygulaması olduğundan, api yönetim anahtarını yalnızca fiyat sınırlayıcı ve faturalandırma çağrıları için kullanırız.
   > Gerçek Yetkilendirme ve Kimlik Doğrulama, Azure AD B2C tarafından işlenir ve JWT'de özetlenir ve bu da iki kez, bir kez API Yönetimi ve ardından Azure Fonksiyonları tarafından doğrulanır.

## <a name="build-the-javascript-spa-to-consume-the-api"></a>API'yi tüketmek için JavaScript SPA'yı oluşturun
1. Azure portalında depolama hesapları blade'ini açın 
1. Oluşturduğunuz hesabı seçin ve Ayarlar bölümünden 'Statik Web Sitesi' bıçağını seçin ('Statik Web Sitesi' seçeneğini görmüyorsanız, bir V2 hesabı oluşturduğunuzdan kontrol edin).
1. Statik web barındırma özelliğini 'etkin' olarak ayarlayın ve dizin belge adını 'index.html' olarak ayarlayın, ardından 'kaydet'i tıklatın.
1. Bu konum, ön uç sitesinin barındırılan yer olduğu için Birincil Bitiş Noktası'nın içeriğini not edin. 

   > [!NOTE]
   > Azure Blob Depolama + CDN yeniden yazmak veya Azure App Service kullanabilirsiniz - ama Blob Depolama Statik Web barındırma özelliği bize statik web içeriği hizmet için varsayılan bir kapsayıcı verir / html / js / Azure Depolama css ve sıfır iş için bizim için varsayılan bir sayfa çıkaracaktır.

## <a name="upload-the-js-spa-sample"></a>JS SPA örneğini yükleyin
1. Depolama hesap bıçağında yken, Blob Service bölümünden 'Blobs' bıçağını seçin ve sağ bölmede görünen $web kabına tıklayın.
1. Aşağıdaki kodu makinenizde yerel olarak index.html olarak bir dosyaya kaydedin ve ardından dosya index.html dosyasını $web kapsayıcısına yükleyin.

   ```html
   <!doctype html>
   <html lang="en">
   <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Sample JS SPA</title>
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
   </head>
   <body>
        <div class="container-fluid">
            <div class="row">
                <div class="col-md-12">
                    <nav class="navbar navbar-expand-lg navbar-light bg-light navbar-dark bg-dark">
                        <a class="navbar-brand" href="#">Sample Code</a>
                        <ul class="navbar-nav ml-md-auto">
                            <li class="nav-item dropdown">
                                <a class="btn btn-large btn-success" onClick="login()">Sign In</a>
                            </li>
                        </ul>
                    </nav>
                </div>
            </div>
            <div class="row">
                <div class="col-md-12">
                    <div class="jumbotron">
                        <h2>
                    <div id="message">Hello, world!</div>
                    </h2>
                        <p>
                            <a class="btn btn-primary btn-large" onClick="GetAPIData()">Call API</a>
                        </p>
                    </div>
                </div>
            </div>
        </div>
        <script src="https://code.jquery.com/jquery-3.2.1.min.js"></script>
        <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
        <script src="https://secure.aadcdn.microsoftonline-p.com/lib/1.0.0/js/msal.js"></script>
        <script lang="javascript">
            var applicationConfig = {
                clientID: "clientidgoeshere",
                authority: "https://tenant.b2clogin.com/tfp/tenant/policy",
                b2cScopes: ["https://tenant/app/scope"],
                webApi: 'http://functionurl',
                subKey: 'apimkeygoeshere'
            };
            var msalConfig = {
                auth: {
                        clientId: applicationConfig.clientID, 
                        authority:  applicationConfig.authority,
                        validateAuthority: false
                },
                cache: {
                        cacheLocation: "localStorage",
                        storeAuthStateInCookie: true
                }
            };
            var clientApplication = new Msal.UserAgentApplication(msalConfig);
            function login() {
                var loginRequest = {
                    scopes: applicationConfig.b2cScopes
                };
                clientApplication.loginPopup(loginRequest).then(function (loginResponse) {
                    var tokenRequest = {
                        scopes: applicationConfig.b2cScopes
                    };
                    clientApplication.acquireTokenSilent(tokenRequest).then(function (tokenResponse) {
                        document.getElementById("signinbtn").innerHTML = "Logged in as: " + clientApplication.account.name;
                        document.getElementById("callapibtn").hidden = false
                        }).catch(function (error) {
                            clientApplication.acquireTokenPopup(tokenRequest).then(function (tokenResponse) {
                                }).catch (function (error) {
                                    console.log("Error acquiring the popup:\n" + error);
                                });
                        })
                    }).catch (function (error) {
                        console.log("Error during login:\n" + error);
                });
            }
            function GetAPIData() {
                var tokenRequest = {
                    scopes: applicationConfig.b2cScopes
                }
                clientApplication.acquireTokenSilent(tokenRequest).then(function (tokenResponse) {
                    callApiWithAccessToken(tokenResponse.accessToken);
                    }).catch(function (error) {
                        clientApplication.acquireTokenPopup(tokenRequest).then(function (tokenResponse) {
                            callApiWithAccessToken(tokenResponse.accessToken);
                            
                        }).catch(function (error) {
                            console.log("Error acquiring the access token to call the Web api:\n" + error);
                        });
                    })
            }
            function callApiWithAccessToken(token)
            {
                console.log("calling "  + applicationConfig.webApi +  " with " + token);
                    // Make the api call here
                $.ajax({
                    type: "get",
                    headers: {'Authorization': 'Bearer ' + token, 'Ocp-Apim-Subscription-Key': applicationConfig.subKey},                   url: applicationConfig.webApi
                }
                ).done(function (body) {
                    document.getElementById("message").innerHTML = "The API Said " + body;
                });
            }
        </script>
    </body>
   </html>
   
   ```

1. Son bölümde daha önce depoladığınız Statik Web Sitesi Birincil Bitiş Noktasına göz atın.

   > [!NOTE]
   > Tebrikler, JS uygulamasını api tuşlarınızla yapılandırmadığımız veya JS uygulamasını Azure AD B2C ayrıntılarınız ile yapılandırmadığımız için Azure Depolama'ya bir JavaScript Tek Sayfa Uygulaması dağıttınız - sayfayı açtığınızda henüz çalışmayacaktır.

## <a name="configure-the-js-spa-for-azure-ad-b2c"></a>Azure AD B2C için JS SPA'yı yapılandırın
1. Artık her şeyin nerede olduğunu biliyoruz: SPA'yı uygun API Yönetimi API adresi ve doğru Azure AD B2C uygulaması / istemci işlisi ile yapılayabiliriz
1. Azure portalı depolama bıçağına geri dön ve index.html'e tıklayın ve ardından 'Blob'u Edit'i seçin' seçeneğini belirleyin 
1. 'B2cScopes' değerlerinin API arka uç için olduğunu belirterek, daha önce B2C'de kaydettiğiniz ön uç uygulamanızla eşleşecek şekilde auth ayrıntılarını güncelleyin.
1. WebApi anahtarı ve api url'si API işlemi için API Yönetimi test bölmesinde bulunabilir.
1. API Management'a geri giderek, 'Abonelikler' seçeneğini seçerek ve 'Abonelik Ekle'yi tıklatarak ve ardından kaydı kaydederek Bir APIM abonelik anahtarı oluşturun. Oluşturulan satırın yanındaki Elipsis'i (...) tıklattığınızda, birincil anahtarı kopyalayabilmeniz için tuşları göstermenize olanak sağlar.
1. Aşağıdaki kod gibi bir şey görünmelidir:-  

   ```javascript
   var applicationConfig =
      clientID: "{aadb2c-clientid-goeshere}",
      authority: "https://{tenant}.b2clogin.com/{tenant}/{policy}",
      b2cScopes: ["https://{tenant}/{app}/{scope}"], 
      webApi: 'http://{apim-url-for-your-function}',
      subKey: '{apim-subscription-key-goes-here}'
   };
   ```

1. Kaydet’e tıklayın.

## <a name="set-the-redirect-uris-for-the-azure-ad-b2c-frontend-app"></a>Azure AD B2C ön yüz uygulaması için yönlendirme URL'lerini ayarlama
1. Azure AD B2C bıçağını açın ve JavaScript Frontend Uygulaması için uygulama kaydına gidin
1. Yukarıdaki statik web sitesi birincil bitiş noktasını ayarladığınızda yeniden yönlendirme URL'sini belirttiğiniz URL'ye ayarlayın

   > [!NOTE] 
   > Bu yapılandırma, ön uç uygulamasının istemcisinin Azure AD B2C'den uygun talepleri içeren bir erişim belirteci almasına neden olur.
   > SPA, arka uç API'sine yapılan çağrıdaki https üstbilgisine bunu taşıyıcı belirteci olarak ekleyebilir.
   > API Yönetimi, isteği alan Azure İşlevi API'sine geçmeden önce belirteç, fiyat sınırı çağrılarını abone anahtarıyla bitiş noktasına önceden doğrular.
   > SPA yanıtı tarayıcıda verecektir.

   > *Tebrikler, mükemmel bir uyum içinde çalışmak için Azure AD B2C, Azure API Yönetimi, Azure İşlevleri, Azure Uygulama Hizmeti Yetkilendirmesi'ni yapılandırıldınız!*

   > [!NOTE]
   > Şimdi basit bir güvenli API ile basit bir uygulama var, bunu test edelim.

## <a name="test-the-client-application"></a>İstemci uygulamasını test edin
1. Daha önce oluşturduğunuz depolama hesabından not aldığınız örnek uygulama URL'sini açın
1. Sağ üst köşede "Oturum Aç"ı tıklayın, bu tıklama Azure AD B2C kaydolur veya profilinde oturum açın.
1. Ekranın "Oturum Açtım" bölümünde Oturum Aç'ı JWT'nizden doldurulacaktır.
1. Şimdi "Web Api'yi Ara"yı tıklatın ve sayfanın güvenli API'nizden gönderilen değerlerle güncelleştirmelisiniz.

## <a name="and-were-done"></a>Ve işimiz bitti.
Yukarıdaki adımlar, API Yönetimi ile Azure AD B2C'nin birçok farklı kullanımına izin verecek şekilde uyarlanabilir ve düzenlenebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory ve OAuth2.0](../active-directory/develop/authentication-scenarios.md)hakkında daha fazla bilgi edinin.
* API Yönetimi hakkında daha fazla [video](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.
* Arka uç hizmetinizi güvence altına almanın diğer yolları için [Karşılıklı Sertifika kimlik doğrulaması'na](api-management-howto-mutual-certificates.md)bakın.
* [BIR API Yönetimi hizmeti örneği oluşturun.](get-started-create-service-instance.md)
* [İlk API'nizi yönetin.](import-and-publish.md)
