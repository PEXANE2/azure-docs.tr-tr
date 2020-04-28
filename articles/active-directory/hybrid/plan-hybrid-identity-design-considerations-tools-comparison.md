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
ms.openlocfilehash: 052d99a819aee415d5e7ad6dc00b8c786af0f636
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80811077"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Karma Kimlik dizin tümleştirme araçları karşılaştırması
Yıllar içinde dizin tümleştirme araçları büyüme ve gelişim göstermiştir.  


- [FIM](https://docs.microsoft.com/previous-versions/windows/desktop/forefront-2010/ff182370%28v%3dvs.100%29) ve [MIM](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) desteklenmeye devam eder ve öncelikle şirket içi sistemler arasında eşitlemeyi etkinleştirir.   [FIM Windows Azure AD Bağlayıcısı](https://docs.microsoft.com/previous-versions/mim/dn511001(v=ws.10)?redirectedfrom=MSDN) hem FIM 'de hem de MIM 'de desteklenir, ancak yeni dağıtımlar için önerilmez. MÜŞTERILER veya sap hcm gibi şirket içi kaynaklara sahip müşteriler, Active Directory Domain Services (AD DS) doldurmak için MIM kullanmalı ve ayrıca AD DS 'den Azure AD 'ye eşitlemek için Azure AD Connect eşitleme veya Azure AD Connect bulut sağlama kullanır.
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) , AD DS ormanları ve Azure AD arasında eşitleme için daha önce DirSync ve Azure AD eşitleme içinde Yayınlanan bileşenleri ve işlevleri içerir.  
- [Azure AD Connect bulut sağlama](../cloud-provisioning/what-is-cloud-provisioning.md) , AD DS Azure AD 'ye eşitleme için yeni bir Microsoft aracısıdır ve elde edilen şirketin ad ormanlarının üst şirketin ad ormanlarından yalıtıldığı birleşme ve Alım gibi senaryolar için faydalıdır.

Azure AD Connect eşitleme ve Azure AD Connect bulut sağlama arasındaki farklar hakkında daha fazla bilgi edinmek için bkz. [Azure AD Connect bulut sağlama nedir?](../cloud-provisioning/what-is-cloud-provisioning.md)

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

