---
title: 'Hızlı başlangıç: Azure CLı kullanarak FHıR için Azure API dağıtma'
description: Bu hızlı başlangıçta, Azure 'da Azure CLı 'yı kullanarak Azure 'da FHıR için Azure API 'SI dağıtmayı öğreneceksiniz.
services: healthcare-apis
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: cavoeg
ms.custom: devx-track-azurecli
ms.openlocfilehash: 3340cd0a39b0f5311487fec5b05d37e3376d433a
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94659261"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak FHıR için Azure API dağıtma

Bu hızlı başlangıçta, Azure 'da Azure CLı 'yı kullanarak Azure 'da FHıR için Azure API 'SI dağıtmayı öğreneceksiniz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../includes/azure-cli-prepare-your-environment.md)]

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

Bu hızlı başlangıç kılavuzunda, FHıR için Azure API 'sini aboneliğinize dağıttınız. FHıR için Azure API 'niz üzerinde ek ayarlar ayarlamak için, ek ayarlar nasıl yapılır Kılavuzu ' na ilerleyin. FHıR için Azure API 'yi kullanmaya başlamaya hazırsanız, uygulamaları kaydetme hakkında daha fazla bilgi edinin.

>[!div class="nextstepaction"]
>[FHıR için Azure API 'de ek ayarlar](azure-api-for-fhir-additional-settings.md)

>[!div class="nextstepaction"]
>[Uygulamaları kaydetme genel bakış](fhir-app-registration.md)
