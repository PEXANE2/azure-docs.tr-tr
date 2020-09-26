---
title: 'Hızlı başlangıç: Azure ön kapısı ile yüksek kullanılabilirliği ayarlama-Azure CLı'
description: Bu hızlı başlangıç, Azure CLı kullanarak yüksek kullanılabilirlik ve yüksek performanslı küresel bir Web uygulaması oluşturmak için Azure ön kapısını nasıl kullanacağınızı gösterir.
services: front-door
author: duongau
manager: KumudD
Customer intent: As an IT admin, I want to direct user traffic to ensure high availability of web applications.
ms.assetid: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/21/2020
ms.author: duau
ms.openlocfilehash: 6aa960837a3bfc7f8a04ca1f554fb10d635c2ea2
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91349212"
---
# <a name="quickstart-create-a-front-door-for-a-highly-available-global-web-application-using-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak yüksek oranda kullanılabilir küresel bir Web uygulaması için ön kapı oluşturma

Yüksek oranda kullanılabilir ve yüksek performanslı bir genel Web uygulaması oluşturmak için Azure CLı kullanarak Azure ön kapısını kullanmaya başlayın.

Ön kapı, Web trafiğini bir arka uç havuzundaki belirli kaynaklara yönlendirir. Ön uç etki alanını tanımladınız, bir arka uç havuzuna kaynak ekleyecek ve bir yönlendirme kuralı oluşturacaksınız. Bu makalede, iki Web uygulaması kaynağına sahip bir arka uç havuzunun basit bir yapılandırması ve varsayılan yol eşleştirme "/*" kullanılarak tek bir yönlendirme kuralı kullanılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Azure CLı yerel olarak veya Azure Cloud Shell yüklendi
- Ön kapı uzantısının Azure CLı 'nize eklendiğinden emin olun

```azurecli-interactive 
az extension add --name front-door
```

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu hızlı başlangıç, Azure CLı sürüm 2.0.28 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure 'da, ilgili kaynakları bir kaynak grubuna ayırabilirsiniz. Var olan bir kaynak grubunu kullanabilir ya da yeni bir tane oluşturabilirsiniz.

Bu hızlı başlangıçta iki kaynak grubu gerekir. Biri *Orta ABD* ve ikincisi *Orta Güney ABD*.

[Az Group Create](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create&preserve-view=true)ile bir kaynak grubu oluşturun:

```azurecli-interactive
az group create \
    --name myRGFDCentral \
    --location centralus

az group create \
    --name myRGFDSouthCentral \
    --location southcentralus
```

## <a name="create-two-instances-of-a-web-app"></a>Bir Web uygulamasının iki örneğini oluşturma

Bu hızlı başlangıç için farklı Azure bölgelerinde çalışan bir Web uygulamasının iki örneği gereklidir. Her iki Web uygulaması örneği de etkin/etkin modda çalışır, bu nedenle bir hizmet trafiğinden biri olabilir.

Henüz bir Web uygulamanız yoksa, iki örnek Web uygulaması kurmak için aşağıdaki betiği kullanın.

### <a name="create-app-service-plans"></a>App Service planları oluşturma

Web uygulamalarını oluşturabilmeniz için, biri *Orta ABD* ve ikincisi *Orta Güney ABD*olmak üzere iki App Service planına ihtiyacınız olacaktır.

[Az appservice plan Create](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az_appservice_plan_create&preserve-view=true)ile App Service planları oluşturun:

```azurecli-interactive
az appservice plan create \
--name myAppServicePlanCentralUS \
--resource-group myRGFDCentral

az appservice plan create \
--name myAppServicePlanSouthCentralUS \
-resource-groupg myRGFDSouthCentral
```

### <a name="create-web-apps"></a>Web uygulamaları oluşturma

Aşağıdaki komutlar çalıştırıldığında, önceki adımda bulunan App Service planlarının her birinde bir Web uygulaması oluşturulur. Web uygulaması adlarının genel olarak benzersiz olması gerekir.

[Az WebApp Create](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az_webapp_create&preserve-view=true)ile Web uygulaması oluşturun:

```azurecli-interactive
az webapp create \
--name WebAppContoso1 \
--resource-group myRGFDCentral \
--plan myAppServicePlanCentralUS 

az webapp create \
--name WebAppContoso2 \
--resource-group myRGFDSouthCentral \
--plan myAppServicePlanSouthCentralUS
```

Bir sonraki adımda ön kapıyı dağıtırken arka uç adreslerini tanımlayabilmeniz için her bir Web uygulamasının varsayılan ana bilgisayar adını unutmayın.

## <a name="create-the-front-door"></a>Ön kapıyı oluşturma

Aşağıdaki adımları izleyerek varsayılan Yük Dengeleme ayarları, sistem durumu araştırması ve yönlendirme kuralları ile temel bir ön kapı oluşturun:

[Az Network ön kapı Create](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext_front_door_az_network_front_door_create&preserve-view=true)Ile ön kapı oluşturun:

```azurecli-interactive
az network front-door create \
--resource-group myRGFDCentral \
--name contoso-frontend \
--accepted-protocols http https \
--backend-address webappcontoso1.azurewebsites.net webappcontoso2.azurewebsites.net 
```

**--Resource-Group:** Ön kapıyı dağıtmak istediğiniz kaynak grubunu belirtin.

**--ad:** Azure ön kapılarınız için genel olarak benzersiz bir ad belirtin. 

**--kabul edilen-protokoller:** Kabul edilen değerler **http** ve **https 'dir**. Her ikisini de bir boşlukla ayırarak kullanmak istiyorsanız.

**--arka uç-Adres:** Her iki Web Apps ana bilgisayar adını boşlukla ayırarak tanımlayın.

Dağıtım başarıyla tamamlandıktan sonra, ön *uçlar* bölümünde ana bilgisayar adını unutmayın.

## <a name="test-the-front-door"></a>Ön kapıyı test etme

Bir Web tarayıcısı açın ve komutlardan al ana bilgisayar adını girin. Ön kapı, isteğinizi arka uç kaynaklarından birine yönlendirir.

:::image type="content" source="./media/quickstart-create-front-door-cli/front-door-testing-page.png" alt-text="Ön kapı sınama sayfası":::

## <a name="clean-up-resources"></a>Kaynakları temizleme

Ön kapıda oluşturduğunuz kaynaklara artık ihtiyacınız kalmadığında her iki kaynak grubunu da silin. Kaynak grubunu sildiğinizde, ön kapıyı ve ilgili tüm kaynakları da silersiniz. 

Kaynak grubunu silmek için [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az_group_delete&preserve-view=true)kullanın:

```azurecli-interactive
az group delete \
--name myRGFDCentral 

az group delete \
--name myRGFDSouthCentral
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta şunu oluşturdunuz:
* Front Door
* İki Web uygulaması

Ön kapıya özel bir etki alanı eklemeyi öğrenmek için ön kapı öğreticilerine geçin.

> [!div class="nextstepaction"]
> [Özel etki alanı ekleme](front-door-custom-domain.md)
