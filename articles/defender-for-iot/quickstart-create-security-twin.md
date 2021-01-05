---
title: 'Hızlı başlangıç: güvenlik modülü ikizi oluşturma'
description: Bu hızlı başlangıçta IoT için Azure Defender ile kullanmak üzere IoT modülü ikizi için bir Defender oluşturma hakkında bilgi edinin.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: 3e48ed870bf405a82441678b5e8c01d199e7ebff
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97835015"
---
# <a name="quickstart-create-an-azureiotsecurity-module-twin"></a>Hızlı başlangıç: azureiotsecurity modülü oluşturma ikizi

Bu hızlı başlangıçta, yeni cihazlar için tek tek _azureiotsecurity_ Module TWINS oluşturma veya bir IoT Hub tüm cihazlar için toplu işlem modülü oluşturma işlemleri açıklanmaktadır.

## <a name="understanding-azureiotsecurity-module-twins"></a>Azureiotsecurity Module TWINS 'i anlama

Azure 'da oluşturulan IoT çözümleri için, cihaz ikis, hem cihaz yönetimi hem de süreç otomasyonu 'nda anahtar rol oynar.

IoT için Defender, mevcut IoT cihaz yönetimi Platformunuzla tam tümleştirme sunarak cihaz güvenlik durumunuzu yönetmenize ve mevcut cihaz denetimi yeteneklerini kullanmanıza olanak sağlar.
IoT tümleştirmesi için Defender, IoT Hub ikizi mekanizması kullanılarak elde edilir.

Azure IoT Hub modül TWINS 'in genel kavramı hakkında daha fazla bilgi edinmek için bkz. [IoT Hub Module TWINS](../iot-hub/iot-hub-devguide-module-twins.md) .

IoT için Defender, Module ikizi mekanizmasını kullanır ve cihazlarınızın her biri için _azureiotsecurity_ adlı bir güvenlik modülünü ikizi.

İkizi güvenlik modülü, cihazlarınızın her biri için cihaz güvenliğiyle ilgili tüm bilgileri tutar.

IoT özellikleri için Defender 'ın tam kullanımını yapmak üzere, bu güvenlik modülü ikikni hizmette her cihaz için oluşturmanız, yapılandırmanız ve kullanmanız gerekir.

## <a name="create-azureiotsecurity-module-twin"></a>Azureiotsecurity modülü oluşturma ikizi

_azureiotsecurity_ Module TWINS iki şekilde oluşturulabilir:

1. [Modül Batch betiği](https://aka.ms/iot-security-github-create-module) -varsayılan yapılandırmayı kullanarak bir modül ikizi olmadan yeni cihazlar veya cihazlar için otomatik olarak modül ikizi oluşturur.
1. Her modül için her bir cihaz için belirli yapılandırmalara sahip her modülü ikizi el ile düzenleyin.

>[!NOTE]
> Batch yönteminin kullanılması, var olan azureiotsecurity Module TWINS 'in üzerine yazmaz. Batch yönteminin kullanılması, yalnızca bir güvenlik modülü ikizi olmayan cihazlar için yeni modül TWINS 'i oluşturur.

Var olan bir modülün ikizi yapılandırmasını değiştirme veya değiştirme hakkında bilgi edinmek için [Aracı yapılandırması](how-to-agent-configuration.md) bölümüne bakın.

Bir cihaz için el ile yeni bir _azureiotsecurity_ Module ikizi oluşturmak için aşağıdaki yönergeleri kullanın:

1. IoT Hub, için güvenlik modülü ikizi oluşturmak istediğiniz cihazı bulun ve seçin.
1. Cihazınıza ve ardından **modül kimliği Ekle**' ye tıklayın.
1. **Modül kimliği adı** alanına **azureiotsecurity** girin.

1. **Kaydet**’e tıklayın.

## <a name="verify-creation-of-a-module-twin"></a>Modül ikizi oluşturmayı doğrulama

Belirli bir cihaz için bir güvenlik modülü ikizi varolup olmadığını doğrulamak için:

1. Azure IoT Hub, **araştırıcılar** menüsünden **IoT cihazları** ' nı seçin.
1. Cihaz KIMLIĞINI girin veya **sorgu cihazı alanında** bir seçenek belirleyin ve **sorgu cihazları**' na tıklayın.
    :::image type="content" source="./media/quickstart/verify-security-module-twin.png" alt-text="Cihazları sorgula":::
1. Cihazı seçin veya çift tıklayarak cihaz ayrıntıları sayfasını açın.
1. **Modül kimlikleri** menüsünü seçin ve cihazla ilişkili modül kimlikleri listesinde **azureiotsecurity** modülünün varlığını onaylayın.
    :::image type="content" source="./media/quickstart/verify-security-module-twin-3.png" alt-text="Bir cihazla ilişkili modüller":::

IoT modül TWINS için Defender 'ın özelliklerini özelleştirme hakkında daha fazla bilgi edinmek için bkz. [Aracı yapılandırması](how-to-agent-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

Güvenlik önerilerini araştırmaya yönelik bilgi edinmek için sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Güvenlik önerilerini araştırma](quickstart-investigate-security-recommendations.md)