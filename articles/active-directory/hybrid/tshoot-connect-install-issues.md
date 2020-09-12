---
title: Azure AD Connect yüklemeleri sorunlarını giderme | Microsoft Docs '
description: Bu konuda Azure AD Connect yükleme ile ilgili sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 25314b4a306678dc877a95194907b3d73979e4f8
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275870"
---
# <a name="troubleshoot-azure-ad-connect-install-issues"></a>Sorun giderme: Azure AD Connect yüklemesi sorunları

## <a name="recommended-steps"></a>**Önerilen Adımlar**
Lütfen hangi [Azure AD Connect yükleme türünün](./how-to-connect-install-select-installation.md) sizin için uygun olduğunu denetleyin. Hızlı yükleme ölçütlerini karşılıyoruz, Hızlı yüklemeye gitmeniz önerilir. Hızlı yükleme, yüklemeyi bitirebilmeniz için gereken en az seçenek sağlar, bu nedenle herhangi bir sorunun daha az olma olasılığı vardır. 

Ancak, hızlı yükleme ölçütlerini karşılamıyorsa ve özel yükleme yapmanız gerekiyorsa, yaygın sorunlardan kaçınmak için izleyebileceğiniz bazı en iyi uygulamalar aşağıda verilmiştir. Kolaylık sağlaması için yalnızca seçmeli seçenekler burada belirtilmiştir:

* AAD Connect 'i yüklemekte olduğunuz makinede yönetici olduğunuzdan emin olun. Aynı yönetici kimlik bilgileriyle makinede oturum açın.

* Mevcut SQL Server kullanmak istiyorsanız, "var olan bir SQL Server kullan" dışında tüm seçeneklerin varsayılan olmasına izin verin. Özel yükleme seçeneklerini kullanma hakkında [daha fazla ayrıntı](./how-to-connect-install-custom.md) aşağıda verilmiştir. 

    ![Mevcut SQL Server kullan](media/tshoot-connect-install-issues/tshoot-connect-install-issues/useexistingsqlserver.png)

* Aşağıdaki sayfada, mevcut hesapla ilgili herhangi bir izin sorununu önlemek için "yeni AD hesabı oluştur" seçeneğini belirleyin.

    ![AD Ormanı hesabı](media/tshoot-connect-install-issues/tshoot-connect-install-issues/createnewaccount.png)

### <a name="common-issues"></a>**Genel Sorunlar**

* Şirket [içi Active Directory bağlantı sorunları](./reference-connect-adconnectivitytools.md).

* [Çevrimiçi Azure Active Directory bağlantı sorunları](./tshoot-connect-connectivity.md).

* Şirket [içi Active Directory izin sorunları](./how-to-connect-configure-ad-ds-connector-account.md).

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect Önkoşulları](./how-to-connect-install-prerequisites.md)
* [Azure AD Connect için hangi yükleme türünün kullanılacağını seçme](./how-to-connect-install-select-installation.md)
* [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](./how-to-connect-install-express.md)
* [Azure AD Connect özel yüklemesi](./how-to-connect-install-custom.md)
* [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](./how-to-upgrade-previous-version.md)
* [Azure AD Connect: hazırlama sunucusu nedir?](./plan-connect-topologies.md#staging-server)
* [ADConnectivityTool PowerShell Modülü nedir?](./how-to-connect-adconnectivitytools.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md).
- [Hibrit kimlik nedir?](whatis-hybrid-identity.md)