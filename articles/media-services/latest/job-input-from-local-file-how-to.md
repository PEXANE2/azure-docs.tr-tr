---
title: Yerel bir dosyadan Azure Medya Hizmetleri iş girişi oluşturma | Microsoft Dokümanlar
description: Bu makalede, yerel bir dosyadan Azure Medya Hizmetleri iş girişinasıl oluşturulacak gösteriş.
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
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80345919"
---
# <a name="create-a-job-input-from-a-local-file"></a>Yerel bir dosyadan iş girişi oluşturma

Media Services v3 sürümünde işlenecek İşleri videolarınıza gönderirken Media Services'a giriş videosunun yerini de bildirmeniz gerekir. Giriş videosu Bir Medya Hizmet Varlığı olarak depolanabilir ve bu durumda bir dosyaya dayalı bir giriş varlığı oluşturursunuz (yerel olarak veya Azure Blob depolama alanında depolanır). Bu konu, yerel bir dosyadan iş girişinin nasıl oluşturulup oluşturulabildiğini gösterir. Tam bir örnek için bu [GitHub örneğine](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs)bakın.

## <a name="prerequisites"></a>Ön koşullar 

* [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)
* Varlıkları yönetmeyi gözden [geçirin.](manage-asset-concept.md)

## <a name="net-sample"></a>.NET örneği

Aşağıdaki kod, bir giriş kıymetinin nasıl oluşturulup iş girişi olarak kullanılacağını gösterir. CreateInputAsset işlevi aşağıdaki eylemleri gerçekleştirir:

* Varlığı Oluşturur
* Varlığın [depolamadaki kapsayıcısına](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#upload-blobs-to-a-container) yazılabilir bir [SAS URL](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)’si alır
* SAS URL’sini kullanarak dosyayı depolamadaki kapsayıcıya yükler

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateInputAsset)]

Aşağıdaki kod snippet zaten yoksa bir çıktı varlık oluşturur:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#CreateOutputAsset)]

Aşağıdaki kod snippet bir kodlama işi gönderir:

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/UploadEncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="job-error-codes"></a>İş hata kodları

[Bkz. Hata kodları.](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)

## <a name="next-steps"></a>Sonraki adımlar

[HTTPS URL'sinden iş girişi oluşturun.](job-input-from-http-how-to.md)
