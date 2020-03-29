---
title: Azure portalında Media Encoder Standard'ı kullanarak bir varlığı kodlayın | Microsoft Dokümanlar
description: Bu öğretici, Azure portalında Media Encoder Standard'ı kullanarak bir varlığı kodlama adımlarında size yol açar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 107d9e9a-71e9-43e5-b17c-6e00983aceab
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 7c147d99eaabee70316521d1f2bdc41933162ab1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69542614"
---
# <a name="encode-an-asset-by-using-media-encoder-standard-in-the-azure-portal"></a>Azure portalında Media Encoder Standard kullanarak varlık kodlama

> [!NOTE]
> Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın. 
> 
> 

Azure Medya Hizmetleri ile çalışırken en yaygın senaryolardan biri, müşterilerinize uyarlanabilir bithızı akışı sağlamaktır. Medya Hizmetleri aşağıdaki uyarlanabilir bitrate akış teknolojilerini destekler: Apple HTTP Live Streaming (HLS), Microsoft Smooth Streaming ve Http üzerinden Dinamik Uyarlamalı Akış (DASH, ayrıca MPEG-DASH olarak da adlandırılır). Videolarınızı uyarlanabilir bithızı akışına hazırlamak için, önce kaynak videonuzu çok bit hızında dosyalar olarak kodlayın. Videolarınızı kodlamak için Media Encoder Standard'ı kullanabilirsiniz.  

Media Services dinamik paketleme olanağı sağlar. Dinamik paketleme ile, çok bit hızındaki MP4'lerinizi HLS, Smooth Streaming ve MPEG-DASH'te bu akış biçimlerinde yeniden paketlemeden teslim edebilirsiniz. Dinamik ambalaj kullandığınızda, dosyaları tek depolama biçiminde saklayabilir ve ödenebilirsiniz. Medya Hizmetleri, müşterinin isteğine bağlı olarak uygun yanıtı oluşturur ve hizmet eder.

Dinamik paketlemeden yararlanmak için kaynak dosyanızı çoklu bit hızına sahip bir dizi MP4 dosyası olarak kodlamanız gerekir. Kodlama adımları daha sonra bu makalede gösterilmiştir.

Medya işlemeyi nasıl ölçeklendireceklerini öğrenmek için [Azure portalını kullanarak medya işlemeyi ölçeklendir'e](media-services-portal-scale-media-processing.md)bakın.

## <a name="encode-in-the-azure-portal"></a>Azure portalında kodlama

Media Encoder Standard'ı kullanarak içeriğinizi kodlamak için:

1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** > **Varlıkları'nı**seçin. Kodlamak istediğiniz varlığı seçin.
3. **Kodla** düğmesini seçin.
4. **Bir varlık kodla** bölmesinde, **Media Encoder Standard** işlemcisini ve bir ön ayarı seçin. Hazır ayarlar hakkında daha fazla bilgi için bkz. [Bit hızı merdivenini otomatik oluşturma](media-services-autogen-bitrate-ladder-with-mes.md) ve [Media Encoder Standard için görev ön ayarları](media-services-mes-presets-overview.md). Girdi videonuz için en iyi sonucu verecek hazır ayarı seçmeniz önemlidir. Örneğin, girdi videonuzun 1920 &#215; 1080 piksel çözünürlüğü olduğunu biliyorsanız, **H264 Çoklu Bit hızı 1080p** ön ayarını kullanabilirsiniz. Düşük çözünürlüklü bir videonuz varsa (640 & #215; 360), **H264 Çoklu Bit Hızı 1080p** ön ayarını kullanmamalısınız.
   
   Kaynaklarınızın yönetmenize yardımcı olmak için çıktı varlığının ve işin adını düzenleyebilirsiniz.
   
   ![Varlıkları kodlama](./media/media-services-portal-vod-get-started/media-services-encode1.png)
5. **Oluştur'u**seçin.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Kodlama işinizin ilerlemesini](media-services-portal-check-job-progress.md) Azure portalında izleyin.  

