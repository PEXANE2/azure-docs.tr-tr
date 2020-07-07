---
title: Azure Active Directory B2C ve Azure API Management kullanarak SPA arka ucunu OAuth 2,0 ile koruyun.
description: Azure Active Directory B2C, Azure API Management ve kolay kimlik doğrulamasını kullanarak bir API 'yi bir JavaScript SPA 'dan çağırarak, OAuth 2,0 ile bir API 'yi koruyun.
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
ms.openlocfilehash: 3c5c13b3aae33b098de79a3429a299cc33f1c4f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82202885"
---
# <a name="protect-spa-backend-with-oauth-20-azure-active-directory-b2c-and-azure-api-management"></a>OAuth 2,0, Azure Active Directory B2C ve Azure API Management ile SPA arka ucunu koruyun

Bu senaryoda, bir API 'yi korumak için Azure API Management örneğinizi nasıl yapılandırabileceğiniz gösterilmektedir.
Azure AD B2C ile OAuth 2,0 protokolünü kullanacağız ve EasyAuth kullanarak bir Azure Işlevleri arka ucunu güvenli hale getirmek için API Management.

## <a name="aims"></a>Amaçlar
API Management, Azure Işlevleri ve Azure AD B2C ile basitleştirilmiş bir senaryoda nasıl kullanılabileceğini görebiliriz. Bir API 'yi çağıran ve Azure AD B2C kullanıcıları oturum açan bir JavaScript (JS) uygulaması oluşturacaksınız. Ardından, arka uç API 'sini korumak için API Management Validate-JWT ilke özelliklerini kullanacaksınız.

Derinlemesine savunma için, yeniden bitiş API 'SI içinde yeniden belirteç doğrulamak üzere EasyAuth ' i kullanırız.

## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki adımları izlemek için, şunları yapmanız gerekir:
* Ön uç JS tek sayfalı uygulamayı barındırmak için bir Azure (StorageV2) Genel Amaçlı v2 depolama hesabı
* Azure API Management örneği 
* Çağrılan API 'yi barındırmak için boş bir Azure Işlev uygulaması (bir Windows tüketim planında v2 .NET Core çalışma zamanı 'nı çalıştıran)
* Bir aboneliğe bağlı Azure AD B2C kiracısı 

Uygulamada, üretim iş yüklerinde aynı bölgedeki kaynakları kullanabilirsiniz, ancak bu nasıl yapılır makalesi için dağıtım bölgesi önemli değildir.

## <a name="overview"></a>Genel Bakış
Bu işlem tamamlandıktan sonra, kullanımdaki bileşenlerin ve aralarındaki akışın bir gösterimi aşağıda verilmiştir.
![Kullanımdaki ve Flow bileşenleri](../api-management/media/howto-protect-backend-frontend-azure-ad-b2c/image-arch.png "Kullanımdaki ve Flow bileşenleri")

Adımlara hızlı bir genel bakış aşağıda verilmiştir:

1. Kapsamlarla (ön uç, API Management) ve API uygulamaları ile API erişimi sağlamak Azure AD B2C oluşturun
1. Kullanıcıların Azure AD B2C oturum açmasını sağlamak için kaydolma veya oturum açma ilkelerini oluşturun 
1. Geliştirici konsolunda OAuth2 Kullanıcı yetkilendirmesini etkinleştirmek için yeni Azure AD B2C Istemci kimlikleri ve anahtarlarıyla API Management yapılandırın
1. Işlev API 'sini oluşturma
1. Yeni Azure AD B2C Istemci KIMLIĞI ve anahtarlarıyla EasyAuth 'yi etkinleştirmek ve APıM VIP 'ye kilitlemek için Işlev API 'sini yapılandırın 
1. API Management içinde API tanımını oluşturma
1. API Management API yapılandırması için Oauth2 ayarlama
1. **CORS** ilkesini ayarlama ve her gelen Istek için OAuth belirtecini doğrulamak üzere **Validate-JWT** ilkesini ekleme
1. API 'YI kullanmak için çağıran uygulamayı oluşturun
1. JS SPA örneğini karşıya yükle
1. Örnek JS Istemci uygulamasını yeni Azure AD B2C Istemci KIMLIĞI ve anahtarlarıyla yapılandırın 
1. Istemci uygulamasını test etme

