---
title: Linux tabanlı HDInsight'ta Hadoop kullanma nın ipuçları - Azure
description: Azure bulutu içinde çalışan tanıdık bir Linux ortamında Linux tabanlı HDInsight (Hadoop) kümelerini kullanmak için uygulama ipuçları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2019
ms.openlocfilehash: 3d9dec0065bb62821fcedcbc4f6e5b578c061caf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272467"
---
# <a name="information-about-using-hdinsight-on-linux"></a>Linux’ta HDInsight kullanma ile ilgili bilgiler

Azure HDInsight kümeleri, Apache Hadoop'u Azure bulutu içinde çalışan tanıdık bir Linux ortamında sağlar. Çoğu şey için, tam olarak başka bir Hadoop-on-Linux yükleme gibi çalışması gerekir. Bu belge, farkında olmanız gereken belirli farklılıkları çağırır.

## <a name="prerequisites"></a>Ön koşullar

Bu belgedeki adımların çoğu, sisteminize yüklenmesi gereken aşağıdaki yardımcı programları kullanır.

* [cURL](https://curl.haxx.se/) - web tabanlı hizmetlerle iletişim kurmak için kullanılır.
* **jq**, bir komut satırı JSON işlemci.  Bkz. [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) - Azure hizmetlerini uzaktan yönetmek için kullanılır.
* **Bir SSH istemcisi**. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="users"></a>Kullanıcılar

[Etki alanı birleştirilmiş](./domain-joined/hdinsight-security-overview.md)sürece, HDInsight **tek kullanıcılı** bir sistem olarak kabul edilmelidir. Kümeyle birlikte yönetici düzeyinde izinlerle tek bir SSH kullanıcı hesabı oluşturulur. Ek SSH hesapları oluşturulabilir, ancak kümeye yönetici erişimi de vardır.

Etki alanına katılan HDInsight, birden çok kullanıcıyı ve daha ayrıntılı izin ve rol ayarlarını destekler. Daha fazla bilgi için [bkz: Etki Alanı birleştirilmiş HDInsight kümelerini yönet.](./domain-joined/apache-domain-joined-manage.md)

## <a name="domain-names"></a>Etki alanı adları

Kümeye internetten bağlanırken kullanılacak tam nitelikli alan adı (FQDN) veya `CLUSTERNAME.azurehdinsight.net` `CLUSTERNAME-ssh.azurehdinsight.net` (yalnızca SSH için).

Dahili olarak, kümedeki her düğüm, küme yapılandırması sırasında atanan bir ada sahiptir. Küme adlarını bulmak için Ambari Web Kullanıcı Arama Birimi'ndeki **Hosts** sayfasına bakın. Ambari REST API'den ana bilgisayarların listesini döndürmek için aşağıdakileri de kullanabilirsiniz:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

`CLUSTERNAME` değerini kümenizin adıyla değiştirin. İstendiğinde, yönetici hesabının parolasını girin. Bu komut, kümedeki ana bilgisayarların listesini içeren bir JSON belgesini döndürür. [jq](https://stedolan.github.io/jq/) her ana `host_name` bilgisayar için öğe değerini ayıklamak için kullanılır.

Belirli bir hizmet için düğümün adını bulmanız gerekiyorsa, bu bileşen için Ambari sorgulayabilirsiniz. Örneğin, HDFS ad düğümü için ana bilgisayarları bulmak için aşağıdaki komutu kullanın:

    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Bu komut hizmeti açıklayan bir JSON belgesi döndürür ve `host_name` [jq](https://stedolan.github.io/jq/) yalnızca ana bilgisayarları için değeri çıkarır.

## <a name="remote-access-to-services"></a>Hizmetlere uzaktan erişim

* **Ambari (web)** - `https://CLUSTERNAME.azurehdinsight.net`

    Küme yöneticisi kullanıcı ve parolayı kullanarak kimlik doğrulaması ve ardından Ambari'de oturum açın.

    Kimlik doğrulama düz metindir - bağlantının güvenli olduğundan emin olmak için her zaman HTTPS'yi kullanın.

    > [!IMPORTANT]  
    > Ambari erişim düğümleri aracılığıyla kullanılabilen web Kullanıcı Ara'larının bazıları dahili bir etki alanı adı kullanır. Dahili alan adları internet üzerinden herkese açık olarak erişilemez. Internet üzerinden bazı özelliklere erişmeye çalışırken "sunucu bulunamadı" hataları alabilirsiniz.
    >
    > Ambari web UI'nin tam işlevselliğini kullanmak için, küme kafa düğümüne web trafiğini proxy etmek için bir SSH tüneli kullanın. Bkz. [Apache Ambari web Kullanıcı Arabirimi, ResourceManager, JobHistory, NameNode, Oozie ve diğer web UI'lerine erişmek için SSH Tünel'i kullanın](hdinsight-linux-ambari-ssh-tunnel.md)

* **Ambari (REST)** - `https://CLUSTERNAME.azurehdinsight.net/ambari`

    > [!NOTE]  
    > Küme yöneticisi kullanıcı ve parolasını kullanarak kimlik doğrulaması.
    >
    > Kimlik doğrulama düz metindir - bağlantının güvenli olduğundan emin olmak için her zaman HTTPS'yi kullanın.

* **WebHCat (Templeton)** - `https://CLUSTERNAME.azurehdinsight.net/templeton`

    > [!NOTE]  
    > Küme yöneticisi kullanıcı ve parolasını kullanarak kimlik doğrulaması.
    >
    > Kimlik doğrulama düz metindir - bağlantının güvenli olduğundan emin olmak için her zaman HTTPS'yi kullanın.

* **SSH** CLUSTERNAME-ssh.azurehdinsight.net - 22 veya 23. Port 22 birincil başlık bağlanmak için kullanılırken, 23 ikincil bağlanmak için kullanılır. Kafa düğümleri hakkında daha fazla bilgi için [HDInsight'taki Apache Hadoop kümelerinin kullanılabilirliği ve güvenilirliği](hdinsight-high-availability-linux.md)bölümüne bakın.

    > [!NOTE]  
    > Küme kafa düğümlerine yalnızca SSH üzerinden istemci makinesinden erişebilirsiniz. Bağlandıktan sonra, bir başlıktan SSH kullanarak alt düğümlere erişebilirsiniz.

Daha fazla bilgi için [HDInsight belgesinde Apache Hadoop hizmetleri tarafından kullanılan Bağlantı Noktaları'na](hdinsight-hadoop-port-settings-for-services.md) bakın.

## <a name="file-locations"></a>Dosya konumları

Hadoop ile ilgili dosyalar küme düğümleri üzerinde `/usr/hdp`bulunabilir. Bu dizin aşağıdaki alt dizinleri içerir:

* **2.6.5.3009-43**: Dizin adı HDInsight tarafından kullanılan Hadoop platformunun sürümüdür. Kümenizdeki sayı burada listelenenden farklı olabilir.
* **akım**: Bu dizin **2.6.5.3009-43** dizini altında alt dizinlere bağlantılar içerir. Bu dizin, sürüm numarasını hatırlamak zorunda kalmamak için vardır.

Örnek veriler ve JAR dosyaları Hadoop Distributed `/example` Dosya `/HdiSamples`Sistemi'nde bulunabilir.

## <a name="hdfs-azure-storage-and-data-lake-storage"></a>HDFS, Azure Depolama ve Veri Gölü Depolama

Çoğu Hadoop dağıtımında, veriler kümedeki makinelerde yerel depolama tarafından desteklenen HDFS'de depolanır. Yerel depolama yı kullanmak, hesaplama kaynakları için saatlik veya dakika dakika ücretlendirildiğiniz bulut tabanlı bir çözüm için maliyetli olabilir.

HDInsight kullanırken, veri dosyaları Azure Blob Depolama ve isteğe bağlı Olarak Azure Veri Gölü Depolama sı kullanılarak bulutta ölçeklenebilir ve esnek bir şekilde depolanır. Bu hizmetler aşağıdaki avantajları sağlar:

* Ucuz uzun vadeli depolama.
* Web siteleri, dosya yükleme/indirme yardımcı programları, çeşitli dil SDK'ları ve web tarayıcıları gibi harici hizmetlerden erişilebilirlik.
* Büyük dosya kapasitesi ve büyük ölçeklenebilir depolama.

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) [Data Lake Storage](https://azure.microsoft.com/services/storage/data-lake-storage/)

Azure Depolama veya Veri Gölü Depolama'yı kullanırken, verilere erişmek için HDInsight'tan özel bir şey yapmanız gerekmez. Örneğin, aşağıdaki komut, Azure `/example/data` Depolama veya Veri Gölü Depolama'da depolanıp depolanmasına bakılmaksızın klasördeki dosyaları listeler:

    hdfs dfs -ls /example/data

HDInsight'ta veri depolama kaynakları (Azure Blob Depolama ve Azure Veri Gölü Depolama) bilgi işlem kaynaklarından ayrıştırılır. Bu nedenle, istediğiniz gibi hesaplama yapmak için HDInsight kümeleri oluşturabilir ve çalışma bittiğinde kümeyi silebilir, bu arada veri dosyalarınızı bulut depolamaalanında istediğiniz sürece güvenli bir şekilde bekletebilirsiniz.

### <a name="uri-and-scheme"></a><a name="URI-and-scheme"></a>URI ve şema

Bazı komutlar, bir dosyaya erişirken düzeni URI'nin bir parçası olarak belirtmenizi gerektirebilir. Örneğin, Storm-HDFS bileşeni düzeni belirtmenizi gerektirir. Varsayılan olmayan depolama alanı (kümeye "ek" depolama alanı olarak eklenen depolama) kullanırken, düzeni her zaman URI'nin bir parçası olarak kullanmanız gerekir.

Azure [**Depolama'yı**](./hdinsight-hadoop-use-blob-storage.md)kullanırken aşağıdaki URI düzenlerinden birini kullanın:

* `wasb:///`: Şifrelenmemiş iletişimi kullanarak varsayılan depolamaya erişin.

* `wasbs:///`: Şifreli iletişimi kullanarak varsayılan depolamaya erişin.  Wasbs şeması yalnızca HDInsight sürüm 3.6'dan itibaren desteklenir.

* `wasb://<container-name>@<account-name>.blob.core.windows.net/`: Varsayılan olmayan bir depolama hesabıyla iletişim kurarken kullanılır. Örneğin, ek bir depolama hesabınız olduğunda veya herkese açık bir depolama hesabında depolanan verilere erişirken.

Azure [**Veri Gölü Depolama Gen2'yi**](./hdinsight-hadoop-use-data-lake-storage-gen2.md)kullanırken aşağıdaki URI düzenini kullanın:

* `abfs://`: Şifreli iletişimi kullanarak varsayılan depolamaya erişin.

* `abfs://<container-name>@<account-name>.dfs.core.windows.net/`: Varsayılan olmayan bir depolama hesabıyla iletişim kurarken kullanılır. Örneğin, ek bir depolama hesabınız olduğunda veya herkese açık bir depolama hesabında depolanan verilere erişirken.

Azure [**Veri Gölü Depolama Gen1'i**](./hdinsight-hadoop-use-data-lake-store.md)kullanırken aşağıdaki URI düzenlerinden birini kullanın:

* `adl:///`: Küme için varsayılan Veri Gölü Depolama'ya erişin.

* `adl://<storage-name>.azuredatalakestore.net/`: Varsayılan olmayan bir Veri Gölü Depolama ile iletişim kurarken kullanılır. HDInsight kümenizin kök dizininizin dışındaki verilere erişmek için de kullanılır.

> [!IMPORTANT]  
> HDInsight için varsayılan depo olarak Veri Gölü Depolama kullanırken, HDInsight depolama kökü olarak kullanmak için mağaza içinde bir yol belirtmeniz gerekir. Varsayılan `/clusters/<cluster-name>/`yol.
>
> Verileri `/` kullanırken veya `adl:///` verilere erişirken, yalnızca kümenin kökünde `/clusters/<cluster-name>/`(örneğin) depolanan verilere erişebilirsiniz. Depodaki herhangi bir yerde verilere `adl://<storage-name>.azuredatalakestore.net/` erişmek için biçimi kullanın.

### <a name="what-storage-is-the-cluster-using"></a>Küme nin kullandığı depolama alanı nedir?

Küme için varsayılan depolama yapılandırmasını almak için Ambari'yi kullanabilirsiniz. Curl kullanarak HDFS yapılandırma bilgilerini almak ve [jq](https://stedolan.github.io/jq/)kullanarak filtrelemek için aşağıdaki komutu kullanın:

```bash
curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

> [!NOTE]  
> Bu komut, bu bilgileri içeren`service_config_version=1`sunucuya uygulanan ilk yapılandırmayı ( ) döndürür. En son yapılandırma sürümlerini bulmak için tüm yapılandırma sürümlerini listele etmeniz gerekebilir.

Bu komut, aşağıdaki ÜRB'lere benzer bir değer döndürür:

* `wasb://<container-name>@<account-name>.blob.core.windows.net`bir Azure Depolama hesabı kullanıyorsanız.

    Hesap adı Azure Depolama hesabının adıdır. Kapsayıcı adı küme depolama kökü olan blob kapsayıcıdır.

* `adl://home`Azure Veri Gölü Depolama'yı kullanıyorsanız. Veri Gölü Depolama adını almak için aşağıdaki REST çağrısını kullanın:

     ```bash
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    Bu komut aşağıdaki ana `<data-lake-store-account-name>.azuredatalakestore.net`bilgisayar adını döndürür: .

    HDInsight'ın kökü olan mağazadaki dizini almak için aşağıdaki REST aramasını kullanın:

    ```bash
    curl -u admin -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    Bu komut aşağıdaki yola benzer bir `/clusters/<hdinsight-cluster-name>/`yol döndürür: .

Azure portalını kullanarak depolama bilgilerini aşağıdaki adımları kullanarak da bulabilirsiniz:

1. Azure [portalından](https://portal.azure.com/)HDInsight kümenizi seçin.

2. **Özellikler** bölümünden **Depolama Hesapları'nı**seçin. Kümenin depolama bilgileri görüntülenir.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>HDInsight dışından dosyalara nasıl erişebilirim?

HDInsight kümesinin dışından verilere erişmenin çeşitli yolları vardır. Verilerinizle çalışmak için kullanılabilecek yardımcı programlara ve SDK'lara birkaç bağlantı ve bağlantı aşağıda veda edebilirsiniz:

Azure __Depolama'yı__kullanıyorsanız, verilerinize erişebileceğiniz yollar için aşağıdaki bağlantılara bakın:

* [Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2): Azure ile çalışmak için Komut Satırı arabirimi komutları. Yükledikten sonra, `az storage` depolama yı kullanma konusunda `az storage blob` yardım için veya blob'a özgü komutlar için komutu kullanın.
* [blobxfer.py](https://github.com/Azure/blobxfer): Azure Depolama'da lekelerle çalışmak için bir python komut dosyası.
* Çeşitli SDK'lar:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Depolama REST API’si](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Azure __Veri Gölü Depolama'yı__kullanıyorsanız, verilerinize erişebileceğiniz yollar için aşağıdaki bağlantılara bakın:

* [Web tarayıcısı](../data-lake-store/data-lake-store-get-started-portal.md)
* [Powershell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS REST API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Visual Studio için Veri Gölü Araçları](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling-your-cluster"></a><a name="scaling"></a>Kümenizi ölçekleme

Küme ölçekleme özelliği, bir küme tarafından kullanılan veri düğümleri sayısını dinamik olarak değiştirmenize olanak tanır. Küme üzerinde çalışan diğer işler veya işlemler çalışırken ölçekleme işlemleri gerçekleştirebilirsiniz.  Ayrıca bakınız, [HDInsight kümelerini ölçeklendirin](./hdinsight-scaling-best-practices.md)

Farklı küme türleri aşağıdaki gibi ölçekleme etkilenir:

* **Hadoop**: Kümedeki düğüm sayısını küçültterken, kümedeki bazı hizmetler yeniden başlatılır. Ölçekleme işlemleri, ölçekleme işlemi tamamlandığında çalışan veya başarısız olmayı bekleyen işlere neden olabilir. İşlem tamamlandıktan sonra işleri yeniden gönderebilirsiniz.
* **HBase**: Bölgesel sunucular, ölçekleme işlemi tamamlandıktan sonra birkaç dakika içinde otomatik olarak dengelenir. Bölgesel sunucuları el ile dengelemek için aşağıdaki adımları kullanın:

    1. SSH'yi kullanarak HDInsight kümesine bağlanın. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

    2. HBase kabuğunu başlatmak için aşağıdakileri kullanın:

            hbase shell

    3. HBase mermisi yüklendikten sonra, bölgesel sunucuları el ile dengelemek için aşağıdakileri kullanın:

            balancer

* **Fırtına**: Bir ölçekleme işlemi yapıldıktan sonra çalışan fırtına topolojilerini yeniden dengelemeniz gerekir. Yeniden dengeleme, topolojinin kümedeki yeni düğüm sayısına göre paralellik ayarlarını yeniden ayarlamasına olanak tanır. Çalışan topolojileri yeniden dengelemek için aşağıdaki seçeneklerden birini kullanın:

    * **SSH**: Sunucuya bağlanın ve bir topolojiyi yeniden dengelemek için aşağıdaki komutu kullanın:

            storm rebalance TOPOLOGYNAME

        Topoloji tarafından sağlanan paralellik ipuçlarını geçersiz kılmak için parametreleri de belirtebilirsiniz. Örneğin, `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` topolojiyi 5 alt işlem, mavi-ekap bileşeni için 3 uygulayıcı ve sarı cıvata bileşeni için 10 uygulayıcıyla yeniden yapılandırır.

    * **Storm UI**: Fırtına UI'sini kullanarak bir topolojiyi yeniden dengelemek için aşağıdaki adımları kullanın.

        1. Web `https://CLUSTERNAME.azurehdinsight.net/stormui` tarayıcınızda açın, `CLUSTERNAME` Fırtına kümenizin adı buradadır. İstenirse, küme oluştururken belirttiğiniz HDInsight küme yöneticisi (yönetici) adını ve parolasını girin.
        2. Yeniden dengelemek istediğiniz topolojiyi seçin ve ardından **Yeniden Denge** düğmesini seçin. Yeniden dengeleme işlemi gerçekleştirilmeden önce gecikmeyi girin.

* **Kafka**: Ölçekleme işlemlerinden sonra bölüm yinelemelerini yeniden dengelemeniz gerekir. Daha fazla bilgi için [HDInsight belgesinde Apache Kafka ile birlikte yüksek veri kullanılabilirliğine](./kafka/apache-kafka-high-availability.md) bakın.

HDInsight kümenizi ölçekleme hakkında özel bilgiler için bkz:

* [Azure portalını kullanarak HDInsight'ta Apache Hadoop kümelerini yönetme](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Azure CLI'yi kullanarak Apache Hadoop kümelerini HDInsight'ta yönetme](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Hue 'yi (veya diğer Hadoop bileşenini) nasıl yüklerim?

HDInsight yönetilen bir hizmettir. Azure kümeyle ilgili bir sorun algılarsa, başarısız düğümü silebilir ve değiştirmek için bir düğüm oluşturabilir. Kümeye el ile yüklerseniz, bu işlem gerçekleştiğinde bunlar kalıcı değildir. Bunun yerine, [HDInsight Script Eylemleri](hdinsight-hadoop-customize-cluster-linux.md)kullanın. Komut dosyası eylemi aşağıdaki değişiklikleri yapmak için kullanılabilir:

* Bir hizmeti veya web sitesini yükleyin ve yapılandırır.
* Kümedeki birden çok düğümde yapılandırma değişiklikleri gerektiren bir bileşen yükleyin ve yapılandırın.

Komut Dosyası Eylemleri Bash komut dosyalarıdır. Komut dosyaları küme oluşturma sırasında çalışır ve ek bileşenleri yüklemek ve yapılandırmak için kullanılır. Kendi Betik Eylemlerinizi geliştirme hakkında daha fazla bilgi için bkz. [HDInsight ile Betik Eylemi geliştirme](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>Kavanoz dosyaları

Bazı Hadoop teknolojileri, MapReduce işinin bir parçası olarak kullanılan işlevleri içeren bağımsız kavanoz dosyalarında veya Pig veya Hive'ın içinden sağlanır. Genellikle herhangi bir kurulum gerektirmez ve oluşturulduktan sonra kümeye yüklenebilir ve doğrudan kullanılabilir. Bileşenin kümenin yeniden görüntülenmesinden sağ kurtulduğundan emin olmak istiyorsanız, kavanoz dosyasını kümeniz (WASB veya ADL) için varsayılan depolama alanında depolayabilirsiniz.

Örneğin, [Apache DataFu'nun](https://datafu.incubator.apache.org/)en son sürümünü kullanmak istiyorsanız, projeyi içeren bir kavanoz indirebilir ve HDInsight kümesine yükleyebilirsiniz. Daha sonra Pig or Hive'dan nasıl kullanılacağına ilişkin DataFu belgelerini takip edin.

> [!IMPORTANT]  
> Bağımsız kavanoz dosyaları olan bazı bileşenler HDInsight ile sağlanır, ancak yolda değildir. Belirli bir bileşen arıyorsanız, kümenizde aramak için aşağıdakileri kullanabilirsiniz:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Bu komut, eşleşen kavanoz dosyalarının yolunu döndürür.

Bir bileşenin farklı bir sürümünü kullanmak için, ihtiyacınız olan sürümü yükleyin ve işlerinizde kullanın.

> [!IMPORTANT]
> HDInsight kümesiyle sağlanan bileşenler tam olarak desteklenir ve Microsoft Desteği bu bileşenlerle ilgili sorunları yalıtmaya ve çözmeye yardımcı olur.
>
> Özel bileşenler, sorunu daha fazla gidermenize yardımcı olmak için ticari olarak makul destek alır. Bu, sorunun çözülmesiyle veya bu teknoloji için derin uzmanlık bulunan açık kaynak teknolojileri için kullanılabilir kanallardan bağlantı kurmanızı istemesiyle sonuçlanabilir. Örneğin, kullanılabilir birçok topluluk sitesi vardır, örneğin: [HDInsight için MSDN forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [https://stackoverflow.com](https://stackoverflow.com). Ayrıca Apache projeleri, [https://apache.org](https://apache.org)örneğin proje siteleri var: [Hadoop](https://hadoop.apache.org/), [Kıvılcım](https://spark.apache.org/).

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetme](./hdinsight-hadoop-manage-ambari-rest-api.md)
* [HDInsight ile Apache Hive'ı kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile MapReduce işleri kullanma](hadoop/hdinsight-use-mapreduce.md)
