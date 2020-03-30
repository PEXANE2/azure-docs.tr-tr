---
title: Visual Studio için R Tools'tan iş gönderme - Azure HDInsight
description: Yerel Visual Studio makinenizden Bir HDInsight kümesine R işlerini gönderin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: 73d1478ec2d6c90428f22a30ec82634df115d2f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435262"
---
# <a name="submit-jobs-from-r-tools-for-visual-studio"></a>Visual Studio için R Araçları’ndan iş gönderme

[R Tools for Visual Studio](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019) (RTVS), Visual Studio [2017 ve Visual Studio 2015](https://www.visualstudio.com/downloads/) [Update 3](https://go.microsoft.com/fwlink/?LinkId=691129) veya üzeri sürümler için Topluluk (ücretsiz), Profesyonel ve Kurumsal sürümleri için ücretsiz, açık kaynak uzantısıdır. RTVS [Visual Studio 2019](https://docs.microsoft.com/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects?view=vs-2019)için kullanılamaz.

RTVS, [R Interactive penceresi](https://docs.microsoft.com/visualstudio/rtvs/interactive-repl) (REPL), intellisense (kod tamamlama), ggplot2 ve ggviz, R kodu hata [ayıklama](https://docs.microsoft.com/visualstudio/rtvs/debugging)ve daha fazlası gibi R kitaplıkları aracılığıyla [görselleştirme çizimi](https://docs.microsoft.com/visualstudio/rtvs/visualizing-data) gibi araçlar sunarak R iş akışınızı artırır.

## <a name="set-up-your-environment"></a>Ortamınızı ayarlama

1. [Visual Studio için R Araçlarını yükleyin.](/visualstudio/rtvs/installing-r-tools-for-visual-studio)

    ![Visual Studio 2017'de RTV'lerin kurulumu](./media/r-server-submit-jobs-r-tools-vs/install-r-tools-for-vs.png)

2. Veri *bilimi ve analitik uygulamalar* iş yükünü seçin, ardından **R dil desteği,** **R geliştirme için Runtime desteği**ve Microsoft R **İstemci** seçeneklerini seçin.

3. SSH kimlik doğrulaması için ortak ve özel anahtarlara sahip olmanız gerekir.
   <!-- {TODO tbd, no such file yet}[use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-windows.md) -->

4. ML [Server'ı](https://msdn.microsoft.com/microsoft-r/rserver-install-windows) makinenize yükleyin. ML Server [`RevoScaleR`](https://msdn.microsoft.com/microsoft-r/scaler/scaler) ve `RxSpark` işlevleri sağlar.

5. Yerel istemcinizden HDInsight kümenize işlevleri `RevoScaleR` çalıştırmak için bir bilgi bağlamı sağlamak için [PuTTY'yi](https://www.putty.org/) yükleyin.

6. Veri Bilimi Ayarlarını, R araçları için çalışma alanınız için yeni bir düzen sağlayan Visual Studio ortamınıza uygulama seçeneğiniz vardır.
   1. Geçerli Visual Studio ayarlarınızı kaydetmek **için, Araçlar > Alma ve Dışa Aktarma Ayarları** komutunu kullanın, ardından **seçili ortam ayarlarını dışa** aktar'ı seçin ve bir dosya adı belirtin. Bu ayarları geri yüklemek için aynı komutu kullanın ve **seçili ortam ayarlarını içe aktar'ı**seçin.

   2. **R Tools** menü öğesine gidin ve ardından Veri **Bilimi Ayarları'nı seçin...**.

       ![Visual Studio Veri Bilimi Ayarları](./media/r-server-submit-jobs-r-tools-vs/data-science-settings.png)

      > [!NOTE]  
      > Adım 1'deki yaklaşımı kullanarak, **Veri Bilimi Ayarları** komutunu yinelemek yerine kişiselleştirilmiş veri bilimci düzeninizi kaydedebilir ve geri yükleyebilirsiniz.

## <a name="execute-local-r-methods"></a>Yerel R yöntemlerini yürütme

1. HDInsight ML Hizmetleri kümenizi oluşturun.
2. [RTVS uzantısını](https://docs.microsoft.com/visualstudio/rtvs/installation)yükleyin.
3. Örnekleri [zip dosyası](https://github.com/Microsoft/RTVS-docs/archive/master.zip)indirin.
4. Visual `examples/Examples.sln` Studio'da çözümü başlatmak için açın.
5. Dosyayı `1-Getting Started with R.R` çözüm `A first look at R` klasöründe açın.
6. Dosyanın üst kısmından başlayarak, her satırı teker teker R Interactive penceresine göndermek için Ctrl+Enter tuşuna basın. Bazı satırların paketleri yüklemesi biraz zaman alabilir.
    * Alternatif olarak, R dosyasındaki (Ctrl+A) tüm satırları seçebilir, ardından tümünü çalıştırabilir (Ctrl+Enter) veya araç çubuğundaki Etkileşimli Yürüt simgesini seçebilirsiniz.

        ![Visual Studio interaktif yürütmek](./media/r-server-submit-jobs-r-tools-vs/execute-interactive1.png)

7. Komut dosyasındaki tüm satırları çalıştırdıktan sonra, buna benzer bir çıktı görmeniz gerekir:

    ![Visual Studio çalışma alanı R araçları](./media/r-server-submit-jobs-r-tools-vs/visual-studio-workspace.png)

## <a name="submit-jobs-to-an-hdinsight-ml-services-cluster"></a>İşlerini HDInsight ML Hizmetleri kümesine gönderme

PuTTY ile donatılmış bir Windows bilgisayarından microsoft ML Server/Microsoft R Client kullanarak, yerel `RevoScaleR` istemcinizden HDInsight kümenize dağıtılmış işlevleri çalıştıracak bir bilgi işlem bağlamı oluşturabilirsiniz. Kullanıcı `RxSpark` adınızı, Apache Hadoop kümesinin kenar düğümünüzü, SSH anahtarlarını vb. belirterek bilgi işlem bağlamını oluşturmak için kullanın.

1. HDInsight'taki ML Hizmetleri kenar `CLUSTERNAME-ed-ssh.azurehdinsight.net` düğümü `CLUSTERNAME` adresi, ML Hizmetleri kümenizin adıdır.

1. Aşağıdaki kodu Visual Studio'daki R Interactive penceresine yapıştırarak kurulum değişkenlerinin değerlerini ortamınızla eşleşecek şekilde değiştirin.

    ```R
    # Setup variables that connect the compute context to your HDInsight cluster
    mySshHostname <- 'r-cluster-ed-ssh.azurehdinsight.net ' # HDI secure shell hostname
    mySshUsername <- 'sshuser' # HDI SSH username
    mySshClientDir <- "C:\\Program Files (x86)\\PuTTY"
    mySshSwitches <- '-i C:\\Users\\azureuser\\r.ppk' # Path to your private ssh key
    myHdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep = "/")
    myShareDir <- paste("/var/RevoShare", mySshUsername, sep = "/")
    mySshProfileScript <- "/usr/lib64/microsoft-r/3.3/hadoop/RevoHadoopEnvVars.site"

    # Create the Spark Cluster compute context
    mySparkCluster <- RxSpark(
          sshUsername = mySshUsername,
          sshHostname = mySshHostname,
          sshSwitches = mySshSwitches,
          sshProfileScript = mySshProfileScript,
          consoleOutput = TRUE,
          hdfsShareDir = myHdfsShareDir,
          shareDir = myShareDir,
          sshClientDir = mySshClientDir
    )

    # Set the current compute context as the Spark compute context defined above
    rxSetComputeContext(mySparkCluster)
    ```

   ![bağlamı ayarlayarak apache kıvılcım](./media/r-server-submit-jobs-r-tools-vs/apache-spark-context.png)

1. R Interactive penceresinde aşağıdaki komutları uygulayın:

    ```R
    rxHadoopCommand("version") # should return version information
    rxHadoopMakeDir("/user/RevoShare/newUser") # creates a new folder in your storage account
    rxHadoopCopy("/example/data/people.json", "/user/RevoShare/newUser") # copies file to new folder
    ```

    Aşağıdakine benzer bir çıktı görmeniz gerekir:

    ![Başarılı rx](./media/r-server-submit-jobs-r-tools-vs/successful-rx-commands.png) komut yürütme a
1. Dosyanın `rxHadoopCopy` `people.json` örnek veri klasöründen yeni oluşturulan `/user/RevoShare/newUser` klasöre başarıyla kopyaladığını doğrulayın:

    1. Azure'daki HDInsight ML Hizmetleri küme bölmesinizden, sol menüden **Depolama hesaplarını** seçin.

        ![Azure HDInsight Depolama hesapları](./media/r-server-submit-jobs-r-tools-vs/hdinsight-storage-accounts.png)

    2. Kapsayıcı/dizin adını not ederek kümenizin varsayılan depolama hesabını seçin.

    3. Depolama hesabı bölmenizdeki sol menüden **Kapsayıcılar'ı** seçin.

        ![Azure HDInsight Depolama kapsayıcıları](./media/r-server-submit-jobs-r-tools-vs/hdi-storage-containers.png)

    4. Kümenizin kapsayıcı adını seçin, **kullanıcı** klasörüne göz atın (listenin en altında *daha fazla yükle'yi* tıklatmanız gerekebilir), ardından *RevoShare'i*, sonra **yeni Kullanıcı'yı**seçin. Dosya `people.json` `newUser` klasörde görüntülenmelidir.

        ![HDInsight kopyalanan dosya klasörü konumu](./media/r-server-submit-jobs-r-tools-vs/hdinsight-copied-file.png)

1. Geçerli Apache Spark bağlamını kullanmayı bitirdikten sonra, bunu durdurmanız gerekir. Aynı anda birden çok bağlamı çalıştıramazsınız.

    ```R
    rxStopEngine(mySparkCluster)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight'ta ML Hizmetleri için bilgi işlem bağlam ı seçenekleri](r-server-compute-contexts.md)
* [ScaleR ve SparkR'ın birleştirilmesi,](../hdinsight-hadoop-r-scaler-sparkr.md) havayolu uçuş gecikme tahminlerine bir örnek sağlar.
