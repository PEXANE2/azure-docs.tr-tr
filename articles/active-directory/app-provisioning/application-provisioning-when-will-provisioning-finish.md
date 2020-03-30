---
title: Azure AD Galerisi uygulamasına kullanıcı sağlama saatleri veya daha uzun sürüyor
description: Başvurunuzun sağlanmasının neden beklediğinizden daha uzun sürdüğünü nasıl öğrenebilirsiniz?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb22d19733fbeb162fba55dd732c10e552879c78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522654"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Azure AD Galerisi uygulamasına kullanıcı sağlama süresi birkaç saat veya daha uzun sürüyor

Bir uygulama için otomatik sağlamayı etkinleştirmeye ilk etkinleştirildiğinde, Azure REKLAM dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak, başlangıç döngüsü 20 dakika ile birkaç saat arasında sürebilir. 

Sonraki eşitlemeler, sonraki eşitlemelerin performansını artırarak, ilk döngüden sonra her iki sistem durumunu temsil eden filigranları depolayan, ilk döngüden sonraki eşitlemelerden sonra daha hızlı olur.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansı nasıl artırılabilen

İlk döngü birkaç saatten fazla sürüyorsa, performansı artırmak için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsam filtreleri.** Kapsam filtreleri, kullanıcıları belirli öznitelik değerlerine göre filtreleyerek, sağlama hizmetinin Azure AD'den çıkardığı verileri ince ayarlaya biliyor. Kapsam filtreleri hakkında daha fazla bilgi için, [kapsam filtreleri ile Öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Sağlama ve Deprovisioning'i otomatikleştirin](user-provisioning.md)

