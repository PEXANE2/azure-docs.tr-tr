---
title: Azure Synapse Analytics ile Apache Spark için .NET'i kullanma
description: Toplu işlem, gerçek zamanlı akış, makine öğrenimi ve Azure Synapse Analytics dizüstü bilgisayarlarında geçici sorgular yazmak için .NET ve Apache Spark'ı kullanma hakkında bilgi edinin.
author: mamccrea
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: mamccrea
ms.reviewer: jrasnick
ms.openlocfilehash: 6943be033653d88b272817d2dcf58042aaaab616
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430519"
---
# <a name="use-net-for-apache-spark-with-azure-synapse-analytics"></a>Azure Synapse Analytics ile Apache Spark için .NET'i kullanma

[.NET for Apache Spark,](https://dot.net/spark) Spark için ücretsiz, açık kaynak kodlu ve çapraz platform .NET desteğidir. .NET for Apache Spark, Spark için C# ve F# üzerinden Spark API'lerine erişmenizi sağlayan .NET bağlamaları sağlar. Apache Spark için .NET ile ,NET'i kullanarak Spark için kullanıcı tanımlı işlevleri yazma ve yürütme olanağına sahipsiniz. Kıvılcım için .NET API'leri, Spark SQL ve Structured Streaming dahil olmak üzere verilerinizi çözümlemenize yardımcı olan Spark'ın tüm yönlerine erişmenizi sağlar.

Verileri .NET for Apache Spark için Spark toplu iş tanımları veya etkileşimli Azure Synapse Analytics dizüstü bilgisayarları aracılığıyla analiz edebilirsiniz. Bu makalede, her iki tekniği kullanarak Azure Synapse ile Apache Spark için .NET'i nasıl kullanacağınızı öğreneceksiniz. 

## <a name="submit-batch-jobs-using-the-spark-job-definition"></a>Spark iş tanımını kullanarak toplu iş gönderme

[Synapse Spark havuzları için Apache Spark iş tanımları oluşturmak için](apache-spark-job-definitions.md)Azure Synapse Analytics'i nasıl kullanacağınızı öğrenmek için öğreticiyi ziyaret edin. Uygulamanızı Azure Synapse'ye göndermek üzere paketlemediyseniz, aşağıdaki adımları tamamlayın.

1. Uygulamanızı yayınlamak için aşağıdaki komutları çalıştırın. *mySparkApp'ı* uygulamanıza giden yol ile değiştirdiğinizden emin olun.

   **Windows'da:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.0 -r ubuntu.16.04-x64
   ```

   **Linux üzerinde:**

### <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>.NET için Apache Spark, Azure Synapse Analytics dizüstü bilgisayarlarda

Yeni bir not defteri oluştururken, iş mantığınızı ifade etmek istediğiniz bir dil çekirdeği seçersiniz. C# dahil olmak üzere çeşitli diller için çekirdek desteği vardır.

Azure Synapse Analytics not defterinizde Apache Spark için .NET'i kullanmak için çekirdek olarak **.NET Spark 'ı (C#)** seçin ve not defterini mevcut bir Spark havuzuna takın.

   ```bash
   zip -r publish.zip
   ```

## <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>.NET için Apache Spark, Azure Synapse Analytics dizüstü bilgisayarlarda 

Dizüstü bilgisayarlar, .NET'inizi Apache Spark boru hatları ve senaryoları için prototiplemek için harika bir seçenektir. Verilerinizi hızlı ve verimli bir şekilde anlamaya, filtrelemeye, görüntülemeye ve görselleştirmeye başlayabilir, anlamaya, filtrelemeye ve görüntülemeye başlayabilirsiniz. Veri mühendisleri, veri bilimciler, iş analistleri ve makine öğrenimi mühendisleri paylaşılan, etkileşimli bir belge üzerinde işbirliği yapabiliyor. Veri aramanın hemen sonuçlarını görürsünüz ve verilerinizi aynı not defterinde görselleştirebilirsiniz.

### <a name="how-to-use-notebooks"></a>Not defterleri nasıl kullanılır?

Yeni bir not defteri oluşturduğunuzda, iş mantığınızı ifade etmek istediğiniz bir dil çekirdeği seçersiniz. C# dahil olmak üzere çeşitli diller için çekirdek desteği vardır. 

Azure Synapse Analytics not defterinizde Apache Spark için .NET'i kullanmak için çekirdek olarak **.NET Spark 'ı (C#)** seçin ve not defterini mevcut bir Spark havuzuna takın. 

.NET Spark dizüstü bilgisayar .NET etkileşimli deneyimleri temel alıyor ve önceden tanımlanmış Spark oturumu değişkeni `spark` ile kutunun dışında Spark için .NET'i kullanma olanağı yla etkileşimli C# deneyimleri sağlar.

### <a name="sparknet-c-kernel-features"></a>Spark.NET C# çekirdek özellikleri

Azure Synapse Analytics not defterinde Apache Spark için .NET'i kullandığınızda aşağıdaki özellikler kullanılabilir:

* Bildirimsel HTML: Üstbilgi, madde işaretli listeler ve hatta görüntüleri görüntülemek gibi HTML sözdizimini kullanarak hücrelerinizden çıktı oluşturun.
* Basit C# deyimleri (atamalar, konsola yazdırma, özel durumlar atma vb.)
* Çok satırlı C# kod blokları (if deyimleri, foreach döngüleri, sınıf tanımları vb. gibi).
* Standart C# kitaplığına erişim (System, LINQ, Enumerables vb. gibi).
* [C# 8.0 dil özellikleri](/dotnet/csharp/whats-new/csharp-8?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)desteği.
* Apache Spark oturumunuza erişmenizi sağlamak için önceden tanımlanmış bir değişken olarak 'kıvılcım'.
* [Apache Spark içinde çalıştırılabilen .NET kullanıcı tanımlı işlevleri](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql)tanımlama desteği.
* `XPlot.Plotly` Kitaplığı kullanarak farklı grafikler (satır, çubuk veya histogram gibi) ve düzenleri (tek, yer kakma vb. gibi) kullanarak Spark işlerinizden çıktıyı görselleştirme desteği.
* NuGet paketlerini C# dizüstü bilgisayarınıza ekleme olanağı.

## <a name="next-steps"></a>Sonraki adımlar

* [.NET Apache Spark belgeleri için](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
* [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
* [.NET Etkileşimli](https://devblogs.microsoft.com/dotnet/creating-interactive-net-documentation/)