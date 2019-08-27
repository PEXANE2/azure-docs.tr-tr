---
title: Kubernetes config ve Deploy adımları Yaklaşım Analizi
titleSuffix: Azure Cognitive Services
description: Kubernetes config ve Deploy adımları Yaklaşım Analizi
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 08/21/2019
ms.author: dapine
ms.openlocfilehash: 5d16500b45cbc6190186835d0c793a48fd121bd0
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051881"
---
## <a name="deploy-the-sentiment-analysis-container-to-an-aks-cluster"></a>Yaklaşım Analizi kapsayıcısını bir AKS kümesine dağıtma

1. Azure CLı 'yi açın ve Azure 'da oturum açın.

    ```azurecli
    az login
    ```

1. AKS kümesinde oturum açın. `your-cluster-name` Ve`your-resource-group` değerlerini uygun değerlerle değiştirin.

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Bu komut çalıştıktan sonra şuna benzer bir ileti bildirir:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Azure hesabınızda kullanabileceğiniz birden çok aboneliğiniz varsa ve `az aks get-credentials` komut bir hatayla döndürülürse, yaygın bir sorun, yanlış aboneliği kullanıyor olmanız olabilir. Azure CLı oturumunuzun bağlamını, kaynaklarını oluşturduğunuz aboneliğin aynısını kullanacak şekilde ayarlayın ve yeniden deneyin.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. İstediğiniz metin düzenleyicisini açın. Bu örnek Visual Studio Code kullanır.

    ```azurecli
    code .
    ```

1. Metin Düzenleyicisi içinde, *Sentiment. YAML*adlı yeni bir dosya oluşturun ve içine aşağıdaki YAML 'yi yapıştırın. `billing/value` Ve`apikey/value` bilgilerinizi kendi bilgileriniz ile değiştirdiğinizden emin olun.

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: # {ENDPOINT_URI}
            - name: apikey
              value: # {API_KEY}
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. Dosyayı kaydedin ve metin düzenleyicisini kapatın.
1. *Sentiment. YAML* dosyası `apply` ile Kubernetes komutunu hedefi olarak çalıştırın:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Komut, dağıtım yapılandırmasını başarılı bir şekilde uyguladıktan sonra aşağıdaki çıktıya benzer bir ileti görünür:

    ```console
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Pod 'ın dağıtıldığını doğrulayın:

    ```console
    kubectl get pods
    ```

    Pod 'un çalışma durumunun çıkışı:

    ```console
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Hizmetin kullanılabilir olduğunu doğrulayın ve IP adresini alın.

    ```console
    kubectl get services
    ```

    Pod 'daki yaklaşım hizmetinin çalışma durumunun çıkışı:

    ```console
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```
