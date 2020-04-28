---
title: Karma Kubernetes kümenizi izlemeyi durdurma | Microsoft Docs
description: Bu makalede, kapsayıcı için Azure Izleyici ile karma Kubernetes kümenizi izlemeyi nasıl durdurulabileceğinizi açıklanmaktadır.
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: f2f3a8671c1f2baf60d399cc87f2f843dfee4f70
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82196224"
---
# <a name="how-to-stop-monitoring-your-hybrid-cluster"></a>Karma kümenizi izlemeyi durdurma

Azure Stack veya şirket içinde çalışan Kubernetes kümenizi izlemeyi etkinleştirdikten sonra, artık bunu izlemek istememeye karar vermeniz durumunda kümeyi, kapsayıcılar için Azure Izleyici ile izlemeyi durdurabilirsiniz. Bu makalede bunun nasıl yapılacağı gösterilmektedir.  

## <a name="how-to-stop-monitoring-using-helm"></a>Held kullanarak izlemeyi durdurma

1. Kümenizde yüklü olan kapsayıcıların Helm grafik sürümünün Azure Izleyicisini belirlemek için aşağıdaki Helm komutunu çalıştırın.

    ```
    helm list
    ```

    Çıktı aşağıdakine benzeyecektir:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    azmon-containers-release-1      default         3               2020-04-21 15:27:24.1201959 -0700 PDT   deployed        azuremonitor-containers-2.7.0   7.0.0-1
    ```

    *Azmon-kapsayıcılar-Release-1* kapsayıcılar Için Azure izleyici için Helm grafik sürümünü temsil eder.

2. Grafik sürümünü silmek için aşağıdaki Held komutunu çalıştırın.

    `helm delete <releaseName>`

    Örnek:

    `helm delete azmon-containers-release-1`

    Bu işlem, yayını kümeden kaldırır. `helm list` Komutunu çalıştırarak doğrulayabilirsiniz:

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Held, sildikten sonra bile yayınlarınızı izliyorsa, bir kümenin geçmişini denetleyebilir ve hatta ile `helm rollback`bir yayını geri alabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Log Analytics çalışma alanı yalnızca kümeyi izlemeyi desteklemek için oluşturulduysa ve artık gerekmiyorsa, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz bkz. [Azure Log Analytics çalışma alanını silme](../../log-analytics/log-analytics-manage-del-workspace.md).
