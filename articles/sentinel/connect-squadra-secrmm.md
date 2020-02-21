---
title: Squadra Technologies secRMM verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Squadra Technologies secRMM verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
author: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2020
ms.author: rkarlin
ms.openlocfilehash: 1e35cd4a5b27b87fd5a022c5e798f4590b0bba1c
ms.sourcegitcommit: 0a9419aeba64170c302f7201acdd513bb4b346c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77501244"
---
# <a name="connect-your-squadra-technologies-secrmm-data-to-azure-sentinel"></a>Squadra Technologies secRMM verilerinizi Azure Sentinel 'e bağlama 

> [!IMPORTANT]
> Azure Sentinel 'deki Squadra Teknolojileri Güvenlik çıkarılabilir medya Yöneticisi (secRMM) veri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Squadra Technologies secRMM Bağlayıcısı, Squadra Technologies secRMM güvenlik çözüm günlüklerinizi Azure Sentinel ile kolayca bağlamanıza olanak tanır. Panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmanızı sağlar. Bu bağlayıcı, USB çıkarılabilir depolama olayları hakkında öngörüler sağlar. Squadra Technologies secRMM ve Azure Sentinel arasındaki tümleştirme REST API kullanır.


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-squadra-technologies-secrmm"></a>Squadra Technologies secRMM 'yi yapılandırma ve bağlama 

Squadra Technologies secRMM, günlükleri doğrudan Azure Sentinel 'e tümleştirebilir ve dışarı aktarabilir.
1. Azure Sentinel portalında, veri bağlayıcıları ' na tıklayın ve Squadra Technologies secRMM ' yi seçin ve ardından bağlayıcı sayfası ' nı açın.

2. Azure Sentinel 'de Squadra secRMM verileri almak için [Azure Sentinel Için Squadra teknolojileri ekleme Kılavuzu](http://www.squadratechnologies.com/StaticContent/ProductDownload/secRMM/9.9.0.0/secRMMAzureSentinelAdministratorGuide.pdf) 'nda açıklanan adımları izleyin.   


## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler CustomLogs secRMM_CL altında Log Analytics görüntülenir.
Squadra Technologies secRMM için Log Analytics ilgili şemayı kullanmak için, secRMM_CL aratın.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula
Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Squadra Technologies secRMM 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

