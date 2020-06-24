---
title: PıM-Azure AD 'de erişim gözden geçirmeleri için kaynak panoları | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) ' de bir erişim incelemesi gerçekleştirmek için bir kaynak panosunun nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e95eaa5b0e86a7470fc48edc23b2dbfb47e4b10c
ms.sourcegitcommit: 24f31287b6a526e23ff5b5469113522d1ccd4467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84743737"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-privileged-identity-management"></a>Privileged Identity Management bir erişim incelemesi gerçekleştirmek için bir kaynak panosu kullanın

Privileged Identity Management (PıM) içinde erişim incelemesi gerçekleştirmek için bir kaynak panosu kullanabilirsiniz. Azure Active Directory (Azure AD) içindeki yönetici görünümü panosu üç birincil bileşene sahiptir:

- Kaynak rolü etkinleştirmeleri grafik gösterimi
- Atama türüne göre rol atamalarının dağılımını görüntüleyen grafikler
- Yeni rol atamalarına bilgi içeren bir veri alanı

![Grafik ve grafiklerin gösterildiği yönetici görünümü panosunun ekran görüntüsü](media/pim-resource-roles-overview-dashboards/rbac-overview-top.png)

![Yönetici görünümü panosunun, veri listelerini gösteren ekran görüntüsü](media/pim-resource-roles-overview-dashboards/role-settings.png)

Kaynak rolü etkinleştirmeleri 'nin grafik temsili son yedi gün içinde yer alır. Bu veriler, seçilen kaynağın kapsamına alınır ve en yaygın rollerin (sahip, katkıda bulunan, Kullanıcı erişimi Yöneticisi) ve tüm rollerin birleştirilme için etkinleştirmeleri görüntüler.

Etkinleştirmeler grafiğinin bir tarafında, iki grafik rol atamalarının her iki Kullanıcı ve grup için atama türüne göre dağılımını görüntüler. Grafiğin bir dilimini seçerek değeri bir yüzde (veya tersi) olarak değiştirebilirsiniz.

Grafiklerin altında, son 30 gün içinde yeni rol atamaları olan Kullanıcı ve grup sayısı ve toplam atamalara göre azalan düzende sıralanan roller listelenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Privileged Identity Management Azure Kaynak rolleri için erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md)
