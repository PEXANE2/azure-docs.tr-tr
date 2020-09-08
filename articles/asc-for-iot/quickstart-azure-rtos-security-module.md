---
title: 'Hızlı başlangıç: Azure RTOS için güvenlik modülünü yapılandırma ve etkinleştirme'
description: Azure IoT Hub Azure RTOS hizmetine yönelik güvenlik modülünü ekleme ve etkinleştirme hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2020
ms.author: mlottner
ms.openlocfilehash: 116361977177b708265232a917e7f31e5f8cd09a
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89515176"
---
# <a name="quickstart-security-module-for-azure-rtos-preview"></a>Hızlı başlangıç: Azure RTOS için güvenlik modülü (Önizleme)

Bu makalede, başlamadan önce önkoşulların açıklaması sağlanmaktadır ve bir IoT Hub Azure RTOS hizmeti için güvenlik modülünün nasıl etkinleştirileceği açıklanır. Şu anda bir IoT Hub yoksa, kullanmaya başlamak için [Azure Portal kullanarak IoT Hub oluşturma](https://docs.microsoft.com/azure/iot-hub/iot-hub-create-through-portal) makalesine bakın.

> [!NOTE]
> Azure RTOS için güvenlik modülü yalnızca Standart katman IoT Hub 'Larında desteklenir.

## <a name="prerequisites"></a>Önkoşullar 

### <a name="supported-devices"></a>Desteklenen cihazlar

- ST STM32F746G bulma seti
- NXP i.MX RT1060 EVK
- Mikro yonga SAM E54 Xplaj Pro EVK

[Azure RTOS GitHub kaynağı Için güvenlik modülündeki](hhtps://github.com/azure-rtos/azure-iot-preview/releases)belirli bir Pano ve araç (IAR, yarı IDE veya bilgisayar) için. zip dosyalarından birini indirin, derleyin ve çalıştırın.

### <a name="azure-resources"></a>Azure kaynakları

Kullanmaya başlamak için sonraki aşama, Azure kaynaklarınızı hazırlıyor. Bir IoT Hub gerekir ve bir Log Analytics çalışma alanı öneririz. IoT Hub için cihazınıza bağlanmak üzere IoT Hub bağlantı dizeniz olması gerekir. 
  
### <a name="iot-hub-connection"></a>IoT Hub bağlantısı

Başlamak için bir IoT Hub bağlantısı gerekir. 

1. **IoT Hub** Azure Portal açın.
1. IoT bağlantı dizesini [yapılandırma dosyasına](how-to-azure-rtos-security-module.md)kopyalayın.


Bağlantı kimlik bilgileri, Kullanıcı uygulama yapılandırmasından **host_name**, **DEVICE_ID**ve **DEVICE_SYMMETRIC_KEY**alınır.

Azure RTOS için güvenlik modülü, **MQTT** protokolüne göre Azure IoT ara yazılım bağlantılarını kullanır.


### <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı

IoT Hub Log Analytics alma, IoT çözümü için Azure Güvenlik Merkezi varsayılan olarak kapalıdır. Azure RTOS için güvenlik modülü ile çalışmaya olanak tanımak üzere şunları yapın: 
1. Azure portal IoT Hub gidin.
1. **Güvenlik** menüsündeki **Ayarlar** ' ı seçin.
   :::image type="content" source="media/quickstart/azure-rtos-hub-settings.png" alt-text="Azure RTOS için veri toplama seçeneğine erişme"::: 
1. **Veri toplamayı**seçin. 
1. **Çalışma alanı yapılandırma** seçeneğinde, geçişi **Açık**olarak değiştirin. 
1. Yeni bir Log Analytics çalışma alanı oluşturun veya var olan bir çalışma alanını ekleyin. **Ham güvenlik verilerine erişim** seçeneğinin seçildiğinden emin olun. 
 :::image type="content" source="media/quickstart/azure-rtos-data-collection-on.png" alt-text="Veri toplama seçeneğini gösteren Azure RTOS yapılandırması ve her ikisi de seçili olan RAW güvenlik verileri seçenekleri":::
1. **Kaydet**’i seçin
1. Azure kaynakları listenize dönün ve oluşturduğunuz Log Analytics çalışma alanının IoT Hub için etkinleştirildiğini doğrulayın.
    :::image type="content" source="media/quickstart/verify-azure-resource-list.png" alt-text="IoT Hub için eklenen doğru Log Analytics çalışma alanının eklenmesini onaylamak için Azure Kaynak listenizi denetleyin"::: 

## <a name="next-steps"></a>Sonraki adımlar

Çözümünüzü yapılandırmayı ve özelleştirmeyi tamamlamak için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure RTOS için güvenlik modülünü yapılandırma](how-to-azure-rtos-security-module.md)
