---
title: (AmortismanA Uğradı) Azure Kubernetes kümesini izleyin - Sysdig
description: Sysdig kullanarak Azure Kapsayıcı Hizmeti'nde Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 68136d5b9ec16c822cb62e4fee85b8ace9b1899a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371109"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>(AmortismanA Uğradı) Sysdig'i kullanarak Azure Kapsayıcı Hizmeti Kubernetes kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Kapsayıcı Hizmeti'ni kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca azure cli ve kubectl araçları yüklü olduğunu varsayar.

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

## <a name="sysdig"></a>Sysdig
Sysdig, Azure'da çalışan Kubernetes kümenizdeki kapsayıcıları izleyebilen bir hizmet şirketi olarak harici bir izlemedir. Sysdig'i kullanmak aktif bir Sysdig hesabı gerektirir.
Kendi [sitesinde](https://app.sysdigcloud.com)bir hesap için kaydolabilirsiniz.

Sysdig bulut web sitesinde oturum açtıktan sonra kullanıcı adınıza tıklayın, sayfada “Erişim Anahtarınızı” göreceksiniz. 

![Sysdig API anahtarı](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Sysdig acentelerinin Kubernetes'e yüklenmesi
Kaplarınızı izlemek için, Sysdig her makinede bir Kubernetes `DaemonSet`kullanarak bir işlem yürüter.
DaemonSets makine başına bir kapsayıcı tek bir örnek çalıştırmak Kubernetes API nesneleridir.
Sysdig'in izleme aracı gibi araçları yüklemek için mükemmeller.

Sysdig daemonset'i yüklemek için önce [şablonu](https://github.com/draios/sysdig-cloud-scripts/tree/master/agent_deploy/kubernetes) sysdig'ten indirmeniz gerekir. Bu dosyayı `sysdig-daemonset.yaml`' olarak kaydet

Linux ve OS X'te şunları çalıştırabilirsiniz:

```console
curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

PowerShell'de:

```powershell
Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Daha sonra, Sysdig hesabınızdan aldığınız Access Key'inizi eklemek için bu dosyayı edin.

Son olarak, DaemonSet oluşturun:

```console
kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>İzlemenizi görüntüleyin
Kurulduktan ve çalıştırıldıktan sonra, ajanlar sysdig geri veri pompagerekir.  [Sysdig panosuna](https://app.sysdigcloud.com) geri dön ve kapsayıcılarınız hakkındaki bilgileri görmelisiniz.

Ayrıca [yeni pano sihirbazı](https://app.sysdigcloud.com/#/dashboards/new)aracılığıyla Kubernetes'e özel panolar yükleyebilirsiniz.
