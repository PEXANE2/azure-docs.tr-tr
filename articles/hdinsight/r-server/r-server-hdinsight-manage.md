---
title: HDInsight'ta ML Hizmetleri kümesini yönetme - Azure
description: Azure HDInsight'ta ML Hizmetleri kümesinde çeşitli görevleri nasıl yönetabileceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/19/2019
ms.openlocfilehash: b2c16c27c0dfc0c30a99c52544cc4d2278eadfc7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647739"
---
# <a name="manage-ml-services-cluster-on-azure-hdinsight"></a>Azure HDInsight'ta ML Hizmetleri kümesini yönetme

Bu makalede, birden çok eşzamanlı kullanıcı ekleme, ml hizmetleri kümesine uzaktan bağlanma, bilgi işlem bağlamı değiştirme gibi görevleri gerçekleştirmek için Azure HDInsight'ta varolan bir ML Hizmetleri kümesini nasıl yönetdiğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir ML Hizmetleri kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **ML Hizmetleri'ni** seçin. **Cluster type**

* Güvenli Kabuk (SSH) istemcisi: HDInsight kümesine uzaktan bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için bir SSH istemcisi kullanılır. Daha fazla bilgi için [HDInsight ile SSH'yi kullanın.](../hdinsight-hadoop-linux-use-ssh-unix.md)

## <a name="enable-multiple-concurrent-users"></a>Birden çok eş zamanlı kullanıcı etkinleştirme

RStudio topluluk sürümünün çalıştığı kenar düğümü için daha fazla kullanıcı ekleyerek HDInsight'ta ML Hizmetleri kümesi için birden çok eşzamanlı kullanıcı etkinleştirebilirsiniz. Bir HDInsight kümesi oluşturduğunuzda bir HTTP kullanıcısı ve bir SSH kullanıcısı olmak üzere iki kullanıcı sağlamanız gerekir:

![HDI Azure portal giriş parametreleri](./media/r-server-hdinsight-manage/hdi-concurrent-users1.png)

- **Küme oturum açma kullanıcı adı**: Oluşturduğunuz HDInsight kümelerini korumak için kullanılan HDInsight ağ geçidinden kimlik doğrulaması yapmak için kullanılan HTTP kullanıcısı. Bu HTTP kullanıcısı Apache Ambari UI, Apache Hadoop İPN UI ve diğer Kullanıcı Arabirimi bileşenlerine erişmek için kullanılır.
- **Secure Shell (SSH) kullanıcı adı:** kümeye güvenli kabuk üzerinden erişen bir SSH kullanıcısı. Bu kullanıcı Linux sisteminde tüm baş düğümler, çalışan düğümleri ve kenar düğümler için kullanılan kullanıcıdır. Bu sayede uzak kümedeki düğümlere erişmek için Secure Shell kullanabilirsiniz.

HDInsight'ta ML Services kümesinde kullanılan R Studio Server Community sürümü, yalnızca Linux kullanıcı adı ve parolasını bir oturum mekanizması olarak kabul eder. Belirteç iletmeyi desteklemez. Yani, bir ML Hizmetleri kümesinde ilk kez R Studio erişmeye çalıştığınızda, iki kez oturum açmanız gerekir.

- HDInsight Ağ Geçidi aracılığıyla HTTP kullanıcı kimlik bilgilerini kullanarak ilk oturum açın.

- Ardından RStudio'da oturum açabilmek için SSH kullanıcı kimlik bilgilerini kullanın.
  
Şu anda bir HDInsight kümesi sağlanırken yalnızca bir SSH kullanıcı hesabı oluşturulabilir. Bu nedenle, birden fazla kullanıcının HDInsight'ta ML Hizmetleri kümesine erişmesini sağlamak için Linux sisteminde ek kullanıcılar oluşturmanız gerekir.

RStudio kümenin kenar düğümünde çalıştığından, burada birkaç adım vardır:

1. Kenar düğümünde oturum açabilmek için varolan SSH kullanıcısını kullanma
2. Kenar düğümüne daha fazla Linux kullanıcısı ekleme
3. RStudio Topluluk sürümünü oluşturulan kullanıcıyla kullanma

### <a name="step-1-use-the-created-ssh-user-to-sign-in-to-the-edge-node"></a>Adım 1: Kenar düğümünde oturum açabilmek için oluşturulan SSH kullanıcısını kullanma

Kenar düğümüne erişmek [için SSH'yi kullanarak HDInsight'a (Apache Hadoop) Bağlan'daki](../hdinsight-hadoop-linux-use-ssh-unix.md) yönergeleri izleyin. HDInsight'taki ML Hizmetleri kümesinin kenar `CLUSTERNAME-ed-ssh.azurehdinsight.net`düğümü adresi.

### <a name="step-2-add-more-linux-users-in-edge-node"></a>2. Adım: Kenar düğümüne daha fazla Linux kullanıcısı ekleme

Kenar düğümüne bir kullanıcı eklemek için şu komutları çalıştırın:

    # Add a user 
    sudo useradd <yournewusername> -m

    # Set password for the new user
    sudo passwd <yournewusername>

Aşağıdaki ekran görüntüsü çıktıları gösterir.

![ekran görüntüsü çıkışı eşzamanlı kullanıcılar](./media/r-server-hdinsight-manage/hdi-concurrent-users2.png)

"Geçerli Kerberos şifresi:" için istendiğinde, yoksaymak için **Enter** tuşuna basmaniz yeterlidir. `useradd` komutundaki `-m` seçeneği, sistemin RStudio Topluluk sürümü için gerekli olan kullanıcı ana klasörünü oluşturacağını belirtir.

### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>3. Adım: RStudio Topluluk sürümünü oluşturulan kullanıcıyla kullanma

RStudio'ya `https://CLUSTERNAME.azurehdinsight.net/rstudio/`buradan erişin. Kümeyi oluşturduktan sonra ilk kez oturum açıyorsanız, oluşturduğunuz SSH kullanıcı kimlik bilgilerini izleyen küme yöneticisi kimlik bilgilerini girin. Bu ilk giriş değilseniz, yalnızca oluşturduğunuz SSH kullanıcısının kimlik bilgilerini girin.

Ayrıca, başka bir tarayıcı penceresinden aynı anda orijinal kimlik bilgilerini kullanarak oturum açabilirsiniz (varsayılan olarak, *sshuser'dir).*

Ayrıca yeni eklenen kullanıcıların Linux sisteminde kök ayrıcalıklarına sahip olmadığına ancak uzak HDFS ve WASB depolama alanındaki tüm dosyalara aynı düzeyde erişime sahip olduğuna dikkat edin.

## <a name="connect-remotely-to-microsoft-ml-services"></a>Microsoft ML Hizmetlerine uzaktan bağlanma

HDInsight Spark bilgi işlem bağlamına masaüstünüzde çalışan ML İstemci'nin uzak bir örneğinden erişim ayarlayabilirsiniz. Bunu yapmak için, masaüstünüzde RxSpark bilgi işlem bağlamını tanımlarken seçenekleri (hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches ve sshProfileScript) belirtmeniz gerekir: Örneğin:

    myNameNode <- "default"
    myPort <- 0

    mySshHostname  <- '<clustername>-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- '<sshuser>'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key

    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")

    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )

Daha fazla bilgi için, [Apache Spark bilgi işlem bağlamında RevoScaleR'in nasıl kullanılacağı](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-spark#more-spark-scenarios) bölümünde "Microsoft Machine Learning Server'ı Apache Hadoop İstemci Olarak Kullanma" bölümüne bakın

## <a name="use-a-compute-context"></a>İşlem bağlamı kullanma

Bir işlem bağlamı, hesaplamanın kenar düğümünde yerel olarak yapılmasını veya HDInsight kümesindeki düğümlere dağıtılmasını denetlemenize olanak tanır.  RStudio Server ile bir bilgi işlem bağlamı ayarlama örneği için, [RStudio Server'ı kullanarak Azure HDInsight'taki ML Hizmetleri kümesinde](machine-learning-services-quickstart-job-rstudio.md)Bir R komut dosyası yürüt'e bakın.

## <a name="distribute-r-code-to-multiple-nodes"></a>R kodunu birden fazla düğüme dağıtma

HDInsight'taki ML Hizmetleri ile varolan R kodunu alabilir ve kümedeki birden `rxExec`çok düğüm üzerinde kullanarak çalıştırabilirsiniz. Bu işlev bir parametre tarama veya benzetme işlemi sırasında yararlıdır. `rxExec` kullanımını gösteren bir kod örneği aşağıda verilmiştir:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Spark bağlamını kullanmaya devam ediyorsanız, bu komut, kodun `(Sys.info()["nodename"])` çalıştırıldığı alt düğümler için düğüm adı değerini döndürür. Örneğin, dört düğüm kümesinde, aşağıdaki parçacıkbenzer çıktı almayı bekliyorsunuz:

    $rxElem1
        nodename
    "wn3-mymlser"

    $rxElem2
        nodename
    "wn0-mymlser"

    $rxElem3
        nodename
    "wn3-mymlser"

    $rxElem4
        nodename
    "wn3-mymlser"

## <a name="access-data-in-apache-hive-and-parquet"></a>Apache Hive ve Parke'deki verilere erişin

HDInsight ML Services, Spark bilgi işlem bağlamında ScaleR işlevleri tarafından kullanılmak üzere Kovan ve Parke'deki verilere doğrudan erişim sağlar. Bu özellikler, ScaleR tarafından analiz edilmek üzere bir Spart DataFrame’e doğrudan veri yüklemek için Spark SQL kullanarak çalışan RxHiveData ve RxParquetData adlı yeni ScaleR veri kaynağı işlevleriyle kullanılabilir.

Yeni işlevlerin kullanımına ilişkin bazı örnek kodlar aşağıdaki kod ile verilmiştir:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
    rxHadoopMakeDir('/share')
    rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
    pqData <- RxParquetData('/share/claimsParquet',
                     colInfo = list(
                age    = list(type = "factor"),
               car.age = list(type = "factor"),
                  type = list(type = "factor")
             ) )
    rxGetInfo(pqData, getVarInfo = TRUE)

    rxNaiveBayes(type ~ age + cost, data = pqData)

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Bu yeni işlevlerin kullanımı hakkında daha fazla bilgi için ML `?RxHivedata` `?RxParquetData` Hizmetleri ve komutları kullanarak çevrimiçi yardım bakın.  

## <a name="install-additional-r-packages-on-the-cluster"></a>Kümeye ek R paketleri yükleme

### <a name="to-install-r-packages-on-the-edge-node"></a>Kenar düğümüne R paketleri yüklemek için

Kenar düğümüne ek R paketleri yüklemek istiyorsanız, SSH aracılığıyla kenar düğümüne bağlandıktan sonra doğrudan R konsolu içinden kullanabilirsiniz. `install.packages()` 

### <a name="to-install-r-packages-on-the-worker-node"></a>İşçi düğümüne R paketleri yüklemek için

Kümenin alt düğümlerine R paketleri yüklemek için Bir Komut Dosyası Eylemi kullanmanız gerekir. Betik Eylemleri, HDInsight kümesinde yapılandırma değişiklikleri yapmak veya ek R paketleri gibi ek yazılımlar yüklemek için kullanılan Bash betikleridir. 

> [!IMPORTANT]  
> Ek R paketleri yüklemek için Betik Eylemleri yalnızca küme oluşturulduktan sonra kullanılabilir. Komut dosyası tamamen yapılandırılan ML Hizmetleri'ne dayandığından, küme oluşturma sırasında bu yordamı kullanmayın.

1. Komut Dosyası [Eylemi'ni kullanarak kümeleri Özelleştir'deki](../hdinsight-hadoop-customize-cluster-linux.md)adımları izleyin.

3. **Komut dosyası eylemi gönder**için aşağıdaki bilgileri sağlayın:

   * **Komut Dosyası türü için** **Özel'i**seçin.

   * **Ad**için, komut dosyası eylemi için bir ad sağlayın.

     * **Bash script URI** `https://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`için , girin . Bu, alt düğüme ek R paketleri yükleyen komut dosyasıdır

   * Yalnızca **İşçi**için onay kutusunu seçin.

   * **Parametreler**: Yüklenecek R paketleri. Örneğin, `bitops stringr arules`

   * **Bu komut dosyası eylemini sürdürmek**için onay kutusunu seçin.  

   > [!NOTE]
   > 1. Varsayılan olarak, tüm R paketleri Microsoft MRAN deposunun yüklenen ML Server sürümüyle tutarlı bir anlık görüntüsünden yüklenir. Paketlerin daha yeni sürümlerini yüklemek istiyorsanız uyumsuzluk riskiyle karşı karşıya kalabilirsiniz. Ancak bu tür yüklemeleri paket listesinin ilk öğesi olarak `useCRAN` belirleyerek mümkün kılabilirsiniz, örneğin `useCRAN bitops, stringr, arules`.  
   > 2. Bazı R paketleri için ek Linux sistem kitaplıkları gerekir. Kolaylık sağlamak için, HDInsight ML Hizmetleri en popüler 100 R paketinin ihtiyaç duyduğu bağımlılıklarla önceden yüklenmiş olarak gelir. Ancak, yüklediğiniz R paketleri bunların dışında kitaplıklar gerektirirse, burada kullanılan temel betiği indirmeniz ve adımları ekleyerek sistem kitaplıklarını yüklemeniz gerekir. Ardından, değiştirilmiş betiği Azure depolama hizmetindeki ortak bir blob kapsayıcıya yüklemeniz ve değiştirilmiş betiği kullanarak paketleri yüklemeniz gerekir.
   >    Komut Dosyası Eylemleri geliştirme hakkında daha fazla bilgi için [Bkz. Komut Dosyası Eylem geliştirme.](../hdinsight-hadoop-script-actions-linux.md)

   ![Azure portalı komut dosyası eylemi gönderme](./media/r-server-hdinsight-manage/submit-script-action.png)

4. Betiği çalıştırmak için **Oluştur**’u seçin. Betik tamamlandıktan sonra R paketleri tüm çalışan düğümlerinde kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde ML Services kümesini kullanıma hazır hale getirme](r-server-operationalize.md)
* [HDInsight'ta ML Hizmet kümesi için bilgi işlem bağlam ı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde ML Services kümesi için Azure Depolama seçenekleri](r-server-storage.md)
