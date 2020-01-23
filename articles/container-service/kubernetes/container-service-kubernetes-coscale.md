---
title: Kullanım DıŞı CoScale ile bir Azure Kubernetes kümesini izleme
description: CoScale kullanarak Azure Container Service bir Kubernetes kümesini izleme
author: fryckbos
ms.service: container-service
ms.topic: conceptual
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f1d0ca1ffc2e7a3d645ac5acbaafdf45f85550be
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76271107"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Kullanım DıŞı CoScale ile Azure Container Service Kubernetes kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Bu makalede, Azure Container Service ile Kubernetes kümenizdeki tüm düğümleri ve kapsayıcıları izlemek için [Coscale](https://web.archive.org/web/20180317071550/https://www.coscale.com/) aracısının nasıl dağıtılacağını göstereceğiz. Bu yapılandırma için CoScale ile bir hesabınız olması gerekir. 


## <a name="about-coscale"></a>CoScale hakkında 

CoScale, çeşitli düzenleme platformlarındaki tüm kapsayıcılardan ölçümleri ve olayları toplayan bir izleme platformudur. CoScale, Kubernetes ortamları için tam yığın izleme sağlar. Yığındaki tüm katmanlar için görsel öğeler ve çözümlemeler sağlar: işletim sistemi, Kubernetes, Docker ve kapsayıcılarınızda çalışan uygulamalar. CoScale, çeşitli yerleşik izleme panoları sunar ve operatörlerin ve geliştiricilerin altyapı ve uygulama sorunlarını hızla bulmasına imkan tanımak için yerleşik anomali algılama içerir.

![CoScale Kullanıcı arabirimi](./media/container-service-kubernetes-coscale/coscale.png)

Bu makalede gösterildiği gibi, bir Kubernetes kümesine aracıları yükleyerek bir SaaS çözümü olarak CoScale çalıştırabilirsiniz. Verilerinizi yerinde tutmak istiyorsanız, şirket içi yükleme için birlikte ölçeklendirme de mevcuttur.


## <a name="prerequisites"></a>Ön koşullar

İlk olarak [bir CoScale hesabı oluşturmanız](https://web.archive.org/web/20170507123133/https://www.coscale.com/free-trial)gerekir.

Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, Azure CLı `az` ve `kubectl` araçlarının yüklü olduğunu varsaymaktadır.

Şunu çalıştırarak `az` aracı yüklüyse test edebilirsiniz:

```azurecli
az --version
```

`az` aracı yüklü değilse, [burada](/cli/azure/install-azure-cli)yönergeler vardır.

Şunu çalıştırarak `kubectl` aracı yüklüyse test edebilirsiniz:

```bash
kubectl version
```

Yüklü `kubectl` yoksa şu şekilde çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>DaemonSet ile CoScale aracısını yükleme
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) , Kubernetes tarafından kümedeki her konakta bir kapsayıcının tek bir örneğini çalıştırmak için kullanılır.
CoScale Aracısı gibi izleme aracılarını çalıştırmak için idealdir.

CoScale 'de oturum açtıktan sonra, DaemonSet kullanarak kümenize CoScale aracıları yüklemek için [aracı sayfasına](https://app.coscale.com/) gidin. CoScale Kullanıcı arabirimi, bir aracı oluşturmak ve tamamen Kubernetes kümenizi izlemeye başlamak için Kılavuzlu yapılandırma adımları sağlar.

![CoScale Aracısı yapılandırması](./media/container-service-kubernetes-coscale/installation.png)

Aracıyı kümede başlatmak için sağlanan komutu çalıştırın:

![CoScale aracısını başlatma](./media/container-service-kubernetes-coscale/agent_script.png)

İşte bu kadar! Aracılar çalışır duruma getirildikten sonra, konsolunda birkaç dakika içinde verileri görmeniz gerekir. Kümenizin özetini görmek, ek yapılandırma adımları gerçekleştirmek ve **Kubernetes kümesine genel bakış**gibi panoları görmek için [Aracı sayfasını](https://app.coscale.com/) ziyaret edin.

![Kubernetes kümesine genel bakış](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

CoScale Aracısı kümedeki yeni makinelere otomatik olarak dağıtılır. Yeni bir sürüm yayınlandığında aracı otomatik olarak güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar

CoScale izleme çözümleri hakkında daha fazla bilgi için [coscale belgelerine](https://web.archive.org/web/20180415164304/http://docs.coscale.com:80/) ve [bloguna](https://web.archive.org/web/20170501021344/http://www.coscale.com:80/blog) bakın. 

