---
title: Endpoint Data için Microsoft Defender 'ı Azure Sentinel 'e bağlama | Microsoft Docs
description: Microsoft Defender for Endpoint (eski adıyla Microsoft Defender ATP) verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: 69ce6cb42fd18364a7b93faa9cc01d1f793d5cd6
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657549"
---
# <a name="connect-alerts-from-microsoft-defender-for-endpoint-formerly-microsoft-defender-atp"></a>Uç nokta için Microsoft Defender 'daki uyarıları bağlama (eski adıyla Microsoft Defender ATP) 


> [!IMPORTANT]
> Endpoint Alerts için Microsoft Defender 'ın alımı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 

[Endpoint Connector Için Microsoft](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) Defender, uç nokta Için Microsoft Defender 'Daki uyarıları Azure Sentinel 'e aktarmanıza olanak tanır. Bu, kuruluşunuzda güvenlik olaylarını çok daha ayrıntılı bir şekilde analiz etmenizi ve etkili ve anında yanıt için PlayBook 'lar oluşturmanıza imkan tanır.

## <a name="prerequisites"></a>Ön koşullar

- Uç nokta [dağıtımı Için Microsoft Defender 'ı ayarlama](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing)bölümünde açıklandığı gibi, uç nokta Için Microsoft Defender için geçerli bir lisansa sahip olmanız gerekir. 
- Azure Sentinel kiracısında yönetici veya güvenlik yöneticisi olmanız gerekir.


## <a name="connect-to-microsoft-defender-for-endpoint"></a>Uç nokta için Microsoft Defender 'a bağlanma

Uç nokta için Microsoft Defender dağıtılırsa ve verileriniz geri alıyorsa, uyarılar Azure Sentinel 'e kolayca akışını sağlayabilir.


1. Azure Sentinel 'de **veri bağlayıcıları**' nı seçin, **uç nokta için Microsoft Defender** ' ı seçin (yine de Microsoft Defender Gelişmiş tehdit koruması olarak adlandırılabilir) ve **bağlayıcı sayfasını aç**' ı seçin.
1. **Bağlan**'a tıklayın. 
1. Defender ATP uyarıları için Log Analytics ' de ilgili şemayı kullanmak için, **Securityalert** araması yapın ve **sağlayıcı adı** **mdadtp**olur.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, uç nokta için Microsoft Defender 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
