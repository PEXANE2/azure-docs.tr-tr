---
title: 'Azure HDInsight: Java örnekleri'
description: Java için HDInsight SDK'yı kullanarak sık kullanılan görevler için GitHub'da Java örneklerini bulun.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: sample
ms.date: 11/29/2019
ms.openlocfilehash: e277814bda41a70f9ac8ff02b6e8018fba4b6f52
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74775372"
---
# <a name="azure-hdinsight-java-samples"></a>Azure HDInsight: Java örnekleri

> [!div class="op_single_selector"]
> * [Java Örnekleri](hdinsight-sdk-java-samples.md)
> * [.NET Örnekleri](hdinsight-sdk-dotnet-samples.md)
> * [Python Örnekleri](hdinsight-sdk-python-samples.md)
<!-- * [Go Examples](hdinsight-sdk-dotnet-samples.md)-->

Bu makalede aşağıdakiler sunulmaktadır:

* Küme oluşturma görevleri için örneklere bağlantılar.
* Diğer yönetim görevleri için başvuru içeriğine bağlantılar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[Java için Azure HDInsight SDK](https://docs.microsoft.com/java/api/overview/azure/hdinsight#sdk-installation)

## <a name="cluster-management---creation"></a>Küme yönetimi - oluşturma

* [Kafka kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateKafkaClusterSample.java)
* [Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateSparkClusterSample.java)
* [Azure Veri Gölü Depolama Gen2 ile Bir Kıvılcım kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateHadoopClusterWithAdlsGen2Sample.java)
* [Kurumsal Güvenlik Paketi (ESP) ile Bir Kıvılcım kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-java-sdk-samples/blob/master/management/src/main/java/com/microsoft/azure/hdinsight/samples/CreateEspClusterSample.java)

Bu örnekleri [hdinsight-java-sdk-örnekleri](https://github.com/Azure-Samples/hdinsight-java-sdk-samples) GitHub deposunu klonlayarak Java için alabilirsiniz.

[!INCLUDE [hdinsight-sdk-additional-functionality](../../includes/hdinsight-sdk-additional-functionality.md)]

Bu ek SDK işlevselliği için kod parçacıkları [Java başvuru belgeleri için HDInsight SDK](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-preview)bulunabilir.
