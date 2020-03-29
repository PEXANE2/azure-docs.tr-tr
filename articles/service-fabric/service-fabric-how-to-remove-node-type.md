---
title: Azure Hizmet Kumaşı'nda düğüm türünü kaldırma | Microsoft Dokümanlar
description: Azure'da çalışan bir Hizmet Kumaşı kümesinden düğüm türünü nasıl kaldırabilirsiniz öğrenin.
author: inputoutputcode
manager: sridmad
ms.topic: conceptual
ms.date: 02/21/2020
ms.author: chrpap
ms.openlocfilehash: 330b455a61c45ccdb59e5aef8162fd1b04859a00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969399"
---
# <a name="how-to-remove-a-service-fabric-node-type"></a>Service Fabric düğüm türü nasıl kaldırılır?
Bu makalede, bir kümeden varolan bir düğüm türünü kaldırarak bir Azure Hizmet Dokusu kümesini nasıl ölçeklendirilir. Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine ye veya VM'ye düğüm denir. Sanal makine ölçek kümeleri, sanal makine koleksiyonunu küme olarak dağıtmak ve yönetmek için kullandığınız bir Azure bilgi işlem kaynağıdır. Azure kümesinde tanımlanan her düğüm türü [ayrı bir ölçek kümesi olarak ayarlanır.](service-fabric-cluster-nodetypes.md) Her düğüm türü daha sonra ayrı ayrı yönetilebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, düğüm türünü (sanal makine ölçek kümesi) ve tüm düğümleri kaldırarak kümeyi yatay olarak ölçeklendirebilirsiniz.  Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz.  Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

> [!WARNING]
> Bir üretim kümesinden bir düğüm türünü kaldırmak için bu yaklaşımın kullanılması sık olarak kullanılması önerilmez. Düğüm türünün arkasındaki sanal makine ölçeği ayarlı kaynağı sildiği için tehlikeli bir komutdur. 

## <a name="durability-characteristics"></a>Dayanıklılık özellikleri
Remove-AzServiceFabricNodeType kullanırken güvenlik hız üzerinde öncelik verilir. Düğüm türü Gümüş veya Altın [dayanıklılık düzeyi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)olmalıdır, çünkü:
- Bronz, durum bilgilerini kaydetme konusunda size herhangi bir garanti vermez.
- Gümüş ve Altın dayanıklılık, ölçek kümesindeki herhangi bir değişikliği yakalar.
- Gold ayrıca ölçek kümesi altında Azure güncelleştirmeleri üzerinde denetim sağlar.

Hizmet Kumaşı, verilerin kaybolmaması için temel değişiklikleri ve güncelleştirmeleri "yönetir". Ancak, Bronz dayanıklılık ile bir düğüm türünü kaldırdığınızda, durum bilgilerini kaybedebilirsiniz. Birincil düğüm türünü kaldırıyorsanız ve uygulamanız durum yoksa, Bronz kabul edilebilir. Üretimde durum sallanabilir iş yüklerini çalıştırdığınızda, minimum yapılandırma Gümüş olmalıdır. Benzer şekilde, üretim senaryoları için birincil düğüm türü her zaman Gümüş veya Altın olmalıdır.

### <a name="more-about-bronze-durability"></a>Bronz dayanıklılık hakkında daha fazla

Bronz olan bir düğüm türünü çıkarırken, düğüm türündeki tüm düğümler hemen aşağı iner. Service Fabric herhangi bir Bronz düğüm ölçeği seti güncelleştirmelerini hapsetmez, böylece tüm VM'ler hemen iner. Bu düğümlerde bir şey belirtebilen varsa, veriler kaybolur. Şimdi, vatansız olsanız bile, Service Fabric'deki tüm düğümler halkaya katılır, bu yüzden bütün bir mahalle kaybolabilir, bu da kümenin kendisini istikrarsızlaştırabilir.

## <a name="remove-a-node-type"></a>Düğüm türünü kaldırma

1. Lütfen işleme başlamadan önce bu ön koşullara dikkat edin.

    - Küme sağlıklı.
    - Düğüm türü kaldırıldıktan sonra yeterli kapasite, örneğin olacaktır. gerekli yineleme sayısını yerleştirmek için düğüm sayısı.

2. Düğüm türünden düğüm türünü kullanmak için yerleşim kısıtlamaları olan tüm hizmetleri taşıyın.

    - Düğüm türüne artık başvuru yapmak için Uygulama / Hizmet Bildirimini değiştirin.
    - Değişikliği dağıtın.

    O zaman bunu doğrulayın:
    - Yukarıda değiştirilen tüm hizmetler artık düğüm türüne ait düğümüzerinde çalışmaz.
    - Tüm hizmetler sağlıklı.

3. Düğüm türünü birincil olmayan olarak işaretleme (Birincil olmayan düğüm türleri için atla)

    - Dağıtım için kullanılan Azure Kaynak Yöneticisi şablonuna yer bulun.
    - Servis Kumaşı bölümünde düğüm türüyle ilgili bölümü bulun.
    - Değişiklik Birincil özelliği false için. ** Bu görevde düğüm türüyle ilgili bölümü kaldırmayın.
    - Değiştirilmiş Azure Kaynak Yöneticisi şablonuna dağıtın. ** Küme yapılandırması bağlı olarak bu adım biraz zaman alabilir.
    
    O zaman bunu doğrulayın:
    - Portal'daki Servis Kumaşı Bölümü kümenin hazır olduğunu gösterir.
    - Küme sağlıklıdır.
    - Düğüm türüne ait düğümlerin hiçbiri Tohum Düğümü olarak işaretlenmemiştir.

