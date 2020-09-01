---
title: Medyayı karşıya yükleme
titleSuffix: Azure Media Services
description: Akış veya kodlama için medya yüklemeyi öğrenin.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.date: 08/31/2020
ms.author: inhenkel
ms.openlocfilehash: 334eebe9680ba0e381bf82ac2e474a47a3b8875e
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89267301"
---
# <a name="upload-media-for-streaming-or-encoding"></a>Akış veya kodlama için medya yükleme

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Services, dijital dosyalarınızı (medya) bir varlıkla ilişkili bir blob kapsayıcısına yüklersiniz. Varlık [varlığı video](/rest/api/media/operations/asset) , ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları (ve bu dosyalar hakkındaki meta veriler) içerebilir. Dosyalar varlığın kapsayıcısına yüklendikten sonra, içeriğiniz daha fazla işlem ve akış için bulutta güvenli bir şekilde depolanır.

Ancak başlamadan önce, birkaç değer toplamanız veya bunun hakkında düşünmeniz gerekir.

1. Karşıya yüklemek istediğiniz dosyanın yerel dosya yolu
1. Varlığın (kapsayıcı) varlık KIMLIĞI
1. Uzantıyı içeren karşıya yüklenen dosya için kullanmak istediğiniz ad
1. Kullanmakta olduğunuz depolama hesabının adı
1. Depolama hesabınızın erişim anahtarı

## <a name="portal"></a>[Portal](#tab/portal/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-portal.md)]

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-cli.md)]

## <a name="rest"></a>[REST](#tab/rest/)

[Postman veya DIĞER Rest yöntemini kullanarak bir varlık oluşturduktan ve varlık IÇIN SAS URL 'sini](how-to-create-asset.md?tabs=rest)ayırdıktan sonra Azure depolama API 'Lerini veya SDK 'larını (örneğin, [depolama REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) kullanın.

---
<!-- add these to the tabs when available -->
Diğer yöntemler için, [.net](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet), [Java](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-java), [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python)ve [JavaScript (Node.js)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-nodejs)içindeki bloblarla çalışma için [Azure depolama belgelerine](https://docs.microsoft.com/azure/storage/blobs/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [Media Services genel bakış](media-services-overview.md)
