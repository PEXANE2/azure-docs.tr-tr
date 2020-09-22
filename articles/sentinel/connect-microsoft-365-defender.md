---
title: Microsoft 365 Defender ham verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Microsoft 365 Defender 'dan Azure Sentinel 'e Ham olay verilerini nasıl alabileceğinizi öğrenin.
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
ms.openlocfilehash: e9e892ae08c65ee5c5008bbdc1b419d8dec30183
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941306"
---
# <a name="connect-data-from-microsoft-365-defender-to-azure-sentinel"></a>Microsoft 365 Defender 'dan Azure Sentinel 'e veri bağlama

> [!IMPORTANT]
>
> **Microsoft 365 Defender** daha önce **Microsoft tehdit koruması** veya **MTP**olarak biliniyordu.
>
> **Uç nokta Için Microsoft Defender, daha** önce **Microsoft Defender Gelişmiş tehdit koruması** veya **mdadtp**olarak biliniyordu.
>
> Eski adların bir süre içinde hala kullanımda olduğunu görebilirsiniz.

## <a name="background"></a>Arka Plan

Yeni [Microsoft 365 Defender](https://docs.microsoft.com/microsoft-365/security/mtp/microsoft-threat-protection) Bağlayıcısı, bir dizi Ham olay verilerinin Azure Sentinel 'e Microsoft 365 Defender **'a akışını sağlamanıza** olanak tanır. 

[Uç nokta Için Microsoft Defender 'ın (MDADTP)](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) Microsoft 365 Defender güvenliği şemsiye tümleştirmesiyle, artık Microsoft Defender 'ı, Microsoft 365 Defender bağlayıcısını kullanarak uç nokta [Gelişmiş](https://aka.ms/mdatpAH) arama olayları Için toplayabilir ve bunları Azure Sentinel çalışma alanınızdaki yeni amaç tarafından oluşturulan tablolarda doğrudan akışla aktarabilirsiniz. Bu tablolar Microsoft 365 Defender portalında kullanılan şemaya kurulmuştur. böylece, gelişmiş arama günlüklerinin tamamına tam erişim sağlar ve şunları yapabilirsiniz:

- Mevcut Microsoft Defender ATP gelişmiş hunme sorgularınızı kolayca Azure Sentinel 'e kopyalayın.

- Uyarılarınız için ek Öngörüler sağlamak ve Azure Sentinel 'de ek veri kaynaklarından gelen verilerle ilişkilendirmek için ham olay günlüklerini kullanın.

- Uç nokta veya Microsoft 365 Defender 'ın 30 günlük varsayılan saklama süresini aşan, daha fazla bekletme ile günlükleri depolayın. Bunu, çalışma alanınızın bekletmesini yapılandırarak veya Log Analytics göre her tablo için tutmayı yapılandırarak yapabilirsiniz.

> [!IMPORTANT]
>
> Microsoft 365 Defender Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

- Uç nokta [dağıtımı Için Microsoft Defender 'ı ayarlama](https://docs.microsoft.com/windows/security/threat-protection/microsoft-defender-atp/licensing)bölümünde açıklandığı gibi, uç nokta Için Microsoft Defender için geçerli bir lisansa sahip olmanız gerekir. 

- Kullanıcıya Kiracıdaki genel yönetici rolü atanmalıdır (Azure Active Directory).

## <a name="connect-to-microsoft-365-defender"></a>Microsoft 365 Defender 'a bağlanma

Uç nokta için Microsoft Defender dağıtılırsa ve verileriniz geri alıyorsa, olay günlükleri kolayca Azure Sentinel 'e akışla eklenebilir.

1. Azure Sentinel 'de **veri bağlayıcıları**' nı seçin, Galeriden **Microsoft 365 Defender (Önizleme)** öğesini seçin ve **bağlayıcı sayfasını aç**' ı seçin.

1. Aşağıdaki olay türleri ilgili gelişmiş hunme tablolarından toplanabilir. Toplamak istediğiniz olay türlerinin onay kutularını işaretleyin:

    | Olay türü | Tablo adı |
    |-|-|
    | Makine bilgileri (işletim sistemi bilgileri dahil) | DeviceInfo |
    | Makinelerin ağ özellikleri | Devicenetworkınfo |
    | İşlem oluşturma ve ilgili olaylar | DeviceProcessEvents |
    | Ağ bağlantısı ve ilgili olaylar | DeviceNetworkEvents |
    | Dosya oluşturma, değiştirme ve diğer dosya sistemi olayları | DeviceFileEvents |
    | Kayıt defteri girişlerini oluşturma ve değiştirme | DeviceRegistryEvents |
    | Oturum açma işlemleri ve diğer kimlik doğrulama olayları | DeviceLogonEvents |
    | DLL yükleme olayları | Deviceımageloadevents |
    | Ek olay türleri | DeviceEvents |
    |

1. **Değişiklikleri Uygula**' ya tıklayın. 

1. Log Analytics gelişmiş hunmiş tabloları sorgulamak için, sorgu penceresinde yukarıdaki listeden tablo adını girin.

## <a name="verify-data-ingestion"></a>Veri alımını doğrulama

Bağlayıcı sayfasındaki veri grafiği, verileri iade ettiğiniz anlamına gelir. Tüm etkin tablolarda olay birimini toplayan tek bir satır gösterdiğine dikkat edin. Bağlayıcıyı etkinleştirdikten sonra, tek bir tablo için bir olay birimi için benzer bir grafik oluşturmak üzere aşağıdaki KQL sorgusunu kullanabilirsiniz ( *Deviceevents* tablosunu seçtiğiniz gerekli tabloyla değiştirin):

```kusto
let Now = now();
(range TimeGenerated from ago(14d) to Now-1d step 1d
| extend Count = 0
| union isfuzzy=true (
    DeviceEvents
    | summarize Count = count() by bin_at(TimeGenerated, 1d, Now)
)
| summarize Count=max(Count) by bin_at(TimeGenerated, 1d, Now)
| sort by TimeGenerated
| project Value = iff(isnull(Count), 0, Count), Time = TimeGenerated, Legend = "Events")
| render timechart
```

**Sonraki adımlar** sekmesinde, eklenen birkaç örnek sorgu bulacaksınız. Bunları leke üzerinde çalıştırabilir veya değiştirebilir ve kaydedebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Microsoft 365 Defender bağlayıcısını kullanarak uç nokta için Microsoft Defender 'dan Azure Sentinel 'e Ham olay verileri almayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
