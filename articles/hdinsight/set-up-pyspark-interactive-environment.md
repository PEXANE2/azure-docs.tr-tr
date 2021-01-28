---
title: Azure HDInsight araçları ile PySpark etkileşimli ortamı
description: Sorgular ve betikler oluşturmak ve göndermek için Visual Studio Code için Azure HDInsight araçlarını kullanmayı öğrenin.
keywords: VScode, Azure HDInsight araçları, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Interactive Hive, etkileşimli sorgu
ms.service: hdinsight
ms.topic: how-to
ms.custom: seoapr2020, devx-track-python
ms.date: 04/23/2020
ms.openlocfilehash: 337a9f3f2ea25e5a4d4fa4204a0f3fa4dcc9369b
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98940646"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Visual Studio Code için PySpark etkileşimli ortamını ayarlama

Aşağıdaki adımlarda, VSCode 'da PySpark etkileşimli ortamının nasıl ayarlanacağı gösterilmektedir. Bu adım yalnızca Windows dışı kullanıcılar içindir.

Ana yolunuzda sanal ortam oluşturmak için **Python/PIP** komutunu kullanıyoruz. Başka bir sürüm kullanmak istiyorsanız, **Python/PIP** komutunun varsayılan sürümünü el ile değiştirmeniz gerekir. Daha fazla ayrıntı için bkz. [Update-alternatifler](https://linux.die.net/man/8/update-alternatives).

1. [Python](https://www.python.org/downloads/) ve [PIP](https://pip.pypa.io/en/stable/installing/)'yi yükler.

   * Python [https://www.python.org/downloads/](https://www.python.org/downloads/) uygulamasını şuradan 
   * PIP 'yi yükleme [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) (Python yüklemesinden yüklenmemişse).
   * İsteğe bağlı olarak, ve komutlarını kullanarak Python ve PIP 'nin başarıyla yüklendiğini doğrulayın `python --version` `pip --version` . 

     > [!NOTE]
     > MacOS varsayılan sürümünü kullanmak yerine Python 'un el ile yüklenmesi önerilir.

2. Aşağıdaki komutu çalıştırarak **virtualenv** 'yi çalıştırın.

   ```bash
   pip install virtualenv
   ```

## <a name="other-packages"></a>Diğer paketler

Linux 'ta aşağıdaki hata iletisiyle karşılaşırsanız, aşağıdaki iki komutu çalıştırarak gerekli paketleri yükleyebilirsiniz.

   ![Python için libkrb5 paketini yükler](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```bash
sudo apt-get install libkrb5-dev
```

```bash
sudo apt-get install python-dev
```

VSCode 'u yeniden başlatın ve ardından VSCode düzenleyicisine dönün ve **Spark: Pyspark Interactive** komutunu çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="demo"></a>Tanıtım

* VS Code için HDInsight: [video](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [Visual Studio Code için Azure HDInsight aracını kullanma](hdinsight-for-vscode.md)
* [Apache Spark Scala uygulamaları oluşturmak ve göndermek için Azure Toolkit for IntelliJ kullanma](spark/apache-spark-intellij-tool-plugin.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](spark/apache-spark-jupyter-notebook-install-locally.md)
