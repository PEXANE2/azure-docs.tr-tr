---
title: Azure Sentinel ile Defender tümleştirmesini Microsoft 365 | Microsoft Docs
description: Microsoft 365 Defender tümleştirmesini Azure Sentinel ile tümleştirme hakkında bilgi edinmek için M365D's güçlerini, M365 güvenlik olaylarını araştırmaya yardımcı olacak şekilde korurken ve ayrıca, Defender bileşenlerinin Azure Sentinel 'e yönelik gelişmiş verileri alma ' ya nasıl alınacağını öğrenmek için Azure Sentinel 'i evrensel olaylar kuyruğu olarak kullanma olanağı sunar.
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
ms.date: 03/02/2021
ms.author: yelevin
ms.openlocfilehash: abace18db51a7a571ecc66d50253277fbd2296d3
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "101747003"
---
# <a name="microsoft-365-defender-integration-with-azure-sentinel"></a>Azure Sentinel ile Defender tümleştirmesi Microsoft 365

> [!IMPORTANT]
> Microsoft 365 Defender Bağlayıcısı Şu anda **Önizleme** aşamasındadır. Beta, önizleme veya henüz genel kullanıma sunulmayan Azure özelliklerine uygulanan ek koşullar için [Microsoft Azure önizlemeleri için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'na bakın.

> [!IMPORTANT]
>
> **Microsoft 365 Defender** daha önce **Microsoft tehdit koruması** veya **MTP** olarak biliniyordu.
>
> **Uç nokta Için Microsoft Defender, daha** önce **Microsoft Defender Gelişmiş tehdit koruması** veya **mdadtp** olarak biliniyordu.
>
> Eski adların bir süre içinde hala kullanımda olduğunu görebilirsiniz.

## <a name="incident-integration"></a>Olay tümleştirme

Azure Sentinel [Microsoft 365 Defender (M365D)](/microsoft-365/security/mtp/microsoft-threat-protection) olay tümleştirmesi, tüm M365D olaylarını Azure Sentinel 'e akışını ve her iki Portal arasında eşitlenmesini sağlar. M365D (eski adıyla Microsoft tehdit koruması veya MTP) olayları, ilişkili tüm uyarıları, varlıkları ve ilgili bilgileri içerir ve Azure Sentinel 'de önceliklendirme ve ön araştırma gerçekleştirmek için yeterli bağlam sağlar. Sentinel 'de, olaylar M365D ile eşitlenmiş olarak kalır ve olay araştırmanızda her iki portalın avantajlarından yararlanmanızı sağlar.

Bu tümleştirme, kuruluşun tamamı genelinde birincil olay sırasının bir parçası olarak Azure Sentinel 'in içinden yönetilmek üzere Microsoft 365 güvenlik olayları sağlar. böylece, tüm diğer bulutlardan ve şirket içi sistemlerinizden bunlarla birlikte M365 olaylarını görebilir ve bunlarla ilişkilendirilebilecek. Aynı zamanda, M365D ve M365 ekosisteminde M365 'e özgü bir deneyim için, ayrıntılı araştırmalar için 'in benzersiz güçlerinden ve özelliğinden yararlanmanızı sağlar. M365 Defender, her ikisi de SOC olay kuyruğunun boyutunu azaltarak ve çözümleme süresini kısaltabilmek için birden çok M365 üründen gelen uyarıları ve grupları gruplandırır. M365 Defender yığınının parçası olan bileşen hizmetleri şunlardır:

- **Uç nokta Için Microsoft Defender** (MDE, eski adıyla mdadtp)
- **Kimlik Için Microsoft Defender** (MDI, eski adıyla AATP)
- **Office 365 Için Microsoft Defender** (MDO, eskı adıyla O365ATP)
- **Microsoft Cloud App Security** (MCAS)

M365 Defender, bu bileşenlerden uyarı toplamaya ek olarak kendi uyarılarını oluşturur. Tüm bu uyarılardan olaylar oluşturur ve bunları Azure Sentinel 'e gönderir.

### <a name="common-use-cases-and-scenarios"></a>Yaygın kullanım örnekleri ve senaryolar

- M365 Defender bileşenlerinden gelen tüm uyarılar ve varlıklar dahil olmak üzere M365 Defender olaylarını tek tıklamayla Azure Sentinel 'e bağlayın.

- Durum, sahip ve kapatma nedeniyle Sentinel ve M365D olayları arasında iki yönlü eşitleme.

- Azure Sentinel 'de M365 Defender uyarı gruplandırması ve zenginleştirme özelliklerinden yararlanın ve bu sayede çözülmesi gereken süreyi azaltır.

- Her iki portaldaki araştırmaların kolaylaştırmasını sağlamak için bir Azure Sentinel olayı ve onun paralel M365 Defender olayı arasında bağlam içi ayrıntılı bağlantı.

### <a name="connecting-to-microsoft-365-defender"></a>Microsoft 365 Defender 'a bağlanılıyor

[Olayları ve uyarıları toplamak](connect-microsoft-365-defender.md)üzere Microsoft 365 Defender veri bağlayıcısını etkinleştirdikten sonra, M365D olayları, M365 Defender ' da üretildikten sonra, **ürün adı** alanında **Microsoft 365 Defender** ile Azure Sentinel olayları kuyruğunda görüntülenir.
- M365 Defender 'da bir olayın oluşturulduğu zamandan Azure Sentinel ' te göründüğü zamandan itibaren 10 dakika kadar sürebilir.

- Olaylar, ek bir maliyet olmadan alınır ve eşitlenir.

M365 Defender tümleştirmesi bağlandıktan sonra, tüm bileşen uyarısı bağlayıcıları (MDE, MDI, MDO, MCAS) zaten yoksa arka planda otomatik olarak bağlanır. M365 Defender bağlandıktan sonra herhangi bir bileşen lisansı satın alındıysa, yeni ürünün uyarıları ve olayları ek yapılandırma veya ücret olmadan Azure Sentinel 'e de akacaktır.

### <a name="m365-defender-incidents-and-microsoft-incident-creation-rules"></a>M365 Defender olayları ve Microsoft olay oluşturma kuralları

- M365 güvenlik ürünlerinden gelen uyarıların temelinde M365 Defender tarafından oluşturulan olaylar, özel M365 mantığı kullanılarak oluşturulur.

- Azure Sentinel 'de Microsoft olay oluşturma kuralları, aynı uyarılardan (farklı bir özel Azure Sentinel mantığı) olaylar da oluşturur.

- İki mekanizmaların birlikte kullanılması tamamen desteklenir ve bu yapılandırma yeni M365 Defender olay oluşturma mantığına geçişi kolaylaştırmak için kullanılabilir. Bununla birlikte, aynı uyarılar için **yinelenen olaylar** oluşturur.

- Aynı uyarılarla aynı olay oluşturulmasını önlemek için, M365 Defender bağlanırken müşterilerin M365 ürünleri (MDE, MDI ve MTHE MCAS) için tüm **Microsoft olay oluşturma kurallarını** kapatmasını öneririz. Bu işlem, bağlayıcı sayfasındaki ilgili onay kutusu işaretlenerek yapılabilir. Bunu yaparsanız, olay oluşturma kuralları tarafından uygulanan filtrelerin M365 Defender olay tümleştirmesine uygulanmayacağını aklınızda bulundurun.

- Microsoft Cloud App Security (MCAS) uyarıları için, tüm uyarı türleri şu anda M365 Defender ' a eklendi. Tüm MCAS uyarılarına ilişkin olayları almaya devam ettiğinizden emin olmak için, eklendi ' e ait *olmayan* Uyarı türleri için **Microsoft olay oluşturma kurallarını** saklamanız veya oluşturmanız gerekir.

### <a name="working-with-m365-defender-incidents-in-azure-sentinel-and-bi-directional-sync"></a>Azure Sentinel ve iki yönlü eşitlemede M365 Defender olayları ile çalışma

M365 Defender olayları, **Microsoft 365 Defender** ürün adı Ile Azure Sentinel olayları kuyruğunda ve diğer Sentinel olaylarına benzer ayrıntılarla ve işlevlerle görüntülenir. Her olay, M365 Defender portalındaki paralel olaya geri bir bağlantı içerir.

Olay M365 Defender 'da geliştikçe ve buna daha fazla uyarı veya varlık eklenirken, Azure Sentinel olayı buna uygun şekilde güncelleştirilir.

Durum, kapatma nedeni veya bir M365 olayının atanması üzerinde yapılan değişiklikler, M365D veya Azure Sentinel 'de, benzer şekilde diğer olay kuyruğunda da güncelleştirilir. Eşitleme, olay değişikliği, gecikme olmadan uygulandıktan hemen sonra her iki portalda gerçekleşir. En son değişiklikleri görmek için yenileme gerekebilir.

M365 Defender 'da, bir olaydan gelen tüm uyarılar başka bir olaya aktarılabilir ve birleştirilmekte olan olaylara neden olur. Bu durumda, Azure Sentinel olayları değişiklikleri yansıtacaktır. Bir olay, her iki özgün olaydan gelen tüm uyarıları içerir ve diğer olay "yeniden yönlendirildi" etiketiyle birlikte otomatik olarak kapatılır.

> [!NOTE]
> Azure Sentinel 'deki olaylar en fazla 150 uyarı içerebilir. M365D olayları bundan daha fazlasına sahip olabilir. 150 'den fazla uyarı içeren bir M365D olayı Azure Sentinel 'e eşitlendiğinde, Sentinel olayı "150 +" uyarılarıyla karşılaşacaktır ve bu durumda, tüm uyarı kümesini göreceğiniz M365D 'de paralel olaya bir bağlantı sağlar.

## <a name="advanced-hunting-event-collection"></a>Gelişmiş bir olay toplama

Microsoft 365 Defender Bağlayıcısı, Microsoft 365 Defender ve bileşen hizmetlerinden Azure Sentinel 'e kadar **Gelişmiş** arama olaylarını (Ham olay verileri) akışa almanızı de sağlar. Şu anda [Microsoft Defender for Endpoint (MDADTP)](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection) [Gelişmiş](/windows/security/threat-protection/microsoft-defender-atp/advanced-hunting-overview) arama olaylarını toplayabilir ve bunları Azure Sentinel çalışma alanınızdaki amaç tarafından oluşturulan tablolarda doğrudan akışla aktarabilirsiniz. Bu tablolar Microsoft 365 Defender portalında kullanılan aynı şemaya kurulmuştur, böylece gelişmiş arama olaylarının tamamına tam erişim sağlar ve şunları yapabilirsiniz:

- Mevcut Microsoft Defender for Endpoint gelişmiş arama sorgularını Azure Sentinel 'e kolayca kopyalayın.

- Uyarı, araştırma ve araştırmanıza yönelik ek Öngörüler sağlamak ve bu olayları Azure Sentinel 'teki ek veri kaynaklarından başkalarıyla ilişkilendirmek için ham olay günlüklerini kullanın.

- Uç nokta veya Microsoft 365 Defender 'ın 30 günlük varsayılan saklama süresini aşan, daha fazla bekletme ile günlükleri depolayın. Bunu, çalışma alanınızın bekletmesini yapılandırarak veya Log Analytics göre her tablo için tutmayı yapılandırarak yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Microsoft 365 Defender bağlayıcısını kullanarak Microsoft 365 Defender 'ı Azure Sentinel ile birlikte kullanmaya nasıl yararlanabileceğinizi öğrendiniz.

- [Microsoft 365 Defender bağlayıcısını etkinleştirmeye](connect-microsoft-365-defender.md)yönelik yönergeler alın.
- [Özel uyarılar](tutorial-detect-threats-custom.md) oluşturun ve [olayları araştırın](tutorial-investigate-cases.md).
