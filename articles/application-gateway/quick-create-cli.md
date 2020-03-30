---
title: 'Quickstart: CLI kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Web trafiğini arka uç havuzundaki sanal makinelere yönlendiren bir Azure Uygulama Ağ Geçidi oluşturmak için Azure CLI'yi nasıl kullanacağınızı öğrenin.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 03/05/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: f60b26756c0affffbd45c8596fdf73d11ffa8e81
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80239521"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI

Bu hızlı başlangıçta, bir uygulama ağ geçidi oluşturmak için Azure CLI'yi kullanırsınız. Sonra doğru çalıştığından emin olmak için test edin. 

Uygulama ağ geçidi, uygulama web trafiğini arka uç havuzundaki belirli kaynaklara yönlendirir. Dinleyicileri bağlantı noktalarına atar, kurallar oluşturur ve arka uç havuzuna kaynak eklersiniz. Basitlik uğruna, bu makalede, bir ortak ön uç IP, uygulama ağ geçidi, temel bir istek yönlendirme kuralı ve arka uç havuzunda iki sanal makine tek bir site barındırmak için temel bir dinleyici ile basit bir kurulum kullanır.

Bu hızlı başlatmayı [Azure PowerShell](quick-create-powershell.md) veya [Azure portalını](quick-create-portal.md)kullanarak da tamamlayabilirsiniz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Azure CLI sürümü 2.0.4 veya sonraki](/cli/azure/install-azure-cli) (Azure CLI'yi yerel olarak çalıştırıyorsanız).

## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Azure'da, ilgili kaynakları bir kaynak grubuna ayırırsınız. `az group create`Kullanarak bir kaynak grubu oluşturun. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

Azure'un oluşturduğunuz kaynaklar arasında iletişim kurabilmesi için sanal bir ağa ihtiyacı vardır.  Uygulama ağ geçidi alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka kaynaklara izin verilmez.  Uygulama Ağ Geçidi için yeni bir alt ağ oluşturabilir veya varolan bir alt ağ oluşturabilirsiniz. Bu örnekte, biri uygulama ağ geçidi, diğeri arka uç sunucuları için olmak üzere iki alt ağ oluşturursunuz. Uygulama Ağ Geçidi'nin Frontend IP'sini kullanım durumunuza göre Genel veya Özel olacak şekilde yapılandırabilirsiniz. Bu örnekte, bir Public Frontend IP adresi seçersiniz.

Sanal ağ ve alt ağı `az network vnet create`oluşturmak için . Genel `az network public-ip create` IP adresini oluşturmak için çalıştırın.

```azurecli-interactive
az network vnet create \
  --name myVNet \
  --resource-group myResourceGroupAG \
  --location eastus \
  --address-prefix 10.0.0.0/16 \
  --subnet-name myAGSubnet \
  --subnet-prefix 10.0.1.0/24
az network vnet subnet create \
  --name myBackendSubnet \
  --resource-group myResourceGroupAG \
  --vnet-name myVNet   \
  --address-prefix 10.0.2.0/24
az network public-ip create \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --allocation-method Static \
  --sku Standard
```

## <a name="create-the-backend-servers"></a>Arka uç sunucularını oluşturma

Arka uç, NIC'lere, sanal makine ölçek kümelerine, genel IP'lere, dahili IP'lere, tam nitelikli alan adları (FQDN) ve Azure Uygulama Hizmeti gibi çok kiracılı arka uçlara sahip olabilir. Bu örnekte, uygulama ağ geçidi için arka uç sunucuolarak kullanmak üzere iki sanal makine oluşturursunuz. Ayrıca uygulama ağ geçidini test etmek için sanal makinelere IIS yüklersiniz.

#### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için NGINX web sunucusunu sanal makinelere yükleyin. NGINX'i yüklemek ve Linux sanal makinesinde "Hello World" Node.js uygulamasını çalıştırmak için bir bulut init yapılandırma dosyası kullanabilirsiniz. Bulut-init hakkında daha fazla bilgi için [Azure'daki sanal makineler için Cloud-init desteğine](../virtual-machines/linux/using-cloud-init.md)bakın.

Azure Bulut Kabuğu'nda, aşağıdaki yapılandırmayı *bulut-init.txt*adlı bir dosyaya kopyalayıp yapıştırın. Dosyayı oluşturmak için *düzenleyici cloud-init.txt* girin.

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

'ile `az network nic create`ağ arabirimlerini oluşturun. Sanal makineleri oluşturmak için. `az vm create`

```azurecli-interactive
for i in `seq 1 2`; do
  az network nic create \
    --resource-group myResourceGroupAG \
    --name myNic$i \
    --vnet-name myVNet \
    --subnet myBackendSubnet
  az vm create \
    --resource-group myResourceGroupAG \
    --name myVM$i \
    --nics myNic$i \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
done
```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Bir uygulama ağ `az network application-gateway create`geçidi oluşturun. Azure CLI ile bir uygulama ağ geçidi oluşturduğunuzda, kapasite, SKU ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Azure daha sonra ağ arabirimlerinin özel IP adreslerini uygulama ağ geçidinin arka uç havuzuna sunucu olarak ekler.

```azurecli-interactive
address1=$(az network nic show --name myNic1 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
address2=$(az network nic show --name myNic2 --resource-group myResourceGroupAG | grep "\"privateIpAddress\":" | grep -oE '[^ ]+$' | tr -d '",')
az network application-gateway create \
  --name myAppGateway \
  --location eastus \
  --resource-group myResourceGroupAG \
  --capacity 2 \
  --sku Standard_v2 \
  --http-settings-cookie-based-affinity Enabled \
  --public-ip-address myAGPublicIPAddress \
  --vnet-name myVNet \
  --subnet myAGSubnet \
  --servers "$address1" "$address2"
```

Azure'un uygulama ağ geçidini oluşturması 30 dakika kadar sürebilir. Oluşturulduktan sonra, **Uygulama ağ geçidi** sayfasının **Ayarlar** bölümünde aşağıdaki ayarları görüntüleyebilirsiniz:

- **appGatewayBackendPool**: **Backend havuzları** sayfasında yer alır. Gerekli arka uç havuzunu belirtir.
- **appGatewayBackendHttpSettings**: HTTP **ayarları** sayfasında bulunur. Uygulama ağ geçidinin iletişim için bağlantı noktası 80 ve HTTP protokolünü kullandığını belirtir.
- **appGatewayHttpListener**: **Dinleyiciler sayfasında**yer almaktadır. Bu **appGatewayBackendPool**ile ilişkili varsayılan dinleyici belirtir.
- **appGatewayFrontendIP**: **Frontend IP yapılandırmaları** sayfasında yer alır. Bu **appGatewayHttpListener** *myAGPublicIPAddress* atar.
- **kural1**: **Kurallar** sayfasında yer alır. Bu **appGatewayHttpListener**ile ilişkili varsayılan yönlendirme kuralı belirtir.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Azure, uygulama ağ geçidini oluşturmak için bir NGINX web sunucusuna gerek duymasa da, Azure'un uygulama ağ geçidini başarıyla oluşturup oluşturmadığını doğrulamak için bu hızlı başlatmaya yükledin. Yeni uygulama ağ geçidinin genel IP `az network public-ip show`adresini almak için . 

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

Genel IP adresini tarayıcınızın adres çubuğuna kopyalayıp yapıştırın.
    
![Uygulama ağ geçidini test etme](./media/quick-create-cli/application-gateway-nginxtest.png)

Tarayıcıyı yenilediğinizde, ikinci VM'nin adını görmeniz gerekir. Bu, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uçla bağlanabileceğini gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidinde oluşturduğunuz kaynaklara artık ihtiyacınız `az group delete` olmadığında, kaynak grubunu silmek için komutu kullanın. Kaynak grubunu sildiğinizde, uygulama ağ geçidini ve ilgili tüm kaynaklarını da silersiniz.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)

