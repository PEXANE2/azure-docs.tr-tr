---
title: Azure AD Raporlama API 'SI ile çalışmaya başlama | Microsoft Docs
description: Azure Active Directory Reporting API 'SI ile çalışmaya başlama
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: cf2e3f98bd96c09fe741a24a1332443c876e2e61
ms.sourcegitcommit: d68c72e120bdd610bb6304dad503d3ea89a1f0f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89225919"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Azure Active Directory Reporting API 'SI ile çalışmaya başlama

Azure Active Directory, SıEM sistemleri, denetim ve iş zekası araçları gibi uygulamalar için yararlı bilgiler içeren çeşitli [raporlar](overview-reports.md)sağlar. 

Azure AD raporları için Microsoft Graph API 'sini kullanarak, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim elde edebilirsiniz. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede, size erişme yolları dahil olmak üzere Raporlama API 'sine ilişkin bir genel bakış sunulmaktadır.

Sorunlarla karşılaşırsanız, bkz. [Azure Active Directory için destek alma](../fundamentals/active-directory-troubleshooting-support-howto.md).

## <a name="prerequisites"></a>Ön koşullar

Raporlama API 'sine erişmek için veya Kullanıcı müdahalesi olmadan, şunları yapmanız gerekir:

1. Rol atama (güvenlik okuyucusu, Güvenlik Yöneticisi, genel yönetici)
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını topla

Ayrıntılı yönergeler için [Azure Active Directory Raporlama API 'sine erişim önkoşulları](howto-configure-prerequisites-for-reporting-api.md)bölümüne bakın. 

## <a name="api-endpoints"></a>API uç noktaları 

Denetim günlükleri için Microsoft Graph API uç noktası `https://graph.microsoft.com/v1.0/auditLogs/directoryAudits` ve oturum açma işlemleri için MICROSOFT Graph API uç noktası `https://graph.microsoft.com/v1.0/auditLogs/signIns` . Daha fazla bilgi için bkz. [API başvurusunu denetleme](/graph/api/resources/directoryaudit?view=graph-rest-1.0) ve [oturum açma API başvurusu](/graph/api/resources/signIn?view=graph-rest-1.0).

Ayrıca, Microsoft Graph kullanarak güvenlik algılamalarından programlı erişim kazanmak için [kimlik koruması risk ALGıLAMA API](/graph/api/resources/identityriskevent?view=graph-rest-beta) 'sini de kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory kimlik koruması ve Microsoft Graph ile çalışmaya başlama](../identity-protection/howto-identity-protection-graph-api.md). 
  
## <a name="apis-with-microsoft-graph-explorer"></a>Microsoft Graph Explorer ile API 'Ler

Oturum açma ve denetim API 'SI verilerini doğrulamak için [Microsoft Graph Gezginini](https://developer.microsoft.com/graph/graph-explorer) kullanabilirsiniz. Graph Explorer Kullanıcı arabirimindeki oturum açma düğmelerinden her ikisini kullanarak hesabınızda oturum açıp **AuditLog. Read. All** ve **Directory. Read.** , kiracınız için gösterildiği gibi tüm izinleri ayarlayın.   

![Graph Explorer](./media/concept-reporting-api/graph-explorer.png)

![İzin Kullanıcı arabirimini değiştirme](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Azure AD Raporlama API 'sine erişmek için sertifikaları kullanma 

Raporlama verilerini kullanıcı müdahalesi olmadan almayı planlıyorsanız, Azure AD Raporlama API 'sini sertifikalarla birlikte kullanın.

Ayrıntılı yönergeler için bkz. [sertifikalarla Azure AD Raporlama API 'sini kullanarak veri edinme](tutorial-access-api-with-certificates.md).

## <a name="next-steps"></a>Sonraki adımlar

 * [Raporlama API 'sine erişim önkoşulları](howto-configure-prerequisites-for-reporting-api.md) 
 * [Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
 * [Azure AD Raporlama API 'sindeki hataların sorunlarını giderme](troubleshoot-graph-api.md)