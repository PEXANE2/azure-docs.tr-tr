---
title: Azure HDInsight araçları ile PySpark etkileşimli ortamı
description: Sorgular ve betikler oluşturmak ve göndermek için Visual Studio Code için Azure HDInsight araçlarını kullanmayı öğrenin.
keywords: VScode, Azure HDInsight araçları, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, etkileşimli sorgu
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2020
ms.openlocfilehash: d9a3356ea18ccf4660d05b3fade9d9e6d6cbb5ee
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131338"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio Code için PySpark etkileşimli ortamını ayarlama

Aşağıdaki adımlarda VS Code 'da PySpark etkileşimli ortamının nasıl ayarlanacağı gösterilmektedir.

Ana yolunuzda sanal ortam oluşturmak için **Python/PIP** komutunu kullanıyoruz. Başka bir sürüm kullanmak istiyorsanız, **Python/PIP** komutunun varsayılan sürümünü el ile değiştirmeniz gerekir. Daha fazla ayrıntı için bkz. [Update-alternatifler](https://linux.die.net/man/8/update-alternatives).

1. [Python](https://www.python.org/downloads/) ve [PIP](https://pip.pypa.io/en/stable/installing/)'yi yükler.

   * Python uygulamasını şuradan [https://www.python.org/downloads/](https://www.python.org/downloads/)
   * PIP 'yi [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) yükleme (Python yüklemesinden yüklenmemişse).
   * Aşağıdaki komutları kullanarak Python ve PIP 'nin başarıyla yüklendiğini doğrulayın. (İsteğe bağlı)

        ![Python pversıon sürümü komutunu denetle](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > MacOS varsayılan sürümünü kullanmak yerine Python 'un el ile yüklenmesi önerilir.

2. Aşağıdaki komutu çalıştırarak **virtualenv** 'yi çalıştırın.

   ```bash
   pip install virtualenv
   ```

## <a name="other-packages"></a>Diğer paketler

Bir hata iletisiyle karşılaşırsanız, aşağıdaki komutları çalıştırarak gerekli paketleri yükleyebilirsiniz:

   ![Python için libkrb5 paketini yükler](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```bash
sudo apt-get install libkrb5-dev
```

```bash
sudo apt-get install python-dev
```

VS Code yeniden başlatın ve ardından HDInsight çalıştıran betik düzenleyicisine geri dönün **: PySpark Interactive**.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım

* VS Code için HDInsight: [video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Visual Studio Code için Azure HDInsight aracını kullanma](hdinsight-for-vscode.md)
* [Apache Spark Scala uygulamaları oluşturmak ve göndermek için Azure Toolkit for IntelliJ kullanma](spark/apache-spark-intellij-tool-plugin.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
