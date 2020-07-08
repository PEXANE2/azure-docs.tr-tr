---
title: Kullanım DıŞı Azure Kubernetes kümesini Web Kullanıcı arabirimi ile yönetme
description: Azure Container Service 'de Kubernetes Web Kullanıcı arabirimini kullanma
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 01abcc961d1c2ad9d3e2cf35f82e62929bc2fb89
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79371146"
---
# <a name="deprecated-using-the-kubernetes-web-ui-with-azure-container-service"></a>Kullanım DıŞı Azure Container Service ile Kubernetes Web Kullanıcı arabirimini kullanma

> [!TIP]
> Azure Kubernetes hizmetini kullanan Bu makalenin güncelleştirilmiş sürümü için bkz. [Azure Kubernetes hizmeti 'nde (AKS) Kubernetes web panosuna erişme](../../aks/kubernetes-dashboard.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.


Ayrıca, Azure CLı ve `kubectl` araçları yüklü olduğunu varsayar.

' İ `az` çalıştırarak aracının yüklenip yüklenmediğini test edebilirsiniz:

```azurecli
az --version
```

`az`Araç yüklü değilse, [burada](https://github.com/azure/azure-cli#installation)yönergeler vardır.

' İ `kubectl` çalıştırarak aracının yüklenip yüklenmediğini test edebilirsiniz:

```console
kubectl version
```

`kubectl`Yüklü değilse şunu çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="overview"></a>Genel Bakış

### <a name="connect-to-the-web-ui"></a>Web Kullanıcı arabirimine bağlanma
Şunu çalıştırarak Kubernetes Web Kullanıcı arabirimini başlatabilirsiniz:

```azurecli
az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Bu, yerel makinenizi Kubernetes Web Kullanıcı arabirimine bağlayan güvenli bir ara sunucu ile konuşmak için yapılandırılmış bir Web tarayıcısını açmalı.

### <a name="create-and-expose-a-service"></a>Hizmet oluşturma ve kullanıma sunma
1. Kubernetes Web Kullanıcı arabiriminde sağ üst penceredeki **Oluştur** düğmesine tıklayın.

    ![Kubernetes UI oluştur](./media/container-service-kubernetes-ui/create.png)

    Uygulamanızı oluşturmaya başlayabileceğiniz bir iletişim kutusu açılır.

2. Adı verin `hello-nginx` . [ `nginx` Docker 'dan kapsayıcıyı](https://hub.docker.com/_/nginx/) kullanın ve bu Web hizmetinin üç çoğaltmasını dağıtın.

    ![Kubernetes Pod oluştur Iletişim kutusu](./media/container-service-kubernetes-ui/nginx.png)

3. **Hizmet**altında **Harici** ' ı seçin ve 80 numaralı bağlantı noktasını girin.

    Bu ayar yükü-trafiği üç çoğaltmaya dengeler.

    ![Kubernetes hizmeti oluşturma Iletişim kutusu](./media/container-service-kubernetes-ui/service.png)

4. Bu kapsayıcıları ve hizmetleri dağıtmak için **Dağıt** ' a tıklayın.

    ![Kubernetes Dağıtımı](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Kapsayıcılarınızı görüntüleyin
**Dağıt**' a tıkladıktan sonra, Kullanıcı arabirimi, dağıttığı için hizmetinizin bir görünümünü gösterir:

![Kubernetes durumu](./media/container-service-kubernetes-ui/status.png)

Her bir Kubernetes nesnesinin durumunu, Kullanıcı arabiriminin sol tarafındaki daire içinde, **pods**altında görebilirsiniz. Kısmi bir tam daire ise, nesne hala dağıtılıyor demektir. Bir nesne tam olarak dağıtıldığında yeşil onay işareti görüntülenir:

![Kubernetes dağıtıldı](./media/container-service-kubernetes-ui/deployed.png)

Her şey çalışırken, çalışan Web hizmeti hakkındaki ayrıntıları görmek için yığınlarınızın birine tıklayın.

![Kubernetes pods](./media/container-service-kubernetes-ui/pods.png)

**Pods** görünümünde, Pod 'daki kapsayıcılar ve bu kapsayıcılar tarafından kullanılan CPU ve bellek kaynakları hakkındaki bilgileri görebilirsiniz:

![Kubernetes kaynakları](./media/container-service-kubernetes-ui/resources.png)

Kaynakları görmüyorsanız, izleme verilerinin yayılması için birkaç dakika beklemeniz gerekebilir.

Kapsayıcının günlüklerini görmek için **günlükleri görüntüle**' ye tıklayın.

![Kubernetes günlükleri](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Hizmetinizi görüntüleme
Kubernetes Kullanıcı arabirimi, Kapsayıcılarınızı çalıştırmanın yanı sıra bir dış oluşturmuş ve `Service` Bu sayede trafiği kümenizdeki kapsayıcılara getirecek bir yük dengeleyici sağlar.

Sol gezinti bölmesinde, tüm hizmetleri görüntülemek için **Hizmetler** ' e tıklayın (yalnızca bir tane olmalıdır).

![Kubernetes Hizmetleri](./media/container-service-kubernetes-ui/service-deployed.png)

Bu görünümde, hizmetinize ayrılmış bir dış uç nokta (IP adresi) görmeniz gerekir.
Bu IP adresine tıklarsanız, NGINX kapsayıcınızı yük dengeleyicinin arkasında çalışan bir şekilde görmeniz gerekir.

![NGINX görünümü](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Hizmetinizi yeniden boyutlandırma
Nesneleri kullanıcı arabiriminde görüntülemenin yanı sıra, Kubernetes API nesnelerini düzenleyebilir ve güncelleştirebilirsiniz.

İlk olarak, hizmetiniz için dağıtımı görmek üzere sol gezinti bölmesindeki **dağıtımlar** ' a tıklayın.

Bu görünümde olduktan sonra, çoğaltma kümesine tıklayın ve sonra üst gezinti çubuğunda **Düzenle** ' ye tıklayın:

![Kubernetes düzenleme](./media/container-service-kubernetes-ui/edit.png)

`spec.replicas`Alanı olacak şekilde düzenleyin `2` ve **Güncelleştir**' e tıklayın.

Bu, yığınlarınızın birini silerek yineleme sayısının ikiye düşürümesine neden olur.

 

