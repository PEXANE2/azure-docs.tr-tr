---
title: IoT için Azure Defender 'ı Azure Sentinel 'e bağlama | Microsoft Docs
description: IoT verileri için Azure Defender 'ı (eski adıyla Azure Güvenlik Merkezi) Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2020
ms.author: yelevin
ms.openlocfilehash: 822d0c742bbd54b5bab0c69e82652743584a0696
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89659614"
---
# <a name="connect-your-data-from-azure-defender-formerly-azure-security-center-for-iot-to-azure-sentinel"></a>IoT için Azure Defender 'daki (eskiden Azure Güvenlik Merkezi) verilerinizi Azure Sentinel 'e bağlama 


> [!IMPORTANT]
> IoT için Azure Defender veri Bağlayıcısı Şu anda genel önizleme aşamasındadır. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IoT için Azure Defender ' a yönelik tüm olayları Azure Sentinel ' e aktarmak için IoT bağlayıcısını kullanın. 

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel 'in dağıtıldığı çalışma alanında **okuma** ve **yazma** izinleri
- **IoT Için Azure Defender 'ın** ilgili IoT Hub **etkinleştirilmiş** olması gerekir
- Bağlanmak istediğiniz **Azure IoT Hub** **okuma** ve **yazma** izinleri
- **Azure IoT Hub kaynak grubunda** **okuma** ve **yazma** izinleri

## <a name="connect-to-azure-defender-for-iot"></a>IoT için Azure Defender 'a bağlanma

1. Azure Sentinel 'de, **veri bağlayıcıları** ' nı seçin ve ardından **Azure Defender** ' ı seçin (IoT için Azure Güvenlik Merkezi 'ni yine de kullanabilirsiniz).
1. Sağ alt bölmeden **bağlayıcı sayfasını aç**' a tıklayın. 
1. Uyarıları ve cihaz uyarılarını Azure Sentinel 'e aktarmak istediğiniz her bir IoT Hub aboneliğinin yanındaki **Bağlan**' a tıklayın. 
    - IoT için Azure Defender bu hub 'da etkinleştirilmemişse, bir **etkinleştirme** uyarı iletisi görürsünüz. Hizmeti başlatmak için **Etkinleştir** bağlantısına tıklayın. 
1. IoT için Azure Defender uyarıları 'nın Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinize karar verebilirsiniz. **Olayları oluştur**' un altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturmak üzere varsayılan analitik kuralını etkinleştirmek için **Etkinleştir** ' i seçin. Bu kural, **analiz**  >  **etkin** kuralları altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
> Bağlantı değişiklikleri yapıldıktan sonra hub listesinin yenilenmesi biraz zaman alabilir. 

## <a name="log-analytics-alert-display"></a>Uyarı görüntüsünü Log Analytics

IoT uyarıları için Azure Defender 'ı göstermek üzere Log Analytics ' deki ilgili şemayı kullanmak için:

1. **Günlükleri**  >  **securityınsights**  >  **securityalert**olarak açın veya **securityalert**araması yapın. 
2. Aşağıdaki KQL filtresini kullanarak IoT tarafından oluşturulan uyarılar için yalnızca Azure Defender 'ı görmek üzere filtreleyin:

```kusto
SecurityAlert | where ProductName == "Azure Defender for IoT"
``` 

### <a name="service-notes"></a>Hizmet notları

Bir IoT Hub bağlandıktan sonra merkez verileri yaklaşık 15 dakika sonra Azure 'da kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede IoT verileri için Azure Defender 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
