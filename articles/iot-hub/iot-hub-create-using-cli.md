---
title: Azure CLI kullanarak Bir IoT Hub'ı oluşturma | Microsoft Dokümanlar
description: Kaynak grubu oluşturmak ve kaynak grubunda bir IoT hub'ı oluşturmak için Azure CLI komutlarını nasıl kullanacağınızı öğrenin. Ayrıca hub'ı nasıl kaldırılacağa da değinin.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/23/2018
ms.author: robinsh
ms.openlocfilehash: e7253747d95917e34725cc7c6c034b1306f65e50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284726"
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Azure CLI'yi kullanarak bir IoT hub'ı oluşturma

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Bu makalede, Azure CLI kullanarak nasıl bir IoT hub'ı oluşturabileceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını tamamlamak için bir Azure aboneliğine ihtiyacınız var. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="sign-in-and-set-your-azure-account"></a>Oturum açın ve Azure hesabınızı ayarlayın

Azure CLI'yi Cloud Shell kullanmak yerine yerel olarak çalıştırıyorsanız, Azure hesabınızda oturum açmanız gerekir.

Komut isteminde [oturum açma komutunu](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) çalıştırın:

   ```azurecli
   az login
   ```

Kodu kullanarak kimlik doğrulaması gerçekleştirmek için yönergeleri uygulayın ve bir web tarayıcısı üzerinden Azure hesabınızda oturum açın.

## <a name="create-an-iot-hub"></a>IoT Hub'ı oluşturma

Bir kaynak grubu oluşturmak ve ardından bir IoT hub'ı eklemek için Azure CLI'yi kullanın.

1. Bir IoT hub'ı oluşturduğunuzda, hub'ı bir kaynak grubunda oluşturmanız gerekir. Mevcut bir kaynak grubunu kullanın veya [kaynak grubu oluşturmak için aşağıdaki komutu](https://docs.microsoft.com/cli/azure/resource) çalıştırabilirsiniz:
    
   ```azurecli
   az group create --name {your resource group name} --location westus
   ```

   > [!TIP]
   > Önceki örnekte kaynak grubu Batı ABD konumunda oluşturulur. Bu komutu çalıştırarak kullanılabilir yerlerin listesini görüntüleyebilirsiniz: 
   >
   >``` bash
   >az account list-locations -o table
   >```
   >

2. Kaynak grubunuzda IoT hub'ınız için genel olarak benzersiz bir ad kullanarak [bir IoT hub'ı oluşturmak](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-create) için aşağıdaki komutu çalıştırın:
    
   ```azurecli
   az iot hub create --name {your iot hub name} \
      --resource-group {your resource group name} --sku S1
   ```

   [!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]


Önceki komut, faturalandırıldığın S1 fiyatlandırma katmanında bir IoT hub'ı oluşturur. Daha fazla bilgi için [Azure IoT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/)için bkz.

## <a name="remove-an-iot-hub"></a>IoT Hub'ı kaldırma

Azure CLI'yi, IoT hub'ı gibi [tek bir kaynağı silmek](https://docs.microsoft.com/cli/azure/resource)veya bir kaynak grubunu ve tüm kaynaklarını silmek için kullanabilirsiniz.

[Bir IoT hub'ını silmek](https://docs.microsoft.com/cli/azure/iot/hub#az-iot-hub-delete)için aşağıdaki komutu çalıştırın:

```azurecli
az iot hub delete --name {your iot hub name} -\
  -resource-group {your resource group name}
```

Bir kaynak grubunu ve tüm kaynaklarını [silmek](https://docs.microsoft.com/cli/azure/group#az-group-delete) için aşağıdaki komutu çalıştırın:

```azurecli
az group delete --name {your resource group name}
```

## <a name="next-steps"></a>Sonraki adımlar

Bir IoT hub'ı kullanma hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [IoT Hub'ı yönetmek için Azure portalını kullanma](iot-hub-create-through-portal.md)
