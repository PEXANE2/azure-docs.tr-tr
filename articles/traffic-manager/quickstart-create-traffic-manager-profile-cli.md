---
title: Quickstart:HA uygulamaları için bir profil oluşturma - Azure CLI - Azure Trafik Yöneticisi
description: Bu hızlı başlangıç makalesi, yüksek kullanılabilir bir web uygulaması oluşturmak için trafik yöneticisi profilinin nasıl oluşturulacağını açıklar.
services: traffic-manager
author: rohinkoul
mnager: twooley
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.service: traffic-manager
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/29/2019
ms.author: rohink
ms.openlocfilehash: e19850243498fc24c9a726f4603590df15f3a046
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79531524"
---
# <a name="quickstart-create-a-traffic-manager-profile-for-a-highly-available-web-application-using-azure-cli"></a>Hızlı başlatma: Azure CLI kullanarak yüksek kullanılabilir bir web uygulaması için Trafik Yöneticisi profili oluşturma

Bu hızlı başlatma, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Trafik Yöneticisi profilinin nasıl oluşturulacağını açıklar.

Bu hızlı başlangıçta, bir web uygulamasının iki örneğini oluşturursunuz. Her biri farklı bir Azure bölgesinde çalışıyor. [Bitiş noktası önceliğine](traffic-manager-routing-methods.md#priority-traffic-routing-method)dayalı bir Trafik Yöneticisi profili oluşturursunuz. Profil, kullanıcı trafiğini web uygulamasını çalıştıran birincil siteye yönlendirir. Trafik Yöneticisi web uygulamasını sürekli olarak izler. Birincil site kullanılamıyorsa, yedekleme sitesine otomatik olarak başarısız olur.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.28 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive

  az group create \
    --name myResourceGroup \
    --location eastus

```

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Son nokta önceliğine göre kullanıcı trafiğini yönlendiren [az ağ trafik yöneticisi profilini](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile?view=azure-cli-latest#az-network-traffic-manager-profile-create) kullanarak bir Trafik Yöneticisi profili oluşturun.

Aşağıdaki örnekte, **<profile_name>'yi** benzersiz bir Trafik Yöneticisi profil adı ile değiştirin.

```azurecli-interactive

az network traffic-manager profile create \
    --name <profile_name> \
    --resource-group myResourceGroup \
    --routing-method Priority \
    --path "/" \
    --protocol HTTP \
    --unique-dns-name <profile_name> \
    --ttl 30 \
    --port 80

```

## <a name="create-web-apps"></a>Web uygulamaları oluşturma

Bu hızlı başlangıç için, iki farklı Azure bölgesinde *(Doğu ABD* ve *Batı Avrupa)* dağıtılan bir web uygulamasının iki örneğine ihtiyacınız vardır. Her biri Trafik Yöneticisi için birincil ve başarısız uç nokta olarak hizmet verecektir.

### <a name="create-web-app-service-plans"></a>Web uygulaması hizmet planları oluşturma
İki farklı Azure bölgesinde dağıtacağınız web uygulamasının iki örneği için az uygulama hizmeti planını kullanarak web uygulaması hizmeti planları [oluşturun.](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create)

Aşağıdaki örnekte, **<appspname_eastus>** ve **<appspname_westeurope>'yi** benzersiz bir Uygulama Hizmeti Planı Adı ile değiştirin

```azurecli-interactive

az appservice plan create \
    --name <appspname_eastus> \
    --resource-group myResourceGroup \
    --location eastus \
    --sku S1

az appservice plan create \
    --name <appspname_westeurope> \
    --resource-group myResourceGroup \
    --location westeurope \
    --sku S1

```

### <a name="create-a-web-app-in-the-app-service-plan"></a>Uygulama hizmet planında bir web uygulaması oluşturma
*Doğu ABD* ve *Batı Avrupa* Azure bölgelerindeki Uygulama Hizmeti planlarında az [webapp oluşturmak](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) kullanarak web uygulaması iki örnek oluşturun.

Aşağıdaki örnekte, **<app1name_eastus>** ve **<app2name_westeurope>'ı** benzersiz bir Uygulama Adı ile değiştirin ve<**appspname_eastus>** ve<**appspname_westeurope>'i** önceki bölümde App Service planlarını oluşturmak için kullanılan adla değiştirin.

```azurecli-interactive

az webapp create \
    --name <app1name_eastus> \
    --plan <appspname_eastus> \
    --resource-group myResourceGroup

az webapp create \
    --name <app2name_westeurope> \
    --plan <appspname_westeurope> \
    --resource-group myResourceGroup

```

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager uç noktalarını ekleme
Trafik Yöneticisi profiline az ağ [trafik yöneticisi bitiş noktası nı](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-create) kullanarak Trafik Yöneticisi uç noktaları olarak iki Web Apps ekleyin:

- Web App kimliğini belirleyin ve *Doğu ABD* Azure bölgesinde bulunan Web Uygulamasını tüm kullanıcı trafiğini yönlendirmek için birincil bitiş noktası olarak ekleyin. 
- Web App kimliğini belirleyin ve *Batı Avrupa* Azure bölgesinde bulunan Web Uygulamasını bitiş noktası olarak ekleyin. 

Birincil bitiş noktası kullanılamıyorsa, trafik otomatik olarak başarısız bitiş noktasına yönlendirir.

Aşağıdaki örnekte, **<app1name_eastus>** ve **<app2name_westeurope>** önceki bölümdeki her bölge için oluşturulan Uygulama Adları ile değiştirin,<appspname_eastus **>** ve appspname_westeurope>önceki bölümde App Service planlarını oluşturmak için kullanılan adla **<** ve profile_name **<>** önceki bölümde kullanılan profil adı ile değiştirin. 

**Doğu ABD bitiş noktası**

```azurecli-interactive

az webapp show \
    --name <app1name_eastus> \
    --resource-group myResourceGroup \
    --query id

```

Çıkışta görüntülenen id'e dikkat edin ve bitiş noktasını eklemek için aşağıdaki komutu kullanın:

```azurecli-interactive

az network traffic-manager endpoint create \
    --name <app1name_eastus> \
    --resource-group myResourceGroup \
    --profile-name <profile_name> \
    --type azureEndpoints \
    --target-resource-id <ID from az webapp show> \
    --priority 1 \
    --endpoint-status Enabled
```

**Batı Avrupa bitiş noktası**

```azurecli-interactive

az webapp show \
    --name <app2name_westeurope> \
    --resource-group myResourceGroup \
    --query id

```

Çıkışta görüntülenen id'e dikkat edin ve bitiş noktasını eklemek için aşağıdaki komutu kullanın:

```azurecli-interactive

az network traffic-manager endpoint create \
    --name <app1name_westeurope> \
    --resource-group myResourceGroup \
    --profile-name <profile_name> \
    --type azureEndpoints \
    --target-resource-id <ID from az webapp show> \
    --priority 2 \
    --endpoint-status Enabled

```

## <a name="test-your-traffic-manager-profile"></a>Trafik Yöneticisi profilinizi test edin

Bu bölümde, Trafik Yöneticisi profilinizin alan adını kontrol eedeceksiniz. Ayrıca, birincil bitiş noktasını kullanılamaz olarak da yapılandıracaksınız. Son olarak, web uygulamasının hala kullanılabilir olduğunu görebilirsiniz. Çünkü Trafik Yöneticisi trafiği başarısız bitiş noktasına gönderir.

Aşağıdaki örnekte, **<app1name_eastus>** ve **<app2name_westeurope>** önceki bölümdeki her bölge için oluşturulan Uygulama Adları ile değiştirin,<appspname_eastus **>** ve appspname_westeurope>önceki bölümde App Service planlarını oluşturmak için kullanılan adla **<** ve profile_name **<>** önceki bölümde kullanılan profil adı ile değiştirin.

### <a name="determine-the-dns-name"></a>DNS adını belirleme

Az network traffic manager profilini kullanarak Trafik Yöneticisi profilinin DNS adını [belirleyin.](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile?view=azure-cli-latest#az-network-traffic-manager-profile-show)

```azurecli-interactive

az network traffic-manager profile show \
    --name <profile_name> \
    --resource-group myResourceGroup \
    --query dnsConfig.fqdn

```

**RelativeDnsName** değerini kopyalayın. Trafik Yöneticisi profilinin DNS adı *http://<* relativednsname *>.trafficmanager.net*. 

### <a name="view-traffic-manager-in-action"></a>Traffic Manager'ın nasıl çalıştığını görün
1. Web Uygulamanızın varsayılan web sitesini görüntülemek için trafik yöneticisi profilinizin DNS adını *(http://<* relativednsname *>.trafficmanager.net)* girin.

    > [!NOTE]
    > Bu hızlı başlangıç senaryosunda, tüm istekler birincil bitiş noktasına yönlendirin. **Öncelik 1**olarak ayarlanır.
2. Trafik Yöneticisi'nin başarısız olduğunu görüntülemek [için, az ağ trafik yöneticisi bitiş noktası güncelleştirmesini](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update)kullanarak birincil sitenizi devre dışı edin.

   ```azurecli-interactive

    az network traffic-manager endpoint update \
        --name <app1name_eastus> \
        --resource-group myResourceGroup \
        --profile-name <profile_name> \
        --type azureEndpoints \
        --endpoint-status Disabled
    
   ```

3. Web sitesini yeni bir web tarayıcısı oturumunda görüntülemek için Trafik Yöneticisi profilinizin DNS adını *(http://<* relativednsname *>.trafficmanager.net)* kopyalayın.
4. Web uygulamasının hala kullanılabilir olduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşi bittiğinde, az grubu kullanarak kaynak gruplarını, web uygulamalarını ve ilgili tüm kaynakları [silin.](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete)

```azurecli-interactive

az group delete \
    --resource-group myResourceGroup

```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, web uygulamanız için yüksek kullanılabilirlik sağlayan bir Trafik Yöneticisi profili oluşturdunuz. Trafiği yönlendirme hakkında daha fazla bilgi edinmek için Trafik Yöneticisi öğreticilerine devam edin.

> [!div class="nextstepaction"]
> [Traffic Manager öğreticileri](tutorial-traffic-manager-improve-website-response.md)
