---
title: Kubernetes config ve Deploy adımları Dil Algılama
titleSuffix: Azure Cognitive Services
description: Kubernetes config ve Deploy adımları Dil Algılama
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: addb044d82429a4471e7ecd302351dd7b7eada84
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2020
ms.locfileid: "78262682"
---
### <a name="deploy-the-language-detection-container-to-an-aks-cluster"></a>Dil Algılama kapsayıcısını bir AKS kümesine dağıtma

1. Azure CLı 'yi açın ve Azure 'da oturum açın.

    ```azurecli
    az login
    ```

1. AKS kümesinde oturum açın. `your-cluster-name` ve `your-resource-group` uygun değerlerle değiştirin.

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Bu komut çalıştıktan sonra şuna benzer bir ileti bildirir:

    ```output
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Azure hesabınızda kullanabileceğiniz birden fazla aboneliğiniz varsa ve `az aks get-credentials` komutu bir hatayla döndürülürse, yaygın bir sorun, yanlış aboneliği kullanmaktır. Azure CLı oturumunuzun bağlamını, kaynaklarını oluşturduğunuz aboneliğin aynısını kullanacak şekilde ayarlayın ve yeniden deneyin.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. İstediğiniz metin düzenleyicisini açın. Bu örnek Visual Studio Code kullanır.

    ```console
    code .
    ```

1. Metin Düzenleyicisi içinde *Language. YAML*adlı yeni bir dosya oluşturun ve içine aşağıdaki YAML 'yi yapıştırın. `billing/value` ve `apikey/value` kendi bilgileriniz ile değiştirdiğinizden emin olun.

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: language
    spec:
      template:
        metadata:
          labels:
            app: language-app
        spec:
          containers:
          - name: language
            image: mcr.microsoft.com/azure-cognitive-services/language
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
      name: language
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: language-app
    ```

1. Dosyayı kaydedin ve metin düzenleyicisini kapatın.
1. Kubernetes `apply` komutunu *Language. YAML* dosyası ile hedef olarak çalıştırın:

    ```console
    kubectl apply -f language.yaml
    ```

    Komut, dağıtım yapılandırmasını başarılı bir şekilde uyguladıktan sonra aşağıdaki çıktıya benzer bir ileti görünür:

    ```output
    deployment.apps "language" created
    service "language" created
    ```
1. Pod 'ın dağıtıldığını doğrulayın:

    ```console
    kubectl get pods
    ```

    Pod 'un çalışma durumunun çıkışı:

    ```output
    NAME                         READY     STATUS    RESTARTS   AGE
    language-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Hizmetin kullanılabilir olduğunu doğrulayın ve IP adresini alın.

    ```console
    kubectl get services
    ```

    Pod 'daki *dil* hizmetinin çalışma durumunun çıkışı:

    ```output
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    language     LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```
