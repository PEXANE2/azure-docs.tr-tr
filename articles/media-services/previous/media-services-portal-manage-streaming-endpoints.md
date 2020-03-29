---
title: Azure portalı ile akış uç noktalarını yönetme | Microsoft Dokümanlar
description: Bu makalede, Azure portalı ile akış uç noktalarının nasıl yönetilen gösterildiği gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
writer: juliako
manager: femila
editor: ''
ms.assetid: bb1aca25-d23a-4520-8c45-44ef3ecd5371
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 57ec22cb02512577f8737718cae9175403fb5603
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900875"
---
# <a name="manage-streaming-endpoints-with-the-azure-portal"></a>Azure portalı ile akış uç noktalarını yönetme 

Bu makalede, akış uç noktalarını yönetmek için Azure portalının nasıl kullanılacağı gösterilmektedir. 

>[!NOTE]
>[Genel bakış](media-services-streaming-endpoints-overview.md) makalesini gözden geçirdiğinizden emin olun. 

Akış bitiş noktasının nasıl ölçeklendirilenhakkında bilgi için [bu](media-services-portal-scale-streaming-endpoints.md) makaleye bakın.

## <a name="start-managing-streaming-endpoints"></a>Akış uç noktalarını yönetmeye başlayın 

Hesabınız için akış uç noktalarını yönetmeye başlamak için aşağıdakileri yapın.

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** bıyıkta, **Akış uç noktalarını**seçin.
   
    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints1.png)

> [!NOTE]
> Yalnızca Akış Bitiş Noktanız çalışırken faturalandırılırsınız.

## <a name="adddelete-a-streaming-endpoint"></a>Akış bitiş noktası ekleme/silme

>[!NOTE]
>Varsayılan akış bitiş noktası silinemez.

Azure portalını kullanarak akış bitiş noktası eklemek/silmek için aşağıdakileri yapın:

1. Akış bitiş noktası eklemek için sayfanın üst kısmındaki **+ Bitiş Noktasını** tıklatın. 

    Farklı CDN'lere veya CDN'lere ve doğrudan erişime sahip olmayı planlıyorsanız, birden çok Akış Bitiş Noktası isteyebilirsiniz.

2. Akış bitiş noktasını silmek için **Sil** düğmesine basın.      
3. Akış bitiş noktasını başlatmak için **Başlat** düğmesini tıklatın.
   
    ![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints2.png)


## <a name="configuring-the-streaming-endpoint"></a><a id="configure_streaming_endpoints"></a>Akış Bitiş Noktasını Yapılandırma
Akış Bitiş Noktası aşağıdaki özellikleri yapılandırmanızı sağlar:

* Erişim denetimi
* Önbellek denetimi
* Site ler arası erişim ilkeleri

Bu özellikler hakkında ayrıntılı bilgi için [Bkz.](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)

>[!NOTE]
>CDN etkinleştirildiğinde, IP erişimine erişemezsiniz. IP erişimi yalnızca CDN'niz yoksa uygulanabilir.

Aşağıdakileri yaparak akış bitiş noktasını yapılandırabilirsiniz:

1. Yapılandırmak istediğiniz akış bitiş noktasını seçin.
2. **Ayarlar**'a tıklayın.

Alanların kısa bir açıklaması aşağıda dır.

![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints4.png)

1. Maksimum önbellek ilkesi: Bu akış bitiş noktası üzerinden hizmet verilen varlıklar için önbellek ömrünü yapılandırmak için kullanılır. Değer ayarlı değilse, varsayılan kullanılır. Varsayılan değerler doğrudan Azure depolama alanında da tanımlanabilir. Akış bitiş noktası için Azure CDN etkinse, önbellek ilkesi değerini 600 saniyenin altında ayarlamamalısınız.  
2. İzin verilen IP adresleri: yayınlanan akış bitiş noktasına bağlanmasına izin verilecek IP adreslerini belirtmek için kullanılır. IP adresi belirtilmemişse, herhangi bir IP adresi bağlanabilir. IP adresleri tek bir IP adresi (örneğin, '10.0.0.0.1'), IP adresi ve CIDR alt ağ maskesi kullanan bir IP aralığı (örneğin, '10.0.0.1/22') veya IP adresi ve noktalı ondalık alt ağ maskesi (örneğin, '10.0.0.1(255.255.25'05.05)) kullanan bir IP aralığı olarak belirtilebilir.
3. Akamai imza üstbilgi kimlik doğrulaması için yapılandırma: Akamai sunucularından imza üstbilgi kimlik doğrulama isteğinin nasıl yapılandırıldığı belirtilir. Sona erme UTC'dedir.

