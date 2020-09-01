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
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: b983602c94acc9ffc0048a9753693d7c16869494
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89265567"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel dosyadan iş girişi oluşturma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Giriş videosu bir medya hizmeti varlığı olarak depolanabilir, bu durumda bir dosyayı temel alan bir giriş varlığı (yerel olarak veya Azure Blob depolama alanında depolanır) oluşturabilirsiniz. Bu konu başlığı altında, yerel bir dosyadan nasıl iş girişi oluşturacağınız gösterilmektedir. Tam bir örnek için bkz. [GitHub örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Ön koşullar 

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
