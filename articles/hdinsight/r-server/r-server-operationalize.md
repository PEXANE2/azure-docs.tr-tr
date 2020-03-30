---
title: HDInsight'ta ML Hizmetlerini Operasyonelleştirin - Azure
description: Azure HDInsight'ta ML Services ile öngörülerde bulunmak için veri modelinizi nasıl işlevsel hale getirebilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/27/2018
ms.openlocfilehash: a05bcdef2b7456fbab852e9728c156e57f847f57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71123559"
---
# <a name="operationalize-ml-services-cluster-on-azure-hdinsight"></a>Azure HDInsight'ta ML Hizmetleri kümesini operasyonel leştirin

Veri modellemenizi tamamlamak için HDInsight'ta ML Hizmetleri kümesini kullandıktan sonra, tahminde bulunmak için modeli işlevsel hale getirebilirsiniz. Bu makalede, bu görevin nasıl gerçekleştirilene ilişkin yönergeler verilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir ML Hizmetleri kümesi. Bkz. [Azure portalını kullanarak Apache Hadoop kümeleri oluşturun](../hdinsight-hadoop-create-linux-clusters-portal.md) ve Küme türü için **ML Hizmetleri'ni** seçin. **Cluster type**

* Güvenli Kabuk (SSH) istemcisi: HDInsight kümesine uzaktan bağlanmak ve komutları doğrudan küme üzerinde çalıştırmak için bir SSH istemcisi kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="operationalize-ml-services-cluster-with-one-box-configuration"></a>ML Hizmetleri kümesini tek kutu yapılandırma ile operasyonel hale

