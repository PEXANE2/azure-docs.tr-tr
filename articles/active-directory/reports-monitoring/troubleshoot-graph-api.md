---
title: Azure Etkin Dizin raporlama API'sinde hata giderme | Microsoft Dokümanlar
description: Azure Etkin Dizin Raporlama API'lerini ararken hatalara çözüm sağlar.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d1fb4f49e4f9ad41f971d869873200e6180b5cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399294"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Azure Etkin Dizin raporlama API'sinde hata giderme

Bu makalede, Microsoft Graph API'sini kullanarak etkinlik raporlarına erişirken karşılaşabileceğiniz yaygın hata iletileri ve bunların çözümlemesi için adımlar listelenilmektedir.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Microsoft Graph V2 bitiş noktasına erişirken 500 HTTP dahili sunucu hatası

Şu anda Microsoft Graph v2 bitiş noktasını destekliyoruz - Microsoft Graph v1 bitiş noktasını kullanarak etkinlik günlüklerine erişebildiğimizden emin olun.

### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Hata: Ne kiracı B2C veya kiracı prim lisansı yok

Oturum açma raporlarına erişmek için Azure Active Directory 1 (P1) lisansı gerekir. Oturum açma işlemlerine erişirken bu hata iletisi görüyorsanız, kiracınızın Azure AD P1 lisansıyla lisans aldığından emin olun.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Hata: Kullanıcı izin verilen rollerde değil 

API'yi kullanarak denetim günlüklerine veya oturum açma işlemlerine erişmeye çalışırken bu hata iletisini görüyorsanız, hesabınızın Azure Etkin Dizin isiniz kiracınızdaki **Güvenlik Okuyucusu** veya **Rapor Okuyucu** rolünün bir parçası olduğundan emin olun. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Hata: Uygulama eksik AAD 'Okuma dizin verileri' izni 

Uygulamanızın doğru izin kümesiyle çalıştığını sağlamak için Azure [Active Directory raporlama API'sine erişmek için](howto-configure-prerequisites-for-reporting-api.md) lütfen Ön Koşullar'daki adımları izleyin. 

### <a name="error-application-missing-microsoft-graph-api-read-all-audit-log-data-permission"></a>Hata: Microsoft Graph API'si eksik uygulama 'Tüm denetim günlüğü verilerini okuma' izni

Uygulamanızın doğru izin kümesiyle çalıştığını sağlamak için Azure [Active Directory raporlama API'sine erişmek için](howto-configure-prerequisites-for-reporting-api.md) lütfen Ön Koşullar'daki adımları izleyin. 

## <a name="next-steps"></a>Sonraki Adımlar

[Denetim API referansını](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit)
[kullanma Oturum açma faaliyet raporu API referansını kullanma](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
