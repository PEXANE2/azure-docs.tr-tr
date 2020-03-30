---
title: Azure HDInsight'ta Jupyter ile Python paketleri için komut dosyası eylemi
description: Harici python paketlerini kullanmak için HDInsight Spark kümeleri ile kullanılabilen Jupyter dizüstü bilgisayarları yapılandırmak için komut dosyası eyleminin nasıl kullanılacağına ilişkin adım adım yönergeler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/16/2020
ms.openlocfilehash: 659af8b85cb3736d663e79676b04af8041aeabfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129655"
---
# <a name="safely-manage-python-environment-on-azure-hdinsight-using-script-action"></a>Betik Eylemi kullanarak Azure HDInsight üzerinde Python ortamını güvenli bir şekilde yönetin

> [!div class="op_single_selector"]
> * [Hücre büyüsünü kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Komut Dosyası Eylemini Kullanma](apache-spark-python-package-installation.md)

HDInsight'ın Spark kümesinde anaconda Python 2.7 ve Python 3.5 olmak üzere iki yerleşik Python yüklemesi vardır. Bazı durumlarda, müşterilerin harici Python paketleri veya başka bir Python sürümü yükleme gibi Python ortamını özelleştirmeleri gerekir. Bu makalede, HDInsight'ta bir [Apache Spark](./apache-spark-overview.md) kümesi için Python ortamlarını güvenli bir şekilde yönetme nin en iyi uygulamasını gösteriyoruz.

## <a name="prerequisites"></a>Ön koşullar

HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md). HDInsight'ta zaten bir Kıvılcım kümeniz yoksa, küme oluşturma sırasında komut dosyası eylemlerini çalıştırabilirsiniz. Özel komut [dosyası eylemlerinin nasıl kullanılacağıyla](../hdinsight-hadoop-customize-cluster-linux.md)ilgili belgeleri ziyaret edin.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynak yazılım desteği

