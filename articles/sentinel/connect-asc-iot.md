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
ms.date: 12/16/2020
ms.author: yelevin
ms.openlocfilehash: 95e78c7557092a4d1203a8df3a107fe7b63eac9b
ms.sourcegitcommit: 8c3a656f82aa6f9c2792a27b02bbaa634786f42d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97631450"
---
# <a name="connect-your-data-from-azure-defender-formerly-azure-security-center-for-iot-to-azure-sentinel"></a>IoT için Azure Defender 'daki (eskiden Azure Güvenlik Merkezi) verilerinizi Azure Sentinel 'e bağlama 


> [!IMPORTANT]
> IoT veri Bağlayıcısı için Defender Şu anda genel önizleme aşamasındadır. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Tüm IoT olayları için Defender 'ı Azure Sentinel 'e aktarmak üzere IoT için Defender bağlayıcısını kullanın. 

Bu tümleştirme, kuruluşların genellikle onu ve sınırları aşan çok aşamalı saldırıları hızla algılamasını sağlar. Ayrıca, IoT 'nin Azure Sentinel 'in güvenlik düzenlemesi, otomasyonu ve yanıtı (SOAR) özellikleri ile tümleştirilmesi için Defender, yerleşik olarak iyileştirilmiş PlayBook 'lar kullanılarak otomatik yanıt ve önlemeye imkan tanıyor. 
## <a name="prerequisites"></a>Önkoşullar

- Azure Sentinel 'in dağıtıldığı çalışma alanında **okuma** ve **yazma** izinleri
- **IoT Için Defender 'ın** ilgili IoT Hub **etkinleştirilmiş** olması gerekir
- Bağlanmak istediğiniz **abonelik** üzerinde **katkıda bulunan** izinlerinizin olması gerekir

## <a name="connect-to-defender-for-iot"></a>IoT için Defender 'a bağlanma

1. Azure Sentinel 'de, **veri bağlayıcıları** ' nı seçin ve sonra, galerinin **(IoT Için Azure** Güvenlik Merkezi 'nin hala bu şekilde adlandırılır) ' i seçin.

1. Sağ bölmenin alt kısmından **bağlayıcı sayfasını aç**' a tıklayın. 

1. Uyarıları ve cihaz uyarılarını Azure Sentinel 'e aktarmak istediğiniz her bir IoT Hub aboneliğinin yanındaki **Bağlan**' a tıklayın. 
    - IoT için Defender, bir abonelik içindeki en az bir IoT Hub etkinleştirilmemişse bir hata iletisi alırsınız. Hatayı kaldırmak için IoT Hub içindeki IoT için Defender 'ı etkinleştirin.

1. IoT için Defender 'daki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinize karar verebilirsiniz. **Olayları oluştur** altında, varsayılan analiz kuralının oluşturulan uyarılardan otomatik olarak olay oluşturmasını sağlamak için **Etkinleştir** ' i seçin. Bu kural, **analiz**  >  **etkin kuralları** altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
> **Abonelik** listesinin bağlantı değişiklikleri yapıldıktan sonra yenilenmesi için 10 saniye veya daha fazla zaman alabilir. 

## <a name="log-analytics-alert-view"></a>Log Analytics uyarı görünümü

Log Analytics bölümünde ilgili şemayı, IoT uyarıları için Defender 'ı göstermek üzere kullanmak için:

1. **Günlükleri**  >  **securityınsights**  >  **securityalert** olarak açın veya **securityalert** araması yapın. 

2. Aşağıdaki KQL filtresini kullanarak yalnızca IoT tarafından oluşturulan uyarılar için Defender 'ı görmek üzere filtreleyin:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
``` 

### <a name="service-notes"></a>Hizmet notları

Bir **aboneliği** bağladıktan sonra merkez verileri yaklaşık 15 dakika sonra Azure 'da kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, IoT için Defender 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
