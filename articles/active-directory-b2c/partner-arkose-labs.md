---
title: Azure Active Directory B2C ile arkoo Labs
titleSuffix: Azure AD B2C
description: Bot saldırılarına karşı korumaya yardımcı olmak, hesap hileli saldırıları ve sahte hesap açılışlarına karşı koruma sağlamak için Arkoo Labs ile Azure AD B2C kimlik doğrulamasını tümleştirmeyi öğrenin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/08/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 92b40fc22a148727760df2d25f73c4cbd80bd852
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84810085"
---
# <a name="tutorial-for-configuring-arkose-labs-with-azure-active-directory-b2c"></a>Azure Active Directory B2C ile Arkoo Labs yapılandırma öğreticisi

Bu öğreticide, Arkoo Labs ile Azure AD B2C kimlik doğrulamasını tümleştirmeyi öğrenin. Arkoo Labs, kuruluşlara bot saldırılarına, hesap hileli saldırıları ve sahte hesap açılışlarına karşı yardımcı olur.  

## <a name="prerequisites"></a>Ön koşullar

Başlamak için şunlar gerekir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Azure aboneliğinize bağlı [bir Azure AD B2C kiracısı](tutorial-create-tenant.md) .

## <a name="scenario-description"></a>Senaryo açıklaması

Aşağıdaki diyagramda, Arkoo Labs Azure AD B2C ile nasıl tümleşildiği açıklanmaktadır.

![Arkoi Labs mimari diyagramı](media/partner-arkose-labs/arkose-architecture-diagram.png)

|   |   |
|---|---|
|1     | Kullanıcı önceden oluşturulmuş bir hesapla oturum açar. Kullanıcı Gönder ' i seçtiğinde bir Arkoo Labs zorlama sınaması görüntülenir. Kullanıcı sınamayı tamamladıktan sonra, bir belirteç oluşturmak için Arkoo Labs 'e gönderilir.        |
|2     |  Arkoo Labs, belirteci Azure AD B2C 'e geri gönderir.       |
|3     |  Oturum açma formu gönderilmeden önce, belirteç doğrulama için Arkoo Labs 'e gönderilir.       |
|4     |  Arkoy, çekişme bir başarı veya başarısızlık sonucunu geri gönderir.       |
|5     |  Sınama başarıyla tamamlanırsa, Azure AD B2C bir oturum açma formu gönderilir ve Azure AD B2C kimlik doğrulamasını tamamlar.       |
|   |   |

## <a name="onboard-with-arkose-labs"></a>Arkoo Labs ile ekleme

