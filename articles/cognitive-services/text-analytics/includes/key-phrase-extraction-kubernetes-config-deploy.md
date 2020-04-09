---
title: Anahtar Tümceme Kubernetes config ve dağıtma adımları
titleSuffix: Azure Cognitive Services
description: Anahtar Tümceme Kubernetes config ve dağıtma adımları
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 6ef7efe3d48fd20c5141803430260a80395faa82
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80877870"
---
### <a name="deploy-the-key-phrase-extraction-container-to-an-aks-cluster"></a>Anahtar Tümcecik Ayıklama kapsayıcısını bir AKS kümesine dağıtma

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

1. Metin düzenleyicisi içinde *keyphrase.yaml*adlı yeni bir dosya oluşturun ve aşağıdaki YAML'yi yapıştırın. Kendi `apikey/value` bilgilerinizle `billing/value` değiştirip değiştirdiğinizden emin olun.

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: keyphrase
    spec:
      template:
        metadata:
          labels:
            app: keyphrase-app
        spec:
          containers:
          - name: keyphrase
            image: mcr.microsoft.com/azure-cognitive-services/keyphrase
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
      name: keyphrase
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: keyphrase-app
    ```

1. Dosyayı kaydedin ve metin düzenleyicisini kapatın.
1. Kubernetes `apply` komutunu *anahtar cümlesi.yaml* dosyasıyla hedef olarak çalıştırın:

    ```console
    kubectl apply -f keyphrase.yaml
    ```

    Komut dağıtım yapılandırmasını başarıyla uyguladığında, aşağıdaki çıktıya benzer bir ileti görüntülenir:

    ```output
    deployment.apps "keyphrase" created
    service "keyphrase" created
    ```
1. Bölmenin dağıtıldığını doğrulayın:

    ```console
    kubectl get pods
    ```

    Bölmenin çalışma durumu için çıktı:

    ```output
    NAME                         READY     STATUS    RESTARTS   AGE
    keyphrase-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Hizmetin kullanılabilir olduğunu doğrulayın ve IP adresini alın.

    ```console
    kubectl get services
    ```

    Bölmedeki *anahtar tümcecik* hizmetinin çalışma durumu için çıktı:

    ```output
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    keyphrase    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```
