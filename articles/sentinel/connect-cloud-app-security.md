---
title: Cloud App Security verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: MCAS 'den Azure Sentinel 'e uyarıları ve Cloud Discovery günlüklerini akışa almak için Microsoft Cloud App Security (MCAS) bağlayıcısını kullanmayı öğrenin. 
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2020
ms.author: yelevin
ms.openlocfilehash: 8439c8f7aa4e75abd727d2ce2e80d98e6fce5411
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563953"
---
# <a name="connect-data-from-microsoft-cloud-app-security"></a>Microsoft Cloud App Security verileri bağlama 

[Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (MCAS) Bağlayıcısı, MCAS 'deki uyarıları ve [Cloud Discovery günlüklerini](https://docs.microsoft.com/cloud-app-security/tutorial-shadow-it) Azure Sentinel 'e aktarmanıza olanak tanır. Bu, bulut uygulamalarınıza ilişkin görünürlük elde etmeniz, siber tehditleri belirleyip öğrenme konusunda gelişmiş analizler elde etmeniz ve verilerinizin nasıl ele alınacağını denetlemenizi sağlar.

## <a name="prerequisites"></a>Ön koşullar

- Kullanıcı çalışma alanında okuma ve yazma izinlerine sahip olmalıdır.
- Kullanıcılarınızın, çalışma alanının kiracısında genel yönetici veya Güvenlik Yöneticisi izinlerine sahip olması gerekir.
- Azure Sentinel 'de Cloud Discovery günlüklerini akışa almak için, [Microsoft Cloud App Security olarak Azure Sentinel 'i etkinleştirin](https://aka.ms/AzureSentinelMCAS).

> [!IMPORTANT]
> Cloud Discovery günlüklerin alımı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
## <a name="connect-to-cloud-app-security"></a>Cloud App Security Bağlan

Zaten Cloud App Security varsa, [Ağınızda etkinleştirildiğinden](https://docs.microsoft.com/cloud-app-security/getting-started-with-cloud-app-security)emin olun.
Cloud App Security dağıtılırsa ve verileriniz geri alıyorsa, uyarı verileri kolayca Azure Sentinel 'e akışla eklenebilir.


1. Azure Sentinel gezinti menüsünde **veri bağlayıcıları**' nı seçin. Bağlayıcılar listesinden **Microsoft Cloud App Security** kutucuğuna ve ardından sağ alt köşedeki **bağlayıcı sayfasını aç** düğmesine tıklayın.

1. Azure Sentinel 'e hangi günlükleri göndermek istediğinizi seçin; **Uyarılar** ve **Cloud Discovery günlükleri** (Önizleme) seçeneğini belirleyebilirsiniz. 

1. **Değişiklikleri Uygula**' ya tıklayın.

1. Azure Güvenlik Merkezi 'ndeki uyarıların Azure Sentinel 'de otomatik olarak olay oluşturmasını isteyip istemediğinizi seçebilirsiniz. **Olayları oluştur**altında, otomatik olarak uyarılardan olayları oluşturan varsayılan analiz kuralını açmak için **etkin** ' i seçin. Daha sonra bu kuralı, **etkin kurallar** sekmesinde **analiz**altında düzenleyebilirsiniz.

1. Log Analytics Cloud App Security uyarılar için ilgili şemayı kullanmak için, `SecurityAlert` sorgu penceresine yazın. Cloud Discovery logs şeması için, yazın `McasShadowItReporting` .

> [!NOTE]
> Cloud Discovery, kullanıcıların bulut uygulamalarına yönelik verileri temel alan verileri toplayarak eğilimleri algılamaya ve belirlemesine yardımcı olur.
>
> Cloud Discovery veriler günlük olarak toplandığından, en son verilerin 24 saate kadar olan değerinde Azure Sentinel 'e yansıtılmadığını unutmayın. Düşük düzey bir araştırmanın daha hızlı veri gerektirmesi durumunda, doğrudan kaynak gereç veya ham verilerin bulunduğu hizmette yapılmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Microsoft Cloud App Security Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Yerleşik](tutorial-detect-threats.md) veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri algılamaya başlayın.
