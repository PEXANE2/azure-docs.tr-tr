---
title: Microsoft Defender ATP verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Microsoft Defender Gelişmiş tehdit koruması verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80756348"
---
# <a name="connect-alerts-from-microsoft-defender-advanced-threat-protection"></a>Microsoft Defender Gelişmiş tehdit koruması 'ndan uyarıları bağlama 


> [!IMPORTANT]
> Microsoft Defender Gelişmiş tehdit koruması uyarılarını alma işlemi şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 

[Microsoft Defender Gelişmiş tehdit koruması](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) Bağlayıcısı, Microsoft Defender Gelişmiş tehdit koruması 'ndan gelen uyarıları Azure Sentinel 'e aktarmanıza olanak tanır. Bu, kuruluşunuzda güvenlik olaylarını çok daha ayrıntılı bir şekilde analiz etmenizi ve etkili ve anında yanıt için PlayBook 'lar oluşturmanıza imkan tanır.

## <a name="prerequisites"></a>Ön koşullar

- Microsoft Defender [ATP dağıtımı ayarlama](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing)bölümünde açıklandığı gibi, Microsoft Defender Gelişmiş tehdit koruması için geçerli bir lisansa sahip olmanız gerekir. 
- Azure Sentinel kiracısında yönetici veya güvenlik yöneticisi olmanız gerekir.


## <a name="connect-to-microsoft-defender-advanced-threat-protection"></a>Microsoft Defender Gelişmiş tehdit koruması 'na bağlanma

Microsoft Defender Gelişmiş tehdit koruması dağıtılırsa ve verileriniz geri alıyorsa, uyarılar Azure Sentinel 'e kolayca akışını sağlayabilir.


1. Azure Sentinel 'de **veri bağlayıcıları**' nı, **Microsoft Defender Gelişmiş tehdit koruması** kutucuğunu ve **bağlayıcı sayfasını aç**' ı seçin.
1. **Bağlan**'a tıklayın. 
1. Defender ATP uyarıları için Log Analytics ' de ilgili şemayı kullanmak için, **Securityalert** araması yapın ve **sağlayıcı adı** **mdadtp**olur.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Microsoft Defender ATP 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
