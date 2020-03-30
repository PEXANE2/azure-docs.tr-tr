---
title: Azure HDInsight'ta Jupyter'ı yerel olarak yükleyin ve Spark'a bağlanın
description: Jupyter dizüstü bilgisayarını bilgisayarınıza yerel olarak nasıl yükleyeceğinizi ve bir Apache Spark kümesine nasıl bağlayabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/07/2019
ms.openlocfilehash: 225ee7028b9610a4974f9bee05da667d78d3355e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73903745"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter dizüstü bilgisayarınızı yükleyin ve HDInsight'ta Apache Spark'a bağlanın

Bu makalede, özel PySpark (Python için) ve Apache Spark (Scala için) çekirdekleri Spark sihirli ile jupyter dizüstü bilgisayar yüklemek için nasıl öğrenmek ve bir HDInsight kümesine dizüstü bağlamak. Jupyter'ı yerel bilgisayarınıza yüklemek için bir dizi neden olabilir ve bazı zorluklar da olabilir. Bu konuda daha fazla bilgi için, bu makalenin sonunda [bilgisayarıma Neden Jupyter yüklemeliyim](#why-should-i-install-jupyter-on-my-computer) bölümüne bakın.

Jupyter'ı yüklemek ve HDInsight'ta Apache Spark'a bağlanmak için dört önemli adım vardır.

* Kıvılcım kümesini yapılandırın.
* Jupyter dizüstü bilgisayar yükleyin.
* Kıvılcım büyüsü ile PySpark ve Kıvılcım çekirdekleri yükleyin.
* Spark büyüsünü HDInsight'taki Spark kümesine erişmek için yapılandırın.

