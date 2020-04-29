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
ms.openlocfilehash: aba987ba232a29ffc240f72039b1e24bb87a2ed4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80345919"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel dosyadan iş girişi oluşturma

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Giriş videosu bir medya hizmeti varlığı olarak depolanabilir, bu durumda bir dosyayı temel alan bir giriş varlığı (yerel olarak veya Azure Blob depolama alanında depolanır) oluşturabilirsiniz. Bu konu başlığı altında, yerel bir dosyadan nasıl iş girişi oluşturacağınız gösterilmektedir. Tam bir örnek için bkz. [GitHub örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs).

## <a name="prerequisites"></a>Ön koşullar 

* [Media Services hesabı oluşturun](create-account-cli-how-to.md).
* [Varlıkları Yönet](manage-asset-concept.md)' i gözden geçirin.

## <a name="net-sample"></a>.NET örneği

Aşağıdaki kod, bir giriş varlığının nasıl oluşturulduğunu ve iş için giriş olarak nasıl kullanılacağını gösterir. Createınputasset işlevi aşağıdaki eylemleri gerçekleştirir:

* Varlığı oluşturur
* Varlığın [depolamadaki kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container) yazılabilir bir [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Aşağıdaki kod parçacığı, zaten mevcut değilse bir çıktı varlığı oluşturur:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Aşağıdaki kod parçacığı bir kodlama işi gönderir:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>İş hata kodları

Bkz. [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="next-steps"></a>Sonraki adımlar

[BIR https URL 'sinden iş girişi oluşturun](job-input-from-http-how-to.md).
