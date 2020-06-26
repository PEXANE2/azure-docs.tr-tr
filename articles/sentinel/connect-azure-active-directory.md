---
title: Azure Active Directory verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Active Directory verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: 5ec8a37a0c782a5426b87f785af2d047ca35aa22
ms.sourcegitcommit: dfa5f7f7d2881a37572160a70bac8ed1e03990ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85374823"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Azure Active Directory verileri bağlama (Azure AD)



Azure Sentinel, [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) verileri toplamanıza ve Azure Sentinel 'e akışla göndermenizi sağlar. [Oturum açma günlüklerinin](../active-directory/reports-monitoring/concept-sign-ins.md) ve [Denetim günlüklerinin](../active-directory/reports-monitoring/concept-audit-logs.md) akışını seçebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

- Azure AD 'den oturum açma verilerini dışarı aktarmak istiyorsanız, bir Azure AD P1 veya P2 lisansınızın olması gerekir.

- Günlüklerini akışa almak istediğiniz kiracı üzerinde genel yönetici veya Güvenlik Yöneticisi izinlerine sahip kullanıcı.

- Bağlantı durumunu görebilmek için Azure AD tanılama günlüklerine erişim izninizin olması gerekir. 


## <a name="connect-to-azure-active-directory"></a>Azure Active Directory Bağlan

1. Azure Sentinel 'de, gezinti menüsünden **veri bağlayıcıları** ' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure Active Directory** öğesini seçin ve ardından **bağlayıcı sayfası aç** düğmesine tıklayın.

1. Azure Sentinel 'e akışa almak istediğiniz günlüklerin yanındaki onay kutularını işaretleyin ve **Bağlan**' a tıklayın.

1. Azure AD 'deki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralı etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

1. Azure AD uyarılarını sorgulamak için Log Analytics içinde ilgili şemayı kullanmak için, `SigninLogs` `AuditLogs` sorgu penceresinde veya yazın.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Active Directory Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