## <a name="configure-azure-ad-b2c"></a>Azure AD B2C Yapılandır 
Portalda Azure AD B2C dikey penceresini açın ve aşağıdaki adımları uygulayın.
1. **Uygulamalar** sekmesini seçin 
1. Aşağıdaki amaçlar için ' Ekle ' düğmesine tıklayın ve üç uygulama oluşturun
   * Ön uç Istemcisi.
   * Arka uç Işlev API 'SI.
   * Seçim API Management geliştirici portalı (Azure API Management 'yi tüketim katmanında çalıştırmadığınız müddetçe daha sonra bu senaryoya daha fazla).
1. Tüm 3 uygulamalar için WebApp/Web API 'yi ayarlayın ve yalnızca ön uç Istemcisi için ' örtük akışa Izin ver ' öğesini Evet olarak ayarlayın.
1. Şimdi uygulama KIMLIĞI URI 'sini ayarlayın, benzersiz bir öğe seçin ve oluşturulmakta olan hizmetle ilgili bir seçim yapın.
1. Şimdilik yanıt URL 'leri için yer tutucuları kullanın https://localhost , bu URL 'leri daha sonra güncelleştireceğiz.
1. ' Oluştur ' düğmesine tıklayın ve yukarıdaki üç uygulamanın her biri için 2-5 adımlarını yineleyin, sonra da her üç uygulama için AppID URI 'sini, adı ve uygulama KIMLIĞINI daha sonra kullanmak üzere kaydetme.
1. Uygulamalar listesinden API Management geliştirici portalı uygulamasını açın ve *anahtarlar* sekmesini seçin (Genel altında) ve ardından ' anahtar oluştur ' düğmesine tıklayarak bir kimlik doğrulama anahtarı oluşturun
1. Kaydet 'e tıklandıktan sonra anahtarı daha sonra kullanılmak üzere güvenli bir yere kaydedin-bu anahtarı görüntülemek ve kopyalamak için alacağınız tek şans olduğunu unutmayın.
1. Şimdi *yayımlanan kapsamlar* sekmesini SEÇIN (API erişimi altında)
1. Işlev API 'niz için bir kapsam oluşturup adlandırın, kapsamı kaydedin ve tam kapsam değerini ve ardından ' Kaydet 'e tıklayın.
   > [!NOTE]
   > Azure AD B2C kapsamları, API 'lerinizin API erişimi dikey penceresi aracılığıyla, diğer uygulamaların uygulama izinlerini, sizin de yalnızca adlandırılmış API 'niz için sizin oluşturduğunuz etkin bir şekilde bir şekilde oluşturabilebileceği etkin izinlere sahiptir.
1. Diğer iki uygulamayı açın ve ardından *API erişimi* sekmesini bulun.
1. Bunlara arka uç API kapsamına erişim izni verin ("kullanıcının profiline erişme").
1. Bir kimlik doğrulama anahtarı oluşturmak ve bu anahtarları daha sonra güvenli bir yerde kaydetmek için, her biri ' genel ' altındaki *anahtarlar* sekmesini seçerek bir anahtar oluşturun.

## <a name="create-a-sign-up-or-sign-in-user-flow"></a>"Kaydolma veya oturum açma" Kullanıcı akışı oluşturma
1. Azure AD B2C dikey penceresinin köküne (veya ' genel bakış ') dön 
1. Ardından "Kullanıcı Akışları (Ilkeler)" öğesini seçin ve "Yeni Kullanıcı akışı" seçeneğine tıklayın
1. ' Kaydol ve oturum aç ' Kullanıcı akış türünü seçin
1. İlkeye bir ad verin ve daha sonra için kaydedin.
1. Ardından ' kimlik sağlayıcıları ' altında ' Kullanıcı KIMLIĞI kaydolma ' seçeneğini işaretleyin (Bu durum ' e-posta kaydı ') ve Tamam ' a tıklayabilirsiniz. 
1. ' Kullanıcı öznitelikleri ve talepler ' altında ' daha fazla göster... ' seçeneğine tıklayın. daha sonra kullanıcılarınızın girmesini ve belirtece döndürülmesini istediğiniz talep seçeneklerini belirleyin. Toplamak ve döndürmek için en azından ' görünen ad ' ve ' e-posta adresi ' seçeneğini işaretleyin ve ' Tamam 'a tıklayın, sonra ' oluştur ' seçeneğine tıklayın.
1. Listede oluşturduğunuz ilkeyi seçin, sonra ' Kullanıcı akışını Çalıştır ' düğmesine tıklayın.
1. Bu eylem kullanıcı akışını Çalıştır dikey penceresini açacak, ön uç uygulamasını seçtiğinizde, ardından ' etki alanı Seç ' için açılan menü altında gösterilen b2clogin.com etki alanının adresini kaydeder.
1. En üstteki bağlantıya tıklayarak ' iyi bilinen OpenID yapılandırma uç noktasını açın ve authorization_endpoint ve token_endpoint değerlerini iyi bilinen OpenID yapılandırma uç noktası olarak bağlantı değerine kaydedin.

   > [!NOTE]
   > B2C Ilkeleri, farklı veri bileşenleri yakalamak ve kullanıcıların farklı yollarla oturum açabilmek için Azure AD B2C oturum açma uç noktalarını kullanıma sunlamanızı sağlar. Bu durumda, iyi bilinen bir yapılandırma uç noktası sunan bir kaydolma veya oturum açma uç noktası yapılandırdık, özellikle oluşturulan ilkeniz p = parametresi tarafından URL 'de tanımlandı.
   > 
   > Bu işlem yapıldıktan sonra, artık kullanıcıları birden çok uygulamada imzalayabileceğiniz bir sektör kimliği platformu olan işlevsel bir Işletmeniz vardır. 
   > İsterseniz ' Kullanıcı akışını Çalıştır ' düğmesine tıklayabilirsiniz (kaydolma veya oturum açma işlemine ulaşmak için) ve uygulamada ne yapacaklarına ilişkin bir fikir edinebilirsiniz, ancak uygulama henüz dağıtılmadığı için uçtaki yeniden yönlendirme adımı başarısız olur.

## <a name="build-the-function-api"></a>İşlev API 'sini oluşturma
1. Aboneliğinizdeki öğeleri yeniden yapılandırabilmemiz için Azure portal standart Azure AD kiracınıza geri dönün 
1. Azure portal Işlev uygulamaları dikey penceresine gidin, boş işlev uygulamanızı açın, sonra hızlı başlangıç yoluyla yeni bir portal Içi ' Web kancası + API ' işlevi oluşturun.
1. Aşağıdaki örnek kodu, görüntülenen varolan kodun üzerinde. CSX öğesine yapıştırın.

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
   > Yeni yapıştırdığınız c# betiği işlev kodu yalnızca bir satırı işlevler günlüklerine kaydeder ve bazı dinamik veriler (Tarih ve saat) ile "Merhaba Dünya" metnini döndürür.

3. Sol taraftaki dikey penceredeki "tümleştir" seçeneğini belirleyin ve ardından bölmenin sağ üst köşesindeki ' Gelişmiş Düzenleyici ' öğesini seçin.
4. Örnek kodu var olan JSON üzerine yapıştırın.

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

5. HttpTrigger1 sekmesine dönün, ' Işlev URL 'SI Al ' seçeneğine tıklayın, ardından görüntülenen URL 'YI kopyalayın.

   > [!NOTE]
   > Az önce oluşturduğunuz bağlamalar, yalnızca yeni kopyaladığınız URL 'ye anonim HTTP GET isteklerini yanıtlamak için IŞLEVLERE söyleyecektir. ( `https://yourfunctionappname.azurewebsites.net/api/hello?code=secretkey` ) Artık çok basit bir yük döndürmekte olan ölçeklenebilir bir sunucusuz https API 'si sunuyoruz.
   > Artık bu API 'yi yukarıdaki URL 'YI kullanarak bir Web tarayıcısından çağırmayı test edebilirsiniz. Ayrıca, URL 'nin? Code = gizli bölümünü de açabilir ve Azure Işlevlerinin bir 401 hatası döndürmesini kanıtlayabilirsiniz.

## <a name="configure-and-secure-the-function-api"></a>İşlev API 'sini yapılandırma ve koruma
1. İşlev uygulamasındaki iki ek alanın yapılandırılması gerekir (kimlik doğrulama ve ağ kısıtlamaları).
1. Kimlik doğrulama/yetkilendirme yapılandırmam, bu nedenle &lt; &gt; genel bakış sayfasını göstermek için işlev uygulamasının adına (Z işlevleri simgesinin yanında) tıklayın.
1. Sonra ' platform özellikleri ' sekmesini seçin ve ' kimlik doğrulama/yetkilendirme ' seçeneğini belirleyin.
1. App Service kimlik doğrulaması özelliğini açın.
1. ' Kimlik doğrulama sağlayıcıları ' altında ' Azure Active Directory ' öğesini seçin ve yönetim modu anahtarından ' Advanced ' öğesini seçin.
1. Arka uç Işlev API 'sinin uygulama KIMLIĞINI (Azure AD B2C ' Istemci KIMLIĞI ' kutusuna) yapıştırın
1. En Iyi bilinen açık kimlik yapılandırma uç noktasını, verenin URL 'SI kutusuna kaydolma veya oturum açma ilkesinden (Bu yapılandırmayı daha önce kaydettik) yapıştırın.
1. Tamam'ı seçin.
1. İsteğin kimliği doğrulanmamış açılan menü açılır listesini "Azure Active Directory ile oturum aç" olarak ayarlayın ve Kaydet ' e tıklayın.

   > [!NOTE]
   > Artık Işlev API 'niz dağıtılır ve doğru anahtar sağlanmazsa 401 yanıt vermelidir ve geçerli bir istek sunulursa verileri döndürmelidir.
   > Kimliği doğrulanmamış istekleri işlemek için ' Azure AD Ile oturum aç ' seçeneğini yapılandırarak EasyAuth 'e ek derinlemesine savunma güvenliği ekledik. Bu işlem arka uç İşlev Uygulaması ve ön uç SPA arasındaki yetkisiz istek davranışını değiştirecek ve EasyAuth, 401 yetkili olmayan bir yanıt yerine AAD 'ye bir 302 yeniden yönlendirme yayınlacağına dikkat edin. bunu API Management daha sonra kullanarak düzeltebiliriz.
   > Hala bir IP güvenliği uygulanamadığından, geçerli bir anahtarınız ve OAuth2 belirteciniz varsa, herkes bunu her yerde çağırabilir ve tüm isteklerin API Management aracılığıyla gelmesini zorlamak istiyoruz.
   > API Management tüketim katmanını kullanıyorsanız, söz konusu katman için adanmış bir statik IP olmadığından, bu kilidi VIP tarafından gerçekleştiremezsiniz, paylaşılan gizlilik işlev anahtarı aracılığıyla API aramalarınızı kilitleme yöntemine bağlı olmanız gerekir, böylece 11-13 adımları mümkün olmayacaktır.

1. ' Kimlik doğrulama/yetkilendirme ' dikey penceresini kapat 
1. ' Ağ ' ' ı seçin ve sonra ' erişim kısıtlamaları ' ' nı seçin
1. Ardından, izin verilen işlev uygulaması IP 'lerini API Management örneği VIP 'sine kilitleyin. Bu VIP, portalın API yönetimi-genel bakış bölümünde gösterilir.
1. İşlevler portalı ile etkileşime devam etmek ve aşağıdaki isteğe bağlı adımları uygulamak istiyorsanız, kendi genel IP adresinizi veya CıDR aralığınızı da buraya eklemeniz gerekir.
1. Listede bir izin verme girişi olduğunda Azure, diğer tüm adresleri engellemek için örtük bir reddetme kuralı ekler. 

IP kısıtlamaları paneline CıDR biçimli adres blokları eklemeniz gerekir. API Management VIP gibi tek bir adres eklemeniz gerektiğinde, bunu xx. xx. xx. xx biçiminde eklemeniz gerekir.

   > [!NOTE]
   > Artık Işlev API 'niz, API Yönetimi veya adresiniz aracılığıyla değil herhangi bir yerden çağrılabilir olmamalıdır.
   
## <a name="import-the-function-app-definition"></a>İşlev uygulaması tanımını içeri aktarma
1. *API Management dikey penceresini*açın ve sonra *örneğinizi*açın.
1. Örneğinizin API Management bölümündeki API dikey penceresini seçin.
1. ' Yeni bir API Ekle ' bölmesinden ' İşlev Uygulaması ' öğesini seçin ve ardından açılan pencerenin üst kısmından ' tam ' seçeneğini belirleyin.
1. Araştır ' a tıklayın, içinde API barındırmakta olduğunuz işlev uygulamasını seçin ve Seç ' e tıklayın.
1. API 'ye API Management iç kullanımı için bir ad ve açıklama verin ve bunu ' sınırsız ' ürüne ekleyin.
1. Daha sonra kullanmak için temel URL 'YI kaydettiğinizden emin olun ve ardından Oluştur ' a tıklayın.

## <a name="configure-oauth2-for-api-management"></a>API Management için Oauth2 yapılandırma

1. Sonra, güvenlik sekmesinden OAuth 2,0 dikey penceresini seçin ve ' Ekle ' seçeneğine tıklayın.
1. Eklenen OAuth uç noktası için *görünen ad* ve *Açıklama* değerlerini verin (Bu değerler bir sonraki adımda Oauth2 uç noktası olarak görünür).
1. Bu değer kullanılmayabilmeniz için, Istemci kayıt sayfası URL 'sine herhangi bir değer girebilirsiniz.
1. *Örtük kimlik doğrulama* izin türünü denetleyin ve yetkilendirme kodu verme türünü işaretli olarak bırakın.
1. *Yetkilendirme* ve *belirteç* uç noktası alanlarına gidin ve daha önce iyi bilinen yapılandırma XML belgesinden yakaladığınız değerleri girin.
1. Aşağı kaydırın ve ' Resource ' adlı *ek gövde parametresini* Azure AD B2C uygulama kaydından arka uç işlev API 'SI istemci kimliği ile doldurun
1. ' Istemci kimlik bilgileri ' ni seçin, Istemci KIMLIĞINI geliştirici konsolu uygulamasının uygulama KIMLIĞI olarak ayarlayın-tüketim API Management modelini kullanıyorsanız bu adımı atlayın.
1. Istemci parolasını daha önce kaydettiğiniz anahtara ayarlayın-tüketim API Management modelini kullanıyorsanız bu adımı atlayın.
1. Son olarak, artık daha sonra kullanılmak üzere API Management kimlik doğrulama kodu verme redirect_uri kaydeder.

## <a name="set-up-oauth2-for-your-api"></a>API 'niz için Oauth2 ayarlama
1. API 'niz portalın sol tarafında ' tüm API 'Ler ' bölümünde görünür, API 'nizi tıklayarak açın.
1. ' Ayarlar ' sekmesini seçin.
1. Ayarlarınızı, kullanıcı yetkilendirmesi radyo düğmesinden "OAuth 2,0" seçerek güncelleştirin.
1. Daha önce tanımladığınız OAuth sunucusunu seçin.
1. ' Kapsamı geçersiz kıl ' onay kutusunu işaretleyin ve arka uç API çağrısı için kaydettiğiniz kapsamı daha önce üzerinde girin.

   > [!NOTE]
   > Artık Azure AD B2C ' dan erişim belirteçlerine nasıl yetki vereceğiz ve Oauth2 Azure Active Directory B2C yapılandırmanızı nasıl anlayacağını bilen bir API Management örneğidir.

## <a name="set-up-the-cors-and-validate-jwt-policies"></a>**CORS** ve **Validate-JWT** ilkelerini ayarlama

> Aşağıdaki bölümler, kullanılan APıM katmanının ne olursa olsun gelmelidir. 

1. Tasarım sekmesine geri dönün ve "tüm API 'Ler" i seçin, sonra da ilke düzenleyicisini göstermek için kod görünümü düğmesine tıklayın.
1. Gelen bölümünü düzenleyin ve aşağıdaki XML 'yi şöyle olacak şekilde yapıştırın.

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
1. OpenID-config URL 'sini, oturum açma veya kaydolma ilkesi için iyi bilinen Azure AD B2C uç noktası ile eşleşecek şekilde düzenleyin.
1. Talep değerini, arka uç API 'SI uygulamasının istemci KIMLIĞI olarak da bilinen geçerli uygulama KIMLIĞIYLE eşleşecek şekilde düzenleyin ve kaydedin.

   > [!NOTE]
   > Artık API Yönetimi, JS SPA uygulamalarına çapraz kaynak isteklerini yanıtlayabilir ve isteğin API 'sine iletilmeden önce, geçirilen JWT kimlik doğrulama belirtecinin, azaltma, hız sınırlaması ve ön doğrulama işlemleri gerçekleştirmeye çalışır.

   > [!NOTE]
   > Aşağıdaki bölüm isteğe bağlıdır ve geliştirici portalını desteklemeyen **Tüketim** katmanına uygulanmaz.
   > Geliştirici portalını kullanmayı düşünmüyorsanız veya tüketim katmanını kullandığınızdan bu yana bunu kullanmazsınız, lütfen bu adımı atlayın ve ["API 'yi kullanmak Için JAVASCRIPT Spa 'Yı derleyin"](#build-the-javascript-spa-to-consume-the-api)bölümüne atlayın.

## <a name="optional-configure-the-developer-portal"></a>Seçim Geliştirici portalını yapılandırma

1. Azure AD B2C dikey penceresini açın ve geliştirici portalı için uygulama kaydına gidin
1. Daha önce API Management kimlik doğrulama kodu verme redirect_uri yapılandırdığınızda, ' yanıt URL 'SI ' girişini not ettiğiniz şekilde ayarlayın.

   OAuth 2,0 kullanıcı yetkilendirmesi üzerinde etkin olduğuna göre, `Echo API` Geliştirici Konsolu API 'yi çağırmadan önce Kullanıcı için bir erişim belirteci alır.

1. Geliştirici Portalında herhangi bir işleme gidin `Echo API` ve bunu geliştirici konsoluna getirmek Için **dene** ' yi seçin.
1. Yeni eklediğiniz yetkilendirme sunucusuna karşılık gelen **Yetkilendirme** bölümünde yeni bir öğe olduğunu aklınızda edin.
1. Yetkilendirme açılan listesinden **yetkilendirme kodu** ' nu seçin ve Azure AD kiracısında oturum açmanız istenir. Zaten hesapla oturum açtıysanız, bu durum istenmez.
1. Başarılı oturum açma işleminden sonra isteğe bir `Authorization: Bearer` üst bilgi eklenir ve Base64 'te kodlanmış Azure AD B2C bir erişim belirteci olur. 
1. **Gönder** ' i seçin ve API 'yi başarıyla çağırabilirsiniz.

   > [!NOTE]
   > Artık API Yönetimi, API 'nizi test etmek için geliştirici portalı için belirteçler edinebilir ve bu tanımı anlayabilir ve geliştirme portalındaki uygun test sayfasını işleyebilir.

1. API Management portalının genel bakış dikey penceresinde, API Yöneticisi olarak oturum açmak için ' Geliştirici Portalı ' seçeneğine tıklayın.
1. Burada ve API 'nizin diğer seçili tüketicileri bir konsoldan test edebilir ve bunları çağırabilir.
1. ' Ürünler ' seçeneğini belirleyip ' sınırsız ' seçeneğini belirleyip daha önce oluşturduğumuz API 'yi seçin ve ' deneyın ' seçeneğine tıklayın
1. API abonelik anahtarını gösterin ve daha sonra ihtiyacınız olacak istek URL 'siyle güvenli bir yere kopyalayın.
1. Ayrıca, OAuth auth açılan listesinden örtük ' i seçin ve bir açılan pencerede burada kimlik doğrulaması yapmanız gerekebilir.
1. ' Gönder ' düğmesine tıklayın ve tümü iyi ise, İşlev Uygulaması 200 Tamam iletisiyle API yönetimi aracılığıyla bir merhaba iletiyle ve bazı JSON ile yanıt vermelidir.

   > [!NOTE]
   > Tebrikler, artık bir API 'yi yayımlamak, güvenli hale getirmek ve kullanmak için birlikte çalışan Azure AD B2C, API Management ve Azure Işlevleriniz vardır. Bu yöntemi kullanarak, API 'nin aslında güvenlik altında olduğunu fark etmiş olabilirsiniz: API Management OCP-Subscription-Key üst bilgisi ve yetkilendirme: taşıyıcı JWT.
   > Bu örnekte, JavaScript tek sayfalı bir uygulama olduğu için API Management anahtarı yalnızca fiyat sınırlaması ve faturalandırma çağrıları için kullanırız.
   > Gerçek yetkilendirme ve kimlik doğrulama Azure AD B2C tarafından işlenir ve JWT içinde kapsüllenir ve bu, API Management ve daha sonra Azure Işlevleri tarafından iki kez onaylanır.

## <a name="build-the-javascript-spa-to-consume-the-api"></a>API 'YI kullanmak için JavaScript SPA 'yı oluşturma
1. Azure portal depolama hesapları dikey penceresini açın 
1. Oluşturduğunuz hesabı seçin ve ayarlar bölümünden ' statik Web sitesi ' dikey penceresini seçin (' statik Web sitesi ' seçeneğini görmüyorsanız, v2 hesabı oluşturduğunuza bakın).
1. Statik web barındırma özelliğini ' Enabled ' olarak ayarlayın ve Dizin belgesi adını ' index.html ' olarak ayarlayın ve ardından ' Kaydet 'e tıklayın.
1. Birincil uç noktanın içeriğini, bu konum ön uç sitesinin barındırıldığı yerde aklınızda olun. 

   > [!NOTE]
   > Azure Blob Storage + CDN yeniden yazma veya Azure App Service, ancak blob Storage 'ın statik Web sitesi barındırma özelliği, Azure depolama 'dan statik Web içeriği/HTML/JS/CSS hizmeti sunmak için varsayılan bir kapsayıcı sağlar ve bizim için sıfır iş için varsayılan bir sayfa çıkarır.

## <a name="upload-the-js-spa-sample"></a>JS SPA örneğini karşıya yükle
1. Hala depolama hesabı dikey penceresinde, blob hizmeti bölümünde ' blob ' dikey penceresini seçin ve sağ bölmede görüntülenen $web kapsayıcısına tıklayın.
1. Aşağıdaki kodu makinenizde index.html olarak yerel olarak bir dosyaya kaydedin ve sonra index.html dosyasını $web kapsayıcısına yükleyin.

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

1. Son bölümde daha önce depoladığınız statik Web sitesi birincil uç noktasına gidin.

   > [!NOTE]
   > Tebrikler, yalnızca bir JavaScript tek sayfalı uygulamayı, API için anahtarlarınız ile yapılandırmadığınız veya yalnızca Azure AD B2C ayrıntılarınız ile JS uygulamasını yapılandırdığımız için Azure depolama 'ya yalnızca bir JavaScript tek sayfalı uygulama dağıttınız, ancak bunu açtığınızda sayfa çalışmaz.

## <a name="configure-the-js-spa-for-azure-ad-b2c"></a>Azure AD B2C için JS SPA 'yı yapılandırın
1. Artık her şeyin nerede olduğunu biliyoruz: SPA 'yı uygun API Management API adresiyle ve doğru Azure AD B2C uygulama/istemci kimlikleri ile yapılandırabiliriz
1. Azure portal depolama dikey penceresine dönün ve index.html 'ye ve ardından ' blobu Düzenle 'yi seçin. 
1. Kimlik doğrulama ayrıntılarını, daha önce B2C 'de kaydettiğiniz ön uç uygulamanızla eşleşecek şekilde güncelleştirin ve ' b2cScopes ' değerlerinin API arka ucu için olduğunu unutmayın.
1. WebApi anahtar ve API URL 'si, API işleminin API Management test bölmesinde bulunabilir.
1. API Management dikey penceresine geri API Management bir APıM abonelik anahtarı oluşturun, ' abonelikler ' seçeneğini belirleyip ' Abonelik Ekle ' seçeneğini ve ardından kaydı kaydedin. Oluşturulan satırın yanındaki üç nokta (...) simgesine tıklanması, birincil anahtarı kopyalayabilmeniz için anahtarları gösterbilmenizi sağlar.
1. Aşağıdaki kod gibi görünmelidir:-  

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

## <a name="set-the-redirect-uris-for-the-azure-ad-b2c-frontend-app"></a>Azure AD B2C ön uç uygulaması için yeniden yönlendirme URI 'Lerini ayarlayın
1. Azure AD B2C dikey penceresini açın ve JavaScript ön uç uygulaması için uygulama kaydına gidin
1. Yukarıdaki statik Web sitesi birincil uç noktasını daha önce ayarlarken, yeniden yönlendirme URL 'sini not ettiğiniz şekilde ayarlayın

   > [!NOTE] 
   > Bu yapılandırma, ön uç uygulamasının istemcisinin Azure AD B2C uygun taleplerle bir erişim belirteci almasına neden olur.
   > SPA, arka uç API 'sine yapılan çağrıda https üstbilgisine bir taşıyıcı belirteci olarak ekleyebilecektir.
   > API Management, istek temelli Azure Işlev API 'sine geçmeden önce, abonelik anahtarı tarafından uç noktaya göre fiyat sınırı çağrılarını ön doğrulayacak olarak doğrular.
   > SPA, yanıtı tarayıcıda işleyecek.

   > *Tebrikler, Azure AD B2C, Azure API Management, Azure Işlevleri Azure App Service yetkilendirmeyi, mükemmel uyum içinde çalışacak şekilde yapılandırdınız!*

   > [!NOTE]
   > Artık basit bir güvenlik API 'SI olan basit bir uygulama sunuyoruz, bunu test edelim.

## <a name="test-the-client-application"></a>İstemci uygulamasını test etme
1. Daha önce oluşturduğunuz depolama hesabından not ettiğiniz örnek uygulama URL 'sini açın
1. Sağ üst köşede "oturum aç" düğmesine tıklayın, bu tıklama Azure AD B2C oturum açma veya oturum açma profilini açılır.
1. Ekranın "oturum açmış olduğu gibi" bölümünde oturum açma sonrası, JWT 'ınızdan doldurulur.
1. Şimdi "Web API 'Sini çağır" seçeneğine tıklayın ve sayfanın, güvenli API 'nizden geri gönderilen değerlerle güncelleştirilmesi gerekir.

## <a name="and-were-done"></a>Biz de yaptık
Yukarıdaki adımlar, API Management Azure AD B2C birçok farklı kullanılmasına izin vermek için uyarlanmıştır ve düzenlenebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Active Directory ve OAuth 2.0](../active-directory/develop/authentication-scenarios.md)hakkında daha fazla bilgi edinin.
* API Management hakkında daha fazla [videoya](https://azure.microsoft.com/documentation/videos/index/?services=api-management) göz atın.
* Arka uç hizmetinizi güvenli hale getirmeye yönelik diğer yollar için bkz. [Karşılıklı sertifika kimlik doğrulaması](api-management-howto-mutual-certificates.md).
* [API Management hizmet örneği oluşturun](get-started-create-service-instance.md).
* [Ilk API 'Nizi yönetin](import-and-publish.md).
