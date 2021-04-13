---
title: 'Öğretici: işlem hatlarla tümleştirmeye başlayın'
description: Bu öğreticide, SYNAPSE Studio kullanarak işlem hatlarını ve etkinlikleri tümleştirmeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: pipeline
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 22631cfd872ed226fc78a97d38c423fbc300f78e
ms.sourcegitcommit: b4fbb7a6a0aa93656e8dd29979786069eca567dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2021
ms.locfileid: "107304724"
---
# <a name="integrate-with-pipelines"></a>İşlem hatları ile tümleştirme

Bu öğreticide, SYNAPSE Studio kullanarak işlem hatlarını ve etkinlikleri tümleştirmeyi öğreneceksiniz. 

## <a name="create-a-pipeline-and-add-a-notebook-activity"></a>İşlem hattı oluşturma ve Not defteri etkinliği ekleme



1. SYNAPSE Studio 'da **tümleştirin** hub 'a gidin.
1. **+**  >  Yeni bir işlem hattı oluşturmak için işlem **hattı** ' nı seçin. Yeni işlem hattı nesnesini tıklatarak işlem hattı tasarımcısını açın.
1. **Etkinlikler**' ın altında **SYNAPSE** klasörünü genişletin ve bir **Not defteri** nesnesini tasarımcıya sürükleyin.
1. Not defteri etkinlik özelliklerinin **Ayarlar** sekmesini seçin. Geçerli SYNAPSE çalışma alanınızdan herhangi bir not defteri seçmek için açılan listeyi kullanın.

## <a name="schedule-the-pipeline-to-run-every-hour"></a>İşlem hattını her saat çalışacak şekilde zamanlama

1. İşlem hattında, yeni **tetikleyici Ekle**  >  **/Düzenle**' yi seçin.
1. **Tetikleyiciyi seçin**' de, **Yeni**' yi seçin ve **yinelemeyi** "her 1 saat" olarak ayarlayın.
1. **Tamam**’ı seçin. 
1. **Tümünü Yayımla**. 


## <a name="monitor-pipeline"></a>İşlem hattını izleme

1. İşlem hattı yayımlandıktan sonra, bir sonraki saat beklemek zorunda kalmadan işlem hattını hemen çalıştırmak için, şimdi **tetikleyici tetikleyicisi Ekle**' yi seçin  >  .
1. SYNAPSE Studio 'da **izleyici** hub 'ına gidin ve işlem hattı yürütme ilerlemesini izlemek Için işlem **hattı çalıştırmaları** ' nı seçin.



## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Power BI ile verileri görselleştirme](get-started-visualize-power-bi.md)
