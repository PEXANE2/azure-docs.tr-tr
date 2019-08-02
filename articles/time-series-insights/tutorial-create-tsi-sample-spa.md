---
title: 'Öğretici: Azure Time Series Insights tek sayfalı Web uygulaması oluşturma | Microsoft Docs'
description: Azure Time Series Insights ortamından verileri sorgulayan ve işleyen tek sayfalı bir Web uygulaması oluşturmayı öğrenin.
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 06/29/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 4d9af918c222107cfca5863309efb391b8e6d2e0
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720876"
---
# <a name="tutorial-create-an-azure-time-series-insights-single-page-web-app"></a>Öğretici: Azure Time Series Insights tek sayfalı web uygulaması oluşturma

Bu öğretici, Azure Time Series Insights verilerine erişmek için kendi tek sayfalı web uygulamanızı (SPA) oluşturma sürecinde size rehberlik eder.

Bu öğreticide şu konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure Active Directory kaydetme (Azure AD)
> * Web uygulamanızı derleme, yayımlama ve test etme

> [!NOTE]
> * Bu öğreticinin kaynak kodu [GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)' da verilmiştir.
> * Time Series Insights [istemci örnek uygulaması](https://insights.timeseries.azure.com/clientsample) , bu öğreticide kullanılan tamamlanmış uygulamayı göstermek için barındırılır.

Henüz yoksa ücretsiz bir [Azure aboneliğine](https://azure.microsoft.com/free/) kaydolun.

## <a name="prerequisites"></a>Önkoşullar

* Visual Studio 'nun ücretsiz bir kopyası. Başlamak için [2017 veya 2019 Community sürümlerini](https://www.visualstudio.com/downloads/) indirin.

* Visual Studio için IIS Express, Web Dağıtımı ve Azure Cloud Services Core araçları bileşenleri. Visual Studio yüklemenizi değiştirerek bileşenleri ekleyin.

## <a name="understand-application-design"></a>Uygulama tasarımını anlama

Time Series Insights örnek SPA, bu öğreticide kullanılan tasarımın ve kodun temelini oluşturur. Kod Time Series Insights JavaScript istemci kitaplığını kullanır. Time Series Insights istemci kitaplığı, iki ana API kategorisi için bir soyutlama sağlar:

- **Time Series Insights sorgu API 'lerini çağırmak Için sarmalayıcı yöntemleri**: JSON tabanlı ifadeler kullanarak Time Series Insights verilerini sorgulamak için kullanabileceğiniz REST API 'Leri. Yöntemler, kitaplığın TsiClient. Server ad alanı altında düzenlenir.

- **Çeşitli grafik denetimleri türlerini oluşturma ve doldurma yöntemleri**: Bir Web sayfasında Time Series Insights verileri görselleştirmek için kullanabileceğiniz yöntemler. Yöntemler, kitaplığın TsiClient. ux ad alanı altında düzenlenir.

Bu öğretici, örnek uygulamanın Time Series Insights ortamındaki verileri de kullanır. Time Series Insights örnek uygulamasının yapısı ve Time Series Insights istemci kitaplığı 'nın nasıl kullanıldığı hakkında ayrıntılar için, bkz. [Azure Time Series Insights JavaScript istemci kitaplığı 'Nı araştırma](tutorial-explore-js-client-lib.md).

## <a name="register-with-azure-ad"></a>Azure AD 'ye kaydolun

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

## <a name="build-and-publish"></a>Derleme ve yayımlama

1. Uygulamanızın proje dosyalarını depolamak için bir dizin oluşturun. Ardından, aşağıdaki URL 'Lerin her birine gidin. Sayfanın sağ üst köşesindeki **RAW** bağlantısına sağ tıklayın ve sonra da dosyaları proje dizininize kaydetmek Için **farklı kaydet** ' i seçin.

   - [*index. html*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html): sayfa için HTML ve JavaScript
   - [*Samplestyles. css*](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/sampleStyles.css): CSS stil sayfası

   > [!NOTE]
   > Tarayıcıya bağlı olarak, dosyayı kaydetmeden önce dosya uzantılarını. html veya. CSS olarak değiştirmeniz gerekebilir.

1. Gerekli bileşenlerin Visual Studio 'da yüklü olduğunu doğrulayın. Visual Studio için IIS Express, Web Dağıtımı ve Azure Cloud Services Core araçları bileşenleri yüklü olmalıdır.

    [![Visual Studio-yüklü bileşenleri değiştirme](media/tutorial-create-tsi-sample-spa/vs-installation.png)](media/tutorial-create-tsi-sample-spa/vs-installation.png#lightbox)

    > [!NOTE]
    > Visual Studio deneyiminiz, sürümünüze ve yapılandırma ayarlarınıza bağlı olarak, gösterilen örneklerden biraz farklılık gösterebilir.

1. Visual Studio 'Yu açın ve oturum açın. Web uygulaması için bir proje oluşturmak için **Dosya** menüsünde,**Web sitesi** **Aç** > ' ı seçin.

    [![Visual Studio-yeni çözüm oluşturma](media/tutorial-create-tsi-sample-spa/vs-solution-create.png)](media/tutorial-create-tsi-sample-spa/vs-solution-create.png#lightbox)

1. **Web sitesi aç** BÖLMESINDE, HTML ve CSS dosyalarını depoladığınız çalışma dizinini seçip **Aç**' ı seçin.

   [![Visual Studio-açık ve Web sitesi seçenekleriyle Dosya menüsü](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png)](media/tutorial-create-tsi-sample-spa/vs-file-open-web-site.png#lightbox)

1. Visual Studio **Görünüm** menüsünde **Çözüm Gezgini**' yi seçin. Yeni çözümünüz açılır. HTML ve CSS dosyalarını içeren bir Web sitesi projesi (Dünya simgesi) içerir.

   [![Visual Studio-Çözüm Gezgini yeni çözüm](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer.png#lightbox)

1. Uygulamanızı yayımlamadan önce *index. html*dosyasındaki yapılandırma ayarlarını değiştirmeniz gerekir.

   1. Bağımlılıkları geliştirmeden üretime geçirmek için açıklama `"PROD RESOURCE LINKS"` altındaki üç satırın açıklamasını kaldırın. Açıklamanın `"DEV RESOURCE LINKS"`altındaki üç satırı açıklama olarak değerlendirin.

      [!code-html[head-sample](~/samples-javascript/pages/tutorial/index.html?range=2-20&highlight=10-13,15-18)]

      Bağımlılıklarınız aşağıdaki örnekte olduğu gibi yorumlanmalıdır:

      ```HTML
      <!-- PROD RESOURCE LINKS -->
      <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="https://unpkg.com/tsiclient@1.1.4/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="https://unpkg.com/tsiclient@1.1.4/tsiclient.css">

      <!-- DEV RESOURCE LINKS -->
      <!-- <link rel="stylesheet" type="text/css" href="./sampleStyles.css">
      <script src="../../dist/tsiclient.js"></script>
      <link rel="stylesheet" type="text/css" href="../../dist/tsiclient.css"> -->
      ```

   1. Uygulamayı Azure AD uygulama kayıt kimliğinizi kullanacak şekilde yapılandırmak `clientID` için, [uygulamayı Azure AD kullanacak şekilde](#register-with-azure-ad)kaydettiğinizde **Adım 3** ' te kopyaladığınız **uygulama kimliğini** kullanmak için değeri değiştirin. Azure AD 'de bir **oturum kapatma URL 'si** oluşturduysanız, bu değeri `postLogoutRedirectUri` değeri olarak ayarlayın.

      [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-153&highlight=4-5)]

      Örneğin:

      ```javascript
      clientId: '8884d4ca-b9e7-403a-bd8a-366d0ce0d460',
      postLogoutRedirectUri: 'https://tsispaapp.azurewebsites.net',
      ```

   1. Değişiklikleri yapmayı tamamladığınızda *index. html*dosyasını kaydedin.

1. Web uygulamasını Azure aboneliğinizde bir Azure App Service olarak yayımlayın.  

   > [!NOTE]
   > Aşağıdaki adımlarda gösterilen ekran görüntülerinde çeşitli seçenekler, Azure aboneliğinizdeki verilerle otomatik olarak doldurulur. Her bölmenin tamamen yüklenmesi birkaç saniye sürebilir.  

   1. Çözüm Gezgini ' de, Web sitesi proje düğümüne sağ tıklayın ve ardından **Web uygulaması Yayımla**' yı seçin.  

      [![Visual Studio-Çözüm Gezgini Web uygulaması Yayımla seçeneğini belirleyin](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png)](media/tutorial-create-tsi-sample-spa/vs-solution-explorer-publish-web-app.png#lightbox)

   1. Uygulamanızı yayımlamaya başlamak için **Başlat** ' ı seçin.

      [![Visual Studio-yayımlama profili bölmesi](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-target.png#lightbox)

   1. Uygulamayı yayımlamak için kullanmak istediğiniz aboneliği seçin. **Tsispaapp** projesini seçin. Sonra **Tamam**’ı seçin.

      [![Visual Studio-yayımlama profili App Service bölmesi](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-app-service.png#lightbox)

   1. Web uygulamasını dağıtmak için **Yayımla** ' yı seçin.

      [![Visual Studio-yayımla seçeneği ve yayımlama günlüğü çıkışı](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png)](media/tutorial-create-tsi-sample-spa/vs-publish-profile-output.png#lightbox)

   1. Başarılı bir yayımlama günlüğü, Visual Studio **çıktı** bölmesinde görünür. Dağıtım tamamlandığında, Visual Studio Web uygulamasını bir tarayıcı sekmesinde açar ve oturum açmayı ister. Başarılı oturum açma işleminden sonra Time Series Insights denetimleri verilerle doldurulur.

## <a name="troubleshoot"></a>Sorun giderme  

Hata kodu/durumu | Açıklama
---------------------| -----------
*AADSTS50011: Uygulama için kaydedilmiş bir yanıt adresi yok.* | Azure AD kaydında **yanıt URL 'si** özelliği eksik. Azure AD uygulama kaydınız için **Ayarlar** > **yanıt URL 'lerine** gidin. **Yeniden yönlendirme URI 'sinin** **Adım 2** ' de veya [Azure AD 'yi kullanmak için uygulamayı](#register-with-azure-ad) kaydettiğinizde **Adım 4** ' te belirtme seçeneğine sahip olduğunuzu doğrulayın.
*AADSTS50011: İstekte belirtilen yanıt URL 'si, uygulama için yapılandırılan yanıt URL 'leriyle eşleşmiyor: '\<Uygulama kimliği GUID > '.* | [Derleme ve yayımlama](#build-and-publish) içindeki **6. adımda** >  belirtilenbuWebuygulamasınıAzureADuygulamakaydınızdaayarlaryanıtURL'lerialtındabelirtilendeğerleaynı`postLogoutRedirectUri` olmalıdır. |
Web uygulaması yüklenir, ancak biçimlendirilmiş, salt metin oturum açma sayfasına beyaz bir arka plana sahiptir. | [Web uygulamasını derleme ve yayımlama](#build-and-publish) içindeki **6. adımda** açıklanan yolların doğru olduğundan emin olun. Web uygulaması .css dosyalarını bulamadığında sayfa stili doğru şekilde uygulanmaz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici çalışan birkaç Azure hizmeti oluşturur. Bu öğretici serisini tamamlamayı planlamıyorsanız, gereksiz maliyetlerin oluşmasını önlemek için tüm kaynakları silmenizi öneririz.

Azure portal sol menüsünde:

1. **Kaynak grupları**' nı seçin ve ardından Time Series Insights ortamı için oluşturduğunuz kaynak grubunu seçin. Sayfanın üst kısmında, **kaynak grubunu sil**' i seçin, kaynak grubunun adını girin ve **Sil**' i seçin.
1. **Kaynak grupları**' nı seçin ve ardından cihaz benzetimi Çözüm Hızlandırıcısı tarafından oluşturulan kaynak grubunu seçin. Sayfanın üst kısmında, **kaynak grubunu sil**' i seçin, kaynak grubunun adını girin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Uygulama tasarımı
> * Uygulamanızı Azure AD 'ye kaydetme
> * Web uygulamanızı derleme, yayımlama ve test etme

Bu öğretici, Azure AD ile tümleşir ve erişim belirteci almak için oturum açan kullanıcının kimliğini kullanır. Bir hizmet veya Daemon uygulamasının kimliğini kullanarak Time Series Insights API 'sine nasıl erişebileceğinizi öğrenmek için şu makaleye bakın:

> [!div class="nextstepaction"]
> [Azure Time Series Insights API’si için kimlik doğrulaması ve yetkilendirme](time-series-insights-authentication-and-authorization.md)
