---
author: msmimart
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 03/19/2020
ms.author: mimart
ms.openlocfilehash: af11283f9e9dbd925ec994dcb1d96393332b90fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80116953"
---
## <a name="use-custom-page-content"></a>Özel sayfa içeriğini kullanma

Sayfa Kullanıcı Arabirimi özelleştirme özelliğini kullanarak, herhangi bir özel ilkenin görünümünü ve hissini özelleştirebilirsiniz. Ayrıca bu sayede uygulamanızla Azure AD B2C arasında marka ve görsel tutarlılığı sağlayabilirsiniz.

### <a name="how-it-works"></a>Nasıl çalışır?

Azure AD B2C, Çapraz Kaynak Paylaşımı [(CORS)](https://www.w3.org/TR/cors/)kullanarak müşterinizin tarayıcısında kod çalıştırır. Çalışma zamanında, içerik kullanıcı akışınızda veya özel politikanızda belirttiğiniz bir URL'den yüklenir. Kullanıcı deneyimindeki her sayfa, içeriğini o sayfa için belirttiğiniz URL'den yükler. Url'nizden içerik yüklendikten sonra, Azure AD B2C tarafından eklenen bir HTML parçasıyla birleştirilir ve sayfa müşterinize gösterilir.

![Özel sayfa içeriği kenar boşluğu](./media/active-directory-b2c-html-templates/html-content-merging.png)

## <a name="custom-html-page-content"></a>Özel HTML sayfa içeriği

Özel sayfa içeriğinize hizmet etmek için kendi markanızla bir HTML sayfası oluşturun. Bu sayfa statik `*.html` bir sayfa veya .NET, Node.js veya PHP gibi dinamik bir sayfa olabilir.

Özel sayfa içeriğiniz CSS ve JavaScript dahil olmak üzere tüm HTML öğelerini içerebilir, ancak iframe'ler gibi güvenli olmayan öğeler içeremez. Gerekli tek `id` `api`öğe, HTML sayfanızdaki bu `<div id="api"></div>` gibi ayarlanmış bir div öğesidir.

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

Özel sayfa içeriğinizi sıfırdan oluşturmak yerine, Azure AD B2C'nin varsayılan sayfa içeriğini özelleştirebilirsiniz.

Aşağıdaki tabloda Azure AD B2C tarafından sağlanan varsayılan sayfa içeriği listelenistir. Dosyaları indirin ve bunları kendi özel sayfalarınızı oluşturmak için bir başlangıç noktası olarak kullanın.

| Varsayılan sayfa | Açıklama | İçerik tanım kimliği<br/>(yalnızca özel ilke) |
|:-----------------------|:--------|-------------|
| [özel durum.html](https://login.microsoftonline.com/static/tenant/default/exception.cshtml) | **Hata sayfası**. Bir özel durum veya bir hata yla karşılaşıldığında bu sayfa görüntülenir. | *api.error* |
| [selfasserted.html](https://login.microsoftonline.com/static/tenant/default/selfAsserted.cshtml) |  **Kendi Kendine Ileri Sayfa**. Bu dosyayı, sosyal hesap kayıt sayfası, yerel hesap kayıt sayfası, yerel hesap oturum açma sayfası, parola sıfırlama ve daha fazlası için özel bir sayfa içeriği olarak kullanın. Form, metin giriş kutusu, parola giriş kutusu, radyo düğmesi, tek seçimli açılır bırakma kutuları ve çok seçimli onay kutuları gibi çeşitli giriş denetimleri içerebilir. | *api.localaccountsignin*, *api.localaccountsignup*, *api.localaccountpasswordreset*, *api.selfasserted* |
| [çok faktörlü-1.0.0.html](https://login.microsoftonline.com/static/tenant/default/multifactor-1.0.0.cshtml) | **Çok faktörlü kimlik doğrulama sayfası.** Bu sayfada, kullanıcılar kaydolma veya oturum açma sırasında telefon numaralarını (metin veya ses kullanarak) doğrulayabilirler. | *api.phonefactor* |
| [updateprofile.html](https://login.microsoftonline.com/static/tenant/default/updateProfile.cshtml) | **Profil güncelleme sayfası**. Bu sayfa, kullanıcıların profillerini güncelleştirmek için erişebileceği bir form içerir. Bu sayfa, parola giriş alanları dışında sosyal hesap kayıt sayfasına benzer. | *api.selfasserted.profileupdate* |
| [unified.html](https://login.microsoftonline.com/static/tenant/default/unified.cshtml) | **Birleşik kaydolma veya oturum açma sayfası.** Bu sayfa, kullanıcı kayıt ve oturum açma işlemini işler. Kullanıcılar kurumsal kimlik sağlayıcılarını, Facebook veya Google+ gibi sosyal kimlik sağlayıcılarını veya yerel hesapları kullanabilir. | *api.signuporsignin* |

## <a name="hosting-the-page-content"></a>Sayfa içeriğini barındırma

Kullanıcı Arabirimi'ni özelleştirmek için kendi HTML ve CSS dosyalarınızı kullanırken, Kullanıcı Arabirimi içeriğinizi CORS'leri destekleyen herkese açık herhangi bir HTTPS bitiş noktasında barındırın. Örneğin, [Azure Blob depolama,](../articles/storage/blobs/storage-blobs-introduction.md) [Azure Uygulama Hizmetleri,](/azure/app-service/)web sunucuları, CDN'ler, AWS S3 veya dosya paylaşım sistemleri.

## <a name="guidelines-for-using-custom-page-content"></a>Özel sayfa içeriğini kullanma yönergeleri

- HTML dosyanıza medya, CSS ve JavaScript dosyaları gibi dış kaynakları eklerken mutlak bir URL kullanın.
- [Sayfa düzeni sürüm](../articles/active-directory-b2c/page-layout.md) 1.2.0 ve üzerini `data-preload="true"` kullanarak, CSS ve JavaScript için yükleme sırasını denetlemek için HTML etiketlerinizde özniteliği ekleyebilirsiniz. , `data-preload=true`sayfa kullanıcıya gösterilmeden önce oluşturulur. Bu öznitelik, kullanıcıya stil değiştirilmemiş HTML gösterilmeden CSS dosyasını önceden yükleyerek sayfanın "titremesini" önlemeye yardımcı olur. Aşağıdaki HTML kod parçacığı `data-preload` etiketin kullanımını gösterir.
  ```HTML
  <link href="https://path-to-your-file/sample.css" rel="stylesheet" type="text/css" data-preload="true"/>
  ```
- Varsayılan sayfa içeriğiyle başlayıp üzerine oluşturmanızı öneririz.
- JavaScript'i hem [kullanıcı akışları](../articles/active-directory-b2c/user-flow-javascript-overview.md) hem de [özel ilkeler](../articles/active-directory-b2c/javascript-samples.md)için özel içeriğinize ekleyebilirsiniz.
- Desteklenen tarayıcı sürümleri şunlardır:
  - Internet Explorer 11, 10 ve Microsoft Edge
  - Internet Explorer 9 ve 8 için sınırlı destek
  - Google Chrome 42.0 ve üzeri
  - Mozilla Firefox 38.0 ve üzeri
  - iOS ve macOS için Safari, sürüm 12 ve üzeri
- Güvenlik kısıtlamaları nedeniyle Azure AD B2C, `frame` `iframe` `form` HTML öğelerini desteklemez.

## <a name="custom-page-content-walkthrough"></a>Özel sayfa içeriği gözden geçirme

İşte sürecin genel bir özeti:

1. Özel sayfa içeriğinizi barındıracak bir konum hazırlayın (herkese açık, CORS özellikli BIR HTTPS bitiş noktası).
1. Örneğin `unified.html`varsayılan sayfa içeriği dosyasını indirin ve özelleştirin.
1. Özel sayfa içeriğinizi herkese açık HTTPS bitiş noktanızı yayınlayın.
1. Web uygulamanız için başlangıçlar arası kaynak paylaşımını (CORS) ayarlayın.
1. İlkenizi özel ilke içeriğiURI'nize yönlendirin.

### <a name="1-create-your-html-content"></a>1. HTML içeriğinizi oluşturun

Başlıkta ürününüzün marka adını içeren özel bir sayfa içeriği oluşturun.

1. Aşağıdaki HTML parçacığıkopyalayın. * \<\>\<\> * *Gövde\> etiketleri içinde bulunan div id="api" /div adı verilen boş bir elemanla iyi biçimlendirilmiş HTML5'tir. \<* Bu öğe, Azure AD B2C içeriğinin nereye ekleneceğini gösterir.

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

1. Kopyalanan parçacığı metin düzenleyiciye yapıştırın ve ardından dosyayı *özelleştir-ui.html olarak kaydedin.*

> [!NOTE]
> html form öğeleri, login.microsoftonline.com kullanırsanız güvenlik kısıtlamaları nedeniyle kaldırılır. Özel HTML içeriğinizde HTML form öğelerini kullanmak istiyorsanız, [b2clogin.com kullanın.](../articles/active-directory-b2c/b2clogin.md)

### <a name="2-create-an-azure-blob-storage-account"></a>2. Azure Blob depolama hesabı oluşturma

Bu makalede, içeriğimizi barındırmak için Azure Blob depolama alanını kullanıyoruz. İçeriğinizi bir web sunucusunda barındırmayı seçebilirsiniz, ancak [WEB SUNUCUNUZDA CORS'u etkinleştirmeniz](https://enable-cors.org/server.html)gerekir.

HTML içeriğinizi Blob depolama alanında barındırmak için aşağıdaki adımları gerçekleştirin:

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. **Hub** menüsünde **Yeni** > Depolama**Depolama hesabı'nı****Storage** > seçin.
1. Depolama hesabınız için bir **Abonelik** seçin.
1. Bir **Kaynak grubu** oluşturun veya varolan bir grup seçin.
1. Depolama hesabınız için benzersiz bir **Ad** girin.
1. Depolama hesabınız için **Coğrafi konumu** seçin.
1. **Dağıtım modeli** **Kaynak Yöneticisi**olarak kalabilir.
1. **Performans** **Standart**olarak kalabilir.
1. **Hesap Türünü** **Blob depolamasına**değiştirin.
1. **Çoğaltma** **RA-GRS**kalabilir.
1. **Erişim katmanı** **Sıcak**kalabilir.
1. Depolama hesabı oluşturmak için **Gözden Geçir + oluştur'u** seçin.
    Dağıtım tamamlandıktan **sonra, Depolama hesabı** sayfası otomatik olarak açılır.

#### <a name="21-create-a-container"></a>2.1 Kapsayıcı oluşturma

Blob depolama alanında ortak bir kapsayıcı oluşturmak için aşağıdaki adımları gerçekleştirin:

1. Sol menüdeki **Blob servisi** altında **Blobs'u**seçin.
1. **+Kapsayıcı'yı**seçin.
1. **Ad**için , *kök*girin . Ad seçtiğiniz bir isim olabilir, örneğin *contoso*, ama biz basitlik için bu örnekte *kök* kullanın.
1. **Ortak erişim düzeyi**için, **Blob**seçin, sonra **Tamam**.
1. Yeni kapsayıcıyı açmak için **kök** seçin.

#### <a name="22-upload-your-custom-page-content-files"></a>2.2 Özel sayfa içerik dosyalarınızı yükleme

1. **Yükle'yi**seçin.
1. **Dosya seç'in**yanındaki klasör simgesini seçin.
1. Sayfa Kullanıcı Arama Birimi özelleştirme bölümünde daha önce oluşturduğunuz **özelleştir-ui.html'e**gidin ve seçin.
1. Bir alt klasöre yüklemek istiyorsanız, **Gelişmiş'i** genişletin ve **Yükle klasörüne**bir klasör adı girin.
1. **Yükle'yi**seçin.
1. Yüklediğiniz **customize-ui.html** blob'u seçin.
1. **URL** metin kutusunun sağında, URL'yi panonuza kopyalamak **için pano simgesine** kopyala'yı seçin.
1. Web tarayıcısında, yüklediğiniz blob'un erişilebilir olduğunu doğrulamak için kopyaladığınız URL'ye gidin. Erişilemiyorsa, örneğin bir `ResourceNotFound` hatayla karşılaşırsanız, kapsayıcı erişim türünün **blob**olarak ayarlandığınızdan emin olun.

### <a name="3-configure-cors"></a>3. Cors'u yapılandır

Aşağıdaki adımları gerçekleştirerek Çapraz Kaynak Paylaşımı için Blob depolamasını yapılandırın:

1. Menüde **CORS'i**seçin.
1. **İzin verilen kökenler**için , girin. `https://your-tenant-name.b2clogin.com` Azure `your-tenant-name` AD B2C kiracınızın adıyla değiştirin. Örneğin, `https://fabrikam.b2clogin.com`. Kiracı adınızı girerken tüm küçük harfleri kullanın.
1. **İzin Verilen Yöntemler** `GET` için, her ikisini de seçin ve. `OPTIONS`
1. **İzin Verilen Üstbilgi için**yıldız işareti (*) girin.
1. **Exposed Üstbilgi için**yıldız işareti (*) girin.
1. **Max yaş**için, 200 girin.
1. **Kaydet'i**seçin.

#### <a name="31-test-cors"></a>3.1 Test CORS

Aşağıdaki adımları gerçekleştirerek hazır olduğunuzu doğrulayın:

1. Cors'u yapılandırın. **İzin verilen kökenler**için , girin`https://www.test-cors.org`
1. [www.test-cors.org'ye](https://www.test-cors.org/) gidin 
1. Uzak **URL** kutusu için HTML dosyanızın URL'sini yapıştırın. Örneğin, `https://your-account.blob.core.windows.net/azure-ad-b2c/unified.html`
1. **İstek Gönder'i**seçin.
    Sonuç. `XHR status: 200` 
    Bir hata alırsanız, CORS ayarlarınızın doğru olduğundan emin olun. Ayrıca Ctrl+Shift+P tuşuna basarak tarayıcı önbelleğinizi temizlemeniz veya özel bir tarama oturumu açmanız gerekebilir.