4. Düğüm türü için verileri devre dışı.

    PowerShell'i kullanarak kümeye bağlanın ve aşağıdaki adımı çalıştırın.
    
    ```powershell
    $nodeType = "" # specify the name of node type
    $nodes = Get-ServiceFabricNode
    
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Disable-ServiceFabricNode -Intent RemoveNode -NodeName $node.NodeName -Force
      }
    }
    ```

    - Bronz dayanıklılık için, tüm düğümlerin engelli durumuna geçmesini bekleyin
    - Gümüş ve altın dayanıklılığı için, bazı düğümler devre dışı gider ve geri kalanı devre dışı bırakma durumunda olacaktır. Düğümlerin ayrıntılarını devre dışı bırakma durumunda kontrol edin, bunların tümü Altyapı hizmeti bölümleri için yeterli çoğunluk sağlamaya takılıp kalmışsa, devam etmek güvenlidir.

5. Düğüm türü için verileri durdurun.

    PowerShell'i kullanarak kümeye bağlanın ve aşağıdaki adımı çalıştırın.
    
    ```powershell
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Start-ServiceFabricNodeTransition -Stop -OperationId (New-Guid) -NodeInstanceId $node.NodeInstanceId -NodeName $node.NodeName -StopDurationInSeconds 10000
      }
    }
    ```
    
    Düğüm türü için tüm düğümler Aşağı işaretlenene kadar bekleyin.
    
6. Düğüm türü için verileri kaldırın.

    PowerShell'i kullanarak kümeye bağlanın ve aşağıdaki adımı çalıştırın.
    
    ```powershell
    foreach($node in $nodes)
    {
      if ($node.NodeType -eq $nodeType)
      {
        $node.NodeName
     
        Remove-ServiceFabricNodeState -NodeName $node.NodeName -Force
      }
    }
    ```

    Tüm düğümler kümeden kaldırılana kadar bekleyin. Düğümler SFX'te görüntülenmemelidir.

7. Hizmet Kumaşı bölümünden düğüm türünü kaldırın.

    - Dağıtım için kullanılan Azure Kaynak Yöneticisi şablonuna yer bulun.
    - Servis Kumaşı bölümünde düğüm türüyle ilgili bölümü bulun.
    - Düğüm türüne karşılık gelen bölümü kaldırın.
    - Yalnızca Gümüş ve daha yüksek dayanıklılık kümeleri için, şablondaki küme kaynağını güncelleştirin ve aşağıda `applicationDeltaHealthPolicies` verilen küme `properties` kaynağının altına ekleyerek doku:/Sistem uygulama durumunu yok saymak üzere sistem ilkeleri yapılandırın. Aşağıdaki ilke varolan hataları yoksaymalı, ancak yeni sistem durumu hatalarına izin vermemelidir. 
 
 
     ```json
    "upgradeDescription":  
    { 
      "forceRestart": false, 
      "upgradeReplicaSetCheckTimeout": "10675199.02:48:05.4775807", 
      "healthCheckWaitDuration": "00:05:00", 
      "healthCheckStableDuration": "00:05:00", 
      "healthCheckRetryTimeout": "00:45:00", 
      "upgradeTimeout": "12:00:00", 
      "upgradeDomainTimeout": "02:00:00", 
      "healthPolicy": { 
        "maxPercentUnhealthyNodes": 100, 
        "maxPercentUnhealthyApplications": 100 
      }, 
      "deltaHealthPolicy":  
      { 
        "maxPercentDeltaUnhealthyNodes": 0, 
        "maxPercentUpgradeDomainDeltaUnhealthyNodes": 0, 
        "maxPercentDeltaUnhealthyApplications": 0, 
        "applicationDeltaHealthPolicies":  
        { 
            "fabric:/System":  
            { 
                "defaultServiceTypeDeltaHealthPolicy":  
                { 
                        "maxPercentDeltaUnhealthyServices": 0 
                } 
            } 
        } 
      } 
    },
    ```

    - Değiştirilmiş Azure Kaynak Yöneticisi şablonuna dağıtın. ** Bu adım genellikle iki saate kadar, bir süre sürer. Bu yükseltme, ayarları InfrastructureService olarak değiştirir, bu nedenle bir düğüm yeniden başlatma gereklidir. Bu durumda `forceRestart` göz ardı edilir. 
    Parametre, `upgradeReplicaSetCheckTimeout` Hizmet Kumaşı'nın güvenli durumda değilse, bir bölümün güvenli durumda olmasını beklediği maksimum süreyi belirtir. Bir düğümdeki tüm bölümler için güvenlik denetimleri geçtikten sonra, Servis Kumaşı bu düğümdeki yükseltmeyle devam eder.
    Parametrenin `upgradeTimeout` değeri 6 saate düşürülebilir, ancak maksimum güvenlik için 12 saat kullanılmalıdır.

    O zaman bunu doğrulayın:
    - Portalda Servis Kumaş Kaynak hazır gösterir.

8. Düğüm türüyle ilgili kaynaklara yapılan tüm başvuruyu kaldırın.

    - Dağıtım için kullanılan Azure Kaynak Yöneticisi şablonuna yer bulun.
    - Sanal makine ölçeği kümesini ve düğüm türüyle ilgili diğer kaynakları şablondan kaldırın.
    - Değişiklikleri dağıtın.

    Ardından:
    - Dağıtımın tamamlanmasını bekleyin.

## <a name="next-steps"></a>Sonraki adımlar
- Küme dayanıklılık [özellikleri](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-durability-characteristics-of-the-cluster)hakkında daha fazla bilgi edinin.
- [Düğüm türleri ve Sanal Makine Ölçek Kümeleri](service-fabric-cluster-nodetypes.md)hakkında daha fazla bilgi edinin.
- [Service Fabric küme ölçekleme](service-fabric-cluster-scaling.md)hakkında daha fazla bilgi edinin.
