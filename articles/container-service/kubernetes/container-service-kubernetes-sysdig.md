---
title: Kullanım DıŞı Azure Kubernetes kümesini izleme-Sysdig
description: Sysdig kullanarak Azure Container Service Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 68136d5b9ec16c822cb62e4fee85b8ace9b1899a
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371109"
---
# <a name="deprecated-monitor-an-azure-container-service-kubernetes-cluster-using-sysdig"></a>Kullanım DıŞı Sysdig kullanarak bir Azure Container Service Kubernetes kümesini izleme

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Önkoşullar
Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, Azure CLI ve kubectl araçlarının yüklü olduğunu varsaymaktadır.

Şunu çalıştırarak `az` aracı yüklüyse test edebilirsiniz:

```azurecli
az --version
```

`az` aracı yüklü değilse, [burada](https://github.com/azure/azure-cli#installation)yönergeler vardır.

Şunu çalıştırarak `kubectl` aracı yüklüyse test edebilirsiniz:

```console
kubectl version
```

Yüklü `kubectl` yoksa şu şekilde çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

## <a name="sysdig"></a>Sysdig
Sysdig, Azure 'da çalışan Kubernetes kümenizdeki kapsayıcıları izleyebilen bir hizmet şirketi olarak bir dış izlemedir. Sysdig kullanmak için etkin bir Sysdig hesabı gerekir.
[Sitelerinde](https://app.sysdigcloud.com)bir hesap için kaydolabilirsiniz.

Sysdig bulut web sitesinde oturum açtıktan sonra kullanıcı adınıza tıklayın, sayfada “Erişim Anahtarınızı” göreceksiniz. 

![Sysdig API anahtarı](./media/container-service-kubernetes-sysdig/sysdig2.png)

## <a name="installing-the-sysdig-agents-to-kubernetes"></a>Kubernetes 'e Sysdig aracılarını yükleme
Kapsayıcılarınızı izlemek için Sysdig, her makinede bir Kubernetes `DaemonSet`kullanarak bir işlem çalıştırır.
DaemonSets, makine başına kapsayıcının tek bir örneğini çalıştıran Kubernetes API nesneleridir.
Sysdig 'nin izleme Aracısı gibi araçları yüklemek için mükemmeldir.

Sysdig daemonset yüklemek için, önce [şablonu](https://github.com/draios/sysdig-cloud-scripts/tree/master/agent_deploy/kubernetes) sysdig 'den indirmeniz gerekir. Bu dosyayı `sysdig-daemonset.yaml`olarak kaydedin.

Linux ve OS X üzerinde şunları çalıştırabilirsiniz:

```console
curl -O https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml
```

PowerShell'de:

```powershell
Invoke-WebRequest -Uri https://raw.githubusercontent.com/draios/sysdig-cloud-scripts/master/agent_deploy/kubernetes/sysdig-daemonset.yaml | Select-Object -ExpandProperty Content > sysdig-daemonset.yaml
```

Daha sonra, Sysdig hesabınızdan edindiğiniz erişim anahtarınızı eklemek için bu dosyayı düzenleyin.

Son olarak, DaemonSet oluşturun:

```console
kubectl create -f sysdig-daemonset.yaml
```

## <a name="view-your-monitoring"></a>İzlemeyi görüntüleyin
Yüklenip çalıştırıldığında, aracılar verileri Sysdig 'ye geri dağıtmalıdır.  [Sysdig panosuna](https://app.sysdigcloud.com) geri dönün ve kapsayıcılarınız hakkında bilgi görmeniz gerekir.

Ayrıca, [Yeni Pano Sihirbazı](https://app.sysdigcloud.com/#/dashboards/new)aracılığıyla Kubernetes 'e özgü panoları da yükleyebilirsiniz.
