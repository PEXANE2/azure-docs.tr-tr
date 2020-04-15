---
title: Azure HDInsight'a erişmek için SSH tünellerini kullanma
description: Linux tabanlı HDInsight düğümlerinizde barındırılan web kaynaklarına güvenli bir şekilde göz atmak için bir SSH tünelini nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/14/2020
ms.openlocfilehash: 9bdf7360ce00637b0eed3de7a3349da8656a3ed0
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81314170"
---
# <a name="use-ssh-tunneling-to-access-apache-ambari-web-ui-jobhistory-namenode-apache-oozie-and-other-uis"></a>Apache Ambari web Kullanıcı Arabirimi, JobHistory, NameNode, Apache Oozie ve diğer Kullanıcı Aralarına erişmek için SSH tünellerini kullanın

HDInsight kümeleri, Apache Ambari web UI'ye Internet üzerinden erişim sağlar. Bazı özellikler için SSH tüneli gerekir. Örneğin, Apache Oozie web UI'ye SSH tüneli olmadan internet üzerinden erişilemez.

## <a name="why-use-an-ssh-tunnel"></a>Neden bir SSH tüneli kullanın?

Ambari'deki menülerin bir çoğu sadece bir SSH tünelinden çalışır. Bu menüler, işçi düğümleri gibi diğer düğüm türlerinde çalışan web sitelerine ve hizmetlere dayanır.

Aşağıdaki Web UI'leri bir SSH tüneli gerektirir:

* İş Geçmişi
* NameNode
* İş Parçacığı Yığınları
* Oozie web UI
* HBase Master ve Günlükleri UI

Bir web hizmetini ortaya çıkaran Komut Dosyası Eylemleri ile yüklenen hizmetler için Bir SSH tüneli gerekir. Komut Dosyası Eylemi ile yüklenen ton, web Web Arama Sürümü'ne erişmek için bir SSH tüneli gerektirir.

> [!IMPORTANT]  
> Sanal ağ üzerinden HDInsight'a doğrudan erişiminiz varsa, SSH tünelleri kullanmanız gerekmez. Sanal ağ üzerinden HDInsight'a doğrudan erişme örneği için, [şirket içi ağ belgenize HDInsight'ı bağlayın'](connect-on-premises-network.md) a bakın.

## <a name="what-is-an-ssh-tunnel"></a>SSH tüneli nedir

[Secure Shell (SSH) tüneli,](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) yerel makinenizdeki bir bağlantı noktasını HDInsight'taki bir baş düğümüne bağlar. Yerel bağlantı noktasına gönderilen trafik, kafa düğümüne bir SSH bağlantısı üzerinden yönlendirilir. İstek, baş düğümünde olduğu gibi çözülür. Yanıt daha sonra tünelden iş istasyonunuza yönlendirilir.

## <a name="prerequisites"></a>Ön koşullar

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* SOCKS5 proxy kullanmak için yapılandırılabilen bir web tarayıcısı.

    > [!WARNING]  
    > Windows Internet ayarlarında yerleşik OLAN SOCKS proxy desteği SOCKS5'i desteklemez ve bu belgedeki adımlarla çalışmaz. Aşağıdaki tarayıcılar Windows proxy ayarlarına dayanır ve şu anda bu belgedeki adımlarla çalışmaz:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome, Windows proxy ayarlarına da güvenir. Ancak, SOCKS5 destekleyen uzantıları yükleyebilirsiniz. [FoxyProxy Standard'ı](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp)öneriyoruz.

## <a name="create-a-tunnel-using-the-ssh-command"></a><a name="usessh"></a>SSH komutunu kullanarak tünel oluşturma

Komutu kullanarak bir SSH tüneli `ssh` oluşturmak için aşağıdaki komutu kullanın. HDInsight kümeniz için bir SSH kullanıcısıyla değiştirin `sshuser` ve HDInsight kümenizin adıyla değiştirin: `CLUSTERNAME`

```cmd
ssh -C2qTnNf -D 9876 sshuser@CLUSTERNAME-ssh.azurehdinsight.net
```

