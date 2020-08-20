---
title: Medyayı karşıya yükle
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
ms.date: 08/11/2020
ms.author: inhenkel
ms.openlocfilehash: 2ff5b7832b894e218cf65447378bfe6fd3e42ca3
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88658467"
---
# <a name="upload-media-for-streaming-or-encoding"></a>Akış veya kodlama için medya yükleme
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

## <a name="cli-shell"></a>[CLı kabuğu](#tab/clishell/)

[!INCLUDE [Upload files with the portal](./includes/task-upload-file-to-asset-cli-shell.md)]

## <a name="rest"></a>[REST](#tab/rest/)

[Postman veya DIĞER Rest yöntemini kullanarak bir varlık oluşturduktan ve varlık IÇIN sus URL 'sini](how-to-create-asset.md?tabs=rest)edindikten sonra Azure depolama API 'Lerini veya SDK 'larını (örneğin, [depolama REST API](../../storage/common/storage-rest-api-auth.md) veya [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) kullanın.

---
<!-- add these to the tabs when available -->
Diğer yöntemler için, [.net](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet), [Java](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-java), [Python](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-python)ve [JavaScript (Node.js)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-nodejs)içindeki bloblarla çalışma için [Azure depolama belgelerine](https://docs.microsoft.com/azure/storage/blobs/) bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [Media Services genel bakış](media-services-overview.md)
