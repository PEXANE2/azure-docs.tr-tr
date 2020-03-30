---
title: Azure Sentinel'i Kaldır| Microsoft Dokümanlar
description: Azure Sentinel örneğini silme.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77581693"
---
# <a name="remove-azure-sentinel-from-your-workspace"></a>Azure Sentinel'i çalışma alanınızdan kaldırma

Azure Sentinel'i artık kullanmak istemiyorsanız, bu makalede çalışma alanınızdan nasıl kaldırılamayacağı açıklanmaktadır.

## <a name="how-to-remove-azure-sentinel"></a>Azure Sentinel nasıl kaldırılır?

Azure Sentinel'i çalışma alanınızdan kaldırmak için bu işlemi izleyin:

1. Azure **Sentinel'e**gidin , **ardından Ayarlar'ı**ve **Azure Sentinel'i Kaldır**sekmesini seçin.

1. Azure Sentinel'i kaldırmadan önce, neden kaldırdığınızı bize bildirmek için lütfen onay kutularını kullanın.

1. **Çalışma alanınızdan Azure Sentinel'i Kaldır'ı**seçin.
    
    ![SecurityInsights çözümünü silme](media/offboard/delete-solution.png)

## <a name="what-happens-behind-the-scenes"></a>Perde arkasında ne oluyor?

Çözümü kaldırdığınızda, Azure Sentinel'in silme işleminin ilk aşamasını tamamlaması 48 saat kadar sürer.

Bağlantı kopukluğu tanımlandıktan sonra, kapatma işlemi başlar.

**Bu bağlayıcıların yapılandırması kaldırılır:**
-   Office 365

-   AWS

-   Microsoft hizmetleri güvenlik uyarıları (Azure ATP, Cloud Discovery Shadow BT raporlaması dahil Microsoft Bulut Uygulama Güvenliği, Azure AD Kimlik Koruması, Microsoft Defender ATP, Azure Güvenlik Merkezi)

-   Tehdit Bilgisi

-   Ortak güvenlik günlükleri (CEF tabanlı günlükler, Barracuda ve Syslog dahil) (Azure Güvenlik Merkeziniz varsa, bu günlükler toplanmaya devam eder.)

-   Windows Güvenlik Etkinlikleri (Azure Güvenlik Merkeziniz varsa, bu günlükler toplanmaya devam eder.)

İlk 48 saat içinde, Azure Sentinel'de veri ve analitik kurallara (gerçek zamanlı otomasyon yapılandırması dahil) artık erişilemez veya sorgulanamanamaz.

**30 gün sonra bu kaynaklar kaldırılır:**

-   Olaylar (araştırma meta verileri dahil)

-   Analitik kurallar

-   Yer İşaretleri

Oyun kitaplarınız, kaydedilmiş çalışma kitaplarınız, kaydedilmiş av sorgularınız ve not defterleriniz kaldırılmaz. **Kaldırılan veriler nedeniyle bazıları kırılabilir. Bunları el ile kaldırabilirsiniz.**

Hizmeti kaldırdıktan sonra, çözümü yeniden etkinleştirebileceğiniz ve verilerinizin ve analitik kurallarınızın geri yüklendiği, ancak bağlantısı kesilen yapılandırılan konektörlerin yeniden bağlanması gereken 30 günlük bir yetkisiz kullanım süresi vardır.

> [!NOTE]
> Çözümü kaldırırsanız, aboneliğiniz Azure Sentinel kaynak sağlayıcısına kaydolmaya devam edecektir. **El ile kaldırabilirsiniz.**




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel hizmetini nasıl kaldırabileceğinizi öğrendiniz. Fikrinizi değiştirir ve yeniden yüklemek isterseniz:
- Azure [Sentinel'e binmeye](quickstart-onboard.md)başlayın.
