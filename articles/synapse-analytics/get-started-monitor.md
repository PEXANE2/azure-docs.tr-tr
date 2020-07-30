---
title: 'Öğretici: Azure SYNAPSE Analytics ile çalışmaya başlama-SYNAPSE çalışma alanınızı izleyin'
description: Bu öğreticide, SYNAPSE çalışma alanınızdaki etkinlikleri izlemeyi öğreneceksiniz.
services: synapse-analytics
author: saveenr
ms.author: saveenr
manager: julieMSFT
ms.reviewer: jrasnick
ms.service: synapse-analytics
ms.topic: tutorial
ms.date: 07/27/2020
ms.openlocfilehash: 43ed46f56be1ad4fa2aa643a6ed06f8fbe094a5b
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352888"
---
# <a name="monitor-your-synapse-workspace"></a>SYNAPSE çalışma alanınızı izleme

Bu öğreticide, SYNAPSE çalışma alanınızdaki etkinlikleri izlemeyi öğreneceksiniz. SQL, Apache Spark için geçerli ve geçmiş etkinliklerini izleyebilirsiniz. ve işlem hatları. 

## <a name="introduction-to-the-monitor-hub"></a>Izleyici hub 'ına giriş

SYNAPSE Studio 'Yu açın ve **izleyici** hub 'ına gidin. Burada, çalışma alanında yer alan tüm etkinliklerin geçmişini görebilir ve bunların şimdi etkin olduğunu görebilirsiniz. 

* **Düzenleme**bölümünde işlem hatlarını, Tetikleyicileri ve tümleştirme çalışma zamanlarını izleyebilirsiniz
* **Etkinlikler**' ın altında Spark ve SQL etkinliklerini izleyebilirsiniz. 

## <a name="orchestration"></a>Düzenleme

1. **Orchestration >** işlem hattına gidin. Bu görünümde, çalışma alanınızda bir işlem hattının her çalıştırılışında görebilirsiniz. 
1. Önceki adımda çalıştırdığınız işlem hattını bulun ve **ardışık düzen adına**tıklayın.
1. Artık söz konusu işlem hattının içindeki tek tek etkinliklerin nasıl çalışacağını görebilirsiniz.
1. SYNAPSE Studio 'nun en üstündeki **Içerik Haritası çubuğu** ' na tıklayın, önceki görünüme dönmek için **tüm işlem hattı çalıştırmaları** ' na tıklayın.

## <a name="apache-spark-activities"></a>Apache Spark etkinlikleri

1. **Apache Spark uygulamalar > Orchestration > etkinliklere**gidin. Artık çalışma alanınızda çalıştırılan veya çalıştırılan tüm Spark uygulamalarını görebilirsiniz.
1. Artık çalıştırmayan bir uygulama bulun ve **uygulamanın adına**tıklayın. Artık Spark uygulamasının ayrıntılarını görebilirsiniz.
1. Apache Spark hakkında bilgi sahibiyseniz **Spark geçmiş sunucusuna**tıklayarak standart Apache Spark geçmiş sunucusu kullanıcı arabirimini bulabilirsiniz.

## <a name="sql-activities"></a>SQL etkinlikleri

1. **SQL istekleri > Orchestration > etkinliklere**gidin.
1. Bu görünümde, SQL isteklerini görebilirsiniz.
1. İzlenecek bir **Havuz** seçin. Artık, çalışmakta olan veya çalışma alanınızda çalıştırılmış olan tüm SQL isteklerini o havuzda görebilirsiniz.
1. Belirli bir SQL isteği bulun ve fareyi bu öğenin üzerine getirin. Üzerine geldiğinizde bir SQL komut dosyası simgesi görürsünüz.
1. SQL isteğinin tam metnini görmek için SQL komut dosyası simgesine tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure SYNAPSE Analytics (çalışma alanları Önizleme)](overview-what-is.md)

