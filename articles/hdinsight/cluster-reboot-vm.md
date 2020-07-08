---
title: Azure HDInsight kümesi için VM 'Leri yeniden başlatma
description: HDInsight kümesi için yanıt vermeyen VM 'Leri yeniden başlatma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: c0f0bd9eb423b3de6a602647dff93fd9fce6e13e
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86077023"
---
# <a name="reboot-vms-for-hdinsight-cluster"></a>HDInsight kümesi için VM 'Leri yeniden başlatma

HDInsight kümeleri, küme düğümleri olarak VM gruplarını içerir. Uzun süre çalışan kümeler için, bu düğümler çeşitli nedenlerle yanıt vermemeye başlayabilir. Bu makalede, bir HDInsight kümesinde yanıt vermeyen VM 'Lerin yeniden başlatılması açıklanmaktadır.

## <a name="when-to-reboot"></a>Ne zaman yeniden başlatılacak

> [!WARNING]  
> VM 'Lerin kümedeki yeniden başlatılması, düğümün kapalı kalma süresini ve düğümdeki hizmetlerin yeniden başlatılmasını sağlar. 

Düğüm yeniden başlatılırken, küme sağlıksız hale gelebilir, işler yavaşlayabilir veya başarısız olabilir. Etkin baş düğümü yeniden başlatmaya çalışıyorsanız, çalışan tüm işler sonlandırılacak ve hizmetler yeniden çalışmaya ana kadar işleri kümeye gönderemezsiniz. Yalnızca gerektiğinde VM 'Leri yeniden başlatmayı göz önünde bulundurmanız gerekir. VM 'Leri yeniden başlatmayı ne zaman düşünmeliyim için bazı rehberlik aşağıda verilmiştir.

- Düğümde SSH oluşturamazsınız, ancak pingler yanıt verir.
- Çalışan düğümü, ambarı Kullanıcı arabiriminde sinyal olmadan kapatılmış.
- Geçici disk, düğüm üzerinde dolu.
- VM 'deki işlem tablosu işlemin tamamlandığı birçok girişe sahiptir, ancak "sonlandırılmış durum" ile listelenir.

> [!WARNING]  
> Veri kaybına neden olabileceğinden, **HBase** ve **Kafka** Clustes için VM 'leri yeniden yüklerken daha dikkatli olmanız gerekir.

## <a name="use-powershell-to-reboot-vms"></a>VM 'Leri yeniden başlatmak için PowerShell 'i kullanma

Düğüm yeniden başlatma işlemini kullanmak için iki adım gereklidir: düğümleri listeleyin ve düğümleri yeniden başlatın.

1. Düğümleri listeleyin. [Get-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost)aracılığıyla küme düğüm listesini alabilirsiniz. 

  ```
  Get-AzHDInsightHost -ClusterName myclustername
  ```

2. Konakları yeniden başlatın. Yeniden başlatmak istediğiniz düğümlerin adlarını aldıktan sonra, [restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)kullanarak düğümleri yeniden başlatın.

  ```
  Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
  ```

## <a name="use-rest-api-to-reboot-vms"></a>VM 'Leri yeniden başlatmak için REST API kullanma

İstekleri HDInsight 'a göndermek için API belgesi içindeki **TRY It** özelliğini kullanabilirsiniz. Düğüm yeniden başlatma işlemini kullanmak için iki adım gereklidir: düğümleri listeleyin ve düğümleri yeniden başlatın.

1. Düğümleri listeleyin. Küme düğümü listesini REST API veya ambarı 'ndan alabilirsiniz. [HDInsight listesinde REST API işlemler](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts)için daha fazla ayrıntı bulunabilir.

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

2. Konakları yeniden başlatın. Yeniden başlatmak istediğiniz düğümlerin adlarını aldıktan sonra, düğümleri yeniden başlatmak için REST API yeniden başlatma düğümlerini kullanın. Düğüm adı " **NodeType (WN/hn/ZK/GW)" + "x" + "küme adının ilk 6 karakteri"** örüntüsünün izler. [HDInsight 'ın yeniden başlatma REST API işlemi](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts)sırasında daha fazla ayrıntı bulunabilir.

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/restartHosts?api-version=2018-06-01-preview
    ```

Yeniden başlatmak istediğiniz düğümlerin gerçek adları, istek gövdesindeki bir JSON dizisinde belirtilmiştir.

```json
[
  "wn0-abcdef",
  "zk1-abcdef"
]
```

## <a name="next-steps"></a>Sonraki adımlar

* [Restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)
* [HDInsight sanal makineleri REST API](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines)
* [HDInsight REST API](https://docs.microsoft.com/rest/api/hdinsight/)
