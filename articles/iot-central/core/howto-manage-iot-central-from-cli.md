---
title: Azure CLı 'dan IoT Central yönetme | Microsoft Docs
description: Bu makalede, CLı kullanarak IoT Central uygulamanızın nasıl oluşturulacağı ve yönetileceği açıklanmaktadır. CLı kullanarak uygulamayı görüntüleyebilir, değiştirebilir ve kaldırabilirsiniz.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/27/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: c3e0beb9a35b7888787d018b5e3b9c4ceea7e1c9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82744555"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLı 'dan IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamaları oluşturup yönetmek yerine uygulamalarınızı yönetmek IÇIN [Azure CLI](/cli/azure/) kullanabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLı 'yi yerel makinenizde çalıştırmayı tercih ediyorsanız bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli). Azure CLı 'yi yerel olarak çalıştırdığınızda, bu makaledeki komutları denemeden önce Azure 'da oturum açmak için **az Login** komutunu kullanın.

> [!TIP]
> CLı komutlarınızı farklı bir Azure aboneliğinde çalıştırmanız gerekiyorsa bkz. [etkin aboneliği değiştirme](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#change-the-active-subscription).

## <a name="install-the-extension"></a>Uzantıyı yükleme

Bu makaledeki komutlar, **Azure-IoT** CLI uzantısının bir parçasıdır. Uzantıyı yüklemek için şu komutu çalıştırın:

```azurecli-interactive
az extension add --name azure-iot
```

## <a name="create-an-application"></a>Uygulama oluşturma

Azure aboneliğinizde bir IoT Central uygulaması oluşturmak için [az IoT Central App Create](/cli/azure/iot/central/app?view=azure-cli-latest#az-iot-central-app-create) komutunu kullanın. Örnek:

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iot central app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku ST1 --template "iotc-pnp-preview" \
  --display-name "My Custom Display Name"
```

Bu komutlar öncelikle uygulamanın Doğu ABD bölgesinde bir kaynak grubu oluşturur. Aşağıdaki tabloda, **az IoT Central App Create** komutuyla kullanılan parametreler açıklanmaktadır:

| Parametre         | Açıklama |
| ----------------- | ----------- |
| resource-group    | Uygulamayı içeren kaynak grubu. Bu kaynak grubu aboneliğinizde zaten var olmalıdır. |
| location          | Varsayılan olarak, bu komut kaynak grubundaki konumu kullanır. Şu anda **Avustralya**, **Asya Pasifik**, **Avrupa**, **Birleşik Devletler**, **Birleşik Krallık**ve **Japonya** coğrafi graflarını IoT Central bir uygulama oluşturabilirsiniz. |
| name              | Azure portal uygulamanın adı. |
| alanınızın         | Uygulamanın URL 'sindeki alt etki alanı. Örnekte, uygulama URL 'SI `https://mysubdomain.azureiotcentral.com` . |
| isteyin               | Şu anda, **ST1** ya da **ST2**kullanabilirsiniz. Bkz. [Azure IoT Central fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-central/). |
| şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın. |
| görünen ad      | Kullanıcı arabiriminde gösterildiği şekilde uygulamanın adı. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-applications"></a>Uygulamalarınızı görüntüleyin

IoT Central uygulamalarınızı listelemek ve meta verileri görüntülemek için [az IoT Central App List](/cli/azure/iot/central/app?view=azure-cli-latest#az-iot-central-app-list) komutunu kullanın.

## <a name="modify-an-application"></a>Bir uygulamayı değiştirme

Bir IoT Central uygulamasının meta verilerini güncelleştirmek için [az IoT Central App Update](/cli/azure/iot/central/app?view=azure-cli-latest#az-iot-central-app-update) komutunu kullanın. Örneğin, uygulamanızın görünen adını değiştirmek için:

```azurecli-interactive
az iot central app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Bir IoT Central uygulamasını silmek için [az IoT Central App Delete](/cli/azure/iot/central/app?view=azure-cli-latest#az-iot-central-app-delete) komutunu kullanın. Örnek:

```azurecli-interactive
az iot central app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamalarını Azure CLı 'dan nasıl yönetebileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
