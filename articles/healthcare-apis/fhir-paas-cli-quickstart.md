---
title: 'Hızlı başlangıç: Azure CLı kullanarak FHıR için Azure API dağıtma'
description: Bu hızlı başlangıçta, Azure 'da Azure CLı 'yı kullanarak Azure 'da FHıR için Azure API 'SI dağıtmayı öğreneceksiniz.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: mihansen
ms.custom: devx-track-azurecli
ms.openlocfilehash: e2fdd4c4298946d943ee8a806ebc53966c8dd624
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87500904"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak FHıR için Azure API dağıtma

Bu hızlı başlangıçta, Azure 'da Azure CLı 'yı kullanarak Azure 'da FHıR için Azure API 'SI dağıtmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="add-healthcareapis-extension"></a>Healthgelişme API 'leri uzantısı Ekle

```azurecli-interactive
az extension add --name healthcareapis
```

Healthumara API komutlarının bir listesini alın:

```azurecli-interactive
az healthcareapis --help
```

## <a name="create-azure-resource-group"></a>Azure Kaynak grubu oluştur

FHıR için Azure API 'sini içerecek kaynak grubu için bir ad seçin ve oluşturun:

```azurecli-interactive
az group create --name "myResourceGroup" --location westus2
```

## <a name="deploy-the-azure-api-for-fhir"></a>FHıR için Azure API 'sini dağıtma

```azurecli-interactive
az healthcareapis create --resource-group myResourceGroup --name nameoffhiraccount --kind fhir-r4 --location westus2 
```

## <a name="fetch-fhir-api-capability-statement"></a>FHıR API özelliği bildirisini getir

FHıR API 'sinden ile bir yetenek beyanı edinin:

```azurecli-interactive
curl --url "https://nameoffhiraccount.azurehealthcareapis.com/metadata"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekecekseniz, kaynak grubunu aşağıdaki adımlarla silin:

```azurecli-interactive
az group delete --name "myResourceGroup"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, FHıR için Azure API 'sini aboneliğinize dağıttınız. FHıR için Azure API 'niz üzerinde ek ayarlar ayarlamak için, ek ayarlar nasıl yapılır Kılavuzu ' na ilerleyin.

>[!div class="nextstepaction"]
>[FHıR için Azure API 'de ek ayarlar](azure-api-for-fhir-additional-settings.md)
