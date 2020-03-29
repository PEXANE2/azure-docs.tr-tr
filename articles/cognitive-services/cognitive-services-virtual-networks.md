---
title: Sanal Ağlar
titleSuffix: Azure Cognitive Services
description: Bilişsel Hizmetler kaynaklarınız için katmanlı ağ güvenliğini yapılandırın.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: dapine
ms.openlocfilehash: 0988c8154c63bb408493edf3243078e625c80d53
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371231"
---
# <a name="configure-azure-cognitive-services-virtual-networks"></a>Azure Bilişsel Hizmetleri sanal ağlarını yapılandırma

Azure Bilişsel Hizmetler katmanlı bir güvenlik modeli sağlar. Bu model, Bilişsel Hizmetler hesaplarınızı belirli bir ağ alt kümesine sabitlemenizi sağlar. Ağ kuralları yapılandırıldığında, yalnızca belirtilen ağ kümesi üzerinden veri isteyen uygulamalar hesaba erişebilir. İstek filtreleme ile kaynaklarınıza erişimi sınırlandırabilirsiniz. Yalnızca belirtilen IP adreslerinden, IP aralıklarından veya [Azure Sanal Ağlar'daki](../virtual-network/virtual-networks-overview.md)alt ağlar listesinden kaynaklanan isteklere izin verme. Bu teklifle ilgileniyorsanız, [önizleme erişimi istemeniz](https://aka.ms/cog-svc-vnet-signup)gerekir.

Ağ kuralları etkin olduğunda Bilişsel Hizmetler kaynağına erişen bir uygulama yetkilendirme gerektirir. Yetkilendirme, Azure [Etkin Dizin](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD) kimlik bilgileriyle veya geçerli bir API anahtarıyla desteklenir.

> [!IMPORTANT]
> Bilişsel Hizmetler hesap bilgileriniz için güvenlik duvarı kurallarını açmak varsayılan olarak gelen veri isteklerini engeller. İsteklerin iletilmesi için aşağıdaki koşullardan birinin karşılanması gerekir:
> * İstek, hedef Bilişsel Hizmetler hesabının izin verilen alt net listesinde ki bir Azure Sanal Ağı (VNet) içinde çalışan bir hizmetten kaynaklanmalıdır. VNet kaynaklı isteklerin bitiş noktasının Bilişsel Hizmetler hesabınızın [özel alt etki alanı](cognitive-services-custom-subdomains.md) olarak ayarlanması gerekir.
> * Veya istek, izin verilen bir IP adresleri listesinden kaynaklanmalıdır.
>
> Engellenen istekler arasında diğer Azure hizmetlerinden, Azure portalından, günlük ve metrik hizmetleri gibi hizmetlerden gelenler yer almaktadır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenarios"></a>Senaryolar

Bilişsel Hizmetler kaynağınızı güvence altına almak için, öncelikle varsayılan olarak tüm ağlardan (internet trafiği dahil) trafiğe erişimi reddetmek için bir kural yapılandırmanız gerekir. Ardından, belirli VNet'lerden trafiğe erişim sağlayan kuralları yapılandırmanız gerekir. Bu yapılandırma, uygulamalarınız için güvenli bir ağ sınırı oluşturmanıza olanak tanır. Ayrıca, belirli internet IP adresi aralıklarından trafiğe erişim sağlamak için kuralları yapılandırarak belirli internet veya şirket içi istemcilerden bağlantılar alabilirsiniz.

AĞ kuralları, REST ve WebSocket dahil olmak üzere Azure Bilişsel Hizmetleri'ne yönelik tüm ağ protokollerinde uygulanır. Azure test konsolları gibi araçları kullanarak verilere erişmek için açık ağ kurallarının yapılandırılması gerekir. Ağ kurallarını varolan Bilişsel Hizmetler kaynaklarına veya yeni Bilişsel Hizmetler kaynakları oluşturduğunuzda uygulayabilirsiniz. Ağ kuralları uygulandıktan sonra, tüm istekler için uygulanır.

## <a name="supported-regions-and-service-offerings"></a>Desteklenen bölgeler ve hizmet teklifleri

Aşağıda listelenen Bilişsel Hizmetler için sanal ağ desteği *Orta ABD EUAP,* *Güney Orta ABD,* Doğu *ABD,* *Batı ABD 2,* Kuzey *Avrupa,* *Güney Afrika Kuzey,* *Batı Avrupa,* *Orta Hindistan,* *Avustralya Doğu,* *Batı ABD*ve ABD *Gov Virginia* Azure bölgeleri ile sınırlıdır. Hizmet teklifi burada listelenmemişse, sanal ağları desteklemez.

> [!div class="checklist"]
> * [Anomali Algılayıcısı](./anomaly-detector/index.yml)
> * [BilgisayarLı Vizyon](./computer-vision/index.yml)
> * [Content Moderator](./content-moderator/index.yml)
> * [Özel Görüntü](./custom-vision-service/index.yml)
> * [Yüz](./face/index.yml)
> * [Form Tanıma](./form-recognizer/index.yml)
> * [LUIS](./luis/index.yml)
> * [Kişiselleştirme](./personalizer/index.yml)
> * [Metin Analizi](./text-analytics/index.yml)
> * [QnA Üreticisi](./qnamaker/index.yml)

Aşağıda listelenen Bilişsel Hizmetler için sanal ağ desteği *Orta ABD EUAP,* *Güney Orta ABD,* Doğu *ABD,* *Batı ABD 2*, *Küresel*ve ABD *Gov Virginia* Azure bölgeleri ile sınırlıdır.
> [!div class="checklist"]
> * [Çevirmen Metni](./translator/index.yml)

## <a name="service-tags"></a>Hizmet Etiketleri
Yukarıdaki hizmetler için sanal ağ hizmeti uç noktalarını desteklemenin yanı sıra, Bilişsel Hizmetler giden ağ kuralları yapılandırması için bir hizmet etiketini de destekler. Aşağıdaki hizmetler CognitiveServicesManagement hizmet etiketine dahildir.
> [!div class="checklist"]
> * [Anomali Algılayıcısı](./anomaly-detector/index.yml)
> * [BilgisayarLı Vizyon](./computer-vision/index.yml)
> * [Content Moderator](./content-moderator/index.yml)
> * [Özel Görüntü](./custom-vision-service/index.yml)
> * [Yüz](./face/index.yml)
> * [Form Tanıma](./form-recognizer/index.yml)
> * [LUIS](./luis/index.yml)
> * [Kişiselleştirme](./personalizer/index.yml)
> * [Metin Analizi](./text-analytics/index.yml)
> * [QnA Üreticisi](./qnamaker/index.yml)
> * [Çevirmen Metni](./translator/index.yml)
> * [Konuşma Servisi](./speech-service/index.yml)

## <a name="change-the-default-network-access-rule"></a>Varsayılan ağ erişim kuralını değiştirme

Varsayılan olarak, Bilişsel Hizmetler kaynakları herhangi bir ağdaki istemcilerin bağlantılarını kabul eder. Seçili ağlara erişimi sınırlamak için öncelikle varsayılan eylemi değiştirmeniz gerekir.

> [!WARNING]
> Ağ kurallarında değişiklik yapmak, uygulamalarınızın Azure Bilişsel Hizmetleri'ne bağlanma yeteneğini etkileyebilir. Erişim sağlayan belirli ağ kuralları da uygulanmadığı sürece, varsayılan ağ kuralının verilere tüm erişimi **engellemeyi** **reddetmesi** için ayarlanması. Varsayılan kuralı erişimi reddedecek şekilde değiştirmeden önce ağ kurallarını kullanarak izin verilen ağlara erişim verdiğinizden emin olun. Şirket içi ağınız için IP adreslerini listelemenize izin verdiyseniz, şirket içi ağınızdan olası tüm giden genel IP adreslerini eklediğinizden emin olun.

### <a name="managing-default-network-access-rules"></a>Varsayılan ağ erişim kurallarını yönetme

Azure portalı, PowerShell veya Azure CLI aracılığıyla Bilişsel Hizmetler kaynakları için varsayılan ağ erişim kurallarını yönetebilirsiniz.

# <a name="azure-portal"></a>[Azure portalında](#tab/portal)

1. Güvenli hale getirmek istediğiniz Bilişsel Hizmetler kaynağına gidin.

1. **Sanal ağ**adlı **KAYNAK YÖNETİmİ** menüsünü seçin.

   ![Sanal ağ seçeneği](media/vnet/virtual-network-blade.png)

1. Varsayılan olarak erişimi reddetmek için, **Seçili ağlardan**erişime izin vermeyi seçin. **Seçili ağlar** yalnızca ayar, yapılandırılmış **Sanal ağlar** veya Adres **aralıkları** ile refakatsiz - tüm erişim etkili bir şekilde reddedilir. Tüm erişim reddedildiğinde, Bilişsel Hizmetler kaynağını tüketmeye çalışan isteklere izin verilmez. Azure portalı, Azure PowerShell veya Azure CLI, Bilişsel Hizmetler kaynağını yapılandırmak için kullanılabilir.
1. Tüm ağlardan gelen trafiğe izin vermek için **Tüm ağlardan** erişime izin vermeyi seçin.

   ![Sanal ağlar inkar](media/vnet/virtual-network-deny.png)

1. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

# <a name="powershell"></a>[Powershell](#tab/powershell)

1. Azure [PowerShell'i](/powershell/azure/install-az-ps) yükleyin ve [oturum açın](/powershell/azure/authenticate-azureps)veya **Deneyin'i**seçin.

1. Bilişsel Hizmetler kaynağı için varsayılan kuralın durumunu görüntüleyin.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).DefaultAction
    ```

1. Varsayılan olarak ağ erişimini reddetmek için varsayılan kuralı ayarlayın.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -DefaultAction Deny
    }
    Update-AzCognitiveServicesAccountNetworkRuleSet @parameters
    ```

1. Varsayılan olarak ağ erişimine izin verecek varsayılan kuralı ayarlayın.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -DefaultAction Allow
    }
    Update-AzCognitiveServicesAccountNetworkRuleSet @parameters
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın](/cli/azure/authenticate-azure-cli)veya **Deneyin'i**seçin.

