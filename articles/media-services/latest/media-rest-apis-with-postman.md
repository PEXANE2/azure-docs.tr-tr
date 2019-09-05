---
title: Azure Media Services REST API'si çağrıları için Postman yapılandırma
description: Media Services REST API çağrıları için Postman yapılandırma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/11/2019
ms.author: juliako
ms.openlocfilehash: d25596884acdb356779eafa4348240239855ce37
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70308459"
---
# <a name="configure-postman-for-media-services-rest-api-calls"></a>Media Services REST API çağrıları için Postman yapılandırma

Bu makalede, **Postman** 'ın Azure Media Services (AMS) REST API 'lerini çağırmak için kullanılabilmesi üzere nasıl yapılandırılacağı gösterilmektedir. Makale, ortam ve koleksiyon dosyalarının **Postman**'a nasıl alınacağını gösterir. Koleksiyon, Azure Media Services (AMS) REST API 'Lerini çağıran HTTP isteklerinin gruplandırılmış tanımlarını içerir. Ortam dosyası, koleksiyon tarafından kullanılan değişkenleri içerir.

Geliştirmeye başlamadan önce [Media Services v3 API 'leri Ile geliştirmeyi](media-services-apis-overview.md)inceleyin.

## <a name="prerequisites"></a>Önkoşullar

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun. 
- [API 'lere erişmek](access-api-cli-how-to.md) için gereken bilgileri alın
- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer alternatifler: REST eklentisi veya **Telerik Fiddler**ile **Visual Studio Code** . 

> [!IMPORTANT]
> [Adlandırma kurallarını](media-services-apis-overview.md#naming-conventions)gözden geçirin.

## <a name="download-postman-files"></a>Postman dosyalarını indirme

Postman koleksiyonunu ve ortam dosyalarını içeren bir GitHub deposunu kopyalayın.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="configure-postman"></a>Postman'i yapılandırma

Bu bölümde Postman yapılandırılmaktadır.

### <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. **Postman**'ı açın.
2. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Ortamı yönetme](./media/develop-with-postman/postman-import-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
2. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Azure Media Service v3 Environment.postman_environment.json` dosyasına gidin.
6. **Azure Media Service v3 Environment** ortamı eklenir.

    > [!Note]
    > Erişim değişkenlerini yukarıdaki **Media Services API'sine erişme** bölümünden aldığınız değerlerle güncelleştirin.

7. Seçili dosyaya çift tıklayın ve API adımlarına erişim ' i izleyerek aldığınız değerleri girin.
8. İletişim kutusunu kapatın.
9. Aşağı açılan listeden **Azure Media Service v3 Environment** ortamını seçin.

    ![Ortamı seçme](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
1. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Media Services v3.postman_collection.json` dosyasına gidin
3. **Media Services v3.postman_collection.json** dosyasını seçin.

    ![Dosya içe aktarma](./media/develop-with-postman/postman-import-collection.png)

## <a name="get-azure-ad-token"></a>Azure AD Belirteci alma 

AMS v3 kaynaklarını değiştirmeye başlamadan önce hizmet sorumlusu kimlik doğrulaması için Azure AD belirtecini almanız ve ayarlamanız gerekir.

1. Postman 'nın sol penceresinde "1. Adım: AAD kimlik doğrulama belirteci al ".
2. Sonra, "Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma"'yı seçin.
3. **Gönder**’e basın.

    Aşağıdaki **POST** işlemi gönderilir.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Yanıt belirteç ile gelir ve "AccessToken" ortam değişkenini belirteç değerine ayarlar.  

    ![AAD belirteci alma](./media/develop-with-postman/postman-get-aad-auth-token.png)

## <a name="see-also"></a>Ayrıca bkz.

- [Media Services hesaba dosya yükleme-REST](upload-files-rest-how-to.md)
- [Media Services REST ile filtre oluşturma](filters-dynamic-manifest-rest-howto.md)
- [Azure Resource Manager tabanlı REST API](https://github.com/Azure-Samples/media-services-v3-arm-templates)

## <a name="next-steps"></a>Sonraki adımlar

- [Rest Ile akış dosyaları](stream-files-tutorial-with-rest.md).  
- [Öğretici: Uzak dosya tabanlı URL kodlama ve video akışı yapma - REST](stream-files-tutorial-with-rest.md)