> [!NOTE]  
> Aşağıdaki adımlar R Server 9.0 ve ML Server 9.1 için geçerlidir. ML Server 9.3 için, [işletmeleştirme yapılandırmasını yönetmek için yönetim aracını kullanın'a](https://docs.microsoft.com/machine-learning-server/operationalize/configure-admin-cli-launch)bakın.

1. Kenar düğümüne SSH uygulayın.

        ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

    Azure HDInsight ile SSH'nin nasıl kullanılacağına ilişkin talimatlar için, [HDInsight ile SSH'yi kullan'a bakın.](../hdinsight-hadoop-linux-use-ssh-unix.md)

1. İlgili sürüm için dizin değiştirin ve nokta net dll sudo: 

    - Microsoft ML Server 9.1 için:

            cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0
            sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

    - Microsoft R Server 9.0 için:

            cd /usr/lib64/microsoft-deployr/9.0.1
            sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

1. Seçim seçenekleri ile sunulmaktadır. **Operasyonelleştirme için ML Server'ı Yapılandırmak için**aşağıdaki ekran görüntüsünde gösterildiği gibi ilk seçeneği seçin.

    ![R sunucu Yönetimi yardımcı programı seçin](./media/r-server-operationalize/admin-util-one-box-1.png)

1. Şimdi ML Server'ı nasıl işlevsel hale getirmek istediğinizi seçme seçeneği sunulur. Sunulan **seçeneklerden, A**girerek ilkini seçin.

    ![R sunucu Yönetimi programı operasyonel](./media/r-server-operationalize/admin-util-one-box-2.png)

1. İstendiğinde, yerel bir yönetici kullanıcısının parolasını girin ve yeniden girin.

1. İşlemin başarılı olduğunu gösteren çıktıları görmelisiniz. Menüden başka bir seçenek seçmeniz de istenir. Ana menüye geri dönmek için E'yi seçin.

    ![R sunucu Yönetimi yardımcı program başarısı](./media/r-server-operationalize/admin-util-one-box-3.png)

1. İsteğe bağlı olarak, tanılama testini aşağıdaki gibi çalıştırarak tanılama denetimleri gerçekleştirebilirsiniz:

    a. Ana menüden tanılama testlerini çalıştırmak için **6'yı** seçin.

    ![R sunucu Yönetimi yardımcı programı tanılama](./media/r-server-operationalize/hdinsight-diagnostic1.png)

    b. Tanılama Testleri menüsünden **A**' yı seçin. İstendiğinde, yerel yönetici kullanıcısı için sağladığınız parolayı girin.

    ![R sunucu Yönetimi yardımcı program testi](./media/r-server-operationalize/hdinsight-diagnostic2.png)

    c. Çıktının genel sağlık durumunun bir geçiş olduğunu gösterdiğini doğrulayın.

    ![R sunucu Yönetimi yardımcı program geçişi](./media/r-server-operationalize/hdinsight-diagnostic3.png)

    d. Sunulan menü seçeneklerinden, ana menüye dönmek için **E** girin ve yönetici yardımcı programı'ndan çıkmak için **8** girin.

### <a name="long-delays-when-consuming-web-service-on-apache-spark"></a>Apache Spark'ta web hizmetini tüketirken uzun gecikmeler

Apache Spark bilgi işlem bağlamında mrsdeploy işlevleriyle oluşturulan bir web hizmetini tüketmeye çalışırken uzun gecikmelerle karşılaşırsanız, bazı eksik klasörler eklemeniz gerekebilir. Spark uygulaması mrsdeoploy işlevleri kullanılarak bir web hizmetinden çağrıldığında '*rserve2*' adlı bir kullanıcıya ait oluyor. Bu soruna geçici bir çözüm olarak:

    # Create these required folders for user 'rserve2' in local and hdfs:

    hadoop fs -mkdir /user/RevoShare/rserve2
    hadoop fs -chmod 777 /user/RevoShare/rserve2

    mkdir /var/RevoShare/rserve2
    chmod 777 /var/RevoShare/rserve2


    # Next, create a new Spark compute context:
 
    rxSparkConnect(reset = TRUE)


Bu aşamada kullanıma hazır hale getirme yapılandırması tamamlanmıştır. Şimdi kenar düğümü `mrsdeploy` üzerinde operasyonelleştirme bağlanmak ve [uzaktan yürütme](https://docs.microsoft.com/machine-learning-server/r/how-to-execute-code-remotely) ve [web hizmetleri](https://docs.microsoft.com/machine-learning-server/operationalize/concept-what-are-web-services)gibi özelliklerini kullanmaya başlamak için RClient üzerinde paketi kullanabilirsiniz. Kümenizin bir sanal ağda ayarlanıp ayarlanmamasına bağlı olarak, SSH oturumu aracılığıyla bağlantı noktası iletme tüneli ayarlamanız gerekebilir. Aşağıdaki bölümlerde bu tüneli nasıl kuracağınız açıklanmaktadır.

### <a name="ml-services-cluster-on-virtual-network"></a>Sanal ağda ML Hizmetleri kümesi

12800 numaralı bağlantı noktası üzerinden kenar düğümüne trafik akışına izin verdiğinizden emin olun. Bu şekilde, Kullanıma Hazır Hale Getirme özelliğine bağlanmak için kenar düğümünü kullanabilirsiniz.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


`remoteLogin()` kenar düğümüne bağlanamadığı halde kenar düğümüne SSH uygulayabiliyorsanız, 12800 numaralı bağlantı noktası üzerinde trafiğe izin veren kuralın doğru şekilde ayarlanıp ayarlanmadığını doğrulamanız gerekir. Sorunla karşılaşmaya devam ederseniz, SSH üzerinden bağlantı noktası iletme tüneli oluşturarak bir geçici çözüm uygulayabilirsiniz. Talimatlar için aşağıdaki bölüme bakın:

### <a name="ml-services-cluster-not-set-up-on-virtual-network"></a>ML Hizmetleri kümesi sanal ağda ayarlanmaz

Kümeniz sanal üzerinde ayarlanmamışsa veya sanal ağ üzerinden bağlantı kurma sorunları yaşıyorsanız, SSH bağlantı noktası iletme tünelini kullanabilirsiniz:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

SSH oturumunuz etkin olduktan sonra, yerel makinenizin bağlantı noktası 12800'den gelen trafik, SSH oturumu aracılığıyla kenar düğümün 12800 bağlantı noktasına iletilir. `remoteLogin()` yönteminizde `127.0.0.1:12800` kullandığınızdan emin olun. Bu, kenar düğümünün bağlantı noktası yönlendirmesi aracılığıyla operasyonelizasyonuna giriş.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="scale-operationalized-compute-nodes-on-hdinsight-worker-nodes"></a>HDInsight işçi düğümlerinde operasyonel işlem düğümlerini ölçeklendirin

İşlem düğümlerini ölçeklendirmek için önce alt düğümleri devre dışı bırakıp, ardından devre dışı bırakılan alt düğümlerde işlem düğümlerini yapılandırın.

### <a name="step-1-decommission-the-worker-nodes"></a>Adım 1: İşçi düğümlerinin devre dışı bırakılması

ML Services kümesi [Apache Hadoop İPLik](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)üzerinden yönetilmez. Alt düğümler devre dışı kaldırılmazsa, IPN Kaynak Yöneticisi sunucu tarafından alınan kaynakların farkında olmadığından beklendiği gibi çalışmaz. Bu durumu önlemek için, işlem düğümlerini ölçeklendirmeden önce çalışan düğümlerinin yetkisinin alınması önerilir.

İşçi düğümlerini devre dışı bırakmak için aşağıdaki adımları izleyin:

1. Kümenin Ambari konsoluna giriş yapın ve **Hosts** sekmesine tıklayın.

1. İşçi düğümlerini seçin (kullanımdan kaldırılacak).

1. **Seçili Ana** > **Bilgisayar Ları** > **Hosts** > **Bakım Modunu Aç'ı**tıklatın. Örneğin, aşağıdaki görüntüde yetkisini almak üzere wn3 ve wn4 seçilmiştir.  

   ![Apache Ambari Açma Bakım Modu](./media/r-server-operationalize/get-started-operationalization.png)  

* **Seçili** >  **Ana** > Bilgisayarlar**DataNodes'i** seçin > **Komisyonu Kaldırma'yı**tıklatın.
* **Seçili** >  **Eylemler** > Hosts**NodeManagers'ı** seçin > **Komisyonu Kaldırma'yı**tıklatın.
* **Seçili** > **Eylemler'i** > seçin**Hosts DataNodes** > **Durdur'u**tıklatın.
* **Seçili Eylemler** > **Hosts** > **NodeManagers'ı** seçin > **Durdur'a**tıklayın.
* **Seçili** >  **Ana** > Bilgisayar**Ları'nı** seçin > **Tüm Bileşenleri Durdur'u**tıklatın.
* Çalışan düğümlerinin seçimini kaldırın ve baş düğümleri seçin.
* **Seçili** **Ana** > Bilgisayarlar > "**Hosts** > **Tüm Bileşenleri Yeniden Başlat'** ı seçin.

### <a name="step-2-configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Adım 2: Kullanımdan kaldırılan her işçi düğümünde işlem düğümlerini yapılandırma

1. Yetkisi alınan her çalışan düğümüne SSH uygulayın.

1. Sahip olduğunuz ML Hizmetleri kümesi için ilgili DLL'yi kullanarak yönetici yardımcı programını çalıştırın. ML Server 9.1 için aşağıdakileri çalıştırın:

        dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

1. **İşleme için ML Server'ı Yapılandır seçeneğini**seçmek için **1** girin.

1. Seçeneğini **C** `C. Compute node`seçmek için C girin. Bu işlem çalışan düğümündeki işlem düğümünü yapılandırır.

1. Yönetim Yardımcı Programından çıkın.

### <a name="step-3-add-compute-nodes-details-on-web-node"></a>Adım 3: Web düğümünde bilgi işlem düğümleri ayrıntıları ekleme

Tüm devre dışı bırakılan alt düğümler işlem düğümü çalıştırmak için yapılandırıldıktan sonra, kenar düğümüne geri gelin ve ML Server web düğümü yapılandırmasına devre dışı bırakılan alt düğümlerin IP adreslerini ekleyin:

1. Kenar düğümüne SSH uygulayın.

1. `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json` öğesini çalıştırın.

1. "Uris" bölümünü arayın ve alt düğümün IP ve bağlantı noktası ayrıntılarını ekleyin.

       "Uris": {
         "Description": "Update 'Values' section to point to your backend machines. Using HTTPS is highly recommended",
         "Values": [
           "http://localhost:12805", "http://[worker-node1-ip]:12805", "http://[workder-node2-ip]:12805"
         ]
       }

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde ML Services kümesini yönetme](r-server-hdinsight-manage.md)
* [HDInsight üzerinde ML Services kümesi için işlem bağlamı seçenekleri](r-server-compute-contexts.md)
* [HDInsight üzerinde ML Services kümesi için Azure Depolama seçenekleri](r-server-storage.md)
