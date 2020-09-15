---
title: Azure HDInsight kümeleri için VM 'Leri yeniden başlatma
description: Azure HDInsight kümeleri için yanıt vermeyen VM 'Leri yeniden başlatma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: how-to
ms.date: 06/22/2020
ms.openlocfilehash: 149a82526263f5e372db81b5a92a9ee90a2c76f3
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90089981"
---
# <a name="reboot-vms-for-hdinsight-clusters"></a>HDInsight kümeleri için VM 'Leri yeniden başlatma

Azure HDInsight kümeleri, küme düğümleri olarak sanal makine (VM) gruplarını içerir. Uzun süre çalışan kümeler için, bu düğümler çeşitli nedenlerle yanıt vermemeye meyebilir. Bu makalede, bir HDInsight kümesinde yanıt vermeyen VM 'Lerin yeniden başlatılması açıklanmaktadır.

## <a name="when-to-reboot"></a>Ne zaman yeniden başlatılacak

> [!WARNING]
> Bir kümedeki VM 'Leri yeniden başlattığınızda, düğüm kullanılamaz ve düğümdeki hizmetlerin yeniden başlatılması gerekir.

Bir düğüm yeniden başlatıldığında, küme sağlıksız hale gelebilir ve işler yavaşlayabilir veya başarısız olabilir. Etkin baş düğümü yeniden başlatmaya çalışıyorsanız, çalışan tüm işler durdurulacak. Hizmetler yeniden çalışmaya ana kadar işleri kümeye gönderemeyeceksiniz. Bu nedenlerden dolayı VM 'Leri yalnızca gerektiğinde yeniden başlatmanız gerekir. VM 'Leri yeniden başlatmayı göz önünde bulundurun:

- Düğüme ulaşmak için SSH kullanamazsınız, ancak pingler yanıt verir.
- Çalışan düğümü, ambarı Kullanıcı arabiriminde sinyal olmadan çalışmıyor.
- Geçici disk, düğüm üzerinde dolu.
- VM 'deki işlem tablosu işlemin tamamlandığı birçok girişe sahiptir, ancak "sonlandırılmış durum" ile listelenir.

> [!WARNING]
> Yeniden başlatma verilerin kaybolmasına neden olabileceğinden, **HBase** ve **Kafka** kümeleri için VM 'leri yeniden başlatma konusunda dikkatli olun.

## <a name="use-powershell-to-reboot-vms"></a>VM 'Leri yeniden başlatmak için PowerShell 'i kullanma

Düğüm yeniden başlatma işlemini kullanmak için iki adım gereklidir: düğümleri listeleyin ve düğümleri yeniden başlatın.

1. Düğümleri listeleyin. [Get-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsighthost)adresinde küme düğümü listesini alabilirsiniz.

      ```
      Get-AzHDInsightHost -ClusterName myclustername
      ```

1. Konakları yeniden başlatın. Yeniden başlatmak istediğiniz düğümlerin adlarını aldıktan sonra [restart-AzHDInsightHost](https://docs.microsoft.com/powershell/module/az.hdinsight/restart-azhdinsighthost)kullanarak düğümleri yeniden başlatın.

      ```
      Restart-AzHDInsightHost -ClusterName myclustername -Name wn0-myclus, wn1-myclus
      ```

## <a name="use-a-rest-api-to-reboot-vms"></a>VM 'Leri yeniden başlatmak için REST API kullanma

İstekleri HDInsight 'a göndermek için API belgesi içindeki **TRY It** özelliğini kullanabilirsiniz. Düğüm yeniden başlatma işlemini kullanmak için iki adım gereklidir: düğümleri listeleyin ve düğümleri yeniden başlatın.

1. Düğümleri listeleyin. Küme düğümü listesini REST API veya ambarı 'ndan alabilirsiniz. Daha fazla bilgi için bkz. [HDInsight List hosts REST API işlemi](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/listhosts).

    ```
    POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.HDInsight/clusters/{clusterName}/listHosts?api-version=2018-06-01-preview
    ```

1. Konakları yeniden başlatın. Yeniden başlatmak istediğiniz düğümlerin adlarını aldıktan sonra, düğümleri yeniden başlatmak için REST API kullanarak düğümleri yeniden başlatın. Düğüm adı, küme adının *NodeType (WN/hn/ZK/GW)*  +  *x*-  +  *ilk altı*karakterinin düzenine uyar. Daha fazla bilgi için bkz. [HDInsight yeniden başlatma konakları REST API işlemi](https://docs.microsoft.com/rest/api/hdinsight/virtualmachines/restarthosts).

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
