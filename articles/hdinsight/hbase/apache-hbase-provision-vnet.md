---
title: Sanal Ağ'da HBase kümeleri oluşturma - Azure
description: Azure HDInsight'ta HBase'i kullanmaya başlayın. Azure Sanal Ağ'da HDInsight HBase kümelerini nasıl oluşturabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/23/2019
ms.openlocfilehash: e4e15d1c6554fc567f668b2033bff5b5664db918
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972791"
---
# <a name="create-apache-hbase-clusters-on-hdinsight-in-azure-virtual-network"></a>Azure Sanal Ağında HDInsight'ta Apache HBase kümeleri oluşturma

Azure [Sanal Ağı'nda](https://azure.microsoft.com/services/virtual-network/)Azure HDInsight Apache HBase kümelerini nasıl oluşturacağınızı öğrenin.

Sanal ağ tümleştirmesi ile Apache HBase kümeleri, uygulamaların HBase ile doğrudan iletişim kurabilmesi için uygulamalarınızla aynı sanal ağa dağıtılabilir. Avantajlara şunlar dahildir:

* Web uygulamasının HBase kümesinin düğümlerine doğrudan bağlantısı, HBase Java uzaktan yordam çağrısı (RPC) API'leri ile iletişimi sağlar.
* Trafiğinizin birden çok ağ geçidi ve yük dengeleyicileri üzerinden geçmesine sahip değil, geliştirilmiş performans.
* Ortak bir bitiş noktası ortaya çıkarmadan hassas bilgileri daha güvenli bir şekilde işleme yeteneği.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-apache-hbase-cluster-into-virtual-network"></a>Sanal ağda Apache HBase kümesi oluşturma

Bu bölümde, [Azure Kaynak Yöneticisi şablonu](../../azure-resource-manager/templates/deploy-powershell.md)kullanarak bir Azure sanal ağında bağımlı Azure Depolama hesabıyla birlikte Linux tabanlı bir Apache HBase kümesi oluşturursunuz. Diğer küme oluşturma yöntemleri ve ayarları anlamak için [bkz.](../hdinsight-hadoop-provision-linux-clusters.md) HDInsight'ta Apache Hadoop kümeleri oluşturmak için şablon kullanma hakkında daha fazla bilgi için Azure [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)

> [!NOTE]  
> Bazı özellikler şablona sabit kodlanır. Örnek:
>
> * **Yer**: Doğu ABD 2
> * **Küme sürümü**: 3.6
> * **Küme işçi düğümü sayısı**: 2
> * **Varsayılan depolama hesabı:** benzersiz bir dize
> * **Sanal ağ adı**: CLUSTERNAME-vnet
> * **Sanal ağ adres alanı**: 10.0.0.0/16
> * **Alt ad**: subnet1
> * **Alt net adres aralığı**: 10.0.0.0/24
>
> `CLUSTERNAME`şablonu kullanırken sağladığınız küme adı ile değiştirilir.

1. Azure portalında şablonu açmak için aşağıdaki resmi seçin. Şablon Azure [quickstart şablonlarında](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-linux-vnet/)bulunur.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-linux-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/apache-hbase-provision-vnet/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

1. Özel **dağıtım** iletişim kutusundan **şablonu edit'i**seçin.

1. Satır 165'te `Standard_A3` değeri `Standard_A4_V2`' den ' e değiştirin Ardından **Kaydet'i**seçin.

1. Kalan şablonu aşağıdaki bilgilerle tamamlayın:

    |Özellik |Değer |
    |---|---|
    |Abonelik|HDInsight kümesini, bağımlı Depolama hesabını ve Azure sanal ağını oluşturmak için kullanılan bir Azure aboneliği seçin.|
    Kaynak grubu|**Yeni oluştur'u**seçin ve yeni bir kaynak grubu adı belirtin.|
    |Konum|Kaynak grubu için bir konum seçin.|
    |Küme Adı|Oluşturulacak Hadoop kümesi için bir ad girin.|
    |Cluster Giriş Kullanıcı Adı ve Şifresi|Varsayılan Kullanıcı Adı **yöneticidir.** Bir parola sağlayın.|
    |Ssh Kullanıcı Adı ve Şifresi|Varsayılan Kullanıcı Adı **sshuser'dir.**  Bir parola sağlayın.|

    **Seç ben şartları ve yukarıda belirtilen koşulları kabul seçin.**

1. **Satın al**'ı seçin. Bir küme oluşturmak yaklaşık 20 dakika sürer. Küme oluşturulduktan sonra, açmak için portaldaki kümeyi seçebilirsiniz.

Makaleyi tamamladıktan sonra kümeyi silmek isteyebilirsiniz. HDInsight ile, verileriniz Azure Storage’da depolanır, böylece kullanılmadığında bir kümeyi güvenle silebilirsiniz. Ayrıca, kullanılmıyorken dahi HDInsight kümesi için sizden ücret kesilir. Küme ücretleri depolama ücretlerinin birkaç katı olduğundan, kullanılmadığında kümelerin silinmesi mantıklı olandır. Bir kümeyi silme yönergeleri için Azure [portalını kullanarak HDInsight'taki Apache Hadoop kümelerini yönet'e](../hdinsight-administer-use-portal-linux.md#delete-clusters)bakın.

Yeni HBase kümenizle çalışmaya başlamak için, [HDInsight'ta Apache Hadoop ile Apache HBase'i kullanmaya başlayın'da](./apache-hbase-tutorial-get-started-linux.md)bulunan yordamları kullanabilirsiniz.

## <a name="connect-to-the-apache-hbase-cluster-using-apache-hbase-java-rpc-apis"></a>Apache HBase Java RPC API'lerini kullanarak Apache HBase kümesine bağlanın

### <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Aynı Azure sanal ağında ve aynı alt ağda hizmet (IaaS) sanal makine olarak bir altyapı oluşturun. Yeni bir IaaS sanal makine oluşturma yla ilgili talimatlar için [bkz.](../../virtual-machines/windows/quick-create-portal.md) Bu belgedeki adımları izleyerek, Ağ yapılandırması için aşağıdaki değerleri kullanmanız gerekir:

* **Sanal ağ**: CLUSTERNAME-vnet
* **Alt net**: subnet1

> [!IMPORTANT]  
> Önceki `CLUSTERNAME` adımlarda HDInsight kümesini oluştururken kullandığınız adla değiştirin.

Bu değerleri kullanarak, sanal makine HDInsight kümesi yle aynı sanal ağa ve alt ağa yerleştirilir. Bu yapılandırma, birbirleriyle doğrudan iletişim kurmalarını sağlar. Boş bir kenar düğümü olan bir HDInsight kümesi oluşturmanın bir yolu vardır. Kenar düğümü kümeyi yönetmek için kullanılabilir.  Daha fazla bilgi için bkz: [HDInsight'ta boş kenar düğümlerini kullan.](../hdinsight-apps-use-edge-node.md)

### <a name="obtain-fully-qualified-domain-name"></a>Tam nitelikli alan adı edinin

HBase'e uzaktan bağlanmak için bir Java uygulamasını kullanırken, tam nitelikli alan adını (FQDN) kullanmanız gerekir. Bunu belirlemek için, HBase kümesinin bağlantıya özgü DNS sonekini almanız gerekir. Bunu yapmak için aşağıdaki yöntemlerden birini kullanabilirsiniz:

* [Apache Ambari](https://ambari.apache.org/) araması yapmak için Bir Web tarayıcısı kullanın:

    `https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts?minimal_response=true` adresine gidin. DNS sonekleri içeren bir JSON dosyasını döndürür.

* Ambari web sitesini kullanın:

    1. `https://CLUSTERNAME.azurehdinsight.net` adresine gidin.
    2. Üst menüden **Ana Bilgisayarlar'ı** seçin.

* REST aramaları yapmak için Curl'i kullanın:

    ```bash
    curl -u <username>:<password> -k https://CLUSTERNAME.azurehdinsight.net/ambari/api/v1/clusters/CLUSTERNAME.azurehdinsight.net/services/hbase/components/hbrest
    ```

Döndürülen JavaScript Nesne Gösterimi (JSON) verilerinde "host_name" girişini bulun. Kümedeki düğümler için FQDN içerir. Örnek:

```
"host_name" : "hn0-hbaseg.hjfrnszlumfuhfk4pi1guh410c.bx.internal.cloudapp.net"
```

Etki alanı adının küme adı ile başlayan bölümü DNS sonekidir. Örneğin, `hjfrnszlumfuhfk4pi1guh410c.bx.internal.cloudapp.net`.

<!--
3.    Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix](./media/apache-hbase-provision-vnet/hdi-primary-dns-suffix.png)
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

### <a name="verify-communication-inside-virtual-network"></a>Sanal ağ içindeki iletişimi doğrulama

Sanal makinenin HBase kümesiyle iletişim kurabileceğini doğrulamak `ping headnode0.<dns suffix>` için sanal makinenin komutunu kullanın. Örneğin, `ping hn0-hbaseg.hjfrnszlumfuhfk4pi1guh410c.bx.internal.cloudapp.net`.

Bu bilgileri bir Java uygulamasında kullanmak için, bir uygulama oluşturmak [için HDInsight (Hadoop) ile Apache HBase kullanan Java uygulamaları oluşturmak için Apache Maven'i kullan](./apache-hbase-build-java-maven-linux.md) adımları izleyebilirsiniz. Uygulamanın uzak bir HBase sunucusuna bağlanması için, zookeeper için FQDN'yi kullanmak için bu örnekteki **hbase-site.xml** dosyasını değiştirin. Örnek:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [!NOTE]  
> Azure sanal ağlarında ad çözümlemesi hakkında daha fazla bilgi için (kendi DNS sunucunuzu nasıl kullanacağınız da dahil olmak üzere) [bkz.](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir Apache HBase kümesi oluşturmayı öğrendiniz. Daha fazla bilgi için bkz:

* [HDInsight'ı kullanmaya başlama](../hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [HDInsight'ta boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md)
* [HDInsight'ta Apache HBase çoğaltmayı yapılandırma](apache-hbase-replication.md)
* [HDInsight'ta Apache Hadoop kümeleri oluşturma](../hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight'ta Apache Hadoop ile Apache HBase'i kullanmaya başlayın](./apache-hbase-tutorial-get-started-linux.md)
* [Sanal Ağ’a Genel Bakış](../../virtual-network/virtual-networks-overview.md)
