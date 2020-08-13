---
title: Azure Active Directory verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Active Directory verileri nasıl toplayacağınızı ve Azure AD oturum açma günlüklerini ve denetim günlüklerini Azure Sentinel 'de nasıl toplayacağınızı öğrenin.
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
ms.openlocfilehash: a19ced1cb9496bddd1f9aa7ea9e3eb58ba7eea3d
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185698"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Azure Active Directory verileri bağlama (Azure AD)



Azure Sentinel, [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) verileri toplamanıza ve Azure Sentinel 'e akışla göndermenizi sağlar. [Oturum açma günlüklerinin](../active-directory/reports-monitoring/concept-sign-ins.md) ve [Denetim günlüklerinin](../active-directory/reports-monitoring/concept-audit-logs.md) akışını seçebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Herhangi bir Azure AD lisansı (ücretsiz/O365/P1/P2), Azure ve Azure 'da oturum açma günlükleri ile tümleştirilebilir. Azure Izleyici veya Azure Sentinel için, gigabayt başına ek ücret uygulanır.

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
