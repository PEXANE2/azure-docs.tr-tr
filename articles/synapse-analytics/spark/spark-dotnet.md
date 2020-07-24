---
title: Apache Spark için .NET kullanın
description: Toplu işleme, gerçek zamanlı akış, makine öğrenimi ve Azure SYNAPSE Analytics not defterlerinde geçici sorgular yazmak için .NET ve Apache Spark kullanma hakkında bilgi edinin.
author: mamccrea
services: synapse-analytics
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: spark
ms.date: 05/01/2020
ms.author: mamccrea
ms.reviewer: jrasnick
ms.openlocfilehash: 677b54a09a446ef8af0b410faea9f94b71634712
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86999046"
---
# <a name="use-net-for-apache-spark-with-azure-synapse-analytics"></a>Azure SYNAPSE Analytics ile Apache Spark için .NET kullanın

[Apache Spark için .net](https://dot.net/spark) , Spark için ücretsiz, açık kaynak ve platformlar arası .NET desteği sağlar. 

C# ve F # aracılığıyla Spark API 'Lerine erişmenize olanak tanıyan Spark için .NET bağlamaları sağlar. Apache Spark için .NET sayesinde, .NET dilinde yazılmış Spark için Kullanıcı tanımlı işlevler yazma ve yürütme olanağınız da vardır. Spark için .NET API 'Leri, Spark SQL, Delta Gölü ve yapılandırılmış akış dahil olmak üzere verilerinizi çözümlemenize yardımcı olan Spark veri çerçevelerinin her yönüyle erişmenizi sağlar.

Spark Batch iş tanımları veya etkileşimli Azure SYNAPSE Analytics Not defterleri aracılığıyla Apache Spark için .NET ile verileri analiz edebilirsiniz. Bu makalede her iki tekniği kullanarak Azure SYNAPSE Apache Spark için .NET kullanmayı öğreneceksiniz.

## <a name="submit-batch-jobs-using-the-spark-job-definition"></a>Spark iş tanımını kullanarak Batch işlerini gönderme

[SYNAPSE Spark havuzları için Apache Spark iş tanımları oluşturmak](apache-spark-job-definitions.md)üzere Azure SYNAPSE Analytics 'i nasıl kullanacağınızı öğrenmek için öğreticiyi ziyaret edin. Uygulamanızı Azure SYNAPSE 'a göndermek üzere paketetmadıysanız aşağıdaki adımları izleyin.

1. Uygulamanızı yayımlamak için aşağıdaki komutları çalıştırın. *MySparkApp* 'i uygulamanızın yoluyla değiştirdiğinizden emin olun.

   **Windows 'da:**

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

   **Linux 'ta:**

   ```bash
   zip -r publish.zip
   ```

## <a name="net-for-apache-spark-in-azure-synapse-analytics-notebooks"></a>Azure SYNAPSE Analytics not defterlerinde Apache Spark için .NET 

Not defterleri, Apache Spark işlem hatları ve senaryolar için .NET uygulamanızı prototip için harika bir seçenektir. Verilerinizi hızla ve verimli bir şekilde çalışmaya başlayabilir, düzenleyebilir, filtreleyerek, görüntüleyerek ve görselleştirmeyi başlatabilirsiniz. Veri mühendisleri, veri bilimcileri, iş analistleri ve makine öğrenimi mühendisleri, paylaşılan ve etkileşimli bir belge üzerinde işbirliği yapabilirler. Veri araştırmasının anında sonuçları görürsünüz ve verilerinizi aynı not defterinde görselleştirebilirsiniz.

### <a name="how-to-use-net-for-apache-spark-notebooks"></a>Apache Spark Not defterleri için .NET kullanma

Yeni bir not defteri oluşturduğunuzda, iş mantığınızı ifade etmek istediğiniz bir dil çekirdeği seçersiniz. C# dahil olmak üzere çeşitli diller için çekirdek desteği vardır.

Azure SYNAPSE Analytics Not defterinizde Apache Spark için .NET kullanmak istiyorsanız, çekirdek olarak **.net Spark (C#)** öğesini seçin ve Not defterini mevcut bir Spark havuzuna ekleyin.

.Net Spark Not defteri, .NET etkileşimli deneyimlerine dayalıdır ve daha önceden tanımlanmış Spark oturum değişkeniyle birlikte Box for Spark for the .NET kullanma özelliği ile etkileşimli C# deneyimleri sağlar `spark` .

### <a name="net-for-apache-spark-c-kernel-features"></a>Apache Spark C# çekirdek özellikleri için .NET

Azure SYNAPSE Analytics not defterinde Apache Spark için .NET kullandığınızda aşağıdaki özellikler mevcuttur:

* Bildirime dayalı HTML: üst bilgiler, madde işaretli listeler ve hatta görüntü görüntüleme gibi HTML sözdizimini kullanarak hücrelerden çıkış oluşturun.
* Basit C# deyimleri (atamalar, konsola yazdırma, özel durum atma vb.).
* Çok satırlık C# kod blokları (if deyimleri, Foreach döngüleri, sınıf tanımları vb.).
* Standart C# kitaplığına (System, LINQ, numaralar vb.) erişin.
* [C# 8,0 dil özellikleri](/dotnet/csharp/whats-new/csharp-8?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)desteği.
* Apache Spark oturumunuza erişim sağlamak için önceden tanımlanmış bir değişken olarak ' Spark '.
* [Apache Spark içinde çalışabilen .NET Kullanıcı tanımlı işlevleri](https://github.com/dotnet/spark/blob/master/examples/Microsoft.Spark.CSharp.Examples/Sql)tanımlama desteği.
* Kitaplığı kullanarak farklı grafikleri (örneğin, çizgi, çubuk veya histogram) ve düzenleri (örneğin, tek, kaplama vb.) kullanarak Spark işlerinizde çıktıyı görselleştirmeye yönelik destek `XPlot.Plotly` .
* C# Not defterinize NuGet paketleri ekleme özelliği.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark belgeleri için .NET](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
* [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
* [.NET etkileşimli](https://devblogs.microsoft.com/dotnet/creating-interactive-net-documentation/)
