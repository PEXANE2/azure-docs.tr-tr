---
title: Azure Dosya Eşitleme ağ uç noktalarını yapılandırma | Microsoft Docs
description: Azure Dosya Eşitleme için ağ seçeneklerine genel bakış.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 5/11/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 68d2b864b0e825756fbcd8e43fee3d6289c77c36
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85512851"
---
# <a name="configuring-azure-file-sync-network-endpoints"></a>Azure Dosya Eşitleme ağının uç noktalarını yapılandırma
Azure dosyaları ve Azure Dosya Eşitleme, Azure dosya paylaşımlarına erişmek için iki ana uç nokta türü sağlar: 
- Genel bir IP adresi olan ve dünyanın herhangi bir yerinden erişilebilen genel uç noktalar.
- Bir sanal ağ içinde bulunan ve bu sanal ağın adres alanından özel bir IP adresine sahip olan özel uç noktalar.

Hem Azure dosyaları hem de Azure Dosya Eşitleme, Azure Yönetim nesneleri, depolama hesabı ve depolama eşitleme hizmeti için, hem genel hem de özel uç noktaları kontrol edin. Depolama hesabı, birden çok dosya paylaşımını ve BLOB kapsayıcıları ya da kuyrukları gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzunu temsil eden bir yönetim yapısıdır. Depolama eşitleme hizmeti, Azure Dosya Eşitleme ile kurulan bir güven ilişkisine sahip Windows dosya sunucuları olan kayıtlı sunucuları temsil eden bir yönetim yapısıdır ve eşitleme ilişkisinin topolojisini tanımlayan eşitleme gruplarıdır. 

Bu makalede, her iki Azure dosyası ve Azure Dosya Eşitleme için ağ uç noktalarının nasıl yapılandırılacağı ele alınmaktadır. Şirket içinde Azure Dosya Eşitleme ile önbelleğe almak yerine doğrudan Azure dosya paylaşımlarına erişmek için ağ uç noktalarını yapılandırma hakkında daha fazla bilgi edinmek için bkz. [Azure dosyaları ağ uç noktalarını yapılandırma](storage-files-networking-endpoints.md).

Bu nasıl yapılır Kılavuzu ' nu okumadan önce [Azure dosya eşitleme ağ konularını](storage-sync-files-networking-overview.md) okumanızı öneririz.

## <a name="prerequisites"></a>Önkoşullar 
Bu makalede şu şekilde varsayılmaktadır:
- Azure aboneliğiniz var. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Şirket içinden bağlamak istediğiniz bir depolama hesabında zaten bir Azure dosya paylaşma oluşturmuş olabilirsiniz. Azure dosya paylaşımının nasıl oluşturulacağını öğrenmek için bkz. [Azure dosya paylaşma oluşturma](storage-how-to-create-file-share.md).
- Zaten bir depolama eşitleme hizmeti oluşturdunuz ve Windows dosya sunucunuzu bununla kaydettiniz. Azure Dosya Eşitleme dağıtmayı öğrenmek için bkz. [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md).

