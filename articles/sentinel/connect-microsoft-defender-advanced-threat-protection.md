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
ms.date: 09/16/2020
ms.author: yelevin
ms.openlocfilehash: d1de9270534a3550d8db043fdd6bcfe56ec31bda
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90895589"
---
# <a name="connect-alerts-from-microsoft-defender-for-endpoint-formerly-microsoft-defender-atp"></a>Uç nokta için Microsoft Defender 'daki uyarıları bağlama (eski adıyla Microsoft Defender ATP)

> [!IMPORTANT]
>
> - **Uç nokta Için Microsoft Defender, daha** önce **Microsoft Defender Gelişmiş tehdit koruması** veya **mdadtp**olarak biliniyordu.
>
>     Eski adı bir süre için üründe hala kullanımda (Azure Sentinel 'deki veri Bağlayıcısı dahil) görebilirsiniz.
>
> - Endpoint Alerts için Microsoft Defender 'ın alımı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Endpoint Connector Için Microsoft](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) Defender, uç nokta Için Microsoft Defender 'Daki uyarıları Azure Sentinel 'e aktarmanıza olanak tanır. Bu, kuruluşunuzda güvenlik olaylarını çok daha ayrıntılı bir şekilde analiz etmenizi ve etkili ve anında yanıt için PlayBook 'lar oluşturmanıza imkan tanır.

> [!NOTE]
>
> Uç noktanın [Gelişmiş](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview)arama Için Microsoft Defender 'daki yeni ham veri günlüklerini almak için, Microsoft 365 Defender için yeni bağlayıcıyı kullanın (eski adıyla Microsoft tehdit koruması, [bkz. belgeler](./connect-microsoft-365-defender.md)).

## <a name="prerequisites"></a>Önkoşullar

- Uç nokta [dağıtımı Için Microsoft Defender 'ı ayarlama](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing)bölümünde açıklandığı gibi, uç nokta Için Microsoft Defender için geçerli bir lisansa sahip olmanız gerekir. 

- Azure Sentinel kiracısında bir genel yönetici veya güvenlik yöneticisi olmanız gerekir.

## <a name="connect-to-microsoft-defender-for-endpoint"></a>Uç nokta için Microsoft Defender 'a bağlanma

Uç nokta için Microsoft Defender dağıtılırsa ve verileriniz geri alıyorsa, uyarılar Azure Sentinel 'e kolayca akışını sağlayabilir.

1. Azure Sentinel 'de **veri bağlayıcıları**' nı seçin, **uç nokta için Microsoft Defender** ' ı seçin (yine de *Microsoft Defender Gelişmiş tehdit koruması*olarak adlandırılabilir) ve **bağlayıcı sayfasını aç**' ı seçin.

1. **Bağlan**'a tıklayın. 

1. **Günlüklerde**uç nokta uyarıları Için Microsoft Defender 'ı sorgulamak üzere sorgu penceresinde **securityalert** girin ve **sağlayıcı adının** **mdadtp**olduğu bir filtre ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, uç nokta için Microsoft Defender 'ı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
