---
title: Azure portalını kullanarak Media Services hesabına dosya yükleme | Microsoft Belgeleri
description: Bu öğretici, Azure portalını kullanarak bir Azure Media Services hesabına dosya yükleme adımlarında size kılavuzluk eder
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 3ad3dcea-95be-4711-9aae-a455a32434f6
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: 3ebeb3c601dd3f734265d49d60728056561928be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61127828"
---
# <a name="upload-files-to-a-media-services-account-in-the-azure-portal"></a>Azure portalını kullanarak Media Services hesabına dosya yükleme 

> [!div class="op_single_selector"]
> * [Portal](media-services-portal-upload-files.md)
> * [.NET](media-services-dotnet-upload-files.md)
> * [Geri kalanı](media-services-rest-upload-files.md)
> 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Media Services’de dijital dosyalar bir varlığa yüklenir. Varlık; video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar için meta veriler) içerebilir. Dosyalar yüklendiğinde, içeriğiniz sonraki işleme ve akışla aktarma faaliyetleri için güvenli bir şekilde bulutta depolanmış olur.

Media Services dosyalarını işlemek için bir maksimum dosya boyutu vardır. Dosya boyutu sınırları hakkında daha fazla ayrıntı için bkz. [Media Services kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).

Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Ayrıntılar için [Azure ücretsiz deneme sürümüne](https://azure.microsoft.com/pricing/free-trial/)bakın. 

## <a name="upload-files"></a>Dosyaları karşıya yükleme
1. [Azure portalında](https://portal.azure.com/) Azure Media Services hesabınızı seçin.
2. **Ayarlar** > **Varlıkları'nı**seçin. Ardından **Karşıya Yükle** düğmesini seçin.
   
    ![Dosyaları karşıya yükleme](./media/media-services-portal-vod-get-started/media-services-upload.png)
   
    **Varlığı karşıya yükle** penceresi görüntülenir.
   
   > [!NOTE]
   > Media Services, karşıya yüklenecek videoların dosya boyutunu kısıtlamaz.
 
3. Bilgisayarınızda, karşıya yüklemek istediğiniz videoya gidin. Videoyu seçin ve ardından **Tamam**’ı seçin.  
   
    Karşıya yükleme başlar. Dosya adı altında ilerleme durumunu görebilirsiniz.  

Karşıya yükleme tamamlandığında, yeni varlık **Varlıklar** bölmesinde listelenir. 

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Karşıya yüklenen varlıklarınızı kodlamayı](media-services-portal-encode.md) öğrenin.

* Yapılandırılmış kapsayıcıya gelen dosyaya göre bir kodlama işi tetiklemek için Azure İşlevleri’ni de kullanabilirsiniz. Daha fazla bilgi için [Media Services: Azure Media Services’ı Azure İşlevleri ve Logic Apps ile tümleştirme](https://azure.microsoft.com/resources/samples/media-services-dotnet-functions-integration/) örneğine bakın.


