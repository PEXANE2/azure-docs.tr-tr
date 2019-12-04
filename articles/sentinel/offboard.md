---
title: Azure Sentinel | Microsoft Docs
description: Azure Sentinel örneğinizi silme.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/02/2019
ms.author: rkarlin
ms.openlocfilehash: 4c0c415235fd290bc47ac402a6b81a1afa7af903
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74777443"
---
# <a name="remove-azure-sentinel-from-your-workspace"></a>Azure Sentinel 'i çalışma alanınızdan kaldırma

Artık Azure Sentinel 'i kullanmak istemiyorsanız, bu makalede çalışma alanınızdan nasıl kaldırılacağı açıklanır.

## <a name="how-to-delete-azure-sentinel"></a>Azure Sentinel 'i silme

Arka planda, Azure Sentinel 'i yüklediğinizde, **Securityınsights** çözümü seçili çalışma alanınıza yüklenir. Yani yapmanız gereken ilk şey **Securityınsights** çözümünü kaldırmın.

1.  **Azure Sentinel**'e, ardından **yapılandırma**' ya, **çalışma alanı ayarları**' na ve ardından **çözümler**' e gidin.

2.  `SecurityInsights` seçin ve üzerine tıklayın.

    ![Securityınsights çözümünü bulun](media/offboard/find-solution.png)

3.  Sayfanın üst kısmında **Sil**' i seçin.

    > [!IMPORTANT]
    > Çalışma alanını kaldırırsanız, Azure Izleyici dahil olmak üzere bu çalışma alanını kullanan diğer çözümleri ve veri kaynaklarını etkileyebilir. Bu çalışma alanını hangi çözümlerin kullandığını denetlemek için bkz. [yüklü izleme çözümlerini listeleme](../azure-monitor/insights/solutions.md#list-installed-monitoring-solutions). Çalışma alanına hangi çözümlerin verilerinin kullanıldığını denetlemek için bkz. alınan [veri birimini anlama](../azure-monitor/platform/manage-cost-storage.md#understanding-ingested-data-volume).

    ![Securityınsights çözümünü silme](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>Arka planda ne olur?

Çözümü sildiğinizde, Azure Sentinel, silme işleminin ilk aşamasını tamamlayacak 48 saate kadar sürer.

Bağlantı kesilmesi tanımlandıktan sonra, çıkarma işlemi başlar.

**Bu bağlayıcıların yapılandırması silinir:**
-   Office 365

-   AWS

-   Microsoft Hizmetleri güvenlik uyarıları (Azure ATP, Cloud Discovery gölge BT raporlama, Azure AD Kimlik Koruması, Microsoft Defender ATP, Azure Güvenlik Merkezi) dahil Microsoft Cloud App Security

-   Tehdit Bilgisi

-   Ortak Güvenlik günlükleri (CEF tabanlı Günlükler, Barkcuda ve syslog dahil) (Azure Güvenlik Merkezi 'Ne sahipseniz, bu Günlükler toplanmaya devam eder.)

-   Windows güvenlik olayları (Azure Güvenlik Merkezi 'Ne sahipseniz, bu Günlükler toplanmaya devam eder.)

İlk 48 saat içinde, veri ve uyarı kuralları (gerçek zamanlı Otomasyon yapılandırması dahil) artık Azure Sentinel 'de erişilebilir veya sorgulanabilir olmayacaktır.

**Bu kaynaklar 30 gün sonra silinir:**

-   Olaylar (araştırma meta verileri dahil)

-   Uyarı kuralları

-   leriniz

Playbooks, kaydettiğiniz çalışma kitapları, kaydetme sorguları ve Not defterleri silinmez. **Kaldırılan veriler nedeniyle bazıları kesilebilir. Bunları el ile kaldırabilirsiniz.**

Hizmeti kaldırdıktan sonra, çözümü yeniden etkinleştirebilmeniz için 30 günlük bir yetkisiz kullanım süresi vardır ve veri ve uyarı kuralları geri yüklenir, ancak bağlantısı kesilen yapılandırılmış bağlayıcıların yeniden bağlanması gerekir.

> [!NOTE]
> Çözümü kaldırırsanız aboneliğiniz Azure Sentinel kaynak sağlayıcısı ile kaydettirilmeye devam edecektir. **El ile kaldırabilirsiniz.**




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel hizmetini kaldırmayı öğrendiniz. Fikrinizi değiştirirseniz ve yeniden yüklemek istiyorsanız:
- [Azure Sentinel 'i oluşturma ile](quickstart-onboard.md)çalışmaya başlayın.

