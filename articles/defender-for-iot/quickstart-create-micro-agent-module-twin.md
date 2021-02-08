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
ms.openlocfilehash: 7935db0a78db59abaf2e9983c5ebb56bb4365a7e
ms.sourcegitcommit: 4784fbba18bab59b203734b6e3a4d62d1dadf031
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2021
ms.locfileid: "99810187"
---
# <a name="create-a-defender-iot-micro-agent-module-twin-preview"></a>Bir Defender IoT mikro aracı modülü oluşturma ikizi (Önizleme)

Yeni cihazlar için bireysel **savunma Deriotmikro aracı** modülü TWINS oluşturabilirsiniz. Ayrıca, bir IoT Hub tüm cihazlar için modül TWINS 'i toplu olarak oluşturabilirsiniz. 

## <a name="device-twins"></a>Cihaz ikikesi 

Azure 'da oluşturulan IoT çözümleri için, cihaz ikis, hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar. 

IoT için Defender, mevcut IoT cihaz yönetimi Platformunuzla tam olarak tümleştirme özelliğine sahiptir. Tam tümleştirme, cihazınızın güvenlik durumunu yönetmenizi sağlar ve var olan tüm cihaz denetimi yeteneklerini kullanmanıza olanak sağlar. Tümleştirme, IoT Hub ikizi mekanizması kullanılarak elde edilir. 

Azure IoT Hub 'de [cihaz](../iot-hub/iot-hub-devguide-device-twins.md)ikseponları kavramı hakkında daha fazla bilgi edinin   . 

## <a name="security-module-twins"></a>Güvenlik modülü TWINS 

IoT için Defender, her cihaz için bir güvenlik modülü ikizi kullanır. İkizi güvenlik modülü, çözümünüzdeki her bir cihaz için cihaz güvenliği ile ilgili tüm bilgileri barındırır. Cihaz güvenlik özellikleri, daha az kaynak gerektiren güncelleştirmeleri ve bakımı etkinleştirmek üzere daha güvenli iletişim için ikizi özel bir güvenlik modülü aracılığıyla yapılandırılır. 

## <a name="understanding-defenderiotmicroagent-module-twins"></a>Savunma, Savuntmikro aracı modül TWINS 'i anlama 

Cihaz TWINS, Azure 'da yerleşik IoT çözümleri için hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar.

IoT için Defender, mevcut IoT cihaz yönetimi platformunuzu tamamen tümleştirme yeteneği sunarak cihaz güvenlik durumunuzu yönetmenizi ve mevcut cihaz denetimi yeteneklerini kullanmanızı sağlar. IoT Hub ikizi mekanizmasını kullanarak Defender 'ı IoT için tümleştirebilirsiniz.  

Azure IoT Hub 'de modül TWINS kavramı hakkında daha fazla bilgi edinmek için bkz. [IoT Hub Module TWINS](../iot-hub/iot-hub-devguide-module-twins.md).

IoT için Defender, Module ikizi mekanizmasını kullanır ve `DefenderIotMicroAgent` cihazlarınızın her biri için adlandırılmış bir güvenlik modülünü ikizi. 

IoT özelliğinin tüm Defender özelliklerinden tam anlamıyla yararlanabilmek için, hizmette bulunan her cihaz için güvenlik modülünü TWINS 'i oluşturmanız, yapılandırmanız ve kullanmanız gerekir. 

## <a name="create-defenderiotmicroagent-module-twin"></a>Savunma Deriotmikro aracı modülü ikizi oluşturma 

**Savunmaiotmikro aracı** modül TWINS her bir cihaza yönelik belirli yapılandırmaların dahil olduğu her bir modülü ikizi el ile düzenleyerek oluşturulabilir. 

Bir cihaz için el ile yeni bir **savunma Deriotmikro aracı** modülü ikizi oluşturma: 

1. IoT Hub, ikizi güvenlik modülünün oluşturulacağı cihazı bulun ve seçin. 

1.  **Modül kimliği Ekle**' yi seçin. 

1.  **Modül kimlik adı**   alanına girin ve yazın  `DefenderIotMicroAgent` . 

1.  **Kaydet**'i seçin. 

## <a name="verify-the-creation-of-a-module-twin"></a>Modül ikizi oluşturmayı doğrulama 

Belirli bir cihaz için bir güvenlik modülü ikizi varolup olmadığını doğrulamak için: 

1. Azure IoT Hub, ****    **araştırıcılar** menüsünden IoT cihazları ' nı seçin   . 

1. Cihaz KIMLIĞINI girin veya **sorgu cihazı** alanında bir seçenek belirleyin ve **sorgu cihazları**' nı seçin.  

    :::image type="content" source="media/quickstart-create-micro-agent-module-twin/iot-devices.png" alt-text="Cihazlarınızın listesini almak için sorgu cihazları ' nı seçin.":::

1. Cihazı seçin ve **cihaz ayrıntıları** sayfasını açın. 

1.  **Modül kimlikleri**   menüsünü seçin ve cihazla ilişkili modül kimlikleri listesinde **Savunııotmikro aracı** modülünün varlığını onaylayın.  

    :::image type="content" source="media/quickstart-create-micro-agent-module-twin/device-details-module.png" alt-text="Sekmesinden modül kimliklerini seçin.":::

## <a name="next-steps"></a>Sonraki adımlar 

[Güvenlik önerilerini araştırmaya](quickstart-investigate-security-recommendations.md)yönelik bilgi edinmek için sonraki makaleye ilerleyin.
