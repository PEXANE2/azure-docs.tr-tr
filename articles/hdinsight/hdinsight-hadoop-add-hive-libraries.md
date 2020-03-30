---
title: Küme oluşturma sırasında Apache Hive kitaplıkları - Azure HDInsight
description: Küme oluşturma sırasında bir HDInsight kümesine Apache Hive kitaplıklarını (kavanoz dosyaları) nasıl ekleyeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 02/14/2020
ms.openlocfilehash: 0b746963cea5a950ba47d8b4dfeb074cb0910436
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77471032"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>HDInsight kümenizi oluştururken özel Apache Hive kitaplıkları ekleyin

HDInsight'ta [Apache Hive](https://hive.apache.org/) kitaplıklarını önceden nasıl yükleyeceğinizi öğrenin. Bu belge, küme oluşturma sırasında kitaplıkları önceden yüklemek için Komut Dosyası Eylemi kullanma hakkında bilgi içerir. Bu belgedeki adımları kullanarak eklenen kitaplıklar Hive'da genel olarak kullanılabilir - bunları yüklemek için [ADD JAR'ı](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) kullanmanıza gerek yoktur.

## <a name="how-it-works"></a>Nasıl çalışır?

Bir küme oluştururken, küme düğümlerini oluşturulurken değiştirmek için Bir Komut Dosyası Eylemi kullanabilirsiniz. Bu belgedeki komut dosyası, kitaplıkların konumu olan tek bir parametreyi kabul eder. Bu konum bir Azure Depolama Hesabı'nda olmalıdır ve kitaplıklar kavanoz dosyaları olarak depolanmalıdır.

Küme oluşturma sırasında, komut dosyası dosyaları sayısallandırır, baş `/usr/lib/customhivelibs/` ve alt düğümlerde dizin için kopyalar, `hive.aux.jars.path` sonra `core-site.xml` dosyadaki özelliğe ekler. Linux tabanlı kümelerde, dosyayı `hive-env.sh` dosyaların konumuyla birlikte güncelleştirir.

Bu makalede komut dosyası eylem kullanarak **WebHCat**ve **HiveServer2**için bir Hive istemcisi kullanırken kitaplıklar kullanılabilir hale getirir.

## <a name="the-script"></a>Komut dosyası

**Betik konumu**

[https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

### <a name="requirements"></a>Gereksinimler

* Komut dosyaları hem **Baş düğümlerine** hem de **İşçi düğümlerine**uygulanmalıdır.

* Yüklemek istediğiniz kavanozlar Azure Blob Depolama'da tek bir **kapta**saklanmalıdır.

* Kavanoz dosyalarıkitaplığını içeren depolama hesabı, oluşturma sırasında HDInsight kümesine **bağlanmalıdır.** Varsayılan depolama hesabı veya __Depolama Hesabı Ayarları__aracılığıyla eklenen bir hesap olmalıdır.

* Kapsayıcıya WASB yolu Komut Dosyası Eylem için bir parametre olarak belirtilmelidir. Örneğin, kavanozlar **mystorage**adlı bir depolama hesabında `wasbs://libs@mystorage.blob.core.windows.net/` **libs** adlı bir kapta depolanırsa, parametre .

  > [!NOTE]  
  > Bu belge, zaten bir depolama hesabı, blob kapsayıcı sıyrık oluşturduğunuzu ve dosyaları bu hesaba yüklediğinizi varsayar.
  >
  > Bir depolama hesabı oluşturmadıysanız, bunu Azure [portalı](https://portal.azure.com)üzerinden yapabilirsiniz. Daha sonra, hesapta bir kapsayıcı oluşturmak ve ona dosya yüklemek için [Azure Depolama Gezgini](https://storageexplorer.com/) gibi bir yardımcı program kullanabilirsiniz.

## <a name="create-a-cluster-using-the-script"></a>Komut dosyasını kullanarak küme oluşturma

1. [Linux'taki Provision HDInsight kümelerinde](hdinsight-hadoop-provision-linux-clusters.md)adımları kullanarak bir küme yi sağlamaya başlayın, ancak sağlamayı tamamlamayın. Bu komut dosyasını kullanarak bir küme oluşturmak için Azure PowerShell veya HDInsight .NET SDK'yı da kullanabilirsiniz. Bu yöntemleri kullanma hakkında daha fazla bilgi için, [Bkz.](hdinsight-hadoop-customize-cluster-linux.md) Azure portalı için **Yapılandırma + fiyatlandırma** sekmesinden + komut dosyası ekle **eylemini**seçin.

1. **Depolama**için, kavanoz dosyalarıkitaplığını içeren depolama hesabı küme için kullanılan hesaptan farklı olacaksa, **ek depolama hesaplarını**tamamlayın.

1. **Komut Dosyası Eylemleri**için aşağıdaki bilgileri sağlayın:

    |Özellik |Değer |
    |---|---|
    |Komut dosyası türü|- Özel|
    |Adı|Kitaplıklar |
    |Bash script URI|`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`|
    |Düğüm türü(ler)|Baş, İşçi|
    |Parametreler|KAVANOZLARI içeren konteyner ve depolama hesabına WASB adresini girin. Örneğin, `wasbs://libs@mystorage.blob.core.windows.net/`.|

    > [!NOTE]
    > Apache Spark 2.1 için, bu `https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v00.sh`bash script URI kullanın: .

1. [Linux'taki Provision HDInsight kümelerinde](hdinsight-hadoop-provision-linux-clusters.md)açıklandığı gibi kümeyi sağlamaya devam edin.

Küme oluşturma tamamlandıktan sonra, `ADD JAR` deyimi kullanmak zorunda kalmadan Hive'dan bu komut dosyası na eklenen kavanozları kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışma hakkında daha fazla bilgi için [HDInsight ile Apache Hive kullanın](hadoop/hdinsight-use-hive.md)
