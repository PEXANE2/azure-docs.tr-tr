---
title: Yerel dosyadan Azure Media Services iş girişi oluştur | Microsoft Docs
description: Bu makalede, yerel bir dosyadan Azure Media Services işi girişi oluşturma işlemi gösterilmektedir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/18/2020
ms.author: inhenkel
ms.openlocfilehash: 4d7e6d9daa5d30c5cbf5b198a147564d0c05c074
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88607227"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel dosyadan iş girişi oluşturma

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Giriş videosu bir medya hizmeti varlığı olarak depolanabilir, bu durumda bir dosyayı temel alan bir giriş varlığı (yerel olarak veya Azure Blob depolama alanında depolanır) oluşturabilirsiniz. Bu konu başlığı altında, yerel bir dosyadan nasıl iş girişi oluşturacağınız gösterilmektedir. Tam bir örnek için bkz. [GitHub örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Önkoşullar 

* [Media Services hesabı oluşturun](./create-account-howto.md).

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
