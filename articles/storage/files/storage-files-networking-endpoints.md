---
title: Azure Dosyaları ağ uç noktalarını yapılandırma | Microsoft Dokümanlar
description: Azure Dosyaları için ağ seçeneklerine genel bakış.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 3/19/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: cc487e8def180735606aa010651dde40ef93908e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80082510"
---
# <a name="configuring-azure-files-network-endpoints"></a>Azure Dosyaları ağ uç noktalarını yapılandırma
Azure Dosyaları, Azure dosya paylaşımları erişmek için iki ana uç nokta türü sağlar: 
- Genel BIR IP adresine sahip olan ve dünyanın her yerinden erişilebilen genel uç noktalar.
- Sanal ağ içinde bulunan ve bu sanal ağın adres alanı içinde özel bir IP adresine sahip olan özel uç noktalar.

Azure depolama hesabında ortak ve özel uç noktalar bulunur. Depolama hesabı, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları veya kuyruklar gibi diğer depolama kaynaklarını dağıtabileceğiniz paylaşılan bir depolama havuzunun yanı sıra yönetim yapısıdır.

Bu makalede, Azure dosya paylaşımına doğrudan erişmek için bir depolama hesabının uç noktalarının nasıl yapılandırılacak olduğu üzerinde duruluyor. Bu belgede sağlanan ayrıntıların çoğu, Azure Dosya Eşitlemi'nin depolama hesabı için ortak ve özel uç noktalarıyla nasıl çalıştığı konusunda da geçerlidir, ancak Azure Dosya Eşitleme dağıtımı için ağ la ilgili ayrıntılar hakkında daha fazla bilgi için Azure [Dosya Eşitleme proxy'sini ve güvenlik duvarı ayarlarını yapılandırma](storage-sync-files-firewall-and-proxy.md)konusuna bakın.

Nasıl yol gösterebileceğimizi okumadan önce Azure Dosyaları ağ la ilgili [hususları](storage-files-networking-overview.md) okumanızı öneririz.