HDInsight kümesine sahip Jupyter dizüstü bilgisayarlar için özel çekirdekler ve Spark sihri hakkında daha fazla bilgi için, [HDInsight'ta Apache Spark Linux kümelerine sahip Jupyter dizüstü bilgisayarlar için kullanılabilir Çekirdekler'e](apache-spark-jupyter-notebook-kernels.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight üzerinde bir Apache Spark kümesi. Yönergeler için bkz. [Azure HDInsight'ta Apache Spark kümeleri oluşturma](apache-spark-jupyter-spark-sql.md). Bu, jupyter dizüstü bilgisayarı dizüstü bilgisayar yüklendikten sonra bir HDInsight kümesine bağlamak için bir ön koşuldur.

* HDInsight üzerinde Spark ile Jupyter Notebook kullanma bilgisi.

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter dizüstü bilgisayarını bilgisayarınıza yükleme

Jupyter dizüstü bilgisayarları yüklemeden önce Python'u yüklemeniz gerekir. [Anaconda dağıtımı](https://www.anaconda.com/download/) hem Python'u hem de Jupyter Notebook'u yükler.

Platformunuz için [Anaconda yükleyicisini](https://www.anaconda.com/download/) indirin ve kurulumu çalıştırın. Kurulum sihirbazını çalıştırırken, PATH değişkeninize Anaconda ekleme seçeneğini seçtiğinizden emin olun.  Ayrıca bakınız, [Anaconda kullanarak Jupyter yükleme.](https://jupyter.readthedocs.io/en/latest/install.html)

## <a name="install-spark-magic"></a>Kıvılcım büyüsünü yükleyin

1. Kıvılcım sihirli yüklemek için aşağıdaki komutlardan birini girin. Ayrıca bakınız, [kıvılcımlı belgeler.](https://github.com/jupyter-incubator/sparkmagic#installation)

    |Küme sürümü | Komutu yükle |
    |---|---|
    |v3.6 ve v3.5 |`pip install sparkmagic==0.13.1`|
    |v3.4|`pip install sparkmagic==0.2.3`|

1. Aşağıdaki `ipywidgets` komutu çalıştırarak düzgün bir şekilde yüklendiğinden emin olun:

    ```cmd
    jupyter nbextension enable --py --sys-prefix widgetsnbextension
    ```

## <a name="install-pyspark-and-spark-kernels"></a>PySpark ve Kıvılcım çekirdeklerini yükleyin

1. Aşağıdaki `sparkmagic` komutu girerek nerede yüklü olduğunu belirleyin:

    ```cmd
    pip show sparkmagic
    ```

    Ardından çalışma dizininizi yukarıdaki komutla tanımlanan konuma değiştirin.

1. Yeni çalışma dizininizden, istediğiniz çekirdeği(ler) yüklemek için aşağıdaki komutlardan birini veya birkaçını girin:

    |Çekirdek | Komut |
    |---|---|
    |Spark|`jupyter-kernelspec install sparkmagic/kernels/sparkkernel`|
    |Kıvılcım|`jupyter-kernelspec install sparkmagic/kernels/sparkrkernel`|
    |PySpark|`jupyter-kernelspec install sparkmagic/kernels/pysparkkernel`|
    |PySpark3|`jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel`|

1. İsteğe bağlı. Sunucu uzantısını etkinleştirmek için aşağıdaki komutu girin:

    ```cmd
    jupyter serverextension enable --py sparkmagic
    ```

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>HDInsight Spark kümesine bağlanmak için Spark büyüsünü yapılandırın

Bu bölümde, bir Apache Spark kümesine bağlanmak için daha önce yüklediğiniz Kıvılcım büyüsünü yapılandırırsınız.

1. Python kabuğunu aşağıdaki komutla başlatın:

    ```cmd
    python
    ```

2. Jupyter yapılandırma bilgileri genellikle kullanıcıların ev dizininde depolanır. Ev dizinini tanımlamak için aşağıdaki komutu girin ve **.sparkmagic**adlı bir klasör oluşturun.  Tam yol çıktı edilecektir.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. Klasör `.sparkmagic`içinde, **config.json** adlı bir dosya oluşturun ve içine aşağıdaki JSON snippet ekleyin.  

    ```json
    {
      "kernel_python_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "kernel_scala_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "custom_headers" : {
        "X-Requested-By": "livy"
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```

4. Dosyada aşağıdaki leri yapın:

    |Şablon değeri | Yeni değer |
    |---|---|
    |{KULLANıCı Adı}|Küme giriş, varsayılan `admin`.|
    |{CLUSTERDNSNAME}|Küme adı|
    |{BASE64ENCODEDPASSWORD}|Gerçek parolanız için base64 kodlanmış bir parola.  Bir base64 şifre oluşturabilirsiniz [https://www.url-encode-decode.com/base64-encode-decode/](https://www.url-encode-decode.com/base64-encode-decode/).|
    |`"livy_server_heartbeat_timeout_seconds": 60`|Kullanıyorsanız `sparkmagic 0.12.7` saklayın (v3.5 ve v3.6 kümeleri).  Kullanıyorsanız `sparkmagic 0.2.3` (v3.4 kümeleri), `"should_heartbeat": true`değiştirin.|

    Örnek [config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json)tam bir örnek dosya görebilirsiniz.

   > [!TIP]  
   > Oturumların sızdırılmaması için sinyaller gönderilir. Bilgisayar uyku moduna geçtiğinde veya kapatıldığında, sinyal gönderilmez ve oturum temizlenir. V3.4 kümeleri için, bu davranışı devre dışı kakmak isterseniz, `livy.server.interactive.heartbeat.timeout` Ambari `0` UI'den Livy config'ini ayarlayabilirsiniz. V3.5 kümeleri için, yukarıdaki 3,5 yapılandırmasını ayarlamazsanız, oturum silinmez.

5. Jupyter'ı başlat. Komut isteminden aşağıdaki komutu kullanın.

    ```cmd
    jupyter notebook
    ```

6. Çekirdeklerle birlikte mevcut Olan Kıvılcım büyüsünü kullanabileceğinizi doğrulayın. Aşağıdaki adımları uygulayın.

    a. Yeni bir not defteri oluşturun. Sağ köşeden **Yeni'yi**seçin. Varsayılan çekirdek **Python 2** veya **Python 3'u** ve yüklediğiniz çekirdekleri görmeniz gerekir. Gerçek değerler yükleme seçeneklerinize bağlı olarak değişebilir.  **PySpark'ı**seçin.

    ![Jupyter dizüstü bilgisayarda mevcut çekirdekler](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels-notebook.png "Jupyter not defterinde çekirdekler")

    > [!IMPORTANT]  
    > Herhangi bir hata için kabuğunuzu **yeni** gözden geçirin' i seçtikten sonra.  Hatayı `TypeError: __init__() got an unexpected keyword argument 'io_loop'` görürseniz, Tornado'nun belirli sürümlerinde bilinen bir sorunla karşılaşabilirsiniz.  Eğer öyleyse, çekirdeği durdurun ve ardından aşağıdaki komutla Tornado `pip install tornado==4.5.3`yüklemenizi düşürün: .

    b. Aşağıdaki kod parçacıklarını çalıştırın.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Çıktıyı başarıyla alabilirseniz, HDInsight kümesine olan bağlantınız sınanabilir.

    Farklı bir kümeye bağlanmak için not defteri yapılandırmasını güncelleştirmek istiyorsanız, yukarıda Adım 3'te gösterildiği gibi config.json değerlerini yeni kümeyle güncelleştirin.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Jupyter'ı neden bilgisayarıma yüklemeliyim?

Jupyter'ı bilgisayarınıza yükleyip HDInsight'taki bir Apache Spark kümesine bağlamak istemenizin birkaç nedeni olabilir.

* Azure HDInsight'ta Spark kümesinde zaten mevcut olsa da, Jupyter'ı bilgisayarınıza yüklemek dizüstü bilgisayarlarınızı yerel olarak oluşturma, uygulamanızı çalışan bir kümeyle test etme ve ardından kümeye not defterleri. Not defterlerini kümeye yüklemek için, bunları çalışan Jupyter not defterini veya kümeyi kullanarak yükleyebilir veya kümeyle ilişkili depolama hesabındaki /HdiNotebooks klasörüne kaydedebilirsiniz. Not defterlerinin kümede nasıl depolandığı hakkında daha fazla bilgi için [bkz.](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)
* Yerel olarak kullanılabilen dizüstü bilgisayarlarla, uygulama gereksiniminize bağlı olarak farklı Spark kümelerine bağlanabilirsiniz.
* Bir kaynak kontrol sistemi uygulamak ve dizüstü bilgisayarlar için sürüm denetimine sahip olmak için GitHub'ı kullanabilirsiniz. Ayrıca, birden çok kullanıcının aynı not defteriyle çalışabileceği ortak bir ortama da sahip olabilirsiniz.
* Bir küme bile olmadan dizüstü bilgisayarlarla yerel olarak çalışabilirsiniz. Not defterlerinizi el ile veya geliştirme ortamına göre değil, not defterlerinizi test etmek için yalnızca bir kümeye ihtiyacınız vardır.
* Kendi yerel geliştirme ortamınızı yapılandırmak, kümedeki Jupyter yüklemesini yapılandırmaktan daha kolay olabilir.  Bir veya daha fazla uzak küme yapılandırmadan yerel olarak yüklediğiniz tüm yazılımlardan yararlanabilirsiniz.

> [!WARNING]  
> Jupyter yerel bilgisayarınızda yüklü olduğu için, birden çok kullanıcı aynı Spark kümesinde aynı not defterini aynı anda çalıştırabilir. Böyle bir durumda, birden fazla Livy oturumları oluşturulur. Bir sorunla karşılaştıysanız ve hata ayıklamak istiyorsanız, hangi Livy oturumunun hangi kullanıcıya ait olduğunu izlemek karmaşık bir görev olacaktır.  

## <a name="next-steps"></a>Sonraki adımlar

* [Genel Bakış: Azure HDInsight’ta Apache Spark](apache-spark-overview.md)
* [BI ile Apache Spark: HDInsight'ta Spark'ı BI araçlarıyla kullanarak etkileşimli veri analizi yapın](apache-spark-use-bi-tools.md)
* [Machine Learning ile Apache Spark: HVAC verilerini kullanarak bina sıcaklığını analiz etmek için HDInsight'ta Kıvılcım'ı kullanın](apache-spark-ipython-notebook-machine-learning.md)
