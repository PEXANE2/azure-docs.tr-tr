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
ms.date: 01/20/2021
ms.author: yelevin
ms.openlocfilehash: e84484990725b0c39b132aead51e9b01dbb7e7ef
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632300"
---
# <a name="connect-data-from-azure-active-directory-azure-ad"></a>Azure Active Directory verileri bağlama (Azure AD)

Azure Sentinel 'in yerleşik bağlayıcısını kullanarak [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) verileri toplayıp Azure Sentinel 'e aktarabilirsiniz. Bağlayıcı, [oturum açma günlüklerini](../active-directory/reports-monitoring/concept-sign-ins.md) ve [Denetim günlüklerini](../active-directory/reports-monitoring/concept-audit-logs.md)akışla kullanmanıza olanak sağlar.

## <a name="prerequisites"></a>Önkoşullar

- Herhangi bir Azure AD lisansı (ücretsiz/O365/P1/P2), oturum açma günlüklerini Azure Sentinel 'e almak için yeterlidir. Azure Izleyici (Log Analytics) ve Azure Sentinel için ek gigabayt başına ücretler uygulanabilir.

- Kullanıcı çalışma alanında Azure Sentinel katılımcısı rolüne atanmalıdır.

- Kullanıcıların, günlüklerini akışını istediğiniz kiracıda genel yönetici veya güvenlik yöneticisi rollerine atanması gerekir.

- Bağlantı durumunu görebilmeniz için, Kullanıcı Azure AD Tanılama ayarları üzerinde okuma ve yazma izinlerine sahip olmalıdır. 

## <a name="connect-to-azure-active-directory"></a>Azure Active Directory Bağlan

1. Azure Sentinel 'de, gezinti menüsünden **veri bağlayıcıları** ' nı seçin.

1. Veri bağlayıcıları galerisinden **Azure Active Directory** ' yi seçin ve ardından **bağlayıcı sayfasını aç**' ı seçin.

1. Azure Sentinel 'de akışa almak istediğiniz günlük türlerinin yanındaki onay kutularını işaretleyin ve **Bağlan**' a tıklayın. Bunlar, aralarından seçim yapabileceğiniz günlük türleridir:

    - Oturum açma günlükleri
    - Denetim günlükleri
    - Etkileşimli olmayan kullanıcı oturum açma günlükleri
    - Hizmet sorumlusu oturum açma günlükleri
    - Yönetilen kimlik oturum açma günlükleri
    - Sağlama günlükleri

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **günlüklerde**, **logmanagement** bölümünün altında aşağıdaki tablolarda görünür:

- `SigninLogs`
- `AuditLogs`
- `AADNonInteractiveUserSignInLogs`
- `AADServicePrincipalSignInLogs`
- `AADManagedIdentitySignInLogs`
- `AADProvisioningLogs`

Azure AD günlüklerini sorgulamak için, sorgu penceresinin üst kısmında ilgili tablo adını girin.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Azure Active Directory Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
