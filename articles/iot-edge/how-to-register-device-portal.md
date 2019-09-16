---
title: Azure Portal - Azure IOT Edge yeni bir cihaz kaydetme | Microsoft Docs
description: Yeni bir IOT Edge cihazı kaydedin ve bağlantı dizesini almak için Azure portalını kullanma
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 7f3d0037bcf0fd33ae23c298679e3157046247cb
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70983536"
---
# <a name="register-a-new-azure-iot-edge-device-from-the-azure-portal"></a>Azure portalında yeni bir Azure IOT Edge cihazı kaydedin

IOT cihazlarınızı Azure IOT Edge ile kullanabilmeniz için önce bunları IOT hub'ınıza kaydetmeniz gerekir. Bir cihazı kaydettikten sonra, cihazınızı IoT Edge iş yükleri için ayarlamak üzere kullanılabilecek bir bağlantı dizesi alırsınız.

Bu makalede, Azure portalını kullanarak yeni bir IOT Edge cihazı kaydettirmek gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğinizde ücretsiz veya standart bir [IoT Hub 'ı](../iot-hub/iot-hub-create-through-portal.md) .

## <a name="create-a-device"></a>Cihaz oluşturma

Azure portalında, IOT Edge cihazları oluşturulur ve IOT hub'ınıza bağlanmak, ancak edge etkin olmayan cihazlardan ayrı olarak yönetilir.

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
2. Seçin **IOT Edge** menüsünde.
3. **IoT Edge cihaz ekle**' yi seçin.
4. Açıklayıcı bir cihaz kimliği sağlayın. Kimlik doğrulama anahtarlarını otomatik oluşturmak ve yeni cihazı hub 'ınıza bağlamak için varsayılan ayarları kullanın.
5. **Kaydet**’i seçin.

## <a name="view-all-devices"></a>Tüm cihazları görüntüle

IOT hub'ınıza bağlanan tüm edge özellikli cihazlar listelenir **IOT Edge** sayfası.

## <a name="retrieve-the-connection-string"></a>Bağlantı dizesi alma

Cihazınızı ayarlamak hazır olduğunuzda, fiziksel cihazınızın IOT hub'ında kimliği ile bağlanan bağlantı dizesine ihtiyacınız vardır.

1. Portaldaki **IoT Edge** sayfasında, IoT Edge cihazları LISTESINDEN cihaz kimliği ' ne tıklayın.
2. Ya da değeri kopyalayın **bağlantı dizesi (birincil anahtar)** veya **bağlantı dizesi (ikincil anahtarı)** .

## <a name="next-steps"></a>Sonraki adımlar

[Azure Portal bir cihaza modül dağıtmayı](how-to-deploy-modules-portal.md)öğrenin.
