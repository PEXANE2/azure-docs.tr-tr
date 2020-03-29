---
title: Dil Algılama Kubernetes config ve dağıtma adımları
titleSuffix: Azure Cognitive Services
description: Dil Algılama Kubernetes config ve dağıtma adımları
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: addb044d82429a4471e7ecd302351dd7b7eada84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78262682"
---
### <a name="deploy-the-language-detection-container-to-an-aks-cluster"></a>Dil Algılama kapsayıcısını bir AKS kümesine dağıtma

1. Azure CLI'yi açın ve Azure'da oturum açın.

    ```azurecli
    az login
    ```

1. AKS kümesinde oturum açın. `your-cluster-name` Değiştirin `your-resource-group` ve uygun değerlerle.

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Bu komut çalıştırıladıktan sonra, aşağıdakilere benzer bir ileti bildirir:

    ```output
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Azure hesabınızda kullanabileceğiniz birden çok aboneliğiniz `az aks get-credentials` varsa ve komut bir hatayla döndürürse, sık karşılaşılan bir sorun yanlış aboneliği kullanıyor olmasıdır. Azure CLI oturumunuzun bağlamını, kaynakları oluşturduğunuz aboneliği kullanacak ve yeniden deneyin.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Seçtiğim metin düzenleyicisini açın. Bu örnek, Visual Studio Code kullanır.

    ```console
    code .
    ```

1. Metin düzenleyicisi *içinde, language.yaml*adlı yeni bir dosya oluşturun ve aşağıdaki YAML'yi yapıştırın. Kendi `apikey/value` bilgilerinizle `billing/value` değiştirip değiştirdiğinizden emin olun.

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
1. Kubernetes `apply` komutunu hedef olarak *language.yaml* dosyasıyla çalıştırın:

    ```console
    kubectl apply -f language.yaml
    ```

    Komut dağıtım yapılandırmasını başarıyla uyguladığında, aşağıdaki çıktıya benzer bir ileti görüntülenir:

    ```output
    deployment.apps "language" created
    service "language" created
    ```
1. Bölmenin dağıtıldığını doğrulayın:

    ```console
    kubectl get pods
    ```

    Bölmenin çalışma durumu için çıktı:

    ```output
    NAME                         READY     STATUS    RESTARTS   AGE
    language-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Hizmetin kullanılabilir olduğunu doğrulayın ve IP adresini alın.

    ```console
    kubectl get services
    ```

    Bölmedeki *dil* hizmetinin çalışma durumu için çıktı:

    ```output
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    language     LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```
