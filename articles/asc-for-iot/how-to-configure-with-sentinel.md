---
title: Sentinel'i yapılandırın (önizleme)
description: Azure Güvenlik Merkezi'nden IoT çözümü için veri alacak şekilde Azure Sentinel'i nasıl yapılandırıştırılabildiğini açıklar.
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
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81311310"
---
# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel-preview"></a>Verilerinizi Azure Güvenlik Merkezi'nden IoT için Azure Sentinel'e bağlayın (önizleme)

> [!IMPORTANT]
> Azure Sentinel'deki IoT veri bağlayıcısı için Azure Güvenlik Merkezi şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bu kılavuzda, IoT verileri için Azure Güvenlik Merkezinizi Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.

> [!div class="checklist"]
> * Ön koşullar
> * Bağlantı ayarları
> * Günlük Analytics uyarı görünümü

IoT için Azure Güvenlik Merkezi'nden uyarıları bağlayın ve doğrudan Azure Sentinel'e aktarın.

## <a name="prerequisites"></a>Ön koşullar

- Workspace'in izinleri **okumasını** ve **yazması** gerekir.
- **IoT için Azure Güvenlik Merkezi, ilgili IoT** Hub(lar) üzerinde **etkinleştirilmelidir.**
- Bağlanmak istediğiniz **Azure IoT** Hub'ında hem **okuma** hem de **yazma** izinleri olmalıdır.
- **Ayrıca Azure IoT Hub kaynak grubunda**izinleri **okuma** ve **yazma** nız gerekir.

> [!NOTE]
> Genel Azure kaynak uyarıları göndermek için aboneliğinizde Azure Güvenlik Merkezi Standart katmanı lisanslama nın çalıştırılaması gerekir. IoT için Azure Güvenlik Merkezi için gereken ücretsiz katman lisanslama ile yalnızca IoT ile ilgili uyarılar için Azure Güvenlik Merkezi Azure Sentinel'e iletilir.

## <a name="connect-to-azure-security-center-for-iot"></a>IoT için Azure Güvenlik Merkezi'ne bağlanın

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **IoT döşemesi için Azure Güvenlik Merkezi'ni** tıklatın.
1. Sağ bölmenin altından Bağlayıcı **yı aç sayfasını**tıklatın.
1. Uyarıları ve aygıt uyarılarını Azure Sentinel'e aktarmak istediğiniz her IoT Hub aboneliğinin yanındaki **Bağlan'ı**tıklatın.
    - Bu Hub'da Azure Güvenlik Merkezi IoT etkin değilse, bir Etkinleştir uyarı iletisi görürsünüz. Hizmeti başlatmak ve etkinleştirmek için **Etkinleştir** bağlantısını tıklatın.
1. IoT için Azure Güvenlik Merkezi'ndeki uyarıların Azure Sentinel'de otomatik olarak olay oluşturmasını isteyip istemediğiniz konusunda karar verebilirsiniz. **Olayları Oluştur**altında, oluşturulan uyarılardan otomatik olarak olay oluşturmak için kuralı etkinleştir'i **etkinleştir'i** seçin.  Bu kural **Analytics** > **Active** kuralları altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
>Bağlantı değişiklikleri yaptıktan sonra hub listesini yenilemek 10 saniye veya daha uzun sürebilir.

## <a name="log-analytics-alert-display"></a>Log Analytics uyarı ekranı

IoT uyarıları için Azure Güvenlik Merkezi'ni görüntülemek için Log Analytics'teki ilgili şemayı kullanmak için:

1. **Açık Günlükleri** > **SecurityInsights** > **SecurityAlert,** ya da **SecurityAlert**arayın.
1. Aşağıdaki kql filtresini kullanarak yalnızca IoT oluşturulan uyarıları görmek için filtre uygulayın:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
```

### <a name="service-notes"></a>Servis notları

Bir IoT Hub'ı bağladıktan sonra hub verileri Yaklaşık 15 dakika sonra Azure Sentinel'de kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, IoT için Azure Güvenlik Merkezi'ni Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Tehdit algılama ve güvenlik veri erişimi hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinizin ve olası tehditlerin görünürlüğünü sağlamak](https://docs.microsoft.com/azure/sentinel/quickstart-get-visibility)için Azure Sentinel'i nasıl kullanacağınızı öğrenin.

- [IoT güvenlik verilerinize nasıl erişilenöğrenin](how-to-security-data-access.md)
