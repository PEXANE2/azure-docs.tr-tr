---
title: Azure dosyaları ağ uç noktalarını yapılandırma | Microsoft Docs
description: Azure dosyaları için ağ seçeneklerine genel bakış.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 3/19/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 4695164e7bcbc63b852f2f4364cdccbc8ea7d8c4
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83849323"
---
# <a name="configuring-azure-files-network-endpoints"></a>Azure dosyaları ağ uç noktalarını yapılandırma
Azure dosyaları, Azure dosya paylaşımlarına erişmek için iki ana uç nokta türü sağlar: 
- Genel bir IP adresi olan ve dünyanın herhangi bir yerinden erişilebilen genel uç noktalar.
- Bir sanal ağ içinde bulunan ve bu sanal ağın adres alanından özel bir IP adresine sahip olan özel uç noktalar.

Azure depolama hesabında genel ve özel uç noktalar mevcuttur. Depolama hesabı, birden çok dosya paylaşımını ve BLOB kapsayıcıları ya da kuyrukları gibi diğer depolama kaynaklarını dağıtabileceğiniz, paylaşılan bir depolama havuzunu temsil eden bir yönetim yapısıdır.

Bu makalede, Azure dosya paylaşımında doğrudan erişim için bir depolama hesabının uç noktalarının nasıl yapılandırılacağı ele alınmaktadır. Bu belgede sunulan ayrıntıların çoğu, depolama hesabının genel ve özel uç noktaları ile birlikte Azure Dosya Eşitleme için de geçerlidir. ancak, bir Azure Dosya Eşitleme dağıtımına yönelik ağ hususları hakkında daha fazla bilgi için, bkz [Azure dosya eşitleme proxy ve güvenlik duvarı ayarlarını yapılandırma](storage-sync-files-firewall-and-proxy.md).

Bu nasıl yapılır kılavuzu okumadan önce [Azure dosyaları ağ oluşturma konuları](storage-files-networking-overview.md) okumanız önerilir.

## <a name="prerequisites"></a>Ön koşullar
- Bu makalede, zaten bir Azure aboneliği oluşturmuş olduğunuz varsayılmaktadır. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
- Bu makalede, Şirket içinden bağlamak istediğiniz bir depolama hesabında bir Azure dosya paylaşımının zaten oluşturulduğunu varsaymış olursunuz. Azure dosya paylaşımının nasıl oluşturulacağını öğrenmek için bkz. [Azure dosya paylaşma oluşturma](storage-how-to-create-file-share.md).
- Azure PowerShell kullanmayı düşünüyorsanız, [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Azure CLı 'yı kullanmayı planlıyorsanız [en son sürümü yükleyebilirsiniz](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma
Depolama hesabınız için özel bir uç nokta oluşturulması, aşağıdaki Azure kaynaklarının dağıtılmasının oluşmasına neden olur:

- **Özel uç nokta**: depolama hesabının özel uç noktasını temsil eden bir Azure kaynağı. Bunu, depolama hesabını ve ağ arabirimini bağlayan bir kaynak olarak düşünebilirsiniz.
- **Ağ arabirimi (NIC)**: belirtilen sanal ağ/alt ağ içinde özel bir IP adresi tutan ağ arabirimi. Bu, bir sanal makineyi dağıtırken dağıtılan aynı kaynaktır, ancak bir VM 'ye atanmak yerine özel uç noktaya aittir.
- **Özel BIR DNS bölgesi**: daha önce bu sanal ağ için özel bir uç nokta dağıtmadıysanız, sanal ağınız için yeni BIR özel DNS bölgesi dağıtılır. DNS A kaydı, bu DNS bölgesindeki depolama hesabı için de oluşturulacaktır. Bu sanal ağda zaten özel bir uç nokta dağıttıysanız, var olan DNS bölgesine depolama hesabı için yeni bir kayıt eklenecektir. Bir DNS bölgesinin dağıtımı isteğe bağlıdır, ancak Azure dosya paylaşımlarınızı bir AD hizmet sorumlusu ile veya FileREST API 'sini kullanarak oluşturursanız gereklidir.

> [!Note]  
> Bu makalede, Azure ortak bölgeleri için depolama hesabı DNS son eki kullanılmaktadır `core.windows.net` . Bu yorum, Azure ABD kamu bulutu ve Azure Çin bulutu gibi Azure Sogeign bulutları için de geçerlidir. ortamınız için uygun son ekleri yerine koyun. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Özel bir uç nokta oluşturmak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **Özel uç nokta bağlantıları**' nı ve ardından **+ Özel uç noktasını** seçerek yeni bir özel uç nokta oluşturun. 

![Depolama hesabı İçindekiler tablosundaki özel uç nokta bağlantıları öğesinin ekran görüntüsü](media/storage-files-networking-endpoints/create-private-endpoint-0.png)

Ortaya çıkan sihirbazda, tamamlanacak birden çok sayfa vardır.

**Temel bilgiler** dikey penceresinde, Özel uç noktanız için istenen kaynak grubunu, adı ve bölgeyi seçin. Bunlar istediğiniz gibi olabilir, ancak, içinde özel uç nokta oluşturmak istediğiniz sanal ağ ile aynı bölgede özel uç nokta oluşturmanız gerekir.

![Özel uç nokta oluştur bölümünün temel bilgiler bölümünün ekran görüntüsü](media/storage-files-networking-endpoints/create-private-endpoint-1.png)

**Kaynak** dikey penceresinde, **Dizinimde bir Azure kaynağına bağlanmak**için radyo düğmesini seçin. **Kaynak**türü altında, kaynak türü için **Microsoft. Storage/storageaccounts** ' ı seçin. **Kaynak** alanı, bağlanmak istediğiniz Azure dosya paylaşımının bulunduğu depolama hesabıdır. Azure dosyaları için olduğundan, hedef alt kaynak **Dosya**.

**Yapılandırma** dikey penceresi, Özel uç noktanızı eklemek istediğiniz belirli sanal ağı ve alt ağı seçmenize olanak sağlar. Yukarıda oluşturduğunuz sanal ağı seçin. Hizmet uç noktanızı eklemiş olduğunuz alt ağdan farklı bir alt ağ seçmelisiniz. Yapılandırma dikey penceresi ayrıca özel DNS bölgesi oluşturma/güncelleştirme bilgilerini içerir. Varsayılan bölgeyi kullanmanızı öneririz `privatelink.file.core.windows.net` .

![Yapılandırma bölümünün ekran görüntüsü](media/storage-files-networking-endpoints/create-private-endpoint-2.png)

Özel uç noktayı oluşturmak için **gözden geçir + oluştur** ' a tıklayın. 

Sanal ağınızın içinde bir sanal makineniz varsa veya DNS iletmeyi [burada](storage-files-networking-dns.md)açıklandığı gibi yapılandırdıysanız, PowerShell, komut satırı veya terminalden (Windows, Linux veya MacOS için çalışır) aşağıdaki komutları çalıştırarak özel uç noktanızın doğru şekilde ayarlandığından test edebilirsiniz. `<storage-account-name>`Uygun depolama hesabı adıyla değiştirmeniz gerekir:

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
Depolama hesabınız için özel bir uç nokta oluşturmak üzere öncelikle depolama hesabınıza ve özel uç noktayı eklemek istediğiniz sanal ağ alt ağına bir başvuru almanız gerekir. ,,,, `<storage-account-resource-group-name>` `<storage-account-name>` `<vnet-resource-group-name>` Ve aşağıdaki gibi değiştirin `<vnet-name>` `<vnet-subnet-name>` :

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

Özel bir uç nokta oluşturmak için, depolama hesabına özel bir bağlantı hizmeti bağlantısı oluşturmanız gerekir. Özel bağlantı hizmeti bağlantısı, Özel uç noktanın oluşturulmasına yönelik bir giriştir. 

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

Azure özel DNS bölgesi oluşturmak, depolama hesabının özgün adını (örneğin, `storageaccount.file.core.windows.net` sanal ağın içindeki özel IP 'ye çözümleme) sağlar. Özel bir uç nokta oluşturma perspektifinden isteğe bağlı olsa da, bir AD Kullanıcı sorumlusu kullanarak Azure dosya paylaşımının bağlanması veya REST API aracılığıyla erişilmesi için açık bir şekilde gerekir.  

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

Artık özel DNS bölgesine bir başvurunuz olduğuna göre, depolama hesabınız için bir kayıt oluşturmanız gerekir.

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

Sanal ağınızın içinde bir sanal makineniz varsa veya [burada](storage-files-networking-dns.md)AÇıKLANDıĞı gibi DNS iletmeyi yapılandırdıysanız, Özel uç noktanızın aşağıdaki komutlarla doğru şekilde ayarlandığından test edebilirsiniz:

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
Depolama hesabınız için özel bir uç nokta oluşturmak üzere öncelikle depolama hesabınıza ve özel uç noktayı eklemek istediğiniz sanal ağ alt ağına bir başvuru almanız gerekir. ,,,, `<storage-account-resource-group-name>` `<storage-account-name>` `<vnet-resource-group-name>` Ve aşağıdaki gibi değiştirin `<vnet-name>` `<vnet-subnet-name>` :

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

Özel bir uç nokta oluşturmak için, önce alt ağın özel uç nokta ağ ilkesinin devre dışı olarak ayarlandığından emin olmanız gerekir. Ardından, komutuyla özel bir uç nokta oluşturabilirsiniz `az network private-endpoint create`

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

Azure özel DNS bölgesi oluşturmak, depolama hesabının özgün adını (örneğin, `storageaccount.file.core.windows.net` sanal ağın içindeki özel IP 'ye çözümleme) sağlar. Özel bir uç nokta oluşturma perspektifinden isteğe bağlı olsa da, bir AD Kullanıcı sorumlusu kullanarak Azure dosya paylaşımının bağlanması veya REST API aracılığıyla erişilmesi için açık bir şekilde gerekir.  

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

Artık özel DNS bölgesine bir başvurunuz olduğuna göre, depolama hesabınız için bir kayıt oluşturmanız gerekir.

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

Sanal ağınızın içinde bir sanal makineniz varsa veya [burada](storage-files-networking-dns.md)AÇıKLANDıĞı gibi DNS iletmeyi yapılandırdıysanız, Özel uç noktanızın aşağıdaki komutlarla doğru şekilde ayarlandığından test edebilirsiniz:

```bash
httpEndpoint=$(az storage account show \
        --resource-group $storageAccountResourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | \
    tr -d '"')

hostName=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint) | tr -d "/")
nslookup $hostName
```

Her şey başarıyla çalıştıysa aşağıdaki çıktıyı görmeniz gerekir, burada `192.168.0.5` sanal ağınızdaki özel uç noktanın özel IP adresidir. Yine de storageaccount.file.core.windows.net kullanarak, Privatelink yolunu dosya paylaşımınıza göre saymanız gerektiğini unutmayın.

```Output
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
storageaccount.file.core.windows.net      canonical name = storageaccount.privatelink.file.core.windows.net.
Name:   storageaccount.privatelink.file.core.windows.net
Address: 192.168.0.5
```

---

## <a name="restrict-access-to-the-public-endpoint"></a>Genel uç noktaya erişimi kısıtlama
Depolama hesabı güvenlik duvarı ayarlarını kullanarak genel uç noktaya erişimi kısıtlayabilirsiniz. Genel olarak, bir depolama hesabı için çoğu güvenlik duvarı ilkesi, ağ erişimini bir veya daha fazla sanal ağa kısıtlar. Bir depolama hesabına erişimi bir sanal ağla kısıtlamak için iki yaklaşım vardır:

- [Depolama hesabı için bir veya daha fazla özel uç nokta oluşturun](#create-a-private-endpoint) ve tüm erişimi genel uç noktaya sınırlayın. Bu, yalnızca istenen sanal ağlardan gelen trafiğin depolama hesabı içindeki Azure dosya paylaşımlarına erişebilmesini sağlar.
- Genel uç noktayı bir veya daha fazla sanal ağla sınırlayın. Bu, *hizmet uç noktaları*adlı sanal ağın bir özelliği kullanılarak işe yarar. Bir hizmet uç noktası aracılığıyla bir depolama hesabıyla trafiği kısıtladığınızda, hala genel IP adresi aracılığıyla depolama hesabına erişmeye devam edersiniz.

### <a name="restrict-all-access-to-the-public-endpoint"></a>Genel uç noktaya tüm erişimi kısıtla
Genel uç noktaya erişim kısıtlı olduğunda, depolama hesabına hala özel uç noktalar üzerinden erişilebilir. Aksi takdirde, depolama hesabının genel uç noktasına yönelik geçerli istekler reddedilir. 

# <a name="portal"></a>[Portal](#tab/azure-portal)
Genel uç noktaya erişimini kısıtlamak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **güvenlik duvarları ve sanal ağlar**' ı seçin.

Sayfanın üst kısmında **Seçili ağlar** radyo düğmesini seçin. Bu, genel uç noktanın kısıtlamasını denetlemeye yönelik bir dizi ayarı kaldırır. Güvenilen Microsoft hizmetlerinin, depolama hesabına erişmek için Azure Dosya Eşitleme gibi güvenilen ilk taraf Microsoft hizmetlerine izin vermek üzere **Bu hizmet hesabına erişmesine Izin verin** .

![Uygun kısıtlarla güvenlik duvarları ve sanal ağlar dikey penceresinin ekran görüntüsü](media/storage-files-networking-endpoints/restrict-public-endpoint-0.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Aşağıdaki PowerShell komutu, depolama hesabının genel uç noktasındaki tüm trafiği reddeder. Bu komutun `-Bypass` parametresi olarak ayarlandığını unutmayın `AzureServices` . Bu, Azure Dosya Eşitleme gibi güvenilen ilk taraf hizmetlerin, genel uç nokta aracılığıyla depolama hesabına erişmesi için izin verir.

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
Aşağıdaki CLı komutu, depolama hesabının genel uç noktasındaki tüm trafiği reddeder. Bu komutun `-bypass` parametresi olarak ayarlandığını unutmayın `AzureServices` . Bu, Azure Dosya Eşitleme gibi güvenilen ilk taraf hizmetlerin, genel uç nokta aracılığıyla depolama hesabına erişmesi için izin verir.

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

### <a name="restrict-access-to-the-public-endpoint-to-specific-virtual-networks"></a>Genel uç noktaya erişimi belirli sanal ağlara kısıtlama
Depolama hesabını belirli sanal ağlarla kısıtladığınızda, belirtilen sanal ağların içinden genel uç noktaya yönelik isteklere izin vermiş olursunuz. Bu, *hizmet uç noktaları*adlı sanal ağın bir özelliği kullanılarak işe yarar. Bu, Özel uç noktalarla veya olmadan kullanılabilir.

# <a name="portal"></a>[Portal](#tab/azure-portal)
Genel uç noktasını belirli sanal ağlara kısıtlamak istediğiniz depolama hesabına gidin. Depolama hesabının içindekiler tablosunda **güvenlik duvarları ve sanal ağlar**' ı seçin. 

Sayfanın üst kısmında **Seçili ağlar** radyo düğmesini seçin. Bu, genel uç noktanın kısıtlamasını denetlemeye yönelik bir dizi ayarı kaldırır. Ortak uç nokta aracılığıyla depolama hesabına erişmesine izin verilecek belirli sanal ağı seçmek için **+ var olan sanal ağı ekle** ' ye tıklayın. Bu, bu sanal ağ için bir sanal ağ ve alt ağ seçmeyi gerektirir. 

Güvenilen Microsoft hizmetlerinin, depolama hesabına erişmek için Azure Dosya Eşitleme gibi güvenilen ilk taraf Microsoft hizmetlerine izin vermek üzere **Bu hizmet hesabına erişmesine Izin verin** .

![Ortak uç nokta aracılığıyla depolama hesabına erişim izni verilen belirli bir sanal ağla güvenlik duvarları ve sanal ağlar dikey penceresinin ekran görüntüsü](media/storage-files-networking-endpoints/restrict-public-endpoint-1.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
Depolama hesabının genel uç noktasına erişimi hizmet uç noktaları kullanarak belirli sanal ağlara kısıtlamak için, önce depolama hesabı ve sanal ağ hakkında bilgi toplamanız gerekir. `<storage-account-resource-group>` `<storage-account-name>` `<vnet-resource-group-name>` `<vnet-name>` `<subnet-name>` Bu bilgileri toplamak için,,, ve girin.

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

Azure ağ dokusunun depolama hesabı genel uç noktasına ulaşmak için sanal ağdan gelen trafiğe izin verilmesi için, sanal ağın alt ağında `Microsoft.Storage` hizmet uç noktası açık olmalıdır. Aşağıdaki PowerShell komutları, `Microsoft.Storage` hizmet uç noktasını henüz yoksa alt ağa ekler.

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

Depolama hesabıyla trafiği kısıtlayan son adım bir ağ kuralı oluşturmak ve depolama hesabının ağ kural kümesine eklemektir.

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
Depolama hesabının genel uç noktasına erişimi hizmet uç noktaları kullanarak belirli sanal ağlara kısıtlamak için, önce depolama hesabı ve sanal ağ hakkında bilgi toplamanız gerekir. `<storage-account-resource-group>` `<storage-account-name>` `<vnet-resource-group-name>` `<vnet-name>` `<subnet-name>` Bu bilgileri toplamak için,,, ve girin.

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

Azure ağ dokusunun depolama hesabı genel uç noktasına ulaşmak için sanal ağdan gelen trafiğe izin verilmesi için, sanal ağın alt ağında `Microsoft.Storage` hizmet uç noktası açık olmalıdır. Aşağıdaki CLı komutları, `Microsoft.Storage` hizmet uç noktasını henüz yoksa alt ağa ekler.

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

Depolama hesabıyla trafiği kısıtlayan son adım bir ağ kuralı oluşturmak ve depolama hesabının ağ kural kümesine eklemektir.

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
- [Azure dosyaları ağ iletişimi konuları](storage-files-networking-overview.md)
- [Azure Dosyalar için DNS iletmeyi yapılandırma](storage-files-networking-dns.md)
- [Azure dosyaları için S2S VPN 'i yapılandırma](storage-files-configure-s2s-vpn.md)