Ek olarak:
- Azure PowerShell kullanmayı düşünüyorsanız, [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Azure CLı 'yı kullanmayı planlıyorsanız [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-the-private-endpoints"></a>Özel uç noktaları oluşturma
Bir Azure kaynağı için özel bir uç nokta oluştururken aşağıdaki kaynaklar dağıtılır:

- **Özel uç nokta**: depolama hesabı ya da depolama eşitleme hizmeti için özel uç noktasını temsil eden bir Azure kaynağı. Bunu, Azure kaynağınızı ve bir ağ arabirimini bağlayan bir kaynak olarak düşünebilirsiniz.
- **Ağ arabirimi (NIC)**: belirtilen sanal ağ/alt ağ içinde özel bir IP adresi tutan ağ arabirimi. Bu, bir sanal makineyi dağıtırken dağıtılan aynı kaynaktır, ancak bir VM 'ye atanmak yerine özel uç noktaya aittir.
- **Özel BIR DNS bölgesi**: daha önce bu sanal ağ için özel bir uç nokta dağıtmadıysanız, sanal ağınız için yeni BIR özel DNS bölgesi dağıtılır. Bu DNS bölgesinde Azure kaynağı için bir DNS A kaydı da oluşturulacak. Bu sanal ağda zaten özel bir uç nokta dağıttıysanız, mevcut DNS bölgesine Azure kaynağı için yeni bir kayıt eklenecektir. DNS bölgesinin dağıtımı isteğe bağlıdır, ancak DNS yönetiminin gerekli olduğu basitleştirilmesi önerilir.

> [!Note]  
> Bu makalede, `core.windows.net` depolama hesapları ve depolama Eşitleme Hizmetleri Için Azure genel BÖLGELERININ DNS sonekleri kullanılmaktadır `afs.azure.net` . Bu yorum, Azure US kamu bulutu gibi Azure Sogeign bulutları için de geçerlidir; ortamınız için uygun son ekleri yerine koyun.

### <a name="create-the-storage-account-private-endpoint"></a>Depolama hesabı özel uç noktasını oluşturma
# <a name="portal"></a>[Portal](#tab/azure-portal)
[!INCLUDE [storage-files-networking-endpoints-private-portal](../../../includes/storage-files-networking-endpoints-private-portal.md)]

Sanal ağınızın içinde bir sanal makineniz varsa veya [Azure dosyaları IÇIN DNS Iletmeyi yapılandırma](storage-files-networking-dns.md)bölümünde AÇıKLANDıĞı gibi DNS iletmeyi yapılandırdıysanız, PowerShell, komut satırı veya terminalden (Windows, Linux veya MacOS için çalışır) aşağıdaki komutları çalıştırarak özel uç noktanızın doğru şekilde ayarlandığını test edebilirsiniz. `<storage-account-name>`Uygun depolama hesabı adıyla değiştirmeniz gerekir:

```
nslookup <storage-account-name>.file.core.windows.net
```

Her şey başarıyla çalıştıysa aşağıdaki çıktıyı görmeniz gerekir, burada `192.168.0.5` sanal ağınızdaki özel uç noktanın özel IP adresidir (Windows için gösterilen çıktı):

```Output
Server:  UnKnown
Address:  10.2.4.4

Non-authoritative answer:
Name:    storageaccount.privatelink.file.core.windows.net
Address:  192.168.0.5
Aliases:  storageaccount.file.core.windows.net
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!INCLUDE [storage-files-networking-endpoints-private-powershell](../../../includes/storage-files-networking-endpoints-private-powershell.md)]

Sanal ağınızın içinde bir sanal makineniz varsa veya [Azure dosyaları IÇIN DNS Iletmeyi yapılandırma](storage-files-networking-dns.md)bölümünde AÇıKLANDıĞı gibi DNS iletmeyi yapılandırdıysanız, Özel uç noktanızın aşağıdaki komutlarla doğru şekilde ayarlandığını test edebilirsiniz:

```PowerShell
$storageAccountHostName = [System.Uri]::new($storageAccount.PrimaryEndpoints.file) | `
    Select-Object -ExpandProperty Host

Resolve-DnsName -Name $storageAccountHostName
```

Her şey başarıyla çalıştıysa aşağıdaki çıktıyı görmeniz gerekir, burada `192.168.0.5` sanal ağınızdaki özel uç noktanın özel IP adresidir:

```Output
Name                             Type   TTL   Section    NameHost
----                             ----   ---   -------    --------
storageaccount.file.core.windows CNAME  60    Answer     storageaccount.privatelink.file.core.windows.net
.net

Name       : storageaccount.privatelink.file.core.windows.net
QueryType  : A
TTL        : 600
Section    : Answer
IP4Address : 192.168.0.5
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
[!INCLUDE [storage-files-networking-endpoints-private-cli](../../../includes/storage-files-networking-endpoints-private-cli.md)]

Sanal ağınızın içinde bir sanal makineniz varsa veya [Azure dosyaları IÇIN DNS Iletmeyi yapılandırma](storage-files-networking-dns.md)bölümünde AÇıKLANDıĞı gibi DNS iletmeyi yapılandırdıysanız, Özel uç noktanızın aşağıdaki komutlarla doğru şekilde ayarlandığını test edebilirsiniz:

```bash
httpEndpoint=$(az storage account show \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | \
    tr -d '"')

hostName=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint) | tr -d "/")
nslookup $hostName
```

Her şey başarıyla çalıştıysa aşağıdaki çıktıyı görmeniz gerekir, burada `192.168.0.5` sanal ağınızdaki özel uç noktanın özel IP adresidir:

```Output
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
storageaccount.file.core.windows.net      canonical name = storageaccount.privatelink.file.core.windows.net.
Name:   storageaccount.privatelink.file.core.windows.net
Address: 192.168.0.5
```

---

### <a name="create-the-storage-sync-private-endpoint"></a>Depolama eşitleme özel uç noktasını oluşturma
> [!Important]  
> Depolama eşitleme hizmeti kaynağında özel uç noktaları kullanmak için, Azure Dosya Eşitleme Aracısı sürüm 10,1 veya üstünü kullanmanız gerekir. 10,1 ' dan önceki Aracı sürümleri, depolama eşitleme hizmeti 'ndeki özel uç noktaları desteklemez. Tüm önceki Aracı sürümleri, depolama hesabı kaynağında özel uç noktaları destekler.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal en üstündeki arama çubuğuna *özel bağlantı* yazarak **özel bağlantı merkezine** gidin. Özel bağlantı Merkezi içindekiler tablosunda **Özel uç noktalar**' ı seçin ve ardından yeni bir özel uç nokta oluşturmak Için **+ Ekle** ' yi seçin.

[![Özel bağlantı merkezinin ekran görüntüsü](media/storage-sync-files-networking-endpoints/create-storage-sync-private-endpoint-0.png)](media/storage-sync-files-networking-endpoints/create-storage-sync-private-endpoint-0.png#lightbox)

Ortaya çıkan sihirbazda, tamamlanacak birden çok sayfa vardır.

**Temel bilgiler** dikey penceresinde, Özel uç noktanız için istenen kaynak grubunu, adı ve bölgeyi seçin. Bunlar istediğiniz gibi olabilir, ancak içinde özel uç nokta oluşturmak istediğiniz sanal ağ ile aynı bölgede özel uç nokta oluşturmanız gerektiği halde, depolama eşitleme hizmeti 'ni herhangi bir şekilde eşleştirmek zorunda kalmaz.

![Özel uç nokta oluştur bölümünün temel bilgiler bölümünün ekran görüntüsü](media/storage-sync-files-networking-endpoints/create-storage-sync-private-endpoint-1.png)

**Kaynak** dikey penceresinde, **Dizinimde bir Azure kaynağına bağlanmak**için radyo düğmesini seçin. **Kaynak**türü altında, kaynak türü için **Microsoft. storagessync/storageSyncServices** öğesini seçin. 

**Yapılandırma** dikey penceresi, Özel uç noktanızı eklemek istediğiniz belirli sanal ağı ve alt ağı seçmenize olanak sağlar. Yukarıdaki depolama hesabı için kullandığınız sanal ağı seçin. Yapılandırma dikey penceresi ayrıca özel DNS bölgesi oluşturma/güncelleştirme bilgilerini içerir.

Özel uç noktayı oluşturmak için **gözden geçir + oluştur** ' a tıklayın.

PowerShell 'den aşağıdaki komutları çalıştırarak özel uç noktanızın doğru şekilde ayarlandığından test edebilirsiniz. 

```powershell
$privateEndpointResourceGroupName = "<your-private-endpoint-resource-group>"
$privateEndpointName = "<your-private-endpoint-name>"

