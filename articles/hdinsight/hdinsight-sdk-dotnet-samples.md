---
title: 'Azure HDInsight: .NET örnekleri'
description: .NET için HDInsight SDK'yı kullanarak sık kullanılan görevler için GitHub'da C# .NET örneklerini bulun.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: sample
ms.date: 12/06/2019
ms.openlocfilehash: 097323b4566daeda737428121bee4d64e9f7248b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74951573"
---
# <a name="azure-hdinsight-net-samples"></a>Azure HDInsight: .NET örnekleri

> [!div class="op_single_selector"]
> * [.NET Örnekleri](hdinsight-sdk-dotnet-samples.md)
> * [Python Örnekleri](hdinsight-sdk-python-samples.md)
> * [Java Örnekleri](hdinsight-sdk-java-samples.md)
<!-- * [Go Examples](hdinsight-sdk-go-samples.md)-->

Bu makalede aşağıdakiler sunulmaktadır:

* Küme oluşturma görevleri için örneklere bağlantılar.
* Diğer yönetim görevleri için başvuru içeriğine bağlantılar.

[Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisite"></a>Önkoşul

[.NET için Azure HDInsight SDK'sı](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight#sdk-installation)

## <a name="cluster-management---creation"></a>Küme yönetimi - oluşturma

* [Kafka kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateKafkaClusterSample.cs)
* [Spark kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateSparkClusterSample.cs)
* [Azure Veri Gölü Depolama Gen2 ile Bir Kıvılcım kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateHadoopClusterWithAdlsGen2Sample.cs)
* [Kurumsal Güvenlik Paketi (ESP) ile Bir Kıvılcım kümesi oluşturma](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples/blob/master/Management/Microsoft.Azure.Management.HDInsight.Samples/Microsoft.Azure.Management.HDInsight.Samples/CreateEspClusterSample.cs)

Bu örnekleri .NET için [hdinsight-dotnet-sdk-örnekleri](https://github.com/Azure-Samples/hdinsight-dotnet-sdk-samples) GitHub deposunu klonlayarak alabilirsiniz.

[!INCLUDE [hdinsight-sdk-additional-functionality](../../includes/hdinsight-sdk-additional-functionality.md)]

Bu ek SDK işlevselliği için kod parçacıkları [.NET başvuru belgeleri için HDInsight SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet)bulunabilir.
