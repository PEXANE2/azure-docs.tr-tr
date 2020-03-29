---
title: Azure Medya Hizmetleri REST API çağrıları için Postacı'yı yapılandır
description: Bu makalede, Media Services REST API çağrıları için Postacı nasıl yapılandırılabilen açıklanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76694999"
---
# <a name="configure-postman-for-media-services-v2-rest-api-calls"></a>Medya Hizmetleri v2 REST API aramaları için Postacı yapılandırma  

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Bu öğretici, **Postacı'yı** Azure Medya Hizmetleri (AMS) REST API'lerini aramak için kullanılabileceğini nasıl yapılandırabileceğinizi gösterir. Öğretici **postacı**içine çevre ve toplama dosyaları nasıl içe aktarılmayı gösterir. Koleksiyon, Azure Medya Hizmetleri (AMS) REST API'leri olarak adlandırdığı HTTP isteklerinin gruplanmış tanımlarını içerir. Ortam dosyası, koleksiyon tarafından kullanılan değişkenleri içerir.

Bu ortam ve koleksiyon, Azure Medya Hizmetleri REST API'leri ile çeşitli görevlerin nasıl başarılabildiğini gösteren makalelerde kullanılır.

## <a name="prerequisites"></a>Ön koşullar

- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer seçenekler şunlardır: REST eklentili **Visual Studio Code** veya **Telerik Fiddler**. 

## <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. AMS öğreticilerinde kullanılan ortam değişkenlerini içeren bir .json dosyası oluşturun. Dosyayı adlandırın (örneğin, **AzureMediaServices.postman_environment.json).** Dosyayı açın ve bu kod listesinden Postacı ortamını tanımlayan kodu [yapıştırın.](postman-environment.md) 
2. **Postman**'ı açın.
3. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-create-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
5. **AzureMediaServices.postman_environment.json** dosyasına göz atın ve seçin.
6. **AzureMedia** ortamı eklenir.
7. İletişim kutusunu kapatın.
8. **AzureMedia** ortamını seçin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-choose-env.png)

## <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Bir **dosyayı** Medya Hizmetleri'ne yüklemek için gereken tüm işlemleri içeren bir .json dosyası oluşturun. Dosyayı adlandırın (örneğin, **AzureMediaServicesOperations.postman_collection.json).** Dosyayı açın ve bu kod listesinden **Postacı** koleksiyonunu tanımlayan kodu [yapıştırın.](postman-collection.md)
2. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
3. **AzureMediaServicesOperations.postman_collection.json** dosyasını seçin.

    ![Dosyayı karşıya yükleme](./media/media-services-rest-upload-files/postman-import-collection.png)

## <a name="next-steps"></a>Sonraki adımlar

[Yükleme varlıkları](media-services-rest-upload-files.md) makalesine göz atın.  
