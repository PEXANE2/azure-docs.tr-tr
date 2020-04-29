---
title: Kullanım DıŞı Azure Kubernetes kümesini izleme-Operations Management
description: Azure Container Service Log Analytics kullanarak Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 02d04076ccc41d243a493838667f5e8cc6bfa5ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79371163"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-log-analytics"></a>Kullanım DıŞı Log Analytics sahip bir Azure Container Service kümesini izleme

> [!TIP]
> Azure Kubernetes hizmetini kullanan Bu makalenin güncelleştirilmiş sürümü için bkz. [kapsayıcılar Için Azure izleyici](../../azure-monitor/insights/container-insights-overview.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Container Service kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, `az` Azure CLI ve `kubectl` araçları yüklü olduğunu varsayar.

' İ çalıştırarak `az` aracının yüklenip yüklenmediğini test edebilirsiniz:

```azurecli
az --version
```

`az` Araç yüklü değilse, [burada](https://github.com/azure/azure-cli#installation)yönergeler vardır.
Alternatif olarak, `az` Azure CLI ve `kubectl` araçları 'nın zaten yüklü olduğu [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)kullanabilirsiniz.

' İ çalıştırarak `kubectl` aracının yüklenip yüklenmediğini test edebilirsiniz:

```console
kubectl version
```

`kubectl` Yüklü değilse şunu çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

Kubectl aracında Kubernetes anahtarlarınızın yüklü olup olmadığını test etmek için şunu çalıştırabilirsiniz:

```console
kubectl get nodes
```

Yukarıdaki komut hata alıyorsa, Kubernetes kümesi anahtarlarını kubectl aracınızı yüklemeniz gerekir. Bunu aşağıdaki komutla yapabilirsiniz:

```azurecli
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Log Analytics ile kapsayıcıları izleme

Log Analytics, Microsoft 'un Şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan bulut tabanlı BT yönetimi çözümüdür.Kapsayıcı çözümü, kapsayıcı envanterini, performansı ve günlükleri tek bir konumda görüntülemenize yardımcı olan Log Analytics bir çözümdür. Günlükleri merkezi konuma görüntüleyerek denetleyebilir, sorun giderebilir ve bir konakta çok fazla kapsayıcı tüketen çok fazla kapsayıcı bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Kapsayıcı çözümü hakkında daha fazla bilgi için lütfen [kapsayıcı çözümü Log Analytics](../../azure-monitor/insights/containers.md)bakın.

## <a name="installing-log-analytics-on-kubernetes"></a>Kubernetes 'te Log Analytics yükleme

### <a name="obtain-your-workspace-id-and-key"></a>Çalışma alanı KIMLIĞINIZI ve anahtarınızı edinin
Log Analytics aracısının hizmet ile iletişim kurmasını sağlamak için, bir çalışma alanı KIMLIĞI ve bir çalışma alanı anahtarıyla yapılandırılması gerekir. Çalışma alanı KIMLIĞINI ve anahtarını almak için ' de <https://mms.microsoft.com>bir hesap oluşturmanız gerekir.
Lütfen hesap oluşturmak için adımları izleyin. Hesabı oluşturmayı tamamladıktan sonra, **Log Analytics** dikey penceresine tıklayarak ve ardından çalışma alanınızın adını girerek kimliğinizi ve anahtarınızı elde edebilirsiniz. Ardından, **Gelişmiş ayarlar**, **bağlı kaynaklar**ve **Linux sunucuları**' nın altında, ihtiyacınız olan bilgileri aşağıda gösterildiği gibi bulacaksınız.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>Bir DaemonSet kullanarak Log Analytics aracısını yükler
DaemonSets, Kubernetes tarafından kümedeki her konakta bir kapsayıcının tek bir örneğini çalıştırmak için kullanılır.
İzleme aracılarını çalıştırmak için idealdir.

İşte [DaemonSet YAML dosyası](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes). Bu dosyayı adlı `oms-daemonset.yaml` bir dosyaya kaydedin ve için `WSID` `KEY` yer tutucu değerlerini, çalışma alanınızın kimliği ve anahtarınızla değiştirin.

Çalışma alanı KIMLIĞINIZI ve anahtarınızı DaemonSet yapılandırmasına ekledikten sonra, Log Analytics aracısını `kubectl` komut satırı aracıyla kümenize yükleyebilirsiniz:

```console
kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Kubernetes gizli anahtarını kullanarak Log Analytics aracısını yükleme
Log Analytics çalışma alanı KIMLIĞINIZI ve anahtarınızı korumak için, DaemonSet YAML dosyasının bir parçası olarak Kubernetes gizli anahtarını kullanabilirsiniz.

- Betiği, gizli şablon dosyasını ve DaemonSet YAML dosyasını ( [depodan](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)) kopyalayın ve aynı dizinde olduklarından emin olun.
  - gizli betik oluşturma-secret-gen.sh
  - gizli şablon-gizli-şablon. YAML
    - DaemonSet YAML dosyası-omsagent-DS-gizlilikler. YAML
- Betiği çalıştırın. Betik Log Analytics çalışma alanı KIMLIĞI ve birincil anahtar ister. Bunu, çalıştırmak için betik gizli bir YAML dosyası oluşturacak şekilde ekleyin.

  ```console
  sudo bash ./secret-gen.sh
  ```

  - Aşağıdakileri çalıştırarak gizli dizileri oluşturun:

     ```console
     kubectl create -f omsagentsecret.yaml
     ```

  - Denetlemek için aşağıdakileri çalıştırın:

  ```console
  kubectl get secrets
  ```

  ```output
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

  - Aşağıdaki komutu çalıştırarak omsagent Daemon 'nizi oluşturun:
  
  ```console
  kubectl create -f omsagent-ds-secrets.yaml
  ```

### <a name="conclusion"></a>Sonuç
İşte bu kadar! Birkaç dakika sonra, Log Analytics panonuzda veri akışını görebilmelisiniz.
