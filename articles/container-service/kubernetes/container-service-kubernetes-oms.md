---
title: (AmortismanA Uğradı) Azure Kubernetes kümesini izleyin - İşlem Yönetimi
description: Log Analytics'i kullanarak Azure Konteyner Hizmeti'nde Kubernetes kümesini izleme
author: bburns
ms.service: container-service
ms.topic: conceptual
ms.date: 12/09/2016
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: 02d04076ccc41d243a493838667f5e8cc6bfa5ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371163"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-log-analytics"></a>(AmortismanA Uğradı) Günlük Analizi ile Azure Kapsayıcı Hizmeti kümesini izleme

> [!TIP]
> Azure Kubernetes Hizmetini kullanan bu makalenin güncelleştirilmiş sürümü [için kapsayıcılar için Azure Monitörü'ne](../../azure-monitor/insights/container-insights-overview.md)bakın.

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu izlenecek yol, [Azure Kapsayıcı Hizmeti'ni kullanarak bir Kubernetes kümesi oluşturduğunuzu](container-service-kubernetes-walkthrough.md)varsayar.

Ayrıca, Azure cli'sinin ve `az` `kubectl` araçlarının yüklü olduğunu varsayar.

Aracı çalıştırarak yüklü `az` yorurarak test edebilirsiniz:

```azurecli
az --version
```

`az` Aracı yüklü değilseniz, [burada](https://github.com/azure/azure-cli#installation)talimatlar vardır.
Alternatif olarak, `az` Azure cli'si ve `kubectl` araçları sizin için zaten yüklenmiş olan Azure Bulut Bulut [Uyp'ı](https://docs.microsoft.com/azure/cloud-shell/overview)kullanabilirsiniz.

Aracı çalıştırarak yüklü `kubectl` yorurarak test edebilirsiniz:

```console
kubectl version
```

`kubectl` Yüklemediyseniz çalıştırabilirsiniz:

```azurecli
az acs kubernetes install-cli
```

Kubectl aracınızda kubernetes tuşları nın yüklü olup olmadığını test etmek için çalıştırabilirsiniz:

```console
kubectl get nodes
```

Yukarıdaki komut hataları varsa, kubectl aracınıza kubernetes küme tuşlarını yüklemeniz gerekir. Bunu aşağıdaki komutla yapabilirsiniz:

```azurecli
RESOURCE_GROUP=my-resource-group
CLUSTER_NAME=my-acs-name
az acs kubernetes get-credentials --resource-group=$RESOURCE_GROUP --name=$CLUSTER_NAME
```

## <a name="monitoring-containers-with-log-analytics"></a>Log Analitiği ile Kapların İzlenmesi

Log Analytics, Microsoft'un şirket içi ve bulut altyapınızı yönetmenize ve korumanıza yardımcı olan bulut tabanlı BT yönetimi çözümüdür.Kapsayıcı Çözümü, Konteyner envanterini, performansını ve günlüklerini tek bir konumda görüntülemenize yardımcı olan Log Analytics'teki bir çözümdür. Merkezi konumdaki günlükleri görüntüleyerek kapsayıcıları denetleyebilir, sorun giderebilir ve bir ana bilgisayarda gürültülü aşırı kapsayıcı tüketen bir kutucağ bulabilirsiniz.

![](media/container-service-monitoring-oms/image1.png)

Konteyner Çözümü hakkında daha fazla bilgi için lütfen [Konteyner Çözüm Günlüğü Analizi'ne](../../azure-monitor/insights/containers.md)bakın.

## <a name="installing-log-analytics-on-kubernetes"></a>Kubernetes'te Günlük Analizi Yükleme

### <a name="obtain-your-workspace-id-and-key"></a>Çalışma alanı kimliğinizi ve anahtarınızı edinin
Log Analytics aracısının hizmetle konuşması için bir çalışma alanı kimliği ve çalışma alanı anahtarıyla yapılandırılması gerekir. Çalışma alanı kimliğini ve anahtarı nı almak için <https://mms.microsoft.com>'de bir hesap oluşturmanız gerekir.
Lütfen bir hesap oluşturmak için adımları izleyin. Hesabı oluşturmayı bitirdikten sonra, log **Analytics** bıçağına, ardından çalışma alanınızın adını tıklayarak kimliğinizi ve anahtarınızı alabilirsiniz. Daha sonra, **Gelişmiş Ayarlar**altında , **Bağlı Kaynaklar**, ve daha sonra **Linux Sunucuları**, aşağıda gösterildiği gibi, ihtiyacınız olan bilgileri bulacaksınız.

 ![](media/container-service-monitoring-oms/image5.png)

### <a name="install-the-log-analytics-agent-using-a-daemonset"></a>DaemonSet kullanarak Log Analytics aracısını yükleme
DaemonSets kümedeki her ana bilgisayarda bir kapsayıcının tek bir örneğini çalıştırmak için Kubernetes tarafından kullanılır.
İzleme ajanlarını çalıştırmak için mükemmeller.

Burada [DaemonSet YAML dosyasıdır.](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) Dosyayı adlı `oms-daemonset.yaml` bir dosyaya kaydedin `WSID` ve `KEY` yer tutucu değerlerini dosyadaki çalışma alanı kimliğiniz ve anahtarınızla değiştirin.

Çalışma alanı kimliğinizi ve Anahtarınızı DaemonSet yapılandırmasına ekledikten sonra, komut satırı aracıyla kümenize Log Analytics aracısını `kubectl` yükleyebilirsiniz:

```console
kubectl create -f oms-daemonset.yaml
```

### <a name="installing-the-log-analytics-agent-using-a-kubernetes-secret"></a>Kubernetes Secret kullanarak Log Analytics aracısını yükleme
Log Analytics çalışma alanı kimliğinizi ve anahtarınızı korumak için Kubernetes Secret'ı DaemonSet YAML dosyasının bir parçası olarak kullanabilirsiniz.

- Komut dosyasını, gizli şablon dosyasını ve DaemonSet YAML dosyasını [(depodan)](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes)kopyalayın ve aynı dizinde olduğundan emin olun.
  - gizli üreten komut dosyası - secret-gen.sh
  - gizli şablon - gizli şablon.yaml
    - DaemonSet YAML dosyası - omsagent-ds-secrets.yaml
- Betiği çalıştırın. Komut dosyası, Log Analytics Çalışma Alanı Kimliği ve Birincil Anahtar isteyecektir. Bunu ekleyin ve komut dosyası çalıştırabilirsiniz böylece gizli bir yaml dosyası oluşturacaktır.

  ```console
  sudo bash ./secret-gen.sh
  ```

  - Aşağıdakileri çalıştırarak sırlar bölmesini oluşturun:

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

  - Aşağıdakileri çalıştırarak omsagent daemon setinizi oluşturun:
  
  ```console
  kubectl create -f omsagent-ds-secrets.yaml
  ```

### <a name="conclusion"></a>Sonuç
İşte bu kadar! Birkaç dakika sonra, Günlük Analizi panonuza akan verileri görebilirsiniz.
