---
title: Forcepoint ürünlerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Forcepoint ürünlerini Azure Sentinel'e nasıl bağlayacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588238"
---
# <a name="connect-your-forcepoint-products-to-azure-sentinel"></a>Forcepoint ürünlerinizi Azure Sentinel'e bağlayın

> [!IMPORTANT]
> Azure Sentinel'deki Forcepoint ürünleri veri bağlayıcısı şu anda genel önizlemededir. Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.


Bu makalede, Forcepoint ürünlerinizi Azure Sentinel'e nasıl bağlayacağınız açıklanmaktadır. 

Forcepoint veri konektörleri, Forcepoint Cloud Access Security Broker ve Forcepoint Yeni Nesil Güvenlik Duvarı günlüklerini Azure Sentinel ile bağlamanızı sağlar. Bu şekilde, kullanıcı tanımlı günlükleri Azure Sentinel'e gerçek zamanlı olarak otomatik olarak dışa aktarabilirsiniz. Konektör, Forcepoint ürünleri tarafından kaydedilen kullanıcı etkinliklerinde daha zengin görünürlük sağlar. Ayrıca, Azure iş yüklerinden ve diğer akışlardan elde edilen verilerle daha fazla korelasyon sağlar ve Azure Sentinel içindeki Çalışma Kitapları ile izleme yeteneğini geliştirir.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.



## <a name="forward-forcepoint-product-logs-to-the-syslog-agent"></a>Forward Forcepoint ürün günlükleri Syslog acentesine 

Forcepoint ürününüzü CEF formatında Syslog iletilerini Syslog aracısı aracılığıyla Azure çalışma alanınıza iletecek şekilde yapılandırın.

1. Forcepoint ürününü, aşağıdaki yükleme kılavuzlarında açıklandığı gibi Azure Sentinel entegrasyonuna ayarlayın:
 - [Forcepoint CASB Entegrasyon Kılavuzu](https://frcpnt.com/casb-sentinel)
 - [Forcepoint NGFW Entegrasyon Kılavuzu](https://frcpnt.com/ngfw-sentinel)

2. DeviceVendor adı ile Log Analytics'teki ilgili şemayı kullanmak için CommonSecurityLog'u arayın 'Forcepoint' içerir. 

3. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)



## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Forcepoint ürünlerini Azure Sentinel'e nasıl bağlayacağınızı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.

- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)