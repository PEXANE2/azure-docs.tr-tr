---
title: Miğfer ile yükseltme giriş denetleyicisi
description: Bu makalede, Helm kullanarak bir Uygulama Ağ Geçidi Girişi yükseltme hakkında bilgi sağlar.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: article
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 3903ccd1c15765d06cd1794a40567e2c70062538
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73795898"
---
# <a name="how-to-upgrade-application-gateway-ingress-controller-using-helm"></a>Helm kullanarak Application Gateway Giriş Denetleyicisini yükseltme 

Kubernetes için Azure Uygulama Ağ Geçidi Denetleyicisi (AGIC), Azure Depolama'da barındırılan Bir Miğfer deposu kullanılarak yükseltilebilir.

Yükseltme prosedürüne başlamadan önce, gerekli depoyu eklediğinizden emin olun:

- Şu anda eklenen Miğfer depolarınızı şu şekilde görüntüleyin:

    ```bash
    helm repo list
    ```

- AGIC repo'yu aşağıdakilerle ekleyin:

    ```bash
    helm repo add \
        application-gateway-kubernetes-ingress \
        https://appgwingress.blob.core.windows.net/ingress-azure-helm-package/
    ```

## <a name="upgrade"></a>Yükseltme

1. En son sürümü almak için AGIC Miğfer deposunu yenileyin:

    ```bash
    helm repo update
    ```

1. Grafiğin kullanılabilir `application-gateway-kubernetes-ingress` sürümlerini görüntüleyin:

    ``` bash
    helm search -l application-gateway-kubernetes-ingress
    ```

    Örnek yanıt:

    ```bash
    NAME                                                    CHART VERSION   APP VERSION     DESCRIPTION
    application-gateway-kubernetes-ingress/ingress-azure    0.7.0-rc1       0.7.0-rc1       Use Azure Application Gateway as the ingress for an Azure...
    application-gateway-kubernetes-ingress/ingress-azure    0.6.0           0.6.0           Use Azure Application Gateway as the ingress for an Azure...
    ```

    Yukarıdaki listeden en son kullanılabilir sürümü:`0.7.0-rc1`

1. Şu anda yüklü olan Miğfer grafiklerini görüntüleyin:

    ```bash
    helm list
    ```

    Örnek yanıt:

    ```bash
    NAME            REVISION        UPDATED                         STATUS  CHART                   APP VERSION     NAMESPACE
    odd-billygoat   22              Fri Jun 21 15:56:06 2019        FAILED  ingress-azure-0.7.0-rc1 0.7.0-rc1       default
    ```

    Yukarıdaki örnek yanıttan Helm grafik `odd-billygoat`yüklemesi adı verilir. Bu adı diğer komutlar için kullanacağız. Gerçek dağıtım adınız büyük olasılıkla farklı olacaktır.

1. Miğfer dağıtımını yeni bir sürüme yükseltin:

    ```bash
    helm upgrade \
        odd-billygoat \
        application-gateway-kubernetes-ingress/ingress-azure \
        --version 0.9.0-rc2
    ```

## <a name="rollback"></a>Geri alma

Miğfer dağıtımı başarısız olursa, önceki sürüme geri dönebilirsiniz.

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

    Komutun `helm history` örnek çıktısından, revizyonun son başarılı `odd-billygoat` dağıtımıgibi görünüyor.`1`

1. Son başarılı revizyona geri dönüş:

    ```bash
    helm rollback odd-billygoat 1
    ```
