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
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811077"
---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Karma Kimlik dizin tümleştirme araçları karşılaştırması
Yıllar içinde dizin tümleştirme araçları büyüme ve gelişim göstermiştir.  


- [FIM](https://docs.microsoft.com/previous-versions/windows/desktop/forefront-2010/ff182370%28v%3dvs.100%29) ve [MIM](https://docs.microsoft.com/microsoft-identity-manager/microsoft-identity-manager-2016) hala desteklenir ve öncelikle şirket içi sistemler arasında eşitlemesini sağlar.   [FIM Windows Azure AD Bağlayıcısı](https://docs.microsoft.com/previous-versions/mim/dn511001(v=ws.10)?redirectedfrom=MSDN) hem FIM hem de MIM'de desteklenir, ancak yeni dağıtımlar için önerilmez - Notlar veya SAP HCM gibi şirket içi kaynakları olan müşteriler Active Directory Domain Services'ı (AD DS) doldurmak için MIM'i kullanmalı ve ardından AD DS'den Azure AD'ye senkronize etmek için Azure AD Connect eşitleme veya Azure AD Connect bulut sağlama yı da kullanmalıdır.
- [Azure AD Connect eşitlemi,](how-to-connect-sync-whatis.md) AD DS ormanları ile Azure AD arasında eşitleme için DirSync ve Azure AD Sync'de daha önce yayımlanmış bileşenleri ve işlevleri içerir.  
- [Azure AD Connect bulut sağlama,](../cloud-provisioning/what-is-cloud-provisioning.md) satın alınan şirketin AD ormanlarının ana şirketin AD ormanlarından izole edildiği birleşme ve satın alma gibi senaryolar için yararlı olan, AD DS'den Azure AD'ye senkronize etmek için yeni bir Microsoft aracısıdır.

Azure AD Connect eşitleme ve Azure AD Connect bulut sağlama arasındaki [What is Azure AD Connect cloud provisioning?](../cloud-provisioning/what-is-cloud-provisioning.md) farklar hakkında daha fazla bilgi edinmek için bkz.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.

