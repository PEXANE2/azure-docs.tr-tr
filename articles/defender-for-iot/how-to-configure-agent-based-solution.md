---
title: IoT Aracısı tabanlı çözüm için Azure Defender 'ı yapılandırma
description: IoT Aracısı tabanlı çözüm için Azure Defender 'da veri toplamayı yapılandırma hakkında bilgi edinin
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 1/21/2021
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: e32b9c690db70baeda7dec2f9f1270f82c6881bd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101733304"
---
# <a name="configure-azure-defender-for-iot-agent-based-solution"></a>IoT Aracısı tabanlı çözüm için Azure Defender 'ı yapılandırma  

Bu makalede IoT Aracısı tabanlı çözüm için Azure Defender 'da veri koleksiyonunun nasıl yapılandırılacağı açıklanır.

## <a name="configure-data-collection"></a>Veri toplamayı yapılandırma

IoT Aracısı tabanlı çözüm için Azure Defender 'da veri toplamayı yapılandırmak için: 

1. Azure portal gidin ve IoT için Defender 'ın eklendiği IoT Hub seçin 

1.  **Güvenlik**   menüsünde **Ayarlar**' ı seçin. 

1.  **Veri toplamayı** seçin. 

    :::image type="content" source="media/how-to-configure-agent-based-solution/data-collection.png" alt-text="Güvenlik menüsü ayarlarından veri toplamayı seçin.":::

## <a name="geolocation-and-ip-address-handling"></a>Coğrafi konum ve IP adresi işleme 

IoT çözümünüzü güvenli hale getirmek için, IoT cihazlarınıza ait IP adresleri, IoT Edge ve IoT Hub, varsayılan olarak toplanır ve depolanır. Bu bilgiler gereklidir ve şüpheli IP adresi kaynaklarından anormal bağlantıları algılamak için kullanılır. Örneğin, bilinen bir botnet 'in bir IP adresi kaynağından veya coğrafi konum dışında bir IP adresi kaynağından bağlantı kurmaya çalıştığında yapılan girişimler olduğunda. IoT hizmeti için Defender, IP adresi verilerinin toplanmasını istediğiniz zaman etkinleştirme ve devre dışı bırakma esnekliği sunar. 

IP adresi verilerini toplamayı etkinleştirmek veya devre dışı bırakmak için: 

1. IoT Hub açın ve ardından ****    **güvenlik** menüsünden Ayarlar ' ı seçin   . 

1.  **Veri toplama**   ekranını seçin ve COĞRAFI konum ve IP adresi işleme ayarlarını gereksinimlerinize uyacak şekilde değiştirin. 

## <a name="log-analytics-creation"></a>Log Analytics oluşturma 

IoT için Defender, Log Analytics çalışma alanınızda güvenlik uyarılarını, önerileri ve ham güvenlik verilerini depolamanıza olanak tanır. IoT Hub içindeki Log Analytics alma işlemi, IoT için Defender çözümünde varsayılan olarak **off** olarak ayarlanmıştır. IoT için Defender 'ı bir log analitik çalışma alanına eklemek ve güvenlik verilerini de depolamak mümkündür. 

Log Analytics çalışma alanınızda IoT için Defender tarafından varsayılan olarak depolanan iki tür bilgi vardır:
 
- Güvenlik uyarıları.

- Öneri. 

Ek bilgi türünün depolanmasını olarak eklemeyi seçebilirsiniz `raw events` . 

> [!Note] 
>  `raw events`   Log Analytics depolama, ek depolama maliyetleri taşır. 

Log Analytics mikro aracıda çalışmasını sağlamak için: 

1. **Çalışma alanı yapılandırma**  >  **verileri toplama**' ya gidin ve geçişi  **Açık üzerine** geçirin. 

1. Yeni bir Log Analytics çalışma alanı oluşturun veya var olan bir çalışma alanını ekleyin. 

1.  **Ham güvenlik verilerine erişim**   seçeneğinin seçildiğini doğrulayın.  

    :::image type="content" source="media/how-to-configure-agent-based-solution/data-settings.png" alt-text="Ham güvenlik verilerine erişimin seçili olduğundan emin olun.":::

1.  **Kaydet**'i seçin.

Her ay, müşteri başına Azure Log Analytics hizmetine alınan ilk 5 gigabayt veri, ücretsiz bir hizmettir. Azure Log Analytics çalışma alanınıza alınan her bir gigabayt verisi, ilk 31 gün boyunca ücretsiz olarak korunur. Fiyatlandırma hakkında daha fazla bilgi için bkz. [Log Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/). 

Log Analytics çalışma alanı yapılandırmasını değiştirmek için: 

1. IoT Hub, **güvenlik** menüsünde  **Ayarlar**' ı seçin. 

1.  **Veri toplama**   ekranını seçin ve Log Analytics ayarların çalışma alanı yapılandırmasını gereksinimlerinize uyacak şekilde değiştirin. 

Yapılandırma sonrasında Log Analytics çalışma alanınızdaki uyarılarınızı erişmek için:

1. IoT için Defender 'da bir uyarı seçin.

1. **Log Analytics çalışma alanında uyarıları Araştır '** ı seçin.

Yapılandırma sonrasında Log Analytics çalışma alanınızdaki uyarılarınızı erişmek için:

1. IoT için Defender 'da bir öneri seçin.

1. **Log Analytics çalışma alanında önerileri Araştır '** ı seçin. 
 
Log Analytics verileri sorgulama hakkında daha fazla bilgi için bkz. [Log Analytics sorguları kullanmaya başlama](../azure-monitor/logs/get-started-queries.md). 

## <a name="turn-off-defender-for-iot"></a>IoT için Defender 'ı kapat 

Belirli bir IoT Hub IoT hizmetine yönelik bir Defender 'ı etkinleştirmek veya kapatmak için: 

1. IoT Hub, **güvenlik** menüsünde  **Ayarlar**' ı seçin.

1.  **Veri toplama**   ekranını seçin ve Log Analytics ayarların çalışma alanı yapılandırmasını gereksinimlerinize uyacak şekilde değiştirin.

## <a name="next-steps"></a>Sonraki adımlar 

[Çözümünüzü yapılandırmak](quickstart-configure-your-solution.md)için sonraki makaleye ilerleyin.