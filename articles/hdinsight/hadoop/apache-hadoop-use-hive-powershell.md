---
title: HDInsight'ta PowerShell ile Apache Hive'ı kullanma - Azure
description: Azure HDInsight'ta Apache Hadoop'ta Apache Hive sorgularını çalıştırmak için PowerShell'i kullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/24/2019
ms.openlocfilehash: deaa934b257fab74830d75e308a283e7608dc590
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552602"
---
# <a name="run-apache-hive-queries-using-powershell"></a>PowerShell'i kullanarak Apache Hive sorgularını çalıştırın

[!INCLUDE [hive-selector](../../../includes/hdinsight-selector-use-hive.md)]

Bu belge, HDInsight kümesindeki Bir Apache Hadoop'ta Apache Hive sorgularını çalıştırmak için Azure PowerShell'i kullanma nın bir örneğini sağlar.

> [!NOTE]  
> Bu belge, örneklerde kullanılan HiveQL deyimlerinin ne yaptığına ilişkin ayrıntılı bir açıklama sağlamaz. Bu örnekte kullanılan HiveQL hakkında bilgi için [HDInsight'ta Apache Hadoop ile Apache Hive kullanın'](hdinsight-use-hive.md)a bakın.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Hadoop kümesi. [Linux'ta HDInsight ile başlayın](./apache-hadoop-linux-tutorial-get-started.md)bakın.

* PowerShell [Az Modülü](https://docs.microsoft.com/powershell/azure/overview) kuruldu.

## <a name="run-a-hive-query"></a>Hive sorgusu çalıştırma

Azure PowerShell, HDInsight'ta Kovan sorgularını uzaktan çalıştırmanızı sağlayan *cmdletsağlar.* Dahili olarak cmdlet'ler HDInsight kümesinde [WebHCat'e](https://cwiki.apache.org/confluence/display/Hive/WebHCat) REST aramaları yapar.

Aşağıdaki cmdlets uzak bir HDInsight kümesinde Hive sorguları çalıştırırken kullanılır:

* `Connect-AzAccount`: Azure aboneliğinizde Azure PowerShell'in kimliğini doğrular.
* `New-AzHDInsightHiveJobDefinition`: Belirtilen HiveQL deyimlerini kullanarak bir *iş tanımı* oluşturur.
* `Start-AzHDInsightJob`: İş tanımını HDInsight'a gönderir ve işi başlatır. Bir *iş* nesnesi döndürülür.
* `Wait-AzHDInsightJob`: İşin durumunu denetlemek için iş nesnesini kullanır. İş tamamlanana veya bekleme süresi aşılana kadar bekler.
* `Get-AzHDInsightJobOutput`: İşin çıktısını almak için kullanılır.
* `Invoke-AzHDInsightHiveJob`: HiveQL deyimlerini çalıştırmak için kullanılır. Bu cmdlet sorguyu engeller, sonra sonuçları döndürür.
* `Use-AzHDInsightCluster`: Komut için kullanılacak geçerli `Invoke-AzHDInsightHiveJob` kümeyi ayarlar.

Aşağıdaki adımlar, HDInsight kümenizde bir işi çalıştırmak için bu cmdletlerin nasıl kullanılacağını gösterir:

1. Bir düzenleyici kullanarak, aşağıdaki `hivejob.ps1`kodu ' olarak kaydedin.

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. Yeni bir **Azure PowerShell** komut istemi açın. Dizinleri `hivejob.ps1` dosyanın konumuyla değiştirin ve komut dosyasını çalıştırmak için aşağıdaki komutu kullanın:

        .\hivejob.ps1

    Komut dosyası çalıştığında, küme adını ve HTTPS/Cluster Admin hesap kimlik bilgilerini girmeniz istenir. Azure aboneliğinizde oturum açmanız da isteolabilir.

3. İş tamamlandığında, aşağıdaki metne benzer bilgileri döndürür:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Daha önce de `Invoke-Hive` belirtildiği gibi, bir sorgu çalıştırmak ve yanıtı beklemek için kullanılabilir. Invoke-Hive'ın nasıl çalıştığını görmek için aşağıdaki komut dosyasını kullanın:

    [!code-powershell[main](../../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    Çıktı aşağıdaki metne benzer:

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]  
   > Daha uzun HiveQL sorguları için Azure PowerShell **Here-Strings** cmdlet veya HiveQL komut dosyası dosyalarını kullanabilirsiniz. Aşağıdaki snippet bir HiveQL komut dosyası dosyası çalıştırmak için `Invoke-Hive` cmdlet nasıl kullanılacağını gösterir. HiveQL komut dosyası dosyası wasbs:// yüklenmelidir.
   >
   > `Invoke-AzHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > **Here-Strings**hakkında daha fazla bilgi için bkz: <a href="https://technet.microsoft.com/library/ee692792.aspx" target="_blank">Windows PowerShell Here-Strings'i kullanma.</a>

## <a name="troubleshooting"></a>Sorun giderme

İş tamamlandığında hiçbir bilgi döndürülmezse, hata günlüklerini görüntüleyin. Bu işin hata bilgilerini görüntülemek `hivejob.ps1` için, dosyanın sonuna aşağıdakileri ekleyin, kaydedin ve sonra yeniden çalıştırın.

```powershell
# Print the output of the Hive job.
Get-AzHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Bu cmdlet, iş işleme sırasında STDERR'ye yazılan bilgileri döndürür.

## <a name="summary"></a>Özet

Gördüğünüz gibi Azure PowerShell, Bir HDInsight kümesinde Hive sorgularını çalıştırmak, iş durumunu izlemek ve çıktıyı almak için kolay bir yol sağlar.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta Hive hakkında genel bilgi için:

* [HDInsight'ta Apache Hadoop ile Apache Hive kullanın](hdinsight-use-hive.md)

HDInsight'ta Hadoop ile çalışabileceğiniz diğer yollar hakkında bilgi için:

* [HDInsight'ta Apache Hadoop ile MapReduce'ı kullanın](hdinsight-use-mapreduce.md)