Get-AzPrivateEndpoint `
        -ResourceGroupName $privateEndpointResourceGroupName `
        -Name $privateEndpointName `
        -ErrorAction Stop | `
    Select-Object -ExpandProperty NetworkInterfaces | `
    Select-Object -ExpandProperty Id | `
    ForEach-Object { Get-AzNetworkInterface -ResourceId $_ } | `
    Select-Object -ExpandProperty IpConfigurations | `
    Select-Object -ExpandProperty PrivateLinkConnectionProperties | `
    Select-Object -ExpandProperty Fqdns | `
    ForEach-Object { Resolve-DnsName -Name $_ } | `
    Format-List
```

Her şey doğru şekilde çalıştıysa,,, `192.168.1.4` `192.168.1.5` `192.168.1.6` ve `192.168.1.7` Özel uç noktaya atanmış özel IP adreslerinden oluşan aşağıdaki çıktıyı görmeniz gerekir:

```Output
Name     : mysssmanagement.westus2.afs.azure.net
Type     : CNAME
TTL      : 60
Section  : Answer
NameHost : mysssmanagement.westus2.privatelink.afs.azure.net


Name       : mysssmanagement.westus2.privatelink.afs.azure.net
QueryType  : A
TTL        : 60
Section    : Answer
IP4Address : 192.168.1.4

Name     : myssssyncp.westus2.afs.azure.net
Type     : CNAME
TTL      : 60
Section  : Answer
NameHost : myssssyncp.westus2.privatelink.afs.azure.net


Name       : myssssyncp.westus2.privatelink.afs.azure.net
QueryType  : A
TTL        : 60
Section    : Answer
IP4Address : 192.168.1.5

Name     : myssssyncs.westus2.afs.azure.net
Type     : CNAME
TTL      : 60
Section  : Answer
NameHost : myssssyncs.westus2.privatelink.afs.azure.net


Name       : myssssyncs.westus2.privatelink.afs.azure.net
QueryType  : A
TTL        : 60
Section    : Answer
IP4Address : 192.168.1.6

Name     : mysssmonitoring.westus2.afs.azure.net
Type     : CNAME
TTL      : 60
Section  : Answer
NameHost : mysssmonitoring.westus2.privatelink.afs.azure.net


Name       : mysssmonitoring.westus2.privatelink.afs.azure.net
QueryType  : A
TTL        : 60
Section    : Answer
IP4Address : 192.168.1.7

```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Depolama eşitleme hizmetiniz için özel bir uç nokta oluşturmak için önce depolama eşitleme hizmetinize bir başvuru almanız gerekir. Öğesini `<storage-sync-service-resource-group>` ve `<storage-sync-service>` ortamınız için doğru değerleri değiştirmeyi unutmayın. Aşağıdaki PowerShell komutları, kullanmakta olduğunuz sanal ağ bilgilerini yukarıda zaten doldurmuş olduğunu varsayar. 

