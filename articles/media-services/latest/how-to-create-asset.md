---
title: Azure CLı kullanarak Azure Media Services varlığa içerik yükleme
description: Bu konudaki Azure CLI betiği, içine içerik yüklenebilecek bir Media Services Varlığı oluşturmayı gösterir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/18/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4d1e5e48e70cee1cf75b1d6605837695d26dcbf0
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608903"
---
# <a name="create-an-asset"></a>Asset oluşturma

Bu makalede, Media Services varlığın nasıl oluşturulacağı gösterilmektedir.  Kodlama ve akış için medya içeriğini tutmak üzere bir varlık kullanacaksınız.  Media Services varlıklar hakkında daha fazla bilgi edinmek için [Azure Media Services v3 Içindeki varlıkları](assets-concept.md) okuyun

## <a name="prerequisites"></a>Önkoşullar

Bir varlık oluşturmak için gerekli Media Services hesabı ve kaynak grubunu oluşturmak üzere [Media Services hesabı oluşturma](./create-account-howto.md) bölümündeki adımları izleyin.

## <a name="methods"></a>Yöntemler

## <a name="cli"></a>[CLI](#tab/cli/)

[!INCLUDE [Create an asset with CLI](./includes/task-create-asset-cli.md)]

## <a name="cli-shell"></a>[CLı kabuğu](#tab/clishell/)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-asset/Create-Asset.sh "Create an asset")]

## <a name="rest"></a>[REST](#tab/rest/)

### <a name="using-rest"></a>REST kullanma

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-rest.md)]

### <a name="using-curl"></a>cURL'yi kullanma

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-curl.md)]

## <a name="net"></a>[.NET](#tab/net/)

[!INCLUDE [media-services-cli-instructions.md](./includes/task-create-asset-dotnet.md)]

---

## <a name="next-steps"></a>Sonraki adımlar

[Media Services genel bakış](media-services-overview.md)
