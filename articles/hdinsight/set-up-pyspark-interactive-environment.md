---
title: Azure HDInsight Araçları ile PySpark etkileşimli ortam
description: Sorgular ve komut dosyaları oluşturmak ve göndermek için Visual Studio Code için Azure HDInsight Araçlarını nasıl kullanacağınızı öğrenin.
keywords: VScode,Azure HDInsight Araçları,Hive,Python,PySpark,Spark,HDInsight,Hadoop,LLAP,İnteraktif Kovan,İnteraktif Sorgu
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: 2a725f3c5c9e1428079807b5b76dbe72d416a9c7
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393670"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio Code için PySpark etkileşimli ortamı nı ayarlama

Aşağıdaki adımlar, VS Code'da PySpark etkileşimli ortamını nasıl ayarlayabileceğinizi gösterir.

Ana sayfanızda sanal ortam oluşturmak için **python/pip** komutunu kullanırız. Başka bir sürüm kullanmak istiyorsanız, **python/pip** komutunun varsayılan sürümünü el ile değiştirmeniz gerekir. Daha fazla ayrıntı [güncelleme alternatifleri](https://linux.die.net/man/8/update-alternatives)bakın.

1. [Python](https://www.python.org/downloads/) ve [pip'i](https://pip.pypa.io/en/stable/installing/)yükleyin.

   + Python'u [https://www.python.org/downloads/](https://www.python.org/downloads/)' dan yükleyin.
   + Pip'i [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) yükleyin (Python yüklemesinden yüklenmiyorsa).
   + Python ve pip'in aşağıdaki komutları kullanarak başarıyla yüklenmesini doğrulayın. (İsteğe bağlı)

        ![Python pip sürüm komutunu kontrol edin](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > MacOS varsayılan sürümünü kullanmak yerine Python'u el ile yüklemeniz önerilir.

2. Aşağıdaki komutu çalıştırarak **virtualenv** yükleyin.

   ```bash
   pip install virtualenv
   ```

## <a name="other-packages"></a>Diğer paketler

Bir hata iletisi ile karşılaşırsanız, aşağıdaki komutları çalıştırarak gerekli paketleri yükleyin:

   ![Python için libkrb5 paketi yükleyin](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```bash
sudo apt-get install libkrb5-dev
```

```bash
sudo apt-get install python-dev
```

VS Code'u yeniden başlatın ve ardından **HDInsight: PySpark Interactive'i**çalıştıran komut dosyası düzenleyicisine geri dön.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım

* VS Kodu için HDInsight: [Video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Visual Studio Kodu için Azure HDInsight Aracı'nı kullanma](hdinsight-for-vscode.md)
* [Apache Spark Scala uygulamaları oluşturmak ve göndermek için IntelliJ için Azure Araç Kiti'ni kullanın](spark/apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarını SSH üzerinden uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Vpn üzerinden Apache Spark uygulamalarını uzaktan hata ayıklamak için IntelliJ için Azure Araç Kiti'ni kullanın](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Spark uygulamaları oluşturmak için Eclipse için Azure Araç Setinde HDInsight Araçlarını kullanın](spark/apache-spark-eclipse-tool-plugin.md)
* [HDInsight'ta Apache Spark kümesine sahip Apache Zeppelin dizüstü bilgisayarları kullanma](spark/apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesinde Jupyter dizüstü bilgisayar için çekirdekler mevcuttur](spark/apache-spark-jupyter-notebook-kernels.md)
* [Jupyter not defterleri ile dış paketleri kullanma](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Azure HDInsight'ta Microsoft Power BI ile Apache Hive verilerini görselleştirin](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Azure HDInsight'da Apache Hive sorgularını çalıştırmak için Apache Zeppelin'i kullanma](./interactive-query/hdinsight-connect-hive-zeppelin.md)
