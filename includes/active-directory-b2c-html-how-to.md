---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/19/2020
ms.author: mimart
ms.openlocfilehash: af11283f9e9dbd925ec994dcb1d96393332b90fc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80116953"
---
## <a name="use-custom-page-content"></a>Özel sayfa içeriğini kullan

Sayfa UI özelleştirmesi özelliğini kullanarak, herhangi bir özel ilkenin görünümünü özelleştirebilirsiniz. Ayrıca bu sayede uygulamanızla Azure AD B2C arasında marka ve görsel tutarlılığı sağlayabilirsiniz.

### <a name="how-it-works"></a>Nasıl çalışır?

Azure AD B2C, [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/)kullanarak müşterinizin tarayıcısında kodu çalıştırır. Çalışma zamanında, içerik, Kullanıcı akışınız veya özel ilkenizde belirttiğiniz bir URL 'den yüklenir. Kullanıcı deneyimindeki her sayfa, içeriğini o sayfa için belirttiğiniz URL 'den yükler. URL 'nizden içerik yüklendikten sonra, Azure AD B2C tarafından yerleştirilen bir HTML parçası ile birleştirilir ve sayfa müşterinize görüntülenir.

![Özel sayfa içeriği kenar boşluğu](./media/active-directory-b2c-html-templates/html-content-merging.png)

## <a name="custom-html-page-content"></a>Özel HTML sayfası içeriği

Özel sayfa içeriğinizi karşılamak için kendi markaınızla bir HTML sayfası oluşturun. Bu sayfa bir statik `*.html` sayfa veya .net, Node.js veya PHP gibi dinamik bir sayfa olabilir.

Özel sayfa içeriğiniz CSS ve JavaScript dahil olmak üzere herhangi bir HTML öğesi içerebilir, ancak iframe 'ler gibi güvenli olmayan öğeleri içeremez. Tek gerekli öğe, `id` `api` HTML sayfanız içinde olduğu gibi, olarak ayarlanmış bir div öğesidir `<div id="api"></div>` .

```html
<!DOCTYPE html>
<html>
<head>
    <title>My Product Brand Name</title>
</head>
<body>
    <div id="api"></div>
</body>
</html>
```

### <a name="customize-the-default-azure-ad-b2c-pages"></a>Varsayılan Azure AD B2C sayfalarını özelleştirme

Özel sayfa içeriğinizi sıfırdan oluşturmak yerine, Azure AD B2C's varsayılan sayfa içeriğini özelleştirebilirsiniz.

Aşağıdaki tabloda, Azure AD B2C tarafından sunulan varsayılan sayfa içeriği listelenmektedir. Dosyaları indirin ve kendi özel sayfalarınızı oluşturmak için bir başlangıç noktası olarak kullanın.

