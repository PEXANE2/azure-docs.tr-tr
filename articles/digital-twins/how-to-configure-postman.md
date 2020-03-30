---
title: Postacı nasıl yapılandırılatır - Azure Digital Twins | Microsoft Dokümanlar
description: Azure Digital Twins API'lerini test etmek için Postacı'yı nasıl yapılandırıp kullanacağınızı öğrenin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: ffcfb4f6ec5f6c654d0b243af85034ab575e0d88
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297166"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Azure Dijital İkizler için Postacı nasıl yapılandırılabilen

Bu makalede, Postacı REST istemcisiyle etkileşimde kalacak ve Azure Dijital İkizler Yönetimi API'lerini test etmek üzere nasıl yapılandırılabilen ler açıklanmaktadır. Özellikle, açıklar:

* OAuth 2.0 örtülü hibe akışını kullanmak için bir Azure Active Directory uygulamasını yapılandırma.
* Yönetici API'lerinize jeton taşıyan HTTP isteklerini yapmak için Postacı REST istemcisini nasıl kullanırsınız?
* Yönetici API'lerinize çok parçalı POST isteklerinde bulunmak için Postacı'yı nasıl kullanırsınız?

## <a name="postman-summary"></a>Postacı özeti

Yerel test ortamınızı hazırlamak için [Postacı](https://www.getpostman.com/) gibi bir REST istemci aracı nı kullanarak Azure Digital Twins'e başlayın. Postacı istemcisi hızlı bir şekilde karmaşık HTTP istekleri ni oluşturmaya yardımcı olur. [www.getpostman.com/apps'a](https://www.getpostman.com/apps)giderek Postacı istemcisinin masaüstü sürümünü indirin.

[Postacı](https://www.getpostman.com/) yararlı bir masaüstü ve eklenti tabanlı GUI içine anahtar HTTP istek işlevleri bulur bir REST test aracıdır.

Postman istemcisi aracılığıyla, çözüm geliştiricilerhttp isteği türünü belirtebilir *(POST*, *GET*, *UPDATE*, *PATCH*, ve *DELETE*), API bitiş noktası aramak için, ve TLS kullanımı. Postacı ayrıca HTTP istek üstbilgileri, parametreler, form verileri ve gövdeler eklemeyi de destekler.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>Azure Active Directory'sini OAuth 2.0 örtülü hibe akışını kullanacak şekilde yapılandırın

1. Azure Active Directory uygulaması oluşturmak ve yapılandırmak için [Quickstart'taki](quickstart-view-occupancy-dotnet.md#set-permissions-for-your-app) adımları izleyin. Alternatif olarak, varolan bir uygulama kaydını yeniden kullanabilirsiniz.

    [![Yeni bir Postacı Redirect URI yapıl](media/how-to-configure-postman/authentication-redirect-uri.png)](media/how-to-configure-postman/authentication-redirect-uri.png#lightbox)

1. Şimdi, bir Yeniden Yönlendirme `https://www.getpostman.com/oauth2/callback` **URI** ekleyin.

1. OAuth 2.0 örtülü hibe akışının kullanılmasına izin vermek için **Örtülü hibe** > **Erişim belirteçleri** onay kutusunu seçin. **Yapılaşı'yı**seçin, ardından **Kaydet'i**.

1. Azure Active Directory uygulamanızın **İstemci Kimliğini** kopyalayın.

## <a name="obtain-an-oauth-20-token"></a>OAuth 2.0 jetonu edinin

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]

Bir Azure Etkin Dizin belirteci elde etmek için Postacı'yı ayarlayın ve yapılandırır. Daha sonra, edinilmiş belirteci kullanarak Azure Digital Twins için kimlik doğrulaması yapılan bir HTTP isteği yapın:

1. **Yetkilendirme** URL'nizin doğru olduğundan doğrulayın. Bu biçimi almalıdır:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Adı  | Şununla değiştir | Örnek |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | Kiracınızın veya kuruluşunuzun adı. Azure Active Directory uygulama kaydınızın alfasayısal **Kiracı Kimliği** yerine insan dostu adı kullanın. | `microsoft` |

1. Uygulamayı indirmek için [www.getpostman.com](https://www.getpostman.com/) gidin.

1. GET talebinde bulunmak istiyoruz. **Yetkilendirme** sekmesini seçin, OAuth 2.0'ı seçin ve ardından **Yeni Erişim Jetonu Al'ı**seçin.

    | Alan  | Değer |
    |---------|---------|
    | Veriliş Türü | `Implicit` |
    | Geri Çağırma URL’si | `https://www.getpostman.com/oauth2/callback` |
    | Kimlik Doğrulama URL’si | **Adım 1'deki** **Yetkilendirme URL'sini** kullanma |
    | İstemci Kimliği | Önceki bölümde oluşturulan veya yeniden kullanılan Azure Etkin Dizin uygulaması için **Uygulama Kimliği'ni** kullanma |
    | Kapsam | Boş bırakın |
    | Durum | Boş bırakın |
    | İstemci Kimlik Doğrulaması | `Send as Basic Auth header` |

1. İstemci şimdi şu şekilde görünmelidir:

    [![Postacı istemci belirteç örneği](media/how-to-configure-postman/configure-postman-oauth-token.png)](media/how-to-configure-postman/configure-postman-oauth-token.png#lightbox)

1. **Belirteç İste**’ye tıklayın.
  
1. Aşağı kaydırın ve **Token'i Kullan'ı**seçin.

## <a name="make-a-multipart-post-request"></a>Çok parçalı POST isteğinde bulunun

Önceki adımları tamamladıktan sonra, Postman'ı doğrulanmış bir HTTP çok parçalı POST isteği yapmak üzere yapılandırın:

1. **Üstbilgi** sekmesinin altına, değeri `multipart/mixed`olan bir HTTP istek üstbilgi anahtarı **Içerik Türü** ekleyin.

   [![İçerik türünü çok parçalı/karışık belirtin](media/how-to-configure-postman/configure-postman-content-type.png)](media/how-to-configure-postman/configure-postman-content-type.png#lightbox)

1. Metin dışı verileri dosyalara serileştirin. JSON verileri JSON dosyası olarak kaydedilir.
1. **Gövde** sekmesinin `form-data`altında. 
1. Bir **anahtar** adı atayarak, seçerek `File`her dosyayı ekleyin.
1. Ardından, **Dosya Seç** düğmesini kullanarak her dosyayı seçin.

   [![Postacı istemci formu vücut örneği](media/how-to-configure-postman/configure-postman-form-body.png)](media/how-to-configure-postman/configure-postman-form-body.png#lightbox)

   >[!NOTE]
   > * Postacı istemcisi, çok parçalı parçaların el ile atanmış bir **İçerik Türü** veya **İçerik-Disposition'a**sahip olduğunu gerektirmez.
   > * Her bölüm için bu başlıkları belirtmeniz gerekmez.
   > * İsteğin `multipart/mixed` tamamı için uygun bir **İçerik Türü** seçmeniz gerekir.

1. Son olarak, çok parçalı HTTP POST isteğinizi göndermek için **Gönder'i** seçin. Bir durum `200` kodu `201` veya başarılı bir istek gösterir. Uygun yanıt iletisi istemci arabiriminde görünür.

1. API bitiş noktasını arayarak HTTP POST istek verilerinizi doğrulayın: 

   ```URL
   YOUR_MANAGEMENT_API_URL/spaces/blobs?includes=description
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Digital Twins yönetim API'leri ve bunları nasıl kullanacağıhakkında bilgi edinmek için [Azure Digital Twins yönetim API'lerini nasıl kullanacağınızı](how-to-navigate-apis.md)okuyun.

- [Azure Digital Twins'in varlıklarına blob eklemek için](./how-to-add-blobs.md)çok parçalı istekleri kullanın.

- Yönetim API'leri ile kimlik doğrulama hakkında bilgi edinmek için [API'lerle Kimlik Doğrula'yı](./security-authenticating-apis.md)okuyun.
