---
title: Denetleme noktası hedefli saldırı koruması (dokunarak) verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Azure Sentinel 'e yönelik denetleme noktası hedefli saldırı koruması (dokunarak) verilerine bağlanmayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: df4292a40335e58c64b8ebc50ffb6ac1ceee8836
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87059902"
---
# <a name="connect-your-proofpoint-tap-to-azure-sentinel-with-azure-function"></a>Azure Işleviyle Azure Sentinel 'e dokunarak, yazım noktanızı bağlayın

> [!IMPORTANT]
> Azure Sentinel 'de veri Bağlayıcısı 'nda bulunan yazım noktası dokunma noktası şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Düzeltme noktası hedeflenen saldırı koruması (TAP) Bağlayıcısı, tüm yazım ve Azure Sentinel ile güvenlik çözüm günlüklerine [dokunarak](https://www.proofpoint.com/us/products/advanced-threat-protection/targeted-attack-protection) panoları görüntülemenizi, özel uyarılar oluşturmayı ve araştırmayı geliştirmeyi sağlar. Redakvpoint TAP ve Azure Sentinel arasında tümleştirme, REST API kullanarak günlük verileri çekmek için Azure Işlevleri kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-proofpoint-tap"></a>Redaket noktası DOKUNMAYı yapılandırma ve bağlama

Azure Işlevleri, olayları ve günlükleri doğrudan yazım noktası dokunarak tümleştirebilir ve çekebilir ve bunları Azure Sentinel 'e iletebilir.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **REDAKTıPOINT dokunun** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Yazım için dokunma noktası dokunma** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **ProofpointTAPMessagesBlocked_CL**, **ProofpointTAPMessagesDelivered_CL**, **ProofpointTAPClicksPermitted_CL** ve **ProofpointTAPClicksBlocked_CL** tabloları altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Işlev uygulamalarını kullanarak, yazım ve Azure Sentinel 'e nasıl bağlanılacağını öğrenirsiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