1. Bilişsel Hizmetler kaynağı için varsayılan kuralın durumunu görüntüleyin.

    ```azurecli-interactive
    az cognitiveservices account show \
        -g "myresourcegroup" -n "myaccount" \
        --query networkRuleSet.defaultAction
    ```

1. Varsayılan olarak ağ erişimini reddetmek için varsayılan kuralı ayarlayın.

    ```azurecli-interactive
    az cognitiveservices account update \
        -g "myresourcegroup" -n "myaccount" \
        --default-action Deny
    ```

1. Varsayılan olarak ağ erişimine izin verecek varsayılan kuralı ayarlayın.

    ```azurecli-interactive
    az cognitiveservices account update \
        -g "myresourcegroup" -n "myaccount" \
        --default-action Allow
    ```

***

## <a name="grant-access-from-a-virtual-network"></a>Sanal ağdan erişim izni verme

Bilişsel Hizmetler kaynaklarını yalnızca belirli alt ağlardan erişime izin verecek şekilde yapılandırabilirsiniz. İzin verilen alt ağlar, aynı abonelikteki bir VNet'e veya farklı bir Azure Etkin Dizin kiracısına ait abonelikler de dahil olmak üzere farklı bir abonelikte bulunabilir.

VNet içindeki Azure Bilişsel Hizmetler için bir [hizmet bitiş noktası](../virtual-network/virtual-network-service-endpoints-overview.md) etkinleştirin. Hizmet bitiş noktası, VNet'ten Azure Bilişsel Hizmetler hizmetine en uygun yoldan trafiği yönlendirir. Alt ağ ve sanal ağ kimlikleri de her istek ile iletilir. Yöneticiler daha sonra, Bir VNet'teki belirli alt ağlardan istekalınmasına izin veren Bilişsel Hizmetler kaynağı için ağ kurallarını yapılandırabilir. Bu ağ kuralları yla erişim izni verilen istemciler, verilere erişmek için Bilişsel Hizmetler kaynağının yetkilendirme gereksinimlerini karşılamaya devam etmelidir.

Her Bilişsel Hizmetler kaynağı, [IP ağ kurallarıyla](#grant-access-from-an-internet-ip-range)birleştirilebilen 100'e kadar sanal ağ kuralını destekler.

### <a name="required-permissions"></a>Gerekli izinler

Bir Sanal ağ kuralını Bilişsel Hizmetler kaynağına uygulamak için, kullanıcının alt ağlar için uygun izinlere sahip olması gerekir. Gerekli izin varsayılan *Katılımcı* rolü veya *Bilişsel Hizmetler Katılımcısı* rolüdür. Gerekli izinler özel rol tanımlarına da eklenebilir.

Bilişsel Hizmetler kaynağı ve erişim verilen sanal ağlar, farklı bir Azure AD kiracısının parçası olan abonelikler de dahil olmak üzere farklı aboneliklerde olabilir.

> [!NOTE]
> Farklı bir Azure Etkin Dizin kiracısının parçası olan sanal ağlardaki alt ağlara erişim sağlayan kuralların yapılandırması şu anda yalnızca Powershell, CLI ve REST API'leri aracılığıyla desteklenir. Bu tür kurallar Azure portalı üzerinden yapılandırılamaz, ancak portalda görüntülenebilir.

### <a name="managing-virtual-network-rules"></a>Sanal ağ kurallarını yönetme

Azure portalı, PowerShell veya Azure CLI aracılığıyla Bilişsel Hizmetler kaynakları için sanal ağ kurallarını yönetebilirsiniz.

# <a name="azure-portal"></a>[Azure portalında](#tab/portal)

1. Güvenli hale getirmek istediğiniz Bilişsel Hizmetler kaynağına gidin.

1. **Sanal ağ**adlı **KAYNAK YÖNETİmİ** menüsünü seçin.

1. **Seçili ağlardan**erişime izin vermek için seçtiğinizden denetlemeyin.

1. **Sanal ağlar**altında varolan bir ağ kuralına sahip sanal ağa erişim sağlamak için **varolan sanal ağ ekle'yi**seçin.

   ![Varolan vNet ekle](media/vnet/virtual-network-add-existing.png)

1. Sanal **ağlar** ve **Alt Ağlar** seçeneklerini seçin ve sonra **Etkinleştir'i**seçin.

   ![Varolan vNet ayrıntılarını ekleme](media/vnet/virtual-network-add-existing-details.png)

1. Yeni bir sanal ağ oluşturmak ve ona erişim sağlamak için **yeni sanal ağ ekle'yi**seçin.

   ![Yeni vNet ekle](media/vnet/virtual-network-add-new.png)

1. Yeni sanal ağ oluşturmak için gerekli bilgileri sağlayın ve ardından **Oluştur'u**seçin.

   ![vNet oluşturma](media/vnet/virtual-network-create.png)

    > [!NOTE]
    > Azure Bilişsel Hizmetleri için bir hizmet bitiş noktası seçili sanal ağ ve alt ağlar için önceden yapılandırılmamışsa, bu işlemin bir parçası olarak yapılandırabilirsiniz.
    >
    > Şu anda, kural oluşturma sırasında seçim için yalnızca aynı Azure Etkin Dizin kiracısına ait sanal ağlar gösterilir. Başka bir kiracıya ait sanal ağdaki bir alt ağa erişim sağlamak için lütfen Powershell, CLI veya REST API'lerini kullanın.

1. Sanal ağ veya alt ağ kuralını kaldırmak için sanal ağ veya alt ağ için bağlam menüsünü açmak için **...** seçeneğini seçin ve **Kaldır'ı**seçin.

   ![vNet'i kaldırma](media/vnet/virtual-network-remove.png)

1. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

# <a name="powershell"></a>[Powershell](#tab/powershell)

1. Azure [PowerShell'i](/powershell/azure/install-az-ps) yükleyin ve [oturum açın](/powershell/azure/authenticate-azureps)veya **Deneyin'i**seçin.

1. Sanal ağ kurallarını listele.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).VirtualNetworkRules
    ```

1. Varolan bir sanal ağ ve alt ağda Azure Bilişsel Hizmetleri için hizmet bitiş noktasını etkinleştirin.

    ```azurepowershell-interactive
    Get-AzVirtualNetwork -ResourceGroupName "myresourcegroup" `
        -Name "myvnet" | Set-AzVirtualNetworkSubnetConfig -Name "mysubnet" `
        -AddressPrefix "10.0.0.0/24" `
        -ServiceEndpoint "Microsoft.CognitiveServices" | Set-AzVirtualNetwork
    ```

1. Sanal ağ ve alt ağ için ağ kuralı ekleyin.

    ```azurepowershell-interactive
    $subParameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myvnet"
    }
    $subnet = Get-AzVirtualNetwork @subParameters | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"

    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -VirtualNetworkResourceId $subnet.Id
    }
    Add-AzCognitiveServicesAccountNetworkRule @parameters
    ```

    > [!TIP]
    > Başka bir Azure AD kiracısına ait bir VNet'teki bir alt ağ için ağ kuralı eklemek için "/abonelikler/abonelik-id/kaynakGruplar/kaynakGrup-Ad/sağlayıcılar/Microsoft.Network/virtualNetworks/vNet adı/alt ağlar/alt net adı" şeklinde tam nitelikli **VirtualNetworkResourceId** parametresi kullanın.

1. Sanal ağ ve alt ağ için ağ kuralını kaldırın.

    ```azurepowershell-interactive
    $subParameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myvnet"
    }
    $subnet = Get-AzVirtualNetwork @subParameters | Get-AzVirtualNetworkSubnetConfig -Name "mysubnet"

    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -VirtualNetworkResourceId $subnet.Id
    }
    Remove-AzCognitiveServicesAccountNetworkRule @parameters
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın](/cli/azure/authenticate-azure-cli)veya **Deneyin'i**seçin.

1. Sanal ağ kurallarını listele.

    ```azurecli-interactive
    az cognitiveservices account network-rule list \
        -g "myresourcegroup" -n "myaccount" \
        --query virtualNetworkRules
    ```

1. Varolan bir sanal ağ ve alt ağda Azure Bilişsel Hizmetleri için hizmet bitiş noktasını etkinleştirin.

    ```azurecli-interactive
    az network vnet subnet update -g "myresourcegroup" -n "mysubnet" \
    --vnet-name "myvnet" --service-endpoints "Microsoft.CognitiveServices"
    ```

1. Sanal ağ ve alt ağ için ağ kuralı ekleyin.

    ```azurecli-interactive
    $subnetid=(az network vnet subnet show \
        -g "myresourcegroup" -n "mysubnet" --vnet-name "myvnet" \
        --query id --output tsv)

    # Use the captured subnet identifier as an argument to the network rule addition
    az cognitiveservices account network-rule add \
        -g "myresourcegroup" -n "myaccount" \
        --subnet $subnetid
    ```

    > [!TIP]
    > Başka bir Azure AD kiracısına ait bir VNet'teki alt ağ için kural eklemek için "/abonelikler/abonelikler/abonelik-ID/kaynakGruplar/kaynakGrup Adı/Sağlayıcıları/Microsoft.Network/virtualNetworks/vNet-name/subnets/subnet adı" şeklinde tam nitelikli bir alt net kimliği kullanın.
    > 
    > **Abonelik** parametresini, başka bir Azure AD kiracısına ait bir VNet'in alt net kimliğini almak için kullanabilirsiniz.

1. Sanal ağ ve alt ağ için ağ kuralını kaldırın.

    ```azurecli-interactive
    $subnetid=(az network vnet subnet show \
        -g "myresourcegroup" -n "mysubnet" --vnet-name "myvnet" \
        --query id --output tsv)

    # Use the captured subnet identifier as an argument to the network rule removal
    az cognitiveservices account network-rule remove \
        -g "myresourcegroup" -n "myaccount" \
        --subnet $subnetid
    ```
***

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**için ayarladığından veya ağ [kurallarının](#change-the-default-network-access-rule) hiçbir etkisi olmadığından emin olun.

## <a name="grant-access-from-an-internet-ip-range"></a>Bir internet IP aralığından erişim izni verme

Belirli genel internet IP adresi aralıklarından erişime izin verecek şekilde Bilişsel Hizmetler kaynaklarını yapılandırabilirsiniz. Bu yapılandırma, genel internet trafiğini etkili bir şekilde engelleyerek belirli hizmetlere ve şirket içi ağlara erişim sağlar.

Formda `16.17.18.0/24` VEYA tek `16.17.18.19`tek IP adresleri olarak [CIDR gösterimini](https://tools.ietf.org/html/rfc4632) kullanarak izin verilen internet adresi aralıkları sağlayın.

   > [!Tip]
   > "/31" veya "/32" öneki boyutlarını kullanan küçük adres aralıkları desteklenmez. Bu aralıklar tek tek IP adresi kuralları kullanılarak yapılandırılmalıdır.

IP ağ kurallarına yalnızca **genel internet** IP adresleri için izin verilir. Özel ağlar için ayrılmış IP adresi aralıklarına [(RFC 1918'de](https://tools.ietf.org/html/rfc1918#section-3)tanımlandığı gibi) IP kurallarında izin verilmez. Özel ağlar, , , `10.*` `172.16.*`  -  `172.31.*`ve `192.168.*`. ile başlayan adresleri içerir.

   > [!NOTE]
   > IP ağ kurallarının Bilişsel Hizmetler kaynağıyla aynı Azure bölgesinden gelen istekler üzerinde hiçbir etkisi yoktur. Aynı bölge isteklerine izin vermek için [Sanal ağ kurallarını](#grant-access-from-a-virtual-network) kullanın.

Şu anda yalnızca IPV4 adresleri desteklenir. Her Bilişsel Hizmetler kaynağı, [Sanal ağ kurallarıyla](#grant-access-from-a-virtual-network)birleştirilebilir 100'e kadar IP ağ kuralını destekler.

### <a name="configuring-access-from-on-premises-networks"></a>Şirket içi ağlardan erişimi yapılandırma

Şirket içi ağlarınızdan, IP ağ kuralıyla Bilişsel Hizmetler kaynağınıza erişim sağlamak için, ağınız tarafından kullanılan internete bakan IP adreslerini belirlemeniz gerekir. Yardım için ağ yöneticinize başvurun.

Public-peering veya Microsoft bakmak için şirket içi [ExpressRoute](../expressroute/expressroute-introduction.md) kullanıyorsanız, NAT IP adreslerini tanımlamanız gerekir. Genel bakış için, her ExpressRoute devresi varsayılan olarak iki NAT IP adresi kullanır. Trafik Microsoft Azure ağ omurgasına girdiğinde her biri Azure hizmet trafiğine uygulanır. Microsoft'un eşleçliği için, kullanılan NAT IP adresleri müşteri tarafından sağlanan veya hizmet sağlayıcısı tarafından sağlanır. Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir. Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). [ExpressRoute genel ve Microsoft eşlemesi için NAT](../expressroute/expressroute-nat.md#nat-requirements-for-azure-public-peering) hakkında daha fazla bilgi edinin.

### <a name="managing-ip-network-rules"></a>IP ağ kurallarını yönetme

Bilişsel Hizmetler kaynakları için IP ağ kurallarını Azure portalı, PowerShell veya Azure CLI üzerinden yönetebilirsiniz.

# <a name="azure-portal"></a>[Azure portalında](#tab/portal)

1. Güvenli hale getirmek istediğiniz Bilişsel Hizmetler kaynağına gidin.

1. **Sanal ağ**adlı **KAYNAK YÖNETİmİ** menüsünü seçin.

1. **Seçili ağlardan**erişime izin vermek için seçtiğinizden denetlemeyin.

1. Bir internet IP aralığına erişim sağlamak **için, Firewall** > **Adres Aralığı'nın**altındaki IP adresini veya adres aralığını [(CIDR formatında)](https://tools.ietf.org/html/rfc4632)girin. Yalnızca geçerli genel IP (rezerve edilmeyen) adresler kabul edilir.

   ![IP aralığı ekle](media/vnet/virtual-network-add-ip-range.png)

1. IP ağ kuralını kaldırmak için, <span class="docon docon-delete x-hidden-focus"></span> adres aralığının yanındaki çöp kutusu simgesini seçin.

   ![IP aralığını silme](media/vnet/virtual-network-delete-ip-range.png)

1. Değişikliklerinizi uygulamak için **Kaydet**’i seçin.

# <a name="powershell"></a>[Powershell](#tab/powershell)

1. Azure [PowerShell'i](/powershell/azure/install-az-ps) yükleyin ve [oturum açın](/powershell/azure/authenticate-azureps)veya **Deneyin'i**seçin.

1. IP ağ kurallarını listele.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
    }
    (Get-AzCognitiveServicesAccountNetworkRuleSet @parameters).IPRules
    ```

1. Tek bir IP adresi için ağ kuralı ekleyin.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.19"
    }
    Add-AzCognitiveServicesAccountNetworkRule @parameters
    ```

1. IP adresi aralığı için ağ kuralı ekleyin.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.0/24"
    }
    Add-AzCognitiveServicesAccountNetworkRule @parameters
    ```

1. Tek bir IP adresi için ağ kuralını kaldırın.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.19"
    }
    Remove-AzCognitiveServicesAccountNetworkRule @parameters
    ```

1. IP adresi aralığı için ağ kuralını kaldırın.

    ```azurepowershell-interactive
    $parameters = @{
        -ResourceGroupName "myresourcegroup"
        -Name "myaccount"
        -IPAddressOrRange "16.17.18.0/24"
    }
    Remove-AzCognitiveServicesAccountNetworkRule @parameters
    ```

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Azure [CLI'yi](/cli/azure/install-azure-cli) yükleyin ve [oturum açın](/cli/azure/authenticate-azure-cli)veya **Deneyin'i**seçin.

1. IP ağ kurallarını listele.

    ```azurecli-interactive
    az cognitiveservices account network-rule list \
        -g "myresourcegroup" -n "myaccount" --query ipRules
    ```

1. Tek bir IP adresi için ağ kuralı ekleyin.

    ```azurecli-interactive
    az cognitiveservices account network-rule add \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.19"
    ```

1. IP adresi aralığı için ağ kuralı ekleyin.

    ```azurecli-interactive
    az cognitiveservices account network-rule add \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.0/24"
    ```

1. Tek bir IP adresi için ağ kuralını kaldırın.

    ```azurecli-interactive
    az cognitiveservices account network-rule remove \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.19"
    ```

1. IP adresi aralığı için ağ kuralını kaldırın.

    ```azurecli-interactive
    az cognitiveservices account network-rule remove \
        -g "myresourcegroup" -n "myaccount" \
        --ip-address "16.17.18.0/24"
    ```

***

> [!IMPORTANT]
> Varsayılan kuralı **reddetmek**için ayarladığından veya ağ [kurallarının](#change-the-default-network-access-rule) hiçbir etkisi olmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

* Çeşitli [Azure Bilişsel Hizmetlerini](welcome.md) keşfedin
* [Azure Sanal Ağ Hizmeti Bitiş Noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) hakkında daha fazla bilgi edinin