---
title: Azure HDInsight için sanal ağ planlama
description: HDInsight'ı diğer bulut kaynaklarına veya veri merkezinizdeki kaynaklara bağlamak için Azure Sanal Ağ dağıtımını nasıl planlayabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/21/2020
ms.openlocfilehash: 18774ae4a98b795846459251174ee47671aef39c
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769892"
---
# <a name="plan-a-virtual-network-for-azure-hdinsight"></a>Azure HDInsight için sanal ağ planlama

Bu makalede, Azure HDInsight ile [Azure Sanal Ağları](../virtual-network/virtual-networks-overview.md) (VNets) kullanma hakkında arka plan bilgileri verilmektedir. Ayrıca, HDInsight kümeniz için sanal bir ağ uygulamadan önce verilmesi gereken tasarım ve uygulama kararlarını da tartışır. Planlama aşaması tamamlandıktan sonra, [Azure HDInsight kümeleri için sanal ağlar oluşturmaya](hdinsight-create-virtual-network.md)devam edebilirsiniz. Ağ güvenlik gruplarını (NSG'ler) ve kullanıcı tanımlı yolları düzgün bir şekilde yapılandırmak için gereken HDInsight yönetimi IP adresleri hakkında daha fazla bilgi için [HDInsight yönetim IP adreslerine](hdinsight-management-ip-addresses.md)bakın.

Azure Sanal Ağı kullanmak aşağıdaki senaryoları sağlar:

* HDInsight'a doğrudan şirket içi bir ağdan bağlanma.
* HDInsight'ı bir Azure Sanal ağındaki veri depolarına bağlama.
* Doğrudan internet üzerinden kamuya açık olmayan Apache Hadoop hizmetlerine erişme. Örneğin, Apache Kafka API'leri veya Apache HBase Java API'si.

> [!IMPORTANT]
> VNET'te HDInsight kümesi oluşturmak, NIC'ler ve yük dengeleyicileri gibi çeşitli ağ kaynakları oluşturur. Kümenizin VNET ile düzgün çalışması için gerekli olan bu ağ kaynaklarını **not** silmeyin.
>
> 28 Şubat 2019'dan sonra, Bir VNET'te oluşturulan Yenİ HDInsight kümeleri için ağ kaynakları (NIC' ler, LB'ler, vb. gibi) aynı HDInsight küme kaynak grubunda sağlanacaktır. Daha önce, bu kaynaklar VNET kaynak grubunda sağlanmış. VNET olmadan oluşturulan geçerli çalışan kümelerde ve bu kümelerde değişiklik yoktur.

## <a name="planning"></a>Planlama

HDInsight'ı sanal bir ağa yüklemeyi planlarken yanıtlamanız gereken sorular şunlardır:

* HDInsight'ı varolan bir sanal ağa yüklemeniz mi gerekiyor? Yoksa yeni bir ağ mı oluşturuyorsunuz?

    Varolan bir sanal ağ kullanıyorsanız, HDInsight'ı yüklemeden önce ağ yapılandırmasını değiştirmeniz gerekebilir. Daha fazla bilgi [için, varolan bir sanal ağ bölümüne HDInsight ekleyin bölümüne](#existingvnet) bakın.

* HDInsight içeren sanal ağı başka bir sanal ağa veya şirket içi ağınıza bağlamak istiyor musunuz?

    Ağlar daki kaynaklarla kolayca çalışmak için özel bir DNS oluşturmanız ve DNS iletmesini yapılandırmanız gerekebilir. Daha fazla bilgi için [birden çok ağ bağlama](#multinet) bölümüne bakın.

* Gelen veya giden trafiği HDInsight'a kısıtlamak/yönlendirmek istiyor musunuz?

    HDInsight'ın Azure veri merkezindeki belirli IP adresleriyle sınırsız iletişimi olmalıdır. İstemci iletişimi için güvenlik duvarlarından geçmesine izin verilmesi gereken birkaç bağlantı noktası da vardır. Daha fazla bilgi için [kontrol ağ trafiği](#networktraffic) bölümüne bakın.

## <a name="add-hdinsight-to-an-existing-virtual-network"></a><a id="existingvnet"></a>Varolan bir sanal ağa HDInsight ekleme

Varolan bir Azure Sanal Ağına nasıl yeni bir HDInsight ekleyeceğinizi keşfetmek için bu bölümdeki adımları kullanın.

> [!NOTE]  
> Varolan bir HDInsight kümesini sanal ağa ekleyemezsiniz.

1. Sanal ağ için klasik veya Kaynak Yöneticisi dağıtım modeli mi kullanıyorsunuz?

    HDInsight 3.4 ve üzeri bir Kaynak Yöneticisi sanal ağ gerektirir. HDInsight'ın önceki sürümlerinde klasik bir sanal ağ gerekiyordu.

    Varolan ağınız klasik bir sanal ağsa, bir Kaynak Yöneticisi sanal ağ oluşturmanız ve sonra ikisini bağlamanız gerekir. [Klasik VNet'leri yeni VNets'lere bağlama.](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md)

    Bir kez katıldıktan sonra, Kaynak Yöneticisi ağında yüklü HDInsight klasik ağdaki kaynaklarla etkileşim kurabilir.

2. Trafiği sanal ağa veya sanal ağa veya dışına sınırlamak için ağ güvenlik gruplarını, kullanıcı tanımlı yolları veya Sanal Ağ Cihazları'nı kullanıyor musunuz?

    Yönetilen bir hizmet olarak HDInsight, Azure veri merkezindeki birkaç IP adresine sınırsız erişim gerektirir. Bu IP adresleriyle iletişime izin vermek için varolan ağ güvenlik gruplarını veya kullanıcı tanımlı yolları güncelleştirin.

    HDInsight, çeşitli bağlantı noktaları kullanan birden çok hizmete ev sahipliği yapmaktadır. Bu bağlantı noktalarının trafiğini engellemeyin. Sanal cihaz güvenlik duvarlarından izin verecek bağlantı noktalarının listesi için Güvenlik bölümüne bakın.

    Varolan güvenlik yapılandırmanızı bulmak için aşağıdaki Azure PowerShell veya Azure CLI komutlarını kullanın:

    * Ağ güvenlik grupları

        Sanal `RESOURCEGROUP` ağı içeren kaynak grubunun adı ile değiştirin ve ardından komutu girin:

        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```

        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        Daha fazla bilgi için [Sorun Giderme ağı güvenlik grupları](../virtual-network/diagnose-network-traffic-filter-problem.md) belgesine bakın.

        > [!IMPORTANT]  
        > Ağ güvenliği grubu kuralları kural önceliğine göre sırayla uygulanır. Trafik deseniyle eşleşen ilk kural uygulanır ve bu trafik için başka hiçbir kural uygulanmaz. En müsamahakardan en az izin verilene kadar emir kuralları. Daha fazla bilgi için ağ [güvenlik grupları belgesiyle Filtre ağı trafiğine](../virtual-network/security-overview.md) bakın.

    * Kullanıcı tanımlı yollar

        Sanal `RESOURCEGROUP` ağı içeren kaynak grubunun adı ile değiştirin ve ardından komutu girin:

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        Daha fazla bilgi için [Sorun Giderme yolları](../virtual-network/diagnose-network-routing-problem.md) belgesine bakın.

3. Bir HDInsight kümesi oluşturun ve yapılandırma sırasında Azure Sanal Ağı'nı seçin. Küme oluşturma işlemini anlamak için aşağıdaki belgelerdeki adımları kullanın:

    * [Azure portalını kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [Azure PowerShell kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [Azure Classic CLI'yi kullanarak HDInsight oluşturun](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [Azure Kaynak Yöneticisi şablonu kullanarak HDInsight oluşturma](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > SANAL ağa HDInsight eklemek isteğe bağlı bir yapılandırma adımıdır. Kümeyi yapılandırırken sanal ağı seçtiğinizden emin olun.

## <a name="connecting-multiple-networks"></a><a id="multinet"></a>Birden çok ağı bağlama

Çok ağ yapılandırması ile en büyük sorun ağlar arasında ad çözümüdür.

Azure, sanal ağda yüklü olan Azure hizmetleri için ad çözümlemesi sağlar. Bu yerleşik ad çözünürlüğü, HDInsight'ın tam nitelikli bir etki alanı adı (FQDN) kullanarak aşağıdaki kaynaklara bağlanmasını sağlar:

* Internet üzerinde kullanılabilir herhangi bir kaynak. Örneğin, microsoft.com, windowsupdate.com.

* Kaynağın __dahili DNS adını__ kullanarak aynı Azure Sanal Ağı'nda bulunan tüm kaynaklar. Örneğin, varsayılan ad çözümlemesi kullanırken, HDInsight alt düğümlerine atanan dahili DNS adlarına örnekler şunlardır:

  * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    Her iki düğüm de dahili DNS adlarını kullanarak birbirleriyle ve HDInsight'taki diğer düğümlerle doğrudan iletişim kurabilir.

Varsayılan ad çözümü, HDInsight'ın sanal ağa katılan ağlardaki kaynakların adlarını çözmesine izin __vermez.__ Örneğin, şirket içi ağınızı sanal ağa katılmak yaygındır. Yalnızca varsayılan ad çözünürlüğüyle, HDInsight şirket içi ağdaki kaynaklara adıyla erişemez. Bunun tersi de doğrudur, şirket içi ağınızdaki kaynaklar sanal ağdaki kaynaklara adıyla erişemez.

> [!WARNING]  
> HdInsight kümesini oluşturmadan önce özel DNS sunucusuoluşturmanız ve sanal ağı kullanacak şekilde yapılandırmanız gerekir.

Sanal ağ ve birleştirilmiş ağlardaki kaynaklar arasında ad çözümlemesini etkinleştirmek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

1. HDInsight'ı yüklemeyi planladığınız Azure Sanal Ağı'nda özel bir DNS sunucusu oluşturun.

2. Sanal ağı özel DNS sunucusunu kullanacak şekilde yapılandırın.

3. Sanal ağınız için Azure atanan DNS sonekini bulun. Bu değer `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`. DNS soneki bulma hakkında bilgi için [Bkz. Örnek: Özel DNS](hdinsight-create-virtual-network.md#example-dns) bölümüne bakın.

4. DNS sunucuları arasında iletme yapılandırma. Yapılandırma uzak ağ türüne bağlıdır.

   * Uzak ağ şirket içi bir ağsa, DNS'yi aşağıdaki gibi yapılandırın:

     * __Özel DNS__ (sanal ağda):

         * Sanal ağın DNS sonekini Azure özyinelemeli çözümleyiciye iletin (168.63.129.16). Azure, sanal ağdaki kaynak isteklerini işler

         * Diğer tüm istekleri şirket içi DNS sunucusuna iletin. Şirket içi DNS, diğer tüm ad çözümleme isteklerini, hatta Microsoft.com gibi internet kaynakları isteklerini işler.

     * __Şirket içi DNS__: Sanal ağ DNS soneki için istekleri özel DNS sunucusuna iletin. Özel DNS sunucusu daha sonra Azure özyinelemeli çözümleyiciye iletilir.

       Bu yapılandırma, sanal ağın DNS sonekini içeren tam nitelikli alan adları isteklerini özel DNS sunucusuna yönlendirir. Diğer tüm istekler (genel internet adresleri için bile) şirket içi DNS sunucusu tarafından işlenir.

   * Uzak ağ başka bir Azure Sanal Ağıysa, DNS'yi aşağıdaki gibi yapılandırın:

     * __Özel DNS__ (her sanal ağda):

         * Sanal ağların DNS soneklerine yönelik istekler özel DNS sunucularına iletilir. Her sanal ağdaki DNS, kendi ağı içindeki kaynakları çözmekten sorumludur.

         * Diğer tüm istekleri Azure özyinelemeli çözümleyicisine iletin. Özyinelemeli çözümleyici, yerel ve internet kaynaklarının çözümünden sorumludur.

       Her ağ için DNS sunucusu, DNS sonekine dayalı olarak istekleri diğerine iletir. Diğer istekler Azure özyinelemeli çözümleyici kullanılarak çözülür.

     Her yapılandırmaörneği için [Bkz. Örnek: Özel DNS](hdinsight-create-virtual-network.md#example-dns) bölümü.

Daha fazla bilgi [için, VM'ler ve Rol Örnekleri belgesi için Ad Çözünürlüğü'ne](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) bakın.

## <a name="directly-connect-to-apache-hadoop-services"></a>Doğrudan Apache Hadoop hizmetlerine bağlanın

Kümeye ' den `https://CLUSTERNAME.azurehdinsight.net`bağlanabilirsiniz. Bu adres, internetten gelen trafiği kısıtlamak için NSG'leri kullandıysanız erişilemeyecek ortak bir IP kullanır. Ayrıca, kümeyi bir VNet'te dağıttığınızda özel bitiş noktasını `https://CLUSTERNAME-int.azurehdinsight.net`kullanarak kümeye erişebilirsiniz. Bu uç nokta küme erişimi için VNet içinde özel bir IP'ye gider.

Sanal ağ üzerinden Apache Ambari ve diğer web sayfalarına bağlanmak için aşağıdaki adımları kullanın:

1. HDInsight küme düğümlerinin dahili tam nitelikli alan adlarını (FQDN) keşfetmek için aşağıdaki yöntemlerden birini kullanın:

    Sanal `RESOURCEGROUP` ağı içeren kaynak grubunun adı ile değiştirin ve ardından komutu girin:

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    Döndürülen düğümler listesinde, baş düğümleri için FQDN'yi bulun ve Ambari ve diğer web hizmetlerine bağlanmak için FQDN'leri kullanın. Örneğin, Ambari'ye erişmek için kullanın. `http://<headnode-fqdn>:8080`

    > [!IMPORTANT]  
    > Baş düğümlerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümüzerinde etkindir. Bir baş düğümünde bir hizmete erişmeye çalışırsanız ve 404 hatası döndürürse, diğer baş düğümüne geçin.

2. Bir hizmetin kullanılabilen düğüm ve bağlantı noktasını belirlemek [için, HDInsight belgesinde Hadoop hizmetleri tarafından kullanılan Bağlantı Noktaları'na](./hdinsight-hadoop-port-settings-for-services.md) bakın.

## <a name="controlling-network-traffic"></a><a id="networktraffic"></a>Ağ trafiğini denetleme

### <a name="techniques-for-controlling-inbound-and-outbound-traffic-to-hdinsight-clusters"></a>HDInsight kümelerine gelen ve giden trafiği denetleme teknikleri

Azure Sanal Ağlar'daki ağ trafiği aşağıdaki yöntemler kullanılarak denetlenebilir:

* **Ağ güvenlik grupları** (NSG), ağa gelen ve giden trafiği filtrelemenize olanak tanır. Daha fazla bilgi için ağ [güvenlik grupları belgesiyle Filtre ağı trafiğine](../virtual-network/security-overview.md) bakın.

* **Ağ sanal cihazları** (NVA) yalnızca giden trafikile kullanılabilir. NVA'lar güvenlik duvarları ve yönlendiriciler gibi aygıtların işlevselliğini çoğaltAbilir. Daha fazla bilgi için [Ağ Cihazları](https://azure.microsoft.com/solutions/network-appliances) belgesine bakın.

Yönetilen bir hizmet olarak HDInsight, Hem VNET'ten gelen hem de giden trafik için HDInsight sağlık ve yönetim hizmetlerine sınırsız erişim gerektirir. NSG kullanırken, bu hizmetlerin HDInsight kümesiyle hala iletişim kuradığından emin olmalısınız.

![Azure özel VNET'te oluşturulan HDInsight varlıklarının diyagramı](./media/hdinsight-plan-virtual-network-deployment/hdinsight-vnet-diagram.png)

### <a name="hdinsight-with-network-security-groups"></a>Ağ güvenlik gruplarıyla HDInsight

Ağ trafiğini denetlemek için **ağ güvenlik gruplarını** kullanmayı planlıyorsanız, HDInsight'ı yüklemeden önce aşağıdaki işlemleri gerçekleştirin:

1. HDInsight için kullanmayı planladığınız Azure bölgesini tanımlayın.

2. HdInsight'ın bölgeniz için gerektirdiği hizmet etiketlerini belirleyin. Daha fazla bilgi için [Azure HDInsight için Ağ güvenlik grubu (NSG) hizmet etiketlerine](hdinsight-service-tags.md)bakın.

3. HDInsight'ı yüklemeyi planladığınız alt ağ için ağ güvenlik gruplarını oluşturun veya değiştirin.

    * __Ağ güvenlik grupları:__ IP adreslerinden bağlantı noktası __443'te__ __gelen__ trafiğe izin verir. Bu, HDInsight yönetim hizmetlerinin kümeye sanal ağ dışından ulaşmasını sağlar.

Ağ güvenlik grupları hakkında daha fazla bilgi için [ağ güvenlik gruplarına genel bakış](../virtual-network/security-overview.md)alabakın.

### <a name="controlling-outbound-traffic-from-hdinsight-clusters"></a>HDInsight kümelerinden giden trafiği denetleme

HDInsight kümelerinden giden trafiği denetleme hakkında daha fazla bilgi için Azure [HDInsight kümeleri için giden ağ trafiği kısıtlamalarını yapılandırma'ya](hdinsight-restrict-outbound-traffic.md)bakın.

#### <a name="forced-tunneling-to-on-premises"></a>Şirket içi zorunlu tünel

Zorunlu tünelleme, bir alt ağdaki tüm trafiğin şirket içi ağınız gibi belirli bir ağa veya konuma zorlandığı kullanıcı tanımlı yönlendirme yapılandırmasıdır. HDInsight, şirket içi ağlara zorla trafik tüneli nin indirilmesine destek __vermez.__

## <a name="required-ip-addresses"></a><a id="hdinsight-ip"></a>Gerekli IP adresleri

Trafiği denetlemek için ağ güvenlik gruplarını veya kullanıcı tanımlı yolları kullanıyorsanız, [HDInsight yönetim IP adreslerine](hdinsight-management-ip-addresses.md)bakın.

## <a name="required-ports"></a><a id="hdinsight-ports"></a>Gerekli bağlantı noktaları

Bir **güvenlik duvarı** kullanmayı ve kümeye belirli bağlantı noktalarında dışarıdan erişmeye planlıyorsanız, senaryonuz için gereken bağlantı noktalarında ki trafiğe izin vermeniz gerekebilir. Varsayılan olarak, önceki bölümde açıklanan Azure yönetim trafiğinin 443 numaralı bağlantı noktasında kümeye ulaşmasına izin verildiği sürece bağlantı noktalarının özel bir beyaz listesine gerek yoktur.

Belirli hizmetleriçin bağlantı noktalarının listesi için, [HDInsight belgesinde Apache Hadoop hizmetleri tarafından kullanılan bağlantı noktalarına](hdinsight-hadoop-port-settings-for-services.md) bakın.

Sanal cihazlar için güvenlik duvarı kuralları hakkında daha fazla bilgi için [sanal cihaz senaryo](../virtual-network/virtual-network-scenario-udr-gw-nva.md) belgesine bakın.

## <a name="load-balancing"></a>Yük dengeleme

Bir HDInsight kümesi oluşturduğunuzda, yük dengeleyicide de bir yük dengeleyici oluşturulur. Bu yük dengeleyicitürü temel [SKU düzeyinde](../load-balancer/concepts-limitations.md#skus), hangi belirli kısıtlamalar vardır. Bu kısıtlamalardan biri, farklı bölgelerde iki sanal ağınız varsa, temel yük dengeleyicilerine bağlanamazsınız. Daha fazla bilgi için [sanal ağlara bakın: küresel vnet eşleme kısıtlamaları.](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)

## <a name="next-steps"></a>Sonraki adımlar

* Kod örnekleri ve Azure Sanal Ağlar oluşturma örnekleri [için](hdinsight-create-virtual-network.md)bkz.
* HDInsight'ı şirket içi bir ağa bağlanacak şekilde yapılandırmanın uçtan uca bir örneği için, [bkz.](./connect-on-premises-network.md)
* Azure sanal ağları hakkında daha fazla bilgi için [Azure Sanal Ağı'na genel bakış](../virtual-network/virtual-networks-overview.md)bilgisine bakın.
* Ağ güvenlik grupları hakkında daha fazla bilgi için [Ağ güvenlik gruplarına](../virtual-network/security-overview.md)bakın.
* Kullanıcı tanımlı rotalar hakkında daha fazla bilgi [için, Kullanıcı tanımlı rotalar ve IP yönlendirme](../virtual-network/virtual-networks-udr-overview.md)bakın.
