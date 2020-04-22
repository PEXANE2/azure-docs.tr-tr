---
title: (AmortismanA Uğradı) CoScale ile Azure Kubernetes kümesini izleme
description: CoScale'i kullanarak Azure Kapsayıcı Hizmeti'nde bir Kubernetes kümesini izleme
author: fryckbos
ms.service: container-service
ms.topic: conceptual
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f195a5c05c6c95dac898b2d471747952a3446d52
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681726"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>(AmortismanA Uğradı) CoScale ile Azure Kapsayıcı Hizmeti Kubernetes kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Bu makalede, Azure Kapsayıcı Hizmeti'ndeki Kubernetes kümenizdeki tüm düğümleri ve kapsayıcıları izlemek için [CoScale](https://web.archive.org/web/20180317071550/https://www.coscale.com/) aracısını nasıl dağıtabileceğinizi gösteriyoruz. Bu yapılandırma için CoScale'de bir hesaba ihtiyacınız var. 


## <a name="about-coscale"></a>CoScale Hakkında 

CoScale, çeşitli orkestrasyon platformlarında tüm kaplardan ölçümler ve olaylar toplayan bir izleme platformudur. CoScale, Kubernetes ortamları için tam yığın izleme hizmeti sunar. Yığındaki tüm katmanlar için görselleştirmeler ve analizler sağlar: işletim sistemi, Kubernetes, Docker ve kaplarınızın içinde çalışan uygulamalar. CoScale birkaç yerleşik izleme panosu sunar ve operatörlerin ve geliştiricilerin altyapı ve uygulama sorunlarını hızlı bir şekilde bulmalarına olanak sağlamak için yerleşik anormallik algılamasına sahiptir.

![CoScale UI](./media/container-service-kubernetes-coscale/coscale.png)

Bu makalede gösterildiği gibi, Bir SaaS çözümü olarak CoScale çalıştırmak için bir Kubernetes kümesine aracılar yükleyebilirsiniz. Verilerinizi yerinde tutmak istiyorsanız, CoScale şirket içi yükleme için de kullanılabilir.


## <a name="prerequisites"></a>Ön koşullar

Öncelikle bir [CoScale hesabı oluşturmanız](https://web.archive.org/web/20170507123133/https://www.coscale.com/free-trial)gerekir.

Bu izlenecek yol, [Azure Kapsayıcı Hizmeti'ni kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca `az` Azure CLI ve `kubectl` araçları yüklü olduğunu varsayar.

Aracı çalıştırarak yüklü `az` yorurarak test edebilirsiniz:

```azurecli
az --version
```

`az` Aracı yüklü değilseniz, [burada](/cli/azure/install-azure-cli)talimatlar vardır.

Aracı çalıştırarak yüklü `kubectl` yorurarak test edebilirsiniz:

```bash
kubectl version
```

`kubectl` Yüklemediyseniz çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-the-coscale-agent-with-a-daemonset"></a>Bir DaemonSet ile CoScale aracıyükleme
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) kümedeki her ana bilgisayarda bir kapsayıcının tek bir örneğini çalıştırmak için Kubernetes tarafından kullanılır.
CoScale ajanı gibi izleme ajanlarını çalıştırmak için mükemmeller.

CoScale'de oturum açtıktan sonra, DaemonSet kullanarak kümenize CoScale aracıları yüklemek için [aracılar sayfasına](https://developer.newrelic.com/) gidin. CoScale Kullanıcı Aracı, bir aracı oluşturmak ve tam Kubernetes kümenizi izlemeye başlamak için kılavuzlu yapılandırma adımları sağlar.

![CoScale aracı yapılandırması](./media/container-service-kubernetes-coscale/installation.png)

Kümedeki aracıyı başlatmak için verilen komutu çalıştırın:

![CoScale aracısını başlatın](./media/container-service-kubernetes-coscale/agent_script.png)

İşte bu kadar! Aracılar çalışmaya başladıktan sonra, birkaç dakika içinde konsoldaki verileri görmeniz gerekir. [Kümenizin](https://developer.newrelic.com/) özetini görmek, ek yapılandırma adımları gerçekleştirmek ve **Kubernetes kümesine genel bakış**gibi panolara bakın.

![Kubernetes küme genel bakış](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

CoScale aracısı kümedeki yeni makinelerde otomatik olarak dağıtılır. Aracı, yeni bir sürüm yayımlandığında otomatik olarak güncellenir.


## <a name="next-steps"></a>Sonraki adımlar

CoScale izleme çözümleri hakkında daha fazla bilgi için [CoScale belgelerine](https://web.archive.org/web/20180415164304/http://docs.coscale.com:80/) ve [bloguna](https://web.archive.org/web/20170501021344/http://www.coscale.com:80/blog) bakın. 

