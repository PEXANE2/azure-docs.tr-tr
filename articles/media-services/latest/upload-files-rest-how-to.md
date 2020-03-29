---
title: REST kullanarak dosyaları Azure Media Services v3 hesabına yükleme | Microsoft Dokümanlar
description: Varlıklar oluşturup yükleyerek medya içeriğini Medya Hizmetleri'ne nasıl dahil ediniz öğrenin.
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
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 38d46978e37ead59deb17a86f643df041452e497
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76705777"
---
# <a name="upload-files-into-a-media-services-v3-account-using-rest"></a>REST kullanarak dosyaları Medya Hizmetleri v3 hesabına yükleme

Medya Hizmetleri'nde, dijital dosyalarınızı bir varlıkla ilişkili bir blob kapsayıcısına yüklersiniz. [Varlık](https://docs.microsoft.com/rest/api/media/operations/asset) varlığı video, ses, görüntü, küçük resim koleksiyonları, metin parçaları ve kapalı altyazı dosyaları (ve bu dosyalarla ilgili meta veriler) içerebilir. Dosyalar varlığın konteynerine yüklendikten sonra, içeriğiniz daha fazla işlem ve akış için bulutta güvenli bir şekilde depolanır.

Bu makalede, REST kullanarak yerel bir dosyayı nasıl yükleyebilirsiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu konuda açıklanan adımları tamamlamak için şunları yapmak zorundasınız:

- Varlık kavramını gözden [geçirin.](assets-concept.md)
- [Azure Medya Hizmetleri REST API çağrıları için Postacı yapılandırın.](media-rest-apis-with-postman.md)
    
    [Azure AD Belirteci'ni alın'](media-rest-apis-with-postman.md#get-azure-ad-token)ın son adımını takip edin. 

## <a name="create-an-asset"></a>Varlık oluşturma

Bu bölümde, yeni bir Varlığın nasıl oluşturulabildiğini gösterilmektedir.

1. **Varlık** -> **Oluştur'u veya Güncelleştir'i**seçin.
2. **Gönder**’e basın.

    ![Varlık oluşturma](./media/upload-files/postman-create-asset.png)

Yeni oluşturulan varlıkla ilgili bilgilerle **Yanıt'ı** görürsünüz.

## <a name="get-a-sas-url-with-read-write-permissions"></a>Okuma yazma izinleri içeren bir SAS URL'si edinin 

Bu bölümde, oluşturulan varlık için oluşturulan bir SAS URL'sinin nasıl alınıldığı gösterilmektedir. SAS URL okuma yazma izinleriyle oluşturulmuştur ve dijital dosyaları Varlık kapsayıcısına yüklemek için kullanılabilir.

1. **Varlıklar** -> **Listesi'ni seçin.**
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/upload-files/postman-create-sas-locator.png)

Varlığın URL'leri hakkındaki bilgileri içeren **Yanıt'ı** görürsünüz. İlk URL'yi kopyalayın ve dosyanızı yüklemek için kullanın.

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Yükleme URL'sini kullanarak blob depolama alanına dosya yükleme

Azure Depolama API'lerini veya SDK'larını (örneğin, [Depolama REST API'sını](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK'yı)](../../storage/blobs/storage-quickstart-blobs-dotnet.md)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışı - REST](stream-files-tutorial-with-rest.md)
