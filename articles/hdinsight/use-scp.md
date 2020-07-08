---
title: Azure HDInsight 'ta Apache Hadoop ile SCP kullanma
description: Bu belge, SSH ve SCP komutlarını kullanarak HDInsight 'a bağlanma hakkında bilgi sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020
ms.date: 04/22/2020
ms.openlocfilehash: 2f602c23f182ede2b9897563a8421163e1328e24
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86079199"
---
# <a name="use-scp-with-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache Hadoop ile SCP kullanma

Bu makalede, HDInsight kümenize dosyaları güvenle aktarma hakkında bilgi sağlanır.

## <a name="copy-files"></a>Dosyaları kopyalama

`scp` yardımcı programı, kümedeki bireysel düğümlerde gelen ve giden dosyaları kopyalamak için kullanılabilir. Örneğin, aşağıdaki komut `test.txt` dizinini yerel sistemden birincil baş düğüme kopyalar.

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

`:` sonrasında yol sonra belirtilmezse dosya `sshuser` giriş dizinine yerleştirilir.

Aşağıdaki örnekte birincil baş düğümdeki `sshuser` giriş dizininden `test.txt` dosyası yerel sisteme kopyalanmaktadır:

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

`scp`, yalnızca küme içindeki tek düğümlerin dosya sistemine erişebilir. Bu, kümenin uyumlu depolama alanındaki verilere erişmek için kullanılamaz.

Bir kaynağı SSH oturumundan kullanmak için karşıya yüklemeniz gerektiğinde `scp` kullanın. Örneğin, bir Python betiğini karşıya yükleyin ve bir SSH oturumundan çalıştırın.

Verileri HDFS uyumlu depolama alanına doğrudan yükleme hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure Depolama kullanarak HDInsight](hdinsight-hadoop-use-blob-storage.md)
* [Azure Data Lake Storage kullanan HDInsight](hdinsight-hadoop-use-data-lake-store.md).

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight ile SSH kullanma](./hdinsight-hadoop-linux-use-ssh-unix.md)
* [HDInsight’ta kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md#access-an-edge-node)
