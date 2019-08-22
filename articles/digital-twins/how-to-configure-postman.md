---
title: Azure dijital TWINS için Postman yapılandırma | Microsoft Docs
description: Azure dijital TWINS için Postman 'ı yapılandırma.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: v-adgera
ms.openlocfilehash: 66dbfd09ef07740c07ddb010b73e33e783340e5a
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69873638"
---
# <a name="how-to-configure-postman-for-azure-digital-twins"></a>Azure dijital TWINS için Postman 'ı yapılandırma

Bu makalede, Azure Digital TWINS yönetim API 'Lerini ile etkileşime geçmek ve test etmek üzere Postman REST istemcisinin nasıl yapılandırılacağı açıklanır. Özellikle şunları açıklar:

* Azure Active Directory uygulamasını OAuth 2,0 örtük izin akışını kullanacak şekilde yapılandırma.
* Yönetim API 'lerinize belirteç oluşturma HTTP istekleri yapmak için Postman REST istemcisini kullanma.
* Yönetim API 'lerinize çok parçalı GÖNDERI istekleri yapmak için Postman 'ı kullanma.

## <a name="postman-summary"></a>Postman Özeti

Yerel test ortamınızı hazırlamak için [Postman](https://www.getpostman.com/) gıbı bir rest istemci aracı kullanarak Azure dijital TWINS 'i kullanmaya başlayın. Postman istemcisi, karmaşık HTTP isteklerinin hızla oluşturulmasına yardımcı olur. [Www.getpostman.com/apps](https://www.getpostman.com/apps)'e giderek Postman istemcisinin Masaüstü sürümünü indirin.

[Postman](https://www.getpostman.com/) , önemli http istek işlevlerini yararlı bir masaüstü ve EKLENTI tabanlı GUI 'ye bulan bir rest test aracıdır.

Çözüm geliştiricileri Postman istemcisi aracılığıyla HTTP isteği türünü (*gönderi*, *Al*, *Güncelleştir*, *Düzeltme Eki*uygulama ve *silme*), API 'yi çağırmak için API uç noktası ve SSL kullanımını belirtebilir. Postman ayrıca HTTP istek üstbilgileri, parametreleri, form verileri ve gövdeleri eklemeyi destekler.

## <a name="configure-azure-active-directory-to-use-the-oauth-20-implicit-grant-flow"></a>OAuth 2,0 örtük izin akışını kullanmak için Azure Active Directory yapılandırma

Azure Active Directory uygulamanızı OAuth 2,0 örtük izin akışını kullanacak şekilde yapılandırın.

1. Yerel türde bir Azure AD uygulaması oluşturmak için [Bu hızlı](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad) başlangıçta bulunan adımları izleyin. Ya da var olan bir yerel uygulama kaydını yeniden kullanabilirsiniz.

1. **Gerekli izinler**altında **Ekle** ' yi seçin ve **API erişimi ekle**altına **Azure dijital TWINS** girin. Arama sonucunda API görüntülenmezse **Azure Smart Spaces** araması yapın. Ardından, **izinleri > temsilci Izinleri ver** ' i ve **bitti**' yi seçin.

    [![Uygulama kayıtlarını Azure Active Directory API Ekle](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png)](../../includes/media/digital-twins-permissions/aad-app-req-permissions.png#lightbox)

1. Uygulamanızın uygulama bildirimini açmak için **bildirim** ' ı seçin. *Oauth2AllowImplicitFlow* olarak `true`ayarlayın.

    [![Azure Active Directory örtük akış](media/how-to-configure-postman/implicit-flow.png)](media/how-to-configure-postman/implicit-flow.png#lightbox)

1. İçin`https://www.getpostman.com/oauth2/callback`bir **yanıt URL 'si** yapılandırın.

    [![Azure Active Directory yanıtı URL 'SI](media/how-to-configure-postman/reply-url.png)](media/how-to-configure-postman/reply-url.png#lightbox)

1. Azure Active Directory uygulamanızın **uygulama kimliğini** kopyalayın ve saklayın. Bu, izleyen adımlarda kullanılır.

## <a name="obtain-an-oauth-20-token"></a>OAuth 2,0 belirteci edinme

[!INCLUDE [digital-twins-management-api](../../includes/digital-twins-management-api.md)]

Azure Active Directory belirtecini almak için Postman ayarlayın ve yapılandırın. Daha sonra, alınan belirteci kullanarak Azure dijital TWINS 'e kimliği doğrulanmış bir HTTP isteği oluşturun:

1. Uygulamayı indirmek için [www.getpostman.com](https://www.getpostman.com/) adresine gidin.
1. **YETKILENDIRME URL** 'nizin doğru olduğundan emin olun. Şu biçimde olmalıdır:

    ```plaintext
    https://login.microsoftonline.com/YOUR_AZURE_TENANT.onmicrosoft.com/oauth2/authorize?resource=0b07f429-9f4b-4714-9392-cc5e8e80c8b0
    ```

    | Name  | Şununla değiştir | Örnek |
    |---------|---------|---------|
    | YOUR_AZURE_TENANT | Kiracınızın veya kuruluşunuzun adı | `microsoft` |

1. **Yetkilendirme** sekmesini seçin, **OAuth 2,0**' i seçin ve ardından **Yeni erişim belirteci al**' ı seçin.

    | Alan  | Value |
    |---------|---------|
    | İzin Verme Türü | `Implicit` |
    | Geri çağırma URL'si | `https://www.getpostman.com/oauth2/callback` |
    | Kimlik doğrulama URL 'SI | 2\. adımdaki **yetkilendirme URL 'sini** kullanın |
    | İstemci Kimliği | Önceki bölümde oluşturulan veya yeniden kullanılan Azure Active Directory uygulama için **uygulama kimliğini** kullanın |
    | `Scope` | Boş bırakın |
    | State | Boş bırakın |
    | İstemci Kimlik Doğrulaması | `Send as Basic Auth header` |

1. İstemci şu şekilde görünmelidir:

    [![Postman istemci örneği](media/how-to-configure-postman/postman-oauth-token.png)](media/how-to-configure-postman/postman-oauth-token.png#lightbox)

1. **Istek belirtecini**seçin.

    >[!TIP]
    >"OAuth 2 tamamlanamadı" hata iletisini alırsanız şunları deneyin:
    > * Postman 'ı kapatın ve yeniden açın ve tekrar deneyin.
  
1. Aşağı kaydırın ve **belirteci kullan**' ı seçin.

<div id="multi"></div>

## <a name="make-a-multipart-post-request"></a>Çok parçalı bir POST isteği oluşturun

Önceki adımları tamamladıktan sonra, kimliği doğrulanmış bir HTTP çok parçalı GÖNDERI isteği oluşturmak için Postman 'ı yapılandırın:

1. **Üst bilgi** SEKMESINDE bir http istek üst bilgisi anahtarı **Content-Type** değeri `multipart/mixed`ile ekleyin.

   [![İçerik türü parçalı/karışık](media/how-to-configure-postman/content-type.png)](media/how-to-configure-postman/content-type.png#lightbox)

1. Metin olmayan verileri dosyalara serileştirme. JSON verileri bir JSON dosyası olarak kaydedilir.
1. **Gövde** sekmesi altında, her dosyayı bir **anahtar** adı atayarak ekleyin, veya `file` `text`öğesini seçin.
1. Sonra, **Dosya Seç** düğmesini kullanarak her bir dosyayı seçin.

   [![Postman istemci örneği](media/how-to-configure-postman/form-body.png)](media/how-to-configure-postman/form-body.png#lightbox)

   >[!NOTE]
   > * Postman istemcisi, çok parçalı öbeklerin el ile atanmış bir **Içerik türü** veya **Content-Disposition**olmasını gerektirmez.
   > * Her bölüm için bu üst bilgileri belirtmeniz gerekmez.
   > * Tüm istek için `multipart/mixed` uygun bir **içerik türü** seçmelisiniz.

1. Son olarak, çok parçalı HTTP POST isteğinizi göndermek için **Gönder** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Dijital TWINS yönetim API 'Leri ve bunların nasıl kullanılacağı hakkında bilgi edinmek için [Azure Digital TWINS yönetim API 'Lerini nasıl kullanacağınızı](how-to-navigate-apis.md)okuyun.

- [Azure dijital TWINS 'in varlıklarına blob eklemek](./how-to-add-blobs.md)için çok parçalı istekleri kullanın.

- Yönetim API 'Lerinde kimlik doğrulama hakkında bilgi edinmek için, [API 'Lerle kimlik doğrulaması](./security-authenticating-apis.md)' nı okuyun.
