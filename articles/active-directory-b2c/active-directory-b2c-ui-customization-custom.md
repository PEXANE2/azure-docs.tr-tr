---
title: Azure Active Directory B2C | içinde özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme | Microsoft Docs
description: Azure Active Directory B2C özel bir ilke kullanarak Kullanıcı arabirimini özelleştirme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: e79d7a4b97f010b035f5c864682b4d3882a21393
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70171910"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Azure Active Directory B2C özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Bu makaleyi tamamladıktan sonra, marka ve görünümünüzdeki bir kaydolma ve oturum açma özel ilkesine sahip olursunuz. Azure Active Directory B2C (Azure AD B2C) sayesinde, kullanıcılara sunulan HTML ve CSS içeriğinin neredeyse tam denetimini alırsınız. Özel bir ilke kullandığınızda, Azure portal denetimleri kullanmak yerine, XML 'de UI özelleştirmesini yapılandırırsınız. 

## <a name="prerequisites"></a>Önkoşullar

[Özel ilkelerle çalışmaya başlama](active-directory-b2c-get-started-custom.md)bölümündeki adımları uygulayın. Kaydolma ve yerel hesaplarla oturum açma için çalışan bir özel ilkenize sahip olmanız gerekir.

## <a name="page-ui-customization"></a>Sayfa UI Özelleştirmesi

Sayfa UI özelleştirmesi özelliğini kullanarak, herhangi bir özel ilkenin görünümünü özelleştirebilirsiniz. Ayrıca bu sayede uygulamanızla Azure AD B2C arasında marka ve görsel tutarlılığı sağlayabilirsiniz.

