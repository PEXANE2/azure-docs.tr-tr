---
title: PıM-Azure Active Directory 'de erişim incelemesi gerçekleştirmek için kaynak panosu kullanma | Microsoft Docs
description: Azure AD Privileged Identity Management (PıM) ' de bir erişim incelemesi gerçekleştirmek için bir kaynak panosunun nasıl kullanılacağını açıklar.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: pim
ms.date: 03/30/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e759ba47c16617aa1783ce6fb0e324aa62ee96d
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804121"
---
# <a name="use-a-resource-dashboard-to-perform-an-access-review-in-pim"></a>PıM 'de erişim incelemesi gerçekleştirmek için bir kaynak panosu kullanın

Azure Active Directory (Azure AD) Privileged Identity Management (PıM) ' de bir erişim incelemesi gerçekleştirmek için bir kaynak panosu kullanabilirsiniz. Yönetici görünümü panosu üç birincil bileşene sahiptir:

- Kaynak rolü etkinleştirmeleri grafik gösterimi.
- Atama türüne göre rol atamalarının dağılımını görüntüleyen iki grafik.
- Yeni rol atamalarıyla ilgili bir veri alanı.

![Grafik ve grafiklerin gösterildiği yönetici görünümü panosunun ekran görüntüsü](media/pim-resource-roles-overview-dashboards/rbac-overview-top.png)

![Yönetici görünümü panosunun, veri listelerini gösteren ekran görüntüsü](media/pim-resource-roles-overview-dashboards/role-settings.png)

Kaynak rolü etkinleştirmeleri 'nin grafik temsili son yedi gün içinde yer alır. Bu veriler, seçilen kaynağın kapsamına alınır ve en yaygın rollerin (sahip, katkıda bulunan, Kullanıcı erişimi Yöneticisi) ve tüm rollerin birleştirilme için etkinleştirmeleri görüntüler.

Etkinleştirmeler grafiğinin sağında, iki grafik rol atamalarının her iki Kullanıcı ve grup için atama türüne göre dağılımını görüntüler. Grafiğin bir dilimini seçerek değeri bir yüzde (veya tersi) olarak değiştirebilirsiniz.

Grafiklerin altında, son 30 gün içinde yeni rol atamalarına sahip kullanıcı ve grup sayısını ve toplam atamalara göre sıralanmış rollerin bir listesini (azalan sırada) görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

- [PIM hizmetinde Azure kaynak rolleri için erişim gözden geçirmesi başlatma](pim-resource-roles-start-access-review.md) 
