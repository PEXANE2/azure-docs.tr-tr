---
title: Azure CLI Betik Örneği - Dönüşüm oluşturma | Microsoft Docs
description: Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır). Bu makaledeki Azure CLI betiği, dönüşüm oluşturmayı gösterir.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/01/2019
ms.author: juliako
ms.custom: devx-track-azurecli
ms.openlocfilehash: 538f48d427a4d8b51f77ae50bb0bee95909f0b09
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87494477"
---
# <a name="cli-example-create-a-transform"></a>CLI örneği: Dönüşüm oluşturma

Bu makaledeki Azure CLI betiği, dönüşüm oluşturmayı gösterir. Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır). Zaten istenen ada ve “tarife” sahip bir Dönüşümün olup olmadığını mutlaka kontrol etmelisiniz. Böyle bir tarif varsa bunu yeniden kullanmalısınız.

## <a name="prerequisites"></a>Ön koşullar 

[Media Services hesabı oluşturun](./create-account-howto.md).

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

> [!NOTE]
> [Standardencoderönayar](/rest/api/media/transforms/createorupdate#standardencoderpreset)için yalnızca özel standart kodlayıcı önceden ayarlanmış JSON dosyası için bir yol belirtebilirsiniz, özel bir dönüşüm örneği [ile](custom-preset-cli-howto.md) kodlamaya bakın.
>
> [Builtınstandardencoderönayar](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset)kullanılırken bir dosya adı geçirilemez.

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="next-steps"></a>Sonraki adımlar

[az AMS Transform (CLı)](/cli/azure/ams/transform?view=azure-cli-latest)
