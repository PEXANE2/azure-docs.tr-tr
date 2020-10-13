---
title: Kubernetes config ve Deploy adımları Yaklaşım Analizi
titleSuffix: Azure Cognitive Services
description: Kubernetes config ve Deploy adımları Yaklaşım Analizi
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: ca8d4d725ff25687d1005ddab1964316a147c730
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779795"
---
### <a name="deploy-the-sentiment-analysis-container-to-an-aks-cluster"></a>Yaklaşım Analizi kapsayıcısını bir AKS kümesine dağıtma

1. Azure CLı 'yi açın ve Azure 'da oturum açın.

    ```azurecli
    az login
    ```

1. AKS kümesinde oturum açın. `your-cluster-name`Ve `your-resource-group` değerlerini uygun değerlerle değiştirin.

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

    ```console
    code .
    ```

1. Metin Düzenleyicisi içinde, *Sentiment. YAML*adlı yeni bir dosya oluşturun ve içine aşağıdaki YAML 'yi yapıştırın. `billing/value`Ve `apikey/value` bilgilerinizi kendi bilgileriniz ile değiştirdiğinizden emin olun.

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
            resources:
              requests:
                memory: 2Gi
                cpu: 1
              limits:
                memory: 4Gi
                cpu: 1
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
1. `apply` *Sentiment. YAML* dosyası Ile Kubernetes komutunu hedefi olarak çalıştırın:

    ```console
    kubectl apply -f sentiment.yaml
    ```

    Komut, dağıtım yapılandırmasını başarılı bir şekilde uyguladıktan sonra aşağıdaki çıktıya benzer bir ileti görünür:

    ```output
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Pod 'ın dağıtıldığını doğrulayın:

    ```console
    kubectl get pods
    ```

    Pod 'un çalışma durumunun çıkışı:

    ```output
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Hizmetin kullanılabilir olduğunu doğrulayın ve IP adresini alın.

    ```console
    kubectl get services
    ```

    Pod 'daki *yaklaşım hizmetinin çalışma* durumunun çıkışı:

    ```output
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```
