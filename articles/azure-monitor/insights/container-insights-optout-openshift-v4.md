---
title: Azure ve Red Hat OpenShift v4 kümenizi izlemeyi durdurma | Microsoft Docs
description: Bu makalede, Azure Red Hat OpenShift ve Red Hat OpenShift sürüm 4 kümenizi kapsayıcılar için Azure Izleyici ile izlemenin nasıl durdurulabileceğinizi anlatmaktadır.
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: 768c4db8d72778b555a4f343cf2e23b8fa861991
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82196445"
---
# <a name="how-to-stop-monitoring-your-azure-and-red-hat-openshift-v4-cluster"></a>Azure ve Red Hat OpenShift v4 kümenizi izlemeyi durdurma

Azure Red Hat OpenShift ve Red Hat OpenShift sürüm 4. x kümesini izlemeyi etkinleştirdikten sonra, artık izlemek istememeye karar verirseniz, kapsayıcıyı kapsayıcılar için Azure Izleyici ile izlemeyi durdurabilirsiniz. Bu makalede bunun nasıl yapılacağı gösterilmektedir.  

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

    Bu işlem, yayını kümeden kaldırır. Komutunu çalıştırarak doğrulayabilirsiniz `helm list` :

    ```
    NAME                            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
    ```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Held, sildikten sonra bile yayınlarınızı izliyorsa, bir kümenin geçmişini denetleyebilir ve hatta ile bir yayını geri alabilirsiniz `helm rollback` .

## <a name="next-steps"></a>Sonraki adımlar

Log Analytics çalışma alanı yalnızca kümeyi izlemeyi desteklemek için oluşturulduysa ve artık gerekmiyorsa, el ile silmeniz gerekir. Bir çalışma alanının nasıl silineceği konusunda bilgi sahibi değilseniz bkz. [Azure Log Analytics çalışma alanını silme](../../log-analytics/log-analytics-manage-del-workspace.md).
