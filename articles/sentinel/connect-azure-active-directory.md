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
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2020
ms.author: yelevin
ms.openlocfilehash: 279f54c3de964580cc37d1288a6e1b7726348e10
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88208619"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Azure Active Directory verileri bağlama (Azure AD)



Azure Sentinel 'in yerleşik bağlayıcısını kullanarak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) verileri toplayıp Azure Sentinel 'e aktarabilirsiniz. Bağlayıcı, [oturum açma günlüklerini](../active-directory/reports-monitoring/concept-sign-ins.md) ve [Denetim günlüklerini](../active-directory/reports-monitoring/concept-audit-logs.md)akışla kullanmanıza olanak sağlar.

## <a name="prerequisites"></a>Ön koşullar


- Herhangi bir Azure AD lisansı (ücretsiz/O365/P1/P2), oturum açma günlüklerini Azure Sentinel 'e almak için yeterlidir. Azure Izleyici (Log Analytics) ve Azure Sentinel için ek gigabayt başına ücretler uygulanabilir.

- Kullanıcı çalışma alanında Azure Sentinel katılımcısı rolüne atanmalıdır.

- Kullanıcıların, günlüklerini akışını istediğiniz kiracıda genel yönetici veya güvenlik yöneticisi rollerine atanması gerekir.

- Bağlantı durumunu görebilmeniz için, Kullanıcı Azure AD Tanılama ayarları üzerinde okuma ve yazma izinlerine sahip olmalıdır. 


## <a name="connect-to-azure-active-directory"></a>Azure Active Directory Bağlan

1. Azure Sentinel 'de, gezinti menüsünden **veri bağlayıcıları** ' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure Active Directory** ' yi seçin ve ardından **bağlayıcı sayfasını aç**' ı seçin.

1. Azure Sentinel 'e akışa almak istediğiniz günlüklerin yanındaki onay kutularını işaretleyin ve **Bağlan**' a tıklayın.

1. Azure AD 'deki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analiz kuralını etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

1. Azure AD uyarılarını sorgulamak için Log Analytics içinde ilgili şemayı kullanmak için, `SigninLogs` `AuditLogs` sorgu penceresinde veya yazın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Active Directory Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
