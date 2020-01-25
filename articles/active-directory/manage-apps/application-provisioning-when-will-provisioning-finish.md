---
title: Azure AD Galeri uygulamasına Kullanıcı hazırlama saati veya daha fazlasını sürüyor
description: Uygulamanıza yönelik sağlamasının neden beklediğinizden uzun sürüyor olabileceğini öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7d0c2586b129935043ae7b2eccd11cc3d65a385c
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76712082"
---
# <a name="user-provisioning-to-an-azure-ad-gallery-application-is-taking-hours-or-more"></a>Azure AD Galeri uygulamasına Kullanıcı hazırlama saati veya daha fazlasını sürüyor

İlk zaman bir uygulama için otomatik sağlamayı etkinleştirdiğinizde, Azure AD dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlı olarak, ilk zaman 20 dakikadan birkaç saate kadar sürebilir. 

İlk döngüden sonra sonraki eşitlemeler daha hızlı olur, çünkü sağlama hizmeti başlangıç döngüsünden sonra her iki sistemin durumunu temsil eden filigranları depolar, sonraki eşitlemeler performansını geliştirir.

## <a name="how-to-improve-provisioning-performance"></a>Sağlama performansını geliştirme

İlk döngüyü birkaç saatten fazla sürerse, performansı artırmak için yapabileceğiniz bir şey vardır:

-   **Kullanıcı kapsamı filtreleri.** Kapsam filtreleri, kullanıcıları belirli öznitelik değerlerine göre filtreleyerek, sağlama hizmetinin Azure AD 'den ayıklayan verileri ayarlamanıza olanak sağlar. Kapsam filtreleri hakkında daha fazla bilgi için bkz. [kapsam filtreleriyle öznitelik tabanlı uygulama sağlama](define-conditional-rules-for-provisioning-user-accounts.md).

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)

