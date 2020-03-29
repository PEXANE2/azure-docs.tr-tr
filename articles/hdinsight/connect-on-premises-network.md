---
title: Azure HDInsight'ı şirket içi ağınıza bağlayın
description: Azure Sanal Ağı'nda hdinsight kümesini nasıl oluşturup şirket içi ağınıza nasıl bağlayabilirsiniz öğrenin. Özel bir DNS sunucusu kullanarak HDInsight ve şirket içi ağınız arasında ad çözünürlüğünü nasıl yapılandıracaklarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/04/2020
ms.openlocfilehash: 2ed7a5b9c81d1b50f80f379a88688b69c49ed382
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78897910"
---
# <a name="connect-hdinsight-to-your-on-premises-network"></a>HDInsight’ı şirket içi ağınıza bağlama

Azure Sanal Ağlar ve VPN ağ geçidi ni kullanarak HDInsight'ı şirket içi ağınıza nasıl bağlayabilirsiniz öğrenin. Bu belge, planlama bilgileri sağlar:

* Şirket içi ağınıza bağlanan bir Azure Sanal Ağında HDInsight'ı kullanma.
* Sanal ağ ve şirket içi ağınız arasında DNS ad çözümlemesi yapılandırma.
* Ağ güvenlik gruplarını HDInsight'a internet erişimini kısıtlayabilmek için yapılandırma.
* HDInsight tarafından sanal ağda sağlanan bağlantı noktaları.

## <a name="overview"></a>Genel Bakış

HDInsight'ın ve birleştirilmiş ağdaki kaynakların adıyla iletişim kurmasına izin vermek için aşağıdaki eylemleri gerçekleştirmeniz gerekir:

1. Azure Sanal Ağı oluşturun.
1. Azure Sanal Ağı'nda özel bir DNS sunucusu oluşturun.
1. Sanal ağı varsayılan Azure Tekrarı Çözümleyicisi yerine özel DNS sunucusunu kullanacak şekilde yapılandırın.
1. Özel DNS sunucusu ile şirket içi DNS sunucunuz arasında iletme yapılandırma.

Bu yapılandırmalar aşağıdaki davranışı sağlar:

* __Sanal ağ için__ DNS soneki olan tam nitelikli alan adları için istekler özel DNS sunucusuna iletilir. Özel DNS sunucusu daha sonra bu istekleri IP adresini döndüren Azure Özyinelemeli Çözümleyici'ye ileter.
* Diğer tüm istekler şirket içi DNS sunucusuna iletilir. microsoft.com gibi genel internet kaynaklarına yönelik istekler bile ad çözümlemesi için şirket içi DNS sunucusuna iletilir.

Aşağıdaki diyagramda, yeşil çizgiler sanal ağın DNS sonekinde biten kaynaklar için isteklerdir. Mavi çizgiler, şirket içi ağdaki veya genel internet üzerindeki kaynaklar için isteklerdir.

![Yapılandırmada DNS isteklerinin nasıl çözüldüğüne göre diyagramı](./media/connect-on-premises-network/on-premises-to-cloud-dns.png)

