---
title: Azure HDInsight üzerinde jupi ile Python paketleri için betik eylemi
description: Betik eyleminin nasıl kullanılacağına ilişkin adım adım yönergeler, HDInsight Spark kümeleri ile birlikte sunulan jupi not defterlerini dış Python paketleri kullanacak şekilde yapılandırma.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/19/2019
ms.openlocfilehash: 109ac20d8a3d3dc87b4a83165c0e6c24808c1340
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75529652"
---
# <a name="safely-manage-python-environment-on-azure-hdinsight-using-script-action"></a>Betik Eylemi kullanarak Azure HDInsight üzerinde Python ortamını güvenli bir şekilde yönetin

> [!div class="op_single_selector"]
> * [Hücre Magic 'i kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Betik eylemini kullanma](apache-spark-python-package-installation.md)

HDInsight, Spark kümesinde, Anaconda Python 2,7 ve Python 3,5 ' de iki yerleşik Python yüklemelerine sahiptir. Bazı durumlarda, müşterilerin, dış Python paketleri veya başka bir Python sürümü yükleme gibi Python ortamını özelleştirmesi gerekir. Bu makalede, HDInsight 'ta bir [Apache Spark](https://spark.apache.org/) kümesi için Python ortamlarını güvenli bir şekilde yönetmeye yönelik en iyi uygulama gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü alma](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md).

   > [!NOTE]  
   > HDInsight Linux üzerinde henüz bir Spark kümeniz yoksa, küme oluşturma sırasında betik eylemleri çalıştırabilirsiniz. [Özel Betik eylemlerinin kullanımı](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)hakkındaki belgeleri ziyaret edin.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>HDInsight kümelerinde kullanılan açık kaynaklı yazılım desteği

Microsoft Azure HDInsight hizmeti Apache Hadoop etrafında oluşturulmuş açık kaynaklı teknolojilerin ekosistemini kullanır. Microsoft Azure, açık kaynaklı teknolojiler için genel bir destek düzeyi sağlar. Daha fazla bilgi için bkz. [Azure DESTEĞI SSS Web sitesi](https://azure.microsoft.com/support/faq/). HDInsight hizmeti, yerleşik bileşenler için ek bir destek düzeyi sağlar.

HDInsight hizmetinde bulunan iki tür açık kaynaklı bileşen vardır:

* **Yerleşik bileşenler** -bu bileşenler HDInsight kümelerinde önceden yüklenir ve kümenin temel işlevlerini sağlar. Örneğin, Apache Hadoop YARN Kaynak Yöneticisi, Apache Hive sorgu dili (HiveQL) ve Mahout kitaplığı bu kategoriye aittir. Tüm küme bileşenleri listesi, [HDInsight tarafından sağlanan Apache Hadoop kümesi sürümlerindeki yenilikler](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning)bölümünde bulunur.
* **Özel bileşenler** -kümenin bir kullanıcısı olarak, kuruluşunuzda bulunan veya sizin tarafınızdan oluşturulan herhangi bir bileşeni iş yükünüze yükleyebilir veya kullanabilirsiniz.

> [!IMPORTANT]
> HDInsight kümesiyle birlikte sunulan bileşenler tam olarak desteklenmektedir. Microsoft Desteği, bu bileşenlerle ilgili sorunları yalıtmaya ve çözmeye yardımcı olur.
>
> Özel bileşenler, sorunu gidermeye yardımcı olmak için ticari açıdan makul destek alır. Microsoft desteği sorunu çözebiliyor olabilir veya bu teknoloji için derin uzmanlığın bulunduğu açık kaynaklı teknolojiler için kullanılabilir kanalları ister. Örneğin, şu şekilde kullanılabilecek birçok topluluk sitesi vardır: [HDInsight Için MSDN Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Ayrıca Apache projelerinin [https://apache.org](https://apache.org)proje siteleri vardır, örneğin: [Hadoop](https://hadoop.apache.org/).

## <a name="understand-default-python-installation"></a>Varsayılan Python yüklemesini anlama

HDInsight Spark kümesi, Anaconda yüklemesiyle oluşturulur. Kümede, Anaconda Python 2,7 ve Python 3,5 olmak üzere iki Python yüklemesi vardır. Aşağıdaki tabloda Spark, Livy ve Jupyıter için varsayılan Python ayarları gösterilmektedir.

| |Python 2.7|Python 3,5|
|----|----|----|
|Yol|/usr/bin/anaconda/bin|/usr/bin/anaconda/envs/py35/bin|
|Spark|Varsayılan olarak 2,7 ayarlanır|Yok|
|Livy|Varsayılan olarak 2,7 ayarlanır|Yok|
|Jupyter|PySpark çekirdeği|PySpark3 çekirdeği|

## <a name="safely-install-external-python-packages"></a>Dış Python paketlerini güvenle yükler

HDInsight kümesi, Python 2,7 ve Python 3,5 yerleşik Python ortamına bağlıdır. Özel paketleri bu varsayılan yerleşik ortamlara doğrudan yüklemek beklenmeyen kitaplık sürümü değişikliklerine neden olabilir ve kümeyi daha fazla kesebilir. Spark uygulamalarınız için özel dış Python paketlerini güvenli bir şekilde yüklemek için aşağıdaki adımları izleyin.

1. Conda kullanarak Python sanal ortamı oluşturun. Sanal bir ortam, başka bir yere kırçıkmadan projeleriniz için yalıtılmış bir alan sağlar. Python sanal ortamını oluştururken, kullanmak istediğiniz Python sürümünü belirtebilirsiniz. Python 2,7 ve 3,5 ' i kullanmak istiyor olsanız bile yine de sanal ortam oluşturmanız gerektiğini unutmayın. Bu, kümenin varsayılan ortamının bromıyor olduğundan emin olmak için kullanılır. Python sanal ortamı oluşturmak için aşağıdaki betiği içeren tüm düğümler için kümenizde betik eylemleri çalıştırın. 

    -   `--prefix` bir Conda sanal ortamının yaşadığı bir yolu belirtir. Burada belirtilen yola göre daha fazla değiştirilmesi gereken birkaç yapılandırma vardır. Bu örnekte, küme zaten py35 adlı mevcut bir sanal ortama sahip olduğu için py35new kullanıyoruz.
    -   `python=` sanal ortam için Python sürümünü belirtir. Bu örnekte, içinde yerleşik olarak bulunan kümeyle aynı sürüme sahip sürüm 3,5 ' i kullanırız. Sanal ortam oluşturmak için diğer Python sürümlerini de kullanabilirsiniz.
    -   `anaconda`, Anaconda paketlerinin sanal ortama yüklenmesi için package_spec belirtir.
    
    ```bash
    sudo /usr/bin/anaconda/bin/conda create --prefix /usr/bin/anaconda/envs/py35new python=3.5 anaconda --yes 
    ```

2. Gerekirse, oluşturulan sanal ortama dış Python paketleri yükler. Dış Python paketlerini yüklemek için aşağıdaki betiği içeren tüm düğümler için kümenizde betik eylemleri çalıştırın. Sanal ortam klasörüne dosya yazmak için burada sudo ayrıcalığına sahip olmanız gerekir.

    Kullanılabilir paketlerin tüm listesi için [paket dizininde](https://pypi.python.org/pypi) arama yapabilirsiniz. Ayrıca, diğer kaynaklardan kullanılabilir paketlerin bir listesini alabilirsiniz. Örneğin, [Conda-Forge](https://conda-forge.org/feedstocks/)aracılığıyla kullanıma sunulan paketleri yükleyebilirsiniz.

    -   `seaborn` yüklemek istediğiniz paket adıdır.
    -   `-n py35new` yeni oluşturulan sanal ortam adını belirtin. Sanal ortam oluşturmaya göre adı karşılık geldiğinden emin olun.

    ```bash
    sudo /usr/bin/anaconda/bin/conda install seaborn -n py35new --yes
    ```

    sanal ortam adını bilmiyorsanız, kümenin baş düğümüne SSH gönderebilir ve tüm sanal ortamları göstermek için `/usr/bin/anaconda/bin/conda info -e` çalıştırabilirsiniz.

3. Spark ve Livy yapılandırmalarını değiştirip oluşturulan sanal ortama işaret edin.

    1. Ambarı Kullanıcı arabirimini açın, Spark2 Page, configs sekmesine gidin.
    
        ![Spark ve Livy yapılandırmasını, ambarı aracılığıyla değiştirme](./media/apache-spark-python-package-installation/ambari-spark-and-livy-config.png)
 
    2. Advanced livy2-env ' yi genişletin, altta aşağıdaki deyimleri ekleyin. Sanal ortamı farklı bir önek ile yüklediyseniz, yolu karşılık gelenle değiştirin.

        ```
        export PYSPARK_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        export PYSPARK_DRIVER_PYTHON=/usr/bin/anaconda/envs/py35new/bin/python
        ```

        ![Ambarı aracılığıyla Livy yapılandırmasını değiştirme](./media/apache-spark-python-package-installation/ambari-livy-config.png)

    3. Advanced spark2-env ' ı genişletin, en altta bulunan dışarı aktarma PYSPARK_PYTHON ifadesini değiştirin. Sanal ortamı farklı bir önek ile yüklediyseniz, yolu karşılık gelenle değiştirin.

        ```
        export PYSPARK_PYTHON=${PYSPARK_PYTHON:-/usr/bin/anaconda/envs/py35new/bin/python}
        ```

        ![Spark config 'i ambarı aracılığıyla değiştirme](./media/apache-spark-python-package-installation/ambari-spark-config.png)

    4. Değişiklikleri kaydedin ve etkilenen hizmetleri yeniden başlatın. Bu değişikliklerin Spark2 hizmetinin yeniden başlatılması gerekir. Ambarı Kullanıcı arabirimi gerekli bir yeniden başlatma anımsatıcısı ister, tüm etkilenen hizmetleri yeniden başlatmak için yeniden Başlat 'a tıklayın.

        ![Spark config 'i ambarı aracılığıyla değiştirme](./media/apache-spark-python-package-installation/ambari-restart-services.png)
 
4.  Jupyıter üzerinde yeni oluşturulan sanal ortamı kullanmak istiyorsanız. Jupi yapılandırmalarını öğesini değiştirmeniz ve jupyıter 'ı yeniden başlatmanız gerekir. Jupi 'yi yeni oluşturulan sanal ortama işaret etmek için aşağıdaki deyimle tüm üst bilgi düğümlerinde betik eylemlerini çalıştırın. Sanal ortamınız için belirttiğiniz önek için yolu değiştirdiğinizden emin olun. Bu betik eylemini çalıştırdıktan sonra, bu değişikliği kullanılabilir hale getirmek için ambarı Kullanıcı arabirimi aracılığıyla Jupyıter hizmetini yeniden başlatın.

    ```
    sudo sed -i '/python3_executable_path/c\ \"python3_executable_path\" : \"/usr/bin/anaconda/envs/py35new/bin/python3\"' /home/spark/.sparkmagic/config.json
    ```

    Aşağıdaki kodu çalıştırarak Jupyter Notebook Python ortamını iki kez doğrulayabilirsiniz:

    ![Jupyter Notebook Python sürümünü denetle](./media/apache-spark-python-package-installation/check-python-version-in-jupyter.png)

## <a name="known-issue"></a>Bilinen sorun

Anaconda Version 4.7.11 ve 4.7.12 için bilinen bir hata vardır. Betik eylemlerinizin `"Collecting package metadata (repodata.json): ...working..."` askıda olduğunu ve `"Python script has been killed due to timeout after waiting 3600 secs"`başarısız olduğunu görürseniz. Sorunu çözebilmeniz için [bu betiği](https://gregorysfixes.blob.core.windows.net/public/fix-conda.sh) indirebilir ve tüm düğümlerde betik eylemleri olarak çalıştırabilirsiniz.

Anaconda sürümünüzü denetlemek için, küme üst bilgisi düğümüne SSH verebilir ve `/usr/bin/anaconda/bin/conda --v`çalıştırabilirsiniz.

## <a name="seealso"></a>Ayrıca bkz.

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)

### <a name="scenarios"></a>Senaryolar

* [BI ile Apache Spark: bı araçlarıyla HDInsight 'ta Spark kullanarak etkileşimli veri çözümlemesi gerçekleştirme](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak oluşturma sıcaklığını çözümlemek için HDInsight 'ta Spark kullanma](apache-spark-ipython-notebook-machine-learning.md)
* [Machine Learning Apache Spark: yemek İnceleme sonuçlarını tahmin etmek için HDInsight 'ta Spark kullanma](apache-spark-machine-learning-mllib-ipython.md)
* [HDInsight 'ta Apache Spark kullanarak Web sitesi günlüğü Analizi](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Uygulamaları oluşturma ve çalıştırma

* [Scala kullanarak tek başına uygulama oluşturma](apache-spark-create-standalone-application.md)
* [Apache Livy kullanarak Apache Spark kümesinde işleri uzaktan çalıştırma](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Araçlar ve uzantılar

* [HDInsight 'ta Apache Spark kümelerinde jupi Not defterleri ile dış paketleri kullanma](apache-spark-jupyter-notebook-use-external-packages.md)
* [Spark Scala uygulamaları oluşturmak ve göndermek amacıyla IntelliJ IDEA için HDInsight Araçları Eklentisini kullanma](apache-spark-intellij-tool-plugin.md)
* [Apache Spark uygulamalarında uzaktan hata ayıklama için IntelliJ fıkır için HDInsight Araçları eklentisini kullanın](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [HDInsight 'ta Apache Spark kümesiyle Apache Zeppelin not defterlerini kullanma](apache-spark-zeppelin-notebook.md)
* [HDInsight için Apache Spark kümesindeki Jupyter Not defteri için kullanılabilir kernels](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter’i bilgisayarınıza yükleme ve bir HDInsight Spark kümesine bağlanma](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Kaynakları yönetme

* [Azure HDInsight’ta Apache Spark kümesi kaynaklarını yönetme](apache-spark-resource-manager.md)
* [HDInsight’ta bir Apache Spark kümesinde çalışan işleri izleme ve hata ayıklama](apache-spark-job-debugging.md)
