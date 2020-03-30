---
title: PIM'deki erişim incelemeleri için kaynak panoları - Azure AD | Microsoft Dokümanlar
description: Azure AD Ayrıcalıklı Kimlik Yönetimi'nde (PIM) bir erişim incelemesi gerçekleştirmek için kaynak panosunun nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6affa2ecc8919dabeb6173622b525280ce96bcfe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73847022"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-privileged-identity-management"></a>Ayrıcalıklı Kimlik Yönetimi'nde erişim incelemesi gerçekleştirmek için kaynak panosu kullanma

Ayrıcalıklı Kimlik Yönetimi'nde (PIM) erişim incelemesi gerçekleştirmek için kaynak panosu kullanabilirsiniz. Azure Etkin Dizini'ndeki (Azure AD) Yönetici Görünümü panosuüç ana bileşenden oluşur:

- Kaynak rol etkinleştirmelerinin grafikgösterimi
- Rol atamalarının atama türüne göre dağılımını gösteren grafikler
- Yeni rol atamalarıiçin bilgi içeren bir veri alanı

![Grafikleri ve grafikleri gösteren Yönetici Görünümü panosunun ekran görüntüsü](media/pim-resource-roles-overview-dashboards/rbac-overview-top.png)

![Veri listelerini gösteren Yönetici Görünümü panosunun ekran görüntüsü](media/pim-resource-roles-overview-dashboards/role-settings.png)

Kaynak rol etkinleştirmelerinin grafik gösterimi son yedi günü kapsar. Bu veriler seçili kaynağa yöneliktir ve en yaygın roller (sahibi, katkıda bulunan kişi, kullanıcı erişim yöneticisi) ve birleştirilmiş tüm roller için etkinleştirmeleri görüntüler.

Etkinleştirme grafiğin bir tarafında, iki grafik hem kullanıcılar hem de gruplar için rol atamalarının atama türüne göre dağılımını görüntüler. Grafiğin bir dilimini seçerek değeri yüzdeye (veya tam tersi) değiştirebilirsiniz.

Grafiklerin altında, son 30 gün içinde yeni rol atamaları olan kullanıcı ve grupların sayısı ve azalan sırada ki toplam atamalara göre sıralanmış roller listelenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ayrıcalıklı Kimlik Yönetimi'nde Azure kaynak rolleri için bir erişim incelemesi başlatma](pim-resource-roles-start-access-review.md)
