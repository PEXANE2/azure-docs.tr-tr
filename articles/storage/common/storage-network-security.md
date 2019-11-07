---
title: Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma | Microsoft Docs
description: Depolama hesabınız için katmanlı ağ güvenliğini yapılandırın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: a02e690e344678b512503f8c3beb57023a838ac0
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686664"
---
# <a name="configure-azure-storage-firewalls-and-virtual-networks"></a>Azure depolama güvenlik duvarlarını ve sanal ağları yapılandırma

Azure depolama, katmanlı bir güvenlik modeli sağlar. Bu model, kullanılan ağların türüne ve alt kümelerine göre uygulamalarınızın ve kurumsal ortamların talep ettiği depolama hesaplarınıza erişim düzeyini sağlamanıza ve denetlemenize olanak sağlar. Ağ kuralları yapılandırıldığında, yalnızca belirtilen ağ kümesi üzerinde veri isteyen uygulamalar bir depolama hesabına erişebilir. Depolama hesabınıza erişimi, belirtilen IP adreslerinden, IP aralıklarından veya bir Azure sanal ağındaki (VNet) bir alt ağ listesinden kaynaklanan isteklerle sınırlayabilirsiniz.

Depolama hesaplarının internet üzerinden erişilebilen genel bir uç noktası vardır. Depolama hesabınız için, sanal ağınızdan depolama hesabına özel bir IP adresi atayan ve sanal ağınız ile depolama hesabı arasındaki tüm trafiği özel bir bağlantı üzerinden güvenlik altına alarak, [depolama hesabınız Için özel uç noktalar](storage-private-endpoints.md)da oluşturabilirsiniz. Azure Storage güvenlik duvarı, depolama hesabınızın genel uç noktası için erişim denetimi erişimi sağlar. Özel uç noktaları kullanırken genel uç nokta aracılığıyla tüm erişimi engellemek için güvenlik duvarını da kullanabilirsiniz. Depolama güvenlik duvarınızın yapılandırması, depolama hesabına güvenli bir şekilde erişmek için güvenilen Azure platform Hizmetleri ' ni de sağlar.

Ağ kuralları etkin olduğunda depolama hesabına erişen bir uygulama, istek için uygun yetkilendirme gerektirir. Yetkilendirme, geçerli bir hesap erişim anahtarı veya SAS belirteci ile blob 'lar ve kuyruklar için Azure Active Directory (Azure AD) kimlik bilgileri ile desteklenir.

