---
title: Azure IoT Edge iş yüklerini dağıtma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure IoT Edge iş yüklerini dağıtma
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: 2a688a221b2f4865d51bca2ebf4aaa0b1f714290
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85193796"
---
# <a name="deploy-azure-iot-edge-workloads-preview"></a>Azure IoT Edge iş yüklerini dağıtma (Önizleme)

## <a name="overview"></a>Genel Bakış

Azure Arc ve Azure IoT Edge birbirlerinin yeteneklerini çok iyi tamamlayalım. Azure Arc, küme işleçleri için bir kümenin temel bileşenlerini yapılandırma ve küme ilkelerini uygulama ve zorunlu kılmak için mekanizmalar sağlar. IoT Edge, uygulama işleçlerinin iş yüklerini uygun bulut alımı ve çift yönlü iletişim temel elemanlarına göre ölçeklendirerek uzaktan dağıtıp yönetmesine olanak tanır. Aşağıdaki diyagramda şunu gösterilmektedir:

![IoT yay yapılandırması](./media/edge-arc.png)

## <a name="pre-requisites"></a>Ön koşullar

* [Bir IoT Edge cihazı kaydedin](https://docs.microsoft.com/azure/iot-edge/quickstart-linux#register-an-iot-edge-device) ve [sanal sıcaklık algılayıcı modülünü dağıtın](https://docs.microsoft.com/azure/iot-edge/quickstart-linux#deploy-a-module). Cihazın bağlantı dizesini aklınızda olduğunuzdan emin olun.

* [Kubernetes için IoT Edge](https://aka.ms/edgek8sdoc) , Azure Arc Flox operatörü aracılığıyla dağıtmak için desteğini kullanın.

* IoT Edge Held grafiği için [**values. YAML**](https://github.com/Azure/iotedge/blob/master/kubernetes/charts/edge-kubernetes/values.yaml) dosyasını indirin ve dosyanın sonundaki **deviceconnectionstring** yer tutucusunu, 1. adımda belirtilen kodla değiştirin. Desteklenen herhangi bir grafik yükleme seçeneğini gerekli şekilde ayarlayabilirsiniz. IoT Edge iş yükü için bir ad alanı oluşturun ve içinde bir gizli dizi oluşturun:

    ```
    $ kubectl create ns iotedge

    $ kubectl create secret generic dcs --from-file=fully-qualified-path-to-values.yaml --namespace iotedge
    ```

    Bu ayarı, [küme yapılandırma örneğini](./use-gitops-connected-cluster.md)kullanarak uzaktan de ayarlayabilirsiniz.

## <a name="connect-a-cluster"></a>Bir kümeyi bağlama

`az` `connectedk8s` Bir Kubernetes kümesini Azure yaya bağlamak için CLI uzantısını kullanın:

  ```
  az connectedk8s connect --name AzureArcIotEdge --resource-group AzureArcTest
  ```

## <a name="create-a-configuration-for-iot-edge"></a>IoT Edge için bir yapılandırma oluşturma

Örnek depo:https://github.com/veyalla/edgearc

Bu depo, IoT Edge Held grafiğine işaret eder ve Önkoşullar bölümünde oluşturulan gizli dizi ile sonuçlanır.

1. `az` `k8sconfiguration` Bağlı kümeyi git deposuna bağlamak için bir yapılandırma oluşturmak üzere CLI uzantısını kullanın:

    ```
    az k8sconfiguration create --name iotedge --cluster-name AzureArcIotEdge --resource-group AzureArcTest --operator-instance-name iotedge --operator-namespace azure-arc-iot-edge --enable-helm-operator --helm-operator-chart-version 0.6.0 --helm-operator-chart-values "--set helm.versions=v3" --repository-url "git://github.com/veyalla/edgearc.git" --cluster-scoped
    ```

    Bir veya iki dakika içinde, kümenizde ad alanına dağıtılan IoT Edge iş yükü modüllerini görmeniz gerekir `iotedge` . `SimulatedTemperatureSensor`Oluşturulan örnek değerleri görmek için bu ad alanındaki Pod 'un günlüklerini görüntüleyebilirsiniz. Ayrıca, [Visual Studio Code Için Azure IoT Hub araç seti uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak IoT Hub 'ınıza gelen iletileri izleyebilirsiniz.

## <a name="cleanup"></a>Temizleme

Yapılandırmayı şu şekilde kaldırabilirsiniz:

```
az k8sconfiguration delete -g AzureArcTest --cluster-name AzureArcIotEdge --name iotedge
```

## <a name="next-steps"></a>Sonraki adımlar

[Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
