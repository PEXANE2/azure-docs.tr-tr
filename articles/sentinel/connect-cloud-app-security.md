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
ms.date: 10/23/2019
ms.author: rkarlin
ms.openlocfilehash: 95e38cee8f6995e09dbbb2194cd5a9d0ebc301c1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73498662"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security verileri bağlama 



[Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) günlüklerini Azure Sentinel 'e tek bir tıklama ile akışla aktarabilirsiniz. Bu bağlantı, uyarıları Azure Sentinel 'e Cloud App Security akışını sağlar. 

## <a name="prerequisites"></a>Önkoşullar

- Genel yönetici veya Güvenlik Yöneticisi izinlerine sahip Kullanıcı
- Azure Sentinel 'de Cloud Discovery günlüklerini akışa almak için, [Microsoft Cloud App Security olarak Azure Sentinel 'i etkinleştirin](aka.ms. https://aka.ms/AzureSentinelMCAS).

> [!IMPORTANT]
> Cloud Discovery günlüklerin alımı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="connect-to-cloud-app-security"></a>Cloud App Security Bağlan

Zaten Cloud App Security varsa, [Ağınızda etkinleştirildiğinden](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)emin olun.
Cloud App Security dağıtılırsa ve verileriniz geri alıyorsa, uyarı verileri kolayca Azure Sentinel 'e akışla eklenebilir.


1. Azure Sentinel 'de **veri bağlayıcıları**' nı, **Cloud App Security** kutucuğunu ve **bağlayıcı sayfasını aç**' ı seçin.

1. Azure Sentinel 'e hangi günlükleri aktarmak istediğinizi seçin, **Uyarılar** ve **Cloud Discovery günlükleri** (Önizleme) seçeneğini belirleyebilirsiniz. 

1. **Bağlan**'a tıklayın.

1. Cloud App Security uyarıları için Log Analytics ilgili şemayı kullanmak için, **Securityalert**aratın.




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Microsoft Cloud App Security Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