```powershell
$storageSyncServiceResourceGroupName = "<storage-sync-service-resource-group>"
$storageSyncServiceName = "<storage-sync-service>"

$storageSyncService = Get-AzStorageSyncService `
        -ResourceGroupName $storageSyncServiceResourceGroupName `
        -Name $storageSyncServiceName `
        -ErrorAction SilentlyContinue

if ($null -eq $storageSyncService) {
    $errorMessage = "Storage Sync Service $storageSyncServiceName not found "
    $errorMessage += "in resource group $storageSyncServiceResourceGroupName."
    Write-Error -Message $errorMessage -ErrorAction Stop
}
```

Özel bir uç nokta oluşturmak için, depolama eşitleme hizmeti 'ne özel bir bağlantı hizmeti bağlantısı oluşturmanız gerekir. Özel bağlantı bağlantısı, Özel uç noktanın oluşturulmasına yönelik bir giriştir.

```PowerShell 
# Disable private endpoint network policies
$subnet.PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork = $virtualNetwork | `
    Set-AzVirtualNetwork -ErrorAction Stop

# Create a private link service connection to the storage account.
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
        -Name "$storageSyncServiceName-Connection" `
        -PrivateLinkServiceId $storageSyncService.ResourceId `
        -GroupId "Afs" `
        -ErrorAction Stop

# Create a new private endpoint.
$privateEndpoint = New-AzPrivateEndpoint `
        -ResourceGroupName $storageSyncServiceResourceGroupName `
        -Name "$storageSyncServiceName-PrivateEndpoint" `
        -Location $virtualNetwork.Location `
        -Subnet $subnet `
        -PrivateLinkServiceConnection $privateEndpointConnection `
        -ErrorAction Stop
```

Azure özel DNS bölgesi oluşturmak, depolama eşitleme hizmeti için konak adlarını (örneğin `mysssmanagement.westus2.afs.azure.net` , sanal ağ Içindeki depolama eşitleme hizmeti için doğru özel IP 'ler) çözümlemek için sağlar. Özel bir uç nokta oluşturma perspektifinden isteğe bağlı olsa da, Azure Dosya Eşitleme aracısının depolama Eşitleme hizmetine erişmesi için açıkça gerekli olur. 

```powershell
# Get the desired Storage Sync Service suffix (afs.azure.net for public cloud).
# This is done like this so this script will seamlessly work for non-public Azure.
$azureEnvironment = Get-AzContext | `
    Select-Object -ExpandProperty Environment | `
    Select-Object -ExpandProperty Name

