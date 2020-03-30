---
title: Azure CLI'den IoT Central'ı yönetme | Microsoft Dokümanlar
description: Bu makalede, CLI kullanarak IoT Merkezi uygulamanızın nasıl oluşturulup yönetilen anlatılmaktadır. CLI kullanarak uygulamayı görüntüleyebilir, değiştirebilir ve kaldırabilirsiniz.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 03/27/2020
ms.topic: how-to
manager: philmea
ms.openlocfilehash: df24a2dc6e9bd058a2f8b1355b8760653ed3128a
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80365526"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLI'den IoT Central'ı yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamalarını oluşturmak ve yönetmek yerine, uygulamalarınızı yönetmek için Azure [CLI'yi](/cli/azure/) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel makinenizde çalıştırmayı tercih ediyorsanız, [bkz.](/cli/azure/install-azure-cli) Azure CLI'yi yerel olarak çalıştırdığınızda, bu makaledeki komutları denemeden önce Azure'da oturum açmak için **az giriş** komutunu kullanın.

> [!TIP]
> CLI komutlarınızı farklı bir Azure aboneliğinde çalıştırmanız gerekiyorsa, [bkz.](/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#change-the-active-subscription)

## <a name="install-the-extension"></a>Uzantıyı yükleme

Bu makaledeki komutlar **azure-iot** CLI uzantısının bir parçasıdır. Uzantıyı yüklemek için aşağıdaki komutu çalıştırın:

```azurecli-interactive
az extension add --name azure-iot
```

## <a name="create-an-application"></a>Uygulama oluşturma

Azure aboneliğinizde bir IoT Central uygulaması oluşturmak için [az iotcentral uygulamasını oluşturun](/cli/azure/iotcentral/app#az-iotcentral-app-create) komutunu kullanın. Örnek:

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

Bu komutlar ilk uygulama için doğu ABD bölgesinde bir kaynak grubu oluşturun. Aşağıdaki tabloda **az iotcentral uygulaması oluşturma** komutu ile kullanılan parametreler açıklanmaktadır:

| Parametre         | Açıklama |
| ----------------- | ----------- |
| resource-group    | Uygulamayı içeren kaynak grubu. Bu kaynak grubunun aboneliğinizde zaten bulunması gerekir. |
| location          | Varsayılan olarak, bu komut kaynak grubundaki konumu kullanır. Şu anda, **Avustralya,** **Asya Pasifik,** **Avrupa,** **Amerika Birleşik Devletleri,** **İngiltere**ve **Japonya** coğrafyalarında bir IoT Merkezi uygulama oluşturabilirsiniz. |
| ad              | Azure portalındaki uygulamanın adı. |
| Alt         | Uygulamanın URL'sindeki alt etki alanı. Örnekte, uygulama URL'si. `https://mysubdomain.azureiotcentral.com` |
| Sku               | Şu anda, **ST1** veya **ST2**kullanabilirsiniz. Bkz. [Azure IoT Merkezi fiyatlandırması.](https://azure.microsoft.com/pricing/details/iot-central/) |
| şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın. |
| görüntü adı      | Kullanıcı Arabirimi'nde görüntülenen uygulamanın adı. |

[!INCLUDE [iot-central-template-list](../../../includes/iot-central-template-list.md)]

## <a name="view-your-applications"></a>Uygulamalarınızı görüntüleyin

IoT Central uygulamalarınızı listelemek ve meta verileri görüntülemek için [az iotcentral uygulama listesi](/cli/azure/iotcentral/app#az-iotcentral-app-list) komutunu kullanın.

## <a name="modify-an-application"></a>Uygulamayı değiştirme

Bir IoT Central uygulamasının meta verilerini güncelleştirmek için [az iotcentral uygulama güncelleştirme](/cli/azure/iotcentral/app#az-iotcentral-app-update) komutunu kullanın. Örneğin, uygulamanızın görüntü adını değiştirmek için:

```azurecli-interactive
az iotcentral app update --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup \
  --set displayName="My new display name"
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Bir IoT Central uygulamasını silmek için [az iotcentral app delete](/cli/azure/iotcentral/app#az-iotcentral-app-delete) komutunu kullanın. Örnek:

```azurecli-interactive
az iotcentral app delete --name myiotcentralapp \
  --resource-group MyIoTCentralResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Azure CLI'den Azure IoT Merkezi uygulamalarını nasıl yöneteceğimize dair öğrendiğiniz egöre, önerilen bir sonraki adım şudur:

> [!div class="nextstepaction"]
> [Uygulamanızı yönetme](howto-administer.md)
