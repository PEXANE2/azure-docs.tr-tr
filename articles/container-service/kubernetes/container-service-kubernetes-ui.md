---
title: (AmortismanA Uğradı) Web Web Web Sürümü ile Azure Kubernetes kümesini yönetme
description: Azure Konteyner Hizmetinde Kubernetes web UI'sini kullanma
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 01abcc961d1c2ad9d3e2cf35f82e62929bc2fb89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371146"
---
# <a name="deprecated-using-the-kubernetes-web-ui-with-azure-container-service"></a>(AmortismanA Uğradı) Azure Konteyner Hizmeti ile Kubernetes web UI'sini kullanma

> [!TIP]
> Azure Kubernetes Hizmetini kullanan bu makalenin güncelleştirilmiş sürümü [için](../../aks/kubernetes-dashboard.md)bkz.

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Kapsayıcı Hizmeti'ni kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.


Ayrıca Azure CLI ve `kubectl` araçları yüklü olduğunu varsayar.

Aracı çalıştırarak yüklü `az` yorurarak test edebilirsiniz:

```azurecli
az --version
```

`az` Aracı yüklü değilseniz, [burada](https://github.com/azure/azure-cli#installation)talimatlar vardır.

Aracı çalıştırarak yüklü `kubectl` yorurarak test edebilirsiniz:

```console
kubectl version
```

`kubectl` Yüklemediyseniz çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="overview"></a>Genel Bakış

### <a name="connect-to-the-web-ui"></a>Web'e bağlantı ara
Kubernetes web UI'sini çalıştırarak başlatabilirsiniz:

```azurecli
az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Bu Kubernetes web UI yerel makine bağlayan güvenli bir proxy konuşmak için yapılandırılmış bir web tarayıcısı açmalıdır.

### <a name="create-and-expose-a-service"></a>Bir hizmet oluşturma ve açığa çıkarma
1. Kubernetes web UI'sinde sağ üst penceredeki **Oluştur** düğmesini tıklatın.

    ![Kubernetes Oluşturma UI](./media/container-service-kubernetes-ui/create.png)

    Uygulamanızı oluşturmaya başlaabileceğiniz bir iletişim kutusu açılır.

2. Ona adını `hello-nginx`ver. [ `nginx` Docker'ın kapsayıcısını](https://hub.docker.com/_/nginx/) kullanın ve bu web hizmetinin üç kopyasını dağıtın.

    ![Kubernetes Pod Oluşturma İletişim](./media/container-service-kubernetes-ui/nginx.png)

3. **Hizmet**altında, **Dış'ı** seçin ve 80 portu girin.

    Bu ayar, üç yinelemeye olan yükü dengeler.

    ![Kubernetes Hizmet İletişim Oluşturma](./media/container-service-kubernetes-ui/service.png)

4. Bu kapsayıcıları ve hizmetleri dağıtmak için **Dağıt'ı** tıklatın.

    ![Kubernetes Dağıtımı](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Konteynerlerinizi görüntüleme
**Dağıt'ı**tıklattıktan sonra, UI hizmetindağıtılırken görünümünü gösterir:

![Kubernetes Durumu](./media/container-service-kubernetes-ui/status.png)

Pods altında, UI'nin sol tarafındaki dairedeki her Kubernetes nesnesinin **Pods**durumunu görebilirsiniz. Kısmen tam bir daireise, nesne hala dağıtılır. Bir nesne tam olarak dağıtıldığında, yeşil bir onay işareti görüntüler:

![Kubernetes Konuşlandırıldı](./media/container-service-kubernetes-ui/deployed.png)

Her şey çalışmaya başladığında, çalışan web hizmetiyle ilgili ayrıntıları görmek için bölmelerinizden birini tıklatın.

![Kubernetes Pods](./media/container-service-kubernetes-ui/pods.png)

Pods görünümünde, bölmedeki kapsayıcılar ve bu **kapsayıcılar** tarafından kullanılan CPU ve bellek kaynakları hakkındaki bilgileri görebilirsiniz:

![Kubernetes Kaynakları](./media/container-service-kubernetes-ui/resources.png)

Kaynakları görmüyorsanız, izleme verilerinin yayılması için birkaç dakika beklemeniz gerekebilir.

Kapsayıcınızın günlüklerini görmek için **günlükleri görüntüle'yi**tıklatın.

![Kubernetes Günlükleri](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Hizmetinizi görüntüleme
Kubernetes UI, kapsayıcılarınızı çalıştırmanın yanı sıra, kümenizdeki kapsayıcılara trafik getirmek için bir yük dengeleyicisi sağlayan bir harici `Service` araç oluşturmuştur.

Sol gezinti bölmesinde, tüm hizmetleri görüntülemek için **Hizmetler'i** tıklatın (yalnızca bir tane olmalıdır).

![Kubernetes Hizmetleri](./media/container-service-kubernetes-ui/service-deployed.png)

Bu görünümde, hizmetinize ayrılmış harici bir uç nokta (IP adresi) görmeniz gerekir.
Bu IP adresini tıklatırsanız, yük dengeleyicisinin arkasında çalışan Nginx kabınızı görmeniz gerekir.

![nginx görünümü](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Hizmetinizi yeniden boyutlandırma
Kullanıcı Bira'sında nesnelerinizi görüntülemenin yanı sıra, Kubernetes API nesnelerini de güncelleyebilir ve güncelleyebilirsiniz.

İlk olarak, hizmetinizin dağıtımını görmek için sol gezinti bölmesinde **Dağıtımlar'ı** tıklatın.

Bu görünüme sahip olduktan sonra yineleme kümesini tıklatın ve ardından üst gezinti çubuğunda **Edit'i** tıklatın:

![Kubernetes Edit](./media/container-service-kubernetes-ui/edit.png)

Alanı olması `spec.replicas` `2`için edin ve **Güncelleştir'i**tıklatın.

Bu, bölmelerinizden birini silerek çoğaltma sayısının ikiye düşmesine neden olur.

 

