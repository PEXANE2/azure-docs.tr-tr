---
title: Azure AD Galeri uygulamasına Kullanıcı hazırlama saati veya daha fazlasını sürüyor
description: Uygulamanıza yönelik sağlamasının neden beklediğinizden uzun sürüyor olabileceğini öğrenin
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.openlocfilehash: d663962dab058a108faa7d5310a3f2892c345a75
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82593905"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Azure AD Galeri uygulamasına Kullanıcı hazırlama saati veya daha fazlasını sürüyor

İlk zaman bir uygulama için otomatik sağlamayı etkinleştirdiğinizde, Azure AD dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak, ilk zaman 20 dakikadan birkaç saate kadar sürebilir. 

İlk döngüden sonra sonraki eşitlemeler daha hızlı olur, çünkü sağlama hizmeti başlangıç döngüsünden sonra her iki sistemin durumunu temsil eden filigranları depolar, sonraki eşitlemeler performansını geliştirir.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansını geliştirme

İlk döngüyü birkaç saatten fazla sürerse, performansı artırmak için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsamı filtreleri.** Kapsam filtreleri, kullanıcıları belirli öznitelik değerlerine göre filtreleyerek, sağlama hizmetinin Azure AD 'den ayıklayan verileri ayarlamanıza olanak sağlar. Kapsam filtreleri hakkında daha fazla bilgi için bkz. [kapsam filtreleriyle öznitelik tabanlı uygulama sağlama](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS uygulamalarına Kullanıcı hazırlama ve sağlamayı kaldırma işlemlerini otomatikleştirme](user-provisioning.md)

