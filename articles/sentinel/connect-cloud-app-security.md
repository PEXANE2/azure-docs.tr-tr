---
title: Cloud App Security verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Cloud App Security verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 6e71028855344d083dedf3493682e1e27685de48
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240118"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security verileri bağlama 



[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) günlüklerini Azure Sentinel 'e tek bir tıklama ile akışla aktarabilirsiniz. Bu bağlantı, uyarıları Azure Sentinel 'e Cloud App Security akışını sağlar. 

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya Güvenlik Yöneticisi izinlerine sahip Kullanıcı

## <a name="connect-to-cloud-app-security"></a>Cloud App Security Bağlan

Zaten Cloud App Security varsa, [Ağınızda etkinleştirildiğinden](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)emin olun.
Cloud App Security dağıtılırsa ve verileriniz geri alıyorsa, uyarı verileri kolayca Azure Sentinel 'e akışla eklenebilir.


1. Azure Sentinel 'de **veri bağlayıcıları** ' nı seçin ve **Cloud App Security** kutucuğuna tıklayın.

1. Azure Sentinel 'e hangi günlükleri aktarmak istediğinizi seçin, **Uyarılar**' ı seçebilirsiniz. 

1. Microsoft Cloud App Security uyarıların otomatik olarak Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. Olayları **Oluştur** altında, bağlı güvenlik hizmetinde oluşturulan uyarılardan olayları otomatik olarak oluşturan varsayılan analitik kuralı etkinleştirmek için **Etkinleştir** ' i seçin. Daha sonra bu kuralı **analiz** ve ardından **etkin kurallar**altında düzenleyebilirsiniz.

1. **Bağlan**'a tıklayın.

1. Cloud App Security uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Microsoft Cloud App Security Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
