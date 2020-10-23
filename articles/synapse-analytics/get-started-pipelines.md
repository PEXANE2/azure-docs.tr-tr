---
title: 'Öğretici: işlem hatlarla çalışmaya başlama'
description: Bu öğreticide, SYNAPSE Studio kullanarak işlem hatlarını ve etkinlikleri nasıl düzenleyeceğinizi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 10/16/2020
ms.openlocfilehash: 42d2ac6cf6592f8e22b0a66aee84c3436d466572
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92329892"
---
# <a name="orchestrate-with-pipelines"></a>İşlem hatları ile düzenleme

Bu öğreticide, SYNAPSE Studio kullanarak işlem hatlarını ve etkinlikleri nasıl düzenleyeceğinizi öğreneceksiniz. 

## <a name="overview"></a>Genel Bakış

Azure SYNAPSE 'te çok çeşitli görevleri düzenleyebilirsiniz.

1. SYNAPSE Studio 'da **tümleştirin** hub 'a gidin.
1. **+**  >  Yeni bir işlem hattı oluşturmak için işlem **hattı** ' nı seçin.
1. **Geliştirme** merkezine gidin ve daha önce oluşturduğunuz not defterlerinden birini seçin.
1. Bu Not defterini işlem hattına sürükleyin (**Not**: işlem hattınızdan çalışırken [belgede](https://docs.microsoft.com/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export#transfer-data-to-or-from-a-sql-pool-attached-with-the-workspace) belirtilen şekilde, not defterine içeri aktarma modülleri ekleyin)
1. İşlem hattında, yeni **tetikleyici Ekle**  >  **/Düzenle**' yi seçin.
1. **Tetikleyiciyi seçin**' de, **Yeni**' yi seçin ve **yinelemeyi** "her 1 saat" olarak ayarlayın.
1. **Tamam**’ı seçin. 
1. **Tümünü Yayımla**.
1. İşlem hattının hemen çalışmasını sağlamak için, bir sonraki saat beklemek zorunda kalmadan tetikleyici tetikleyicisi **Ekle**' yi seçin  >  **Trigger now**.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile verileri görselleştirme](get-started-visualize-power-bi.md)
                                 
