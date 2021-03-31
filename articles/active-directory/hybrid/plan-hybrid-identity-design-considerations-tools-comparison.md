---
title: 'Karma Kimlik: Dizin tümleştirme araçları karşılaştırması | Microsoft Belgeleri'
description: Bu sayfada, dizin tümleştirme için kullanılabilen çeşitli dizin tümleştirme araçlarını karşılaştıran kapsamlı bir tablo sunulmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/07/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70e5c7e5fa428d5a71ca1a7468bbaab2fc94078e
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101643856"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Karma Kimlik dizin tümleştirme araçları karşılaştırması
Yıllar içinde dizin tümleştirme araçları büyüme ve gelişim göstermiştir.  


- [FIM](/previous-versions/windows/desktop/forefront-2010/ff182370(v=vs.100)) ve [MIM](/microsoft-identity-manager/microsoft-identity-manager-2016) desteklenmeye devam eder ve öncelikle şirket içi sistemler arasında eşitlemeyi etkinleştirir.   [FIM Windows Azure AD Bağlayıcısı](/previous-versions/mim/dn511001(v=ws.10)) hem FIM 'de hem de MIM 'de desteklenir, ancak yeni dağıtımlar için önerilmez. MÜŞTERILER veya sap hcm gibi şirket içi kaynaklara sahip müşteriler, Active Directory Domain Services (AD DS) doldurmak için MIM kullanmalı ve ayrıca AD DS 'den Azure AD 'ye eşitlemek için Azure AD Connect eşitleme veya Azure AD Connect bulut sağlama kullanır.
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) , AD DS ormanları ve Azure AD arasında eşitleme için daha önce DirSync ve Azure AD eşitleme içinde Yayınlanan bileşenleri ve işlevleri içerir.  
- [Azure AD Connect bulut sağlama](../cloud-sync/what-is-cloud-sync.md) , AD DS Azure AD 'ye eşitleme için yeni bir Microsoft aracısıdır ve elde edilen şirketin ad ormanlarının üst şirketin ad ormanlarından yalıtıldığı birleşme ve Alım gibi senaryolar için faydalıdır.

Azure AD Connect eşitleme ve Azure AD Connect bulut sağlama arasındaki farklar hakkında daha fazla bilgi edinmek için bkz. [Azure AD Connect bulut sağlama nedir?](../cloud-sync/what-is-cloud-sync.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.