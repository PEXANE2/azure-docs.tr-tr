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
ms.openlocfilehash: c21a16d043f972042949d6340985774741b3df6a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74888624"
---
# <a name="cli-example-create-a-transform"></a>CLI örneği: Dönüşüm oluşturma

Bu makaledeki Azure CLI betiği, dönüşüm oluşturmayı gösterir. Dönüşümler video veya ses dosyalarınızın işlenmesine yönelik görevlerden oluşan basit bir iş akışı tanımlar (genellikle "tarif" olarak adlandırılır). Zaten istenen ada ve “tarife” sahip bir Dönüşümün olup olmadığını mutlaka kontrol etmelisiniz. Böyle bir tarif varsa bunu yeniden kullanmalısınız.

## <a name="prerequisites"></a>Ön koşullar 

[Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

> [!NOTE]
> Yalnızca [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset)için özel bir Standart Encoder önceden ayarlanmış JSON dosyasına bir yol belirtebilirsiniz, [özel bir dönüştürme](custom-preset-cli-howto.md) örneği ile kodlama bakın.
>
> [BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset)kullanırken bir dosya adını geçiremezsiniz.

## <a name="example-script"></a>Örnek betik

[!code-azurecli-interactive[main](../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="next-steps"></a>Sonraki adımlar

[az ams dönüşümü (CLI)](https://docs.microsoft.com/cli/azure/ams/transform?view=azure-cli-latest)
