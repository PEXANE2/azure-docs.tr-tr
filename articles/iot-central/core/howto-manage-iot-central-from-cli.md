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
ms.openlocfilehash: df24a2dc6e9bd058a2f8b1355b8760653ed3128a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80365526"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLı 'dan IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamaları oluşturup yönetmek yerine uygulamalarınızı yönetmek IÇIN [Azure CLI](/cli/azure/) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

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

Azure aboneliğinizde bir IoT Central uygulaması oluşturmak için [az ıotcentral App Create](/cli/azure/iotcentral/app#az-iotcentral-app-create) komutunu kullanın. Örneğin:

```azurecli-interactive
# Create a resource group for the IoT Central application
az group create --location "East US" \
    --name "MyIoTCentralResourceGroup"
```

```azurecli-interactive
# Create an IoT Central application
az iotcentral app create \
  --resource-group "MyIoTCentralResourceGroup" \
  --name "myiotcentralapp" --subdomain "mysubdomain" \
  --sku ST1 --template "iotc-pnp-preview@1.0.0" \
  --display-name "My Custom Display Name"
```

Bu komutlar öncelikle uygulamanın Doğu ABD bölgesinde bir kaynak grubu oluşturur. Aşağıdaki tabloda **az iotcentral App Create** komutuyla kullanılan parametreler açıklanmaktadır:

| Parametre         | Açıklama |
| ----------------- | ----------- |
| resource-group    | Uygulamayı içeren kaynak grubu. Bu kaynak grubu aboneliğinizde zaten var olmalıdır. |
| location          | Varsayılan olarak, bu komut kaynak grubundaki konumu kullanır. Şu anda **Avustralya**, **Asya Pasifik**, **Avrupa**, **Birleşik Devletler**, **Birleşik Krallık**ve **Japonya** coğrafi graflarını IoT Central bir uygulama oluşturabilirsiniz. |
| ad              | Azure portal uygulamanın adı. |
| alanınızın         | Uygulamanın URL 'sindeki alt etki alanı. Örnekte, uygulama URL 'SI `https://mysubdomain.azureiotcentral.com`. |
| isteyin               | Şu anda, **ST1** ya da **ST2**kullanabilirsiniz. Bkz. [Azure IoT Central fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-central/). |
| şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın. |
| görünen ad      | Kullanıcı arabiriminde gösterildiği şekilde uygulamanın adı. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-applications"></a>Uygulamalarınızı görüntüleyin

IoT Central uygulamalarınızı listelemek ve meta verileri görüntülemek için [az ıotcentral App List](/cli/azure/iotcentral/app#az-iotcentral-app-list) komutunu kullanın.

## <a name="modify-an-application"></a>Bir uygulamayı değiştirme

Bir IoT Central uygulamasının meta verilerini güncelleştirmek için [az ıotcentral App Update](/cli/azure/iotcentral/app#az-iotcentral-app-update) komutunu kullanın. Örneğin, uygulamanızın görünen adını değiştirmek için:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Bir IoT Central uygulamasını silmek için [az ıotcentral App Delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete) komutunu kullanın. Örneğin:

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamalarını Azure CLı 'dan nasıl yönetebileceğinizi öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
