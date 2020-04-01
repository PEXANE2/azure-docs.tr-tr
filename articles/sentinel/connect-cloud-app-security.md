---
title: Bulut Uygulama Güvenliği verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Bulut Uygulaması Güvenliği verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.date: 03/24/2020
ms.author: yelevin
ms.openlocfilehash: 266d97e834247088d40837cbec1436e00d0f4be2
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422152"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security'den veri bağlama 



[Microsoft Bulut Uygulama Güvenliği](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (MCAS) konektörü, MCAS'tan Azure Sentinel'e uyarılar ve Bulut Bulma [günlükleri](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it) akışı sağlar. Bu, bulut uygulamalarınızda görünürlük elde etmenizi, siber tehditleri belirlemek ve bunlarla mücadele etmek için gelişmiş analizler elde etmenizi ve verilerinizin nasıl gittiğini kontrol etmenizi sağlar.

## <a name="prerequisites"></a>Ön koşullar

- Kullanıcınızın çalışma alanında izinleri okuması ve yazması gerekir.
- Kullanıcınızın çalışma alanının kiracıüzerinde Global Administrator veya Güvenlik Yöneticisi izinleri olmalıdır.
- Bulut Bulma günlüklerini Azure Sentinel'de aktarmak için, [Microsoft Cloud App Security'de Azure Sentinel'i SIEM olarak etkinleştirin.](https://aka.ms/AzureSentinelMCAS)

> [!IMPORTANT]
> Bulut Bulma günlüklerinin alınması şu anda genel önizlemede.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
 
## <a name="connect-to-cloud-app-security"></a>Bulut Uygulaması Güvenliğine Bağlanın

Bulut Uygulaması Güvenliği zaten varsa, [ağınızda etkinleştirildiğinden](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)emin olun.
Bulut Uygulaması Güvenliği dağıtılıyorsa ve verilerinizi alıyorsa, uyarı verileri kolayca Azure Sentinel'e aktarılabilir.


1. Azure Sentinel gezinti menüsünden **Veri bağlayıcılarını**seçin. Bağlayıcılar listesinden Microsoft Cloud **App Security** döşemesini ve ardından sağ alttaki **Açık bağlayıcı sayfası** düğmesini tıklatın.

1. Azure Sentinel'de hangi günlükleri aktarmak istediğinizi seçin; **Uyarılar** ve Bulut **Bulma Günlükleri** (önizleme) seçebilirsiniz. 

1. **Değişiklikleri Uygula'yı**tıklatın.

1. Bulut Uygulaması Güvenliği uyarıları için Log Analytics'teki ilgili `SecurityAlert` şemayı kullanmak için sorgu penceresini yazın. Cloud Discovery şema günlükleri için, yazın. `McasShadowItReporting`

> [!NOTE]
> Bulut Bulma, kullanıcıların bulut uygulamalarına olan bağlantılarını temel alan verileri toplayarak eğilimleri algılamaya ve belirlemeye yardımcı olur.
>
> Bulut Bulma verileri günlük bazda toplanmış olduğundan, en son 24 saate kadar en son verilerin Azure Sentinel'e yansıtılmayacağını unutmayın. Düşük düzeyli bir araştırmanın daha acil veri gerektirmesi durumunda, ham verilerin bulunduğu kaynak cihazveya hizmette doğrudan yapılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft Cloud App Security'yi Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Yerleşik veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri [algılamaya](tutorial-detect-threats.md) başlayın.
