---
title: 'Hızlı başlatma: IoT için Azure Güvenlik Merkezi için bir güvenlik modülü ikizi oluşturma'
description: Bu hızlı başlangıçta, IoT için Azure Güvenlik Merkezi ile kullanılmak üzere IoT modülü için bir Azure Güvenlik Merkezi oluşturmayı öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: c782692e-1284-4c54-9d76-567bc13787cc
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2019
ms.author: mlottner
ms.openlocfilehash: b362130c2b717f813a6332f81a3c8179bea4166a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "73904157"
---
# <a name="quickstart-create-an-azureiotsecurity-module-twin"></a>Quickstart: Azureiotsecurity modülü ikizi oluşturma

Bu hızlı başlatma, yeni aygıtlar için tek tek _azureiotsecurity_ modülü ikizleri veya bir IoT Hub'daki tüm aygıtlar için toplu oluşturma modülü ikizleri oluşturmanın nasıl açıkladığını açıklar.  

## <a name="understanding-azureiotsecurity-module-twins"></a>Azureiotsecurity modülü ikizleri anlama 

Azure'da yerleşik IoT çözümleri için aygıt ikizleri hem cihaz yönetiminde hem de proses otomasyonunda önemli bir rol oynar. 

Azure Güvenlik Merkezi IoT, mevcut IoT aygıt yönetim platformunuzla tam entegrasyon sunarak cihaz güvenlik durumunuzu yönetmenize ve mevcut aygıt kontrol özelliklerinden yararlanmanıza olanak tanır.
IoT tümleştirmesi için Azure Güvenlik Merkezi, IoT Hub çift mekanizmasından yararlanılarak sağlanır.  

Azure IoT Hub'daki modül ikizleri genel konsepti hakkında daha fazla bilgi edinmek için [IoT Hub modülü ikizleri](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-module-twins) bölümüne bakın. 
 
Azure Güvenlik Merkezi IoT modülü ikiz mekanizmasını kullanır ve her aygıtınız için _azureiotsecurity_ adlı bir güvenlik modülü ikizi tutar.

Güvenlik modülü ikizi, aygıtlarınızın her biri için aygıt güvenliğiyle ilgili tüm bilgileri tutar. 
 
IoT özellikleri için Azure Güvenlik Merkezi'nden tam olarak yararlanmak için, bu güvenlik modüllerini hizmetteki her cihaz için oluşturmanız, yapılandırmanız ve kullanmanız gerekir.  

## <a name="create-azureiotsecurity-module-twin"></a>Azureiotsecurity modülü ikiz oluşturma 

_azureiotsecurity_ modülü ikizler iki şekilde oluşturulabilir:
1. [Modül toplu komut dosyası](https://aka.ms/iot-security-github-create-module) - varsayılan yapılandırmayı kullanarak modül ikizi olmayan yeni aygıtlar veya aygıtlar için otomatik olarak modül ikizi oluşturur.
2. Her modülü her cihaz için belirli yapılandırmalarla ayrı ayrı elle düzenleme.

>[!NOTE] 
> Toplu iş yöntemini kullanmak, varolan azureiotsecurity modülü ikizlerinin üzerine yazmaz. Toplu iş yöntemini kullanmak SADECE, güvenlik modülü ikizi olmayan aygıtlar için yeni modül ikizleri oluşturur. 

Varolan bir modül ikizinin yapılandırmasını nasıl değiştireceğinizi veya değiştireceğinizi öğrenmek için [aracı yapılandırmasına](how-to-agent-configuration.md) bakın. 

Bir aygıt için el ile yeni bir _azureiotsecurity_ modülü ikizi oluşturmak için aşağıdaki yönergeleri kullanın: 

1. IoT Hub'ınızda, bir güvenlik modülü oluşturmak istediğiniz aygıtı bulun ve seçin.
1. Cihazınıza tıklayın ve ardından **modül kimliği ekle'ye**tıklayın.
1. Modül **Kimlik Adı** alanına **azureiotsecurity**girin.

1. **Kaydet**'e tıklayın. 

## <a name="verify-creation-of-a-module-twin"></a>Modül ikizi oluşturmayı doğrulayın

Belirli bir aygıt için bir güvenlik modülü ikizi olup olmadığını doğrulamak için:

1. Azure IoT Hub'ınızda, **Explorers** menüsünden **IoT aygıtlarını** seçin.    
1. Aygıt kimliğini girin veya **Sorgu aygıtı alanına** bir seçenek seçin ve Sorgu **aygıtlarını**tıklatın. 
    ![Sorgu cihazları](./media/quickstart/verify-security-module-twin.png)
1. Aygıtı seçin veya Aygıt ayrıntıları sayfasını açmak için aygıtı çift tıklatın. 
1. Modül **kimlikleri** menüsünü seçin ve aygıtla ilişkili modül kimlikleri listesinde **azureiotsecurity** modülünün varlığını doğrulayın. 
    ![Bir aygıtla ilişkili modüller](./media/quickstart/verify-security-module-twin-3.png)


IoT modülü ikizleri için Azure Güvenlik Merkezi'nin özelliklerini özelleştirme hakkında daha fazla bilgi edinmek için [Temsilci yapılandırmasına](how-to-agent-configuration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Özel uyarıları yapılandırmayı öğrenmek için bir sonraki makaleye geçin...

> [!div class="nextstepaction"]
> [Özel uyarıları yapılandırma](quickstart-create-custom-alerts.md)