Şöyle çalışır: Azure AD B2C, müşterinizin tarayıcısında kodu çalıştırır ve [çıkış noktaları arası kaynak paylaşımı (CORS)](https://www.w3.org/TR/cors/)adlı modern bir yaklaşım kullanır. İlk olarak, özel ilkede özelleştirilmiş HTML içeriğiyle bir URL belirtirsiniz. Azure AD B2C, kullanıcı arabirimi öğelerini URL'nizden yüklenen HTML içeriğiyle birleştirdikten sonra sayfayı müşteriye gösterir.

## <a name="create-your-html5-content"></a>HTML5 içeriğinizi oluşturma

Başlığında ürününüzün marka adıyla HTML içeriği oluşturun.

1. Aşağıdaki HTML kod parçacığını kopyalayın. *\<\>\<\>* *Gövde etiketleriiçinde\<yer alan div ID = "api"/div adlı boş bir öğe ile iyi biçimlendirilmiş HTML5.\>* Bu öğe Azure AD B2C içeriğin nereye ekleneceğini gösterir.

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

2. Kopyalanmış parçacığı bir metin düzenleyicisine yapıştırın, sonra dosyayı *Customize-ui. html*olarak kaydedin.

> [!NOTE]
> Login.microsoftonline.com kullanırsanız, güvenlik kısıtlamaları nedeniyle HTML form öğeleri kaldırılır. Özel HTML içerikinizdeki HTML form öğelerini kullanmak istiyorsanız lütfen b2clogin.com kullanın. Diğer avantajlar için bkz. [B2clogin.com kullanma](b2clogin.md) .

## <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob Depolama hesabı oluşturma

>[!NOTE]
> Bu makalede, içeriğimizi barındırmak için Azure Blob depolamayı kullanırız. İçeriğinizi bir Web sunucusunda barındırabilirsiniz, ancak [Web SUNUCUNUZDA CORS 'yi etkinleştirmeniz](https://enable-cors.org/server.html)gerekir.

Bu HTML içeriğini blob depolamada barındırmak için aşağıdakileri yapın:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Hub** menüsünde, **Yeni** > **depolama** > **depolama hesabı**' nı seçin.
3. Depolama hesabınız için bir **abonelik** seçin.
4. Bir **kaynak grubu** oluşturun veya var olan bir grubu seçin.
5. Depolama hesabınız için benzersiz bir **ad** girin.
6. Depolama hesabınız için **coğrafi konumu** seçin. 
7. **Dağıtım modeli** **Kaynak Yöneticisi**kalabilirler.
8. **Performans** **Standart**kalabilirler.
9. **Hesap türünü** **BLOB depolama alanı**olarak değiştirin.
10. **Çoğaltma** , **RA-GRS**kalabilir.
11. **Erişim katmanı** , **sık**kalabilir. 
12. Depolama hesabı oluşturmak için **gözden geçir + oluştur** ' a tıklayın.  
    Dağıtım tamamlandıktan sonra, **depolama hesabı** dikey penceresi otomatik olarak açılır.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Blob depolamada ortak bir kapsayıcı oluşturmak için aşağıdakileri yapın:

1. Sol taraftaki menüdeki **BLOB hizmeti** altında Bloblar ' ı seçin.
2. **+ Kapsayıcı**' ya tıklayın.
3. **Ad**için *kök*girin. Bu, seçtiğimiz bir ad olabilir, örneğin *wingtiptoys*, ancak basitlik için bu örnekteki *kökü* kullanırız.
4. **Genel erişim düzeyi**için **BLOB**' u ve ardından **Tamam**' ı seçin.
5. Yeni kapsayıcıyı açmak için **kök** öğesine tıklayın.
6. **Karşıya Yükle**'ye tıklayın.
7. **Dosya Seç**' in yanındaki klasör simgesine tıklayın.
8. Sayfasına gidin ve sayfa UI özelleştirmesi bölümünde daha önce oluşturduğunuz **Customize-ui. html** dosyasını seçin.
9. Bir alt klasöre yüklemek istiyorsanız, **Gelişmiş** ' i genişletin ve klasöre **Yükle ' ye**bir klasör adı girin.
10. **Karşıya Yükle**’yi seçin.
11. Karşıya yüklediğiniz **Customize-ui. html** blobu seçin.
12. URL metin kutusunun sağında, URL 'yi panonuza kopyalamak Için **Panoya Kopyala** simgesini seçin.
13. Web tarayıcısında, karşıya yüklediğiniz Blobun erişilebilir olduğunu doğrulamak için kopyaladığınız URL 'ye gidin. Erişilemezse, örneğin bir `ResourceNotFound` hatayla karşılaşırsanız, kapsayıcı erişim türünün **BLOB**olarak ayarlandığından emin olun.

## <a name="configure-cors"></a>CORS Yapılandırma

Aşağıdaki işlemleri gerçekleştirerek, çıkış noktaları arası kaynak paylaşımı için blob depolamayı yapılandırın:

1. Menüsünde **CORS**' yi seçin.
2. **Izin verilen çıkış noktaları**için `https://your-tenant-name.b2clogin.com`girin. Değiştirin `your-tenant-name` Azure AD B2C kiracınızın adı. Örneğin: `https://fabrikam.b2clogin.com`. Kiracı adınızı girerken tüm küçük harfleri kullanmanız gerekir.
3. **İzin verilen Yöntemler**için hem hem `GET` de `OPTIONS`öğesini seçin.
4. **Izin verilen üst bilgiler**için bir yıldız işareti (*) girin.
5. **Gösterilen üstbilgiler**için bir yıldız işareti (*) girin.
6. **Maksimum yaş**için 200 girin.
7. **Kaydet**’e tıklayın.

## <a name="test-cors"></a>Test CORS

Aşağıdakileri yaparak hazırsanız emin olun:

1. [Www.test-CORS.org](https://www.test-cors.org/) Web sitesine gidin ve ardından **uzak URL** kutusuna URL 'yi yapıştırın.
2. **Istek gönder**' e tıklayın.  
    Bir hata alırsanız, [CORS ayarlarınızın](#configure-cors) doğru olduğundan emin olun. Ayrıca CTRL + SHIFT + P tuşlarına basarak tarayıcı önbelleğinizi temizlemeniz veya özel bir gözatma oturumu açmanız gerekebilir.

## <a name="modify-the-extensions-file"></a>Uzantılar dosyasını değiştirme

UI özelleştirmesini yapılandırmak için, **ContentDefinition** ve onun alt öğelerini temel dosyadan uzantılar dosyasına kopyalayın.

1. İlkenizin temel dosyasını açın. Örneğin, *TrustFrameworkBase. xml*.
2. **ContentDefinitions** öğesinin tüm içeriğini arayın ve kopyalayın.
3. Uzantı dosyasını açın. Örneğin, *TrustFrameworkExtensions. xml*. **Buildingblocks** öğesi için arama yapın. Öğe yoksa, ekleyin.
4. **Buildingblocks** öğesinin bir alt öğesi olarak kopyaladığınız **ContentDefinitions** öğesinin tüm içeriğini yapıştırın. 
5. Kopyaladığınız XML içinde içeren `Id="api.signuporsignin"` **ContentDefinition** öğesini arayın.
6. **Loaduri** değerini, depolama ALANıNA yüklediğiniz HTML dosyasının URL 'si ile değiştirin. Örneğin: `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.
    
    Özel ilkeniz aşağıdaki gibi görünmelidir:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

7. Uzantı dosyasını kaydedin.

## <a name="upload-your-updated-custom-policy"></a>Güncelleştirilmiş özel ilkenizi karşıya yükleyin

1. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Kimlik deneyimi çerçevesini**seçin.
2. **Tüm ilkeler**' e tıklayın.
3. **Ilkeyi karşıya yükle**' ye tıklayın.
4. Daha önce değiştirdiğiniz uzantıları dosyasını karşıya yükleyin.

## <a name="test-the-custom-policy-by-using-run-now"></a>**Şimdi Çalıştır** 'ı kullanarak özel ilkeyi test etme

1. **Azure AD B2C** dikey penceresinde **Tüm ilkeler**' e gidin.
2. Karşıya yüklediğiniz özel ilkeyi seçin ve **Şimdi Çalıştır** düğmesine tıklayın.
3. Bir e-posta adresi kullanarak kaydolabilirsiniz.

## <a name="reference"></a>Başvuru

### <a name="sample-templates"></a>Örnek şablonlar
UI özelleştirmesi için örnek şablonları buradan bulabilirsiniz:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Sample_templates/Wingtip klasörü aşağıdaki HTML dosyalarını içerir:

| HTML5 şablonu | Açıklama |
|----------------|-------------|
| *PhoneFactor. html* | Bu dosyayı Multi-Factor Authentication sayfası için şablon olarak kullanın. |
| *ResetPassword. html* | Parolayı unuttum sayfası için bu dosyayı şablon olarak kullanın. |
| *selfasserted.html* | Bu dosyayı bir sosyal hesap kaydolma sayfası, yerel hesap kaydolma sayfası veya yerel hesap oturum açma sayfası için şablon olarak kullanın. |
| *Birleşik. html* | Bu dosyayı Birleşik kaydolma veya oturum açma sayfası için şablon olarak kullanın. |
| *updateprofile.html* | Bu dosyayı profil güncelleştirme sayfası için şablon olarak kullanın. |

Örneğin nasıl kullanılacağına ilişkin adımlar aşağıda verilmiştir. 
1. Yerel makinenizde depoyu kopyalayın. Sample_templates altında bir şablon klasörü seçin. `wingtip` Veya`contoso`kullanabilirsiniz.
2. Önceki bölümlerde açıklandığı gibi `css`, `fonts`, ve `images` klasörlerinin altındaki tüm dosyaları BLOB depolama alanına yükleyin. 
3. Ardından \*, her `wingtip` bir. html dosyasını veya `contoso` (ilk adımda seçtiğiniz) kök dizininde açın ve tüm "http://localhost" örneklerini 2. adımda karşıya yüklediğiniz CSS, resim ve yazı tipi dosyalarının URL 'leriyle değiştirin.
4. \*. Html dosyalarını kaydedin ve BLOB depolamaya yükleyin.
5. Şimdi uzantılar dosyasını [değiştirme](#modify-the-extensions-file)bölümünde belirtildiği gibi uzantılar dosyasını değiştirin.
6. Eksik yazı tiplerini, görüntüleri veya CSS 'yi görürseniz, lütfen uzantılar ilkesi ve \*. html dosyaları içindeki başvurularınızı kontrol edin.

### <a name="content-defintion-ids"></a>İçerik tanımı kimlikleri

Kaydolma veya oturum açma özel ilkesini değiştirme bölümünde, için `api.idpselections`içerik tanımını yapılandırdınız. Azure AD B2C Identity Experience Framework ve açıklamaları tarafından tanınan içerik tanımı kimliklerinin tam kümesi aşağıdaki tabloda verilmiştir:

| İçerik tanımı KIMLIĞI | Açıklama | 
|-----------------------|-------------|
| *api.error* | **Hata sayfası**. Bu sayfa bir özel durum veya hata ile karşılaşıldığında görüntülenir. |
| *api. ıdpseçimlerin* | **Kimlik sağlayıcısı seçim sayfası**. Bu sayfa, kullanıcının oturum açma sırasında aralarından seçim yapabileceğiniz kimlik sağlayıcılarının bir listesini içerir. Bu seçenekler, kurumsal kimlik sağlayıcılarıdır, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar. |
| *api.idpselections.signup* | **Kaydolma Için kimlik sağlayıcısı seçimi**. Bu sayfa, kullanıcının kaydolma sırasında aralarından seçim yapabileceğiniz kimlik sağlayıcılarının bir listesini içerir. Bu seçenekler, kurumsal kimlik sağlayıcılarıdır, Facebook ve Google + gibi sosyal kimlik sağlayıcıları ya da yerel hesaplar. |
| *api.localaccountpasswordreset* | **Parolayı unuttum sayfası**. Bu sayfa, kullanıcının parola sıfırlama işlemini başlatmak için tamamlaması gereken bir form içerir.  |
| *api.localaccountsignin* | **Yerel hesap oturum açma sayfası**. Bu sayfa, bir e-posta adresine veya Kullanıcı adına dayalı bir yerel hesapla oturum açmak için bir oturum açma formu içerir. Form bir metin girişi kutusu ve parola giriş kutusu içerebilir. |
| *api.localaccountsignup* | **Yerel hesap kaydolma sayfası**. Bu sayfa, bir e-posta adresini veya Kullanıcı adını temel alan bir yerel hesaba kaydolma için bir kayıt formu içerir. Form, metin girişi kutusu, parola girişi kutusu, radyo düğmesi, tek seçim açılan kutuları ve çoklu seçim onay kutuları gibi çeşitli giriş denetimleri içerebilir. |
| *api. phonefactor* | **Multi-Factor Authentication sayfası**. Bu sayfada, kullanıcılar telefon numaralarını (metin veya ses kullanarak) kaydolma veya oturum açma sırasında doğrulayabilirler. |
| *api.selfasserted* | **Sosyal hesap kaydolma sayfası**. Bu sayfa, kullanıcıların Facebook veya Google + gibi bir sosyal kimlik sağlayıcısından mevcut bir hesabı kullanarak kaydolduklarında tamamlaması gereken bir kaydolma formu içerir. Bu sayfa, parola girişi alanları hariç, önceki sosyal hesap kaydolma sayfasına benzer. |
| *api.selfasserted.profileupdate* | **Profil güncelleştirme sayfası**. Bu sayfa, kullanıcıların profilini güncelleştirmek için kullanabileceği bir form içerir. Bu sayfa, parola girişi alanları hariç sosyal hesap kaydolma sayfasına benzerdir. |
| *api.signuporsignin* | **Birleşik kaydolma veya oturum açma sayfası**. Bu sayfa, kullanıcıların kaydolma ve oturum açma öğelerinin yanı sıra kurumsal kimlik sağlayıcıları, Facebook veya Google + gibi sosyal kimlik sağlayıcıları veya yerel hesaplar tarafından kullanılabilir.  |

## <a name="next-steps"></a>Sonraki adımlar

Özelleştirilebilen UI öğeleri hakkında daha fazla bilgi için bkz. [Yerleşik ilkeler IÇIN UI özelleştirmesi için başvuru kılavuzu](active-directory-b2c-reference-ui-customization.md).
