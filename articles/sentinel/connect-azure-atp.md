---
title: Azure ATP verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Azure ATP verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 387d04cbbb125006efcc4efc53a02015fe3f5919
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588595"
---
# <a name="connect-data-from-azure-advanced-threat-protection-atp"></a>Azure Gelişmiş Tehdit Koruması 'ndan (ATP) veri bağlama

> [!IMPORTANT]
> Azure Sentinel'deki Azure Gelişmiş Tehdit Koruması veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Tek bir tıklamayla [Azure Gelişmiş Tehdit Koruması'ndan](https://docs.microsoft.com/azure-advanced-threat-protection/what-is-atp) Azure Sentinel'e günlük akışı yapabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Genel yönetici veya güvenlik yöneticisi izinleri olan kullanıcı
- Azure ATP'nin önizleme müşterisi olmalısınız ve Azure ATP ile Microsoft Bulut Uygulama Güvenliği arasında tümleştirmeyi etkinleştirmelisiniz. Daha fazla bilgi için Azure [Gelişmiş Koruma Tümleştirmesi'ne](https://docs.microsoft.com/cloud-app-security/aatp-integration)bakın.

## <a name="connect-to-azure-atp"></a>Azure ATP'ye bağlanma

Azure ATP önizleme [sürümünün abunuzca etkin](https://docs.microsoft.com/azure-advanced-threat-protection/install-atp-step1)olduğundan emin olun.
Azure ATP dağıtılıyorsa ve verilerinizi alıyorsa, şüpheli uyarılar kolayca Azure Sentinel'e aktarılabilir. Uyarıların Azure Sentinel'de yayınlanmaya başlaması 24 saat kadar sürebilir.


1. Azure ATP'yi Azure Sentinel'e bağlamak için öncelikle Azure ATP ile Microsoft Bulut Uygulama Güvenliği arasında tümleştirmeyi etkinleştirmeniz gerekir. Bunun nasıl yapılacağını öğrenmek için [Azure Gelişmiş Tehdit Koruması tümleştirmesine](https://docs.microsoft.com/cloud-app-security/aatp-integration)bakın.

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **Azure Gelişmiş Tehdit Koruması (Önizleme)** döşemesini tıklatın.

1. Azure ATP'deki uyarıların Azure Sentinel'de otomatik olarak olay oluşturmasını isteyip istemediğiniz konusunda seçim yapabilirsiniz. **Olayları Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralını etkinleştir'i **seçin.** Daha sonra bu kuralı **Analytics** ve ardından **Etkin kurallar**altında edinebilirsiniz.

1. **Bağlan**'a tıklayın.

1. Azure ATP uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **SecurityAlert'i**arayın.

> [!NOTE]
> Uyarılar 30 KB'den büyükse, Azure Sentinel uyarılardaki Varlıklar alanını görüntülemeyi durdurur.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Gelişmiş Tehdit Koruması'nı Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

