---
title: Apache Hadoop ile MapReduce ve PowerShell'i kullanma - Azure HDInsight
description: HDInsight'ta Apache Hadoop ile MapReduce işlerini uzaktan çalıştırmak için PowerShell'i nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/08/2020
ms.openlocfilehash: b3c1abb7bff54e3e2d294b073b867c6c0e06f482
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75830080"
---
# <a name="run-mapreduce-jobs-with-apache-hadoop-on-hdinsight-using-powershell"></a>PowerShell'i kullanarak HDInsight'ta Apache Hadoop ile MapReduce işlerini çalıştırın

[!INCLUDE [mapreduce-selector](../../../includes/hdinsight-selector-use-mapreduce.md)]

Bu belge, HDInsight kümesindeki bir Hadoop'ta MapReduce işini çalıştırmak için Azure PowerShell'i kullanmanın bir örneğini sağlar.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun.](../hdinsight-hadoop-create-linux-clusters-portal.md)

* PowerShell [Az Modülü](https://docs.microsoft.com/powershell/azure/overview) kuruldu.

## <a name="run-a-mapreduce-job"></a>MapReduce işini çalıştırın

Azure PowerShell, HDInsight'ta MapReduce işlerini uzaktan çalıştırmanızı sağlayan *cmdletsağlar.* Dahili olarak PowerShell, HDInsight kümesinde çalışan [WebHCat'e](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (eski adıyla Templeton) REST aramaları yapar.

Aşağıdaki cmdlets uzak bir HDInsight kümesinde MapReduce işleri çalıştırırken kullanılır.

|Cmdlet | Açıklama |
|---|---|
|Bağlan-AzHesap|Azure aboneliğinizde Azure PowerShell'in kimliğini doğrular.|
|Yeni-AzHDInsightMapReduceJobDefinition|Belirtilen MapReduce bilgilerini kullanarak yeni bir *iş tanımı* oluşturur.|
|Başlangıç-AzHDInsightJob|İş tanımını HDInsight'a gönderir ve işi başlatır. Bir *iş* nesnesi döndürülür.|
|Bekleyin-AzHDInsightJob|İş durumunu denetlemek için iş nesnesini kullanır. İş tamamlanana veya bekleme süresi aşılana kadar bekler.|
|Get-AzHDInsightJobOutput|İşin çıktısını almak için kullanılır.|

Aşağıdaki adımlar, HDInsight kümenizde bir işi çalıştırmak için bu cmdletlerin nasıl kullanılacağını gösterir.

1. Bir düzenleyici kullanarak, **mapreducejob.ps1**olarak aşağıdaki kodu kaydedin.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. Yeni bir **Azure PowerShell** komut istemi açın. Dizinleri **mapreducejob.ps1** dosyasının konumuyla değiştirin ve komut dosyasını çalıştırmak için aşağıdaki komutu kullanın:

        .\mapreducejob.ps1

    Komut dosyasını çalıştırdığınızda, HDInsight kümesinin adı ve küme girişi için istenirsiniz. Azure aboneliğinizde kimlik doğrulamanız da istenebilir.

3. İş tamamlandığında, aşağıdaki metne benzer çıktı alırsınız:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Bu çıktı, işin başarıyla tamamlandığını gösterir.

    > [!NOTE]  
    > **ExitCode** 0'dan başka bir değerse, [sorun giderme'ye](#troubleshooting)bakın.

    Bu örnek, indirdiğiniz dosyaları komut dosyasını çalıştırdığınız dizinde **bir output.txt** dosyasına da depolar.

### <a name="view-output"></a>Çıktıyı görüntüleme

İş tarafından üretilen sözcükleri ve sayıları görmek için, bir metin düzenleyicisinde **output.txt** dosyasını açın.

> [!NOTE]  
> MapReduce işinin çıktı dosyaları değişmez. Bu nedenle, bu örneği yeniden çalıştıracaksanız, çıktı dosyasının adını değiştirmeniz gerekir.

## <a name="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülmezse, iş için hataları görüntüleyin. Bu işin hata bilgilerini görüntülemek için **mapreducejob.ps1** dosyasının sonuna aşağıdaki komutu ekleyin. Sonra dosyayı kaydedin ve komut dosyasını yeniden çalıştırın.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet, iş çalışırken STDERR'ye yazılan bilgileri döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Gördüğünüz gibi Azure PowerShell, MapReduce işlerini bir HDInsight kümesinde çalıştırmak, iş durumunu izlemek ve çıktıyı almak için kolay bir yol sağlar. HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için:

* [HDInsight Hadoop'ta MapReduce'i kullanın](hdinsight-use-mapreduce.md)
* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)
