---
title: Azure HDInsight kümeleri için sanal ağlar oluşturma
description: HDInsight'ı diğer bulut kaynaklarına veya veri merkezinizdeki kaynaklara bağlamak için bir Azure Sanal Ağı'nı nasıl oluşturabilirsiniz öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/16/2020
ms.openlocfilehash: 0c7791d43ffbbc13ab151362c5c3026ebbdb0d34
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81531025"
---
# <a name="create-virtual-networks-for-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri için sanal ağlar oluşturma

Bu makalede, [Azure Sanal Ağları](../virtual-network/virtual-networks-overview.md)oluşturmak ve yapılandırmak için örnekler ve kod örnekleri verilmektedir. Azure HDInsight kümelerini kullanmak için. Ağ güvenlik grupları (NSGs) oluşturma ve DNS yapılandırma ayrıntılı örnekler sunulmaktadır.

Azure HDInsight ile sanal ağları kullanma hakkında arka plan bilgileri [için Azure HDInsight için sanal ağ planı'na](hdinsight-plan-virtual-network-deployment.md)bakın.

## <a name="prerequisites-for-code-samples-and-examples"></a>Kod örnekleri ve örnekler için ön koşullar

Bu makaledeki kod örneklerinden herhangi birini yürütmeden önce, TCP/IP ağ larını anlayın. TCP/IP ağlarını bilmiyorsanız, üretim ağlarında değişiklik yapmadan önce birisine danışın.

Bu makaledeki örnekler için diğer ön koşullar şunlardır:

* PowerShell kullanıyorsanız, AZ Modül'ünyüklenmesi [AZ Module](https://docs.microsoft.com/powershell/azure/overview)gerekir.
* Azure CLI'yi kullanmak istiyorsanız ve henüz yüklemediyseniz, Bkz. [Azure CLI'yi yükle.](https://docs.microsoft.com/cli/azure/install-azure-cli)

> [!IMPORTANT]  
> Bir Azure Sanal Ağı kullanarak HDInsight'ı şirket içi ağınıza bağlama konusunda adım adım kılavuz arıyorsanız, [hdinsight'ı şirket içi ağ belgenize bağlayın'a](connect-on-premises-network.md) bakın.

## <a name="example-network-security-groups-with-hdinsight"></a><a id="hdinsight-nsg"></a>Örnek: HDInsight ile ağ güvenlik grupları

Bu bölümdeki örnekler, ağ güvenliği grubu kurallarının nasıl oluşturulacak olduğunu göstermektedir. Kurallar, HDInsight'ın Azure yönetim hizmetleriyle iletişim kurmasına olanak tanır. Örnekleri kullanmadan önce IP adreslerini, kullanmakta olduğunuz Azure bölgesiyle eşleşecek şekilde ayarlayın. Bu bilgileri [HDInsight yönetimi IP adreslerinde](hdinsight-management-ip-addresses.md)bulabilirsiniz.

### <a name="azure-resource-management-template"></a>Azure Kaynak Yönetimi şablonu

Aşağıdaki Kaynak Yönetimi şablonu, gelen trafiği kısıtlayan, ancak HDInsight tarafından gerekli IP adreslerinden gelen trafiğine izin veren sanal bir ağ oluşturur. Bu şablon aynı zamanda sanal ağda bir HDInsight kümesi oluşturur.

* [Güvenli bir Azure Sanal Ağı ve HDInsight Hadoop kümesini dağıtma](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

### <a name="azure-powershell"></a>Azure PowerShell

Gelen trafiği kısıtlayan ve Kuzey Avrupa bölgesi için IP adreslerinden gelen trafiğe izin veren sanal bir ağ oluşturmak için aşağıdaki PowerShell komut dosyasını kullanın.

> [!IMPORTANT]  
> Ip adreslerini `hdirule1` ve `hdirule2` bu örnekte kullanmakta olduğunuz Azure bölgesiyle eşleşecek şekilde değiştirin. Bu bilgileri [HDInsight yönetimi IP adreslerini](hdinsight-management-ip-addresses.md)bulabilirsiniz.

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with the resource group the virtual network is in"
$subnetName = "Replace with the name of the subnet that you plan to use for HDInsight"

# Get the Virtual Network object
$vnet = Get-AzVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName

# Get the region the Virtual network is in.
$location = $vnet.Location

# Get the subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName

# Create a Network Security Group.
# And add exemptions for the HDInsight health and management services.
$nsg = New-AzNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule3" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule4" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule5" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzNetworkSecurityRuleConfig `
        -Name "hdirule6" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `

# Set the changes to the security group
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $nsg

# Apply the NSG to the subnet
Set-AzVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
$vnet | Set-AzVirtualNetwork
```

Bu örnek, gerekli IP adreslerinde gelen trafiğe izin vermek için kuralların nasıl ekleyeceğini gösterir. Diğer kaynaklardan gelen erişimi kısıtlamak için bir kural içermez. Aşağıdaki kod, Internet'ten SSH erişiminin nasıl etkinleştirilen gösteriş olduğunu gösterir:

```powershell
Get-AzNetworkSecurityGroup -Name hdisecure -ResourceGroupName RESOURCEGROUP |
Add-AzNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
```

### <a name="azure-cli"></a>Azure CLI

Gelen trafiği kısıtlayan, ancak HDInsight tarafından gerekli IP adreslerinden gelen trafiğe izin veren bir sanal ağ oluşturmak için aşağıdaki adımları kullanın.

1. Adlı `hdisecure`yeni bir ağ güvenlik grubu oluşturmak için aşağıdaki komutu kullanın. Azure `RESOURCEGROUP` Sanal Ağı'nı içeren kaynak grubuyla değiştirin. Grubun `LOCATION` oluşturulduğu konumla (bölge) değiştirin.

    ```azurecli
    az network nsg create -g RESOURCEGROUP -n hdisecure -l LOCATION
    ```

    Grup oluşturulduktan sonra, yeni grup hakkında bilgi alırsınız.

2. Azure HDInsight sistem durumu ve yönetim hizmetinden gelen bağlantı noktası 443'te gelen iletişime izin veren yeni ağ güvenliği grubuna kurallar eklemek için aşağıdakileri kullanın. Azure `RESOURCEGROUP` Sanal Ağı'nı içeren kaynak grubunun adıyla değiştirin.

    > [!IMPORTANT]  
    > Ip adreslerini `hdirule1` ve `hdirule2` bu örnekte kullanmakta olduğunuz Azure bölgesiyle eşleşecek şekilde değiştirin. Bu bilgileri [HDInsight yönetimi IP adreslerinde](hdinsight-management-ip-addresses.md)bulabilirsiniz.

    ```azurecli
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule3 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule4 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n hdirule6 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    ```

3. Bu ağ güvenlik grubu için benzersiz tanımlayıcıyı almak için aşağıdaki komutu kullanın:

    ```azurecli
    az network nsg show -g RESOURCEGROUP -n hdisecure --query "id"
    ```

    Bu komut, aşağıdaki metne benzer bir değer döndürür:

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

4. Ağ güvenlik grubunu bir alt ağa uygulamak için aşağıdaki komutu kullanın. `GUID` Ve `RESOURCEGROUP` değerleri önceki adımdan döndürülenlerle değiştirin. Oluşturmak `VNETNAME` `SUBNETNAME` istediğiniz sanal ağ adı ve alt ağ adı ile değiştirin.

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUP --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUP/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    Bu komut tamamlandıktan sonra, SANAL Ağ'a HDInsight yükleyebilirsiniz.

Bu adımlar yalnızca Azure bulutundaki HDInsight sağlık ve yönetim hizmetine erişimi açar. Sanal Ağ dışından HDInsight kümesine diğer erişim engellenir. Sanal ağ dışından erişimi etkinleştirmek için ek Ağ Güvenlik Grubu kuralları eklemeniz gerekir.

Aşağıdaki kod, Internet'ten SSH erişiminin nasıl etkinleştirilen gösteriş olduğunu gösterir:

```azurecli
az network nsg rule create -g RESOURCEGROUP --nsg-name hdisecure -n ssh --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
```

## <a name="example-dns-configuration"></a><a id="example-dns"></a>Örnek: DNS yapılandırması

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a>Sanal ağ ve bağlı şirket içi ağ arasında ad çözümlemesi

Bu örnekte aşağıdaki varsayımlar yapar:

* VPN ağ geçidi kullanarak şirket içi ağa bağlı bir Azure Sanal Ağınız vardır.

* Sanal ağdaki özel DNS sunucusu, işletim sistemi olarak Linux veya Unix çalıştırıyor.

* [Bind](https://www.isc.org/downloads/bind/) özel DNS sunucusuna yüklenir.

Sanal ağdaki özel DNS sunucusunda:

1. Sanal ağın DNS sonek ekini bulmak için Azure PowerShell veya Azure CLI'yi kullanın:

    Sanal `RESOURCEGROUP` ağı içeren kaynak grubunun adı ile değiştirin ve ardından komutu girin:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

1. Sanal ağ için özel DNS sunucusunda, `/etc/bind/named.conf.local` dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` Değeri sanal ağınızın DNS sonekiyle değiştirin.

    Bu yapılandırma, sanal ağın DNS sonekindeki tüm DNS isteklerini Azure özyinelemeli çözümleyiciye yönlendirir.

1. Sanal ağ için özel DNS sunucusunda, `/etc/bind/named.conf.options` dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```
    // Clients to accept requests from
    // TODO: Add the IP range of the joined network to this list
    acl goodclients {
        10.0.0.0/16; # IP address range of the virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent to the following
            forwarders {
                192.168.0.1; # Replace with the IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```
    
    * `10.0.0.0/16` Değeri sanal ağınızın IP adresi aralığıyla değiştirin. Bu giriş, bu aralıktaki ad çözümleme istekleri adreslerine izin verir.

    * Şirket içi ağın IP adresi aralığını `acl goodclients { ... }` bölüme ekleyin.  giriş, şirket içi ağdaki kaynaklardan gelen ad çözümleme isteklerine izin verir.
    
    * Değeri `192.168.0.1` şirket içi DNS sunucunuzun IP adresiyle değiştirin. Bu giriş, diğer tüm DNS isteklerini şirket içi DNS sunucusuna yönlendirir.

1. Yapılandırmayı kullanmak için Bind'i yeniden başlatın. Örneğin, `sudo service bind9 restart`.

1. Şirket içi DNS sunucusuna koşullu bir iletme ekleyin. DNS soneki için istekleri adım 1'den özel DNS sunucusuna gönderecek koşullu iletmeyi yapılandırın.

    > [!NOTE]  
    > Koşullu bir iletme nin nasıl ekleniş yaptığına ilişkin ayrıntılar için DNS yazılımınızın belgelerine başvurun.

Bu adımları tamamladıktan sonra, tam nitelikli alan adları (FQDN) kullanarak her iki ağdaki kaynaklara bağlanabilirsiniz. Artık HDInsight'ı sanal ağa yükleyebilirsiniz.

### <a name="name-resolution-between-two-connected-virtual-networks"></a>Bağlı iki sanal ağ arasında ad çözümlemesi

Bu örnekte aşağıdaki varsayımlar yapar:

* VPN ağ geçidi veya bakanlık kullanarak birbirine bağlı iki Azure Sanal Ağınız vardır.

* Her iki ağdaki özel DNS sunucusu işletim sistemi olarak Linux veya Unix çalıştırıyor.

* [Bind](https://www.isc.org/downloads/bind/) özel DNS sunucularında yüklenir.

1. Her iki sanal anın DNS sonekini bulmak için Azure PowerShell veya Azure CLI'yi kullanın:

    Sanal `RESOURCEGROUP` ağı içeren kaynak grubunun adı ile değiştirin ve ardından komutu girin:

    ```powershell
    $NICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP"
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. Özel DNS sunucusunda `/etc/bind/named.config.local` dosyanın içeriği olarak aşağıdaki metni kullanın. Bu değişikliği her iki sanal ağdaki özel DNS sunucusunda yapın.

    ```
    // Forward requests for the virtual network suffix to Azure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # The IP address of the DNS server in the other virtual network
    };
    ```

    `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` Değeri __diğer__ sanal ağın DNS sonekiyle değiştirin. Bu giriş, uzak ağın DNS sonekindeki istekleri ni o ağdaki özel DNS'ye yönlendirir.

3. Her iki sanal ağdaki özel DNS `/etc/bind/named.conf.options` sunucularında, dosyanın içeriği olarak aşağıdaki metni kullanın:

    ```
    // Clients to accept requests from
    acl goodclients {
        10.1.0.0/16; # The IP address range of one virtual network
        10.0.0.0/16; # The IP address range of the other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform to RFC1035
            listen-on { any; };
    };
    ```

   `10.0.0.0/16` Sanal ağlarınızın IP adresi aralıklarıyla ve `10.1.0.0/16` değerleri değiştirin. Bu giriş, her ağdaki kaynakların DNS sunucularından istekte bulunmasına olanak tanır.

    Sanal ağların DNS sonekleri için olmayan tüm istekler (örneğin, microsoft.com) Azure özyinelemeli çözümleyici tarafından işlenir.

4. Yapılandırmayı kullanmak için Bind'i yeniden başlatın. Örneğin, `sudo service bind9 restart` her iki DNS sunucusunda.

Bu adımları tamamladıktan sonra, tam nitelikli alan adlarını (FQDN) kullanarak sanal ağdaki kaynaklara bağlanabilirsiniz. Artık HDInsight'ı sanal ağa yükleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* HDInsight'ı şirket içi bir ağa bağlanmak üzere yapılandırmanın tam bir örneği için, [bkz.](./connect-on-premises-network.md)
* Azure sanal ağlarda Apache HBase kümelerini yapılandırmak için Azure [Sanal Ağ'da HDInsight'ta Apache HBase kümeleri oluştur'a](hbase/apache-hbase-provision-vnet.md)bakın.
* Apache HBase coğrafi çoğaltmayı yapılandırmak için [bkz.](hbase/apache-hbase-replication.md)
* Azure sanal ağları hakkında daha fazla bilgi için [Azure Sanal Ağı'na genel bakış](../virtual-network/virtual-networks-overview.md)bilgisine bakın.

* Ağ güvenlik grupları hakkında daha fazla bilgi için [Ağ güvenlik gruplarına](../virtual-network/security-overview.md)bakın.

* Kullanıcı tanımlı rotalar hakkında daha fazla bilgi [için, Kullanıcı tanımlı rotalar ve IP yönlendirme](../virtual-network/virtual-networks-udr-overview.md)bakın.
