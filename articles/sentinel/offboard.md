---
title: Azure Sentinel 'i kaldır | Microsoft Docs
description: Azure Sentinel örneğinizi silme.
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
ms.date: 12/29/2019
ms.author: yelevin
ms.openlocfilehash: 4042f7b9d0f6fdf293f338c89d5783fe266c4edf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77581693"
---
# <a name="remove-azure-sentinel-from-your-workspace"></a>Azure Sentinel 'i çalışma alanınızdan kaldırma

Artık Azure Sentinel 'i kullanmak istemiyorsanız, bu makalede çalışma alanınızdan nasıl kaldırılacağı açıklanır.

## <a name="how-to-remove-azure-sentinel"></a>Azure Sentinel 'i kaldırma

Çalışma alanınızdan Azure Sentinel 'i kaldırmak için bu işlemi izleyin:

1. **Azure Sentinel**' e ve ardından **Ayarlar**' a gidin ve **Azure Sentinel 'i kaldır**sekmesini seçin.

1. Azure Sentinel 'i kaldırmadan önce, neden kaldırdığınızı bize bildirmek için lütfen onay kutularını kullanın.

1. **Çalışma alanınızdan Azure Sentinel 'ı kaldır**' ı seçin.
    
    ![Securityınsights çözümünü silme](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>Arka planda ne olur?

Çözümü kaldırdığınızda, Azure Sentinel, silme işleminin ilk aşamasını tamamlayacak 48 saate kadar sürer.

Bağlantı kesilmesi tanımlandıktan sonra, çıkarma işlemi başlar.

**Bu bağlayıcıların yapılandırması kaldırılır:**
-   Office 365

-   AWS

-   Microsoft Hizmetleri güvenlik uyarıları (Azure ATP, Cloud Discovery gölge BT raporlama, Azure AD Kimlik Koruması, Microsoft Defender ATP, Azure Güvenlik Merkezi) dahil Microsoft Cloud App Security

-   Tehdit Bilgisi

-   Ortak Güvenlik günlükleri (CEF tabanlı Günlükler, Barkcuda ve syslog dahil) (Azure Güvenlik Merkezi 'Ne sahipseniz, bu Günlükler toplanmaya devam eder.)

-   Windows güvenlik olayları (Azure Güvenlik Merkezi 'Ne sahipseniz, bu Günlükler toplanmaya devam eder.)

İlk 48 saat içinde veri ve analitik kuralları (gerçek zamanlı Otomasyon yapılandırması dahil) artık Azure Sentinel 'de erişilebilir veya sorgulanabilir olmayacaktır.

**Bu kaynaklar kaldırıldıktan 30 gün sonra:**

-   Olaylar (araştırma meta verileri dahil)

-   Analitik kuralları

-   Yer İşaretleri

Playbooks, kaydettiğiniz çalışma kitapları, kaydetme sorguları ve Not defterleri kaldırılmaz. **Kaldırılan veriler nedeniyle bazıları kesilebilir. Bunları el ile kaldırabilirsiniz.**

Hizmeti kaldırdıktan sonra, çözümü yeniden etkinleştirebilmeniz için 30 günlük bir yetkisiz kullanım süresi vardır ve veri ve analitik kuralları geri yüklenir, ancak bağlantısı kesilen yapılandırılmış bağlayıcıların yeniden bağlanması gerekir.

> [!NOTE]
> Çözümü kaldırırsanız aboneliğiniz Azure Sentinel kaynak sağlayıcısı ile kaydettirilmeye devam edecektir. **El ile kaldırabilirsiniz.**




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel hizmetini kaldırmayı öğrendiniz. Fikrinizi değiştirirseniz ve yeniden yüklemek istiyorsanız:
- [Azure Sentinel 'i oluşturma ile](quickstart-onboard.md)çalışmaya başlayın.
