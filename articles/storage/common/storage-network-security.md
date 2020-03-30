---
title: Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma | Microsoft Dokümanlar
description: Depolama hesabınız için katmanlı ağ güvenliğini yapılandırın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/21/2020
ms.author: tamram
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: 77ad8579f31ce900a67e2ba3ddc53a5b034b6d42
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454678"
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks"></a>Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma

Azure Depolama, katmanlı bir güvenlik modeli sağlar. Bu model, kullanılan ağların türüne ve alt kümesine bağlı olarak uygulamalarınızın ve kurumsal ortamlarınızın talep ettiği depolama hesaplarınıza erişim düzeyini güvenli hale almanızı ve denetlemenizi sağlar. Ağ kuralları yapılandırıldığında, yalnızca belirtilen ağ kümesi üzerinden veri isteyen uygulamalar bir depolama hesabına erişebilir. Depolama hesabınıza erişimi, belirtilen IP adreslerinden, IP aralıklarından veya Azure Sanal Ağı'ndaki (VNet) alt ağlar listesinden kaynaklanan isteklerle sınırlandırabilirsiniz.

Depolama hesapları, internet üzerinden erişilebilen genel bir bitiş noktasına sahiptir. Ayrıca, VNet'inizden depolama hesabınıza özel bir IP adresi atayan ve VNet'iniz ile depolama hesabınız arasındaki tüm trafiği özel bir bağlantı üzerinden güvence altına alan [depolama hesabınız için Özel Uç Noktalar](storage-private-endpoints.md)da oluşturabilirsiniz. Azure depolama güvenlik duvarı, depolama hesabınızın genel bitiş noktası için erişim denetimi erişimi sağlar. Güvenlik duvarını, özel uç noktaları kullanırken ortak bitiş noktası üzerinden tüm erişimi engellemek için de kullanabilirsiniz. Depolama güvenlik duvarı yapılandırmanız, belirli güvenilir Azure platform hizmetlerinin depolama hesabına güvenli bir şekilde erişmesini de sağlar.

Ağ kuralları etkin olduğunda bir depolama hesabına erişen bir uygulama, istek için uygun yetkilendirme yi gerektirir. Yetkilendirme, blob'lar ve kuyruklar için Azure Active Directory (Azure AD) kimlik bilgileriyle, geçerli bir hesap erişim anahtarıyla veya SAS belirteciyle desteklenir.

