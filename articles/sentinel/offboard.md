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
ms.date: 09/16/2020
ms.author: yelevin
ms.openlocfilehash: f9c400b55b0da47495db4f1ff4ceb86aa39fe2cc
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90885837"
---
# <a name="remove-azure-sentinel-from-your-workspace"></a>Azure Sentinel'i çalışma alanınızdan kaldırma

Artık Azure Sentinel 'i kullanmak istemiyorsanız, bu makalede çalışma alanınızdan nasıl kaldırılacağı açıklanır.

## <a name="how-to-remove-azure-sentinel"></a>Azure Sentinel 'i kaldırma

Çalışma alanınızdan Azure Sentinel 'i kaldırmak için bu işlemi izleyin:

1. **Azure Sentinel**' e ve ardından **Ayarlar**' a gidin ve **Azure Sentinel 'i kaldır** sekmesini seçin.

1. Azure Sentinel 'i kaldırmadan önce, neden kaldırdığınızı bize bildirmek için lütfen onay kutularını kullanın.

1. **Çalışma alanınızdan Azure Sentinel 'ı kaldır**' ı seçin.
    
    ![Securityınsights çözümünü silme](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>Arka planda ne olur?

Çözümü kaldırdığınızda, Azure Sentinel, silme işleminin ilk aşamasını tamamlayacak 48 saate kadar sürer.

Bağlantı kesilmesi tanımlandıktan sonra, çıkarma işlemi başlar.

**Bu bağlayıcıların yapılandırması kaldırılır:**
-   Office 365

-   AWS

-   Microsoft Hizmetleri güvenlik uyarıları: kimlik için Microsoft Defender (*Eski adıyla Azure ATP*) Microsoft Cloud App Security, Cloud DISCOVERY gölge BT raporlama, Azure AD kimlik koruması, uç nokta Için Microsoft Defender (*eski ADıYLA Microsoft Defender ATP*), Azure Güvenlik Merkezi 'ndeki Azure Defender uyarıları

-   Tehdit Bilgisi

-   Ortak Güvenlik günlükleri (CEF tabanlı Günlükler, Barkcuda ve syslog dahil) (Azure Defender uyarılarını Azure Güvenlik Merkezi 'nden alırsanız, bu Günlükler toplanmaya devam eder.)

-   Windows güvenlik olayları (Azure Güvenlik Merkezi 'nden Azure Defender uyarıları alırsanız, bu Günlükler toplanmaya devam eder.)

İlk 48 saat içinde veri ve analitik kuralları (gerçek zamanlı Otomasyon yapılandırması dahil) artık Azure Sentinel 'de erişilebilir veya sorgulanabilir olmayacaktır.

**Bu kaynaklar kaldırıldıktan 30 gün sonra:**

-   Olaylar (araştırma meta verileri dahil)

-   Analiz kuralları

-   Yer işaretleri

Playbook'larınız, kaydedilen çalışma kitaplarınız, kaydedilen avlanma sorgularınız ve not defterleriniz kaldırılmaz. **Kaldırılan veriler nedeniyle bazıları kesilebilir. Bunları el ile kaldırabilirsiniz.**

Hizmeti kaldırdıktan sonra, çözümü yeniden etkinleştirebilmeniz için 30 günlük bir yetkisiz kullanım süresi vardır ve veri ve analitik kuralları geri yüklenir, ancak bağlantısı kesilen yapılandırılmış bağlayıcıların yeniden bağlanması gerekir.

> [!NOTE]
> Çözümü kaldırırsanız aboneliğiniz Azure Sentinel kaynak sağlayıcısına kayıtlı olmaya devam eder. **Bunu el ile kaldırabilirsiniz.**




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel hizmetini kaldırmayı öğrendiniz. Fikrinizi değiştirirseniz ve yeniden yüklemek istiyorsanız:
- [Azure Sentinel 'i oluşturma ile](quickstart-onboard.md)çalışmaya başlayın.
