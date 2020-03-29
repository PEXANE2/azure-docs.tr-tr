---
title: Sanal ağlarda HBase küme çoğaltma - Azure HDInsight
description: Yük dengeleme, yüksek kullanılabilirlik, sıfır kesinti geçişi ve güncelleştirmeleri ve olağanüstü durum kurtarma için bir HDInsight sürümünden diğerine HBase çoğaltmasını nasıl ayarlayayacağımı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/06/2019
ms.openlocfilehash: 1e6465584dd4e67f736b94d2939678c1a69163bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75435658"
---
# <a name="set-up-apache-hbase-cluster-replication-in-azure-virtual-networks"></a>Azure sanal ağlarda Apache HBase küme çoğaltmasını ayarlama

[Apache HBase](https://hbase.apache.org/) çoğaltmasını sanal ağ içinde veya Azure'daki iki sanal ağ arasında nasıl ayarlayabilirsiniz öğrenin.

Küme çoğaltma bir kaynak itme metodolojisi kullanır. Bir HBase kümesi bir kaynak veya hedef olabilir veya her iki rolü de aynı anda yerine getirebilir. Çoğaltma asynchronous olduğunu. Çoğaltma amacı nihai tutarlılık olduğunu. Çoğaltma etkinleştirildiğinde kaynak bir sütun ailesine bir edit aldığında, edit tüm hedef kümelere yayılır. Veriler bir kümeden diğerine çoğaltıldığında, çoğaltma döngülerini önlemek için kaynak küme ve verileri zaten tüketmiş olan tüm kümeler izlenir.

Bu makalede, bir kaynak hedef çoğaltma ayarlayın. Diğer küme topolojileri için [Apache HBase başvuru kılavuzuna](https://hbase.apache.org/book.html#_cluster_replication)bakın.

Tek bir sanal ağ için HBase çoğaltma kullanım örnekleri şunlardır:

* Yük dengeleme. Örneğin, hedef kümedeki taramaları çalıştırabilir veya MapReduce işlerini çalıştırabilir ve kaynak kümedeki verileri yutabilirsiniz.
* Yüksek kullanılabilirlik ekleme.
* Verileri bir HBase kümesinden diğerine geçirme.
* Azure HDInsight kümesini bir sürümden diğerine yükseltme.

İki sanal ağ için HBase çoğaltma kullanım örnekleri şunlardır:

* Olağanüstü durum kurtarma ayarlaması.
* Yük dengeleme ve uygulama bölümleme.
* Yüksek kullanılabilirlik ekleme.

[GitHub'dan](https://github.com/Azure/hbase-utils/tree/master/replication)komut [dosyası eylem](../hdinsight-hadoop-customize-cluster-linux.md) komut dosyalarını kullanarak kümeleri çoğaltabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu makaleye başlamadan önce bir Azure aboneliğiniz olması gerekir. Bkz. [Azure ücretsiz deneme sürümü alın.](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

## <a name="set-up-the-environments"></a>Ortamları ayarlama

Üç yapılandırma seçeneğiniz var:

- Bir Azure sanal ağında iki Apache HBase kümesi.
- Aynı bölgede iki farklı sanal ağda iki Apache HBase kümesi.
- İki farklı bölgede iki farklı sanal ağda iki Apache HBase kümesi (coğrafi çoğaltma).

Bu makalede, coğrafi çoğaltma senaryosu kapsar.

Ortamları ayarlamanıza yardımcı olmak için bazı [Azure Kaynak Yöneticisi şablonları](../../azure-resource-manager/management/overview.md)oluşturduk. Ortamları diğer yöntemleri kullanarak ayarlamayı tercih ederseniz, bkz:

- [HDInsight'ta Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
- [Azure Sanal Ağ'da Apache HBase kümeleri oluşturma](apache-hbase-provision-vnet.md)

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>İki farklı bölgede iki sanal ağ kurma

İki farklı bölgede iki sanal ağ ve VNet'ler arasındaki VPN bağlantısını oluşturan bir şablon kullanmak için aşağıdaki **Azure'a Dağıt** düğmesini seçin. Şablon tanımı ortak bir [blob depolama](https://hditutorialdata.blob.core.windows.net/hbaseha/azuredeploy.json)saklanır.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-replication/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

Şablondaki sabit kodlanmış değerlerden bazıları:

**VNet 1**

| Özellik | Değer |
|----------|-------|
| Konum | Batı ABD |
| VNet adı | &lt;ClusterNamePrevix>-vnet1 |
| Adres alanı öneki | 10.1.0.0/16 |
| Alt ağ adı | alt ağ 1 |
| Alt ağ öneki | 10.1.0.0/24 |
| Alt ağ geçidi adı | GatewaySubnet (değiştirilemez) |
| Alt ağ geçidi öneki | 10.1.255.0/27 |
| Ağ geçidi adı | vnet1gw |
| Ağ geçidi türü | VPN |
| Ağ geçidi VPN türü | RouteBased |
| Ağ Geçidi SKU | Temel |
| Ağ Geçidi IP | vnet1gwip |

**VNet 2**

| Özellik | Değer |
|----------|-------|
| Konum | Doğu ABD |
| VNet adı | &lt;ClusterNamePrevix>-vnet2 |
| Adres alanı öneki | 10.2.0.0/16 |
| Alt ağ adı | alt ağ 1 |
| Alt ağ öneki | 10.2.0.0/24 |
| Alt ağ geçidi adı | GatewaySubnet (değiştirilemez) |
| Alt ağ geçidi öneki | 10.2.255.0/27 |
| Ağ geçidi adı | vnet2gw |
| Ağ geçidi türü | VPN |
| Ağ geçidi VPN türü | RouteBased |
| Ağ Geçidi SKU | Temel |
| Ağ Geçidi IP | vnet1gwip |

## <a name="setup-dns"></a>Kurulum DNS

Son bölümde, şablon iki sanal adedin her birinde bir Ubuntu sanal makinesi oluşturur.  Bu bölümde, bind'i iki DNS sanal makineye yükler ve sonra iki sanal makinede DNS iletmesini yapılandırırsınız.

Bind'i yüklemek için yon'un iki DNS sanal makinesinin genel IP adresini bulması gerekir.

1. Azure [portalını](https://portal.azure.com)açın.
2. **[vnet1DNS] > [kaynak grubu adı] > Kaynak grupları**seçerek DNS sanal makineyi açın.  Kaynak grubu adı, son yordamda oluşturduğunuz addır. Varsayılan DNS sanal makine adları *vnet1DNS* ve *vnet2NDS*vardır.
3. Sanal ağın özellikleri sayfasını açmak için **Özellikler'i** seçin.
4. **Genel IP adresini**yazın ve Ayrıca Özel IP **adresini**doğrulayın.  Özel IP adresi vnet1DNS için **10.1.0.4** ve vnet2DNS için **10.2.0.4** olacaktır.  
5. Bind'i aşağıdaki adımlarla yüklemek için gelen ve giden erişimin indirme paketlerine erişmesine izin vermek için varsayılan (Azure'dan Sağlanan) DNS sunucularını kullanmak üzere her iki sanal ağ için de DNS Sunucularını değiştirin.

Bind'i yüklemek için aşağıdaki yordamı kullanın:

1. DNS sanal makinenin __genel IP adresine__ bağlanmak için SSH'yi kullanın. Aşağıdaki örnek 40.68.254.142 de sanal bir makineye bağlanır:

    ```bash
    ssh sshuser@40.68.254.142
    ```

    DNS sanal makineoluştururken belirttiğiniz SSH kullanıcı hesabı ile değiştirin. `sshuser`

    > [!NOTE]  
    > `ssh` Yardımcı programı elde etmenin çeşitli yolları vardır. Linux, Unix ve macOS'ta işletim sisteminin bir parçası olarak sağlanır. Windows kullanıyorsanız, aşağıdaki seçeneklerden birini göz önünde bulundurun:
    >
    > * [Azure Cloud Shell](../../cloud-shell/quickstart.md)
    > * [Windows 10 üzerinde Ubuntu üzerinde Bash](https://msdn.microsoft.com/commandline/wsl/about)
    > * [Git (https://git-scm.com/)](https://git-scm.com/)
    > * [OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)

2. Bind'i yüklemek için SSH oturumundaki aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Bind'i şirket içi DNS sunucunuza ad çözümleme isteklerini iletmek için yapılandırın. Bunu yapmak `/etc/bind/named.conf.options` için, dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```
    acl goodclients {
        10.1.0.0/16; # Replace with the IP address range of the virtual network 1
        10.2.0.0/16; # Replace with the IP address range of the virtual network 2
        localhost;
        localhost;
    };
    
    options {
        directory "/var/cache/bind";
        recursion yes;
        allow-query { goodclients; };

        forwarders {
            168.63.129.16; #This is the Azure DNS server
        };

        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
    };
    ```
    
    > [!IMPORTANT]  
    > Bölümdeki `goodclients` değerleri iki sanal anın IP adres aralığıyla değiştirin. Bu bölümde, bu DNS sunucusunun istekleri kabul ettiği adresler tanımlanır.

    Bu dosyayı yeniden kullanmak için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.options
    ```

    Dosyayı kaydetmek için __Ctrl+X__, __Y__ve ardından __Enter__'u kullanın.

4. SSH oturumundan aşağıdaki komutu kullanın:

    ```bash
    hostname -f
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

        vnet1DNS.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net

    Metin, `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` bu sanal ağ için __DNS sonekidir.__ Bu değeri kaydedin çünkü daha sonra kullanılacaktır.

    Ayrıca diğer DNS sunucusundan DNS soneki ni de bulmanız gerekir. Bir sonraki adımda ona ihtiyacın var.

5. Sanal ağdaki kaynakların DNS adlarını çözümlemek için Bind'i yapılandırmak `/etc/bind/named.conf.local` için, dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```
    // Replace the following with the DNS suffix for your virtual network
    zone "v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net" {
            type forward;
            forwarders {10.2.0.4;}; # The Azure recursive resolver
    };
    ```

    > [!IMPORTANT]  
    > Diğer sanal `v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` ağın DNS sonekiyle değiştirmeniz gerekir. Ve iletme IP diğer sanal ağda DNS sunucusunun özel IP adresidir.

    Bu dosyayı yeniden kullanmak için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Dosyayı kaydetmek için __Ctrl+X__, __Y__ve ardından __Enter__'u kullanın.

6. Bind'i başlatmak için aşağıdaki komutu kullanın:

    ```bash
    sudo service bind9 restart
    ```

7. Bind'in diğer sanal ağdaki kaynakların adlarını çözebileceğini doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt install dnsutils
    nslookup vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    ```

    > [!IMPORTANT]  
    > Diğer `vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net` ağdaki DNS sanal makinesinin tam nitelikli alan adı (FQDN) ile değiştirin.
    >
    > Diğer `10.2.0.4` sanal ağdaki özel DNS sunucunuzun __dahili IP adresiyle__ değiştirin.

    Yanıt aşağıdaki metne benzer görünür:

    ```
    Server:         10.2.0.4
    Address:        10.2.0.4#53
    
    Non-authoritative answer:
    Name:   vnet2dns.v5ant3az2hbe1edzthhvwwkcse.bx.internal.cloudapp.net
    Address: 10.2.0.4
    ```

    Şimdiye kadar, belirtilen DNS sunucu IP adresi olmadan diğer ağdan IP adresine bakamazsınız.

### <a name="configure-the-virtual-network-to-use-the-custom-dns-server"></a>Özel DNS sunucusunu kullanacak şekilde sanal ağı yapılandırma

Sanal ağı Azure özyinelemeli çözümleyici yerine özel DNS sunucusunu kullanacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. Azure [portalında,](https://portal.azure.com)sanal ağı seçin ve ardından __DNS Sunucularını__seçin.

2. __Özel'i__seçin ve özel DNS sunucusunun __iç IP adresini__ girin. Son olarak, __Kaydet'i__seçin.

6. Vnet1'de DNS sunucusu sanal makinesini açın ve **Yeniden Başlat'ı**tıklatın.  DNS yapılandırmasını etkili kılmak için sanal ağdaki tüm sanal makineleri yeniden başlatmanız gerekir.
7. Yinelenen adımlar vnet2 için özel DNS sunucusunu yapılandırın.

DNS yapılandırmasını sınamak için, SSH kullanarak iki DNS sanal makinesine bağlanabilir ve ana bilgisayar adını kullanarak diğer sanal ağın DNS sunucusuna ping atabilirsiniz. Çalışmıyorsa, DNS durumunu denetlemek için aşağıdaki komutu kullanın:

```bash
sudo service bind9 status
```

## <a name="create-apache-hbase-clusters"></a>Apache HBase kümeleri oluşturma

İki sanal anın her birinde aşağıdaki yapılandırmaya sahip bir [Apache HBase](https://hbase.apache.org/) kümesi oluşturun:

- **Kaynak grubu adı**: sanal ağları oluşturduğunuz da aynı kaynak grup adını kullanın.
- **Küme türü**: HBase
- **Sürüm**: HBase 1.1.2 (HDI 3.6)
- **Konum**: Sanal ağla aynı konumu kullanın.  Varsayılan olarak, vnet1 *Batı ABD*ve vnet2 *Doğu ABD*olduğunu.
- **Depolama**: Küme için yeni bir depolama hesabı oluşturun.
- **Sanal ağ** (portaldaki Gelişmiş ayarlardan): Son yordamda oluşturduğunuz vnet1'i seçin.
- **Alt ağ**: Şablonda kullanılan varsayılan ad **subnet1'dir.**

Ortamın doğru şekilde yapılandırıldığından emin olmak için, kafa nodonunun FQDN'sini iki küme arasında pingleyebilmelisin.

## <a name="load-test-data"></a>Test verilerini yükleme

Bir kümeyi çoğaltırken, çoğaltmak istediğiniz tabloları belirtmeniz gerekir. Bu bölümde, kaynak kümeye bazı verileri yüklersiniz. Sonraki bölümde, iki küme arasında çoğaltma yı etkinleştirin.

**Kişiler** tablosu oluşturmak ve tabloya bazı veriler eklemek için, [Apache HBase öğreticisindeki yönergeleri izleyin: HDInsight'ta Apache HBase'i kullanmaya başlayın.](apache-hbase-tutorial-get-started-linux.md)

> [!NOTE]
> Tabloları özel bir ad alanından çoğaltmak istiyorsanız, hedef kümede de uygun özel ad alanlarının tanımlandığından emin olmanız gerekir.
>

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Aşağıdaki adımlar, Azure portalından komut dosyası eylem komut dosyasının nasıl çağrılmasını açıklar. Azure PowerShell ve Azure Classic CLI'yi kullanarak komut dosyası eylemi çalıştırma hakkında daha fazla bilgi [için, komut dosyası eylemini kullanarak HDInsight kümelerini özelleştir'e](../hdinsight-hadoop-customize-cluster-linux.md)bakın.

**Azure portalından HBase çoğaltmasını etkinleştirmek için**

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Kaynak HBase kümesini açın.
3. Küme menüsünde **Komut Dosyası Eylemleri'ni**seçin.
4. Sayfanın üst kısmında **Yeni Gönder'i**seçin.
5. Aşağıdaki bilgileri seçin veya girin:

   1. **Adı**: **Çoğaltmayı Etkinleştir'i**girin .
   2. **Bash Script**URL **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**: Girin .
   3. **Baş**: Bunun seçildiğinden emin olun. Diğer düğüm türlerini temizleyin.
   4. **Parametreler**: Aşağıdaki örnek parametreler varolan tüm tablolar için çoğaltmayı etkinleştirin ve kaynak kümedeki tüm verileri hedef kümeye kopyalar:

          -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata
    
      > [!NOTE]
      > Hem kaynak hem de hedef küme DNS adı için FQDN yerine ana bilgisayar adı kullanın.
      >
      > Bu gözden geçirme, hn1'i etkin headnode olarak kabul eder. Etkin kafa düğümlerini tanımlamak için lütfen kümenizi kontrol edin.

6. **Oluştur'u**seçin. Komut dosyasının çalışması biraz zaman alabilir, özellikle **de -copydata** bağımsız değişkenini kullandığınızda.

Gerekli bağımsız değişkenler:

|Adı|Açıklama|
|----|-----------|
|-s, --src-küme | Kaynak HBase kümesinin DNS adını belirtir. Örneğin: -s hbsrccluster, --src-cluster=hbsrccluster |
|-d, --dst-küme | Hedef (yineleme) HBase kümesinin DNS adını belirtir. Örneğin: -s dsthbcluster, --src-cluster=dsthbcluster |
|-sp, --src-ambari-şifre | Ambari'nin yönetici parolasını kaynak HBase kümesinde belirtir. |
|-dp, --dst-ambari-şifre | Hedef HBase kümesinde Ambari'nin yönetici parolasını belirtir.|

İsteğe bağlı bağımsız değişkenler:

|Adı|Açıklama|
|----|-----------|
|-su, --src-ambari-kullanıcı | Kaynak HBase kümesinde Ambari'nin yönetici kullanıcı adını belirtir. Varsayılan değer **yöneticidir.** |
|-du, --dst-ambari-kullanıcı | Hedef HBase kümesinde Ambari'nin yönetici kullanıcı adını belirtir. Varsayılan değer **yöneticidir.** |
|-t, --tablo listesi | Çoğaltılacak tabloları belirtir. Örneğin: --table-list="table1;table2;table3". Tabloları belirtmezseniz, varolan tüm HBase tabloları çoğaltılır.|
|-m, --makine | Komut dosyası eyleminin çalıştığı baş düğümü belirtir. Değer, etkin kafa düğümüne göre seçilmelidir. 0 TL'lik komut dosyasını HDInsight portalından veya Azure PowerShell'den komut dosyası eylemi olarak çalıştırırken bu seçeneği kullanın.|
|-cp, -copydata | Çoğaltmanın etkin olduğu tablolardaki varolan verilerin geçişini sağlar. |
|-rpm, -çoğaltma-anka-meta | Phoenix sistem tablolarında çoğaltmayı sağlar. <br><br>*Bu seçeneği dikkatli kullanın.* Bu komut dosyalarını kullanmadan önce çoğaltma kümelerinde Phoenix tablolarını yeniden oluşturmanızı öneririz. |
|-h, --yardım | Kullanım bilgilerini görüntüler. |

Komut `print_usage()` [dosyasının](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) bölümünde parametrelerin ayrıntılı bir açıklaması vardır.

Komut dosyası eylemi başarıyla dağıtıldıktan sonra, hedef HBase kümesine bağlanmak için SSH'yi kullanabilir ve verilerin çoğaltıldığını doğrulayabilirsiniz.

### <a name="replication-scenarios"></a>Çoğaltma senaryoları

Aşağıdaki liste, bazı genel kullanım durumlarını ve parametre ayarlarını gösterir:

- **İki küme arasındaki tüm tablolarda çoğaltmayı etkinleştirin.** Bu senaryo, tablolardaki varolan verileri kopyalamayı veya geçişini gerektirmez ve Phoenix tablolarını kullanmaz. Aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Belirli tablolarda çoğaltmayı etkinleştirin.** Tablo1, tablo2 ve tablo3'te çoğaltmayı etkinleştirmek için aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Belirli tablolarda çoğaltmayı etkinleştirin ve varolan verileri kopyalayın.** Tablo1, tablo2 ve tablo3'te çoğaltmayı etkinleştirmek için aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Tüm tablolarda çoğaltmayı etkinleştirin ve Phoenix meta verilerini kaynaktan hedefe çoğaltın.** Phoenix meta veri çoğaltma mükemmel değildir. Dikkatli kullanın. Aşağıdaki parametreleri kullanın:

        -m hn1 -s <source hbase cluster name> -d <destination hbase cluster name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Verileri kopyalama ve geçirme

Çoğaltma etkinleştirildikten sonra verileri kopyalamak veya geçirmek için kullanılabilen iki ayrı komut dosyası eylem komut dosyası vardır:

- [Küçük tablolar için komut dosyası](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (birkaç gigabayt boyutunda olan tablolar ve genel kopyanın bir saatten kısa sürede tamamlanması bekleniyor)

- [Büyük tablolar için komut dosyası](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (kopyalanması bir saatten uzun sürmesi beklenen tablolar)

Komut dosyası eylemini çağırmak için [çoğaltmayı etkinleştir'de](#enable-replication) açıklanan yordamı izleyebilirsiniz. Aşağıdaki parametreleri kullanın:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

Komut `print_usage()` [dosyasının](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) bölümünde parametrelerin ayrıntılı bir açıklaması vardır.

### <a name="scenarios"></a>Senaryolar

- **Şimdiye kadar düzenlenen tüm satırlar için belirli tabloları (test1, test2 ve test3) kopyalayın (geçerli zaman damgası)**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Veya:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Belirli bir zaman aralığına sahip belirli tabloları kopyalayın:**

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Çoğaltmayı devre dışı bırakma

Çoğaltmayı devre dışı kullanabilirsiniz, [GitHub'dan](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh)başka bir komut dosyası eylem komut dosyası kullanın. Komut dosyası eylemini çağırmak için [çoğaltmayı etkinleştir'de](#enable-replication) açıklanan yordamı izleyebilirsiniz. Aşağıdaki parametreleri kullanın:

    -m hn1 -s <source hbase cluster name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

Komut `print_usage()` [dosyasının](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) bölümünde parametrelerin ayrıntılı bir açıklaması vardır.

### <a name="scenarios"></a>Senaryolar

- **Tüm tablolarda çoğaltma devre dışı:**

        -m hn1 -s <source hbase cluster name> -sp Mypassword\!789 -all
  or

        --src-cluster=<source hbase cluster name> --dst-cluster=<destination hbase cluster name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Belirtilen tablolarda çoğaltmayı devre dışı( tablo1, tablo2 ve tablo3)**:

        -m hn1 -s <source hbase cluster name> -sp <source cluster Ambari password> -t "table1;table2;table3"

> [!NOTE]
> Hedef kümeyi silmeyi planlıyorsanız, kaynak kümenin eş listesinden kaldırdığınızdan emin olun. Bu, kaynak kümedeki hbase kabuğunda '1' komutu remove_peer çalıştırılarak yapılabilir. Bu başarısız kaynak küme düzgün çalışmayabilir.
>

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, sanal ağ içinde veya iki sanal ağ arasında Apache HBase çoğaltma sını nasıl ayarlayabileceğinizi öğrendiniz. HDInsight ve Apache HBase hakkında daha fazla bilgi edinmek için şu makalelere bakın:

* [HDInsight'ta Apache HBase ile başlayın](./apache-hbase-tutorial-get-started-linux.md)
* [HDInsight Apache HBase'e genel bakış](./apache-hbase-overview.md)
* [Azure Sanal Ağ'da Apache HBase kümeleri oluşturma](./apache-hbase-provision-vnet.md)

