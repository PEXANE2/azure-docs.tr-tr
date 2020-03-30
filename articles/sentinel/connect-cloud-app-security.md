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
ms.date: 10/23/2019
ms.author: yelevin
ms.openlocfilehash: 348576fbbdd1037f9e2e792218b96bbbecf36668
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588374"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security'den veri bağlama 



[Bulut App Security'deki](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) günlükleri tek bir tıklamayla Azure Sentinel'e aktarabilirsiniz. Bu bağlantı, uyarıları Bulut Uygulaması Güvenliği'nden Azure Sentinel'e aktarabilmenizi sağlar. 

## <a name="prerequisites"></a>Ön koşullar

- Genel yönetici veya güvenlik yöneticisi izinleri olan kullanıcı
- Bulut Bulma günlüklerini Azure Sentinel'de aktarmak için, [Microsoft Cloud App Security'de Azure Sentinel'i SIEM olarak etkinleştirin.](https://aka.ms/AzureSentinelMCAS)

> [!IMPORTANT]
> Bulut Bulma günlüklerinin alınması şu anda genel önizlemede.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
 
## <a name="connect-to-cloud-app-security"></a>Bulut Uygulaması Güvenliğine Bağlanın

Bulut Uygulaması Güvenliği zaten varsa, [ağınızda etkinleştirildiğinden](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)emin olun.
Bulut Uygulaması Güvenliği dağıtılıyorsa ve verilerinizi alıyorsa, uyarı verileri kolayca Azure Sentinel'e aktarılabilir.


1. Azure Sentinel'de **Veri bağlayıcılarını**seçin, **Bulut Uygulaması Güvenlik** döşemesini tıklatın ve **Bağlayıcıyı Aç sayfasını**seçin.

1. Azure Sentinel'de hangi günlükleri aktarmak istediğinizi seçin, **Uyarılar** ve **Bulut Bulma günlüklerini** (önizleme) seçebilirsiniz. 

1. **Bağlan**'a tıklayın.

1. Bulut Uygulaması Güvenliği uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **SecurityAlert'i**arayın.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft Cloud App Security'yi Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
