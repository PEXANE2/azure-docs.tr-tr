---
title: IoT için Azure Güvenlik Merkezi'ni Azure Sentinel'e bağlayın | Microsoft Dokümanlar
description: IoT verileri için Azure Güvenlik Merkezi'ni Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/18/2020
ms.author: yelevin
ms.openlocfilehash: 3af51110a4c4604444573f62be65077c786db606
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588646"
---
# <a name="connect-your-data-from-azure-security-center-for-iot-to-azure-sentinel"></a>Verilerinizi Azure Güvenlik Merkezi'nden IoT için Azure Sentinel'e bağlayın 


> [!IMPORTANT]
> IoT veri bağlayıcısı için Azure Güvenlik Merkezi şu anda genel önizlemededir. Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Tüm Azure Güvenlik Merkezi'nizi IoT etkinlikleri için Azure Güvenlik Merkezi'ne aktarmak için IoT konektörü için Azure Güvenlik Merkezi'ni kullanın. 

## <a name="prerequisites"></a>Ön koşullar

- Azure Sentinel'in dağıtıldığı Çalışma Alanında İzinleri **Okuma** ve **Yazma**
- **IoT için Azure Güvenlik Merkezi, ilgili IoT** Hub(lar) üzerinde **etkinleştirilmelidir**
- Bağlanmak istediğiniz **Azure IoT** Hub'ında izinleri **okuma** ve **yazma**
- **Azure IoT Hub kaynak grubunda** izinleri **okuma** ve **yazma**

> [!NOTE]
> IoT kaynak uyarılarını Azure Sentinel'e aktarmak için aboneliğinizdeki Azure Güvenlik Merkezi **Standart** katman lisansını etkinleştirmeniz gerekirken, Aboneliğinizdeki Azure Güvenlik Merkezi **Ücretsiz** katman lisansını Azure Sentinel'de IoT uyarıları için Azure Güvenlik Merkezi'ni görüntülemek için etkinleştirmeniz gerekir. 

## <a name="connect-to-azure-security-center-for-iot"></a>IoT için Azure Güvenlik Merkezi'ne bağlanın

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **IoT döşemesi için Azure Güvenlik Merkezi'ni** tıklatın.
1. Sağ alt bölmeden **Bağlayıcı yı aç sayfasını**tıklatın. 
1. Uyarıları ve aygıt uyarılarını Azure Sentinel'e aktarmak istediğiniz her IoT Hub aboneliğinin yanındaki **Bağlan'ı**tıklatın. 
    - Bu Hub'da Azure Güvenlik Merkezi IoT etkin değilse, bir **Etkinleştir** uyarı iletisi görürsünüz. Hizmeti başlatmak için **Etkinleştir** bağlantısını tıklatın. 
1. IoT için Azure Güvenlik Merkezi'ndeki uyarıların Azure Sentinel'de otomatik olarak olay oluşturmasını isteyip istemediğiniz konusunda karar verebilirsiniz. **Olayları Oluştur**altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan otomatik olarak olay oluşturmak için varsayılan analitik kuralını etkinleştir'i **seçin.** Bu kural **Analytics** > **Active** kuralları altında değiştirilebilir veya düzenlenebilir.

> [!NOTE]
> Bağlantı değişiklikleri yaptıktan sonra hub listesinin yenilenmesi biraz zaman alabilir. 

## <a name="log-analytics-alert-display"></a>Log Analytics uyarı ekranı

IoT uyarıları için Azure Güvenlik Merkezi'ni görüntülemek için Log Analytics'teki ilgili şemayı kullanmak için:

1. **Açık Günlükleri** > **SecurityInsights** > **SecurityAlert,** ya da **SecurityAlert**arayın. 
2. Aşağıdaki kql filtresini kullanarak yalnızca IoT oluşturulan uyarıları görmek için filtre uygulayın:

```kusto
SecurityAlert | where ProductName == "Azure Security Center for IoT"
``` 

### <a name="service-notes"></a>Servis notları

Bir IoT Hub'ı bağladıktan sonra hub verileri Yaklaşık 15 dakika sonra Azure Sentinel'de kullanılabilir.


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, IoT verileri için Azure Güvenlik Merkezi'ni Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)