switch($azureEnvironment) {
    "AzureCloud" {
        $storageSyncSuffix = "afs.azure.net"
    }

    "AzureUSGovernment" {
        $storageSyncSuffix = "afs.azure.us"
    }
    
    default {
        Write-Error 
                -Message "The Azure environment $_ is not currently supported by Azure File Sync." `
                -ErrorAction Stop
    }
}

# For public cloud, this will generate the following DNS suffix:
# privatelink.afs.azure.net
$dnsZoneName = "privatelink.$storageSyncSuffix"

# Find a DNS zone matching desired name attached to this virtual network.
$dnsZone = Get-AzPrivateDnsZone | `
    Where-Object { $_.Name -eq $dnsZoneName } | `
    Where-Object {
        $privateDnsLink = Get-AzPrivateDnsVirtualNetworkLink `
                -ResourceGroupName $_.ResourceGroupName `
                -ZoneName $_.Name `
                -ErrorAction SilentlyContinue
        
        $privateDnsLink.VirtualNetworkId -eq $virtualNetwork.Id
    }

if ($null -eq $dnsZone) {
    # No matching DNS zone attached to virtual network, so create new one.
    $dnsZone = New-AzPrivateDnsZone `
            -ResourceGroupName $virtualNetworkResourceGroupName `
            -Name $dnsZoneName `
            -ErrorAction Stop

    $privateDnsLink = New-AzPrivateDnsVirtualNetworkLink `
            -ResourceGroupName $virtualNetworkResourceGroupName `
            -ZoneName $dnsZoneName `
            -Name "$virtualNetworkName-DnsLink" `
            -VirtualNetworkId $virtualNetwork.Id `
            -ErrorAction Stop
}
```
Artık özel DNS bölgesine bir başvurunuz olduğuna göre, depolama eşitleme hizmetiniz için bir kayıt oluşturmanız gerekir.

```PowerShell 
$privateEndpointIpFqdnMappings = $privateEndpoint | `
    Select-Object -ExpandProperty NetworkInterfaces | `
    Select-Object -ExpandProperty Id | `
    ForEach-Object { Get-AzNetworkInterface -ResourceId $_ } | `
    Select-Object -ExpandProperty IpConfigurations | `
    ForEach-Object { 
        $privateIpAddress = $_.PrivateIpAddress; 
        $_ | `
            Select-Object -ExpandProperty PrivateLinkConnectionProperties | `
            Select-Object -ExpandProperty Fqdns | `
            Select-Object `
                @{ 
                    Name = "PrivateIpAddress"; 
                    Expression = { $privateIpAddress } 
                }, `
                @{ 
                    Name = "FQDN"; 
                    Expression = { $_ } 
                } 
    }

