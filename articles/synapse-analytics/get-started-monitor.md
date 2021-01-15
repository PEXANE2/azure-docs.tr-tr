---
title: 'Öğretici: Azure SYNAPSE Analytics ile çalışmaya başlama-SYNAPSE çalışma alanınızı izleyin'
description: Bu öğreticide, SYNAPSE çalışma alanınızdaki etkinlikleri izlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.subservice: monitoring
ms.topic: tutorial
ms.date: 12/31/2020
ms.openlocfilehash: 8c0cdcad9a7803e0d8063362ca62887990045c86
ms.sourcegitcommit: f5b8410738bee1381407786fcb9d3d3ab838d813
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98209823"
---
# <a name="monitor-your-synapse-workspace"></a>SYNAPSE çalışma alanınızı izleme

Bu öğreticide, SYNAPSE çalışma alanınızdaki etkinlikleri izlemeyi öğreneceksiniz. SQL, Apache Spark ve işlem hatları için geçerli ve geçmiş etkinlikleri izleyebilirsiniz. 

## <a name="introduction-to-the-monitor-hub"></a>Izleyici hub 'ına giriş

SYNAPSE Studio 'Yu açın ve **izleyici** hub 'ına gidin. Burada, çalışma alanında yer alan tüm etkinliklerin geçmişini görebilir ve bunların şimdi etkin olduğunu görebilirsiniz. 

* **Tümleştirme** altında işlem hatlarını, Tetikleyicileri ve tümleştirme çalışma zamanlarını izleyebilirsiniz.
* **Etkinlikler**' ın altında Spark ve SQL etkinliklerini izleyebilirsiniz. 

## <a name="integration"></a>Tümleştirme

1. **Tümleştirme >** işlem hattına gidin. Bu görünümde, çalışma alanınızda bir işlem hattının her çalıştırılışında görebilirsiniz. 
1. Önceki adımda çalıştırdığınız işlem hattını bulun ve ayrıntılarını görüntülemek için **ardışık düzen adına** tıklayın.
1. SYNAPSE Studio 'nun en üstündeki **Içerik Haritası çubuğu** ' na tıklayın, önceki görünüme dönmek için **tüm işlem hattı çalıştırmaları** ' na tıklayın.

## <a name="apache-spark-activities"></a>Apache Spark etkinlikleri

1. **Apache Spark uygulamalar > etkinliklere** gidin. Artık çalışma alanınızda çalıştırılan veya çalıştırılan tüm Spark uygulamalarını görebilirsiniz.
1. Artık çalıştırmayan bir uygulama bulun ve **uygulamanın adına** tıklayın. Artık Spark uygulamasının ayrıntılarını görebilirsiniz.
1. Apache Spark hakkında bilgi sahibiyseniz **Spark geçmiş sunucusuna** tıklayarak standart Apache Spark geçmiş sunucusu kullanıcı arabirimini bulabilirsiniz.

## <a name="sql-activities"></a>SQL etkinlikleri

1. **SQL istekleri > etkinliklere** gidin.
1. Bu görünümde, SQL isteklerini görebilirsiniz.
1. **Havuz** filtresinden Izlenecek bir **Havuz** seçin. Artık, çalışmakta olan veya çalışma alanınızda çalıştırılmış olan tüm SQL isteklerini o havuzda görebilirsiniz.
1. Belirli bir SQL isteği bulun ve fareyi bu öğenin üzerine getirin. Üzerine geldiğinizde bir SQL komut dosyası simgesi görürsünüz.
1. SQL isteğinin tam metnini görmek için SQL komut dosyası simgesine tıklayın.

    > [!NOTE] 
    > Bir çalışma alanı etkin adanmış SQL Havuzu (eski adıyla SQL DW) ile SYNAPSE Studio aracılığıyla gönderilen SQL istekleri, Izleyici hub 'ında görüntülenebilir. Diğer tüm izleme etkinlikleri için Azure portal adanmış SQL Havuzu (eski adıyla SQL DW) izlemeye gidebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi merkezini keşfet](get-started-knowledge-center.md)
