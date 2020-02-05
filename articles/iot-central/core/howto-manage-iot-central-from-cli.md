---
title: Azure CLı 'dan IoT Central yönetme | Microsoft Docs
description: Bu makalede, CLı kullanarak IoT Central uygulamanızın nasıl oluşturulacağı ve yönetileceği açıklanmaktadır. CLı kullanarak uygulamayı görüntüleyebilir, değiştirebilir ve kaldırabilirsiniz.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 8526eb50faf300892c66ac186eac25adecf62231
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019035"
---
# <a name="manage-iot-central-from-azure-cli"></a>Azure CLı 'dan IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamaları oluşturup yönetmek yerine uygulamalarınızı yönetmek IÇIN [Azure CLI](/cli/azure/) kullanabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Azure CLı 'yi yerel makinenizde çalıştırmayı tercih ediyorsanız bkz. [Azure CLI 'Yı yüklemek](/cli/azure/install-azure-cli). Azure CLı 'yi yerel olarak çalıştırdığınızda, bu makaledeki komutları denemeden önce Azure 'da oturum açmak için **az Login** komutunu kullanın.

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
  --sku ST1 --template "iotc-demo@1.0.0" \
  --display-name "My Custom Display Name"
```

Bu komutlar öncelikle uygulamanın Doğu ABD bölgesinde bir kaynak grubu oluşturur. Aşağıdaki tabloda **az iotcentral App Create** komutuyla kullanılan parametreler açıklanmaktadır:

| Parametre         | Açıklama |
| ----------------- | ----------- |
| resource-group    | Uygulamayı içeren kaynak grubu. Bu kaynak grubu aboneliğinizde zaten var olmalıdır. |
| location          | Varsayılan olarak, bu komut kaynak grubundaki konumu kullanır. Şu anda **Doğu ABD**, **Batı ABD**, **Kuzey Avrupa**veya **Batı Avrupa** bölgelerinde veya **Avustralya** ya da **Asya Pasifik** coğrafi bölgelerde IoT Central bir uygulama oluşturabilirsiniz. |
| ad              | Azure portal uygulamanın adı. |
| alanınızın         | Uygulamanın URL 'sindeki alt etki alanı. Örnekte, uygulama URL 'SI https://mysubdomain.azureiotcentral.com. |
| isteyin               | Şu anda, **ST1** ya da **ST2**kullanabilirsiniz. Bkz. [Azure IoT Central fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-central/). |
| şablon          | Kullanılacak uygulama şablonu. Daha fazla bilgi için aşağıdaki tabloya bakın: |
| görünen ad      | Kullanıcı arabiriminde gösterildiği şekilde uygulamanın adı. |

**Uygulama şablonları**

| Şablon adı            | Açıklama |
| ------------------------ | ----------- |
| iotc-default@1.0.0       | Kendi cihaz şablonlarınız ve cihazlarınızla doldurabileceğiniz boş bir uygulama oluşturur.
| iotc-pnp-preview@1.0.0   | Kendi cihaz şablonlarınızla ve cihazlarınızla doldurmanız için boş bir Tak ve Kullan (Önizleme) uygulaması oluşturur. |
| iotc-condition@1.0.0     | Store Analytics – Condition izleme şablonuyla bir uygulama oluşturur. Mağaza ortamına bağlanmak ve bunları izlemek için bu şablonu kullanın. |
| iotc-consumption@1.0.0   | Su tüketim izleme şablonuyla bir uygulama oluşturur. Su akışını izlemek ve denetlemek için bu şablonu kullanın. |
| iotc-distribution@1.0.0  | Dijital dağıtım şablonu olan bir uygulama oluşturur. Ana varlıkları ve eylemleri artırarak ambar çıkış verimliliğini artırmak için bu şablonu kullanın. |
| iotc-inventory@1.0.0     | Akıllı envanter yönetimi şablonu ile bir uygulama oluşturur. Bu şablonu, almayı, ürün hareketini, dönem döngüsünü ve sensörlerin izlenmesini otomatik hale getirmek için kullanın. |
| iotc-logistics@1.0.0     | Bağlı bir lojistik şablonuyla bir uygulama oluşturur. Bu şablonu, Sevkiyat, su ve konum izlemeye sahip uçak genelinde gerçek zamanlı olarak sevk irsaliyesini izlemek için kullanın. |
| iotc-meter@1.0.0         | Akıllı ölçüm izleme şablonuyla bir uygulama oluşturur. Enerji tüketimini, ağ durumunu izlemek ve müşteri desteğini ve akıllı ölçüm yönetimini geliştirmenin eğilimlerini belirlemek için bu şablonu kullanın.  |
| iotc-patient@1.0.0       | Sürekli hasta izleme şablonuyla bir uygulama oluşturur. Hasta bakımı, yeniden Admissions ve daha fazla bilgi için bu şablonu kullanın. |
| iotc-power@1.0.0         | Solar paneli izleme şablonuyla bir uygulama oluşturur. Bu şablonu, güneş paneli durumunu ve enerji oluşturma eğilimlerini izlemek için kullanın. |
| iotc-quality@1.0.0       | Su kalitesi izleme şablonuyla bir uygulama oluşturur. Su kalitesini dijital olarak izlemek için bu şablonu kullanın.|
| iotc-store@1.0.0         | Mağaza Analizi – kullanıma alma şablonu olan bir uygulama oluşturur. Mağazalarınızın içindeki kullanıma alma akışını izlemek ve yönetmek için bu şablonu kullanın. |
| iotc-waste@1.0.0         | Bağlı bir çöp yönetimi şablonuna sahip bir uygulama oluşturur. Atık bölmeleri ve dağıtım alanı işleçlerini izlemek için bu şablonu kullanın. |

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
