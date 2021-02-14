---
title: Azure IoT Edge iş yüklerini dağıtma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 02/10/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure IoT Edge iş yüklerini dağıtma
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: f228b79f14ab24281415cd4bd5964fc86a095d3c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100390445"
---
# <a name="deploy-azure-iot-edge-workloads-preview"></a>Azure IoT Edge iş yüklerini dağıtma (Önizleme)

## <a name="overview"></a>Genel Bakış

Azure Arc ve Azure IoT Edge birbirlerinin yeteneklerini kolayca tamamlayabilir. 

Azure Arc, küme işleçleri için bir kümenin temel bileşenlerini yapılandırmak ve küme ilkelerini uygulamak ve zorlamak için mekanizmalar sağlar. 

Azure IoT Edge, uygulama işleçlerinin iş yüklerini uygun bulut alımı ve çift yönlü iletişim temel elemanlarına göre ölçeklendirerek uzaktan dağıtmasına ve yönetmesine olanak tanır. 

Aşağıdaki diyagramda Azure Arc ve Azure IoT Edge ilişkisi gösterilmektedir:

![IoT yay yapılandırması](./media/edge-arc.png)

## <a name="pre-requisites"></a>Ön koşullar

* [Bir IoT Edge cihazı kaydedin](../../iot-edge/quickstart-linux.md#register-an-iot-edge-device) ve [sanal sıcaklık algılayıcı modülünü dağıtın](../../iot-edge/quickstart-linux.md#deploy-a-module). Aşağıda bahsedilen *. YAML değerleri* için cihazın bağlantı dizesine göz önünde bulabilirsiniz.

* [Kubernetes için IoT Edge](https://aka.ms/edgek8sdoc) , Azure Arc Flox operatörü aracılığıyla dağıtmak için desteğini kullanın.

* IoT Edge Held grafiği için [*values. YAML*](https://github.com/Azure/iotedge/blob/preview/iiot/kubernetes/charts/edge-kubernetes/values.yaml) dosyasını indirin ve `deviceConnectionString` dosyanın sonundaki yer tutucuyu, daha önce not ettiğiniz bağlantı dizesiyle değiştirin. Gerektiğinde desteklenen diğer grafik yükleme seçeneklerini ayarlayın. IoT Edge iş yükü için bir ad alanı oluşturun ve içinde bir gizli dizi oluşturun:

  ```
  $ kubectl create ns iotedge

  $ kubectl create secret generic dcs --from-file=fully-qualified-path-to-values.yaml --namespace iotedge
  ```

  Ayrıca, [küme yapılandırma örneğini](./use-gitops-connected-cluster.md)kullanarak uzaktan de ayarlayabilirsiniz.

## <a name="connect-a-cluster"></a>Bir kümeyi bağlama

`az` `connectedk8s` Bir Kubernetes kümesini Azure yaya bağlamak IÇIN Azure CLI uzantısını kullanın:

  ```
  az connectedk8s connect --name AzureArcIotEdge --resource-group AzureArcTest
  ```

## <a name="create-a-configuration-for-iot-edge"></a>IoT Edge için bir yapılandırma oluşturma

[Örnek git deposu](https://github.com/veyalla/edgearc) , IoT Edge Held grafiğine işaret eder ve önkoşul bölümünde oluşturulan gizli dizi ile sonuçlanır.

`az` `k8sconfiguration` Bağlı kümeyi git deposuna bağlayan bir yapılandırma oluşturmak IÇIN Azure CLI uzantısı 'nı kullanın:

  ```
  az k8sconfiguration create --name iotedge --cluster-name AzureArcIotEdge --resource-group AzureArcTest --operator-instance-name iotedge --operator-namespace azure-arc-iot-edge --enable-helm-operator --helm-operator-chart-version 0.6.0 --helm-operator-chart-values "--set helm.versions=v3" --repository-url "git://github.com/veyalla/edgearc.git" --cluster-scoped
  ```

Birkaç dakika içinde, kümenizin ad alanına dağıtılan IoT Edge iş yükü modüllerini görmeniz gerekir `iotedge` . 

`SimulatedTemperatureSensor`Oluşturulan örnek değerleri görmek için bu ad alanındaki Pod günlüklerini görüntüleyin. Ayrıca, [Visual Studio Code Için Azure IoT Hub araç seti uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit)kullanarak IoT Hub 'ınıza gelen iletileri izleyebilirsiniz.

## <a name="cleanup"></a>Temizleme

Yapılandırmayı şu kullanarak kaldır:

```
az k8sconfiguration delete -g AzureArcTest --cluster-name AzureArcIotEdge --name iotedge
```

## <a name="next-steps"></a>Sonraki adımlar

[Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
