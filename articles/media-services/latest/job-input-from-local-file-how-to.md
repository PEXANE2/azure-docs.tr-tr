---
title: Yerel dosyadan Azure Media Services iş girişi oluştur | Microsoft Docs
description: Bu makalede, yerel bir dosyadan Azure Media Services işi girişi oluşturma işlemi gösterilmektedir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/26/2020
ms.author: juliako
ms.openlocfilehash: a635a75a64db59fc56c6890f79af967a58735853
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87001137"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel dosyadan iş girişi oluşturma

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Giriş videosu bir medya hizmeti varlığı olarak depolanabilir, bu durumda bir dosyayı temel alan bir giriş varlığı (yerel olarak veya Azure Blob depolama alanında depolanır) oluşturabilirsiniz. Bu konu başlığı altında, yerel bir dosyadan nasıl iş girişi oluşturacağınız gösterilmektedir. Tam bir örnek için bkz. [GitHub örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Önkoşullar 

* [Media Services hesabı oluşturun](./create-account-howto.md).
* [Varlıkları Yönet](manage-asset-concept.md)' i gözden geçirin.

## <a name="net-sample"></a>.NET örneği

Aşağıdaki kod, bir giriş varlığının nasıl oluşturulduğunu ve iş için giriş olarak nasıl kullanılacağını gösterir. Createınputasset işlevi aşağıdaki eylemleri gerçekleştirir:

* Varlığı oluşturur
* Varlığın [depolamadaki kapsayıcısına](../../storage/blobs/storage-quickstart-blobs-dotnet.md#upload-blobs-to-a-container) yazılabilir bir [SAS URL](../../storage/common/storage-sas-overview.md)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Aşağıdaki kod parçacığı, zaten mevcut değilse bir çıktı varlığı oluşturur:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Aşağıdaki kod parçacığı bir kodlama işi gönderir:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>İş hata kodları

Bkz. [hata kodları](/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

[BIR https URL 'sinden iş girişi oluşturun](job-input-from-http-how-to.md).
