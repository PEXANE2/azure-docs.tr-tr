---
title: Azure HDInsight kümesi için VM 'Leri yeniden başlatma
description: HDInsight kümesi için yanıt vermeyen VM 'Leri yeniden başlatma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/22/2020
ms.openlocfilehash: 46efccad60c06c73145bbf30c119f6a47cb856d5
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85323459"
---
# <a name="reboot-vms-for-hdinsight-cluster"></a>HDInsight kümesi için VM 'Leri yeniden başlatma

HDInsight kümeleri, küme düğümleri olarak VM gruplarını içerir. Uzun süre çalışan kümeler için, bu düğümler çeşitli nedenlerle yanıt vermemeye başlayabilir. Bu makalede, bir HDInsight kümesinde yanıt vermeyen VM 'Lerin yeniden başlatılması açıklanmaktadır.

## <a name="when-to-reboot"></a>Ne zaman yeniden başlatılacak

VM 'Lerin kümedeki yeniden başlatılması, düğümün kapalı kalma süresini ve düğümdeki hizmetlerin yeniden başlatılmasını sağlar. Düğüm yeniden başlatılırken, küme sağlıksız hale gelebilir, işler yavaşlayabilir veya başarısız olabilir. Etkin baş düğümü yeniden başlatmaya çalışıyorsanız, çalışan tüm işler sonlandırılacak ve hizmetler yeniden çalışmaya ana kadar işleri kümeye gönderemezsiniz. Yalnızca gerektiğinde VM 'Leri yeniden başlatmayı göz önünde bulundurmanız gerekir. VM 'Leri yeniden başlatmayı ne zaman düşünmeliyim için bazı rehberlik aşağıda verilmiştir.

- Düğümde SSH oluşturamazsınız, ancak pingler yanıt verir.
- Çalışan düğümü, ambarı Kullanıcı arabiriminde sinyal olmadan kapatılmış.
- Geçici disk, düğüm üzerinde dolu.
- VM 'deki işlem tablosu işlemin tamamlandığı birçok girişe sahiptir, ancak "sonlandırılmış durum" ile listelenir.

## <a name="use-rest-api-to-reboot-vms"></a>VM 'Leri yeniden başlatmak için REST API kullanma

Düğüm yeniden başlatma yalnızca REST API aracılığıyla desteklenir. İstekleri HDInsight 'a göndermek için API belgesi içindeki **TRY It** özelliğini kullanabilirsiniz. Düğüm yeniden başlatma işlemini kullanmak için iki adım gereklidir: düğümleri listeleyin ve düğümleri yeniden başlatın.

1. Düğümleri listeleyin. Küme düğümü listesini REST API veya ambarı 'ndan alabilirsiniz. [HDInsight listesinde REST API işlemler](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts) için daha fazla ayrıntı bulunabilir

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

2. Konakları yeniden başlatın. Yeniden başlatmak istediğiniz düğümlerin adlarını aldıktan sonra, REST API yeniden başlatma Konakları ' nı kullanarak düğümleri yeniden başlatın. [HDInsight yeniden başlatma konakları REST API işlemi](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts) için daha fazla ayrıntı bulunabilir

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

Yeniden başlatmak istediğiniz düğümlerin gerçek adları, istek gövdesindeki bir JSON dizisinde belirtilmiştir.

```json
[
  "gateway1",
  "gateway3"
]
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight sanal makineleri REST API](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [HDInsight REST API](https://docs.microsoft.com/rest/api/hdinsight/)