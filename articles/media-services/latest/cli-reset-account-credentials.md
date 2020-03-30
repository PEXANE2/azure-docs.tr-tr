---
title: Azure CLI Betik Örneği - Hesabınızın kimlik bilgilerini sıfırlama | Microsoft Docs
description: Azure CLI betiğini kullanarak hesabınızın kimlik bilgilerini sıfırlayın ve app.config ayarlarını geri alın.
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
ms.date: 08/20/2019
ms.author: juliako
ms.openlocfilehash: 63f2abe7a3890efbaf4c79186467a3eb20d8afda
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382996"
---
# <a name="azure-cli-example-reset-the-account-credentials"></a>Azure CLI örneği: Hesap kimlik bilgilerini sıfırlama

Bu makaledeki Azure CLI betiği, hesabınızın kimlik bilgilerini sıfırlamayı ve app.config ayarlarını geri almayı gösterir.

## <a name="prerequisites"></a>Ön koşullar

[Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)

[!INCLUDE [media-services-cli-instructions.md](../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Örnek betik

```azurecli-interactive
# Update the following variables for your own settings:
resourceGroup=amsResourceGroup
amsAccountName=amsmediaaccountname

az ams account sp reset-credentials \
  --account-name $amsAccountName \
  --resource-group $resourceGroup
 ```

## <a name="next-steps"></a>Sonraki adımlar

* [az ams](/cli/azure/ams)
* [Kimlik bilgilerini sıfırlama](/cli/azure/ams/account/sp#az-ams-account-sp-reset-credentials)
