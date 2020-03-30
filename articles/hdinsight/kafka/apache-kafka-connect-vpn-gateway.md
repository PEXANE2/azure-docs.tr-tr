---
title: Sanal ağları kullanarak Kafka'ya bağlanın - Azure HDInsight
description: Azure Sanal Ağı üzerinden HDInsight'ta Kafka'ya doğrudan nasıl bağlanıştınız öğrenin. Vpn ağ geçidi kullanarak geliştirme istemcilerinden veya şirket içi ağınızdaki istemcilerden bir VPN ağ geçidi aygıtı kullanarak Kafka'ya nasıl bağlanıp bağlanılamayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/04/2020
ms.openlocfilehash: 36ff0d5f1fc96b2013555d37a869ebf629a22be7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272129"
---
# <a name="connect-to-apache-kafka-on-hdinsight-through-an-azure-virtual-network"></a>Azure Sanal Ağı üzerinden HDInsight üzerinde Apache Kafka'ya bağlanma

Azure Sanal Ağı üzerinden HDInsight'ta Apache Kafka'ya doğrudan nasıl bağlanacağınız hakkında bilgi edinin. Bu belge, aşağıdaki yapılandırmaları kullanarak Kafka'ya bağlanma hakkında bilgi sağlar:

* Şirket içi ağdaki kaynaklardan. Bu bağlantı, yerel abunuzdaki bir VPN aygıtı (yazılım veya donanım) kullanılarak kurulur.
* VPN yazılım istemcisi kullanarak geliştirme ortamından.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="architecture-and-planning"></a>Mimarlık ve planlama

HDInsight, Kafka'ya halka açık internet üzerinden doğrudan bağlantı ya da Bunun yerine, Kafka müşterileri (üreticiler ve tüketiciler) aşağıdaki bağlantı yöntemlerinden birini kullanmalıdır:

* Istemciyi HDInsight'ta Kafka ile aynı sanal ağda çalıştırın. Bu yapılandırma HDInsight belgesinde [Apache Kafka ile Başlat'da](apache-kafka-get-started.md) kullanılır. İstemci doğrudan HDInsight küme düğümlerinde veya aynı ağdaki başka bir sanal makinede çalışır.

