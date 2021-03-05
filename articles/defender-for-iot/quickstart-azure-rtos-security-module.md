---
title: 'Hızlı başlangıç: Azure RTOS için güvenlik modülünü yapılandırma ve etkinleştirme'
description: Azure IoT Hub Azure RTOS hizmetine yönelik güvenlik modülünü ekleme ve etkinleştirme hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: e8e9eae576c330f17cd33d027152d463fe19a82b
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102123912"
---
# <a name="quickstart-security-module-for-azure-rtos"></a>Hızlı başlangıç: Azure RTOS için güvenlik modülü 

Bu makalede, başlamadan önce önkoşulların açıklaması sağlanmaktadır ve bir IoT Hub Azure RTOS hizmeti için güvenlik modülünün nasıl etkinleştirileceği açıklanır. Şu anda bir IoT Hub yoksa, kullanmaya başlamak için [Azure Portal kullanarak IoT Hub oluşturma](../iot-hub/iot-hub-create-through-portal.md) makalesine bakın.

## <a name="prerequisites"></a>Önkoşullar 

### <a name="supported-devices"></a>Desteklenen cihazlar

- ST STM32F746G bulma seti
- NXP i.MX RT1060 EVK
- Mikro yonga SAM E54 Xplaj Pro EVK

[Azure RTOS GitHub kaynağı Için güvenlik modülündeki](https://github.com/azure-rtos/azure-iot-preview/releases)belirli bir Pano ve araç (IAR, yarı IDE veya bilgisayar) için. zip dosyalarından birini indirin, derleyin ve çalıştırın.

### <a name="azure-resources"></a>Azure kaynakları

Kullanmaya başlamak için sonraki aşama, Azure kaynaklarınızı hazırlıyor. Bir IoT Hub gerekir ve bir Log Analytics çalışma alanı öneririz. IoT Hub için cihazınıza bağlanmak üzere IoT Hub bağlantı dizeniz olması gerekir. 
  
### <a name="iot-hub-connection"></a>IoT Hub bağlantısı

Başlamak için bir IoT Hub bağlantısı gerekir. 

1. **IoT Hub** Azure Portal açın.

1. **IoT cihazlarına** gidin.

1. **Oluştur**’u seçin.

1. IoT bağlantı dizesini [yapılandırma dosyasına](how-to-azure-rtos-security-module.md)kopyalayın.

Bağlantı kimlik bilgileri, Kullanıcı uygulama yapılandırmasından **host_name**, **DEVICE_ID** ve **DEVICE_SYMMETRIC_KEY** alınır.

Azure RTOS için güvenlik modülü, **MQTT** protokolüne göre Azure IoT ara yazılım bağlantılarını kullanır.

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırmayı ve özelleştirmeyi tamamlamak için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure RTOS için Güvenlik Modülü’nü yapılandırma](how-to-azure-rtos-security-module.md)