> [!IMPORTANT]
> Talepler Azure Sanal Ağı 'nda (VNet) veya izin verilen genel IP adreslerinden gelen bir hizmetten kaynaklanmadığı sürece, depolama hesabınız için güvenlik duvarı kurallarını açmak varsayılan olarak gelen veri isteklerini engeller. Engellenen istekler arasında diğer Azure hizmetlerinden, Azure portalından, günlük ve metrik hizmetleri gibi hizmetlerden gelenler yer almaktadır.
>
> Hizmet örneğini barındıran alt ağdan gelen trafiğe izin vererek VNet içinden çalışan Azure hizmetlerine erişim izni verebilirsiniz. Ayrıca, aşağıda açıklanan [Özel Durumlar](#exceptions) mekanizması aracılığıyla sınırlı sayıda senaryoyu da etkinleştirebilirsiniz. Azure portalı üzerinden depolama hesabındaki verilere erişmek için, ayarladığınız güvenilir sınır (IP veya VNet) içindeki bir makinede olmanız gerekir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="scenarios"></a>Senaryolar

Depolama hesabınızı n güvenliğini sağlamak için, öncelikle varsayılan olarak genel bitiş noktasındaki tüm ağlardan (internet trafiği dahil) trafiğe erişimi reddetmek için bir kural yapılandırmanız gerekir. Ardından, belirli VNet'lerden trafiğe erişim sağlayan kuralları yapılandırmanız gerekir. Ayrıca, belirli internet IP adresi aralıklarından trafiğe erişim sağlamak için kuralları yapılandırarak belirli internet veya şirket içi istemcilerden bağlantılar alabilirsiniz. Bu yapılandırma, uygulamalarınız için güvenli bir ağ sınırı oluşturmanıza olanak tanır.

Belirli sanal ağlardan ve aynı depolama hesabındaki genel IP adresi aralıklarından erişime izin veren güvenlik duvarı kurallarını birleştirebilirsiniz. Depolama güvenlik duvarı kuralları, varolan depolama hesaplarına veya yeni depolama hesapları oluştururken uygulanabilir.

Depolama güvenlik duvarı kuralları, depolama hesabının genel bitiş noktasına uygulanır. Bir depolama hesabının özel uç noktaları için trafiğe izin vermek için herhangi bir güvenlik duvarı erişim kuralına gerek yoktur. Özel bir bitiş noktası oluşturulmasını onaylama işlemi, özel bitiş noktasını barındıran alt ağdan trafiğe örtülü erişim sağlar.

AĞ kuralları, REST ve SMB dahil olmak üzere Azure depolamasına yönelik tüm ağ protokollerinde uygulanır. Azure portalı, Depolama Gezgini ve AZCopy gibi araçları kullanarak verilere erişmek için açık ağ kurallarının yapılandırılması gerekir.

Ağ kuralları uygulandıktan sonra, tüm istekler için uygulanır. Belirli bir IP adresine erişim sağlayan SAS belirteçleri, belirteç sahibinin erişimini sınırlamaya hizmet eder, ancak yapılandırılmış ağ kurallarının ötesinde yeni erişim vermez.

Sanal makine disk trafiği (montaj ve montaj sökme işlemleri ve disk IO dahil) ağ kurallarından etkilenmez. Sayfa blobs REST erişim ağ kuralları ile korunmaktadır.

Klasik depolama hesapları güvenlik duvarlarını ve sanal ağları desteklemez.

Bir özel durum oluşturarak yedekleme ve geri yükleme Için uygulanan ağ kuralları ile depolama hesaplarında yönetilmeyen diskler kullanabilirsiniz. Bu işlem, bu makalenin [Özel Durumlar](#exceptions) bölümünde belgelenmiştir. Güvenlik duvarı özel durumları, Azure tarafından zaten yönetildiği için yönetilen diskler için geçerli değildir.

## <a name="change-the-default-network-access-rule"></a>Varsayılan ağ erişim kuralını değiştirme

Varsayılan olarak, depolama hesapları herhangi bir ağ üzerindeki istemcilerden gelen bağlantıları kabul eder. Seçili ağlara erişimi sınırlamak için öncelikle varsayılan eylemi değiştirmeniz gerekir.

> [!WARNING]
> Ağ kurallarında değişiklikler yapmak uygulamalarınızın Azure Depolamaya bağlanma yeteneğini etkileyebilir. Erişim sağlayan belirli ağ kuralları da uygulanmadığı sürece, varsayılan ağ kuralının verilere tüm erişimi **engellemeyi** **reddetmesi** için ayarlanması. Varsayılan kuralı erişimi reddedecek şekilde değiştirmeden önce ağ kurallarını kullanarak izin verilen ağlara erişim verdiğinizden emin olun.

### <a name="managing-default-network-access-rules"></a>Varsayılan ağ erişim kurallarını yönetme

Azure portalı, PowerShell veya CLIv2 aracılığıyla depolama hesapları için varsayılan ağ erişim kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portalında

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. Güvenlik Duvarları ve sanal ağlar adı verilen ayarlar **menüsüne**tıklayın.

1. Varsayılan olarak erişimi reddetmek için, **Seçili ağlardan**erişime izin vermeyi seçin. Tüm ağlardan gelen trafiğe izin vermek için **Tüm ağlardan** erişime izin vermeyi seçin.

1. Değişikliklerinizi uygulamak için **Kaydet**’e tıklayın.

#### <a name="powershell"></a>PowerShell

1. Azure [PowerShell'i](/powershell/azure/install-Az-ps) yükleyin ve [oturum açın.](/powershell/azure/authenticate-azureps)

1. Depolama hesabı için varsayılan kuralın durumunu görüntüleyin.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
    ```

1. Varsayılan olarak ağ erişimini reddetmek için varsayılan kuralı ayarlayın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
    ```

1. Varsayılan olarak ağ erişimine izin verecek varsayılan kuralı ayarlayın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
    ```

#### <a name="cliv2"></a>CLIv2

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın.](/cli/azure/authenticate-azure-cli)

1. Depolama hesabı için varsayılan kuralın durumunu görüntüleyin.

    ```azurecli
    az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
    ```

1. Varsayılan olarak ağ erişimini reddetmek için varsayılan kuralı ayarlayın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --default-action Deny
    ```

1. Varsayılan olarak ağ erişimine izin verecek varsayılan kuralı ayarlayın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --default-action Allow
    ```

## <a name="grant-access-from-a-virtual-network"></a>Sanal ağdan erişim izni verme

Depolama hesaplarını yalnızca belirli alt ağlardan erişime izin verecek şekilde yapılandırabilirsiniz. İzin verilen alt ağlar, aynı abonelikteki bir VNet'e veya farklı bir Azure Etkin Dizin kiracısına ait abonelikler de dahil olmak üzere farklı bir abonelikteki lere ait olabilir.

VNet'teki Azure Depolama için [Hizmet bitiş noktasını](/azure/virtual-network/virtual-network-service-endpoints-overview) etkinleştirin. Hizmet bitiş noktası, VNet'ten Azure Depolama hizmetine en uygun yoldan trafiği yönlendirir. Alt ağ ve sanal ağ kimlikleri de her istek ile iletilir. Yöneticiler daha sonra, bir VNet'teki belirli alt ağlardan istek alınmasına izin veren depolama hesabı için ağ kurallarını yapılandırabilir. Bu ağ kuralları yla erişim verilen istemcilerin verilere erişmek için depolama hesabının yetkilendirme gereksinimlerini karşılamaya devam etmesi gerekir.

Her depolama hesabı, [IP ağ kurallarıyla](#grant-access-from-an-internet-ip-range)birleştirilebilen en fazla 100 sanal ağ kuralını destekler.

### <a name="available-virtual-network-regions"></a>Kullanılabilir sanal ağ bölgeleri

Genel olarak, hizmet bitiş noktaları aynı Azure bölgesindeki sanal ağlar ve hizmet örnekleri arasında çalışır. Azure Depolama ile hizmet bitiş noktalarını kullanırken, bu kapsam [eşleştirilmiş bölgeyi](/azure/best-practices-availability-paired-regions)içerecek şekilde büyür. Hizmet bitiş noktaları, bölgesel bir arıza sırasında sürekliliğe ve salt okunur coğrafi yedekli depolama (RA-GRS) örneklerine erişime olanak sağlar. Sanal ağdan depolama hesabına erişim sağlayan ağ kuralları da herhangi bir RA-GRS örneğine erişim sağlar.

Bölgesel bir kesinti sırasında olağanüstü durum kurtarma için planlar yaparken, önceden eşleştirilmiş bölgede VNets oluşturmanız gerekir. Bu alternatif sanal ağlardan erişim sağlayan ağ kurallarıyla Azure Depolama için hizmet bitiş noktalarını etkinleştirin. Ardından bu kuralları coğrafi yedekli depolama hesaplarınıza uygulayın.

> [!NOTE]
> Hizmet bitiş noktaları, sanal ağın ve belirlenen bölge çiftinin dışındaki trafik için geçerli değildir. Sanal ağlardan erişim sağlayan ağ kurallarını yalnızca bir depolama hesabının birincil bölgesindeki veya belirlenen eşleştirilmiş bölgedeki depolama hesaplarına uygulayabilirsiniz.

### <a name="required-permissions"></a>Gerekli izinler

Sanal ağ kuralını bir depolama hesabına uygulamak için, kullanıcının alt ağlar için uygun izinlere sahip olması gerekir. Gereken izin *Bir Subnet'e Hizmete Katılma'dır* ve *Depolama Hesabı Oluşturkanı* rolüne dahildir. Özel rol tanımlarına da eklenebilir.

Depolama hesabı ve erişim verilen sanal ağlar, farklı bir Azure AD kiracısının parçası olan abonelikler de dahil olmak üzere farklı aboneliklerde olabilir.

> [!NOTE]
> Farklı bir Azure Etkin Dizin kiracısının parçası olan sanal ağlardaki alt ağlara erişim sağlayan kuralların yapılandırması şu anda yalnızca Powershell, CLI ve REST API'leri aracılığıyla desteklenir. Bu tür kurallar Azure portalı üzerinden yapılandırılamaz, ancak portalda görüntülenebilir.

### <a name="managing-virtual-network-rules"></a>Sanal ağ kurallarını yönetme

Azure portalı, PowerShell veya CLIv2 aracılığıyla depolama hesapları için sanal ağ kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portalında

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. Güvenlik Duvarları ve sanal ağlar adı verilen ayarlar **menüsüne**tıklayın.

1. **Seçili ağlardan**erişime izin vermek için seçtiğinizden denetlemeyin.

1. Yeni bir ağ kuralına sahip sanal ağa erişim sağlamak için, **Sanal ağlar**altında, **Varolan sanal ağ ekle'yi**tıklatın, Sanal **ağlar** ve **Alt Ağlar** seçeneklerini seçin ve sonra **Ekle'yi**tıklatın. Yeni bir sanal ağ oluşturmak ve ona erişim sağlamak için **yeni sanal ağ ekle'yi**tıklatın. Yeni sanal ağ oluşturmak için gerekli bilgileri sağlayın ve ardından **Oluştur'u**tıklatın.

    > [!NOTE]
    > Azure Depolama için bir hizmet bitiş noktası seçili sanal ağ ve alt ağlar için önceden yapılandırılmamışsa, bu işlemin bir parçası olarak yapılandırabilirsiniz.
    >
    > Şu anda, kural oluşturma sırasında seçim için yalnızca aynı Azure Etkin Dizin kiracısına ait sanal ağlar gösterilir. Başka bir kiracıya ait sanal ağdaki bir alt ağa erişim sağlamak için lütfen Powershell, CLI veya REST API'lerini kullanın.

1. Sanal ağ veya alt ağ kuralını kaldırmak için ... sanal ağ veya alt ağ için bağlam menüsünü açmak için **...'u** tıklatın ve **Kaldır'ı**tıklatın.

1. Değişikliklerinizi uygulamak için **Kaydet**’e tıklayın.

#### <a name="powershell"></a>PowerShell

1. Azure [PowerShell'i](/powershell/azure/install-Az-ps) yükleyin ve [oturum açın.](/powershell/azure/authenticate-azureps)

1. Sanal ağ kurallarını listele.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
    ```

1. Varolan bir sanal ağ da ve alt ağda Azure Depolama için hizmet bitiş noktasını etkinleştirin.

    ```powershell
    Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.0.0.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzVirtualNetwork
    ```

1. Sanal ağ ve alt ağ için ağ kuralı ekleyin.

    ```powershell
    $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id
    ```

    > [!TIP]
    > Başka bir Azure AD kiracısına ait bir VNet'teki bir alt ağ için ağ kuralı eklemek için "/abonelikler/abonelik-id/kaynakGruplar/kaynakGrup-Ad/sağlayıcılar/Microsoft.Network/virtualNetworks/vNet adı/alt ağlar/alt net adı" şeklinde tam nitelikli **VirtualNetworkResourceId** parametresi kullanın.

1. Sanal ağ ve alt ağ için ağ kuralını kaldırın.

    ```powershell
    $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id
    ```

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**için ayarladığından veya ağ [kurallarının](#change-the-default-network-access-rule) hiçbir etkisi olmadığından emin olun.

#### <a name="cliv2"></a>CLIv2

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın.](/cli/azure/authenticate-azure-cli)

1. Sanal ağ kurallarını listele.

    ```azurecli
    az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
    ```

1. Varolan bir sanal ağ da ve alt ağda Azure Depolama için hizmet bitiş noktasını etkinleştirin.

    ```azurecli
    az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
    ```

1. Sanal ağ ve alt ağ için ağ kuralı ekleyin.

    ```azurecli
    $subnetid=(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
    ```

    > [!TIP]
    > Başka bir Azure AD kiracısına ait bir VNet'teki bir alt ağ için kural eklemek için\<"/abonelikler/ abonelik-ID\>\</resourceGroups/ resourceGroup-Name/providers/Microsoft.Network/virtualNetworks/\<\>\>\<vNet-name\>/subnets/ subnet adı" şeklinde tam nitelikli bir alt net kimliği kullanın.
    >
    > **Abonelik** parametresini, başka bir Azure AD kiracısına ait bir VNet'in alt net kimliğini almak için kullanabilirsiniz.

1. Sanal ağ ve alt ağ için ağ kuralını kaldırın.

    ```azurecli
    $subnetid=(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
    ```

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**için ayarladığından veya ağ [kurallarının](#change-the-default-network-access-rule) hiçbir etkisi olmadığından emin olun.

## <a name="grant-access-from-an-internet-ip-range"></a>Bir internet IP aralığından erişim izni verme

Depolama hesaplarını belirli genel internet IP adres aralıklarından erişime izin verecek şekilde yapılandırabilirsiniz. Bu yapılandırma, belirli internet tabanlı hizmetlere ve şirket içi ağlara erişim sağlar ve genel internet trafiğini engeller.

*16.17.18.0/24* formunda VEYA *16.17.18.19*gibi bireysel IP adresleri olarak [CIDR gösterimini](https://tools.ietf.org/html/rfc4632) kullanarak izin verilen internet adres aralıklarını sağlayın.

   > [!NOTE]
   > "/31" veya "/32" öneki boyutlarını kullanan küçük adres aralıkları desteklenmez. Bu aralıklar tek tek IP adresi kuralları kullanılarak yapılandırılmalıdır.

IP ağ kurallarına yalnızca **genel internet** IP adresleri için izin verilir. Özel ağlar için ayrılmış IP adresi aralıklarına [(RFC 1918'de](https://tools.ietf.org/html/rfc1918#section-3)tanımlandığı gibi) IP kurallarında izin verilmez. Özel _ağlar, 10.*_, _172.16.*_ - _172.31.*_ ve _192.168_ile başlayan adresleri içerir.

   > [!NOTE]
   > IP ağ kurallarının depolama hesabıyla aynı Azure bölgesinden gelen istekler üzerinde hiçbir etkisi yoktur. Aynı bölge isteklerine izin vermek için [Sanal ağ kurallarını](#grant-access-from-a-virtual-network) kullanın.

  > [!NOTE]
  > Depolama hesabıyla aynı bölgede dağıtılan hizmetler, iletişim için özel Azure IP adreslerini kullanır. Bu nedenle, belirli Azure hizmetlerine erişimi, genel gelen IP adresi aralığına göre kısıtlayamazsınız.

Depolama güvenlik duvarı kurallarının yapılandırması için yalnızca IPV4 adresleri desteklenir.

Her depolama hesabı en fazla 100 IP ağ kuralını destekler.

### <a name="configuring-access-from-on-premises-networks"></a>Şirket içi ağlardan erişimi yapılandırma

Şirket içi ağlarınızdan IP ağı kuralıyla depolama hesabınıza erişim sağlamak için, ağınız tarafından kullanılan internete bakan IP adreslerini tanımlamanız gerekir. Yardım için ağ yöneticinize başvurun.

ExpressRoute'u [ExpressRoute](/azure/expressroute/expressroute-introduction) binanızdan kullanıyorsanız, herkese açık bir şekilde bakmak veya Microsoft'a bakmak için kullanılan NAT IP adreslerini tanımlamanız gerekir. Ortak eşleme için, her bir ExpressRoute varsayılan olarak bağlantı hattında trafik Microsoft Azure omurga ağına girdiğinde Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanılır. Microsoft'un eşlemi için kullanılan NAT IP adresleri müşteri tarafından sağlanır veya servis sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](/azure/expressroute/expressroute-nat#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.

### <a name="managing-ip-network-rules"></a>IP ağ kurallarını yönetme

Azure portalı, PowerShell veya CLIv2 aracılığıyla depolama hesapları için IP ağ kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portalında

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. Güvenlik Duvarları ve sanal ağlar adı verilen ayarlar **menüsüne**tıklayın.

1. **Seçili ağlardan**erişime izin vermek için seçtiğinizden denetlemeyin.

1. Bir internet IP aralığına erişim sağlamak **için, Firewall** > **Adres Aralığı'nın**altındaki IP adresini veya adres aralığını (CIDR formatında) girin.

1. IP ağ kuralını kaldırmak için, adres aralığının yanındaki çöp kutusu simgesini tıklatın.

1. Değişikliklerinizi uygulamak için **Kaydet**’e tıklayın.

#### <a name="powershell"></a>PowerShell

1. Azure [PowerShell'i](/powershell/azure/install-Az-ps) yükleyin ve [oturum açın.](/powershell/azure/authenticate-azureps)

1. IP ağ kurallarını listele.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
    ```

1. Tek bir IP adresi için ağ kuralı ekleyin.

    ```powershell
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"
    ```

1. IP adresi aralığı için ağ kuralı ekleyin.

    ```powershell
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"
    ```

1. Tek bir IP adresi için ağ kuralını kaldırın.

    ```powershell
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"
    ```

1. IP adresi aralığı için ağ kuralını kaldırın.

    ```powershell
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"
    ```

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**için ayarladığından veya ağ [kurallarının](#change-the-default-network-access-rule) hiçbir etkisi olmadığından emin olun.

#### <a name="cliv2"></a>CLIv2

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın.](/cli/azure/authenticate-azure-cli)

1. IP ağ kurallarını listele.

    ```azurecli
    az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
    ```

1. Tek bir IP adresi için ağ kuralı ekleyin.

    ```azurecli
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
    ```

1. IP adresi aralığı için ağ kuralı ekleyin.

    ```azurecli
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
    ```

1. Tek bir IP adresi için ağ kuralını kaldırın.

    ```azurecli
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
    ```

1. IP adresi aralığı için ağ kuralını kaldırın.

    ```azurecli
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
    ```

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**için ayarladığından veya ağ [kurallarının](#change-the-default-network-access-rule) hiçbir etkisi olmadığından emin olun.

## <a name="exceptions"></a>Özel durumlar

Ağ kuralları, çoğu senaryo için uygulamalarınız ve verileriniz arasındaki bağlantılar için güvenli bir ortam oluşturmaya yardımcı olur. Ancak, bazı uygulamalar sanal ağ veya IP adresi kuralları aracılığıyla benzersiz olarak yalıtılamayan Azure hizmetlerine bağlıdır. Ancak, tam uygulama işlevselliğini etkinleştirmek için bu tür hizmetlerin depolamaya verilmesi gerekir. Bu gibi durumlarda, bu tür hizmetlerin verilerinize, günlüklerinize veya analizlerinize erişmesini sağlamak için ***güvenilen Microsoft hizmetlerine İzin Ver*** ayarını kullanabilirsiniz.

### <a name="trusted-microsoft-services"></a>Güvenilir Microsoft hizmetleri

Bazı Microsoft hizmetleri, ağ kurallarınıza dahil edilemeyecek ağlarda çalışır. Diğer uygulamalar için ağ kurallarını korurken, bu tür güvenilir Microsoft hizmetlerinin bir alt kümesini depolama hesabına erişebilirsiniz. Bu güvenilir hizmetler daha sonra depolama hesabınıza güvenli bir şekilde bağlanmak için güçlü kimlik doğrulaması kullanır. Microsoft hizmetleri için iki kez güvenilir erişim modu sağladık.

- Aboneliğinizde kayıtlı **olan**bazı hizmetlerin kaynakları, günlük yazma veya yedekleme gibi belirli işlemler için **depolama hesabınıza aynı abonelikte** erişebilir.
- Bazı hizmetlerin kaynaklarına, sistem tarafından atanan yönetilen kimliğine **bir RBAC rolü atayarak** depolama hesabınıza açık erişim hakkı verilebilir.


**Güvenilen Microsoft hizmetlerine İzin Ver...** ayarını etkinleştirdiğinizde, depolama hesabınızla aynı abonelikte kayıtlı aşağıdaki hizmetlerin kaynaklarına açıklandığı gibi sınırlı bir işlem kümesi için erişim hakkı verilir:

| Hizmet                  | Kaynak Sağlayıcı Adı     | İzin verilen işlemler                 |
|:------------------------ |:-------------------------- |:---------------------------------- |
| Azure Backup             | Microsoft.RecoveryServices | IAAS sanal makinelerinde yönetilmeyen disklerin yedeklerini ve geri yüklemelerini çalıştırın. (yönetilen diskler için gerekli değildir). [Daha fazla bilgi edinin](/azure/backup/backup-introduction-to-azure-backup). |
| Azure Data Box           | Microsoft.DataBox          | Veri Kutusu'ni kullanarak Azure'a veri aktarılmasını sağlar. [Daha fazla bilgi edinin](/azure/databox/data-box-overview). |
| Azure DevTest Labs       | Microsoft.DevTestLab       | Özel görüntü oluşturma ve yapı yükleme. [Daha fazla bilgi edinin](/azure/devtest-lab/devtest-lab-overview). |
| Azure Event Grid         | Microsoft.EventGrid        | Blob Depolama olay yayımlama etkinleştirin ve Olay Grid depolama kuyruklarında yayımlamak için izin. [Blob depolama olayları](/azure/event-grid/event-sources) ve [kuyruklara yayımlama](/azure/event-grid/event-handlers)hakkında bilgi edinin. |
| Azure Event Hubs         | Microsoft.EventHub         | Olay Hub'ları Yakalama ile verileri arşivleyin. [Daha fazla bilgi edinin.](/azure/event-hubs/event-hubs-capture-overview) |
| Azure Dosya Eşitleme          | Microsoft.StorageSync      | Prem dosya sunucunuzu Azure Dosyası paylaşımları için bir önbelleğe dönüştürmenizi sağlar. Çok siteli eşitleme, hızlı olağanüstü durum kurtarma ve bulut tarafı yedeklemesine izin verme. [Daha fazlasını öğrenin](../files/storage-sync-files-planning.md) |
| Azure HDInsight          | Microsoft.HDInsight        | Yeni bir HDInsight kümesi için varsayılan dosya sisteminin ilk içeriğini sağlama. [Daha fazla bilgi edinin](/azure/hdinsight/hdinsight-hadoop-use-blob-storage). |
| Azure İçe Aktarma Dışa Aktarma      | Microsoft.ImportExport     | İçe Aktarma/Dışa Aktarma hizmetini kullanarak Verilerin Azure'a aktarılmasını ve Azure'dan veri dışa aktarılmasını sağlar. [Daha fazla bilgi edinin](/azure/storage/common/storage-import-export-service).  |
| Azure İzleyici            | Microsoft.Insights         | Kaynak tanılama günlükleri, Azure Active Directory oturum açma ve denetim günlükleri ve Microsoft Intune günlükleri de dahil olmak üzere güvenli bir depolama hesabına izleme verilerinin yazılmasına olanak tanır. [Daha fazla bilgi edinin](/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security). |
| Azure Ağ         | Microsoft.Network          | Ağ trafiği günlüklerini depolayın ve analiz edin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview). |
| Azure Site Recovery      | Microsoft.SiteKurtarma     | Güvenlik duvarı etkin önbellek, kaynak veya hedef depolama hesapları kullanırken Azure IaaS sanal makinelerinin olağanüstü durum kurtarma işlemini etkinleştirin.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication). |

**Güvenilen Microsoft hizmetlerine İzin Ver...** ayarı, bu kaynak örneği için [sistem tarafından atanan yönetilen kimliğe](../../active-directory/managed-identities-azure-resources/overview.md) açıkça bir [RBAC rolü atarsanız,](storage-auth-aad.md#assign-rbac-roles-for-access-rights) aşağıdaki hizmetlerin belirli bir örneğinin depolama hesabına erişmesine de izin verir. Bu durumda, örneğin erişim kapsamı yönetilen kimliğe atanan RBAC rolüne karşılık gelir.

| Hizmet                        | Kaynak Sağlayıcı Adı                 | Amaç            |
| :----------------------------- | :------------------------------------- | :----------------- |
| Azure Bilişsel Arama         | Microsoft.Search/searchServices        | Bilişsel Arama hizmetlerinin dizin oluşturma, işleme ve sorgulama için depolama hesaplarına erişmesini sağlar. |
| Azure Container Registry Görevleri | Microsoft.ContainerRegistry/registries | ACR Görevleri, kapsayıcı görüntüleri yaparken depolama hesaplarına erişebilir. |
| Azure Data Factory             | Microsoft.DataFactory/fabrikalar        | ADF çalışma süresi üzerinden depolama hesaplarına erişim sağlar. |
| Azure Veri Paylaşımı               | Microsoft.DataShare/hesapları           | Veri Paylaşımı aracılığıyla depolama hesaplarına erişim sağlar. |
| Azure Logic Apps               | Microsoft.Logic/iş akışları              | Depolama hesaplarına erişmek için mantık uygulamaları sağlar. [Daha fazla bilgi edinin](/azure/logic-apps/create-managed-service-identity#authenticate-access-with-managed-identity). |
| Azure Machine Learning Hizmeti | Microsoft.MachineLearningServices      | Yetkili Azure Machine Learning çalışma alanları deneme çıktısını, modelleri ve günlükleri Blob depolamasına yazar ve verileri okur. [Daha fazla bilgi edinin](/azure/machine-learning/service/how-to-enable-virtual-network#use-a-storage-account-for-your-workspace). | 
| Azure SQL Veri Ambarı       | Microsoft.Sql                          | PolyBase kullanarak belirli SQL Veritabanı örneklerinden veri alma ve dışa aktarmaya izin verir. [Daha fazla bilgi edinin](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview). |
| Azure Stream Analytics         | Microsoft.StreamAnalytics             | Akış işinden gelen verilerin Blob depolamaalanına yazılmasına izin verir. Bu özellik şu anda önizleme sürümündedir. [Daha fazla bilgi edinin](/azure/stream-analytics/blob-output-managed-identity). |
| Azure Synapse Analytics        | Microsoft.Synapse/çalışma alanları          | Synapse Analytics'ten Azure Depolama'daki verilere erişim sağlar. |


### <a name="storage-analytics-data-access"></a>Depolama analitiği veri erişimi

Bazı durumlarda, ağ sınırı dışından okunan tanılama günlüklerine ve ölçümlere erişim gereklidir. Güvenilen hizmetleri depolama hesabına erişirirken, günlük dosyaları, ölçüm tabloları veya her ikisi için okuma erişimine izin verebilirsiniz. [Depolama analitiği ile çalışma hakkında daha fazla bilgi edinin.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Özel durumları yönetme

Ağ kuralı özel durumlarını Azure portalı, PowerShell veya Azure CLI v2 üzerinden yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portalında

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. Güvenlik Duvarları ve sanal ağlar adı verilen ayarlar **menüsüne**tıklayın.

1. **Seçili ağlardan**erişime izin vermek için seçtiğinizden denetlemeyin.

1. **Özel Durumlar**altında, vermek istediğiniz özel durumları seçin.

1. Değişikliklerinizi uygulamak için **Kaydet**’e tıklayın.

#### <a name="powershell"></a>PowerShell

1. Azure [PowerShell'i](/powershell/azure/install-Az-ps) yükleyin ve [oturum açın.](/powershell/azure/authenticate-azureps)

1. Depolama hesabı ağı kurallarının özel durumlarını görüntüleyin.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
    ```

1. Özel durumları depolama hesabı ağı kurallarının yapılandırın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -Bypass AzureServices,Metrics,Logging
    ```

1. Depolama hesabı ağı kurallarının özel durumlarını kaldırın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -Bypass None
    ```

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**veya özel durumları kaldırmak için [ayarladıktan](#change-the-default-network-access-rule) emin olun.

#### <a name="cliv2"></a>CLIv2

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın.](/cli/azure/authenticate-azure-cli)

1. Depolama hesabı ağı kurallarının özel durumlarını görüntüleyin.

    ```azurecli
    az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
    ```

1. Özel durumları depolama hesabı ağı kurallarının yapılandırın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
    ```

1. Depolama hesabı ağı kurallarının özel durumlarını kaldırın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
    ```

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**veya özel durumları kaldırmak için [ayarladıktan](#change-the-default-network-access-rule) emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet bitiş noktalarında](/azure/virtual-network/virtual-network-service-endpoints-overview)Azure Ağı hizmet bitiş noktaları hakkında daha fazla bilgi edinin.

[Azure Depolama güvenlik kılavuzunda](../blobs/security-recommendations.md)Azure Depolama güvenliğini daha derinlemesine araştırın.