Microsoft Azure HDInsight hizmeti, Apache Hadoop çevresinde oluşturulmuş açık kaynak teknolojilerinden oluşan bir ekosistem kullanır. Microsoft Azure, açık kaynak teknolojileri için genel bir destek düzeyi sağlar. Daha fazla bilgi için [Azure Destek SSS web sitesine](https://azure.microsoft.com/support/faq/)bakın. HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

HDInsight hizmetinde kullanılabilen iki tür açık kaynak bileşeni vardır:

|Bileşen |Açıklama |
|---|---|
|Yerleşik|Bu bileşenler HDInsight kümelerine önceden yüklenir ve kümenin temel işlevselliğini sağlar. Örneğin, Apache Hadoop İP Kaynak Yöneticisi, Apache Hive sorgu dili (HiveQL) ve Mahout kitaplığı bu kategoriye aittir. [HDInsight tarafından sağlanan Apache Hadoop küme sürümlerinde yeni olan](../hdinsight-component-versioning.md)küme bileşenlerinin tam listesi mevcuttur.|
|Özel|Kümenin bir kullanıcısı olarak, toplulukta bulunan veya sizin oluşturduğunuz herhangi bir bileşeni iş yükler veya iş yüklersiniz.|

> [!IMPORTANT]
> HDInsight kümesiyle sağlanan bileşenler tam olarak desteklenir. Microsoft Destek, bu bileşenlerle ilgili sorunları yalıtmaya ve çözmeye yardımcı olur.
>
> Özel bileşenler, sorunu daha fazla gidermenize yardımcı olmak için ticari olarak makul destek alır. Microsoft desteği sorunu çözebilir VEYA bu teknolojiiçin derin uzmanlık bulunan açık kaynak teknolojileri için kullanılabilir kanallardan bağlantı kurmanızı isteyebilir. Örneğin, kullanılabilir birçok topluluk sitesi vardır, örneğin: [HDInsight için MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Ayrıca Apache projeleri, [https://apache.org](https://apache.org)örneğin proje siteleri var: [Hadoop](https://hadoop.apache.org/).

## <a name="understand-default-python-installation"></a>Varsayılan Python yüklemesini anlama

HDInsight Spark kümesi Anaconda kurulumu ile oluşturulur. Kümede anaconda Python 2.7 ve Python 3.5 olmak üzere iki Python yüklemesi vardır. Aşağıdaki tabloda Spark, Livy ve Jupyter için varsayılan Python ayarları gösterilmektedir.

| |Python 2.7|Python 3.5|
|----|----|----|
|Yol|/usr/bin/anaconda/bin|/usr/bin/anaconda/envs/py35/bin|
|Spark|Varsayılan olarak 2,7 olarak ayarlandı|Yok|
|Livy|Varsayılan olarak 2,7 olarak ayarlandı|Yok|
|Jupyter|PySpark çekirdeği|PySpark3 çekirdeği|

## <a name="safely-install-external-python-packages"></a>Harici Python paketlerini güvenli bir şekilde yükleyin

HDInsight kümesi yerleşik Python ortamına bağlıdır, hem Python 2.7 hem de Python 3.5. Bu varsayılan yerleşik ortamlara özel paketlerin doğrudan yüklenmesi beklenmeyen kitaplık sürüm değişikliklerine neden olabilir ve kümeyi daha da bozabilir. Spark uygulamalarınız için özel harici Python paketlerini güvenli bir şekilde yüklemek için aşağıdaki adımları izleyin.

1. Conda kullanarak Python sanal ortamı oluşturun. Sanal ortam, başkalarını kırmadan projeleriniz için yalıtılmış bir alan sağlar. Python sanal ortamını oluştururken, kullanmak istediğiniz python sürümünü belirtebilirsiniz. Python 2.7 ve 3.5'i kullanmak isteseniz bile sanal ortam oluşturmanız gerektiğini unutmayın. Bu, kümenin varsayılan ortamının kırılmadığından emin olmak içindir. Python sanal ortamı oluşturmak için aşağıdaki komut dosyası ile tüm düğümler için kümenizde komut dosyası eylemleri çalıştırın.

    -   `--prefix`bir kozalsanal ortamın yaşadığı bir yol belirtir. Burada belirtilen yola bağlı olarak daha fazla değiştirilmesi gereken birkaç configs vardır. Bu örnekte, kümezaten py35 denilen mevcut bir sanal ortam avarladığı için py35new'yi kullanırız.
    -   `python=`sanal ortam için Python sürümünü belirtir. Bu örnekte, kümenin içinde yerleşik olduğu sürüm le aynı sürüm olan sürüm 3.5'i kullanırız. Sanal ortamı oluşturmak için diğer Python sürümlerini de kullanabilirsiniz.
    -   `anaconda`anaconda paketleri sanal ortamda yüklemek için anaconda olarak package_spec belirtir.
    
    ```bash
    sudo /usr/bin/anaconda/bin/conda create --prefix /usr/bin/anaconda/envs/py35new python=3.5 anaconda --yes
    ```

2. Gerekirse oluşturulan sanal ortama harici Python paketleri yükleyin. Dış Python paketleri yüklemek için aşağıda komut dosyası ile tüm düğümler için kümenizde komut dosyası eylemleri çalıştırın. Sanal ortam klasörüne dosya yazmak için burada sudo ayrıcalığına sahip olmanız gerekir.

    Paket [dizininde](https://pypi.python.org/pypi) kullanılabilir paketlerin tam listesini arayabilirsiniz. Ayrıca diğer kaynaklardan kullanılabilir paketlerin bir listesini alabilirsiniz. Örneğin, [conda-forge](https://conda-forge.org/feedstocks/)aracılığıyla kullanılabilir paketleri yükleyebilirsiniz.

    En son sürümü olan bir kitaplığı yüklemek istiyorsanız aşağıdaki komutu kullanın:

    - Conda kanallarını kullanın:

        -   `seaborn`yüklemek istediğiniz paket adıdır.
        -   `-n py35new`oluşturulan sanal ortam adını belirtin. Sanal ortam oluşturmanıza bağlı olarak adı değiştirdiğinizden emin olun.

        ```bash
        sudo /usr/bin/anaconda/bin/conda install seaborn -n py35new --yes
        ```

    - Veya PyPi repo' `seaborn` `py35new` su, değiştir ve buna karşılık kullanın:
        ```bash
        sudo /usr/bin/anaconda/env/py35new/bin/pip install seaborn
        ```

    Belirli bir sürümü olan bir kitaplık yüklemek istiyorsanız aşağıdaki komutu kullanın:

    - Conda kanallarını kullanın:

        -   `numpy=1.16.1`yüklemek istediğiniz paket adı ve sürümüdür.
        -   `-n py35new`oluşturulan sanal ortam adını belirtin. Sanal ortam oluşturmanıza bağlı olarak adı değiştirdiğinizden emin olun.

        ```bash
        sudo /usr/bin/anaconda/bin/conda install numpy=1.16.1 -n py35new --yes
        ```

    - Veya PyPi repo' `numpy==1.16.1` `py35new` su, değiştir ve buna karşılık kullanın:

        ```bash
        sudo /usr/bin/anaconda/env/py35new/bin/pip install numpy==1.16.1
        ```

    Sanal ortam adını bilmiyorsanız, kümenin baş düğümüne SSH yapabilir ve `/usr/bin/anaconda/bin/conda info -e` tüm sanal ortamları göstermek için çalıştırabilirsiniz.

3. Kıvılcım ve Livy configs değiştirin ve oluşturulan sanal ortama işaret.

    1. Ambari UI'yi açın, Spark2 sayfasına, Configs sekmesine gidin.

        ![Ambari ile Kıvılcım ve Livy config değiştirin](./media/apache-spark-python-package-installation/ambari-spark-and-livy-config.png)

    2. Gelişmiş livy2-env genişletin, alt kısmında ifadeler aşağıda ekleyin. Sanal ortamı farklı bir önek ile yüklediyseniz, yolu buna göre değiştirin.

        ```
        export PYSPARK_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        export PYSPARK_DRIVER_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        ```

        ![Ambari ile Livy config değiştirin](./media/apache-spark-python-package-installation/ambari-livy-config.png)

    3. Gelişmiş spark2-env'i genişletin, alttaki mevcut ihracat PYSPARK_PYTHON deyimini değiştirin. Sanal ortamı farklı bir önek ile yüklediyseniz, yolu buna göre değiştirin.

        ```
        export PYSPARK_PYTHON=${PYSPARK_PYTHON:-/usr/bin/anaconda/envs/py35new/bin/python}
        ```

        ![Ambari ile Kıvılcım config değiştirin](./media/apache-spark-python-package-installation/ambari-spark-config.png)

    4. Değişiklikleri kaydedin ve etkilenen hizmetleri yeniden başlatın. Bu değişikliklerin Spark2 hizmetinin yeniden başlatılması gerekir. Ambari UI gerekli bir yeniden başlatma anımsatıcısını ister, etkilenen tüm hizmetleri yeniden başlatmak için Yeniden Başlat'ı tıklatın.

        ![Ambari ile Kıvılcım config değiştirin](./media/apache-spark-python-package-installation/ambari-restart-services.png)

4. Jupyter'da yeni oluşturulan sanal ortamı kullanmak isterseniz. Jupyter configs'i değiştirip Jupyter'ı yeniden başlatmalısın. Jupyter'ı yeni oluşturulan sanal ortama çekmek için aşağıdaki ifadeyle tüm üstbilgi düğümlerinde komut dosyası eylemleri çalıştırın. Sanal ortamınız için belirttiğiniz önek yolunu değiştirdiğinizden emin olun. Bu komut dosyası eylemini çalıştırdıktan sonra, bu değişikliği kullanılabilir hale getirmek için Ambari UI aracılığıyla Jupyter hizmetini yeniden başlatın.

    ```bash
    sudo sed -i '/python3_executable_path/c\ \"python3_executable_path\" : \"/usr/bin/anaconda/envs/py35new/bin/python3\"' /home/spark/.sparkmagic/config.json
    ```

    Jupyter Notebook'taki Python ortamını aşağıdaki kodu çalıştırarak iki kez onaylayabilirsiniz:

    ![Jupyter Notebook'ta Python sürümünü kontrol edin](./media/apache-spark-python-package-installation/check-python-version-in-jupyter.png)

## <a name="known-issue"></a>Bilinen sorun

Anaconda sürüm 4.7.11, 4.7.12 ve 4.8.0 için bilinen bir hata vardır. Komut dosyası eylemlerinizi asılı `"Collecting package metadata (repodata.json): ...working..."` ve `"Python script has been killed due to timeout after waiting 3600 secs"`başarısız görürseniz. [Bu komut dosyasını](https://gregorysfixes.blob.core.windows.net/public/fix-conda.sh) karşıdan yükleyebilir ve sorunu gidermek için tüm düğümlerde komut dosyası eylemleri olarak çalıştırabilirsiniz.

Anaconda sürümünüzü denetlemek için, küme üstbilgi düğümüne SSH yapabilir ve çalıştırabilirsiniz. `/usr/bin/anaconda/bin/conda --v`

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)
* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