## <a name="prerequisites"></a>Ön koşullar

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](./hdinsight-hadoop-linux-use-ssh-unix.md)bakın.
* PowerShell kullanıyorsanız, [AZ Modülü](https://docs.microsoft.com/powershell/azure/overview)gerekir.
* Azure CLI'yi kullanmak istiyorsanız ve henüz yüklemediyseniz, Azure [CLI'yi yükleyin'e](https://docs.microsoft.com/cli/azure/install-azure-cli)bakın.

## <a name="create-virtual-network-configuration"></a>Sanal ağ yapılandırması oluşturma

Şirket içi ağınıza bağlı bir Azure Sanal Ağı'nı nasıl oluşturabilirsiniz öğrenmek için aşağıdaki belgeleri kullanın:

* [Azure portalını kullanma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
* [Azure PowerShell’i kullanma](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
* [Azure CLI’yı kullanma](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md)

## <a name="create-custom-dns-server"></a>Özel DNS sunucusu oluşturma

> [!IMPORTANT]  
> HDInsight'ı sanal ağa yüklemeden önce DNS sunucusuoluşturmalı ve yapılandırmanız gerekir.

Bu adımlar, Azure Sanal Makinesi oluşturmak için [Azure portalını](https://portal.azure.com) kullanır. Sanal makine oluşturmanın diğer yolları için [VM Oluştur - Azure CLI](../virtual-machines/linux/quick-create-cli.md) ve [Create VM - Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)'e bakın.  [Bind](https://www.isc.org/downloads/bind/) DNS yazılımını kullanan bir Linux VM oluşturmak için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com)oturum açın.
  
1. Üst menüden **+ Kaynak oluştur'u**seçin.

    ![Ubuntu sanal makinesi oluşturma](./media/connect-on-premises-network/azure-portal-create-resource.png)

1. **Sanal makine oluştur** sayfasına gitmek için Sanal **İşlem** > **Sanal makinesini** seçin.

1. Temel __Bilgiler__ sekmesinden aşağıdaki bilgileri girin:  
  
    | Alan | Değer |
    | --- | --- |
    |Abonelik |Uygun aboneliğinizi seçin.|
    |Kaynak grubu |Daha önce oluşturulan sanal ağı içeren kaynak grubunu seçin.|
    |Sanal makine adı | Bu sanal makineyi tanımlayan bir ad girin. Bu **örnekte DNSProxy**kullanır.|
    |Bölge | Daha önce oluşturulan sanal ağla aynı bölgeyi seçin.  Tüm VM boyutları tüm bölgelerde kullanılamaz.  |
    |Kullanılabilirlik seçenekleri |  İstediğiniz kullanılabilirlik düzeyini seçin.  Azure, uygulamalarınız için kullanılabilirliği ve esnekliği yönetmek için çeşitli seçenekler sunar.  Uygulamalarınızı ve verilerinizi veri merkezi kesintilerinden ve bakım olaylarından korumak için Kullanılabilirlik Bölgelerinde veya Kullanılabilirlik Kümelerinde çoğaltılan VM'leri kullanacak şekilde çözümünüzü tasarlayın. Bu örnekte **altyapı artıklığı gerekmez.** |
    |Görüntü | **Ubuntu Server 18.04 LTS'de**bırakın. |
    |Kimlik doğrulaması türü | __Parola__ veya __SSH ortak anahtarı__: SSH hesabının kimlik doğrulama yöntemidir. Daha güvenli oldukları için ortak anahtarları kullanmanızı öneririz. Bu **örnekte Parola**kullanır.  Daha fazla bilgi için Linux VM belgesi [için SSH tuşu oluştur ve kullan.](../virtual-machines/linux/mac-create-ssh-keys.md)|
    |Kullanıcı adı |VM için yönetici kullanıcı adını girin.  Bu **örneksshuser**kullanır.|
    |Parola veya SSH ortak anahtarı | Kullanılabilir **alan, Kimlik Doğrulama türü**için seçtiğiniz alana göre belirlenir.  Uygun değeri girin.|
    |Genel gelen bağlantı noktaları|**Seçili bağlantı noktalarına izin ver'i**seçin. Ardından **gelen bağlantı noktalarını** seç açılır listesinden **SSH (22)** seçeneğini belirleyin.|

    ![Sanal makine temel yapılandırma](./media/connect-on-premises-network/virtual-machine-basics.png)

    Varsayılan değerlerde diğer girişleri bırakın ve ardından **Ağ** sekmesini seçin.

4. **Ağ** sekmesinden aşağıdaki bilgileri girin:

    | Alan | Değer |
    | --- | --- |
    |Sanal ağ | Daha önce oluşturduğunuz sanal ağı seçin.|
    |Alt ağ | Daha önce oluşturduğunuz sanal ağ için varsayılan alt ağı seçin. VPN ağ geçidi tarafından kullanılan alt ağı __seçmeyin.__|
    |Genel IP | Otomatik doldurulan değeri kullanın.  |

    ![HDInsight Sanal ağ ayarları](./media/connect-on-premises-network/virtual-network-settings.png)

    Varsayılan değerlerde diğer girişleri bırakın ve sonra **Gözden Geçir + oluştur'u**seçin.

5. Gözden **Geçir + oluştur** sekmesinden, sanal makineyi oluşturmak için **Oluştur'u** seçin.

### <a name="review-ip-addresses"></a>IP Adreslerini İncele

Sanal makine oluşturulduktan sonra, kaynak için **git** düğmesiyle **dağıtım başarılı** bir bildirim alırsınız.  Yeni sanal makinenize gitmek **için kaynağa git'i** seçin.  Yeni sanal makinenizin varsayılan görünümünden, ilişkili IP Adreslerini tanımlamak için aşağıdaki adımları izleyin:

1. **Ayarlar'dan** **Özellikler'i**seçin.

2. Daha sonra kullanmak için **GENEL IP ADRESİ/DNS AD ETIKETI** ve ÖZEL IP **ADRESi** değerlerini not edin.

   ![Genel ve özel IP adresleri](./media/connect-on-premises-network/virtual-machine-ip-addresses.png)

### <a name="install-and-configure-bind-dns-software"></a>Bind 'i yükleme ve yapılandırma (DNS yazılımı)

1. Sanal makinenin genel __IP adresine__ bağlanmak için SSH'yi kullanın. VM oluştururken belirttiğiniz SSH kullanıcı hesabıile değiştirin. `sshuser` Aşağıdaki örnek 40.68.254.142 de sanal bir makineye bağlanır:

    ```bash
    ssh sshuser@40.68.254.142
    ```

2. Bind'i yüklemek için SSH oturumundaki aşağıdaki komutları kullanın:

    ```bash
    sudo apt-get update -y
    sudo apt-get install bind9 -y
    ```

3. Bind'i şirket içi DNS sunucunuza ad çözümleme isteklerini iletmek için `/etc/bind/named.conf.options` yapılandırmak için, dosyanın içeriği olarak aşağıdaki metni kullanın:

        acl goodclients {
            10.0.0.0/16; # Replace with the IP address range of the virtual network
            10.1.0.0/16; # Replace with the IP address range of the on-premises network
            localhost;
            localnets;
        };

        options {
                directory "/var/cache/bind";

                recursion yes;

                allow-query { goodclients; };

                forwarders {
                192.168.0.1; # Replace with the IP address of the on-premises DNS server
                };

                dnssec-validation auto;

                auth-nxdomain no;    # conform to RFC1035
                listen-on { any; };
        };

    > [!IMPORTANT]  
    > Bölümdeki `goodclients` değerleri sanal ağın IP adresi aralığı ve şirket içi ağ ile değiştirin. Bu bölümde, bu DNS sunucusunun istekleri kabul ettiği adresler tanımlanır.
    >
    > Bölümdeki `192.168.0.1` `forwarders` girişi şirket içi DNS sunucunuzun IP adresiyle değiştirin. Bu giriş, DNS isteklerini çözüm için şirket içi DNS sunucunuza yönlendirir.

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

    ```output
    dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net
    ```

    Metin, `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` bu sanal ağ için __DNS sonekidir.__ Daha sonra kullanıldığı gibi bu değeri kaydedin.

5. Sanal ağdaki kaynakların DNS adlarını çözümlemek için Bind'i yapılandırmak `/etc/bind/named.conf.local` için, dosyanın içeriği olarak aşağıdaki metni kullanın:

        // Replace the following with the DNS suffix for your virtual network
        zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
            type forward;
            forwarders {168.63.129.16;}; # The Azure recursive resolver
        };

    > [!IMPORTANT]  
    > Daha önce `icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net` aldığınız DNS sonekiyle değiştirmeniz gerekir.

    Bu dosyayı yeniden kullanmak için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /etc/bind/named.conf.local
    ```

    Dosyayı kaydetmek için __Ctrl+X__, __Y__ve ardından __Enter__'u kullanın.

6. Bind'i başlatmak için aşağıdaki komutu kullanın:

    ```bash
    sudo service bind9 restart
    ```

7. Bind'in şirket içi ağınızdaki kaynakların adlarını çözebileceğini doğrulamak için aşağıdaki komutları kullanın:

    ```bash
    sudo apt install dnsutils
    nslookup dns.mynetwork.net 10.0.0.4
    ```

    > [!IMPORTANT]  
    > Şirket `dns.mynetwork.net` içi ağınızdaki bir kaynağın tam nitelikli alan adı (FQDN) ile değiştirin.
    >
    > Sanal `10.0.0.4` ağdaki özel DNS sunucunuzun __dahili IP adresiyle__ değiştirin.

    Yanıt aşağıdaki metne benzer görünür:

    ```output
    Server:         10.0.0.4
    Address:        10.0.0.4#53

    Non-authoritative answer:
    Name:   dns.mynetwork.net
    Address: 192.168.0.4
    ```

## <a name="configure-virtual-network-to-use-the-custom-dns-server"></a>Özel DNS sunucusunu kullanmak için sanal ağı yapılandırma

Sanal ağı Azure özyinelemeli çözümleyici yerine özel DNS sunucusunu kullanacak şekilde yapılandırmak için [Azure portalından](https://portal.azure.com)aşağıdaki adımları kullanın:

1. Sol menüden Sanal**ağlara****Ağ Açan** >  **Tüm hizmetlere** > gidin.

2. Sanal ağınızın varsayılan görünümünü açacak listeden sanal ağınızı seçin.  

3. Varsayılan görünümden Ayarlar **altında** **DNS sunucularını**seçin.  

4. __Özel'i__seçin ve özel DNS sunucusunun **ÖZEL IP ADRESİ'ni** girin.

5. __Kaydet'i__seçin.  <br />  

    ![Ağ için özel DNS sunucusunu ayarlama](./media/connect-on-premises-network/configure-custom-dns.png)

## <a name="configure-on-premises-dns-server"></a>Şirket içi DNS sunucusuyapılandırma

Önceki bölümde, özel DNS sunucusunu istekleri şirket içi DNS sunucusuna iletecek şekilde yapılandırıldınız. Ardından, istekleri özel DNS sunucusuna iletecek şirket içi DNS sunucusunu yapılandırmanız gerekir.

DNS sunucunuzu nasıl yapılandırılabildiğinize ilişkin belirli adımlar için, DNS sunucu yazılımınızın belgelerine başvurun. __Koşullu bir iletme__yapılandırma ile ilgili adımları arayın.

Koşullu bir iletme yalnızca belirli bir DNS soneki için istekleri iletir. Bu durumda, sanal ağın DNS soneki için bir iletme yapılandırmanız gerekir. Bu sonek için istekler özel DNS sunucusunun IP adresine iletilmelidir. 

Aşağıdaki **metin, Bind** DNS yazılımı için koşullu iletme yapılandırması örneğidir:

    zone "icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The custom DNS server's internal IP address
    };

**Windows Server 2016'da**DNS kullanma hakkında daha fazla bilgi için [Add-DnsServerConditionalForwarderZone](https://technet.microsoft.com/itpro/powershell/windows/dnsserver/add-dnsserverconditionalforwarderzone) belgelerine bakın...

Şirket içi DNS sunucusunu yapılandırdıktan sonra, `nslookup` sanal ağdaki adları çözebileceğinizi doğrulamak için şirket içi ağdan kullanabilirsiniz. Aşağıdaki örnek 

```bash
nslookup dnsproxy.icb0d0thtw0ebifqt0g1jycdxd.ex.internal.cloudapp.net 196.168.0.4
```

Bu örnek, özel DNS sunucusunun adını çözmek için 196.168.0.4'teki şirket içi DNS sunucusunu kullanır. IP adresini şirket içi DNS sunucusuyla değiştirin. `dnsproxy` Adresi özel DNS sunucusunun tam nitelikli etki alanı adı ile değiştirin.

## <a name="optional-control-network-traffic"></a>İsteğe bağlı: Ağ trafiğini denetleme

Ağ trafiğini denetlemek için ağ güvenlik gruplarını (NSG) veya kullanıcı tanımlı yolları (UDR) kullanabilirsiniz. NSG'ler gelen ve giden trafiği filtrelemenize ve trafiğe izin vermenize veya reddetmenize olanak sağlar. ÜVER'ler, sanal ağ, internet ve şirket içi ağdaki kaynaklar arasında trafiğin nasıl aktığını denetlemenize olanak tanır.

> [!WARNING]  
> HDInsight, Azure bulutundaki belirli IP adreslerinden gelen erişim ve sınırsız giden erişim gerektirir. Trafiği kontrol etmek için NSG'leri veya ÜD'leri kullanırken aşağıdaki adımları gerçekleştirmeniz gerekir:

1. Sanal ağınızı içeren konumun IP adreslerini bulun. Konuma göre gerekli IP'lerin listesi için [Gerekli IP adreslerine](./hdinsight-management-ip-addresses.md)bakın.

2. Adım 1'de tanımlanan IP adresleri için, bu IP adreslerinden gelen trafiğe izin verin.

   * __NSG__kullanıyorsanız : IP adresleri için __443__ no'daki bağlantı __noktasındagelen__ trafiğe izin verin.
   * __UDR__kullanıyorsanız : IP adresleri için __Internet'e__ giden rotanın __Sonraki Atlama__ türünü ayarlayın.

NSG'ler oluşturmak için Azure PowerShell veya Azure CLI'yi kullanma örneği için, Azure Sanal Ağlar la [HDInsight'ı Genişlet](hdinsight-create-virtual-network.md#hdinsight-nsg) belgesine bakın.

## <a name="create-the-hdinsight-cluster"></a>HDInsight kümesini oluşturma

> [!WARNING]  
> SANAL ağa HDInsight yüklemeden önce özel DNS sunucusunu yapılandırmanız gerekir.

Bir HDInsight kümesi oluşturmak için [Azure portal belgesini kullanarak BIR HDInsight kümesi](./hdinsight-hadoop-create-linux-clusters-portal.md) oluştur'daki adımları kullanın.

> [!WARNING]  
> * Küme oluşturma sırasında, sanal ağınızı içeren konumu seçmeniz gerekir.
> * Yapılandırmanın __Gelişmiş ayarlar__ bölümünde, daha önce oluşturduğunuz sanal ağı ve alt ağı seçmeniz gerekir.

## <a name="connecting-to-hdinsight"></a>HDInsight'a bağlanma

HDInsight'taki çoğu belge, kümeye internet üzerinden erişebildiğinizi varsayar. Örneğin, `https://CLUSTERNAME.azurehdinsight.net` konumundaki kümeye bağlanabildiğiniz kabul edilir. Bu adres, Internet erişimini kısıtlamak için NSG'leri veya ÜD'leri kullandıysanız kullanılamayan ortak ağ geçidini kullanır.

Bazı belgeler, `headnodehost` bir SSH oturumundan kümeye bağlanırken de başvurur. Bu adres yalnızca küme içindeki düğümlerden kullanılabilir ve sanal ağ üzerinden bağlanan istemcilerde kullanılamaz.

Sanal ağ üzerinden HDInsight'a doğrudan bağlanmak için aşağıdaki adımları kullanın:

1. HDInsight küme düğümlerinin dahili tam nitelikli alan adlarını keşfetmek için aşağıdaki yöntemlerden birini kullanın:

    ```powershell
    $resourceGroupName = "The resource group that contains the virtual network used with HDInsight"

    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

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
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

2. Bir hizmetin kullanılabildiği bağlantı noktasını belirlemek için, [HDInsight belgesinde Apache Hadoop hizmetlerinin kullandığı Bağlantı Noktaları'na](./hdinsight-hadoop-port-settings-for-services.md) bakın.

    > [!IMPORTANT]  
    > Baş düğümlerinde barındırılan bazı hizmetler aynı anda yalnızca bir düğümüzerinde etkindir. Bir baş düğümünde bir hizmete erişmeye çalışırsanız ve başarısız olursa, diğer baş düğümüne geçin.
    >
    > Örneğin, Apache Ambari bir seferde yalnızca bir baş düğümü üzerinde etkindir. Bir baş düğümünde Ambari'ye erişmeye çalışırsanız ve 404 hata döndürürse, diğer baş düğümünde çalışır.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal ağda HDInsight kullanma hakkında daha fazla bilgi için [Azure HDInsight kümeleri için sanal ağ dağıtımı planı'na](./hdinsight-plan-virtual-network-deployment.md)bakın.

* Azure sanal ağları hakkında daha fazla bilgi için [Azure Sanal Ağı'na genel bakış](../virtual-network/virtual-networks-overview.md)bilgisine bakın.

* Ağ güvenlik grupları hakkında daha fazla bilgi için [Ağ güvenlik gruplarına](../virtual-network/security-overview.md)bakın.

* Kullanıcı tanımlı rotalar hakkında daha fazla bilgi [için, Kullanıcı tanımlı rotalar ve IP yönlendirme](../virtual-network/virtual-networks-udr-overview.md)bakın.
