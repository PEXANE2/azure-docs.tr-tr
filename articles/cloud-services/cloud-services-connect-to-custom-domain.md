---
title: Bulut Hizmetini özel bir Etki Alanı Denetleyicisine bağlayın | Microsoft Dokümanlar
description: PowerShell ve AD Etki Alanı Uzantısını kullanarak web/çalışan rollerinizi özel bir AD Etki Alanına nasıl bağlayabilirsiniz öğrenin
services: cloud-services
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 07/18/2017
ms.author: tagore
ms.openlocfilehash: d40e392984d2675c748bda00c61cdaeb1c0932da
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75387029"
---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure Bulut Hizmetleri Rollerini Azure'da barındırılan özel bir AD Etki Alanı Denetleyicisine bağlama
Öncelikle Azure'da bir Sanal Ağ (VNet) kuracağız. Daha sonra VNet'e Bir Active Directory Etki Alanı Denetleyicisi (Azure Sanal Makinede barındırılan) ekleriz. Daha sonra, önceden oluşturulmuş VNet'e varolan bulut hizmeti rollerini ekleyip etki alanı denetleyicisine bağlarız.

Başlamadan önce, akılda tutulması gereken birkaç şey:

1. Bu öğretici PowerShell'i kullanır, bu nedenle Azure PowerShell'in yüklü ve kullanıma hazır olduğundan emin olun. Azure PowerShell'i ayarlama konusunda yardım almak [için Azure PowerShell'i nasıl yükleyip yapılandırılatırsınız.](/powershell/azure/overview)
2. AD Etki Alanı Denetleyiciniz ve Web/İşçi Rolü örneklerinizin VNet'te olması gerekir.

Bu adım adım kılavuzu izleyin ve herhangi bir sorunla karşılaştıysanız, makalenin sonunda bize bir yorum bırakın. Birisi size geri alacak (evet, biz yorum okumak yok).

Bulut hizmeti tarafından başvurulan ağ klasik bir **sanal ağ**olmalıdır.

## <a name="create-a-virtual-network"></a>Sanal Ağ Oluşturma
Azure portalını veya PowerShell'i kullanarak Azure'da sanal ağ oluşturabilirsiniz. Bu öğretici için PowerShell kullanılır. Azure portalını kullanarak sanal ağ oluşturmak için [bkz.](../virtual-network/quick-create-portal.md) Makale sanal ağ (Kaynak Yöneticisi) oluşturmayı kapsar, ancak bulut hizmetleri için bir sanal ağ (Klasik) oluşturmanız gerekir. Bunu yapmak için, portalda, **bir kaynak oluştur'u,** **Arama** kutusuna *sanal ağ* yazın'ı seçin ve ardından **Enter**tuşuna basın. Arama sonuçlarında, **Her şey**altında, **Sanal ağ**seçin. **Dağıtım modelini seçin,** **Klasik'i**seçin ve ardından **Oluştur'u**seçin. Daha sonra makaledeki adımları izleyebilirsiniz.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Sanal Makine Oluşturma
Sanal Ağı kurmayı tamamladıktan sonra bir AD Etki Alanı Denetleyicisi oluşturmanız gerekir. Bu öğretici için, Azure Sanal Makine'de bir AD Etki Alanı Denetleyicisi kuracağız.

Bunu yapmak için, aşağıdaki komutları kullanarak PowerShell üzerinden sanal bir makine oluşturun:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Sanal Makinenizi Etki Alanı Denetleyicisine Tanıtın
Sanal Makineyi AD Etki Alanı Denetleyicisi olarak yapılandırmak için VM'de oturum açmanız ve yapılandırmanız gerekir.

VM'de oturum açmak için RDP dosyasını PowerShell üzerinden alabilir ve aşağıdaki komutları kullanabilirsiniz:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

VM'de oturum açtıktan sonra, [müşteriNIZIN AD Etki Alanı Denetleyicisini nasıl kurabileceğinize](https://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx)ilişkin adım adım kılavuzunu izleyerek Sanal Makinenizi AD Etki Alanı Denetleyicisi olarak ayarlayın.

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Bulut Hizmetinizi Sanal Ağa Ekleyin
Ardından, bulut hizmeti dağıtımınızı yeni VNet'e eklemeniz gerekir. Bunu yapmak için Visual Studio'yu veya seçtiğiniz editörü kullanarak ilgili bölümleri cscfg'nize ekleyerek bulut hizmeti cscfg'nizi değiştirin.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Ardından bulut hizmetleri projenizi oluşturun ve Azure'a dağıtın. Bulut hizmetleri paketinizi Azure'a dağıtma konusunda yardım almak için [Bulut Hizmeti Oluşturma ve Dağıtma hakkında](cloud-services-how-to-create-deploy-portal.md)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Web/çalışan rollerinizi etki alanına bağlama
Bulut hizmeti projeniz Azure'da dağıtıldıktan sonra, AD Etki Alanı Uzantısı'nı kullanarak rol örneklerinizi özel AD etki alanına bağlayın. AD Etki Alanı Uzantısı'nı mevcut bulut hizmetleri dağıtımınıza eklemek ve özel etki alanına katılmak için PowerShell'de aşağıdaki komutları uygulayın:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

İşte bu kadar.

Bulut hizmetleriniz özel etki alanı denetleyicinize katılmalı. AD Etki Alanı Uzantısını yapılandırmak için mevcut olan farklı seçenekler hakkında daha fazla bilgi edinmek istiyorsanız PowerShell yardımını kullanın. Birkaç örnek:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```



