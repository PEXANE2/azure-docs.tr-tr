---
title: Azure AD verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Active Directory verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
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
ms.author: rkarlin
ms.openlocfilehash: f08cd731e40b204d042e5df418b03626b9082c3b
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894652"
---
# <a name="connect-data-from-azure-active-directory"></a>Azure Active Directory verileri bağlama



Azure Sentinel, [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) verileri toplamanıza ve Azure Sentinel 'e akışla göndermenizi sağlar. [Oturum açma günlüklerinin](../active-directory/reports-monitoring/concept-sign-ins.md) ve [Denetim günlüklerinin](../active-directory/reports-monitoring/concept-audit-logs.md) akışını seçebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Active Directory oturum açma verilerini dışarı aktarmak istiyorsanız, bir Azure AD P1 veya P2 lisansına sahip olmanız gerekir.

- Günlüklerini akışa almak istediğiniz kiracı üzerinde genel yönetici veya Güvenlik Yöneticisi izinlerine sahip kullanıcı.

- Bağlantı durumunu görebilmek için Azure AD tanılama günlüklerine erişim izninizin olması gerekir. 


## <a name="connect-to-azure-ad"></a>Azure AD'ye Bağlanma

1. Azure Sentinel 'de **veri bağlayıcıları** ' nı seçin ve **Azure Active Directory** kutucuğuna tıklayın.

1. Azure Sentinel 'e akışa almak istediğiniz günlüklerin yanında **Bağlan**' a tıklayın.

1. Azure AD 'deki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralı etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

1. Azure AD uyarıları için Log Analytics ilgili şemayı kullanmak için, **Signınlogs** ve **auditlogs**için arama yapın.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure AD 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
