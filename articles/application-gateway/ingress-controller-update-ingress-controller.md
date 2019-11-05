---
title: Inress denetleyicisini Held ile yükseltme
description: Bu makale, Held kullanarak bir Application Gateway girişini yükseltme hakkında bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 10/23/2019
ms.author: caya
ms.openlocfilehash: 6ed73a2172e09e7255447b4467698670c95b63af
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73513243"
---
# <a name="how-to-upgrade-application-gateway-ingress-controller-using-helm"></a>Held kullanarak Application Gateway giriş denetleyicisini yükseltme 

Kubernetes (AGIC) için Azure Application Gateway giriş denetleyicisi, Azure depolama 'da barındırılan bir Held deposu kullanılarak yükseltilebilir.

Yükseltme yordamına başlamadan önce, gerekli depoyu eklemiş olduğunuzdan emin olun:

- Şu anda eklenmiş olan Held depolarınızı görüntüleyin:

    ```bash
    helm repo list
    ```

- AGIC depoyu şu şekilde ekleyin:

    ```bash
    helm repo add \
        application-gateway-kubernetes-ingress \
        https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    ```

## <a name="upgrade"></a>Yükseltme

1. En son sürümü almak için AGIC Held deposunu yenileyin:

    ```bash
    helm repo update
    ```

1. `application-gateway-kubernetes-ingress` grafiğin kullanılabilir sürümlerini görüntüleyin:

    ``` bash
    helm search -l application-gateway-kubernetes-ingress
    ```

    Örnek yanıt:

    ```bash
    NAME                                                    CHART VERSION   APP VERSION     DESCRIPTION
    application-gateway-kubernetes-ingress/ingress-azure    0.7.0-rc1       0.7.0-rc1       Use Azure Application Gateway as the ingress for an Azure...
    application-gateway-kubernetes-ingress/ingress-azure    0.6.0           0.6.0           Use Azure Application Gateway as the ingress for an Azure...
    ```

    Yukarıdaki listeden kullanılabilir en son sürüm: `0.7.0-rc1`

1. Şu anda yüklü olan Held grafiklerini görüntüleyin:

    ```bash
    helm list
    ```

    Örnek yanıt:

    ```bash
    NAME            REVISION        UPDATED                         STATUS  CHART                   APP VERSION     NAMESPACE
    odd-billygoat   22              Fri Jun 21 15:56:06 2019        FAILED  ingress-azure-0.7.0-rc1 0.7.0-rc1       default
    ```

    Yukarıdaki örnek yanıttan hela grafik yüklemesi `odd-billygoat`olarak adlandırılmıştır. Bu adı komutların geri kalanı için kullanacağız. Gerçek dağıtım adınız büyük olasılıkla farklı olacaktır.

1. Held dağıtımını yeni bir sürüme yükseltin:

    ```bash
    helm upgrade \
        odd-billygoat \
        application-gateway-kubernetes-ingress/ingress-azure \
        --version 0.9.0-rc2
    ```

## <a name="rollback"></a>Etkin

Held dağıtımı başarısız olursa, önceki bir sürüme geri alabilirsiniz.

1. Bilinen son sağlıklı sürüm numarasını alın:

    ```bash
    helm history odd-billygoat
    ```

    Örnek çıktı:

    ```bash
    REVISION        UPDATED                         STATUS          CHART                   DESCRIPTION
    1               Mon Jun 17 13:49:42 2019        DEPLOYED        ingress-azure-0.6.0     Install complete
    2               Fri Jun 21 15:56:06 2019        FAILED          ingress-azure-xx        xxxx
    ```

    `helm history` komutunun örnek çıktısından, `odd-billygoat` son başarılı dağıtımına benzer `1`

1. Son başarılı düzeltmeye geri al:

    ```bash
    helm rollback odd-billygoat 1
    ```