| Varsayılan sayfa | Açıklama | İçerik tanımı KIMLIĞI<br/>(yalnızca özel ilke) |
|:-----------------------|:--------|-------------|
| [exception.html](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Hata sayfası**. Bu sayfa bir özel durum veya hata ile karşılaşıldığında görüntülenir. | *api. Error* |
| [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) |  **Otomatik olarak onaylanan sayfa**. Bu dosyayı bir sosyal hesap kaydolma sayfası, yerel hesap kaydolma sayfası, yerel hesap oturum açma sayfası, parola sıfırlama ve daha fazlası için özel bir sayfa içeriği olarak kullanın. Form, metin girişi kutusu, parola girişi kutusu, radyo düğmesi, tek seçim açılan kutuları ve çoklu seçim onay kutuları gibi çeşitli giriş denetimleri içerebilir. | *api. localaccountsignın*, *api. localaccountsignup*, *API. localaccountpasswordreset*, *API. selfasted* |
| [multifactor-1.0.0.html](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Multi-Factor Authentication sayfası**. Bu sayfada, kullanıcılar telefon numaralarını (metin veya ses kullanarak) kaydolma veya oturum açma sırasında doğrulayabilirler. | *api. phonefactor* |
| [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Profil güncelleştirme sayfası**. Bu sayfa, kullanıcıların profilini güncelleştirmek için erişebileceği bir form içerir. Bu sayfa, parola girişi alanları hariç sosyal hesap kaydolma sayfasına benzerdir. | *api. selfasserted. profileUpdate* |
| [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Birleşik kaydolma veya oturum açma sayfası**. Bu sayfa Kullanıcı kayıt ve oturum açma sürecini işler. Kullanıcılar, kurumsal kimlik sağlayıcılarını, Facebook veya Google + gibi sosyal kimlik sağlayıcılarını veya yerel hesapları kullanabilir. | *api. signuporsignın* |

## <a name="hosting-the-page-content"></a>Sayfa içeriğini barındırma

Kullanıcı arabirimini özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanırken, Kullanıcı arabirimi içeriğinizi CORS 'yi destekleyen genel kullanıma açık bir HTTPS uç noktası üzerinde barındırın. Örneğin, [Azure Blob depolama](../articles/storage/blobs/storage-blobs-introduction.md), [Azure App Services](/azure/app-service/), Web sunucuları, CDNS, AWS S3 veya dosya paylaşım sistemleri.

## <a name="guidelines-for-using-custom-page-content"></a>Özel sayfa içeriğini kullanma yönergeleri

- HTML dosyanıza medya, CSS ve JavaScript dosyaları gibi dış kaynakları dahil ettiğinizde mutlak bir URL kullanın.
- [Sayfa düzeni](../articles/active-directory-b2c/page-layout.md) 1.2.0 ve üstünü kullanarak, `data-preload="true"` CSS ve JavaScript 'in yükleme sırasını denetlemek için özniteliğini HTML etiketinize ekleyebilirsiniz. İle `data-preload=true` , sayfa kullanıcıya gösterilmeden önce oluşturulur. Bu öznitelik, Kullanıcı tarafından gösterilen stilsiz HTML olmadan CSS dosyasını önceden yükleyerek sayfanın "titreşme" olmasını önlemeye yardımcı olur. Aşağıdaki HTML kod parçacığı etiketinin kullanımını gösterir `data-preload` .
  ```HTML
  <link href="https://path-to-your-file/sample.css" rel="stylesheet" type="text/css" data-preload="true"/>
  ```
- Varsayılan sayfa içeriğiyle başlayıp üzerine inşa etmenizi öneririz.
- Her iki [Kullanıcı akışı](../articles/active-directory-b2c/user-flow-javascript-overview.md) ve [özel ilke](../articles/active-directory-b2c/javascript-samples.md)için özel içeriğinize JavaScript ekleyebilirsiniz.
- Desteklenen tarayıcı sürümleri şunlardır:
  - Internet Explorer 11, 10 ve Microsoft Edge
  - Internet Explorer 9 ve 8 için sınırlı destek
  - Google Chrome 42,0 ve üzeri
  - Mozilla Firefox 38,0 ve üzeri
  - İOS ve macOS için Safari, sürüm 12 ve üzeri
- Güvenlik kısıtlamaları nedeniyle, Azure AD B2C `frame` , `iframe` veya `form` HTML öğelerini desteklemez.

## <a name="custom-page-content-walkthrough"></a>Özel sayfa içeriği Kılavuzu

İşleme genel bir bakış aşağıda verilmiştir:

1. Özel sayfa içeriğinizi barındırmak için bir konum hazırlayın (genel olarak erişilebilen, CORS özellikli bir HTTPS uç noktası).
1. Örneğin, varsayılan bir sayfa içerik dosyasını indirip özelleştirin `unified.html` .
1. Özel sayfa içeriğinizi herkese açık olan HTTPS uç noktanızla yayımlayın.
1. Web uygulamanız için çıkış noktaları arası kaynak paylaşımı (CORS) ayarlayın.
1. İlkenize özel ilke içerik URI 'sine işaret edin.

### <a name="1-create-your-html-content"></a>1. HTML içeriğinizi oluşturun

Başlığında ürününüzün marka adıyla özel bir sayfa içeriği oluşturun.

1. Aşağıdaki HTML kod parçacığını kopyalayın. *\<div id="api"\>\</div\>* Etiketler içinde yer alan boş bir öğe ile iyi BIÇIMLENDIRILMIŞ HTML5 *\<body\>* . Bu öğe Azure AD B2C içeriğin nereye ekleneceğini gösterir.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

1. Kopyalanmış parçacığı bir metin düzenleyicisine yapıştırın ve sonra dosyayı *customize-ui.html*olarak kaydedin.

> [!NOTE]
> Login.microsoftonline.com kullanırsanız, güvenlik kısıtlamaları nedeniyle HTML form öğeleri kaldırılır. Özel HTML içerikinizdeki HTML form öğelerini kullanmak istiyorsanız, [b2clogin.com kullanın](../articles/active-directory-b2c/b2clogin.md).

### <a name="2-create-an-azure-blob-storage-account"></a>2. bir Azure Blob depolama hesabı oluşturun

Bu makalede, içeriğimizi barındırmak için Azure Blob depolamayı kullanırız. İçeriğinizi bir Web sunucusunda barındırabilirsiniz, ancak [Web SUNUCUNUZDA CORS 'yi etkinleştirmeniz](https://enable-cors.org/server.html)gerekir.

Blob depolamada HTML içeriğinizi barındırmak için aşağıdaki adımları gerçekleştirin:

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. **Hub** menüsünde, **Yeni**  >  **depolama**  >  **depolama hesabı**' nı seçin.
1. Depolama hesabınız için bir **abonelik** seçin.
1. Bir **kaynak grubu** oluşturun veya var olan bir grubu seçin.
1. Depolama hesabınız için benzersiz bir **ad** girin.
1. Depolama hesabınız için **coğrafi konumu** seçin.
1. **Dağıtım modeli** **Kaynak Yöneticisi**kalabilirler.
1. **Performans** **Standart**kalabilirler.
1. **Hesap türünü** **BLOB depolama alanı**olarak değiştirin.
1. **Çoğaltma** , **RA-GRS**kalabilir.
1. **Erişim katmanı** , **sık**kalabilir.
1. Depolama hesabı oluşturmak için **gözden geçir + oluştur** ' u seçin.
    Dağıtım tamamlandıktan sonra, **depolama hesabı** sayfası otomatik olarak açılır.

#### <a name="21-create-a-container"></a>2,1 kapsayıcı oluşturma

Blob depolamada ortak bir kapsayıcı oluşturmak için aşağıdaki adımları uygulayın:

1. Sol taraftaki menüdeki **BLOB hizmeti** altında **Bloblar**' ı seçin.
1. **+ Kapsayıcı**seçin.
1. **Ad**için *kök*girin. Ad, seçtiğiniz bir ad olabilir, örneğin *contoso*, ancak basitlik için bu örnekteki *kökü* kullanırız.
1. **Genel erişim düzeyi**için **BLOB**' u ve ardından **Tamam**' ı seçin.
1. Yeni kapsayıcıyı açmak için **kök** ' ı seçin.

#### <a name="22-upload-your-custom-page-content-files"></a>2,2 özel sayfa içerik dosyalarınızı karşıya yükleyin

1. **Karşıya Yükle**'yi seçin.
1. **Dosya Seç**' in yanındaki klasör simgesini seçin.
1. Sayfasına gidin ve sayfa UI özelleştirmesi bölümünde daha önce oluşturduğunuz **customize-ui.html**' ı seçin.
1. Bir alt klasöre yüklemek istiyorsanız, **Gelişmiş** ' i genişletin ve klasöre **Yükle ' ye**bir klasör adı girin.
1. **Karşıya Yükle**'yi seçin.
1. Karşıya yüklediğiniz **customize-ui.html** blobu seçin.
1. **URL metin kutusunun** SAĞıNDA, URL 'yi panonuza kopyalamak Için **Panoya Kopyala** simgesini seçin.
1. Web tarayıcısında, karşıya yüklediğiniz Blobun erişilebilir olduğunu doğrulamak için kopyaladığınız URL 'ye gidin. Erişilemezse, örneğin bir `ResourceNotFound` hatayla karşılaşırsanız, kapsayıcı erişim türünün **BLOB**olarak ayarlandığından emin olun.

### <a name="3-configure-cors"></a>3. CORS 'yi yapılandırma

Aşağıdaki adımları gerçekleştirerek, çıkış noktaları arası kaynak paylaşımı için blob depolamayı yapılandırın:

1. Menüsünde **CORS**' yi seçin.
1. **Izin verilen çıkış noktaları**için girin `https://your-tenant-name.b2clogin.com` . `your-tenant-name`Azure AD B2C kiracınızın adıyla değiştirin. Örneğin, `https://fabrikam.b2clogin.com`. Kiracı adınızı girerken tüm küçük harfleri kullanın.
1. **Izin verilen Yöntemler**için hem hem de öğesini seçin `GET` `OPTIONS` .
1. **Izin verilen üst bilgiler**için bir yıldız işareti (*) girin.
1. **Gösterilen üstbilgiler**için bir yıldız işareti (*) girin.
1. **Maksimum yaş**için 200 girin.
1. **Kaydet**'i seçin.

#### <a name="31-test-cors"></a>3,1 test CORS

Aşağıdaki adımları gerçekleştirerek hazırlandığınızı doğrulayın:

1. CORS yapılandırma adımını tekrarlayın. **Izin verilen çıkış noktaları**için şunu girin`https://www.test-cors.org`
1. [Www.test-CORS.org](https://www.test-cors.org/) adresine gidin 
1. **Uzak URL** kutusu IÇIN, HTML dosyanızın URL 'sini yapıştırın. Örneğin, `https://your-account.blob.core.windows.net/azure-ad-b2c/unified.html`
1. **Istek gönder**' i seçin.
    Sonuç olmalıdır `XHR status: 200` . 
    Bir hata alırsanız, CORS ayarlarınızın doğru olduğundan emin olun. Ayrıca CTRL + SHIFT + P tuşlarına basarak tarayıcı önbelleğinizi temizlemeniz veya özel bir gözatma oturumu açmanız gerekebilir.
