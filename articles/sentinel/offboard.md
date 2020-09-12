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
ms.openlocfilehash: 0d918de4669e1b4bac6c1775a813eb1d224f39cf
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89662018"
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

-   Microsoft Hizmetleri güvenlik uyarıları: kimlik için Microsoft Defender (*eskiden Azure ATP*) Microsoft Cloud App Security, Cloud DISCOVERY gölge BT raporlaması, Azure AD kimlik koruması, uç nokta Için Microsoft Defender (*eski ADıYLA Microsoft Defender ATP*), Azure Defender (*Eski adıyla Azure Güvenlik Merkezi*)

-   Tehdit Bilgisi

-   Ortak Güvenlik günlükleri (CEF tabanlı Günlükler, Barkcuda ve syslog dahil) (Azure Defender kullanıyorsanız, bu Günlükler toplanmaya devam eder.)

-   Windows güvenlik olayları (Azure Defender kullanıyorsanız, bu Günlükler toplanmaya devam eder.)

İlk 48 saat içinde veri ve analitik kuralları (gerçek zamanlı Otomasyon yapılandırması dahil) artık Azure Sentinel 'de erişilebilir veya sorgulanabilir olmayacaktır.

**Bu kaynaklar kaldırıldıktan 30 gün sonra:**

-   Olaylar (araştırma meta verileri dahil)

-   Analitik kuralları

-   Yer işaretleri

Playbooks, kaydettiğiniz çalışma kitapları, kaydetme sorguları ve Not defterleri kaldırılmaz. **Kaldırılan veriler nedeniyle bazıları kesilebilir. Bunları el ile kaldırabilirsiniz.**

Hizmeti kaldırdıktan sonra, çözümü yeniden etkinleştirebilmeniz için 30 günlük bir yetkisiz kullanım süresi vardır ve veri ve analitik kuralları geri yüklenir, ancak bağlantısı kesilen yapılandırılmış bağlayıcıların yeniden bağlanması gerekir.

> [!NOTE]
> Çözümü kaldırırsanız aboneliğiniz Azure Sentinel kaynak sağlayıcısı ile kaydettirilmeye devam edecektir. **El ile kaldırabilirsiniz.**




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel hizmetini kaldırmayı öğrendiniz. Fikrinizi değiştirirseniz ve yeniden yüklemek istiyorsanız:
- [Azure Sentinel 'i oluşturma ile](quickstart-onboard.md)çalışmaya başlayın.
