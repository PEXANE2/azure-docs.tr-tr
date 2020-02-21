---
title: IoT için Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlama | Microsoft Docs
description: IoT verileri için Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: rkarlin
ms.openlocfilehash: b68745f04a49565090b0f9b14331036f76e5f132
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77501296"
---
# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel"></a>IoT için Azure Güvenlik Merkezi 'nden verilerinizi Azure Sentinel 'e bağlama 


> [!IMPORTANT]
> IoT veri Bağlayıcısı için Azure Güvenlik Merkezi şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IoT olayları için Azure Güvenlik Merkezi 'ni Azure Sentinel 'e aktarmak üzere IoT Bağlayıcısı için Azure Güvenlik Merkezi 'ni kullanın. 

## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel 'in dağıtıldığı çalışma alanında **okuma** ve **yazma** izinleri
- **IoT Için Azure Güvenlik Merkezi** 'nin ilgili IoT Hub **etkinleştirilmiş** olması gerekir
- Bağlanmak istediğiniz **Azure IoT Hub** **okuma** ve **yazma** izinleri
- **Azure IoT Hub kaynak grubunda** **okuma** ve **yazma** izinleri

> [!NOTE]
> IoT kaynak uyarılarını Azure Sentinel 'e aktarmak için aboneliğinizde Azure Güvenlik Merkezi **Standart** katmanı lisansını etkinleştirmeniz gerekir, ancak Azure Sentinel 'de IoT uyarıları Için Azure Güvenlik Merkezi 'ni görüntülemek için aboneliğinizdeki Azure Güvenlik Merkezi **ücretsiz** katman lisansını etkinleştirmeniz gerekir. 

## <a name="connect-to-azure-security-center-for-iot"></a>IoT için Azure Güvenlik Merkezi 'ne bağlanma

1. Azure Sentinel 'de **veri bağlayıcıları** ' nı ve ardından **IoT Için Azure Güvenlik Merkezi** ' ni seçin.
1. Sağ alt bölmeden **bağlayıcı sayfasını aç**' a tıklayın. 
1. Uyarıları ve cihaz uyarılarını Azure Sentinel 'e aktarmak istediğiniz her bir IoT Hub aboneliğinin yanındaki **Bağlan**' a tıklayın. 
    - IoT için Azure Güvenlik Merkezi bu hub 'da etkinleştirilmemişse, bir **etkinleştirme** uyarı iletisi görürsünüz. Hizmeti başlatmak için **Etkinleştir** bağlantısına tıklayın. 
1. IoT için Azure Güvenlik Merkezi 'ndeki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinize karar verebilirsiniz. **Olayları oluştur**' un altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturmak üzere varsayılan analitik kuralını etkinleştirmek için **Etkinleştir** ' i seçin. Bu kural, **analiz** > **etkin** kurallar altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
> Bağlantı değişiklikleri yapıldıktan sonra hub listesinin yenilenmesi biraz zaman alabilir. 

## <a name="log-analytics-alert-display"></a>Uyarı görüntüsünü Log Analytics

IoT uyarıları için Azure Güvenlik Merkezi 'ni göstermek üzere Log Analytics 'de ilgili şemayı kullanmak için:

1. **Günlükleri** > **Securityınsights** > **securityalert**' ya açın veya **securityalert**araması yapın. 
2. Aşağıdaki KQL filtresi kullanılarak IoT tarafından oluşturulan uyarılar için yalnızca Azure Güvenlik Merkezi 'ni görmek üzere filtreleyin:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
``` 

### <a name="service-notes"></a>Hizmet notları

Bir IoT Hub bağlandıktan sonra merkez verileri yaklaşık 15 dakika sonra Azure 'da kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede IoT verileri için Azure Güvenlik Merkezi 'ni Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
