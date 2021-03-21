---
title: Azure CLı betik örneği-dönüşüm oluşturma
description: Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır). Bu makaledeki Azure CLI betiği, dönüşüm oluşturmayı gösterir.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: multiple
ms.topic: how-to
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/18/2020
ms.author: inhenkel
ms.custom: devx-track-azurecli
ms.openlocfilehash: 4623610960d8f21a2dab3293c7499a2112416254
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "101718922"
---
# <a name="create-a-transform"></a>Dönüşüm oluşturma

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Bu makaledeki Azure CLI betiği, dönüşüm oluşturmayı gösterir. Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır). Zaten istenen ada ve “tarife” sahip bir Dönüşümün olup olmadığını mutlaka kontrol etmelisiniz. Böyle bir tarif varsa bunu yeniden kullanmalısınız.

## <a name="prerequisites"></a>Önkoşullar

[Media Services hesabı oluşturun](./create-account-howto.md).

## <a name="cli"></a>[CLI](#tab/cli/)

> [!NOTE]
> [Standardencoderönayar](/rest/api/media/transforms/createorupdate#standardencoderpreset)için yalnızca özel standart kodlayıcı önceden ayarlanmış JSON dosyası için bir yol belirtebilirsiniz, özel bir dönüşüm örneği [ile](custom-preset-cli-howto.md) kodlamaya bakın.
>
> [Builtınstandardencoderönayar](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset)kullanılırken bir dosya adı geçirilemez.

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="rest"></a>[REST](#tab/rest/)

[!INCLUDE [task general transform creation](./includes/task-create-basic-audio-rest.md)]

---

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [transforms next steps](./includes/transforms-next-steps.md)]