> [!IMPORTANT]
> İstekler bir Azure sanal ağı (VNet) içinde veya izin verilen ortak IP adreslerinden bir hizmetten kaynaklanmadığı takdirde, depolama hesabınız için Güvenlik Duvarı kurallarının etkinleştirilmesi, varsayılan olarak gelen istekleri engeller. Engellenen istekler diğer Azure hizmetlerinden, Azure portal, günlük ve ölçüm hizmetlerinden ve bu şekilde devam eder.
>
> Hizmet örneğini barındıran alt ağdan gelen trafiğe izin vererek VNet içinden çalışan Azure hizmetlerine erişim izni verebilirsiniz. Ayrıca, aşağıda açıklanan [özel durum](#exceptions) mekanizmasıyla sınırlı sayıda senaryoyu etkinleştirebilirsiniz. Depolama hesabındaki verilere Azure portal aracılığıyla erişmek için, ayarladığınız güvenilir sınır (IP veya VNet) içinde bir makinede olmanız gerekir.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="scenarios"></a>Senaryolar

Depolama hesabınızın güvenliğini sağlamak için, varsayılan olarak genel uç noktasındaki tüm ağlardan (internet trafiği dahil) trafiğe erişimi reddetmek üzere bir kural yapılandırmanız gerekir. Ardından, belirli sanal ağlardan gelen trafiğe erişim veren kurallar yapılandırmanız gerekir. Ayrıca, belirli internet veya şirket içi istemcilerden gelen bağlantıları etkinleştirerek, genel İnternet IP adresi aralıklarından gelen trafiğe erişim izni vermek için kurallar da yapılandırabilirsiniz. Bu yapılandırma, uygulamalarınız için güvenli bir ağ sınırı oluşturmanıza olanak sağlar.

Belirli sanal ağlardan ve aynı depolama hesabındaki genel IP adres aralıklarından erişime izin veren güvenlik duvarı kurallarını birleştirebilirsiniz. Depolama güvenlik duvarı kuralları mevcut depolama hesaplarına veya yeni depolama hesapları oluştururken uygulanabilir.

Depolama güvenlik duvarı kuralları, bir depolama hesabının genel uç noktası için geçerlidir. Bir depolama hesabının özel uç noktaları için trafiğe izin vermek üzere herhangi bir güvenlik duvarı erişim kuralına gerek yoktur. Özel bir uç noktanın oluşturulmasını onaylama işlemi, Özel uç noktasını barındıran alt ağdan gelen trafiğe örtülü erişim verir.

Ağ kuralları, Azure depolama 'ya REST ve SMB dahil tüm ağ protokollerinde zorlanır. Azure portal, Depolama Gezgini ve AZCopy gibi araçları kullanarak verilere erişmek için açık ağ kurallarının yapılandırılması gerekir.

Ağ kuralları uygulandıktan sonra, tüm istekler için zorlanır. Belirli bir IP adresine erişim izni veren SAS belirteçleri, belirteç tutucusuna erişimi sınırlandırmaya, ancak yapılandırılan ağ kurallarından daha fazla yeni erişim vermemelidir.

Sanal makine disk trafiği (bağlama ve çıkarma işlemleri ve disk GÇ dahil) ağ kurallarından etkilenmez. Sayfa bloblarına REST erişimi ağ kuralları tarafından korunur.

Klasik depolama hesapları, güvenlik duvarlarını ve sanal ağları desteklemez.

VM 'Leri yedekleme ve geri yükleme için bir özel durum oluşturarak, ağ kuralları uygulanmış olan depolama hesaplarında yönetilmeyen diskleri kullanabilirsiniz. Bu işlem, bu makalenin [özel durumlar](#exceptions) bölümünde belgelenmiştir. Güvenlik Duvarı özel durumları, zaten Azure tarafından yönetildikleri için yönetilen disklerle ilgili değildir.

## <a name="change-the-default-network-access-rule"></a>Varsayılan ağ erişim kuralını değiştirme

Varsayılan olarak, depolama hesapları herhangi bir ağdaki istemcilerden gelen bağlantıları kabul eder. Seçilen ağlara erişimi sınırlandırmak için, önce varsayılan eylemi değiştirmeniz gerekir.

> [!WARNING]
> Ağ kurallarında değişiklik yapmak, uygulamalarınızın Azure depolama 'ya bağlanma yeteneğini etkileyebilir. Varsayılan ağ kuralını **Reddet** olarak ayarlamak, erişim **izni** veren belirli ağ kuralları da uygulanmamışsa veriye tüm erişimi engeller. Erişimi reddetmek üzere varsayılan kuralı değiştirmeden önce ağ kurallarını kullanarak izin verilen ağlara erişim izni vermediğinizden emin olun.

### <a name="managing-default-network-access-rules"></a>Varsayılan ağ erişim kurallarını yönetme

Azure portal, PowerShell veya CLIv2 aracılığıyla depolama hesapları için varsayılan ağ erişim kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. **Güvenlik duvarları ve sanal ağlar**adlı ayarlar menüsüne tıklayın.

1. Erişimi varsayılan olarak reddetmek için, **Seçili ağlardan**erişime izin ver ' i seçin. Tüm ağlardan gelen trafiğe izin vermek için **tüm ağlardan**erişime izin ver ' i seçin.

1. Değişikliklerinizi uygulamak için **Kaydet** ' e tıklayın.

#### <a name="powershell"></a>PowerShell

1. [Azure PowerShell](/powershell/azure/install-Az-ps) yükleyip [oturum açın](/powershell/azure/authenticate-azureps).

1. Depolama hesabı için varsayılan kuralın durumunu görüntüleyin.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").DefaultAction
    ```

1. Varsayılan olarak ağ erişimini reddetmek için varsayılan kuralı ayarlayın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Deny
    ```

1. Varsayılan olarak ağ erişimine izin vermek için varsayılan kuralı ayarlayın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -DefaultAction Allow
    ```

#### <a name="cliv2"></a>CLIv2

1. [Azure CLI](/cli/azure/install-azure-cli) 'yı yükleyip [oturum açın](/cli/azure/authenticate-azure-cli).

1. Depolama hesabı için varsayılan kuralın durumunu görüntüleyin.

    ```azurecli
    az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.defaultAction
    ```

1. Varsayılan olarak ağ erişimini reddetmek için varsayılan kuralı ayarlayın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --default-action Deny
    ```

1. Varsayılan olarak ağ erişimine izin vermek için varsayılan kuralı ayarlayın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --default-action Allow
    ```

## <a name="grant-access-from-a-virtual-network"></a>Bir sanal ağdan erişim izni verme

Depolama hesaplarını yalnızca belirli alt ağlardan erişime izin verecek şekilde yapılandırabilirsiniz. İzin verilen alt ağlar, farklı bir Azure Active Directory kiracısına ait abonelikler de dahil olmak üzere, aynı abonelikte bulunan bir VNet 'e veya farklı bir aboneliğe ait olabilir.

VNet içinde Azure depolama için bir [hizmet uç noktası](/azure/virtual-network/virtual-network-service-endpoints-overview) etkinleştirin. Hizmet uç noktası, trafiği VNet 'ten Azure depolama hizmetine en uygun bir yol üzerinden yönlendirir. Alt ağın ve sanal ağın kimlikleri de her istekle birlikte iletilir. Yöneticiler daha sonra depolama hesabı için ağ kurallarını, isteklerin bir sanal ağdaki belirli alt ağlardan alınmasına izin veren ağ kurallarını yapılandırabilir. Bu ağ kuralları aracılığıyla erişim izni verilen istemciler, verileri erişmek için depolama hesabının yetkilendirme gereksinimlerini karşılamaya devam etmelidir.

Her depolama hesabı, [IP ağ kuralları](#grant-access-from-an-internet-ip-range)ile birleştirilebilir en fazla 100 sanal ağ kuralını destekler.

### <a name="available-virtual-network-regions"></a>Kullanılabilir sanal ağ bölgeleri

Genel olarak, hizmet uç noktaları aynı Azure bölgesindeki sanal ağlar ve hizmet örnekleri arasında çalışır. Azure depolama ile hizmet uç noktaları kullanılırken, bu kapsam [eşleştirilmiş bölgeyi](/azure/best-practices-availability-paired-regions)dahil etmek için büyür. Hizmet uç noktaları bölgesel yük devretme sırasında sürekliliği ve salt Erişimli Coğrafi olarak yedekli depolama (RA-GRS) örneklerine erişim sağlar. Bir sanal ağdan bir depolama hesabına erişim izni veren ağ kuralları da herhangi bir RA-GRS örneğine erişim sağlar.

Bölgesel bir kesinti sırasında olağanüstü durum kurtarma için planlama yaparken, sanal ağları eşleştirilmiş bölgede önceden oluşturmalısınız. Bu alternatif sanal ağlardan erişim sağlayan ağ kuralları ile Azure depolama için hizmet uç noktalarını etkinleştirin. Ardından bu kuralları coğrafi olarak yedekli depolama hesaplarınıza uygulayın.

> [!NOTE]
> Hizmet uç noktaları, sanal ağın ve belirtilen bölge çiftinin bölgesi dışındaki trafiğe uygulanmaz. Yalnızca bir depolama hesabının birincil bölgesindeki veya belirtilen eşlenmiş bölgedeki depolama hesaplarına sanal ağlardan erişim sağlayan ağ kuralları uygulayabilirsiniz.

### <a name="required-permissions"></a>Gerekli izinler

Bir depolama hesabına sanal ağ kuralı uygulamak için, kullanıcının eklenmekte olan alt ağlar için uygun izinlere sahip olması gerekir. Gerekli olan izin, *hizmete bir alt ağa katılır* ve *depolama hesabı katılımcısı* yerleşik rolüne dahildir. Özel rol tanımlarına da eklenebilir.

Depolama hesabı ve erişim verilen sanal ağlar, farklı bir Azure AD kiracısının parçası olan abonelikler de dahil olmak üzere farklı aboneliklerde olabilir.

> [!NOTE]
> Farklı bir Azure Active Directory kiracının parçası olan sanal ağlardaki alt ağlara erişim izni veren kuralların yapılandırılması Şu anda yalnızca PowerShell, CLı ve REST API 'Leri aracılığıyla desteklenir. Bu kurallar, portalda görüntülenebilse de Azure portal aracılığıyla yapılandırılamaz.

### <a name="managing-virtual-network-rules"></a>Sanal ağ kurallarını yönetme

Azure portal, PowerShell veya CLIv2 aracılığıyla depolama hesapları için sanal ağ kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. **Güvenlik duvarları ve sanal ağlar**adlı ayarlar menüsüne tıklayın.

1. **Seçili ağlardan**erişime izin vermeyi seçtiğinizden emin olun.

1. Yeni bir ağ kuralıyla bir sanal ağa erişim vermek için, **sanal ağlar**altında **var olan sanal ağı ekle**' ye tıklayın, **sanal ağlar** ve **alt ağlar** seçeneklerini belirleyin ve ardından **Ekle**' ye tıklayın. Yeni bir sanal ağ oluşturmak ve bu ağa erişim vermek için **Yeni sanal ağ ekle**' ye tıklayın. Yeni sanal ağı oluşturmak için gereken bilgileri girin ve ardından **Oluştur**' a tıklayın.

    > [!NOTE]
    > Azure Storage için bir hizmet uç noktası, seçilen sanal ağ ve alt ağlar için önceden yapılandırılmadıysa, bu işlemin bir parçası olarak yapılandırabilirsiniz.
    >
    > Şu anda, kural oluşturma sırasında seçim için yalnızca aynı Azure Active Directory kiracıya ait olan sanal ağlar gösterilir. Başka bir kiracıya ait bir sanal ağdaki bir alt ağa erişim vermek için lütfen PowerShell, CLı veya REST API 'Leri kullanın.

1. Bir sanal ağ veya alt ağ kuralını kaldırmak için **.** .. öğesine tıklayarak sanal ağ veya alt ağ için bağlam menüsünü açın ve **Kaldır**' a tıklayın.

1. Değişikliklerinizi uygulamak için **Kaydet** ' e tıklayın.

#### <a name="powershell"></a>PowerShell

1. [Azure PowerShell](/powershell/azure/install-Az-ps) yükleyip [oturum açın](/powershell/azure/authenticate-azureps).

1. Sanal ağ kurallarını listeleyin.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").VirtualNetworkRules
    ```

1. Mevcut bir sanal ağ ve alt ağ üzerinde Azure depolama için hizmet uç noktasını etkinleştirin.

    ```powershell
    Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" -AddressPrefix "10.0.0.0/24" -ServiceEndpoint "Microsoft.Storage" | Set-AzVirtualNetwork
    ```

1. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.

    ```powershell
    $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id
    ```

    > [!TIP]
    > Başka bir Azure AD kiracısına ait bir sanal ağa ait bir ağ kuralı eklemek için, "/subscriptions/subscription-ID/resourceGroups/resourceGroup-Name/providers/Microsoft.Network/virtualNetworks/vNet-name/subnets/subnet-name" biçiminde tam nitelikli bir **Virtualnetworkresourceıd** parametresi kullanın.

1. Bir sanal ağ ve alt ağ için bir ağ kuralını kaldırın.

    ```powershell
    $subnet = Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" -Name "myvnet" | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -VirtualNetworkResourceId $subnet.Id
    ```

> [!IMPORTANT]
> [Varsayılan kuralı](#change-the-default-network-access-rule) **Reddet**olarak ayarladığınızdan emin olun veya ağ kurallarının hiçbir etkisi yoktur.

#### <a name="cliv2"></a>CLIv2

1. [Azure CLI](/cli/azure/install-azure-cli) 'yı yükleyip [oturum açın](/cli/azure/authenticate-azure-cli).

1. Sanal ağ kurallarını listeleyin.

    ```azurecli
    az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query virtualNetworkRules
    ```

1. Mevcut bir sanal ağ ve alt ağ üzerinde Azure depolama için hizmet uç noktasını etkinleştirin.

    ```azurecli
    az network vnet subnet update --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --service-endpoints "Microsoft.Storage"
    ```

1. Bir sanal ağ ve alt ağ için bir ağ kuralı ekleyin.

    ```azurecli
    $subnetid=(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
    ```

    > [!TIP]
    > Başka bir Azure AD kiracısına ait olan bir sanal ağa yönelik bir kural eklemek için, "/Subscriptions/\<abonelik-KIMLIĞI\>/resourceGroups/\<resourceGroup-Name\>/providers/Microsoft.Network/virtualNetworks/\<vNet-adı\>/Subnets/\<alt ağ-adı\>" biçiminde tam olarak nitelenmiş bir alt ağ KIMLIĞI kullanın.
    > 
    > Başka bir Azure AD kiracısına ait olan bir sanal ağın alt ağ KIMLIĞINI almak için **abonelik** parametresini kullanabilirsiniz.

1. Bir sanal ağ ve alt ağ için bir ağ kuralını kaldırın.

    ```azurecli
    $subnetid=(az network vnet subnet show --resource-group "myresourcegroup" --vnet-name "myvnet" --name "mysubnet" --query id --output tsv)
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --subnet $subnetid
    ```

> [!IMPORTANT]
> [Varsayılan kuralı](#change-the-default-network-access-rule) **Reddet**olarak ayarladığınızdan emin olun veya ağ kurallarının hiçbir etkisi yoktur.

## <a name="grant-access-from-an-internet-ip-range"></a>İnternet IP aralığından erişim izni verme

Depolama hesaplarını, belirli genel İnternet IP adresi aralıklarından erişime izin verecek şekilde yapılandırabilirsiniz. Bu yapılandırma, belirli Internet tabanlı hizmetlere ve şirket içi ağlara erişim verir ve genel İnternet trafiğini engeller.

*16.17.18.0/24* biçiminde veya *16.17.18.19*gibi ayrı IP adresleri olarak [CIDR gösterimini](https://tools.ietf.org/html/rfc4632) kullanarak izin verilen internet adres aralıklarını sağlayın.

   > [!NOTE]
   > "/31" veya "/32" önek boyutları kullanılarak küçük adres aralıkları desteklenmez. Bu aralıklar tek tek IP adresi kuralları kullanılarak yapılandırılmalıdır.

IP ağ kurallarına yalnızca **genel İnternet** IP adresleri için izin verilir. Özel ağlar için ayrılan IP adresi aralıklarına ( [RFC 1918](https://tools.ietf.org/html/rfc1918#section-3)' de tanımlandığı gıbı) IP kurallarında izin verilmez. Özel ağlarda, _10. *_ , _172,16. *_  - _172,31. *_ ve _192,168. *_ ile başlayan adresler bulunur.

   > [!NOTE]
   > IP ağ kurallarının, depolama hesabıyla aynı Azure bölgesinden kaynaklanan isteklere etkisi yoktur. Aynı bölge isteklerine izin vermek için [sanal ağ kurallarını](#grant-access-from-a-virtual-network) kullanın.

  > [!NOTE]
  > Depolama hesabı ile aynı bölgede dağıtılan hizmetler, iletişim için özel Azure IP adreslerini kullanır. Bu nedenle, belirli Azure hizmetlerine erişimi, genel gelen IP adresi aralığına göre kısıtlayamazsınız.

Depolama Güvenlik Duvarı kurallarının yapılandırılması için yalnızca ıPV4 adresleri desteklenir.

Her depolama hesabı en fazla 100 IP ağ kuralını destekler.

### <a name="configuring-access-from-on-premises-networks"></a>Şirket içi ağlardan erişimi yapılandırma

Şirket içi ağlarınızdan bir IP ağ kuralıyla depolama hesabınıza erişim izni vermek için ağınız tarafından kullanılan internet 'e yönelik IP adreslerini belirlemeniz gerekir. Yardım için ağ yöneticinize başvurun.

Şirket içinde genel eşleme veya Microsoft eşlemesi için [ExpressRoute](/azure/expressroute/expressroute-introduction) kullanıyorsanız, kullanılan NAT IP adreslerini belirlemeniz gerekir. Ortak eşleme için, her bir ExpressRoute varsayılan olarak bağlantı hattında trafik Microsoft Azure omurga ağına girdiğinde Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanılır. Microsoft eşlemesi için, kullanılan NAT IP adresleri müşteri tarafından sağlanmış veya hizmet sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](/azure/expressroute/expressroute-nat#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.

### <a name="managing-ip-network-rules"></a>IP ağ kurallarını yönetme

Azure portal, PowerShell veya CLIv2 aracılığıyla depolama hesapları için IP ağ kurallarını yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. **Güvenlik duvarları ve sanal ağlar**adlı ayarlar menüsüne tıklayın.

1. **Seçili ağlardan**erişime izin vermeyi seçtiğinizden emin olun.

1. Bir internet IP aralığına erişim izni vermek için, **güvenlik duvarı** > **adres aralığı**altında IP adresini veya adres aralığını (CIDR biçiminde) girin.

1. Bir IP ağ kuralını kaldırmak için, adres aralığının yanındaki çöp kutusu simgesine tıklayın.

1. Değişikliklerinizi uygulamak için **Kaydet** ' e tıklayın.

#### <a name="powershell"></a>PowerShell

1. [Azure PowerShell](/powershell/azure/install-Az-ps) yükleyip [oturum açın](/powershell/azure/authenticate-azureps).

1. IP ağ kurallarını listeleyin.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount").IPRules
    ```

1. Tek bir IP adresi için bir ağ kuralı ekleyin.

    ```powershell
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"
    ```

1. Bir IP adresi aralığı için ağ kuralı ekleyin.

    ```powershell
    Add-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"
    ```

1. Tek bir IP adresi için bir ağ kuralını kaldırın.

    ```powershell
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.19"
    ```

1. Bir IP adresi aralığı için ağ kuralını kaldır.

    ```powershell
    Remove-AzStorageAccountNetworkRule -ResourceGroupName "myresourcegroup" -AccountName "mystorageaccount" -IPAddressOrRange "16.17.18.0/24"
    ```

> [!IMPORTANT]
> [Varsayılan kuralı](#change-the-default-network-access-rule) **Reddet**olarak ayarladığınızdan emin olun veya ağ kurallarının hiçbir etkisi yoktur.

#### <a name="cliv2"></a>CLIv2

1. [Azure CLI](/cli/azure/install-azure-cli) 'yı yükleyip [oturum açın](/cli/azure/authenticate-azure-cli).

1. IP ağ kurallarını listeleyin.

    ```azurecli
    az storage account network-rule list --resource-group "myresourcegroup" --account-name "mystorageaccount" --query ipRules
    ```

1. Tek bir IP adresi için bir ağ kuralı ekleyin.

    ```azurecli
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
    ```

1. Bir IP adresi aralığı için ağ kuralı ekleyin.

    ```azurecli
    az storage account network-rule add --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
    ```

1. Tek bir IP adresi için bir ağ kuralını kaldırın.

    ```azurecli
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.19"
    ```

1. Bir IP adresi aralığı için ağ kuralını kaldır.

    ```azurecli
    az storage account network-rule remove --resource-group "myresourcegroup" --account-name "mystorageaccount" --ip-address "16.17.18.0/24"
    ```

> [!IMPORTANT]
> [Varsayılan kuralı](#change-the-default-network-access-rule) **Reddet**olarak ayarladığınızdan emin olun veya ağ kurallarının hiçbir etkisi yoktur.

## <a name="exceptions"></a>Özel durumlar

Ağ kuralları, çoğu senaryo için Uygulamalarınız ve verileriniz arasındaki bağlantılar için güvenli bir ortam oluşturmaya yardımcı olur. Ancak, bazı uygulamalar sanal ağ veya IP adresi kuralları aracılığıyla benzersiz bir şekilde yalıtıtılmamış Hizmetleri kullanır. Ancak, tam uygulama işlevselliğini etkinleştirmek için bu hizmetler depolama alanına verilmelidir. Bu gibi durumlarda, verilerinize, günlüklerinize veya analizlerinize erişimi etkinleştirmek için ***Güvenilen Microsoft hizmetlerine Izin ver...*** ayarını kullanabilirsiniz.

### <a name="trusted-microsoft-services"></a>Güvenilen Microsoft Hizmetleri

Bazı Microsoft Hizmetleri, ağ kurallarınıza dahil olmayan ağlardan çalışır. Diğer uygulamalar için ağ kurallarını koruyarak, bu tür güvenilen Microsoft hizmetlerinin depolama hesabına erişiminin bir alt kümesine izin verebilirsiniz. Bu güvenilen hizmetler daha sonra, depolama hesabınıza güvenli bir şekilde bağlanmak için güçlü kimlik doğrulaması kullanabilir. Microsoft Hizmetleri için iki tür güvenilen erişimi etkinliyoruz.

- **Aboneliğinizde kayıtlı**olan bazı hizmetlerin kaynakları, günlük veya yedekleme yazma gibi seçim işlemleri için **aynı abonelikte** depolama hesabınıza erişebilir.
- Kaynak örneğine [**BIR RBAC rolü atayarak**](storage-auth-aad.md#assign-rbac-roles-for-access-rights) , bazı hizmetlerin kaynaklarına, depolama hesabınıza açık erişim verilebilir.


**Güvenilen Microsoft hizmetlerine Izin ver...** ayarını etkinleştirdiğinizde, depolama hesabınızla aynı abonelikte kayıtlı olan aşağıdaki hizmetlerin kaynaklarına, açıklandığı şekilde sınırlı bir işlem kümesi erişimi verilir:

| Hizmet                  | Kaynak sağlayıcısı adı     | İzin verilen işlemler                 |
|:------------------------ |:-------------------------- |:---------------------------------- |
| Azure Backup             | Microsoft. RecoveryServices | IAAS sanal makinelerinde yedeklemeleri çalıştırın ve yönetilmeyen diskleri geri yükler. (yönetilen diskler için gerekli değildir). [Daha fazla bilgi edinin](/azure/backup/backup-introduction-to-azure-backup). |
| Azure Data Box           | Microsoft. DataBox          | Data Box kullanarak Azure 'a veri aktarmayı sağlar. [Daha fazla bilgi edinin](/azure/databox/data-box-overview). |
| Azure DevTest Labs       | Microsoft. DevTestLab       | Özel görüntü oluşturma ve yapıt yüklemesi. [Daha fazla bilgi edinin](/azure/devtest-lab/devtest-lab-overview). |
| Azure Event Grid         | Microsoft. EventGrid        | Blob Storage olay yayımlamayı etkinleştirin ve Event Grid depolama sıralarında yayımlamaya izin verin. [BLOB depolama olayları](/azure/event-grid/event-sources) ve [kuyruklarda yayımlama](/azure/event-grid/event-handlers)hakkında bilgi edinin. |
| Azure Event Hubs         | Microsoft. EventHub         | Event Hubs yakalama ile verileri arşivleme. [Daha fazla bilgi edinin](/azure/event-hubs/event-hubs-capture-overview). |
| Azure Dosya Eşitleme          | Microsoft. Storagessync      | Şirket içi dosya sunucunuzu Azure dosya paylaşımları için bir önbelleğe dönüştürmenizi sağlar. Çok siteli eşitleme, hızlı olağanüstü durum kurtarma ve bulut tarafı yedekleme için izin verme. [Daha fazla bilgi](../files/storage-sync-files-planning.md) |
| Azure HDInsight          | Microsoft. HDInsight        | Yeni bir HDInsight kümesi için varsayılan dosya sisteminin başlangıçtaki içeriğini sağlayın. [Daha fazla bilgi edinin](/azure/hdinsight/hdinsight-hadoop-use-blob-storage). |
| Azure İzleyici            | Microsoft. Insights         | İzleme verilerinin güvenli bir depolama hesabına yazılmasına izin verir [daha fazla bilgi edinin](/azure/monitoring-and-diagnostics/monitoring-roles-permissions-security). |
| Azure Ağı         | Microsoft.Network          | Ağ trafiği günlüklerini depolayın ve çözümleyin. [Daha fazla bilgi edinin](/azure/network-watcher/network-watcher-packet-capture-overview). |
| Azure Site Recovery      | Microsoft. Sıterecovery     | Güvenlik Duvarı özellikli önbellek, kaynak veya hedef depolama hesapları kullanırken Azure IaaS sanal makinelerinin olağanüstü durum kurtarma için çoğaltmayı etkinleştirin.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-enable-replication). |

**Güvenilen Microsoft hizmetlerine Izin ver...** ayarı, bu kaynak örneği için [sistem tarafından atanan yönetilen KIMLIĞE](../../active-directory/managed-identities-azure-resources/overview.md) açıkça bir RBAC rolü atarsanız, aşağıdaki hizmetlerin belirli bir örneğinin depolama hesabına erişmesini sağlar.

| Hizmet                        | Kaynak sağlayıcısı adı          | Amaç                            |
| :----------------------------- | :------------------------------ | :--------------------------------- |
| Azure Data Factory             | Microsoft. DataFactory/Factory | ADF çalışma zamanı aracılığıyla depolama hesaplarına erişime izin verir. |
| Azure Logic Apps               | Microsoft. Logic/iş akışları       | Mantıksal uygulamaların depolama hesaplarına erişmesini sağlar. [Daha fazla bilgi edinin](/azure/logic-apps/create-managed-service-identity#authenticate-access-with-managed-identity.md). |
| Azure Machine Learning Hizmeti | Microsoft.MachineLearningServices | Yetkili Azure Machine Learning çalışma alanları BLOB depolama alanına deneme çıkış, model ve Günlükler yazar. [Daha fazla bilgi edinin](/azure/machine-learning/service/how-to-enable-virtual-network#use-a-storage-account-for-your-workspace). | 
| Azure SQL Veri Ambarı       | Microsoft.Sql                   | PolyBase kullanarak belirli SQL veritabanı örneklerinden verilerin içeri ve dışarı aktarılmasını sağlar. [Daha fazla bilgi edinin](/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview). |
| Azure Stream Analytics         | Microsoft. StreamAnalytics       | Akış işindeki verilerin blob depolamaya yazılmasına izin verir. Bu özellik şu anda önizleme sürümündedir. [Daha fazla bilgi edinin](/azure/stream-analytics/blob-output-managed-identity.md). |


### <a name="storage-analytics-data-access"></a>Depolama Analizi veri erişimi

Bazı durumlarda, ağ sınırının dışında tanılama günlüklerine ve ölçümlerine okuma erişimi de gereklidir. Depolama hesabına güvenilen hizmetler erişimi yapılandırılırken, günlük dosyaları, ölçüm tabloları veya her ikisi için okuma erişimine izin verebilirsiniz. [Depolama analizi ile çalışma hakkında daha fazla bilgi edinin.](/azure/storage/storage-analytics)

### <a name="managing-exceptions"></a>Özel durumları yönetme

Ağ kuralı özel durumlarını Azure portal, PowerShell veya Azure CLı v2 aracılığıyla yönetebilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Güvenli hale getirmek istediğiniz depolama hesabına gidin.

1. **Güvenlik duvarları ve sanal ağlar**adlı ayarlar menüsüne tıklayın.

1. **Seçili ağlardan**erişime izin vermeyi seçtiğinizden emin olun.

1. **Özel durumlar**' ın altında, vermek istediğiniz özel durumları seçin.

1. Değişikliklerinizi uygulamak için **Kaydet** ' e tıklayın.

#### <a name="powershell"></a>PowerShell

1. [Azure PowerShell](/powershell/azure/install-Az-ps) yükleyip [oturum açın](/powershell/azure/authenticate-azureps).

1. Depolama hesabı ağ kuralları için özel durumları görüntüleyin.

    ```powershell
    (Get-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount").Bypass
    ```

1. Özel durumları depolama hesabı ağ kuralları için yapılandırın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -Bypass AzureServices,Metrics,Logging
    ```

1. Depolama hesabı ağ kuralları için özel durumları kaldırın.

    ```powershell
    Update-AzStorageAccountNetworkRuleSet -ResourceGroupName "myresourcegroup" -Name "mystorageaccount" -Bypass None
    ```

> [!IMPORTANT]
> [Varsayılan kuralı](#change-the-default-network-access-rule) **Reddet**olarak ayarladığınızdan emin olun veya özel durumların kaldırılması etkisizdir.

#### <a name="cliv2"></a>CLIv2

1. [Azure CLI](/cli/azure/install-azure-cli) 'yı yükleyip [oturum açın](/cli/azure/authenticate-azure-cli).

1. Depolama hesabı ağ kuralları için özel durumları görüntüleyin.

    ```azurecli
    az storage account show --resource-group "myresourcegroup" --name "mystorageaccount" --query networkRuleSet.bypass
    ```

1. Özel durumları depolama hesabı ağ kuralları için yapılandırın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass Logging Metrics AzureServices
    ```

1. Depolama hesabı ağ kuralları için özel durumları kaldırın.

    ```azurecli
    az storage account update --resource-group "myresourcegroup" --name "mystorageaccount" --bypass None
    ```

> [!IMPORTANT]
> [Varsayılan kuralı](#change-the-default-network-access-rule) **Reddet**olarak ayarladığınızdan emin olun veya özel durumların kaldırılması etkisizdir.

## <a name="next-steps"></a>Sonraki adımlar

[Hizmet uç noktalarında](/azure/virtual-network/virtual-network-service-endpoints-overview)Azure ağ hizmeti uç noktaları hakkında daha fazla bilgi edinin.

Azure [Storage Güvenlik Kılavuzu](storage-security-guide.md)'Nda Azure Storage Security 'de daha ayrıntılı bilgi edinin.
