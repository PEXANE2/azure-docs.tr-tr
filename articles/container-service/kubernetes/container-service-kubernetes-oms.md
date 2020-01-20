---
title: Kullanım DıŞı Azure Kubernetes kümesini izleme-Operations Management
description: Azure Container Service Log Analytics kullanarak Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 3cb500d2f00d6657420d7f294a7318b339e1f81e
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76271063"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-log-analytics"></a>Kullanım DıŞı Log Analytics sahip bir Azure Container Service kümesini izleme

> [!TIP]
> Azure Kubernetes hizmetini kullanan Bu makalenin güncelleştirilmiş sürümü için bkz. [kapsayıcılar Için Azure izleyici](../../azure-monitor/insights/container-insights-overview.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, Azure CLI `az` ve `kubectl` araçlarının yüklü olduğunu varsaymaktadır.

Şunu çalıştırarak `az` aracı yüklüyse test edebilirsiniz:

```console
$ az --version
```

`az` aracı yüklü değilse, [burada](https://github.com/azure/azure-cli#installation)yönergeler vardır.
Alternatif olarak, Azure CLI `az` ve `kubectl` araçlarının zaten yüklü olduğu [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)kullanabilirsiniz.

Şunu çalıştırarak `kubectl` aracı yüklüyse test edebilirsiniz:

```console
$ kubectl version
```

Yüklü `kubectl` yoksa şu şekilde çalıştırabilirsiniz:
```console
$ az acs kubernetes install-cli
```

Kubectl aracında Kubernetes anahtarlarınızın yüklü olup olmadığını test etmek için şunu çalıştırabilirsiniz:
```console
$ kubectl get nodes
```

Yukarıdaki komut hata alıyorsa, Kubernetes kümesi anahtarlarını kubectl aracınızı yüklemeniz gerekir. Bunu aşağıdaki komutla yapabilirsiniz:
```console
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Log Analytics ile kapsayıcıları izleme

Log Analytics, Microsoft 'un Şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan bulut tabanlı BT yönetimi çözümüdür. Kapsayıcı çözümü, kapsayıcı envanterini, performansı ve günlükleri tek bir konumda görüntülemenize yardımcı olan Log Analytics bir çözümdür. Günlükleri merkezi konuma görüntüleyerek denetleyebilir, sorun giderebilir ve bir konakta çok fazla kapsayıcı tüketen çok fazla kapsayıcı bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözümü hakkında daha fazla bilgi için lütfen [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md)bakın.

## <a name="installing-log-analytics-on-kubernetes"></a>Kubernetes 'te Log Analytics yükleme

### <a name="obtain-your-workspace-id-and-key"></a>Çalışma alanı KIMLIĞINIZI ve anahtarınızı edinin
Log Analytics aracısının hizmet ile iletişim kurmasını sağlamak için, bir çalışma alanı KIMLIĞI ve bir çalışma alanı anahtarıyla yapılandırılması gerekir. Çalışma alanı KIMLIĞINI ve anahtarını almak için <https://mms.microsoft.com>bir hesap oluşturmanız gerekir.
Lütfen hesap oluşturmak için adımları izleyin. Hesabı oluşturmayı tamamladıktan sonra, **Log Analytics** dikey penceresine tıklayarak ve ardından çalışma alanınızın adını girerek kimliğinizi ve anahtarınızı elde edebilirsiniz. Ardından, **Gelişmiş ayarlar**, **bağlı kaynaklar**ve **Linux sunucuları**' nın altında, ihtiyacınız olan bilgileri aşağıda gösterildiği gibi bulacaksınız.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>Bir DaemonSet kullanarak Log Analytics aracısını yükler
DaemonSets, Kubernetes tarafından kümedeki her konakta bir kapsayıcının tek bir örneğini çalıştırmak için kullanılır.
İzleme aracılarını çalıştırmak için idealdir.

İşte [DaemonSet YAML dosyası](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). `oms-daemonset.yaml` adlı bir dosyaya kaydedin ve `WSID` ve `KEY` için yer tutucu değerlerini, çalışma alanınızın KIMLIĞI ve anahtarınızla değiştirin.

Çalışma alanı KIMLIĞINIZI ve anahtarınızı DaemonSet yapılandırmasına ekledikten sonra, `kubectl` komut satırı aracıyla Log Analytics aracısını kümenize yükleyebilirsiniz:

```console
$ kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Kubernetes gizli anahtarını kullanarak Log Analytics aracısını yükleme
Log Analytics çalışma alanı KIMLIĞINIZI ve anahtarınızı korumak için, DaemonSet YAML dosyasının bir parçası olarak Kubernetes gizli anahtarını kullanabilirsiniz.

- Betiği, gizli şablon dosyasını ve DaemonSet YAML dosyasını ( [depodan](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) kopyalayın ve aynı dizinde olduklarından emin olun.
  - gizli betik oluşturma-secret-gen.sh
  - Gizli şablon - gizli template.yaml
    - DaemonSet YAML dosyası-omsagent-DS-gizlilikler. YAML
- Betiği çalıştırın. Betik Log Analytics çalışma alanı KIMLIĞI ve birincil anahtar ister. Bunu, çalıştırmak için betik gizli bir YAML dosyası oluşturacak şekilde ekleyin.
  ```
  #> sudo bash ./secret-gen.sh
  ```

  - Aşağıdakileri çalıştırarak gizli dizileri oluşturun: ```kubectl create -f omsagentsecret.yaml```

  - Denetlemek için şu komutu çalıştırın:

  ```
  root@ubuntu16-13db:~# kubectl get secrets
  NAME                  TYPE                                  DATA      AGE
  default-token-gvl91   kubernetes.io/service-account-token   3         50d
  omsagent-secret       Opaque                                2         1d
  root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
  Name:           omsagent-secret
  Namespace:      default
  Labels:         <none>
  Annotations:    <none>

  Type:   Opaque

  Data
  ====
  WSID:   36 bytes
  KEY:    88 bytes
  ```

  - Arka plan programı kümesi çalıştırarak, omsagent oluşturma ```kubectl create -f omsagent-ds-secrets.yaml```

### <a name="conclusion"></a>Sonuç
İşte bu kadar! Birkaç dakika sonra, Log Analytics panonuzda veri akışını görebilmelisiniz.