1. [Arkoo Labs](https://www.arkoselabs.com/book-a-demo/) ile iletişime geçerek ve bir hesap oluşturarak başlayın.

2. Hesabınız oluşturulduktan sonra öğesine gidin https://dashboard.arkoselabs.com/login .

3. Pano içinde, ortak anahtarınızı ve özel anahtarınızı bulmak için site ayarları ' na gidin. Azure AD B2C yapılandırmak için bu bilgiler daha sonra gerekecektir.

## <a name="integrate-with-azure-ad-b2c"></a>Azure AD B2C ile tümleştirme

### <a name="part-1--create-blob-storage-to-store-the-custom-html"></a>1. Bölüm – özel HTML 'yi depolamak için BLOB depolama oluşturma

Bir depolama hesabı oluşturmak için aşağıdaki adımları izleyin:  

1. Azure portal oturum açın.

2. Azure aboneliğinizi içeren dizini kullandığınızdan emin olun. Üstteki menüden **Dizin + abonelik** filtresini seçin ve aboneliğinizi içeren dizini seçin. Bu dizin, Azure B2C kiracınızı içeren olandan farklı.

3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından  **depolama hesapları**' nı arayıp seçin.

4.  **Ekle**'yi seçin.

5.  **Kaynak grubu**altında **Yeni oluştur**' u seçin, yeni kaynak grubu için bir ad girin ve ardından **Tamam**' ı seçin.

6. Depolama hesabı için bir ad girin. Seçtiğiniz ad Azure’da benzersiz olmalı, uzunluğu 3 ile 24 karakter arasında olmalı ve yalnızca sayı ile küçük harf içermelidir.

7. Depolama hesabının konumunu seçin veya varsayılan konumu kabul edin.

8. Diğer tüm varsayılan değerleri kabul et  **&**  >  **Oluştur**' u seçin.

9. Depolama hesabı oluşturulduktan sonra  **Kaynağa Git**' i seçin.

#### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

1. Depolama hesabının genel bakış sayfasında,  **Bloblar**' ı seçin.

2.   **Kapsayıcı**' yı seçin, kapsayıcı için bir ad girin,  **BLOB** ' u seçin (yalnızca blob 'lar için anonim okuma erişimi) ve ardından **Tamam**' ı seçin.

#### <a name="enable-cross-origin-resource-sharing-cors"></a>Çıkış noktaları arası kaynak paylaşımını (CORS) etkinleştirme

Bir tarayıcıda Azure AD B2C kod, Kullanıcı akışında belirttiğiniz bir URL 'den özel içerik yüklemek için modern ve standart bir yaklaşım kullanır. CORS, Web sayfasındaki kısıtlanmış kaynakların diğer etki alanlarından istenme izin verir.

1. Menüsünde  **CORS**' yi seçin.

2.   **Izin verilen çıkış noktaları**için girin  `https://your-tenant-name.b2clogin.com` . Kiracı adınızı Azure AD B2C kiracınızın adıyla değiştirin. Örneğin  `https://fabrikam.b2clogin.com`. Kiracı adınızı girerken tüm küçük harfleri kullanın.

3.  **Izin verilen Yöntemler**için **Al**, **koy**ve **Seçenekler**' i seçin.

4. **Izin verilen üst bilgiler**için bir yıldız işareti (*) girin.

5.  **Gösterilen üstbilgiler**için bir yıldız işareti (*) girin.

6. **Maksimum yaş**için 200 girin.

   ![Arkoo Labs kaydolma ve oturum açma](media/partner-arkose-labs/signup-signin-arkose.png)

7. **Kaydet**’i seçin.

### <a name="part-2--set-up-a-back-end-server"></a>Bölüm 2 – bir arka uç sunucusu ayarlama

Git Bash 'i indirin ve aşağıdaki adımları izleyin:

1. "Tebrikler!" iletisiyle [bir Web uygulaması oluşturmak](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-php)için yönergeleri izleyin.İlk PHP uygulamanızı App Service "ekranlar için dağıttık.

2. Yerel klasörünüzü açın ve **index. php** dosyasını **Verify-Token. php**olarak yeniden adlandırın.

3. Yeni yeniden adlandırılmış Verify-Token. php dosyasını açın ve:

   a. İçeriği [GitHub deposunda](https://github.com/ArkoseLabs/Azure-AD-B2C)bulunan Verify-Token. php dosyasındaki içerikle değiştirin.

   b. 3. satırdaki <private_key>, Arkoi Labs panosundan elde edilen özel anahtarınızla değiştirin.

4. Yerel Terminal penceresinde, değişikliklerinizi git 'e işleyin ve git Bash içine aşağıdakini yazarak kod değişikliklerini Azure 'a gönderin:

   ``git commit -am "updated output"``

   ``git push azure master``  

### <a name="part-3---final-setup"></a>Bölüm 3 – son kurulum

#### <a name="store-the-custom-html"></a>Özel HTML 'yi depolayın

1. [GitHub deposunda](https://github.com/ArkoseLabs/Azure-AD-B2C)depolanan index.html dosyasını açın.

2. Tüm örneklerini `<tenantname>` b2C kiracı adınızla değiştirin (diğer bir deyişle, `<tenantname>.b2clogin.com` ). Dört örnek olmalıdır.

3. `<appname>`Öğesini Bölüm 2 ' de oluşturduğunuz uygulama adıyla değiştirin, 1. adım.

   ![Arkoo Labs Azure CLı 'yi gösteren ekran görüntüsü](media/partner-arkose-labs/arkose-azure-cli.png)

4. `<public_key>`64. satırını, Arkoi Labs panosundan edindiğiniz ortak anahtarla değiştirin.

5. index.html dosyasını yukarıda oluşturulan BLOB depolama alanına yükleyin.

6. **Depolama**  >  **kapsayıcısı**  >  **karşıya yükleme**bölümüne gidin.

#### <a name="set-up-azure-ad-b2c"></a>Azure AD B2C ayarlama

> [!NOTE]
> Henüz bir tane yoksa, Azure aboneliğinize bağlı [bir Azure AD B2C kiracı oluşturun](tutorial-create-tenant.md) .

1. [Buradaki](tutorial-create-user-flows.md)bilgilere göre bir Kullanıcı akışı oluşturun. **Kullanıcı akışını test**etme bölümüne geldiğinizde durun.

2. [Kullanıcı akışındaki](user-flow-javascript-overview.md)JavaScript 'i etkinleştirin.

3. Aynı kullanıcı akışı sayfasında özel sayfa URL 'sini etkinleştirin: **Kullanıcı akışına**git  >  **sayfa düzeni**  >  **özel sayfa içeriğini kullan**  =  **Evet**  >  **özel sayfa URL 'si Ekle**.
Bu özel sayfa URL 'SI, BLOB depolama içindeki index.html dosyasının konumundan alınır  

   ![Arkoo Labs depolama URL 'sini gösteren ekran görüntüsü](media/partner-arkose-labs/arkose-storage-url.png)

## <a name="test-the-user-flow"></a>Kullanıcı akışını test etme

1. Azure AD B2C kiracıyı açın ve **ilkeler**altında **Kullanıcı akışları**' nı seçin.

2. Daha önce oluşturduğunuz Kullanıcı akışınızı seçin.

3. **Kullanıcı akışını Çalıştır** ' ı seçin ve ayarları seçin:

   a. **Uygulama** -kayıtlı uygulamayı seçin (örnek JWT).

   b. **Yanıt URL 'si** -YENIDEN yönlendirme URL 'sini seçin.

   c. **Kullanıcı akışını Çalıştır**' ı seçin.

4. Kaydolma akışı ' na gidip bir hesap oluşturun.

5. Oturumunuzu kapatın.

6. Oturum açma akışına gidin.

7. **Devam**' ı seçtikten sonra bir arkoo Labs bulmaca görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makaleleri gözden geçirin:

- [Azure AD B2C'deki özel ilkeler](custom-policy-overview.md)

- [Azure AD B2C özel ilkeleri kullanmaya başlama](custom-policy-get-started.md?tabs=applications)