## <a name="scale-your-premium-streaming-endpoint"></a>Premium akış bitiş noktanızı ölçeklendirin

Daha fazla bilgi için [bu makaleye](media-services-portal-scale-streaming-endpoints.md) bakın.

## <a name="enable-azure-cdn-integration"></a><a id="enable_cdn"></a>Azure CDN tümleştirmesini etkinleştirme

Yeni bir hesap oluşturduğunuzda, varsayılan Olarak Son Nokta Sonu Azure CDN tümleştirmesi etkinleştirilir.

Daha sonra CDN'yi devre dışı/etkinleştirmek istiyorsanız, akış bitiş noktanızın **durdurulmuş** durumda olması gerekir. Azure CDN tümleştirmesinin etkinleştirilmesi ve değişikliklerin tüm CDN AP'leri arasında etkin olması iki saat kadar sürebilir. Ancak, akış bitiş noktası ve akışı nızı akış bitiş noktasından kesintisiz olarak başlatabilirsiniz ve tümleştirme tamamlandıktan sonra akış CDN'den teslim edilir. Sağlama süresi boyunca akış bitiş noktanız **başlangıç** durumunda olacaktır ve bozulmuş performans gözlemleebilirsiniz.

CDN tümleştirmesi Çin ve Federal Devlet bölgeleri dışındaki tüm Azure veri merkezlerinde etkinleştirilir.

Etkinleştirildiğinde, Erişim **Denetimi**, **Özel ana bilgisayar adı ve **Akamai Signature kimlik doğrulama** yapılandırması devre dışı bırakılır.
 
> [!IMPORTANT]
> Azure CDN ile Azure Medya Hizmetleri entegrasyonu, standart akış uç noktaları için **Verizon'dan Azure CDN'de** uygulanır. Premium akış bitiş noktaları tüm **Azure CDN fiyatlandırma katmanları ve sağlayıcıları**kullanılarak yapılandırılabilir. Azure CDN özellikleri hakkında daha fazla bilgi için [CDN genel bakış](../../cdn/cdn-overview.md)bilgisine bakın.
 
### <a name="additional-considerations"></a>Diğer konular

* Akış bitiş noktası için CDN etkinleştirildiğinde, istemciler doğrudan kaynaktan içerik isteyemez. CdN'li veya CDN'siz içeriğinizi sınamak için gereken lere ihtiyacınız varsa, CDN etkin olmayan başka bir akış bitiş noktası oluşturabilirsiniz.
* CDN'yi etkinleştirdikten sonra akış uç nokta ana adınızıaynı kalır. CDN etkinleştirildikten sonra medya hizmetleri iş akışınızda herhangi bir değişiklik yapmanız gerekmez. Örneğin, akış uç nokta ana adınızı strasbourg.streaming.mediaservices.windows.net, CDN etkinleştirdikten sonra, aynı ana bilgisayar adı kullanılır.
* Yeni akış uç noktaları için, cdn'yi yalnızca yeni bir uç nokta oluşturarak etkinleştirebilirsiniz; varolan akış uç noktaları için önce bitiş noktasını durdurmanız ve ardından CDN'yi etkinleştirmeniz/devre dışı bırakmanız gerekir.
* Standart akış bitiş noktası yalnızca Azure klasik portalı kullanılarak **Verizon Standart CDN sağlayıcısı** kullanılarak yapılandırılabilir. Ancak, REST API'lerini kullanarak diğer Azure CDN sağlayıcılarına etkinleştirebilirsiniz.

## <a name="configure-cdn-profile"></a>CDN profilini yapılandırma

CdN profilini üstten **CDN'yi yönet** düğmesini seçerek yapılandırabilirsiniz.

![Akış uç noktası](./media/media-services-portal-manage-streaming-endpoints/media-services-manage-streaming-endpoints6.png)

## <a name="next-steps"></a>Sonraki adımlar
Media Services öğrenme yollarını gözden geçirin.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

