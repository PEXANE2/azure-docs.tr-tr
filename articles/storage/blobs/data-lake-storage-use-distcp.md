---
title: DistCp kullanarak verileri Azure Veri Gölü Depolama Gen2'sine kopyalama| Microsoft Dokümanlar
description: Verileri Veri Gölü Depolama Gen2'ye ve bu verilerden kopyalamak için DistCp aracını kullanın
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 3c09a95309e001def306698bbba4f6d0a1a2804d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255541"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen2"></a>Azure Depolama Blobs ve Azure Veri Gölü Depolama Gen2 arasındaki verileri kopyalamak için DistCp'yi kullanın

Genel amaçlı bir V2 depolama hesabı ile hiyerarşik ad alanı etkinleştirilmiş genel amaçlı V2 depolama hesabı arasındaki verileri kopyalamak için [DistCp'yi](https://hadoop.apache.org/docs/stable/hadoop-distcp/DistCp.html) kullanabilirsiniz. Bu makalede, DistCp aracının nasıl kullanıldığına ilişkin yönergeler verilmektedir.

DistCp çeşitli komut satırı parametreleri sağlar ve kullanımınızı optimize etmek için bu makaleyi okumanızı şiddetle öneririz. Bu makalede, hiyerarşik ad alanı etkin bir hesaba veri kopyalamak için kullanımı üzerinde odaklanırken temel işlevselliği gösterir.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Veri Gölü Depolama Gen2 özellikleri (hiyerarşik ad alanı) içermeyen varolan bir Azure Depolama hesabı etkinleştirildi.**
* **Veri Gölü Depolama Gen2 özelliğine sahip bir Azure Depolama hesabı etkinleştirildi.** Nasıl oluşturulacağına ilişkin talimatlar [için](data-lake-storage-quickstart-create-account.md) bkz.
* Depolama hesabında hiyerarşik ad alanı etkinleştirilmiş bir **dosya sistemi.**
* Veri Gölü Depolama Gen2 etkin leştirilmiş bir depolama hesabına erişimi olan **Azure HDInsight kümesi.** Bkz. [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanın.](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>HDInsight Linux kümesinden DistCp'yi kullanma

BIR HDInsight kümesi, farklı kaynaklardan gelen verileri bir HDInsight kümesine kopyalamak için kullanılabilen DistCp yardımcı programıyla birlikte gelir. HDInsight kümesini Azure Blob Depolama ve Azure Veri Gölü Depolamasını birlikte kullanacak şekilde yapılaştırdıysanız, DistCp yardımcı programı verileri kopyalamak için de kullanılabilir. Bu bölümde, DistCp yardımcı programının nasıl kullanılacağına bakAcağız.

1. HDI kümenize bir SSH oturumu oluşturun. Bkz. [Linux tabanlı HDInsight kümesine bağlanın.](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md)

2. Varolan genel amaçlı V2 hesabınıza erişip erişemeyeceğiniz (hiyerarşik ad alanı etkin olmadan) doğrulayın.

        hdfs dfs –ls wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/

    Çıktı kapsayıcıdaki içeriğin bir listesini sağlamalıdır.

3. Benzer şekilde, kümeden hiyerarşik ad alanı etkinken depolama hesabına erişip erişemeyeceğiniz de doğrulayın. Şu komutu çalıştırın:

        hdfs dfs -ls abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/

    Çıktı, Veri Gölü Depolama hesabındaki dosyaların/klasörlerin bir listesini sağlamalıdır.

4. WASB'deki verileri Veri Gölü Depolama hesabına kopyalamak için DistCp'yi kullanın.

        hadoop distcp wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

    Komut, Blob depolamasındaki **/example/data/gutenberg/klasörün** içeriğini Veri Gölü Depolama hesabındaki **/myfolder'a** kopyalar.

5. Benzer şekilde, Veri Gölü Depolama hesabından Blob Depolama (WASB) verilerini kopyalamak için DistCp kullanın.

        hadoop distcp abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg

    Komut, Veri Gölü Deposu hesabındaki **/myfolder'ın** içeriğini WASB'daki **/example/data/gutenberg/** klasörüne kopyalar.

## <a name="performance-considerations-while-using-distcp"></a>DistCp kullanırken performans hususları

DistCp'nin en düşük parçalı olması tek bir dosya olduğundan, en fazla eşzamanlı kopya sayısını ayarlamak, onu Veri Gölü Depolamasına karşı en iyi duruma getirmek için en önemli parametredir. Eşzamanlı kopya sayısı, komut satırındaki mappers (**m**) parametresayısına eşittir. Bu parametre, verileri kopyalamak için kullanılan en fazla haritacı sayısını belirtir. Varsayılan değer 20'dir.

**Örnek**

    hadoop distcp -m 100 wasbs://<CONTAINER_NAME>@<STORAGE_ACCOUNT_NAME>.blob.core.windows.net/example/data/gutenberg abfss://<FILE_SYSTEM_NAME>@<STORAGE_ACCOUNT_NAME>.dfs.core.windows.net/myfolder

### <a name="how-do-i-determine-the-number-of-mappers-to-use"></a>Kullanılacak harita oluşturucu sayısını nasıl belirleyebilirim?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **Adım 1: 'varsayılan' YARN uygulama kuyruğuiçin kullanılabilir toplam belleği belirleyin** - İlk adım, 'varsayılan' YARN uygulama kuyruğunda bulunan belleği belirlemektir. Bu bilgiler kümeyle ilişkili Ambari portalında mevcuttur. 'Varsayılan' uygulama kuyruğunda bulunan İplik belleği görmek için İPLik'e gidin ve Configs sekmesini görüntüleyin. Bu, DistCp işiniz için kullanılabilir toplam bellektir (aslında bir MapReduce işidir).

* **Adım 2: Mappers sayısını hesaplamak** - **m** değeri YARN konteyner boyutuna bölünen toplam İPLik bellek bölümüne eşittir. İplik konteyner boyutu bilgileri Ambari portalında da mevcuttur. İPLik'e gidin ve Configs sekmesini görüntüleyin. İplik kapsayıcı boyutu bu pencerede görüntülenir. Mappers sayısına ulaşmak için denklem (**m**)

        m = (number of nodes * YARN memory for each node) / YARN container size

**Örnek**

4x D14v2s kümeniz olduğunu ve 10 farklı klasörden 10 TB veri aktarmaya çalıştığınızı varsayalım. Klasörlerin her biri farklı miktarda veri içerir ve her klasördeki dosya boyutları farklıdır.

* **Toplam İplik belleği**: Ambari portalından, İplik belleği bir D14 düğümü için 96 GB olduğunu belirlersiniz. Yani, dört düğüm kümesi için toplam İplik belleği: 

        YARN memory = 4 * 96GB = 384GB

* **Haritacı sayısı**: Ambari portalından, Bir D14 küme düğümü için İplik konteyner boyutunun 3.072 MB olduğunu belirlersiniz. Yani, mappers sayısı:

        m = (4 nodes * 96GB) / 3072MB = 128 mappers

Diğer uygulamalar bellek kullanıyorsa, kümenizin İplik belleği yalnızca bir bölümünü DistCp için kullanmayı seçebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesinin boyutu büyükse (örneğin, >1 TB) veya çok sayıda farklı klasörünüz varsa, birden çok DIsCp işi kullanmayı düşünmelisiniz. Büyük olasılıkla hiçbir performans kazancı yoktur, ancak herhangi bir iş başarısız olursa, sadece tüm iş yerine belirli bir işi yeniden başlatmanız gerekir, böylece işleri yayılır.

### <a name="limitations"></a>Sınırlamalar

* DistCp, performansı optimize etmek için boyutu benzer harita oluşturucular oluşturmaya çalışır. Haritapers sayısını artırmak her zaman performansı artırmak olmayabilir.

* DistCp dosya başına yalnızca bir mapper ile sınırlıdır. Bu nedenle, dosyalarınız olduğundan daha fazla harita oluşturucunuz olmamalıdır. DistCp bir dosyaya yalnızca bir mapper atayabildiği için, bu büyük dosyaları kopyalamak için kullanılabilecek eşzamanlılık miktarını sınırlar.

* Az sayıda büyük dosyanız varsa, daha fazla potansiyel eşzamanlılık sağlamak için bunları 256 MB dosya parçalarına bölmeniz gerekir.