## <a name="prerequisites"></a>Ön koşullar
- Bu makalede, zaten bir Azure aboneliği oluşturduğunuz varsayar. Zaten bir aboneliğiniz yoksa, başlamadan önce ücretsiz bir [hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Bu makalede, şirket içinde bağlanmak istediğiniz bir depolama hesabında zaten bir Azure dosya paylaşımı oluşturduğunuz varsayar. Azure dosya paylaşımını nasıl oluşturabilirsiniz öğrenmek için [bkz.](storage-how-to-create-file-share.md)
- Azure PowerShell'i kullanmak istiyorsanız, [en son sürümü yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)
- Azure CLI'yi kullanmak istiyorsanız, [en son sürümü yükleyin.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma
Depolama hesabınız için özel bir bitiş noktası oluşturmak, aşağıdaki Azure kaynaklarının dağıtılmasına neden olur:

- **Özel bitiş noktası**: Depolama hesabının özel bitiş noktasını temsil eden bir Azure kaynağı. Bunu bir depolama hesabı ve ağ arabirimi bağlayan bir kaynak olarak düşünebilirsiniz.
- **Ağ arabirimi (NIC)**: Belirtilen sanal ağ/alt ağ içinde özel bir IP adresi tutan ağ arabirimi. Bu, sanal bir makine dağıttığınızda dağıtılan aynı kaynaktır, ancak vm'ye atanmak yerine, özel bitiş noktasına aittir.
- **Özel Bir DNS bölgesi**: Bu sanal ağ için daha önce hiç özel bir bitiş noktası dağıtmadıysanız, sanal ağınız için yeni bir özel DNS bölgesi dağıtılır. Bu DNS bölgesindeki depolama hesabı için bir DNS A kaydı da oluşturulur. Bu sanal ağda zaten özel bir bitiş noktası dağıttıysanız, depolama hesabı için yeni bir A kaydı varolan DNS bölgesine eklenir. Bir DNS bölgesi dağıtmak isteğe bağlıdır, ancak son derece önerilir ve Azure dosya paylaşımlarınızı bir AD hizmeti ilkesiyle monte ediyorsanız veya FileREST API'sini kullanıyorsanız gereklidir.

> [!Note]  
> Bu makalede, Azure Genel bölgeleri için depolama hesabı DNS soneki kullanır. `core.windows.net` Bu yorum, Azure ABD Devlet bulutu ve Azure Çin bulutu gibi Azure Sovereign bulutları için de geçerlidir - ortamınız için uygun soneklerin yerine geçer. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Özel bir bitiş noktası oluşturmak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda, yeni bir özel bitiş noktası oluşturmak için **Özel bitiş noktası bağlantılarını**ve ardından **+ Özel bitiş noktasını** seçin. 

![İçeriğin depolama hesabı tablosundaki özel uç nokta bağlantıları öğesinin ekran görüntüsü](media/storage-files-networking-endpoints/create-private-endpoint-0.png)

Ortaya çıkan sihirbazın tamamlanması gereken birden çok sayfa vardır.

Temel **Bilgiler** bıçak larında, özel bitiş noktanız için istediğiniz kaynak grubunu, adı ve bölgeyi seçin. Bunlar istediğiniz her şey olabilir, yine de depolama hesabıyla eşleşmek zorunda değildir, ancak içinde özel bitiş noktası oluşturmak istediğiniz sanal ağla aynı bölgede özel bitiş noktası oluşturmanız gerekir.

![Özel bitiş noktası oluşturma bölümünün Temeller bölümünün ekran görüntüsü](media/storage-files-networking-endpoints/create-private-endpoint-1.png)

**Kaynak** bıyısında, **dizinimdeki bir Azure kaynağına bağlan'ın**radyo düğmesini seçin. **Kaynak türüaltında,** kaynak türü için **Microsoft.Storage/storageAccounts'u** seçin. **Kaynak** alanı, bağlanmak istediğiniz Azure dosya paylaşımının depolama hesabıdır. Hedef alt kaynak **dosyadır**, çünkü bu Azure Files içindir.

**Configuration** blade, özel bitiş noktanızı eklemek istediğiniz belirli sanal ağı ve alt ağı seçmenize olanak tanır. Yukarıda oluşturduğunuz sanal ağı seçin. Hizmet bitiş noktanızı yukarıda eklediğiniz alt ağdan farklı bir alt ağ seçmeniz gerekir. Configuration blade ayrıca özel DNS bölgesini oluşturmak/güncellemek için bilgileri de içerir. Varsayılan `privatelink.file.core.windows.net` bölgeyi kullanmanızı öneririz.

![Yapılandırma bölümünün ekran görüntüsü](media/storage-files-networking-endpoints/create-private-endpoint-2.png)

Özel bitiş noktasını oluşturmak için **Gözden Geçir +'yı** tıklatın. 

Sanal ağınızın içinde sanal bir makine varsa veya [Burada](storage-files-networking-dns.md)açıklandığı gibi DNS iletmesini yapınız varsa, PowerShell, komut satırı veya terminalden (Windows, Linux veya macOS için çalışır) aşağıdaki komutları çalıştırarak özel bitiş noktanızın doğru şekilde ayarlandığını test edebilirsiniz. Uygun depolama `<storage-account-name>` hesabı adı ile değiştirmeniz gerekir:

```
nslookup <storage-account-name>.file.core.windows.net
```

Her şey başarılı bir şekilde çalıştıysa, sanal `192.168.0.5` ağınızdaki özel bitiş noktasının özel IP adresi (Windows için gösterilen çıktı) aşağıdaki çıktıyı görmeniz gerekir:

```Output
Server:  UnKnown
Address:  10.2.4.4

Non-authoritative answer:
Name:    storageaccount.privatelink.file.core.windows.net
Address:  192.168.0.5
Aliases:  storageaccount.file.core.windows.net
```

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Depolama hesabınız için özel bir bitiş noktası oluşturmak için öncelikle depolama hesabınıza ve özel bitiş noktasını eklemek istediğiniz sanal ağ alt ağına bir başvuru almanız gerekir. Değiştir `<storage-account-resource-group-name>` `<storage-account-name>`, `<vnet-resource-group-name>` `<vnet-name>`, `<vnet-subnet-name>` , , ve aşağıda:

```PowerShell
$storageAccountResourceGroupName = "<storage-account-resource-group-name>"
$storageAccountName = "<storage-account-name>"
$virtualNetworkResourceGroupName = "<vnet-resource-group-name>"
$virtualNetworkName = "<vnet-name>"
$subnetName = "<vnet-subnet-name>"

# Get storage account reference, and throw error if it doesn't exist
$storageAccount = Get-AzStorageAccount `
        -ResourceGroupName $storageAccountResourceGroupName `
        -Name $storageAccountName `
        -ErrorAction SilentlyContinue

if ($null -eq $storageAccount) {
    $errorMessage = "Storage account $storageAccountName not found "
    $errorMessage += "in resource group $storageAccountResourceGroupName."
    Write-Error -Message $errorMessage -ErrorAction Stop
}

# Get virtual network reference, and throw error if it doesn't exist
$virtualNetwork = Get-AzVirtualNetwork `
        -ResourceGroupName $virtualNetworkResourceGroupName `
        -Name $virtualNetworkName `
        -ErrorAction SilentlyContinue

if ($null -eq $virtualNetwork) {
    $errorMessage = "Virtual network $virtualNetworkName not found "
    $errorMessage += "in resource group $virtualNetworkResourceGroupName."
    Write-Error -Message $errorMessage -ErrorAction Stop
}

# Get reference to virtual network subnet, and throw error if it doesn't exist
$subnet = $virtualNetwork | `
    Select-Object -ExpandProperty Subnets | `
    Where-Object { $_.Name -eq $subnetName }

if ($null -eq $subnet) {
    Write-Error `
            -Message "Subnet $subnetName not found in virtual network $virtualNetworkName." `
            -ErrorAction Stop
}
```

Özel bir bitiş noktası oluşturmak için depolama hesabına özel bir bağlantı hizmeti bağlantısı oluşturmanız gerekir. Özel bağlantı hizmeti bağlantısı, özel bitiş noktasının oluşturulmasına giriştir. 

```PowerShell
# Disable private endpoint network policies
$subnet.PrivateEndpointNetworkPolicies = "Disabled"
$virtualNetwork | Set-AzVirtualNetwork | Out-Null

# Create a private link service connection to the storage account.
$privateEndpointConnection = New-AzPrivateLinkServiceConnection `
        -Name "$storageAccountName-Connection" `
        -PrivateLinkServiceId $storageAccount.Id `
        -GroupId "file"

# Create a new private endpoint.
$privateEndpoint = New-AzPrivateEndpoint `
        -ResourceGroupName $storageAccountResourceGroupName `
        -Name "$storageAccountName-PrivateEndpoint" `
        -Location $virtualNetwork.Location `
        -Subnet $subnet `
        -PrivateLinkServiceConnection $privateEndpointConnection `
        -ErrorAction Stop
```

Azure özel DNS bölgesi oluşturmak, sanal ağın içindeki `storageaccount.file.core.windows.net` özel IP'yi çözmek gibi depolama hesabının özgün adını etkinleştirir. Özel bir bitiş noktası oluşturma açısından isteğe bağlı olsa da, Bir AD kullanıcı ilkesi kullanarak Azure dosya paylaşımını monte etmek veya REST API üzerinden erişmek için açıkça gereklidir.  

```PowerShell
# Get the desired storage account suffix (core.windows.net for public cloud).
# This is done like this so this script will seamlessly work for non-public Azure.
$storageAccountSuffix = Get-AzContext | `
    Select-Object -ExpandProperty Environment | `
    Select-Object -ExpandProperty StorageEndpointSuffix

# For public cloud, this will generate the following DNS suffix:
# privatelink.file.core.windows.net.
$dnsZoneName = "privatelink.file.$storageAccountSuffix"

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

Artık özel DNS bölgesine bir başvurunuz olduğuna göre, depolama hesabınız için bir A kaydı oluşturmanız gerekir.

```PowerShell
$privateEndpointIP = $privateEndpoint | `
    Select-Object -ExpandProperty NetworkInterfaces | `
    Select-Object @{ 
        Name = "NetworkInterfaces"; 
        Expression = { Get-AzNetworkInterface -ResourceId $_.Id } 
    } | `
    Select-Object -ExpandProperty NetworkInterfaces | `
    Select-Object -ExpandProperty IpConfigurations | `
    Select-Object -ExpandProperty PrivateIpAddress

$privateDnsRecordConfig = New-AzPrivateDnsRecordConfig `
        -IPv4Address $privateEndpointIP

New-AzPrivateDnsRecordSet `
        -ResourceGroupName $virtualNetworkResourceGroupName `
        -Name $storageAccountName `
        -RecordType A `
        -ZoneName $dnsZoneName `
        -Ttl 600 `
        -PrivateDnsRecords $privateDnsRecordConfig `
        -ErrorAction Stop | `
    Out-Null
```

Sanal ağınızın içinde sanal bir makine varsa veya [burada](storage-files-networking-dns.md)açıklandığı gibi DNS iletmesini yapılandırmışsanız, özel bitiş noktanızın aşağıdaki komutlarla doğru şekilde ayarlandığını test edebilirsiniz:

```PowerShell
$storageAccountHostName = [System.Uri]::new($storageAccount.PrimaryEndpoints.File) | `
    Select-Object -ExpandProperty Host

Resolve-DnsName -Name $storageAccountHostName
```

Her şey başarılı bir şekilde çalıştıysa, sanal `192.168.0.5` ağınızdaki özel bitiş noktasının özel IP adresi aşağıdaki çıktıyı görmeniz gerekir:

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
Depolama hesabınız için özel bir bitiş noktası oluşturmak için öncelikle depolama hesabınıza ve özel bitiş noktasını eklemek istediğiniz sanal ağ alt ağına bir başvuru almanız gerekir. Değiştir `<storage-account-resource-group-name>` `<storage-account-name>`, `<vnet-resource-group-name>` `<vnet-name>`, `<vnet-subnet-name>` , , ve aşağıda:

```bash
storageAccountResourceGroupName="<storage-account-resource-group-name>"
storageAccountName="<storage-account-name>"
virtualNetworkResourceGroupName="<vnet-resource-group-name>"
virtualNetworkName="<vnet-name>"
subnetName="<vnet-subnet-name>"

# Get storage account ID 
storageAccount=$(az storage account show \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --query "id" | \
    tr -d '"')

# Get virtual network ID
virtualNetwork=$(az network vnet show \
        --resource-group $virtualNetworkResourceGroupName \
        --name $virtualNetworkName \
        --query "id" | \
    tr -d '"')

# Get subnet ID
subnet=$(az network vnet subnet show \
        --resource-group $virtualNetworkResourceGroupName \
        --vnet-name $virtualNetworkName \
        --name $subnetName \
        --query "id" | \
    tr -d '"')
```

Özel bir bitiş noktası oluşturmak için öncelikle alt ağın özel uç noktası ağ ilkesinin devre dışı bırakıladığından emin olmalısınız. Daha sonra `az network private-endpoint create` komutu ile özel bir bitiş noktası oluşturabilirsiniz

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
        --resource-group $virtualNetworkResourceGroupName \
        --name "$storageAccountName-PrivateEndpoint" \
        --location $region \
        --subnet $subnet \
        --private-connection-resource-id $storageAccount \
        --group-ids "file" \
        --connection-name "$storageAccountName-Connection" \
        --query "id" | \
    tr -d '"')
```

Azure özel DNS bölgesi oluşturmak, sanal ağın içindeki `storageaccount.file.core.windows.net` özel IP'yi çözmek gibi depolama hesabının özgün adını etkinleştirir. Özel bir bitiş noktası oluşturma açısından isteğe bağlı olsa da, Bir AD kullanıcı ilkesi kullanarak Azure dosya paylaşımını monte etmek veya REST API üzerinden erişmek için açıkça gereklidir.  

```bash
# Get the desired storage account suffix (core.windows.net for public cloud).
# This is done like this so this script will seamlessly work for non-public Azure.
storageAccountSuffix=$(az cloud show \
        --query "suffixes.storageEndpoint" | \
    tr -d '"')

# For public cloud, this will generate the following DNS suffix:
# privatelink.file.core.windows.net.
dnsZoneName="privatelink.file.$storageAccountSuffix"

# Find a DNS zone matching desired name attached to this virtual network.
possibleDnsZones=$(az network private-dns zone list \
        --query "[?name == '$dnsZoneName'].id" \
        --output tsv)

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
        1 > /dev/null
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
            --resource-group $resourceGroupName \
            --zone-name $zoneName \
            --name "$virtualNetworkName-DnsLink" \
            --virtual-network $virtualNetwork \
            --registration-enabled false \
            --output none
fi
```

Artık özel DNS bölgesine bir başvurunuz olduğuna göre, depolama hesabınız için bir A kaydı oluşturmanız gerekir.

```bash
privateEndpointNIC=$(az network private-endpoint show \
        --ids $privateEndpoint \
        --query "networkInterfaces[0].id" | \
    tr -d '"')

privateEndpointIP=$(az network nic show \
        --ids $privateEndpointNIC \
        --query "ipConfigurations[0].privateIpAddress" | \
    tr -d '"')

az network private-dns record-set a create \
        --resource-group $dnsZoneResourceGroup \
        --zone-name $dnsZoneName \
        --name $storageAccountName \
        --output none

az network private-dns record-set a add-record \
        --resource-group $dnsZoneResourceGroup \
        --zone-name $dnsZoneName \
        --record-set-name $storageAccountName \
        --ipv4-address $privateEndpointIP \
        --output none
```

Sanal ağınızın içinde sanal bir makine varsa veya [burada](storage-files-networking-dns.md)açıklandığı gibi DNS iletmesini yapılandırmışsanız, özel bitiş noktanızın aşağıdaki komutlarla doğru şekilde ayarlandığını test edebilirsiniz:

```bash
httpEndpoint=$(az storage account show \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.File" | \
    tr -d '"')

hostName=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint) | tr -d "/")
nslookup $hostName
```

Her şey başarılı bir şekilde çalıştıysa, sanal `192.168.0.5` ağınızdaki özel bitiş noktasının özel IP adresi aşağıdaki çıktıyı görmeniz gerekir:

```Output
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
storageaccount.file.core.windows.net      canonical name = storageaccount.privatelink.file.core.windows.net.
Name:   storageaccount.privatelink.file.core.windows.net
Address: 192.168.0.5
```

---

## <a name="restrict-access-to-the-public-endpoint"></a>Genel bitiş noktasına erişimi kısıtlama
Depolama hesabı güvenlik duvarı ayarlarını kullanarak ortak uç noktaya erişimi kısıtlayabilirsiniz. Genel olarak, bir depolama hesabı için çoğu güvenlik duvarı ilkeleri, ağ daki bir veya daha fazla sanal ağa erişimi kısıtlar. Bir depolama hesabına erişimi sanal ağla sınırlamak için iki yaklaşım vardır:

- [Depolama hesabı için bir veya daha fazla özel uç nokta oluşturun](#create-a-private-endpoint) ve tüm genel bitiş noktasına erişimi kısıtlayın. Bu, yalnızca istenen sanal ağlar içinden kaynaklanan trafiğin depolama hesabı içindeki Azure dosya paylaşımlarına erişmesini sağlar.
- Ortak bitiş noktasını bir veya daha fazla sanal ağla sınırlandırın. Bu, *hizmet bitiş noktaları*olarak adlandırılan sanal ağın bir yeteneğini kullanarak çalışır. Bir hizmet bitiş noktası aracılığıyla trafiği bir depolama hesabıyla sınırladığınızı, yine de ortak IP adresi üzerinden depolama hesabına erişmeye devam esiniz.

### <a name="restrict-all-access-to-the-public-endpoint"></a>Tüm genel bitiş noktasına erişimi kısıtlama
Ortak bitiş noktasına tüm erişim kısıtlandığında, depolama hesabına özel uç noktaları üzerinden erişilebilir. Aksi takdirde depolama hesabının genel bitiş noktasına yönelik geçerli istekler reddedilir. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Tüm ortak bitiş noktasına erişimi kısıtlamak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **Güvenlik Duvarları ve sanal ağları**seçin.

Sayfanın üst kısmında **Seçili ağlar** radyo düğmesini seçin. Bu, genel bitiş noktasının kısıtlamasını denetlemek için bir dizi ayarı gizleyin. Azure Dosya Eşitlemi gibi güvenilir ilk taraf Microsoft hizmetlerinin depolama hesabına erişmesine izin vermek için **güvenilen Microsoft hizmetlerine izin ver'i** işaretleyin.

![Uygun kısıtlar yerinde Firewalls ve sanal ağlar bıçak ekran görüntüsü](media/storage-files-networking-endpoints/restrict-public-endpoint-0.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Aşağıdaki PowerShell komutu, depolama hesabının genel bitiş noktasına yönelik tüm trafiği reddeder. Bu komutun `-Bypass` parametre kümesi `AzureServices`ne olduğunu unutmayın. Bu, Azure Dosya Eşitlemi gibi güvenilir ilk taraf hizmetlerinin depolama hesabına ortak bitiş noktası üzerinden erişmesine olanak tanır.

```PowerShell
# This assumes $storageAccount is still defined from the beginning of this of this guide.
$storageAccount | Update-AzStorageAccountNetworkRuleSet `
        -DefaultAction Deny `
        -Bypass AzureServices `
        -WarningAction SilentlyContinue `
        -ErrorAction Stop | `
    Out-Null
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Aşağıdaki CLI komutu, depolama hesabının genel bitiş noktasına yönelik tüm trafiği reddeder. Bu komutun `-bypass` parametre kümesi `AzureServices`ne olduğunu unutmayın. Bu, Azure Dosya Eşitlemi gibi güvenilir ilk taraf hizmetlerinin depolama hesabına ortak bitiş noktası üzerinden erişmesine olanak tanır.

```bash
# This assumes $storageAccountResourceGroupName and $storageAccountName 
# are still defined from the beginning of this guide.
az storage account update \
    --resource-group $storageAccountResourceGroupName \
    --name $storageAccountName \
    --bypass "AzureServices" \
    --default-action "Deny" \
    --output none
```
---

### <a name="restrict-access-to-the-public-endpoint-to-specific-virtual-networks"></a>Ortak bitiş noktasına erişimi belirli sanal ağlarla sınırlama
Depolama hesabını belirli sanal ağlarla sınırladiğinizde, isteklerin belirtilen sanal ağlar içinden genel bitiş noktasına girmesine izin verirsiniz. Bu, *hizmet bitiş noktaları*olarak adlandırılan sanal ağın bir yeteneğini kullanarak çalışır. Bu özel uç noktaları ile veya olmadan kullanılabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Ortak bitiş noktasını belirli sanal ağlarla sınırlamak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **Güvenlik Duvarları ve sanal ağları**seçin. 

Sayfanın üst kısmında **Seçili ağlar** radyo düğmesini seçin. Bu, genel bitiş noktasının kısıtlamasını denetlemek için bir dizi ayarı gizleyin. Depolama hesabına ortak bitiş noktası üzerinden erişmesine izin verilmesi gereken belirli sanal ağı seçmek için **+Ekle'yi** tıklatın. Bu, bu sanal ağ için bir sanal ağ ve bir alt ağ seçilmesi gerekir. 

Azure Dosya Eşitlemi gibi güvenilir ilk taraf Microsoft hizmetlerinin depolama hesabına erişmesine izin vermek için **güvenilen Microsoft hizmetlerine izin ver'i** işaretleyin.

![Belirli bir sanal ağ ile Güvenlik Duvarları ve sanal ağlar bıçak ekran görüntüsü ortak bitiş noktası üzerinden depolama hesabına erişmek için izin](media/storage-files-networking-endpoints/restrict-public-endpoint-1.png)

# <a name="powershell"></a>[Powershell](#tab/azure-powershell)
Hizmet bitiş noktalarını kullanarak depolama hesabının genel bitiş noktasına erişimi belirli sanal ağlarla sınırlamak için öncelikle depolama hesabı ve sanal ağ hakkında bilgi toplamamız gerekir. `<vnet-name>` `<subnet-name>` Bu bilgileri doldurun ve bu bilgileri toplayın. `<storage-account-resource-group>` `<storage-account-name>` `<vnet-resource-group-name>`

```PowerShell
$storageAccountResourceGroupName = "<storage-account-resource-group>"
$storageAccountName = "<storage-account-name>"
$restrictToVirtualNetworkResourceGroupName = "<vnet-resource-group-name>"
$restrictToVirtualNetworkName = "<vnet-name>"
$subnetName = "<subnet-name>"

$storageAccount = Get-AzStorageAccount `
        -ResourceGroupName $storageAccountResourceGroupName `
        -Name $storageAccountName `
        -ErrorAction Stop

$virtualNetwork = Get-AzVirtualNetwork `
        -ResourceGroupName $restrictToVirtualNetworkResourceGroupName `
        -Name $restrictToVirtualNetworkName `
        -ErrorAction Stop

$subnet = $virtualNetwork | `
    Select-Object -ExpandProperty Subnets | `
    Where-Object { $_.Name -eq $subnetName }

if ($null -eq $subnet) {
    Write-Error `
            -Message "Subnet $subnetName not found in virtual network $restrictToVirtualNetworkName." `
            -ErrorAction Stop
}
```

Sanal ağdaki trafiğin Azure ağ dokusu tarafından depolama hesabı nın genel bitiş noktasına ulaşabilmesi için, sanal ağın alt ağının hizmet bitiş noktasının `Microsoft.Storage` açığa çıkmasına izin vermesi gerekir. Aşağıdaki PowerShell komutları, zaten `Microsoft.Storage` orada değilse, hizmet bitiş noktasını alt ağa ekler.

```PowerShell
$serviceEndpoints = $subnet | `
    Select-Object -ExpandProperty ServiceEndpoints | `
    Select-Object -ExpandProperty Service

if ($serviceEndpoints -notcontains "Microsoft.Storage") {
    if ($null -eq $serviceEndpoints) {
        $serviceEndpoints = @("Microsoft.Storage")
    } elseif ($serviceEndpoints -is [string]) {
        $serviceEndpoints = @($serviceEndpoints, "Microsoft.Storage")
    } else {
        $serviceEndpoints += "Microsoft.Storage"
    }

    $virtualNetwork = $virtualNetwork | Set-AzVirtualNetworkSubnetConfig `
            -Name $subnetName `
            -AddressPrefix $subnet.AddressPrefix `
            -ServiceEndpoint $serviceEndpoints `
            -WarningAction SilentlyContinue `
            -ErrorAction Stop | `
        Set-AzVirtualNetwork `
            -ErrorAction Stop
}
```

Depolama hesabı trafiğini kısıtlamanın son adımı, bir ağ kuralı oluşturmak ve depolama hesabının ağ kuralı kümesine eklemektir.

```PowerShell
$networkRule = $storageAccount | Add-AzStorageAccountNetworkRule `
    -VirtualNetworkResourceId $subnet.Id `
    -ErrorAction Stop

$storageAccount | Update-AzStorageAccountNetworkRuleSet `
        -DefaultAction Deny `
        -Bypass AzureServices `
        -VirtualNetworkRule $networkRule `
        -WarningAction SilentlyContinue `
        -ErrorAction Stop | `
    Out-Null
```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
Hizmet bitiş noktalarını kullanarak depolama hesabının genel bitiş noktasına erişimi belirli sanal ağlarla sınırlamak için öncelikle depolama hesabı ve sanal ağ hakkında bilgi toplamamız gerekir. `<vnet-name>` `<subnet-name>` Bu bilgileri doldurun ve bu bilgileri toplayın. `<storage-account-resource-group>` `<storage-account-name>` `<vnet-resource-group-name>`

```bash
storageAccountResourceGroupName="<storage-account-resource-group>"
storageAccountName="<storage-account-name>"
restrictToVirtualNetworkResourceGroupName="<vnet-resource-group-name>"
restrictToVirtualNetworkName="<vnet-name>"
subnetName="<subnet-name>"

storageAccount=$(az storage account show \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --query "id" | \
    tr -d '"')

virtualNetwork=$(az network vnet show \
        --resource-group $restrictToVirtualNetworkResourceGroupName \
        --name $restrictToVirtualNetworkName \
        --query "id" | \
    tr -d '"')

subnet=$(az network vnet subnet show \
        --resource-group $restrictToVirtualNetworkResourceGroupName \
        --vnet-name $restrictToVirtualNetworkName \
        --name $subnetName \
        --query "id" | \
    tr -d '"')
```

Sanal ağdaki trafiğin Azure ağ dokusu tarafından depolama hesabı nın genel bitiş noktasına ulaşabilmesi için, sanal ağın alt ağının hizmet bitiş noktasının `Microsoft.Storage` açığa çıkmasına izin vermesi gerekir. Aşağıdaki CLI komutları, zaten `Microsoft.Storage` orada değilse, hizmet bitiş noktasını alt ağa ekler.

```bash
serviceEndpoints=$(az network vnet subnet show \
        --resource-group $restrictToVirtualNetworkResourceGroupName \
        --vnet-name $restrictToVirtualNetworkName \
        --name $subnetName \
        --query "serviceEndpoints[].service" \
        --output tsv)

foundStorageServiceEndpoint=false
for serviceEndpoint in $serviceEndpoints
do
    if [ $serviceEndpoint = "Microsoft.Storage" ]
    then
        foundStorageServiceEndpoint=true
    fi
done

if [ $foundStorageServiceEndpoint = false ] 
then
    serviceEndpointList=""

    for serviceEndpoint in $serviceEndpoints
    do
        serviceEndpointList+=$serviceEndpoint
        serviceEndpointList+=" "
    done
    
    serviceEndpointList+="Microsoft.Storage"

    az network vnet subnet update \
            --ids $subnet \
            --service-endpoints $serviceEndpointList \
            --output none
fi
```

Depolama hesabı trafiğini kısıtlamanın son adımı, bir ağ kuralı oluşturmak ve depolama hesabının ağ kuralı kümesine eklemektir.

```bash
az storage account network-rule add \
        --resource-group $storageAccountResourceGroupName \
        --account-name $storageAccountName \
        --subnet $subnet \
        --output none

az storage account update \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --bypass "AzureServices" \
        --default-action "Deny" \
        --output none
```

---

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyaları ağ la ilgili hususlar](storage-files-networking-overview.md)
- [Azure Dosyaları için DNS yönlendirmeyi yapılandırma](storage-files-networking-dns.md)
- [Azure Dosyaları için S2S VPN Yapılandırma](storage-files-configure-s2s-vpn.md)