foreach($ipFqdn in $privateEndpointIpFqdnMappings) {
    $privateDnsRecordConfig = New-AzPrivateDnsRecordConfig `
        -IPv4Address $ipFqdn.PrivateIpAddress
    
    $dnsEntry = $ipFqdn.FQDN.Substring(0, 
        $ipFqdn.FQDN.IndexOf(".", $ipFqdn.FQDN.IndexOf(".") + 1))

    New-AzPrivateDnsRecordSet `
            -ResourceGroupName $virtualNetworkResourceGroupName `
            -Name $dnsEntry `
            -RecordType A `
            -ZoneName $dnsZoneName `
            -Ttl 600 `
            -PrivateDnsRecords $privateDnsRecordConfig `
            -ErrorAction Stop | `
        Out-Null
}
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Depolama eşitleme hizmetiniz için özel bir uç nokta oluşturmak için önce depolama eşitleme hizmetinize bir başvuru almanız gerekir. Öğesini `<storage-sync-service-resource-group>` ve `<storage-sync-service>` ortamınız için doğru değerleri değiştirmeyi unutmayın. Aşağıdaki CLı komutları, kullanmakta olduğunuz sanal ağ bilgilerini yukarıda zaten doldurmuş olduğunu varsayar. 

```bash
storageSyncServiceResourceGroupName="<storage-sync-service-resource-group>"
storageSyncServiceName="<storage-sync-service>"

storageSyncService=$(az resource show \
        --resource-group $storageSyncServiceResourceGroupName \
        --name $storageSyncServiceName \
        --resource-type "Microsoft.StorageSync/storageSyncServices" \
        --query "id" | \
    tr -d '"')

storageSyncServiceRegion=$(az resource show \
        --resource-group $storageSyncServiceResourceGroupName \
        --name $storageSyncServiceName \
        --resource-type "Microsoft.StorageSync/storageSyncServices" \
        --query "location" | \
    tr -d '"')
```

Özel bir uç nokta oluşturmak için, önce alt ağın özel uç nokta ağ ilkesinin devre dışı olarak ayarlandığından emin olmanız gerekir. Ardından, komutuyla özel bir uç nokta oluşturabilirsiniz `az network private-endpoint create` .

```bash
# Disable private endpoint network policies
az network vnet subnet update \
        --ids $subnet \
        --disable-private-endpoint-network-policies \
        --output none

# Get virtual network location
region=$(az network vnet show \
        --ids $virtualNetwork \
        --query "location" | \
    tr -d '"')

# Create a private endpoint
privateEndpoint=$(az network private-endpoint create \
        --resource-group $storageSyncServiceResourceGroupName \
        --name "$storageSyncServiceName-PrivateEndpoint" \
        --location $region \
        --subnet $subnet \
        --private-connection-resource-id $storageSyncService \
        --group-id "Afs" \
        --connection-name "$storageSyncServiceName-Connection" \
        --query "id" | \
    tr -d '"')
```

Azure özel DNS bölgesi oluşturmak, depolama eşitleme hizmeti için konak adlarını (örneğin `mysssmanagement.westus2.afs.azure.net` , sanal ağ Içindeki depolama eşitleme hizmeti için doğru özel IP 'ler) çözümlemek için sağlar. Özel bir uç nokta oluşturma perspektifinden isteğe bağlı olsa da, Azure Dosya Eşitleme aracısının depolama Eşitleme hizmetine erişmesi için açıkça gerekli olur. 

```bash
# Get the desired storage account suffix (afs.azure.net for public cloud).
# This is done like this so this script will seamlessly work for non-public Azure.
azureEnvironment=$(az cloud show \
        --query "name" |
    tr -d '"')

storageSyncSuffix=""
if [ $azureEnvironment == "AzureCloud" ]
then
    storageSyncSuffix="afs.azure.net"
elif [ $azureEnvironment == "AzureUSGovernment" ]
then
    storageSyncSuffix="afs.azure.us"
else
    echo "Unsupported Azure environment $azureEnvironment."
fi

# For public cloud, this will generate the following DNS suffix:
# privatelinke.afs.azure.net.
dnsZoneName="privatelink.$storageSyncSuffix"

# Find a DNS zone matching desired name attached to this virtual network.
possibleDnsZones=""
possibleDnsZones=$(az network private-dns zone list \
        --query "[?name == '$dnsZoneName'].id" \
        --output tsv)

dnsZone=""
possibleDnsZone=""
for possibleDnsZone in $possibleDnsZones
do
    possibleResourceGroupName=$(az resource show \
            --ids $possibleDnsZone \
            --query "resourceGroup" | \
        tr -d '"')
    
    link=$(az network private-dns link vnet list \
            --resource-group $possibleResourceGroupName \
            --zone-name $dnsZoneName \
            --query "[?virtualNetwork.id == '$virtualNetwork'].id" \
            --output tsv)
    
    if [ -z $link ]
    then
        echo "1" > /dev/null
    else 
        dnsZoneResourceGroup=$possibleResourceGroupName
        dnsZone=$possibleDnsZone
        break
    fi  
done

if [ -z $dnsZone ]
then
    # No matching DNS zone attached to virtual network, so create a new one
    dnsZone=$(az network private-dns zone create \
            --resource-group $virtualNetworkResourceGroupName \
            --name $dnsZoneName \
            --query "id" | \
        tr -d '"')
    
    az network private-dns link vnet create \
            --resource-group $virtualNetworkResourceGroupName \
            --zone-name $dnsZoneName \
            --name "$virtualNetworkName-DnsLink" \
            --virtual-network $virtualNetwork \
            --registration-enabled false \
            --output none
    
    dnsZoneResourceGroup=$virtualNetworkResourceGroupName
fi
```

Artık özel DNS bölgesine bir başvurunuz olduğuna göre, depolama eşitleme hizmetiniz için bir kayıt oluşturmanız gerekir.

```bash
privateEndpointNIC=$(az network private-endpoint show \
        --ids $privateEndpoint \
        --query "networkInterfaces[0].id" | \
    tr -d '"')

privateIpAddresses=$(az network nic show \
        --ids $privateEndpointNIC \
        --query "ipConfigurations[].privateIpAddress" \
        --output tsv) 

hostNames=$(az network nic show \
        --ids $privateEndpointNIC \
        --query "ipConfigurations[].privateLinkConnectionProperties.fqdns[]" \
        --output tsv)

i=0
for privateIpAddress in $privateIpAddresses
do
    j=0
    targetHostName=""
    for hostName in $hostNames
    do
        if [ $i == $j ]
        then
            targetHostName=$hostName
            break
        fi

        j=$(expr $j + 1)
    done

    endpointName=$(echo $targetHostName | \
        cut -c1-$(expr $(expr index $targetHostName ".") - 1))

    az network private-dns record-set a create \
        --resource-group $dnsZoneResourceGroup \
        --zone-name $dnsZoneName \
        --name "$endpointName.$storageSyncServiceRegion" \
        --output none
    
    az network private-dns record-set a add-record \
        --resource-group $dnsZoneResourceGroup \
        --zone-name $dnsZoneName \
        --record-set-name "$endpointName.$storageSyncServiceRegion" \
        --ipv4-address $privateIpAddress \
        --output none

    i=$(expr $i + 1)
done
```
---

## <a name="restrict-access-to-the-public-endpoints"></a>Genel uç noktalara erişimi kısıtlama
Hem depolama hesabının hem de depolama eşitleme hizmetlerinin genel uç noktalarına erişimi kısıtlayabilirsiniz. Genel uç noktaya erişimi kısıtlamak, ağ paketlerinin yalnızca onaylanan konumlardan kabul edilmesini sağlayarak ek güvenlik sağlar. 

### <a name="restrict-access-to-the-storage-account-public-endpoint"></a>Depolama hesabı genel uç noktasına erişimi kısıtla
Genel uç noktaya erişim kısıtlaması, depolama hesabı güvenlik duvarı ayarları kullanılarak yapılır. Genel olarak, bir depolama hesabı için çoğu güvenlik duvarı ilkesi, ağ erişimini bir veya daha fazla sanal ağa kısıtlar. Bir depolama hesabına erişimi bir sanal ağla kısıtlamak için iki yaklaşım vardır:

- [Depolama hesabı için bir veya daha fazla özel uç nokta oluşturun](#create-the-storage-account-private-endpoint) ve genel uç noktaya erişimi devre dışı bırakın. Bu, yalnızca istenen sanal ağlardan gelen trafiğin depolama hesabı içindeki Azure dosya paylaşımlarına erişebilmesini sağlar.
- Genel uç noktayı bir veya daha fazla sanal ağla sınırlayın. Bu, *hizmet uç noktaları*adlı sanal ağın bir özelliği kullanılarak işe yarar. Bir hizmet uç noktası aracılığıyla bir depolama hesabıyla trafiği kısıtladığınızda, hala genel IP adresi aracılığıyla depolama hesabına erişmeye devam edersiniz.

#### <a name="disable-access-to-the-storage-account-public-endpoint"></a>Depolama hesabı genel uç noktasına erişimi devre dışı bırak
Genel uç noktaya erişim devre dışı bırakıldığında, depolama hesabına hala özel uç noktalar aracılığıyla erişilebilir. Aksi takdirde, depolama hesabının genel uç noktasına yönelik geçerli istekler reddedilir. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
[!INCLUDE [storage-files-networking-endpoints-public-disable-portal](../../../includes/storage-files-networking-endpoints-public-disable-portal.md)]

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!INCLUDE [storage-files-networking-endpoints-public-disable-powershell](../../../includes/storage-files-networking-endpoints-public-disable-powershell.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
[!INCLUDE [storage-files-networking-endpoints-public-disable-cli](../../../includes/storage-files-networking-endpoints-public-disable-cli.md)]

---

#### <a name="restrict-access-to-the-storage-account-public-endpoint-to-specific-virtual-networks"></a>Depolama hesabı genel uç noktasına erişimi belirli sanal ağlara kısıtlama
Depolama hesabını belirli sanal ağlarla kısıtladığınızda, belirtilen sanal ağların içinden genel uç noktaya yönelik isteklere izin vermiş olursunuz. Bu, *hizmet uç noktaları*adlı sanal ağın bir özelliği kullanılarak işe yarar. Bu, Özel uç noktalarla veya olmadan kullanılabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)
[!INCLUDE [storage-files-networking-endpoints-public-restrict-portal](../../../includes/storage-files-networking-endpoints-public-restrict-portal.md)]

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
[!INCLUDE [storage-files-networking-endpoints-public-restrict-powershell](../../../includes/storage-files-networking-endpoints-public-restrict-powershell.md)]

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
[!INCLUDE [storage-files-networking-endpoints-public-restrict-cli](../../../includes/storage-files-networking-endpoints-public-restrict-cli.md)]

---

### <a name="disable-access-to-the-storage-sync-service-public-endpoint"></a>Depolama eşitleme hizmeti genel uç noktasına erişimi devre dışı bırakma
Azure Dosya Eşitleme, yalnızca özel uç noktalar aracılığıyla belirli sanal ağlara erişimi kısıtlamanıza olanak sağlar; Azure Dosya Eşitleme, genel uç noktaya erişimi belirli sanal ağlara kısıtlamak için hizmet uç noktalarını desteklemez. Bu, depolama eşitleme hizmeti 'nin Genel uç noktası için iki durumun etkinleştirildiği ve devre dışı bırakıldığı anlamına gelir.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portal aracılığıyla bu mümkün değildir. Depolama eşitleme hizmeti genel uç noktasını devre dışı bırakma hakkında yönergeler almak için lütfen Azure PowerShell veya Azure CLı Tab yönergelerini seçin. 

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Depolama eşitleme hizmeti 'nin Genel uç noktasına erişimi devre dışı bırakmak için `incomingTrafficPolicy` depolama eşitleme hizmeti ' nde özelliğini olarak ayarlayacağız `AllowVirtualNetworksOnly` . Depolama eşitleme hizmeti 'nin Genel uç noktasına erişimi etkinleştirmek istiyorsanız, `incomingTrafficPolicy` `AllowAllTraffic` bunun yerine olarak ayarlayın. Ve ' nin değiştirilmesini unutmayın `<storage-sync-service-resource-group>` `<storage-sync-service>` .

```powershell
$storageSyncServiceResourceGroupName = "<storage-sync-service-resource-group>"
$storageSyncServiceName = "<storage-sync-service>"

$storageSyncService = Get-AzResource `
        -ResourceGroupName $storageSyncServiceResourceGroupName `
        -ResourceName $storageSyncServiceName `
        -ResourceType "Microsoft.StorageSync/storageSyncServices"

$storageSyncService.Properties.incomingTrafficPolicy = "AllowVirtualNetworksOnly"
$storageSyncService = $storageSyncService | Set-AzResource -Confirm:$false -Force
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Depolama eşitleme hizmeti 'nin Genel uç noktasına erişimi devre dışı bırakmak için `incomingTrafficPolicy` depolama eşitleme hizmeti ' nde özelliğini olarak ayarlayacağız `AllowVirtualNetworksOnly` . Depolama eşitleme hizmeti 'nin Genel uç noktasına erişimi etkinleştirmek istiyorsanız, `incomingTrafficPolicy` `AllowAllTraffic` bunun yerine olarak ayarlayın. Ve ' nin değiştirilmesini unutmayın `<storage-sync-service-resource-group>` `<storage-sync-service>` .

```bash
storageSyncServiceResourceGroupName="<storage-sync-service-resource-group>"
storageSyncServiceName="<storage-sync-service>"

az resource update \
        --resource-group $storageSyncServiceResourceGroupName \
        --name $storageSyncServiceName \
        --resource-type "Microsoft.StorageSync/storageSyncServices" \
        --set "properties.incomingTrafficPolicy=AllowVirtualNetworksOnly" \
        --output none
```
---

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosya Eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)