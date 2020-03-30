---
title: Linux tabanlı HDInsight'ta Hadoop MapReduce ile .NET'i kullanın - Azure
description: Linux tabanlı HDInsight'ta MapReduce akışı için .NET uygulamalarını nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 4b402975a151d26e8f335c07930274c156ac52fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272376"
---
# <a name="migrate-net-solutions-for-windows-based-hdinsight-to-linux-based-hdinsight"></a>Windows tabanlı HDInsight için .NET çözümlerini Linux tabanlı HDInsight'a geçirin

Linux tabanlı HDInsight kümeleri [Mono 'yu kullanarakhttps://mono-project.com) ](https://mono-project.com) .NET uygulamalarını çalıştırAr. Mono, Linux tabanlı HDInsight ile MapReduce uygulamaları gibi .NET bileşenlerini kullanmanıza olanak tanır. Bu belgede, Linux tabanlı HDInsight'ta Mono ile çalışmak üzere Windows tabanlı HDInsight kümeleri için oluşturulan .NET çözümlerini nasıl geçirebilirsiniz öğrenin.

## <a name="mono-compatibility-with-net"></a>.NET ile mono uyumluluk

Mono sürüm 4.2.1 HDInsight sürüm 3.6 ile birlikte verilir. HDInsight ile birlikte mono sürümü hakkında daha fazla bilgi için [HDInsight bileşen sürümlerine](hdinsight-component-versioning.md)bakın.

Mono ve .NET arasındaki uyumluluk hakkında daha fazla bilgi için [Mono uyumluluğuhttps://www.mono-project.com/docs/about-mono/compatibility/) (belge)](https://www.mono-project.com/docs/about-mono/compatibility/) bakın.

> [!IMPORTANT]  
> SCP.NET çerçevesi Mono ile uyumludur. Mono ile SCP.NET kullanma hakkında daha fazla bilgi için, [HDInsight'ta Apache Storm için C# topolojileri geliştirmek için Visual Studio'yu kullanın.](storm/apache-storm-develop-csharp-visual-studio-topology.md)

## <a name="automated-portability-analysis"></a>Otomatik taşınabilirlik analizi

[.NET Taşınabilirlik Çözümleyicisi,](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer) uygulamanız ve Mono arasında uyumsuzluklar raporu oluşturmak için kullanılabilir. Uygulamanızı Mono taşınabilirlik için denetlemek için çözümleyiciyi yapılandırmak için aşağıdaki adımları kullanın:

1. [.NET Taşınabilirlik Çözümleyicisini](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)kurun. Yükleme sırasında kullanmak üzere Visual Studio sürümünü seçin.

2. Visual Studio 2015'ten,__Taşınabilirlik Çözümleyici ayarlarını__ __analiz et'i__ > seçin ve __4.5'in__ __Mono__ bölümünde kontrol edildiğinden emin olun.

    ![Analizör ayarları için Mono bölümünde 4.5 işaretli](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-settings.png)

    Yapılandırmayı kaydetmek için __Tamam'ı__ seçin.

3.  > __Derleme Taşınabilirliğini Analiz__ __Et'i__seçin. Çözümünüzü içeren derlemeyi seçin ve ardından çözümlemeye başlamak için __Aç'ı__ seçin.

4. Çözümleme tamamlandıktan sonra Analiz__Görünümü analiz raporlarını__ __seçin.__ >  __Taşınabilirlik Analizi Sonuçlarında,__ raporu açmak için __Raporu Aç'ı__ seçin.

    ![Taşınabilirlik çözümleyici sonuçları iletişim kutusu](./media/hdinsight-hadoop-migrate-dotnet-to-linux/portability-analyzer-results.png)

> [!IMPORTANT]  
> Çözümleyici çözümünizle ilgili her sorunu yakalayamaz. Örneğin, Mono Windows'da çalışıyorsa `c:\temp\file.txt` dosya yolu tamam olarak kabul edilir. Aynı yol bir Linux platformunda geçerli değildir.

## <a name="manual-portability-analysis"></a>Manuel taşınabilirlik analizi

[Uygulama Taşınabilirliğihttps://www.mono-project.com/docs/getting-started/application-portability/) (belge)](https://www.mono-project.com/docs/getting-started/application-portability/) deki bilgileri kullanarak kodunuzu el ile denetleyin.

## <a name="modify-and-build"></a>Değiştirin ve oluşturun

HDInsight için .NET çözümlerinizi oluşturmak için Visual Studio'yı kullanmaya devam edebilirsiniz. Ancak, projenin .NET Framework 4.5'i kullanacak şekilde yapılandırıldığından emin olmalısınız.

## <a name="deploy-and-test"></a>Dağıtma ve test

.NET Taşınabilirlik Çözümleyicisi'nin önerilerini kullanarak veya manuel analizden çözümünüzü değiştirdikten sonra HDInsight ile test etmelisiniz. Çözümü Linux tabanlı bir HDInsight kümesiüzerinde test etmek, düzeltilmesi gereken ince sorunları ortaya çıkarabilir. Test ederken uygulamanızda ek günlüğe kaydetmeyi etkinleştirmenizi öneririz.

Günlüklere erişim hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Linux tabanlı HDInsight'ta Apache Hadoop İPLik uygulama günlüklerine erişin](hdinsight-hadoop-access-yarn-app-logs-linux.md)

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta MapReduce ile C# kullanın](hadoop/apache-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Apache Hive ve Apache Pig ile C# kullanıcı tanımlı işlevlerini kullanma](hadoop/apache-hadoop-hive-pig-udf-dotnet-csharp.md)

* [HDInsight'ta Apache Storm için C# topolojileri geliştirin](storm/apache-storm-develop-csharp-visual-studio-topology.md)
