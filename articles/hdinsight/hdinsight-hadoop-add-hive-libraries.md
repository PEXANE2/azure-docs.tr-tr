---
title: Küme oluşturma sırasında kitaplıklar Apache Hive-Azure HDInsight
description: Küme oluşturma sırasında bir HDInsight kümesine Apache Hive kitaplıklarının (jar dosyaları) nasıl ekleneceğini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.date: 12/23/2019
ms.openlocfilehash: 57b4440a29dde470f91bbaae091bf65a0d2a1b51
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552279"
---
# <a name="add-custom-apache-hive-libraries-when-creating-your-hdinsight-cluster"></a>HDInsight kümenizi oluştururken özel Apache Hive kitaplıkları ekleyin

HDInsight 'ta [Apache Hive](https://hive.apache.org/) kitaplıklarını önceden yüklemeyi öğrenin. Bu belge, küme oluşturma sırasında kitaplıkları önceden yüklemek için bir betik eylemi kullanmayla ilgili bilgiler içerir. Bu belgedeki adımlar kullanılarak eklenen kitaplıklar, Hive içinde genel kullanıma sunulmuştur. yüklemek için [jar Ekle](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) ' nin kullanılmasına gerek yoktur.

## <a name="how-it-works"></a>Nasıl çalışır

Bir küme oluştururken, küme düğümlerini oluşturuldukları sırada değiştirmek için bir betik eylemi kullanabilirsiniz. Bu belgedeki betik, kitaplıkların konumu olan tek bir parametreyi kabul eder. Bu konum bir Azure depolama hesabında olmalıdır ve kitaplıkların jar dosyaları olarak depolanması gerekir.

Küme oluşturma sırasında betik, dosyaları numaralandırır, baş ve çalışan düğümlerinde `/usr/lib/customhivelibs/` dizinine kopyalar ve ardından bunları `core-site.xml` dosyasındaki `hive.aux.jars.path` özelliğine ekler. Linux tabanlı kümeler üzerinde `hive-env.sh` dosyasını da dosyaların konumuyla güncelleştirir.

Bu makaledeki betik eyleminin kullanılması, **Web Hcat**ve **HiveServer2**için Hive istemcisi kullanılırken kitaplıkları kullanılabilir hale getirir.

## <a name="the-script"></a>Betik

**Betik konumu**

[https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

**Gereksinimler**

* Betikler hem **baş düğümlere** hem de **çalışan düğümlerine**uygulanmalıdır.

* Yüklemek istediğiniz jar dosyaları dışındaki 'ın Azure Blob depolama alanında **tek bir kapsayıcıda**depolanması gerekir.

* Jar dosyalarının kitaplığını içeren depolama hesabı, oluşturma sırasında HDInsight **kümesine bağlanmalıdır.** Bu, varsayılan depolama hesabı ya da __depolama hesabı ayarları__aracılığıyla eklenmiş bir hesap olmalıdır.

* Kapsayıcının ınsıb yolu, komut dosyası eyleminin parametresi olarak belirtilmelidir. Örneğin, JAR dosyaları dışındaki, **depolamam**adlı bir depolama hesabındaki **LIBS** adlı bir kapsayıcıda depolanıyorsa, parametre `wasbs://libs@mystorage.blob.core.windows.net/`olur.

  > [!NOTE]  
  > Bu belge, zaten bir depolama hesabı, blob kapsayıcısı oluşturmuş olduğunuzu ve dosyaları ona yüklediğinizi varsayar.
  >
  > Bir depolama hesabı oluşturmadıysanız, [Azure Portal](https://portal.azure.com)aracılığıyla bunu yapabilirsiniz. Daha sonra [Azure Depolama Gezgini](https://storageexplorer.com/) gibi bir yardımcı programını kullanarak hesapta bir kapsayıcı oluşturabilir ve dosyaları karşıya yükleyebilirsiniz.

## <a name="create-a-cluster-using-the-script"></a>Betiği kullanarak bir küme oluşturma

1. [Linux 'Ta HDInsight kümeleri sağlama](hdinsight-hadoop-provision-linux-clusters.md)bölümündeki adımları kullanarak bir kümeyi sağlamaya başlayın, ancak sağlamayı tamammayın. Bu betiği kullanarak bir küme oluşturmak için Azure PowerShell veya HDInsight .NET SDK 'sını de kullanabilirsiniz. Bu yöntemleri kullanma hakkında daha fazla bilgi için bkz. [HDInsight kümelerini betik eylemleriyle özelleştirme](hdinsight-hadoop-customize-cluster-linux.md). Azure portal için **Klasik deneyime git** seçeneğini ve ardından **özel (boyut, ayarlar, uygulamalar)** öğesini seçmeniz gerekir.

1. **Depolama**için, jar dosyalarının kitaplığını içeren depolama hesabı, küme için kullanılan hesaptan farklı olur, **ek depolama hesapları**doldurun.

1. **Betik eylemleri**için aşağıdaki bilgileri sağlayın:

    |Özellik |Değer |
    |---|---|
    |Betik türü|-Özel|
    |Ad|Kitaplıklar |
    |Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh`|
    |Düğüm türleri|Baş, çalışan|
    |Parametreler|Cars 'ı içeren kapsayıcı ve depolama hesabına ait olan adresi girin. Örneğin, `wasbs://libs@mystorage.blob.core.windows.net/`.|

1. [Linux 'Ta HDInsight kümeleri sağlama](hdinsight-hadoop-provision-linux-clusters.md)bölümünde açıklandığı gibi kümeyi sağlamaya devam edin.

Küme oluşturma işlemi tamamlandıktan sonra, `ADD JAR` ifadesini kullanmak zorunda kalmadan bu betik aracılığıyla eklenen jar dosyaları dışındaki 'ı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Hive ile çalışma hakkında daha fazla bilgi için bkz. [HDInsight ile Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
