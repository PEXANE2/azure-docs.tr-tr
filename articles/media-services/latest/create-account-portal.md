---
title: Azure portalı ile Azure Media Services hesabı oluşturma
description: Bu öğretici, Azure portalıyla bir Azure Media Services hesabı oluşturma adımlarında size kılavuzluk eder.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 03/09/2020
ms.author: juliako
ms.openlocfilehash: c90278eccf889595378d3b6b07de2468910c660c
ms.sourcegitcommit: 72c2da0def8aa7ebe0691612a89bb70cd0c5a436
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2020
ms.locfileid: "79080885"
---
# <a name="use-the-azure-portal-to-create-a-media-services-account"></a>Media Services hesabı oluşturmak için Azure portal kullanın

Azure portal, hızlı bir şekilde Azure Media Services hesabı oluşturmak için bir yol sağlar. Azure’da medya içeriği depolamanıza, şifrelemenize, kodlamanıza, yönetmenize ve akış yapmanıza imkan tanıyan Media Services’e erişmek için hesabınızı kullanabilirsiniz.

Şu anda [Azure Portal](https://portal.azure.com/) şunları yapmak için kullanabilirsiniz:

* Media Services v3 [canlı olaylarını](live-events-outputs-concept.md)yönetin, 
* v3 [varlıklarını](assets-concept.md)görüntüleme (yönetme), 
* [API 'lere erişme hakkında bilgi alın](access-api-portal.md). 

Diğer tüm yönetim görevleri (örneğin, [dönüşümler ve işler](transforms-jobs-concept.md) ve [içerik koruması](content-protection-overview.md)) için [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK 'lardan](media-services-apis-overview.md#sdks)birini kullanın.

Bu makalede Azure portal kullanılarak Media Services hesabının nasıl oluşturulacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açın.
1. **+ Kaynak oluştur** > **medya** > **Media Services**' na tıklayın.
1. **Media Services hesabı oluştur** bölümünde gerekli değerleri girin.
    
    | Adı | Açıklama |
    | ---|---|
    |**Hesap Adı**|Yeni Media Services hesabının adını girin. Media Services hesabı adı, boşluk olmadan, tümü sayı ve küçük harften oluşmalı ve 3-24 karakter uzunluğunda olmalıdır.|
    |**Abonelik**|Birden fazla aboneliğiniz varsa, erişiminiz olan Azure abonelikleri listesinden birini seçin.|
    |**Kaynak Grubu**|Yeni veya mevcut kaynağı seçin. Kaynak grubu; yaşam döngüsünü, izinleri ve ilkeleri paylaşan kaynakların bir koleksiyonudur. [Burada](../../azure-resource-manager/management/overview.md#resource-groups) daha fazla bilgi edinin.|
    |**Konum**|Media Services hesabınız için medya ve meta veri kayıtlarını depolamak üzere kullanılacak coğrafi bölgeyi seçin. Bu bölge medyanızı işlemek ve akışını sağlamak için kullanılır. Yalnızca Media Services kullanılabilen bölgeler açılır listede görüntülenir. |
    |**Depolama Hesabı**|Media Services hesabınızdan medya içeriğinin blob depolamasını sağlamak için bir depolama hesabı seçin. Media Services hesabınızla aynı coğrafi bölgede bulunan mevcut bir depolama hesabını seçebilir ya da yeni bir depolama hesabı oluşturabilirsiniz. Aynı bölgede yeni bir depolama hesabı oluşturulur. Depolama hesabı adları için kurallar Media Services hesapları ile aynıdır.<br/><br/>Tek bir **Birincil** depolama hesabınız olması gerekir, ancak Media Services hesabınızla ilişkili istediğiniz sayıda **İkincil** depolama hesabınız olabilir. İkincil depolama hesaplarını eklemek için Azure portal kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Media Services hesapları Ile Azure depolama hesapları](storage-account-concept.md).<br/><br/>Media Services hesabı ve onunla ilişkili tüm depolama hesaplarının aynı Azure aboneliğinde olması gerekir. Gecikme süreleri ve veri çıkış maliyetleri ile karşı karşıya kalmamak için mutlaka Media Services ile aynı konumda bulunan depolama hesaplarını kullanmanız önerilir.|
    
1. Hesap dağıtımını ilerleme durumunu görmek için **Panoya sabitle**’yi seçin.
1. Formun alt kısmındaki **Oluştur**’a tıklayın.
   
    Hesap başarıyla oluşturulduktan sonra genel bakış sayfası yüklenir. Akış uç noktası tablosunda, hesabın **durdurulmuş** durumda bir varsayılan akış uç noktası olacaktır. 

    Media Services hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçeriğinizi akışa almak ve [dinamik paketleme](dynamic-packaging-overview.md) ile [dinamik şifrelemeden](content-protection-overview.md)yararlanmak için içerik akışı yapmak Istediğiniz akış uç noktasının **çalışıyor** durumda olması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar

Media Services API ile programlı olarak erişmeyi planlıyorsanız bkz. [Azure AD kimlik doğrulamasıyla Azure Media Services API 'Sine erişme](access-api-portal.md).

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

