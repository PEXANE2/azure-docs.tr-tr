---
title: Microsoft Defender ATP verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Microsoft Defender Gelişmiş Tehdit Koruması verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.date: 10/13/2019
ms.author: yelevin
ms.openlocfilehash: 831baabcc26130d9b2c730afd85f2ed4b3842690
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756348"
---
# <a name="connect-alerts-from-microsoft-defender-advanced-threat-protection"></a>Microsoft Defender Gelişmiş Tehdit Koruması uyarılarını bağlama 


> [!IMPORTANT]
> Microsoft Defender Gelişmiş Tehdit Koruması uyarılarının alınması şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
 

[Microsoft Defender Gelişmiş Tehdit Koruması](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) konektörü, Microsoft Defender Gelişmiş Tehdit Koruması'ndan gelen uyarıları Azure Sentinel'e aktarmanızı sağlar. Bu, kuruluşunuzdaki güvenlik olaylarını daha kapsamlı bir şekilde analiz etmenizi ve etkili ve anında yanıt için oyun kitapları oluşturmanıza olanak tanır.

## <a name="prerequisites"></a>Ön koşullar

- [Microsoft Defender ATP dağıtımını ayarla'da](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing)açıklandığı gibi, Microsoft Defender Gelişmiş Tehdit Koruması için geçerli bir lisansa sahip olmalısınız. 
- Azure Sentinel kiracısının yöneticisi veya güvenlik yöneticisi olmalısınız.


## <a name="connect-to-microsoft-defender-advanced-threat-protection"></a>Microsoft Defender Gelişmiş Tehdit Koruması'na bağlanın

Microsoft Defender Gelişmiş Tehdit Koruması dağıtılırsa ve verilerinizi sindiriyorsa, uyarılar kolayca Azure Sentinel'e aktarılabilir.


1. Azure Sentinel'de **Veri bağlayıcılarını**seçin, **Microsoft Defender Gelişmiş Tehdit Koruması** döşemesini tıklatın ve Açık bağlayıcı **sayfasını**seçin.
1. **Bağlan**'a tıklayın. 
1. Defender ATP uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **SecurityAlert'i** arayın ve **Sağlayıcı adı** **MDATP'dir.**




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft Defender ATP'yi Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
