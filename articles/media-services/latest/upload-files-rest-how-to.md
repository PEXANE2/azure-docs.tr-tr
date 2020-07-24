---
title: REST kullanarak Azure Media Services v3 hesabına dosya yükleme | Microsoft Docs
description: Varlıklar oluşturup karşıya yükleyerek Media Services medya içeriğini nasıl alabileceğinizi öğrenin.
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
ms.openlocfilehash: 57839c82af71f7cedfe1b3c77095210d273be4a1
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091853"
---
# <a name="upload-files-into-a-media-services-v3-account-using-rest"></a>REST kullanarak Media Services v3 hesabına dosya yükleme

Media Services, dijital dosyalarınızı bir varlıkla ilişkili bir blob kapsayıcısına yüklersiniz. Varlık [varlığı video](/rest/api/media/operations/asset) , ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler) içerebilir. Dosyalar varlığın kapsayıcısına yüklendikten sonra, içeriğiniz daha fazla işlem ve akış için bulutta güvenli bir şekilde depolanır.

Bu makalede, REST kullanarak yerel bir dosyayı karşıya yükleme işleminin nasıl yapılacağı gösterilir.

## <a name="prerequisites"></a>Önkoşullar

Bu konuda açıklanan adımları tamamlayabilmeniz için şunları yapmanız gerekir:

- [Varlık kavramını](assets-concept.md)gözden geçirin.
- [Azure Media Services REST API'si çağrıları Için Postman 'ı yapılandırın](media-rest-apis-with-postman.md).
    
    [Azure AD belirtecini al](media-rest-apis-with-postman.md#get-azure-ad-token)konusunun son adımını izlediğinizden emin olun. 

## <a name="create-an-asset"></a>Varlık oluşturma

Bu bölümde, yeni bir varlığın nasıl oluşturulacağı gösterilmektedir.

1. **Assets**  ->  **Varlık oluşturma veya güncelleştirme**varlıkları ' nı seçin.
2. **Gönder**’e basın.

    ![Varlık oluşturma](./media/upload-files/postman-create-asset.png)

Yeni oluşturulan varlık hakkında bilgi ile **yanıtı** görürsünüz.

## <a name="get-a-sas-url-with-read-write-permissions"></a>Okuma/yazma izinlerine sahip SAS URL 'SI alın 

Bu bölüm, oluşturulan varlık için oluşturulan bir SAS URL 'sinin nasıl alınacağını gösterir. SAS URL 'SI okuma-yazma izinleriyle oluşturulmuştur ve varlık kapsayıcısına dijital dosyaları yüklemek için kullanılabilir.

1. **Assets**  ->  **Varlık URL 'lerini**seçin varlıklar listesini seçin.
2. **Gönder**’e basın.

    ![Dosyayı karşıya yükleme](./media/upload-files/postman-create-sas-locator.png)

Varlığın URL 'Leri hakkında bilgi içeren **yanıtı** görürsünüz. İlk URL 'YI kopyalayın ve dosyanızı karşıya yüklemek için kullanın.

## <a name="upload-a-file-to-blob-storage-using-the-upload-url"></a>Karşıya yükleme URL 'sini kullanarak blob depolamaya bir dosya yükleme

Azure depolama API 'Lerini veya SDK 'Larını (örneğin, [depolama REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Öğretici: Uzak dosya tabanlı URL kodlama ve video akışı yapma - REST](stream-files-tutorial-with-rest.md)
