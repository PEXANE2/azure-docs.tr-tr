---
title: Sentinel yapılandırma (Önizleme)
description: IoT çözümü için Azure Güvenlik Merkezi 'nden veri almak üzere Azure Sentinel 'in nasıl yapılandırılacağını açıklar.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: mlottner
ms.openlocfilehash: fe8e4c1b08f96e5f6b2fc7649f7a4361616b7c87
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81311310"
---
# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel-preview"></a>IoT için Azure Güvenlik Merkezi 'nden verilerinizi Azure Sentinel 'e bağlama (Önizleme)

> [!IMPORTANT]
> Azure Sentinel 'de IoT veri Bağlayıcısı için Azure Güvenlik Merkezi şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu kılavuzda IoT verileri için Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlamayı öğrenin.

> [!div class="checklist"]
> * Önkoşullar
> * Bağlantı ayarları
> * Log Analytics uyarı görünümü

IoT için Azure Güvenlik Merkezi 'ndeki uyarıları bağlayın ve doğrudan Azure Sentinel 'e ekleyin.

## <a name="prerequisites"></a>Önkoşullar

- Çalışma alanının **okuma** ve **yazma** izinlerine sahip olmanız gerekir.
- **IoT Için Azure Güvenlik Merkezi** 'nin ilgili IoT Hub **etkinleştirilmiş** olması gerekir.
- Bağlanmak istediğiniz **Azure IoT Hub** için hem **okuma** hem de **yazma** izinlerinizin olması gerekir.
- Ayrıca **Azure IoT Hub kaynak grubunda** **okuma** ve **yazma** izinlerinizin olması gerekir.

> [!NOTE]
> Genel Azure Kaynak uyarılarını göndermek için aboneliğinizde çalışan Azure Güvenlik Merkezi Standart katmanı lisanslaması olmalıdır. IoT için Azure Güvenlik Merkezi için gereken ücretsiz katman lisanslaması sayesinde, IoT ile ilgili uyarılar için yalnızca Azure Güvenlik Merkezi, Azure Sentinel 'e iletilir.

## <a name="connect-to-azure-security-center-for-iot"></a>IoT için Azure Güvenlik Merkezi 'ne bağlanma

1. Azure Sentinel 'de **veri bağlayıcıları** ' nı ve ardından **IoT Için Azure Güvenlik Merkezi** ' ni seçin.
1. Sağ bölmenin alt kısmından **bağlayıcı sayfasını aç**' a tıklayın.
1. Uyarıları ve cihaz uyarılarını Azure Sentinel 'e aktarmak istediğiniz her bir IoT Hub aboneliğinin yanındaki **Bağlan**' a tıklayın.
    - IoT için Azure Güvenlik Merkezi bu hub 'da etkinleştirilmemişse, bir etkinleştirme uyarı iletisi görürsünüz. Hizmeti başlatmak ve etkinleştirmek için **Etkinleştir** bağlantısına tıklayın.
1. IoT için Azure Güvenlik Merkezi 'ndeki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinize karar verebilirsiniz. **Olay oluştur**' un altında, kuralın oluşturulan uyarılardan otomatik olarak olay oluşturmasını sağlamak için **Etkinleştir** ' i seçin.  Bu kural, **analiz**  >  **etkin** kuralları altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
>Bağlantı değişiklikleri yaptıktan sonra hub listesini yenilemek 10 saniye veya daha fazla sürebilir.

## <a name="log-analytics-alert-display"></a>Uyarı görüntüsünü Log Analytics

IoT uyarıları için Azure Güvenlik Merkezi 'ni göstermek üzere Log Analytics 'de ilgili şemayı kullanmak için:

1. **Günlükleri**  >  **securityınsights**  >  **securityalert**olarak açın veya **securityalert**araması yapın.
1. Aşağıdaki KQL filtresi kullanılarak IoT tarafından oluşturulan uyarılar için yalnızca Azure Güvenlik Merkezi 'ni görmek üzere filtreleyin:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
```

### <a name="service-notes"></a>Hizmet notları

Bir IoT Hub bağlandıktan sonra merkez verileri yaklaşık 15 dakika sonra Azure 'da kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede IoT için Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlamayı öğrendiniz. Tehdit algılama ve güvenlik verileri erişimi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- Azure Sentinel kullanarak [verilerinize ilişkin görünürlük ve potansiyel tehditler alma](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility)hakkında bilgi edinin.

- [IoT güvenlik verilerinize erişme](how-to-security-data-access.md) hakkında bilgi edinin
