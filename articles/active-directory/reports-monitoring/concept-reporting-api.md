---
title: Azure AD raporlama API'si | Microsoft Dokümanlar
description: Azure Etkin Dizin raporlama API'sine nasıl başlarsınız?
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
ms.openlocfilehash: 56f240a5191dd483f89889f3ffe13b1819ca1e53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399318"
---
# <a name="get-started-with-the-azure-active-directory-reporting-api"></a>Azure Active Directory raporlama API'si ile başlayın

Azure Active Directory, SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalar için yararlı bilgiler içeren çeşitli [raporlar](overview-reports.md)sağlar. 

Azure AD raporları için Microsoft Graph API'sini kullanarak, REST tabanlı API'ler kümesi aracılığıyla verilere programlı erişim elde edebilirsiniz. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede, raporlama API'sine erişim yolları da dahil olmak üzere genel bir bakış sağlar.

Sorunlarla karşılaştıysanız, [Azure Etkin Dizin imi için nasıl destek alacağınızı](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Raporlama API'sine kullanıcı müdahalesi ile veya kullanıcı müdahalesi olmadan erişmek için şunları yapmanız gerekir:

1. Roller atama (Güvenlik Okuyucusu, Güvenlik Yöneticisi, Genel Yönetici)
2. Bir uygulamayı kaydetme
3. İzinleri verme
4. Yapılandırma ayarlarını toplama

Ayrıntılı talimatlar [için, Azure Etkin Dizin raporlama API'sine erişmek için ön koşullara](howto-configure-prerequisites-for-reporting-api.md)bakın. 

## <a name="api-endpoints"></a>API Uç Noktaları 

Denetim günlükleri için Microsoft Graph API bitiş noktası ve `https://graph.microsoft.com/beta/auditLogs/directoryAudits` oturum açma için `https://graph.microsoft.com/beta/auditLogs/signIns`Microsoft Graph API bitiş noktasıdır. Daha fazla bilgi için [denetim API başvurusuna](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) ve [oturum açma API başvurusuna](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signIn)bakın.

Ayrıca, Microsoft Graph'ı kullanarak güvenlik algılamalarına programlı erişim elde etmek için [Kimlik Koruması risk algılamaları API'sini](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) kullanabilirsiniz. Daha fazla bilgi için Azure [Active Directory Identity Protection ve Microsoft Graph ile başlayın'](../identity-protection/graph-get-started.md)a bakın. 
  
## <a name="apis-with-microsoft-graph-explorer"></a>Microsoft Graph Explorer ile API'ler

Oturum açma ve denetleme API verilerinizi doğrulamak için [Microsoft Graph gezginini](https://developer.microsoft.com/graph/graph-explorer) kullanabilirsiniz. Graph Explorer UI'deki oturum açma düğmelerinin her ikisini de kullanarak hesabınızda oturum açtırıp **AuditLog.Read.All** ve **Directory.Read.All'ı** belirlediğinizden emin olun.   

![Graph Gezgini](./media/concept-reporting-api/graph-explorer.png)

![İzinleri değiştirme UI](./media/concept-reporting-api/modify-permissions.png)

## <a name="use-certificates-to-access-the-azure-ad-reporting-api"></a>Azure AD raporlama API'sine erişmek için sertifikaları kullanma 

Kullanıcı müdahalesi olmadan raporlama verilerini almayı planlıyorsanız, sertifikaları içeren Azure AD Raporlama API'sini kullanın.

Ayrıntılı talimatlar için [bkz.](tutorial-access-api-with-certificates.md)

## <a name="next-steps"></a>Sonraki adımlar

 * [Raporlama API'sine erişmek için ön koşullar](howto-configure-prerequisites-for-reporting-api.md) 
 * [Sertifikalarla Azure AD Raporlama API’sini kullanarak veri alma](tutorial-access-api-with-certificates.md)
 * [Azure AD raporlama API'sinde hata giderme](troubleshoot-graph-api.md)


