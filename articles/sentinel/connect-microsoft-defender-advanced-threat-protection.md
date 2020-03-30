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
ms.openlocfilehash: ee2c917d02a442ebabc0e1b40bc245982af794f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588221"
---
# <a name="connect-alerts-from-microsoft-defender-advanced-threat-protection"></a>Microsoft Defender Gelişmiş Tehdit Koruması uyarılarını bağlama 


> [!IMPORTANT]
> Microsoft Defender Gelişmiş Tehdit Koruması günlüklerinin alınması şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
 

Microsoft Defender Gelişmiş [Tehdit Koruması'ndan](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) uyarıları tek bir tıklamayla Azure Sentinel'e aktarabilirsiniz. Bu bağlantı, Microsoft Defender Gelişmiş Tehdit Koruması'ndaki uyarıları Azure Sentinel'e aktarabilmenizi sağlar. 

## <a name="prerequisites"></a>Ön koşullar

- Microsoft Defender Gelişmiş Tehdit Koruması için geçerli lisans, Microsoft Defender Gelişmiş Tehdit Koruması için lisans [sağlamayı doğrula ve Microsoft Defender Gelişmiş Tehdit Koruması için tam kurulumda](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing)açıklandığı gibi etkinleştirilir. 
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