Bu komut, trafiği yerel bağlantı noktası 9876'ya, SSH üzerinden kümeye ileten bir bağlantı oluşturur. Seçenekler şunlardır:

    |Seçenek |Açıklama |
    |---|---|
    |D 9876|Tünelin içinden trafiği sağlayan yerel bağlantı noktası.|
    |C|Web trafiği çoğunlukla metin olduğundan, tüm verileri sıkıştırın.|
    |2|SSH'yi yalnızca protokol sürüm 2'yi denemeye zorlar.|
    |q|Sessiz mod.|
    |T|Bir bağlantı noktasını ilettiğiniz için sözde tty tahsisatını devre dışı bırak.|
    |n|Sadece bir bağlantı noktası ilettiğiniz için STDIN'in okunmasını engelleyin.|
    |N|Bir bağlantı noktasını sadece ilettiğiniz için uzaktan kumandayı yürütmeyin.|
    |f|Arka planda koş.|

Komut bittikten sonra, yerel bilgisayardaki 9876 numaralı bağlantı noktasına gönderilen trafik küme kafa düğümüne yönlendirilir.

## <a name="create-a-tunnel-using-putty"></a><a name="useputty"></a>PuTTY kullanarak tünel oluşturma

[PuTTY,](https://www.chiark.greenend.org.uk/~sgtatham/putty) Windows için grafiksel bir SSH istemcisidir. PuTTY'ye aşina değilseniz, [PuTTY belgelerine](https://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)bakın. PuTTY kullanarak bir SSH tüneli oluşturmak için aşağıdaki adımları kullanın:

### <a name="create-or-load-a-session"></a>Oturum oluşturma veya yükleme

1. PuTTY'yi açın ve oturumun sol menüde seçildiğinden **emin** olun. Bir oturumu zaten kaydettiyseniz, **Kaydedilen Oturumlar** listesinden oturum adını seçin ve **Yükle'yi**seçin.

1. Kaydedilmiş bir oturumunuz yoksa, bağlantı bilgilerinizi girin:

    |Özellik |Değer |
    |---|---|
    |Ana Bilgisayar Adı (veya IP adresi)|HDInsight kümesinin SSH adresi. Örneğin, **mycluster-ssh.azurehdinsight.net**.|
    |Bağlantı noktası|22|
    |Bağlantı Türü|SSH|

1. **Kaydet**’i seçin

    ![HDInsight macun oturumu oluşturmak](./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-create-putty-session.png)

1. İletişimin solundaki **Kategori** bölümünde **Bağlantıyı**genişletin, **SSH'yi**genişletin ve ardından **Tüneller'i**seçin.

1. **SSH bağlantı noktası iletme** formunu kontrol eden Seçenekler hakkında aşağıdaki bilgileri sağlayın:

    |Özellik |Değer |
    |---|---|
    |Kaynak bağlantı noktası|Iletmek istediğiniz istemcinin bağlantı noktası. Örneğin, **9876**.|
    |Hedef|HDInsight kümesinin SSH adresi. Örneğin, **mycluster-ssh.azurehdinsight.net**.|
    |Dinamik|Dinamik SOCKS proxy yönlendirmesini sağlar.|

    ![PuTTY Yapılandırma tünel leme seçenekleri](./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-putty-tunnel.png)

1. Ayarları eklemek için **Ekle'yi** ve ardından SSH bağlantısını açmak için **Aç'ı** seçin.

1. İstendiğinde, sunucuda oturum açın.

## <a name="use-the-tunnel-from-your-browser"></a>Tarayıcınızdan tüneli kullanma

> [!IMPORTANT]  
> Tüm platformlarda aynı proxy ayarlarını sağladığından, bu bölümdeki adımlar Mozilla FireFox tarayıcısını kullanır. Google Chrome gibi diğer modern tarayıcılar, tünelle çalışmak için FoxyProxy gibi bir uzantı gerektirebilir.

1. Bir **SOCKS v5** proxy olarak tünel oluştururken **localhost** ve kullandığınız bağlantı noktası kullanmak için tarayıcı yapılandırır. Firefox ayarları şu şekilde görünür. 9876'dan farklı bir bağlantı noktası kullandıysanız, bağlantı noktasını kullandığınız bağlantı noktasıyla değiştirin:

    ![firefox tarayıcı proxy ayarları](./media/hdinsight-linux-ambari-ssh-tunnel/firefox-proxy-settings.png)

   > [!NOTE]  
   > Uzaktan **DNS** seçilmesi, HDInsight kümesini kullanarak Etki Alanı Adı Sistemi (DNS) isteklerini giderir. Bu ayar, kümenin baş düğümlerini kullanarak DNS'yi çözer.

2. Tünelin aşağıdaki gibi [https://www.whatismyip.com/](https://www.whatismyip.com/)bir siteyi ziyaret ederek çalıştığını doğrulayın. Döndürülen IP, Microsoft Azure veri merkezi tarafından kullanılmalıdır.

## <a name="verify-with-ambari-web-ui"></a>Ambari web UI ile doğrula

Küme oluşturulduktan sonra, Ambari Web'den hizmet web uI'lerine erişebileceğinizi doğrulamak için aşağıdaki adımları kullanın:

1. Tarayıcınızda `http://headnodehost:8080` adresine gidin. Adres `headnodehost` tünel üzerinden kümeye gönderilir ve Ambari'nin üzerinde çalıştığının baş düğümüne çözülür. İstendiğinde, kümeniz in admin kullanıcı adını (yönetici) ve parolasını girin. Ambari web UI tarafından ikinci kez istenebilir. Eğer öyleyse, bilgileri yeniden girin.

   > [!NOTE]  
   > Kümeye `http://headnodehost:8080` bağlanmak için adresi kullanırken, tünelden bağlanAbilirsiniz. Https yerine SSH tüneli kullanılarak iletişim güvenlidir. HTTPS kullanarak internet üzerinden bağlanmak `https://clustername.azurehdinsight.net`için, kümenin adı nerede. `clustername`

2. From the Ambari Web UI, select HDFS from the list on the left of the page.

    ![Apache Ambari hdfs hizmeti seçildi](./media/hdinsight-linux-ambari-ssh-tunnel/hdfs-service-selected.png)

3. HDFS hizmet bilgileri **görüntülendiğinde, Hızlı Bağlantılar'ı**seçin. Küme kafa düğümlerinin listesi görüntülenir. Kafa düğümlerinden birini seçin ve ardından **NameNode UI'yi**seçin.

    ![QuickLinks menüsü ile görüntü genişletilmiş](./media/hdinsight-linux-ambari-ssh-tunnel/namenode-drop-down-menu.png)

    > [!NOTE]  
    > __Hızlı Bağlantılar'ı__seçtiğinizde, bir bekleme göstergesi alabilirsiniz. Yavaş bir internet bağlantınız varsa bu durum oluşabilir. Verilerin sunucudan alınması için bir veya iki dakika bekleyin ve listeyi yeniden deneyin.
    >
    > **Hızlı Bağlantılar** menüsündeki bazı girişler ekranın sağ tarafından kesilebilir. Eğer öyleyse, farenizi kullanarak menüyü genişletin ve sağ ok tuşunu kullanarak ekranın sağına kaydırarak menünün geri kalanını görün.

4. Aşağıdaki resme benzer bir sayfa görüntülenir:

    ![Hadoop NameNode UI resmi](./media/hdinsight-linux-ambari-ssh-tunnel/hdinsight-namenode-ui.png)

    > [!NOTE]  
    > Bu sayfanın URL'sine dikkat edin; benzer `http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster`olmalıdır. Bu URI düğümün dahili tam nitelikli etki alanı adını (FQDN) kullanıyor ve yalnızca bir SSH tüneli kullanırken erişilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bir SSH tüneli oluşturmayı ve kullanmayı öğrendiğiniz için Ambari'yi kullanmanın diğer yolları için aşağıdaki belgeye bakın:

* [Apache Ambari'yi kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md)
