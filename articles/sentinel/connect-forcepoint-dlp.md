---
title: Forcepoint DLP 'yi Azure Sentinel 'e bağlama | Microsoft Docs
description: Forcepoint DLP 'yi Azure Sentinel 'e bağlamayı öğrenin.
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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77588255"
---
# <a name="connect-your-forcepoint-dlp-to-azure-sentinel"></a>Forcepoint DLP 'nizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'de Forcepoint veri kaybı önleme (DLP) veri Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).



Forcepoint DLP Bağlayıcısı, DLP olay verilerini otomatik olarak Azure Sentinel 'e aktarmanızı sağlar. Kullanıcı etkinliklerini ve veri kaybı olaylarını görünürlüğünü almak için kullanabilirsiniz. Ayrıca, Azure iş yüklerinden ve diğer akışlardaki verilerle bağıntıları sağlar ve Azure Sentinel içindeki çalışma kitapları ile izleme yeteneklerini geliştirir.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-forcepoint-dlp"></a>Forcepoint DLP 'yi yapılandırma ve bağlama

Forcepoint DLP [tümleştirme kılavuzunda](https://frcpnt.com/dlp-sentinel)açıklandığı gibi REST API aracılığıyla olay verilerini JSON biçiminde Azure çalışma alanınıza Iletmek Için FORCEPOINT DLP 'yi yapılandırın.


## <a name="find-your-data"></a>Verilerinizi bulun

Forcepoint DLP bağlayıcısını ayarladıktan sonra, veriler CustomLogs **ForcepointDLPEvents_CL**altında Log Analytics görüntülenir.


Forcepoint DLP için Log Analytics ilgili şemayı kullanmak için, **ForcepointDLPEvents_CL**aratın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Forcepoint DLP 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .