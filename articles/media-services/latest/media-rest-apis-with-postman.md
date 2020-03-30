---
title: Azure Medya Hizmetleri v3 REST API aramaları için Postacı'yı yapılandır
description: Bu makalede, Postacı'yı Azure Medya Hizmetleri (AMS) REST API'lerini aramak için kullanılabilecek şekilde nasıl yapılandırabileceğiniz gösterilmektedir.
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
ms.date: 12/05/2019
ms.author: juliako
ms.openlocfilehash: 872dad95fc5b536c51e251612f40439da020a059
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75779646"
---
# <a name="configure-postman-for-media-services-v3-rest-api-calls"></a>Medya Hizmetleri v3 REST API aramaları için Postacı yapılandırma

Bu makalede, **Postacı'yı** Azure Medya Hizmetleri (AMS) REST API'lerini aramak için kullanılabilecek şekilde nasıl yapılandırabileceğiniz gösterilmektedir. Makale, **postacıya**ortam ve toplama dosyalarının nasıl aktarılsüreceğini gösterir. Koleksiyon, Azure Medya Hizmetleri (AMS) REST API'leri olarak adlandırdığı HTTP isteklerinin gruplanmış tanımlarını içerir. Ortam dosyası, koleksiyon tarafından kullanılan değişkenleri içerir.

Geliştirmeye başlamadan önce, [Medya Hizmetleri v3 API'leri ile Geliştirme'yi gözden geçirin.](media-services-apis-overview.md)

## <a name="prerequisites"></a>Ön koşullar

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun. 
- [API'lere erişmek](access-api-cli-how-to.md) için gereken bilgileri alın
- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer seçenekler şunlardır: REST eklentili **Visual Studio Code** veya **Telerik Fiddler**. 

> [!IMPORTANT]
> Adlandırma kurallarını gözden [geçirin.](media-services-apis-overview.md#naming-conventions)

## <a name="download-postman-files"></a>Postman dosyalarını indirme

Postman koleksiyonunu ve ortam dosyalarını içeren bir GitHub deposunu kopyalayın.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="configure-postman"></a>Postman'i yapılandırma

### <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. **Postacı** uygulamasını açın.
2. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Ortamı yönetme](./media/develop-with-postman/postman-import-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
2. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Azure Media Service v3 Environment.postman_environment.json` dosyasına gidin.
6. **Azure Media Service v3 Environment** ortamı eklenir.

    > [!Note]
    > Erişim değişkenlerini yukarıdaki **Media Services API'sine erişme** bölümünden aldığınız değerlerle güncelleştirin.

7. Seçili dosyaya çift tıklayın ve API'ye erişim adımlarını izleyerek aldığınız değerleri girin.
8. İletişim kutusunu kapatın.
9. Aşağı açılan listeden **Azure Media Service v3 Environment** ortamını seçin.

    ![Ortamı seçme](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
1. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Media Services v3.postman_collection.json` dosyasına gidin
3. **Media Services v3.postman_collection.json** dosyasını seçin.

    ![Dosya içe aktarma](./media/develop-with-postman/postman-import-collection.png)

## <a name="get-azure-ad-token"></a>Azure AD Belirteci alma 

AMS v3 kaynaklarını manipüle etmeye başlamadan önce Hizmet Müdürü Kimlik Doğrulaması için Azure AD Belirteci'ni almanız ve ayarlamanız gerekir.

1. Postacı uygulamasının sol penceresinde "Adım 1: AAD Auth belirteci alın" seçeneğini belirleyin.
2. Sonra, "Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma"'yı seçin.
3. **Gönder**’e basın.

    Aşağıdaki **POST** işlemi gönderilir.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Yanıt belirteç ile gelir ve "AccessToken" ortam değişkenini belirteç değerine ayarlar.  

    ![AAD belirteci alma](./media/develop-with-postman/postman-get-aad-auth-token.png)

## <a name="troubleshooting"></a>Sorun giderme 

* Uygulamanız "HTTP 504: Gateway Timeout" ile başarısız olursa, konum değişkeninin Medya Hizmetleri hesabının beklenen konumu dışında açıkça ayarlanmadığından emin olun. 
* Bir "hesap bulunamadı" hatası alırsanız, Body JSON iletisindeki konum özelliğinin Medya Hizmetleri hesabının bulunduğu konuma ayarlı olduğundan emin olmak için de denetleyin. 

## <a name="see-also"></a>Ayrıca bkz.

- [Medya Hizmetleri hesabına dosya yükleme - REST](upload-files-rest-how-to.md)
- [Medya Hizmetleri ile filtre oluşturma - REST](filters-dynamic-manifest-rest-howto.md)
- [Azure Resource Manager tabanlı REST API](https://github.com/Azure-Samples/media-services-v3-arm-templates)

## <a name="next-steps"></a>Sonraki adımlar

- [REST ile dosyaları akış.](stream-files-tutorial-with-rest.md)  
- [Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışı - REST](stream-files-tutorial-with-rest.md)
