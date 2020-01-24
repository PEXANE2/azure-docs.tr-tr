---
title: Azure Media Services REST API'si çağrıları için Postman yapılandırma
description: Bu makalede, Media Services REST API çağrıları için Postman 'ın nasıl yapılandırılacağı açıklanır.
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
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 11c9c26e7c0f36e1e3dba732e90a6aef95e6ee14
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76694999"
---
# <a name="configure-postman-for-media-services-v2-rest-api-calls"></a>Media Services V2 REST API çağrıları için Postman yapılandırma  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz. [v2 'den v3 'e geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)

Bu öğreticide, Azure Media Services (AMS) REST API 'Lerini çağırmak için kullanılacak **Postman** 'ın nasıl yapılandırılacağı gösterilmektedir. Öğreticide ortam ve koleksiyon dosyalarının **Postman**'a nasıl aktarılacağı gösterilmektedir. Koleksiyon, Azure Media Services (AMS) REST API 'Lerini çağıran HTTP isteklerinin gruplandırılmış tanımlarını içerir. Ortam dosyası, koleksiyon tarafından kullanılan değişkenleri içerir.

Bu ortam ve koleksiyon, Azure Media Services REST API 'Leri ile çeşitli görevlerin nasıl elde alınacağını gösteren makalelerde kullanılır.

## <a name="prerequisites"></a>Ön koşullar

- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer seçenekler şunlardır: REST eklentili **Visual Studio Code** veya **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. AMS öğreticilerinde kullanılan ortam değişkenlerini içeren bir. JSON dosyası oluşturun. Dosyayı adlandırın (örneğin, **Azudüzeltici Aservices. postman_environment. JSON**). Dosyayı açın ve Postman ortamını tanımlayan kodu [Bu kod](postman-environment.md)listesinden yapıştırın. 
2. **Postman**'ı açın.
3. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
5. **Azudüzeltici Aservices. postman_environment. JSON** dosyasına gözatıp seçin.
6. **AzureMedia** ortamı eklenir.
7. İletişim kutusunu kapatın.
8. **AzureMedia** ortamını seçin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Media Services bir dosyayı karşıya yüklemek için gereken tüm işlemlerle **Postman** koleksiyonunu içeren bir. JSON dosyası oluşturun. Dosyayı adlandırın (örneğin, **Azudüzeltici Aservicesoperations. postman_collection. JSON**). Dosyayı açın ve **Postman** koleksiyonunu tanımlayan kodu [Bu kod](postman-collection.md)listesinden yapıştırın.
2. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
3. **Azudüzeltici Aservicesoperations. postman_collection. JSON** dosyasını seçin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Sonraki adımlar

[Varlık yükle](media-services-rest-upload-files.md) makalesine göz atın.  
