---
title: Azure 'da bir güvenlik duvarı oluşturmak için FreeBSD 'nin paket filtresini kullanma
description: Azure 'da FreeBSD 'yi kullanarak bir NAT güvenlik duvarı dağıtmayı öğrenin.
author: KylieLiang
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: eeeb60cc41f96edbf8733468ca0cfd18d2939af7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83652530"
---
# <a name="how-to-use-freebsds-packet-filter-to-create-a-secure-firewall-in-azure"></a>Azure 'da güvenli bir güvenlik duvarı oluşturmak için FreeBSD 'nin paket filtresini kullanma
Bu makalede, ortak Web sunucusu senaryosuna yönelik Azure Resource Manager şablonu aracılığıyla FreeBSD 'nin Packer filtresini kullanarak bir NAT güvenlik duvarının nasıl dağıtılacağı açıklanır.

## <a name="what-is-pf"></a>PF nedir?
PF (Ayrıca, ara filtre), güvenlik duvarı için merkezi bir yazılım olan bir BSD lisanslı durum bilgisi paket filtresidir. PF, hızlı bir şekilde gelişmiştir ve artık diğer kullanılabilir güvenlik duvarlarından daha fazla avantaj sağlar. Ağ adresi çevirisi (NAT), günde bir günden bu yana, bu durumda, ALTQ tümleştirilerek ve PF 'nin yapılandırması aracılığıyla yapılandırılabilir hale getirerek, Paket Zamanlayıcısı ve etkin kuyruk yönetimi PF ile tümleştirilmiştir. Yük devretme ve artıklık için pfsync ve CARP gibi özellikler, oturum kimlik doğrulaması için authpf ve FTP-proxy, zor FTP protokolünün güvenlik düzeyini hafifletmek için de geniş bir şekilde genişletildi. Kısaca PF, güçlü ve özellik açısından zengin bir güvenlik duvarıdır. 

## <a name="get-started"></a>başlarken
Web sunucularınız için bulutta güvenli bir güvenlik duvarı ayarlamayı ilgileniyorsanız, kullanmaya başlayalım. Ayrıca, ağ topolojinizi ayarlamak için bu Azure Resource Manager şablonunda kullanılan komut dosyalarını da uygulayabilirsiniz.
Azure Resource Manager şablonu, PF ve NGINX web sunucusu yüklü ve yapılandırılmış iki FreeBSD sanal makinesi kullanarak NAT/yeniden yönlendirme gerçekleştiren bir FreeBSD sanal makinesini ayarlar. İki Web sunucusu çıkış trafiği için NAT gerçekleştirmeye ek olarak, NAT/yeniden yönlendirme sanal makinesi HTTP isteklerini keser ve hepsini bir kez deneme biçiminde iki Web sunucusuna yönlendirir. VNet, özel yönlendirilebilir olmayan IP adresi alanını (10.0.0.2/24) kullanır ve şablonun parametrelerini değiştirebilirsiniz. Azure Resource Manager şablonu, hedef IP adresine bağlı olarak Azure varsayılan yollarını geçersiz kılmak için kullanılan tek tek yolların bir koleksiyonu olan tüm sanal ağ için bir yol tablosu da tanımlar. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Azure CLı aracılığıyla dağıtma
En son [Azure CLI](/cli/azure/install-az-cli2) 'nın yüklü olması ve [az oturum açma](/cli/azure/reference-index)kullanarak bir Azure hesabında oturum açmış olması gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, konumunda bir kaynak grubu adı oluşturur `myResourceGroup` `West US` .

```azurecli
az group create --name myResourceGroup --location westus
```

Sonra, [az Group Deployment Create](/cli/azure/group/deployment)ile PF-FreeBSD-Setup şablonunu dağıtın. Aynı yol altında azuredeploy.parameters.jsindirin ve, ve gibi kendi kaynak değerlerinizi tanımlayın `adminPassword` `networkPrefix` `domainNamePrefix` . 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Yaklaşık beş dakika sonra, bilgileri alacaksınız `"provisioningState": "Succeeded"` . Ardından, ön uç VM 'ye (NAT) SSH ekleyebilir veya genel IP adresini veya ön uç VM 'nin (NAT) FQDN 'sini kullanarak bir tarayıcıda NGINX web sunucusuna erişebilirsiniz. Aşağıdaki örnek, kaynak grubunda ön uç VM 'sine (NAT) atanan FQDN ve genel IP adresini listeler `myResourceGroup` . 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Sonraki adımlar
Azure 'da kendi NAT 'nizi ayarlamak istiyor musunuz? Açık kaynak, ücretsiz ancak güçlü mi? Daha sonra PF iyi bir seçimdir. PF-FreeBSD-Setup şablonunu kullanarak, Azure 'da ortak Web sunucusu senaryosu için FreeBSD 'un PF 'sini kullanarak bir NAT güvenlik duvarı ayarlamanız yalnızca beş dakika gerekir. 

Azure 'da FreeBSD teklifini öğrenmek isterseniz, [Azure 'Da FreeBSD 'ye giriş](freebsd-intro-on-azure.md)konusuna bakın.

PF hakkında daha fazla bilgi edinmek istiyorsanız, [FreeBSD el kitabı](https://www.freebsd.org/doc/handbook/firewalls-pf.html) veya [PF-User Kılavuzu](https://www.freebsd.org/doc/handbook/firewalls-pf.html)' na bakın.
