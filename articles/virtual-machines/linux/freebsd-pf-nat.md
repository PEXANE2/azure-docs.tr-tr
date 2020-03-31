---
title: Azure'da güvenlik duvarı oluşturmak için FreeBSD'nin Paket Filtresi'ni kullanma
description: Azure'da FreeBSD'nin PF'sini kullanarak NAT güvenlik duvarLarını nasıl dağıtacaklarını öğrenin.
author: KylieLiang
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 9b78c0d93b57a3e3f4963088d0b93f121f57483c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78945101"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Azure'da güvenli bir güvenlik duvarı oluşturmak için FreeBSD'nin Paket Filtresi nasıl kullanılır?
Bu makalede, ortak web sunucusu senaryosu için Azure Kaynak Yöneticisi şablonu aracılığıyla FreeBSD'nin Packer Filtresini kullanarak nat güvenlik duvarının nasıl dağıtılancayacağın açıklanmaktadır.

## <a name="what-is-pf"></a>PF nedir?
PF (Packet Filter, ayrıca pf yazılı) bir BSD lisanslı stateful paket filtresi, güvenlik duvarı için yazılım merkezi bir parçası. PF o zamandan beri hızlı bir şekilde gelişti ve şimdi diğer mevcut güvenlik duvarları üzerinde çeşitli avantajları vardır. Ağ Adresi Çevirisi (NAT) ilk günden beri PF'dedir, daha sonra paket zamanlayıcısı ve etkin sıra yönetimi, ALTQ'yu entegre ederek ve PF'nin yapılandırması yoluyla yapılandırılabilir hale getirerek PF'ye entegre edilmiştir. Başarısızlık ve artıklık için pfsync ve CARP, oturum kimlik doğrulaması için authpf ve zor FTP protokolünü güvenlik duvarını rahatlatmak için ftp-proxy gibi özellikler de PF'yi genişletti. Kısacası, PF güçlü ve zengin özelliklere sahip bir güvenlik duvarıdır. 

## <a name="get-started"></a>Kullanmaya başlayın
Web sunucularınız için bulutta güvenli bir güvenlik duvarı kurmak istiyorsanız, başlayalım. Ağ topolojinizi ayarlamak için bu Azure Kaynak Yöneticisi şablonunda kullanılan komut dosyalarını da uygulayabilirsiniz.
Azure Kaynak Yöneticisi şablonu, PF kullanarak NAT/yeniden yönlendirme gerçekleştiren bir FreeBSD sanal makine ve Nginx web sunucusu yüklü ve yapılandırılmış iki FreeBSD sanal makineyi ayarlar. İki web sunucusu için NAT gerçekleştirmenin yanı sıra, NAT/yeniden yönlendirme sanal makine HTTP isteklerini keser ve bunları iki web sunucusuna round-robin şekilde yönlendirir. VNet, özel non-routable IP adresi alanı 10.0.0.2/24 kullanır ve şablonun parametrelerini değiştirebilirsiniz. Azure Kaynak Yöneticisi şablonu, hedef IP adresini temel alan Azure varsayılan rotalarını geçersiz kılmak için kullanılan tek tek yolların bir koleksiyonu olan tüm VNet için bir rota tablosu da tanımlar. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Azure CLI ile dağıtma
En son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, `myResourceGroup` `West US` konumda bir kaynak grubu adı oluşturur.

```azurecli
az group create --name myResourceGroup --location westus
```

Daha sonra, az grup dağıtımı oluşturmak ile şablon [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) [dağıtın.](/cli/azure/group/deployment) [Azuredeploy.parameters.json'u](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) aynı yol altında indirin ve `adminPassword`, `networkPrefix`, `domainNamePrefix`ve . 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Yaklaşık beş dakika sonra, bilgi `"provisioningState": "Succeeded"`alırsınız . Daha sonra ön uç VM (NAT) veya ön uç VM (NAT) fqdn kamu IP adresi veya FQDN kullanarak bir tarayıcıda Nginx web sunucusuna erişmek için ssh olabilir. Aşağıdaki örnekte, kaynak grubunda ön uç VM'ye (NAT) atanan `myResourceGroup` FQDN ve genel IP adresi listelenebedilir. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Sonraki adımlar
Azure'da kendi NAT'nizi kurmak istiyor musunuz? Açık Kaynak, ücretsiz ama güçlü? O zaman PF iyi bir seçim. [Pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup)şablonunu kullanarak, ortak web sunucusu senaryosu için Azure'da FreeBSD'nin PF'sini kullanarak round-robin yük dengelemesine sahip bir NAT güvenlik duvarı kurmak için yalnızca beş dakikanız vardır. 

Azure'da FreeBSD teklifini öğrenmek istiyorsanız, [Azure'da FreeBSD'ye giriş bölümüne](freebsd-intro-on-azure.md)bakın.

PF hakkında daha fazla şey öğrenmek istiyorsanız, [FreeBSD el kitabına](https://www.freebsd.org/doc/handbook/firewalls-pf.html) veya [PF-Kullanıcı Kılavuzu'na](https://www.freebsd.org/doc/handbook/firewalls-pf.html)bakın.
