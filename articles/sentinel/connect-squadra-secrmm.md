---
title: Squadra Technologies secRMM verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Squadra Technologies secRMM verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.openlocfilehash: d904e51321870fb4b61a237c23e425034b76dc0b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588119"
---
# <a name="connect-your-squadra-technologies-secrmm-data-to-azure-sentinel"></a>Squadra Technologies secRMM verilerinizi Azure Sentinel'e bağlayın 

> [!IMPORTANT]
> Azure Sentinel'deki Squadra Technologies Security Çıkarılabilir Medya Yöneticisi (secRMM) veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.


Squadra Technologies secRMM konektörü, Squadra Technologies secRMM güvenlik çözüm günlüklerinizi Azure Sentinel ile kolayca bağlamanızı sağlar. Panoları görüntülemenize, özel uyarılar oluşturmanıza ve araştırmayı geliştirmenize olanak tanır. Bu konektör, USB çıkarılabilir depolama olayları hakkında bilgi verir. Squadra Technologies secRMM ve Azure Sentinel arasındaki tümleştirme, REST API'yi kullanır.


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-squadra-technologies-secrmm"></a>Squadra Technologies secRMM'yi yapılandırma ve bağlama 

Squadra Technologies secRMM günlükleri doğrudan Azure Sentinel'e entegre edebilir ve dışa aktarabilir.
1. Azure Sentinel portalında Veri bağlayıcılarını tıklatın ve Squadra Technologies secRMM'i seçin ve ardından bağlayıcı sayfasını açın.

2. Azure Sentinel'de Squadra secRMM verilerini almak [için Azure Sentinel için Squadra Technologies onboarding kılavuzunda](http://www.squadratechnologies.com/StaticContent/ProductDownload/secRMM/9.9.0.0/secRMMAzureSentinelAdministratorGuide.pdf) belirtilen adımları izleyin.   


## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra veriler CustomLogs secRMM_CL altında Log Analytics'te görünür.
Squadra Technologies secRMM için Log Analytics'teki ilgili şemayı kullanmak için secRMM_CL arayın.

## <a name="validate-connectivity"></a>Bağlantıyı doğrulama
Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir. 


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Squadra Technologies secRMM'yi Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

