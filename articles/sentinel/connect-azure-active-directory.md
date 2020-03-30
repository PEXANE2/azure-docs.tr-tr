---
title: Azure AD verilerini Azure Sentinel'e bağlayın | Microsoft Dokümanlar
description: Azure Active Directory verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0a8f4a58-e96a-4883-adf3-6b8b49208e6a
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: be9241a6156621d3f90dbab2da5bebeb463b4232
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588629"
---
# <a name="connect-data-from-azure-active-directory"></a>Azure Active Directory'den veri bağlama



Azure Sentinel, Azure Active [Directory'den](../active-directory/fundamentals/active-directory-whatis.md) veri toplamanızı ve Azure Sentinel'e aktarabilmenizi sağlar. [Oturum açma günlüklerini](../active-directory/reports-monitoring/concept-sign-ins.md) ve denetim [günlüklerini](../active-directory/reports-monitoring/concept-audit-logs.md) akışı seçebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Active Directory'den oturum açma verilerini dışa aktarmak istiyorsanız, bir Azure AD P1 veya P2 lisansına sahip olmalısınız.

- Günlükleri aktarmak istediğiniz kiracıüzerinde global yönetici veya güvenlik yöneticisi izinleri olan kullanıcı.

- Bağlantı durumunu görebilmek için Azure AD tanı günlüklerine erişmek için izniniz olması gerekir. 


## <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma

1. Azure Sentinel'de **Veri bağlayıcılarını** seçin ve ardından **Azure Etkin Dizin** döşemesini tıklatın.

1. Azure Sentinel'de aktarmak istediğiniz günlüklerin yanında **Bağlan'ı**tıklatın.

1. Azure AD'deki uyarıların Azure Sentinel'de otomatik olarak olay oluşturmasını isteyip istemediğiniz konusunda seçim yapabilirsiniz. **Olayları Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralını etkinleştir'i **seçin.** Daha sonra bu kuralı **Analytics** ve ardından **Etkin kurallar**altında edinebilirsiniz.

1. Azure AD uyarıları için Log Analytics'teki ilgili şemayı kullanmak için **SigninLogs** ve **AuditLogs'u**arayın.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure AD'yi Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
