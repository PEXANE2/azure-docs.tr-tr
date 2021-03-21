---
title: Bir Defender IoT mikro aracı modülü oluşturma ikizi (Önizleme)
titleSuffix: Azure Defender for IoT
description: Yeni cihazlar için bireysel savunma, her bir savunma Aracısı modül TWINS oluşturmayı öğrenin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/20/2021
ms.topic: quickstart
ms.service: azure
ms.openlocfilehash: e1c2f4e79b9f0f0ba1ec0da0c5a2a29cdc8bc6d7
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103489888"
---
# <a name="create-a-defender-iot-micro-agent-module-twin-preview"></a>Bir Defender IoT mikro aracı modülü oluşturma ikizi (Önizleme)

Yeni cihazlar için bireysel **savunma Deriotmikro aracı** modülü TWINS oluşturabilirsiniz. Ayrıca, bir IoT Hub tüm cihazlar için modül TWINS 'i toplu olarak oluşturabilirsiniz. 

## <a name="device-twins"></a>Cihaz ikikesi 

Azure 'da oluşturulan IoT çözümleri için, cihaz ikis, hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar. 

IoT için Defender, mevcut IoT cihaz yönetimi Platformunuzla tam olarak tümleştirme özelliğine sahiptir. Tam tümleştirme, cihazınızın güvenlik durumunu yönetmenizi sağlar ve var olan tüm cihaz denetimi yeteneklerini kullanmanıza olanak sağlar. Tümleştirme, IoT Hub ikizi mekanizması kullanılarak elde edilir. 

Azure IoT Hub 'de [cihaz](../iot-hub/iot-hub-devguide-device-twins.md)ikseponları kavramı hakkında daha fazla bilgi edinin   . 

## <a name="defender-iot-micro-agent-twins"></a>Defender-IoT-Micro-Agent TWINS 

IoT için Defender, her cihaz için bir Defender-IoT-mikro-Agent ikizi kullanır. Defender-IoT-Micro-Agent ikizi, çözümünüzdeki her bir cihaz için cihaz güvenliği ile ilgili tüm bilgileri tutar. Cihaz güvenlik özellikleri, daha az kaynak gerektiren güncelleştirmeleri ve bakımı etkinleştirmek üzere daha güvenli iletişim için adanmış bir Defender-IoT-mikro-Agent ikizi aracılığıyla yapılandırılır. 

## <a name="understanding-defenderiotmicroagent-module-twins"></a>Savunma, Savuntmikro aracı modül TWINS 'i anlama 

Cihaz TWINS, Azure 'da yerleşik IoT çözümleri için hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar.

IoT için Defender, mevcut IoT cihaz yönetimi platformunuzu tamamen tümleştirme yeteneği sunarak cihaz güvenlik durumunuzu yönetmenizi ve mevcut cihaz denetimi yeteneklerini kullanmanızı sağlar. IoT Hub ikizi mekanizmasını kullanarak Defender 'ı IoT için tümleştirebilirsiniz.  

Azure IoT Hub 'de modül TWINS kavramı hakkında daha fazla bilgi edinmek için bkz. [IoT Hub Module TWINS](../iot-hub/iot-hub-devguide-module-twins.md).

IoT için Defender, Module ikizi mekanizmasını kullanır ve cihazlarınızın her biri için adlandırılmış bir Defender-IoT-mikro-Agent ikizi bulundurur `DefenderIotMicroAgent` . 

IoT özelliğinin tüm Defender özelliklerinden tam anlamıyla yararlanabilmek için, hizmette bulunan her cihaz için Defender-IoT-mikro-Agent TWINS 'i oluşturmanız, yapılandırmanız ve kullanmanız gerekir. 

## <a name="create-defenderiotmicroagent-module-twin"></a>Savunma Deriotmikro aracı modülü ikizi oluşturma 

**Savunmaiotmikro aracı** modül TWINS her bir cihaza yönelik belirli yapılandırmaların dahil olduğu her bir modülü ikizi el ile düzenleyerek oluşturulabilir. 

Bir cihaz için el ile yeni bir **savunma Deriotmikro aracı** modülü ikizi oluşturma: 

1. IoT Hub, üzerinde bir Defender-IoT-mikro-Agent ikizi oluşturmak istediğiniz cihazı bulun ve seçin. 

1.  **Modül kimliği Ekle**' yi seçin. 

1.  **Modül kimlik adı**   alanına girin ve yazın  `DefenderIotMicroAgent` . 

1.  **Kaydet**'i seçin. 

## <a name="verify-the-creation-of-a-module-twin"></a>Modül ikizi oluşturmayı doğrulama 

Bir Defender-IoT-Micro-Agent ikizi 'in belirli bir cihaz için mevcut olup olmadığını doğrulamak için: 

1. Azure IoT Hub, ****    **araştırıcılar** menüsünden IoT cihazları ' nı seçin   . 

1. Cihaz KIMLIĞINI girin veya **sorgu cihazı** alanında bir seçenek belirleyin ve **sorgu cihazları**' nı seçin.  

    :::image type="content" source="media/quickstart-create-micro-agent-module-twin/iot-devices.png" alt-text="Cihazlarınızın listesini almak için sorgu cihazları ' nı seçin.":::

1. Cihazı seçin ve **cihaz ayrıntıları** sayfasını açın. 

1.  **Modül kimlikleri**   menüsünü seçin ve cihazla ilişkili modül kimlikleri listesinde **Savunııotmikro aracı** modülünün varlığını onaylayın.  

    :::image type="content" source="media/quickstart-create-micro-agent-module-twin/device-details-module.png" alt-text="Sekmesinden modül kimliklerini seçin.":::

## <a name="next-steps"></a>Sonraki adımlar 

[Güvenlik önerilerini araştırmaya](quickstart-investigate-security-recommendations.md)yönelik bilgi edinmek için sonraki makaleye ilerleyin.
