---
title: IoT için Azure Sentinel for IoT (Önizleme) yapılandırma
description: Azure Sentinel 'in, bir IoT çözümü için Defender 'dan veri almak üzere nasıl yapılandırılacağını açıklar.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/09/2020
ms.author: mlottner
ms.openlocfilehash: c5c0f74ed8a5688b20eea4e74f747d3ff6dd0e63
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96340008"
---
# <a name="connect-your-data-from-defender-for-iot-to-azure-sentinel-preview"></a>IoT için Defender 'daki verilerinizi Azure Sentinel 'e bağlama (Önizleme)

Azure Sentinel 'de IoT için Azure Defender verileri Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/en-us/support/legal/preview-supplemental-terms/).

Bu kılavuzda, Azure Sentinel 'e IoT verileri için Defender 'ı nasıl bağlayacağınızı öğrenin.

> [!div class="checklist"]
> * Önkoşullar
> * Bağlantı ayarları
> * Log Analytics uyarı görünümü

IoT için Defender 'daki uyarıları bağlama ve doğrudan Azure Sentinel 'e akış.

Azure Sentinel ile IoT için Azure Defender 'ı daha sıkı bir şekilde tümleştirerek, birinci bulutta yerel SıEM ve birinci SıEM ile yerel IoT ve OT güvenliği sayesinde Microsoft, BT ve endüstriyel ağlarda Birleşik güvenlik konusunda daha basit bir yaklaşım sağlar. Bu tümleştirme, Azure Sentinel 'in Machine Learning ile birleştirildiğinde, kuruluşların genellikle bu ve daha fazla zaman alan çok aşamalı saldırıları hızla algılamasını sağlar. Ayrıca, IoT 'nin Azure Sentinel 'in güvenlik düzenlemesi, otomasyonu ve yanıt (SOAR) özellikleri ile tümleştirilmesi için Azure Defender, yerleşik olarak iyileştirilmiş PlayBook 'lar kullanılarak otomatik yanıt ve önlemeye imkan tanıyor. 

## <a name="prerequisites"></a>Önkoşullar

- Çalışma alanının **okuma** ve **yazma** izinlerine sahip olmanız gerekir.
- **IoT Için Defender 'ın** ilgili IoT Hub **etkinleştirilmiş** olması gerekir.
- Bağlanmak istediğiniz **Azure IoT Hub** için hem **okuma** hem de **yazma** izinlerinizin olması gerekir.
- Ayrıca **Azure IoT Hub kaynak grubunda** **okuma** ve **yazma** izinlerinizin olması gerekir.


## <a name="connect-to-defender-for-iot"></a>IoT için Defender 'a bağlanma

1. Azure Sentinel 'de **veri bağlayıcıları** ' nı ve ardından **IoT için Defender** kutucuğunu seçin.
1. Sağ bölmenin alt kısmından **bağlayıcı sayfasını aç**' a tıklayın.
1. Uyarıları ve cihaz uyarılarını Azure Sentinel 'e aktarmak istediğiniz her bir IoT Hub aboneliğinin yanındaki **Bağlan**' a tıklayın.
    - IoT için Defender bu hub 'da etkinleştirilmemişse, bir etkinleştirme uyarı iletisi görürsünüz. Hizmeti başlatmak ve etkinleştirmek için **Etkinleştir** bağlantısına tıklayın.
1. IoT için Defender 'daki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinize karar verebilirsiniz. **Olay oluştur**' un altında, kuralın oluşturulan uyarılardan otomatik olarak olay oluşturmasını sağlamak için **Etkinleştir** ' i seçin.  Bu kural, **analiz**  >  **etkin** kuralları altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
>Bağlantı değişiklikleri yaptıktan sonra hub listesini yenilemek 10 saniye veya daha fazla sürebilir.

## <a name="using-log-analytics-for-alert-display"></a>Uyarı görüntüleme için Log Analytics kullanma

Log Analytics bölümünde ilgili şemayı, IoT uyarıları için Defender 'ı göstermek üzere kullanmak için:

1. **Günlükleri**  >  **securityınsights**  >  **securityalert** olarak açın veya **securityalert** araması yapın.
1. Aşağıdaki KQL filtresini kullanarak yalnızca IoT tarafından oluşturulan uyarılar için Defender 'ı görmek üzere filtreleyin:

```kusto
SecurityAlert | where ProductName == "Defender for IoT"
```

### <a name="service-notes"></a>Hizmet notları

Bir IoT Hub bağlandıktan sonra merkez verileri yaklaşık 15 dakika sonra Azure 'da kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, IoT için Defender 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Tehdit algılama ve güvenlik verileri erişimi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- Azure Sentinel kullanarak [verilerinize ilişkin görünürlük ve potansiyel tehditler alma](../sentinel/quickstart-get-visibility.md)hakkında bilgi edinin.

- [IoT güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin