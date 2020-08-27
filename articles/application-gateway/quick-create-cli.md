---
title: 'Hızlı başlangıç: CLı kullanarak doğrudan web trafiği'
titleSuffix: Azure Application Gateway
description: Bu hızlı başlangıçta, Web trafiğini bir arka uç havuzundaki sanal makinelere yönlendiren bir Azure Application Gateway oluşturmak için Azure CLı 'yı nasıl kullanacağınızı öğreneceksiniz.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 08/27/2020
ms.author: victorh
ms.custom: mvc, devx-track-javascript, devx-track-azurecli
ms.openlocfilehash: b6b2077a71870d619c023b0d66c37ebc043aa3f3
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88961763"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-cli"></a>Hızlı Başlangıç: Azure Application Gateway ile web trafiğini yönlendirme - Azure CLI

Bu hızlı başlangıçta, uygulama ağ geçidi oluşturmak için Azure CLı 'yi kullanırsınız. Ardından, doğru çalıştığından emin olmak için test edersiniz. 

Uygulama ağ geçidi, uygulama Web trafiğini bir arka uç havuzundaki belirli kaynaklara yönlendirir. Bağlantı noktalarına dinleyicileri atar, kurallar oluşturur ve bir arka uç havuzuna kaynak ekleyebilirsiniz. Kolaylık sağlaması için bu makalede, genel ön uç IP 'si olan basit bir kurulum ve uygulama ağ geçidinde tek bir siteyi barındırmak için temel bir dinleyici, temel bir istek yönlendirme kuralı ve arka uç havuzundaki iki sanal makine kullanılmaktadır.

Bu hızlı başlangıcı [Azure PowerShell](quick-create-powershell.md) veya [Azure Portal](quick-create-portal.md)kullanarak da tamamlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Azure CLI sürüm 2.0.4 veya üzeri](/cli/azure/install-azure-cli) (Azure CLI 'yi yerel olarak çalıştırırsanız).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]



## <a name="create-resource-group"></a>Kaynak grubu oluşturma

Azure 'da, ilgili kaynakları bir kaynak grubuna ayırabilirsiniz. Kullanarak bir kaynak grubu oluşturun `az group create` . 

Aşağıdaki örnek *eastus* konumunda *myResourceGroupAG* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroupAG --location eastus
```

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma 

Azure 'un, oluşturduğunuz kaynaklar arasında iletişim kurması için bir sanal ağa ihtiyacı vardır.  Application Gateway alt ağı yalnızca uygulama ağ geçitleri içerebilir. Başka hiçbir kaynağa izin verilmez.  Application Gateway için yeni bir alt ağ oluşturabilir veya var olan bir alt ağı kullanabilirsiniz. Bu örnekte, iki alt ağ oluşturursunuz: biri uygulama ağ geçidi için ve arka uç sunucuları için bir diğeri. Application Gateway ön uç IP 'sini kullanım çalışmanıza göre genel veya özel olacak şekilde yapılandırabilirsiniz. Bu örnekte, genel ön uç IP adresi seçersiniz.

Sanal ağ ve alt ağ oluşturmak için kullanın `az network vnet create` . `az network public-ip create`Genel IP adresini oluşturmak için ' i çalıştırın.

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

Arka uçta NIC 'Ler, sanal makine ölçek kümeleri, genel IP 'Ler, iç IP 'Ler, tam nitelikli etki alanı adları (FQDN) ve Azure App Service gibi çok kiracılı arka uçlar bulunabilir. Bu örnekte, uygulama ağ geçidi için arka uç sunucusu olarak kullanmak üzere iki sanal makine oluşturursunuz. Ayrıca, uygulama ağ geçidini test etmek için sanal makinelere IIS yüklersiniz.

#### <a name="create-two-virtual-machines"></a>İki sanal makine oluşturma

Uygulama ağ geçidinin başarıyla oluşturulduğunu doğrulamak için sanal makinelere NGıNX Web sunucusunu yükler. Bir Cloud-init yapılandırma dosyasını kullanarak NGıNX 'i yükleyebilir ve bir Linux sanal makinesinde bir "Merhaba Dünya" Node.js uygulaması çalıştırabilirsiniz. Cloud-init hakkında daha fazla bilgi için bkz. [Azure 'da sanal makineler Için Cloud-init desteği](../virtual-machines/linux/using-cloud-init.md).

Azure Cloud Shell aşağıdaki yapılandırmayı kopyalayıp *cloud-init.txt*adlı bir dosyaya yapıştırın. Dosyayı oluşturmak için *düzenleyici cloud-init.txt* girin.

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

İle ağ arabirimlerini oluşturun `az network nic create` . Sanal makineleri oluşturmak için kullanırsınız `az vm create` .

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

Kullanarak bir uygulama ağ geçidi oluşturun `az network application-gateway create` . Azure CLı ile bir uygulama ağ geçidi oluşturduğunuzda, kapasite, SKU ve HTTP ayarları gibi yapılandırma bilgilerini belirtirsiniz. Daha sonra Azure, ağ arabirimlerinin özel IP adreslerini uygulama ağ geçidinin arka uç havuzunda sunucu olarak ekler.

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

Azure 'un uygulama ağ geçidini oluşturması 30 dakika kadar sürebilir. Oluşturulduktan sonra, **uygulama ağ geçidi** sayfasının **Ayarlar** bölümünde aşağıdaki ayarları görebilirsiniz:

- **Appgatewaybackendpool**: **arka uç havuzları** sayfasında bulunur. Gerekli arka uç havuzunu belirtir.
- **Appgatewaybackendhttpsettings**: **http ayarları** sayfasında bulunur. Uygulama ağ geçidinin, iletişim için 80 bağlantı noktasını ve HTTP protokolünü kullandığını belirtir.
- **Appgatewayhttplistener**: **dinleyiciler sayfasında**bulunur. **Appgatewaybackendpool**ile ilişkili varsayılan dinleyiciyi belirtir.
- **Appgatewayfrontendıp**: **ön uç IP yapılandırması** sayfasında bulunur. **Appgatewayhttplistener**öğesine *Myagpublicıpaddress* atar.
- **rule1**: **Rules** sayfasında bulunur. **Appgatewayhttplistener**ile ilişkili varsayılan yönlendirme kuralını belirtir.

## <a name="test-the-application-gateway"></a>Uygulama ağ geçidini test etme

Azure, uygulama ağ geçidini oluşturmak için bir NGıNX Web sunucusu gerektirmese de, Azure 'un uygulama ağ geçidini başarıyla oluşturup oluşturmadığını doğrulamak için bu hızlı başlangıç hızlı yüklüdür. Yeni uygulama ağ geçidinin genel IP adresini almak için kullanın `az network public-ip show` . 

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroupAG \
  --name myAGPublicIPAddress \
  --query [ipAddress] \
  --output tsv
```

Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın.
    
![Uygulama ağ geçidini test etme](./media/quick-create-cli/application-gateway-nginxtest.png)

Tarayıcıyı yenilediğinizde ikinci VM 'nin adını görmeniz gerekir. Bu, uygulama ağ geçidinin başarıyla oluşturulduğunu ve arka uca bağlanıp bağlanamayacağını gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Uygulama ağ geçidiyle oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında, `az group delete` kaynak grubunu silmek için komutunu kullanın. Kaynak grubunu sildiğinizde, uygulama ağ geçidini ve ilgili tüm kaynakları da silersiniz.

```azurecli-interactive 
az group delete --name myResourceGroupAG
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure CLI kullanarak bir uygulama ağ geçidi ile web trafiğini yönetme](./tutorial-manage-web-traffic-cli.md)

