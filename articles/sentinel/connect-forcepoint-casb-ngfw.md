---
title: Forcepoint ürünlerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Forcepoint ürünlerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: eb099a786a84f9b7d0a6f0dc6e6df9c3459af295
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77588238"
---
# <a name="connect-your-forcepoint-products-to-azure-sentinel"></a>Forcepoint ürünlerinizi Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'de Forcepoint Products veri Bağlayıcısı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Bu makalede, Forcepoint ürünlerinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. 

Forcepoint veri bağlayıcıları, Forcepoint Cloud Access Security Broker ve Forcepoint yeni nesil güvenlik duvarı günlüklerini Azure Sentinel ile bağlamanıza olanak tanır. Bu şekilde, Kullanıcı tanımlı günlükleri gerçek zamanlı olarak Azure Sentinel 'e otomatik olarak aktarabilirsiniz. Bağlayıcı, Forcepoint ürünleri tarafından kaydedilen Kullanıcı etkinliklerinin görünürlüğünü zenginleştirmenizi sağlar. Ayrıca Azure iş yüklerinden ve diğer akışlara ait verilerle daha fazla bağıntı sağlar ve Azure Sentinel içindeki çalışma kitapları ile izleme yeteneklerini geliştirir.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.



## <a name="forward-forcepoint-product-logs-to-the-syslog-agent"></a>Forcepoint ürün günlüklerini Syslog aracısına ilet 

Forcepoint ürününü, syslog Aracısı aracılığıyla CEF biçimindeki syslog iletilerini Azure çalışma alanınıza iletecek şekilde yapılandırın.

1. Forcepoint ürününü aşağıdaki yükleme kılavuzlarında açıklandığı şekilde Azure Sentinel tümleştirmesi ' ne ayarlayın:
 - [Forcepoint CASB tümleştirme Kılavuzu](https://frcpnt.com/casb-sentinel)
 - [Forcepoint NGFW tümleştirme Kılavuzu](https://frcpnt.com/ngfw-sentinel)

2. Devicevençi adı ' Forcepoint ' içeren Log Analytics ilgili şemayı kullanmak için CommonSecurityLog ' ı arayın. 

3. 3\. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.



## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Forcepoint ürünlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.

- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .