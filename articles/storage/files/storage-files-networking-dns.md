---
title: Azure Dosyaları için DNS yönlendirmeyapılandırma | Microsoft Dokümanlar
description: Azure Dosyaları için ağ seçeneklerine genel bakış.
author: roygara
ms.service: storage
ms.topic: overview
ms.date: 3/19/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 35dfbcb274721049f2160719222ca89038c93356
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80082503"
---
# <a name="configuring-dns-forwarding-for-azure-files"></a>Azure Dosyaları için DNS yönlendirmeyi yapılandırma
Azure Dosyaları, dosya paylaşımlarınızı içeren depolama hesapları için özel uç noktalar oluşturmanıza olanak tanır. Birçok farklı uygulama için yararlı olsa da, özel uç noktalar özel bakışlı kullanarak VPN veya ExpressRoute bağlantısını kullanarak şirket içi ağınızdaki Azure dosya paylaşımlarınıza bağlanmak için özellikle yararlıdır. 

Depolama hesabınıza olan bağlantıların ağ tünelinizüzerinden geçebilmesi için, depolama hesabınızın tam nitelikli alan adı (FQDN) özel bitiş noktanızın özel IP adresine giderilmelidir. Bunu başarmak için, depolama bitiş noktası son`core.windows.net` ekini (genel bulut bölgeleri için) sanal ağınızdan erişilebilen Azure özel DNS hizmetine iletmeniz gerekir. Bu kılavuz, depolama hesabınızın özel bitiş noktası IP adresine düzgün bir şekilde çözümlenecek Şekilde DNS iletmenin nasıl ayarlanacağını ve yapılandırAcağını gösterir.

Bu makalede açıklanan adımları tamamlamadan önce [Azure Dosyaları dağıtımı için Planlama](storage-files-planning.md) ve Azure Dosyaları ağ la ilgili [konuları](storage-files-networking-overview.md) okumanızı öneririz.

## <a name="overview"></a>Genel Bakış
Azure Dosyaları, Azure dosya paylaşımları erişmek için iki ana uç nokta türü sağlar: 
- Genel BIR IP adresine sahip olan ve dünyanın her yerinden erişilebilen genel uç noktalar.
- Sanal ağ içinde bulunan ve bu sanal ağın adres alanı içinde özel bir IP adresine sahip olan özel uç noktalar.

Azure depolama hesabında ortak ve özel uç noktalar bulunur. Depolama hesabı, birden çok dosya paylaşımının yanı sıra blob kapsayıcıları veya kuyruklar gibi diğer depolama kaynaklarını dağıtabileceğiniz paylaşılan bir depolama havuzunun yanı sıra yönetim yapısıdır.

Her depolama hesabının tam nitelikli bir etki alanı adı (FQDN) vardır. Genel bulut bölgeleri için bu FQDN, `storageaccount` depolama hesabının adının bulunduğu deseni `storageaccount.file.core.windows.net` izler. SMB kullanarak iş istasyonunuzdaki payı montaj gibi bu ada karşı istekte bulunduğunda, işletim sisteminiz tam nitelikli alan adını SMB isteklerini göndermek için kullanabileceği bir IP adresine çözümlemek için bir DNS araması gerçekleştirir.

Varsayılan olarak, `storageaccount.file.core.windows.net` ortak uç noktanın IP adresine gider. Depolama hesabının ortak bitiş noktası, diğer birçok depolama hesabının ortak uç noktalarını barındıran bir Azure depolama kümesinde barındırılır. Özel bir bitiş noktası oluşturduğunuzda, özel bir DNS bölgesi eklendiğinde eklenmiş olduğu `storageaccount.file.core.windows.net` sanal ağa bağlanır ve depolama hesabınızın özel bitiş noktasının özel IP adresiiçin bir Kayıt girişine CNAME kaydı eşlemesi ile bağlanır. Bu, sanal ağ `storageaccount.file.core.windows.net` içinde FQDN'yi kullanmanıza ve özel bitiş noktasının IP adresine kadar çözülmesini sağlar.

Nihai hedefimiz, VPN veya ExpressRoute bağlantısı gibi bir ağ tüneli kullanarak depolama hesabında barındırılan Azure dosya paylaşımlarına şirket içinde erişmek olduğundan, şirket içi DNS sunucularınızı Azure'a yapılan istekleri iletmek için yapılandırmanız gerekir Azure özel DNS hizmetine dosya hizmeti. Bunu başarmak için, Azure sanal ağınızda `*.core.windows.net` barındırılan bir DNS sunucusuna *koşullu iletme* (veya ABD Hükümeti, Almanya veya Çin ulusal bulutları için uygun depolama bitiş noktası soneki) ayarlamanız gerekir. Bu DNS sunucusu daha sonra, depolama hesabının tam nitelikli alan adını uygun özel IP adresine çözecek olan isteği Azure'un özel DNS hizmetine özyinelemeli olarak iletir.

Azure Dosyaları için DNS iletmeyapılandırma, istekleri iletmek için bir DNS sunucusu barındırmak için sanal bir makine çalıştırmayı gerektirir, ancak bu sanal ağınızda barındırılan tüm Azure dosya paylaşımları için tek seferlik bir adımdır. Ayrıca, bu Azure Dosyaları için özel bir gereklilik değildir - şirket içinde erişmek istediğiniz özel uç noktaları destekleyen herhangi bir Azure hizmeti, bu kılavuzda yapılandıracağınız DNS iletmeden yararlanabilir: Azure Blob depolama, SQL Azure, Cosmos DB, Vb. 

Bu kılavuz, Azure depolama bitiş noktası için DNS iletme yapılandırma adımlarını gösterir, bu nedenle Azure Dosyaları'na ek olarak, diğer Tüm Azure depolama hizmetleri (Azure Blob depolama alanı, Azure Tablo depolama alanı, Azure Sıra depolama, vb.) için DNS ad çözümleme istekleri Azure'un özel DNS hizmetine iletilebilir. İstenirse diğer Azure hizmetleri için ek uç noktalar da eklenebilir. Şirket içi DNS sunucularınıza geri iletilmesi de yapılandırılacak ve sanal ağınızdaki (DFS-N sunucusu gibi) bulut kaynaklarının şirket içi makine adlarını çözmesine olanak tanır. 

## <a name="prerequisites"></a>Ön koşullar
DNS iletmesini Azure Dosyaları'na atamadan önce aşağıdaki adımları tamamlamış olmanız gerekir:

- Monte etmek istediğiniz bir Azure dosya paylaşımı içeren bir depolama hesabı. Depolama hesabı ve Azure dosya paylaşımı nasıl oluşturulacak öğrenmek için [bkz.](storage-how-to-create-file-share.md)
- Depolama hesabı için özel bir bitiş noktası. Azure Dosyaları için özel bir bitiş noktası oluşturmayı öğrenmek için [bkz.](storage-files-networking-endpoints.md#create-a-private-endpoint)
- Azure PowerShell modülünün [en son sürümü.](https://docs.microsoft.com/powershell/azure/install-az-ps)

> [!Important]  
> Bu kılavuz, Windows Server içindeki DNS sunucusunu şirket içi ortamınızda kullandığınızı varsayar. Bu kılavuzda açıklanan tüm adımlar, sadece Windows DNS Sunucusu ile değil, herhangi bir DNS sunucusuyla da mümkündür.

## <a name="manually-configuring-dns-forwarding"></a>DNS iletmeişlemlerini el ile yapılandırma
Azure sanal ağınızda zaten DNS sunucularınız varsa veya kuruluşunuzun kullandığı metodolojiye göre kendi sanal makinelerinizi DNS sunucusu olarak dağıtmayı tercih ediyorsanız, DNS'yi yerleşik DNS sunucusuyla el ile yapılandırabilirsiniz PowerShell cmdlets.

Şirket içi DNS sunucularınızda, koşullu `Add-DnsServerConditionalForwarderZone`bir iletme oluşturun. Bu koşullu iletme, trafiği Azure'a düzgün bir şekilde iletmede etkili olabilmesi için tüm şirket içi DNS sunucularınızda dağıtılmalıdır. Ortamınız `<azure-dns-server-ip>` için uygun IP adreslerini değiştirmeyi unutmayın.

```powershell
$vnetDnsServers = "<azure-dns-server-ip>", "<azure-dns-server-ip>"

$storageAccountEndpoint = Get-AzContext | `
    Select-Object -ExpandProperty Environment | `
    Select-Object -ExpandProperty StorageEndpointSuffix

Add-DnsServerConditionalForwarderZone `
        -Name $storageAccountEndpoint `
        -MasterServers $vnetDnsServers
```

Azure sanal ağınızdaki DNS sunucularında, depolama hesabı DNS bölgesine yönelik isteklerin ayrılmış IP adresinin `168.63.129.16`önündeki Azure özel DNS hizmetine yönlendirilmesi için bir iletme aracı yerleştirmeniz de gerekir. (Komutları farklı `$storageAccountEndpoint` bir PowerShell oturumunda çalıştırıyorsanız doldurmayı unutmayın.)

```powershell
Add-DnsServerConditionalForwarderZone `
        -Name $storageAccountEndpoint `
        -MasterServers "168.63.129.16"
```

## <a name="using-the-azure-files-hybrid-module-to-configure-dns-forwarding"></a>DNS iletme yapılandırmak için Azure Files Karma modüllerini kullanma
DNS yönlendirmeyi mümkün olduğunca kolay hale getirmek için Azure Files Karma modülünde otomasyon sağladık. Bu modülde DNS'yi manipüle etmek için sağlanan cmdletler, Azure sanal ağınızda DNS sunucularını dağıtmanıza ve şirket içi DNS sunucularınızı güncellemenize yardımcı olur. 

Azure Files Karma modüllerini hiç kullanmadıysanız, önce bu modülü iş istasyonunuzun üzerine yüklemeniz gerekir. Azure Files Hybrid PowerShell modülünün [en son sürümünü](https://github.com/Azure-Samples/azure-files-samples/releases) indirin:

```PowerShell
# Unzip the downloaded file
Expand-Archive -Path AzFilesHybrid.zip

# Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\AzFilesHybrid\CopyToPSPath.ps1 

# Import AzFilesHybrid module
Import-Module -Name AzFilesHybrid
```

DNS iletme çözümünün dağıtılmasının iki adımı vardır: Hangi Azure hizmetlerine iletmek istediğinizi ve DNS iletmecilerin gerçek dağıtımını tanımlayan bir DNS iletme kuralı kümesi oluşturur. 

Aşağıdaki örnek, talepleri depolama hesabına, kapsamlı istekleri Azure Dosyaları'na, Azure Blob depolama alanına, Azure Tablo depolama alanına ve Azure Sıra depolama alanına iletin. İstenirseniz, `-AzureEndpoints` `New-AzDnsForwardingRuleSet` cmdlet parametresi aracılığıyla kurala ek Azure hizmeti için iletme ekleyebilirsiniz. Çevreniz `<virtual-network-resource-group>`için `<virtual-network-name>`uygun `<subnet-name>` değerleri değiştirmeyi ve değiştirmeyi unutmayın.

```PowerShell
# Create a rule set, which defines the forwarding rules
$ruleSet = New-AzDnsForwardingRuleSet -AzureEndpoints StorageAccountEndpoint

# Deploy and configure DNS forwarders
New-AzDnsForwarder `
        -DnsForwardingRuleSet $ruleSet `
        -VirtualNetworkResourceGroupName "<virtual-network-resource-group>" `
        -VirtualNetworkName "<virtual-network-name>" `
        -VirtualNetworkSubnetName "<subnet-name>"
```

Ayrıca birkaç ek parametre sağlamanın yararlı/gerekli olduğunu görebilirsiniz:

| Parametre adı | Tür | Açıklama |
|----------------|------|-------------|
| `DnsServerResourceGroupName` | `string` | Varsayılan olarak, DNS sunucuları sanal ağla aynı kaynak grubuna dağıtılır. Bu islenmiyorsa, bu parametre, dağıtılmak üzere alternatif bir kaynak grubu seçmeniz için izin verir. |
| `DnsForwarderRootName` | `string` | Varsayılan olarak, Azure'da dağıtılan DNS sunucularında `DnsFwder-*`yıldız işaretinin bir yineleyici tarafından doldurulduğu adlar vardır. Bu parametre bu adın kökünü değiştirir `DnsFwder`(örn. ). |
| `VmTemporaryPassword` | `SecureString` | Varsayılan olarak, bir VM'nin etki alanına katılmadan önce sahip olduğu geçici varsayılan hesap için rasgele bir parola seçilir. Etki alanı birleştikten sonra varsayılan hesap devre dışı bırakılır. |
| `DomainToJoin` | `string` | DNS VM(ler) katılmak için etki alanı. Varsayılan olarak, bu etki alanı cmdlets çalıştırdığınız bilgisayarın etki alanına göre seçilir. |
| `DnsForwarderRedundancyCount` | `int` | Sanal ağınız için dağıtılabilen DNS VM sayısı. Varsayılan olarak, `New-AzDnsForwarder` artıklığı sağlamak için Azure sanal ağınızdaki iki DNS sunucusunun kullanılabilirlik kümesinde dağıtılır. Bu sayı istesildiği gibi değiştirilebilir. |
| `OnPremDnsHostNames` | `HashSet<string>` | İlericileri oluşturmak için şirket içi DNS ana bilgisayar adlarının el ile belirtilen listesi. Bu parametre, el ile belirtilen DNS adlarına sahip bir istemci aralığınız olduğunda olduğu gibi, tüm şirket içi DNS sunucularında iletmeleri uygulamak istemediğiniz zaman yararlıdır. |
| `Credential` | `PSCredential` | DNS sunucularını güncellerken kullanılacak bir kimlik bilgisi. Bu, oturum açtığınız kullanıcı hesabıDNS ayarlarını değiştirme izinleri yoksa kullanışlıdır. |
| `SkipParentDomain` | `SwitchParameter` | Varsayılan olarak, DNS iletmecileri ortamınızda bulunan en üst düzey etki alanına uygulanır. Örneğin, bir `northamerica.corp.contoso.com` alt etki `corp.contoso.com`alanı ise, ile ilişkili DNS sunucuları `corp.contoso.com`için iletme oluşturulur. Bu parametre iletmecilerin ''de `northamerica.corp.contoso.com`oluşturulmasına neden olur. |

## <a name="confirm-dns-forwarders"></a>DNS iletmelerini onaylayın
DNS iletmelerinin başarıyla uygulanıp uygulanmadığını test etmeden önce, yerel iş istasyonunuzdaki `Clear-DnsClientCache`DNS önbelleğini kullanarak temizlemenizi öneririz. Depolama hesabınızın tam nitelikli alan adını başarıyla çözüp çözebildiğinizi test etmek için, `Resolve-DnsName` `nslookup`

```powershell
# Replace storageaccount.file.core.windows.net with the appropriate FQDN for your storage account.
# Note the proper suffix (core.windows.net) depends on the cloud your deployed in.
Resolve-DnsName -Name storageaccount.file.core.windows.net
```

Ad çözümlemesi başarılı olursa, çözülmüş IP adresinin depolama hesabınızın IP adresiyle eşleştiğini görmeniz gerekir.

```Output
Name                              Type   TTL   Section    NameHost
----                              ----   ---   -------    --------
storageaccount.file.core.windows. CNAME  29    Answer     csostoracct.privatelink.file.core.windows.net
net

Name       : storageaccount.privatelink.file.core.windows.net
QueryType  : A
TTL        : 1769
Section    : Answer
IP4Address : 192.168.0.4
```

Vpn veya ExpressRoute bağlantısı zaten ayarladıysanız, depolama `Test-NetConnection` hesabınıza başarılı bir Şekilde TCP bağlantısı yapıldığını görmek için de kullanabilirsiniz.

```PowerShell
Test-NetConnection -ComputerName storageaccount.file.core.windows.net -CommonTCPPort SMB
```

## <a name="see-also"></a>Ayrıca bkz.
- [Azure Dosyaları dağıtımı planlama](storage-files-planning.md)
- [Azure Dosyaları ağ la ilgili hususlar](storage-files-networking-overview.md)
- [Azure Dosyaları ağ uç noktalarını yapılandırma](storage-files-networking-endpoints.md)