* Şirket içi ağınız gibi özel bir ağı sanal ağa bağlayın. Bu yapılandırma, şirket içi ağınızdaki istemcilerin Kafka ile doğrudan çalışmasını sağlar. Bu yapılandırmayı etkinleştirmek için aşağıdaki görevleri gerçekleştirin:

  1. Sanal ağ oluşturun.
  2. Siteden siteye yapılandırma kullanan bir VPN ağ geçidi oluşturun. Bu belgede kullanılan yapılandırma, şirket içi ağınızdaki bir VPN ağ geçidi aygıtına bağlanır.
  3. Sanal ağda bir DNS sunucusu oluşturun.
  4. Her ağdaki DNS sunucusu arasında iletme yapılandırma.
  5. Sanal ağda HDInsight kümesinde bir Kafka oluşturun.

     Daha fazla bilgi için [şirket içi ağ bölümünden Apache Kafka'ya Bağlan](#on-premises) bölümüne bakın.

* VPN ağ geçidi ve VPN istemcisi kullanarak tek tek makineleri sanal ağa bağlayın. Bu yapılandırmayı etkinleştirmek için aşağıdaki görevleri gerçekleştirin:

  1. Sanal ağ oluşturun.
  2. Noktadan siteye yapılandırma kullanan bir VPN ağ geçidi oluşturun. Bu yapılandırma hem Windows hem de MacOS istemcileri ile kullanılabilir.
  3. Sanal ağda HDInsight kümesinde bir Kafka oluşturun.
  4. Kafka'yı IP reklamları için yapılandırın. Bu yapılandırma, istemcinin etki alanı adları yerine aracı IP adreslerini kullanarak bağlanmasını sağlar.
  5. Geliştirme sisteminde VPN istemcisini indirin ve kullanın.

     Daha fazla bilgi için [VPN istemcisi bölümüyle Apache Kafka'ya Bağlan](#vpnclient) bölümüne bakın.

     > [!WARNING]  
     > Bu yapılandırma yalnızca aşağıdaki sınırlamalar nedeniyle geliştirme amacıyla önerilir:
     >
     > * Her istemci bir VPN yazılım istemcisi kullanarak bağlanmak gerekir.
     > * VPN istemcisi sanal ağa ad çözümleme isteklerini iletmez, bu nedenle Kafka ile iletişim kurmak için IP adresleme kullanmanız gerekir. IP iletişimi Kafka kümesi üzerinde ek yapılandırma gerektirir.

Sanal ağda HDInsight kullanma hakkında daha fazla bilgi için [Azure HDInsight kümeleri için sanal ağ planı'na](../hdinsight-plan-virtual-network-deployment.md)bakın.

## <a name="connect-to-apache-kafka-from-an-on-premises-network"></a><a id="on-premises"></a>Apache Kafka'ya şirket içi bir ağdan bağlanın

Şirket içi ağınızla iletişim kuran bir Kafka kümesi oluşturmak [için, şirket içi ağ belgenize HDInsight'ı bağlayın](./../connect-on-premises-network.md) adımlarını izleyin.

> [!IMPORTANT]  
> HDInsight kümesini oluştururken __Kafka__ küme türünü seçin.

Bu adımlar aşağıdaki yapılandırmayı oluşturur:

* Azure Sanal Ağ
* Siteden siteye VPN ağ geçidi
* Azure Depolama hesabı (HDInsight tarafından kullanılır)
* HDInsight üzerinde Kafka

Kafka istemcisinin kümeye şirket içinde bağlanabiliyor doğrulamak için [Örnek: Python istemcisi](#python-client) bölümündeki adımları kullanın.

## <a name="connect-to-apache-kafka-with-a-vpn-client"></a><a id="vpnclient"></a>VPN istemcisi ile Apache Kafka'ya bağlanın

Aşağıdaki yapılandırmayı oluşturmak için bu bölümdeki adımları kullanın:

* Azure Sanal Ağ
* Noktadan siteye VPN ağ geçidi
* Azure Depolama Hesabı (HDInsight tarafından kullanılır)
* HDInsight üzerinde Kafka

1. Siteye nokta lı bağlantılar belgesi [için kendi imzalı sertifikalarla Çalışma'daki](../../vpn-gateway/vpn-gateway-certificates-point-to-site.md) adımları izleyin. Bu belge ağ geçidi için gereken sertifikaları oluşturur.

2. Bir PowerShell istemi açın ve Azure aboneliğinizde oturum açmak için aşağıdaki kodu kullanın:

    ```powershell
    Connect-AzAccount
    # If you have multiple subscriptions, uncomment to set the subscription
    #Select-AzSubscription -SubscriptionName "name of your subscription"
    ```

3. Yapılandırma bilgileri içeren değişkenler oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Prompt for generic information
    $resourceGroupName = Read-Host "What is the resource group name?"
    $baseName = Read-Host "What is the base name? It is used to create names for resources, such as 'net-basename' and 'kafka-basename':"
    $location = Read-Host "What Azure Region do you want to create the resources in?"
    $rootCert = Read-Host "What is the file path to the root certificate? It is used to secure the VPN gateway."

    # Prompt for HDInsight credentials
    $adminCreds = Get-Credential -Message "Enter the HTTPS user name and password for the HDInsight cluster" -UserName "admin"
    $sshCreds = Get-Credential -Message "Enter the SSH user name and password for the HDInsight cluster" -UserName "sshuser"

    # Names for Azure resources
    $networkName = "net-$baseName"
    $clusterName = "kafka-$baseName"
    $storageName = "store$baseName" # Can't use dashes in storage names
    $defaultContainerName = $clusterName
    $defaultSubnetName = "default"
    $gatewaySubnetName = "GatewaySubnet"
    $gatewayPublicIpName = "GatewayIp"
    $gatewayIpConfigName = "GatewayConfig"
    $vpnRootCertName = "rootcert"
    $vpnName = "VPNGateway"

    # Network settings
    $networkAddressPrefix = "10.0.0.0/16"
    $defaultSubnetPrefix = "10.0.0.0/24"
    $gatewaySubnetPrefix = "10.0.1.0/24"
    $vpnClientAddressPool = "172.16.201.0/24"

    # HDInsight settings
    $HdiWorkerNodes = 4
    $hdiVersion = "3.6"
    $hdiType = "Kafka"
    ```

4. Azure kaynak grubunu ve sanal ağı oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the resource group that contains everything
    New-AzResourceGroup -Name $resourceGroupName -Location $location

    # Create the subnet configuration
    $defaultSubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -AddressPrefix $defaultSubnetPrefix
    $gatewaySubnetConfig = New-AzVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -AddressPrefix $gatewaySubnetPrefix

    # Create the subnet
    New-AzVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AddressPrefix $networkAddressPrefix `
        -Subnet $defaultSubnetConfig, $gatewaySubnetConfig

    # Get the network & subnet that were created
    $network = Get-AzVirtualNetwork -Name $networkName `
        -ResourceGroupName $resourceGroupName
    $gatewaySubnet = Get-AzVirtualNetworkSubnetConfig -Name $gatewaySubnetName `
        -VirtualNetwork $network
    $defaultSubnet = Get-AzVirtualNetworkSubnetConfig -Name $defaultSubnetName `
        -VirtualNetwork $network

    # Set a dynamic public IP address for the gateway subnet
    $gatewayPublicIp = New-AzPublicIpAddress -Name $gatewayPublicIpName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -AllocationMethod Dynamic
    $gatewayIpConfig = New-AzVirtualNetworkGatewayIpConfig -Name $gatewayIpConfigName `
        -Subnet $gatewaySubnet `
        -PublicIpAddress $gatewayPublicIp

    # Get the certificate info
    # Get the full path in case a relative path was passed
    $rootCertFile = Get-ChildItem $rootCert
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($rootCertFile)
    $certBase64 = [System.Convert]::ToBase64String($cert.RawData)
    $p2sRootCert = New-AzVpnClientRootCertificate -Name $vpnRootCertName `
        -PublicCertData $certBase64

    # Create the VPN gateway
    New-AzVirtualNetworkGateway -Name $vpnName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -IpConfigurations $gatewayIpConfig `
        -GatewayType Vpn `
        -VpnType RouteBased `
        -EnableBgp $false `
        -GatewaySku Standard `
        -VpnClientAddressPool $vpnClientAddressPool `
        -VpnClientRootCertificates $p2sRootCert
    ```

    > [!WARNING]  
    > Bu işlemin tamamlanması birkaç dakika sürebilir.

5. Azure Depolama Hesabı ve blob kapsayıcısını oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the storage account
    New-AzStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $storageName `
        -SkuName Standard_GRS `
        -Location $location `
        -Kind StorageV2 `
        -EnableHttpsTrafficOnly 1

    # Get the storage account keys and create a context
    $defaultStorageKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName `
        -Name $storageName)[0].Value
    $storageContext = New-AzStorageContext -StorageAccountName $storageName `
        -StorageAccountKey $defaultStorageKey

    # Create the default storage container
    New-AzStorageContainer -Name $defaultContainerName `
        -Context $storageContext
    ```

6. HDInsight kümesini oluşturmak için aşağıdaki kodu kullanın:

    ```powershell
    # Create the HDInsight cluster
    New-AzHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $hdiWorkerNodes `
        -ClusterType $hdiType `
        -OSType Linux `
        -Version $hdiVersion `
        -HttpCredential $adminCreds `
        -SshCredential $sshCreds `
        -DefaultStorageAccountName "$storageName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageKey `
        -DefaultStorageContainer $defaultContainerName `
        -DisksPerWorkerNode 2 `
        -VirtualNetworkId $network.Id `
        -SubnetName $defaultSubnet.Id
    ```

   > [!WARNING]  
   > Bu işlemin tamamlanması yaklaşık 15 dakika sürer.

### <a name="configure-kafka-for-ip-advertising"></a>IP reklamları için Kafka'yı yapılandırın

Varsayılan olarak, Apache Zookeeper Kafka brokerlarının etki alanı adını müşterilere iade eder. Sanal ağdaki varlıklar için ad çözümlemesi kullanamadığı için bu yapılandırma VPN yazılım istemcisiyle çalışmaz. Bu yapılandırma için, Kafka'yı etki alanı adları yerine IP adreslerinin reklamını yapacak şekilde yapılandırmak için aşağıdaki adımları kullanın:

1. Web tarayıcısını kullanarak `https://CLUSTERNAME.azurehdinsight.net` adresine gidin. HDInsight kümesinde Kafka adı ile değiştirin. `CLUSTERNAME`

    İstendiğinde, küme için HTTPS kullanıcı adı ve parolasını kullanın. Kümeiçin Ambari Web UI görüntülenir.

2. Kafka hakkındaki bilgileri görüntülemek için soldaki listeden __Kafka'yı__ seçin.

    ![Kafka ile servis listesi vurgulandı](./media/apache-kafka-connect-vpn-gateway/select-kafka-service.png)

3. Kafka yapılandırmasını görüntülemek için, üst ten __Configs'i__ seçin.

    ![Apache Ambari hizmetleri yapılandırması](./media/apache-kafka-connect-vpn-gateway/select-kafka-config1.png)

4. __Kafka-env__ yapılandırmasını bulmak `kafka-env` için sağ üstteki __Filtre__ alanına girin.

    ![Kafka konfigürasyonu, kafka-env için](./media/apache-kafka-connect-vpn-gateway/search-for-kafka-env.png)

5. Kafka'yı IP adreslerinin reklamını yapacak şekilde yapılandırmak için __kafka-env şablonu__ alanının altına aşağıdaki metni ekleyin:

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

6. Kafka'nın dinlediği arayüzü yapılandırmak `listeners` için sağ üstteki __Filtre__ alanına girin.

7. Kafka'yı tüm ağ arabirimlerinde dinleyecek şekilde __listeners__ yapılandırmak `PLAINTEXT://0.0.0.0:9092`için dinleyici ler alanındaki değeri .

8. Yapılandırma değişikliklerini kaydetmek için __Kaydet__ düğmesini kullanın. Değişiklikleri açıklayan bir kısa mesaj girin. Değişiklikler kaydedildikten sonra __Tamam'ı__ seçin.

    ![Apache Ambari yapılandırmayı kaydedin](./media/apache-kafka-connect-vpn-gateway/save-configuration-button.png)

9. Kafka'yı yeniden başlatırken hataları önlemek __için, Servis Eylemleri__ düğmesini kullanın ve __Bakım Modunu Aç'ı__seçin. Bu işlemi tamamlamak için Tamam'ı seçin.

    ![Servis eylemleri, bakım açma özelliği vurgulanır](./media/apache-kafka-connect-vpn-gateway/turn-on-maintenance-mode.png)

10. Kafka'yı yeniden başlatmak için __Yeniden Başlat__ düğmesini kullanın ve __Etkilenen Tümünü Yeniden Başlat'ı__seçin. Yeniden başlat'ı onaylayın ve işlem tamamlandıktan sonra __TAMAM__ düğmesini kullanın.

    ![Etkilenen tüm vurgulanan yeniden başlat düğmesi ile yeniden başlat](./media/apache-kafka-connect-vpn-gateway/restart-required-button.png)

11. Bakım modunu devre dışı kalmak için __Servis Eylemleri__ düğmesini kullanın ve __Bakım Modunu Kapat'ı__seçin. Bu işlemi tamamlamak için **Tamam'ı** seçin.

### <a name="connect-to-the-vpn-gateway"></a>VPN ağ geçidine bağlanın

VPN ağ geçidine bağlanmak için, [Bir Noktadan Noktaya Bağlantı Belgesini Yapılandır'ın](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md#connect) __Azure'a Bağlan__ bölümünü kullanın.

## <a name="example-python-client"></a><a id="python-client"></a>Örnek: Python istemcisi

Kafka'ya bağlantıyı doğrulamak için, bir Python üreticisi ve tüketicisi oluşturmak ve çalıştırmak için aşağıdaki adımları kullanın:

1. Kafka kümesindeki düğümlerin tam nitelikli etki alanı adını (FQDN) ve IP adreslerini almak için aşağıdaki yöntemlerden birini kullanın:

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

    Bu komut dosyası, sanal ağı içeren Azure kaynak grubunun adı olduğunu `$resourceGroupName` varsayar.

    Döndürülen bilgileri sonraki adımlarda kullanmak üzere kaydedin.

2. [Kafka-python](https://kafka-python.readthedocs.io/) istemcisini yüklemek için aşağıdakileri kullanın:

    ```bash
    pip install kafka-python
    ```

3. Kafka'ya veri göndermek için aşağıdaki Python kodunu kullanın:

   ```python
   from kafka import KafkaProducer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # NOTE: you don't need the full list of worker nodes, just one or two.
   producer = KafkaProducer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'])
   for _ in range(50):
      producer.send('testtopic', b'test message')
   ```

    `'kafka_broker'` Girişleri, bu bölümdeki 1.

   * __Bir Yazılım VPN istemcisi__kullanıyorsanız, `kafka_broker` girişleri işçi düğümlerinizin IP adresiyle değiştirin.

   * __Özel bir DNS sunucusu aracılığıyla ad çözümlemesi sağladıysanız,__ `kafka_broker` girişleri alt düğümlerin FQDN'si ile değiştirin.

     > [!NOTE]
     > Bu kod dizeyi `test message` `testtopic`konuya gönderir. HDInsight'ta Kafka'nın varsayılan yapılandırması, yoksa konuyu oluşturmaktır.

4. Kafka'dan iletileri almak için aşağıdaki Python kodunu kullanın:

   ```python
   from kafka import KafkaConsumer
   # Replace the `ip_address` entries with the IP address of your worker nodes
   # Again, you only need one or two, not the full list.
   # Note: auto_offset_reset='earliest' resets the starting offset to the beginning
   #       of the topic
   consumer = KafkaConsumer(bootstrap_servers=['kafka_broker_1','kafka_broker_2'],auto_offset_reset='earliest')
   consumer.subscribe(['testtopic'])
   for msg in consumer:
     print (msg)
   ```

    `'kafka_broker'` Girişleri, bu bölümdeki 1.

    * __Bir Yazılım VPN istemcisi__kullanıyorsanız, `kafka_broker` girişleri işçi düğümlerinizin IP adresiyle değiştirin.

    * __Özel bir DNS sunucusu aracılığıyla ad çözümlemesi sağladıysanız,__ `kafka_broker` girişleri alt düğümlerin FQDN'si ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

SANAL ağda HDInsight kullanma hakkında daha fazla bilgi için [Azure HDInsight kümeleri](../hdinsight-plan-virtual-network-deployment.md) belgesi için sanal ağ dağıtımı planı'na bakın.

Siteye Uygun VPN ağ geçidine sahip bir Azure Sanal Ağı oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [Azure portalını kullanarak Bir Noktadan Siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)

* [Azure PowerShell'i kullanarak Bir Noktadan Siteye bağlantı yapılandırma](../../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

HDInsight'ta Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight'ta Apache Kafka ile başlayın](apache-kafka-get-started.md)
* [HDInsight'ta Apache Kafka ile yansıtmayı kullanma](apache-kafka-mirroring.md)
