---
title: Postman Yapılandırma-Azure dijital TWINS | Microsoft Docs
description: Azure dijital TWINS API 'Lerini test etmek için Postman yapılandırma ve kullanma hakkında bilgi edinin.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: ffcfb4f6ec5f6c654d0b243af85034ab575e0d88
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80297166"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Azure dijital TWINS için Postman 'ı yapılandırma

Bu makalede, Azure Digital TWINS yönetim API 'Lerini ile etkileşime geçmek ve test etmek üzere Postman REST istemcisinin nasıl yapılandırılacağı açıklanır. Özellikle şunları açıklar:

* Azure Active Directory uygulamasını OAuth 2,0 örtük izin akışını kullanacak şekilde yapılandırma.
* Yönetim API 'lerinize belirteç oluşturma HTTP istekleri yapmak için Postman REST istemcisini kullanma.
* Yönetim API 'lerinize çok parçalı GÖNDERI istekleri yapmak için Postman 'ı kullanma.

## <a name="postman-summary"></a>Postman Özeti

Yerel test ortamınızı hazırlamak için [Postman](https://www.getpostman.com/) gıbı bir rest istemci aracı kullanarak Azure dijital TWINS 'i kullanmaya başlayın. Postman istemcisi, karmaşık HTTP isteklerinin hızla oluşturulmasına yardımcı olur. [Www.getpostman.com/apps](https://www.getpostman.com/apps)'e giderek Postman istemcisinin Masaüstü sürümünü indirin.

[Postman](https://www.getpostman.com/) , önemli http istek işlevlerini yararlı bir masaüstü ve EKLENTI tabanlı GUI 'ye bulan bir rest test aracıdır.

Çözüm geliştiricileri Postman istemcisi aracılığıyla HTTP isteği türünü (*gönderi*, *Al*, *Güncelleştir*, *Düzeltme Eki*uygulama ve *silme*), API 'yi çağırmak için API uç noktası ve TLS kullanımını belirtebilir. Postman ayrıca HTTP istek üstbilgileri, parametreleri, form verileri ve gövdeleri eklemeyi destekler.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>OAuth 2,0 örtük izin akışını kullanmak için Azure Active Directory yapılandırma

1. Azure Active Directory uygulaması oluşturmak ve yapılandırmak için [hızlı](quickstart-view-occupancy-dotnet.md#set-permissions-for-your-app) başlangıçtaki adımları izleyin. Alternatif olarak, var olan bir uygulama kaydını yeniden kullanabilirsiniz.

    [![Yeni bir Postman yeniden yönlendirme URI 'SI yapılandırın](media/how-to-configure-postman/authentication-redirect-uri.png)](media/how-to-configure-postman/authentication-redirect-uri.png#lightbox)

1. Şimdi, için `https://www.getpostman.com/oauth2/callback`bir **yeniden yönlendirme URI 'si** ekleyin.

1. OAuth 2,0 örtük verme akışının kullanılmasına izin vermek için **örtük** > **erişim belirteçleri** ver onay kutusunu seçin. **Yapılandır**' ı ve ardından **Kaydet**' i seçin.

1. Azure Active Directory uygulamanızın **ISTEMCI kimliğini** kopyalayın.

## <a name="obtain-an-oauth-20-token"></a>OAuth 2,0 belirteci edinme

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]

Azure Active Directory belirtecini almak için Postman ayarlayın ve yapılandırın. Daha sonra, alınan belirteci kullanarak Azure dijital TWINS 'e kimliği doğrulanmış bir HTTP isteği oluşturun:

1. **YETKILENDIRME URL** 'nizin doğru olduğundan emin olun. Şu biçimde olmalıdır:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Adı  | Şununla değiştir | Örnek |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | Kiracınızın veya kuruluşunuzun adı. Azure Active Directory Uygulama kaydınızın alfasayısal **KIRACı kimliği** yerine insandaki kolay adı kullanın. | `microsoft` |

1. Uygulamayı indirmek için [www.getpostman.com](https://www.getpostman.com/) adresine gidin.

1. İstek almak istiyoruz. **Yetkilendirme** sekmesini seçin, OAuth 2,0 ' i seçin ve ardından **Yeni erişim belirteci al**' ı seçin.

    | Alan  | Değer |
    |---------|---------|
    | Veriliş Türü | `Implicit` |
    | Geri Çağırma URL’si | `https://www.getpostman.com/oauth2/callback` |
    | Kimlik Doğrulama URL’si | **1. adımdaki** **yetkilendirme URL 'sini** kullanın |
    | İstemci Kimliği | Önceki bölümden oluşturulmuş veya yeniden kullanılan Azure Active Directory uygulamasının **uygulama kimliğini** kullanın |
    | Kapsam | Boş bırakın |
    | Durum | Boş bırakın |
    | İstemci Kimlik Doğrulaması | `Send as Basic Auth header` |

1. İstemci şu şekilde görünmelidir:

    [![Postman istemci belirteci örneği](media/how-to-configure-postman/configure-postman-oauth-token.png)](media/how-to-configure-postman/configure-postman-oauth-token.png#lightbox)

1. **Belirteç İste**’ye tıklayın.
  
1. Aşağı kaydırın ve **belirteci kullan**' ı seçin.

## <a name="make-a-multipart-post-request"></a>Çok parçalı bir POST isteği oluşturun

Önceki adımları tamamladıktan sonra, kimliği doğrulanmış bir HTTP çok parçalı GÖNDERI isteği oluşturmak için Postman 'ı yapılandırın:

1. **Üstbilgiler** sekmesi ALTıNA bir http istek üst bilgisi anahtarı **Content-Type** değeri `multipart/mixed`ile ekleyin.

   [![Çok parçalı/karma içerik türünü belirtin](media/how-to-configure-postman/configure-postman-content-type.png)](media/how-to-configure-postman/configure-postman-content-type.png#lightbox)

1. Metin olmayan verileri dosyalara serileştirme. JSON verileri bir JSON dosyası olarak kaydedilir.
1. **Gövde** sekmesinde öğesini seçin `form-data`. 
1. Her dosyayı bir **anahtar** adı atayarak ekleyin, öğesini seçin `File`.
1. Sonra, **Dosya Seç** düğmesini kullanarak her bir dosyayı seçin.

   [![Postman istemci formu gövdesi örneği](media/how-to-configure-postman/configure-postman-form-body.png)](media/how-to-configure-postman/configure-postman-form-body.png#lightbox)

   >[!NOTE]
   > * Postman istemcisi, çok parçalı öbeklerin el ile atanmış bir **Içerik türü** veya **Content-Disposition**olmasını gerektirmez.
   > * Her bölüm için bu üst bilgileri belirtmeniz gerekmez.
   > * Tüm istek için `multipart/mixed` uygun bir **içerik türü** seçmelisiniz.

1. Son olarak, çok parçalı HTTP POST isteğinizi göndermek için **Gönder** ' i seçin. Durum kodu `200` veya `201` başarılı bir isteği gösterir. Uygun yanıt iletisi, istemci arabiriminde görüntülenir.

1. API uç noktasını çağırarak HTTP POST isteği verilerinizi doğrulayın: 

   ```URL
   YOUR_MANAGEMENT_API_URL/spaces/blobs?includes=description
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Dijital TWINS yönetim API 'Leri ve bunların nasıl kullanılacağı hakkında bilgi edinmek için [Azure Digital TWINS yönetim API 'Lerini nasıl kullanacağınızı](how-to-navigate-apis.md)okuyun.

- [Azure dijital TWINS 'in varlıklarına blob eklemek](./how-to-add-blobs.md)için çok parçalı istekleri kullanın.

- Yönetim API 'Lerinde kimlik doğrulama hakkında bilgi edinmek için, [API 'Lerle kimlik doğrulaması](./security-authenticating-apis.md)' nı okuyun.
