---
title: Forcepoint DLP'yi Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Forcepoint DLP'yi Azure Sentinel'e nasıl bağlayacağınızı öğrenin.
services: sentinel
author: yelevin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: yelevin
ms.openlocfilehash: 3bdb9d2b23ce05929ba5612e0c6a03fe1aab05de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588255"
---
# <a name="connect-your-forcepoint-dlp-to-azure-sentinel"></a>Forcepoint DLP'nizi Azure Sentinel'e bağlayın

> [!IMPORTANT]
> Azure Sentinel'deki Forcepoint Veri Kaybı Önleme (DLP) veri bağlayıcısı şu anda genel önizlemededir. Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.



Forcepoint DLP konektörü, DLP olay verilerini Azure Sentinel'e otomatik olarak dışa aktarmanızı sağlar. Kullanıcı etkinliklerinde ve veri kaybı olaylarında görünürlük elde etmek için kullanabilirsiniz. Ayrıca, Azure iş yüklerinden ve diğer akışlardan elde edilen verilerle korelasyon sağlar ve Azure Sentinel içindeki Çalışma Kitapları ile izleme yeteneğini geliştirir.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-forcepoint-dlp"></a>Forcepoint DLP'yi yapılandırma ve bağlama

[Forcepoint DLP Tümleştirme Kılavuzu'nda](https://frcpnt.com/dlp-sentinel)açıklandığı gibi, REST API aracılığıyla JSON formatındaki olay verilerini Azure çalışma alanınıza iletmek için Forcepoint DLP'yi yapılandırın.


## <a name="find-your-data"></a>Verilerinizi bulun

Forcepoint DLP konektörü kurulduktan sonra, veriler CustomLogs **ForcepointDLPEvents_CL**altında Log Analytics'te görünür.


Forcepoint DLP için Log Analytics'teki ilgili şemayı kullanmak için **ForcepointDLPEvents_CL.**


## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Forcepoint DLP'yi Azure Sentinel'e nasıl bağlayacağınızı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)