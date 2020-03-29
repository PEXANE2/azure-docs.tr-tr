---
title: Azure Site Kurtarma izleme ile ilgili sık sorulan sorular
description: Dahili izleme ve Azure Monitörünü (Log Analytics) kullanarak Azure Site Kurtarma izleme yle ilgili sık sorulan soruların yanıtlarını alın
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 07/31/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: c1d30a9cdd2cd6ca288edd609a2e2e7bee9174d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68718268"
---
# <a name="common-questions-about-site-recovery-monitoring"></a>Site Kurtarma izleme hakkında sık sorulan sorular

Bu makalede, dahili Site Kurtarma izleme ve Azure Monitörü (Log Analytics) kullanarak Azure [Site Kurtarma](site-recovery-overview.md)izleme ile ilgili sık sorulan soruları yanıtlar.

## <a name="general"></a>Genel

### <a name="how-is-the-rpo-value-logged-different-from-the-latest-available-recovery-point"></a>RPO değeri en son kullanılabilir kurtarma noktasından nasıl farklı günlüğe kaydedilir?

Site Kurtarma, makineleri Azure'a çoğaltmak için çok aşamalı, eşzamanlı bir işlem kullanır.

- Çoğaltmanın sondan bir önceki adımında, meta verilerle birlikte makinedeki son değişiklikler bir günlük/önbellek depolama hesabına kopyalanır.
- Bu değişiklikler, kurtarılabilir bir noktayı tanımlayan etiketle birlikte, hedef bölgedeki depolama hesabına/yönetilen diske yazılır.
- Site Kurtarma artık makine için kurtarılabilir bir nokta oluşturabilir.
- Bu noktada, RPO şimdiye kadar depolama hesabına yüklenen değişiklikler için karşılandı. Başka bir deyişle, bu noktadamakine RPO kurtarılabilir noktaya karşılık gelen zaman damgası geçen süre miktarına eşittir.
- Şimdi, Site Kurtarma depolama hesabından yüklenen verileri seçer ve makine için oluşturulan çoğaltma disklerine uygular.
- Site Kurtarma sonra bir kurtarma noktası oluşturur ve bu noktada başarısız kurtarma için kullanılabilir hale getirir.
- Bu nedenle, kullanılabilir en son kurtarma noktası, zaten işlenmiş ve yineleme disklerine uygulanan en son kurtarma noktasına karşılık gelen zaman damgasını gösterir.


Çoğaltma kaynak makinesinde veya şirket içi altyapı sunucularında yanlış bir sistem süresi, hesaplanan RPO değerini çarpıtacaktır. Doğru RPO raporlaması için, sistem saatinin tüm sunucularda ve makinelerde doğru olduğundan emin olun.



## <a name="inbuilt-site-recovery-logging"></a>Dahili Site Kurtarma günlüğü


### <a name="why-is-the-vm-count-in-the-vault-infrastructure-view-different-from-the-total-count-shown-in-replicated-items"></a>Kasa altyapısı görünümündeki VM sayısı, Çoğaltılan Öğeler'de gösterilen toplam sayımdan neden farklıdır?

Kasa altyapısı görünümü çoğaltma senaryoları tarafından kapsama. Yalnızca seçili çoğaltma senaryosundaki makineler görünüm sayısına dahil edilir. Ayrıca, yalnızca Azure'a çoğaltmak üzere yapılandırılan VM'leri sayarız. Makineler üzerinde başarısız olan veya şirket içi bir siteye geri döndüren makineler görünümde sayılmaz.

### <a name="why-is-the-count-of-replicated-items-in-essentials-different-from-the-total-count-of-replicated-items-on-the-dashboard"></a>Temel öğelerde çoğaltılan öğelerin sayısı, panodaki çoğaltılan öğelerin toplam sayısından neden farklıdır?

Yalnızca ilk çoğaltmanın tamamlandığı makineler Essentials'ta gösterilen sayıma dahil edilir. Çoğaltılan öğeler toplamı, ilk çoğaltmanın şu anda devam etmekte olduğu makineler de dahil olmak üzere kasadaki tüm makineleri içerir.

## <a name="azure-monitor-logging"></a>Azure İzleyici günlükleri


### <a name="how-often-does-site-recovery-send-diagnostic-logs-to-azure-monitor-log"></a>Site Kurtarma, tanılama günlüklerini Azure Monitor Günlüğü'ne ne sıklıkta gönderir? 

- AzureSiteRecoveryReplicationİstatistiks ve AzureSiteRecoveryRecoveryPoints her 15 dakikada gönderilir.  
- AzureSiteRecoveryReplicationDataUploadRate ve AzureSiteRecoveryProtectedDiskDataChurn her beş dakikada bir gönderilir. 
- AzureSiteRecoveryJobs, bir işin tetikleyicisi ve tamamlanması sırasında gönderilir.
- AzureSiteRecoveryEvents bir olay oluşturulduğunda gönderilir. 
- AzureSiteRecoveryReplicatedItems herhangi bir ortam değişikliği olduğunda gönderilir. Genellikle, veri yenileme süresi bir değişiklikten 15 dakika sonradır. 

### <a name="how-long-is-data-kept-in-azure-monitor-logs"></a>Veriler Azure Monitor günlüklerinde ne kadar süreyle saklanır? 

Varsayılan olarak, bekletme 31 gün sürer. Log Analytics çalışma alanında **Kullanım ve Tahmini Maliyet** bölümündeki dönemi artırabilirsiniz. Veri **Saklama'ya**tıklayın ve aralığı seçin.

### <a name="whats-the-size-of-the-diagnostic-logs"></a>Teşhis kayıtlarının boyutu nedir? 

Genellikle bir günlük boyutu 15-20 KB olduğunu. 


## <a name="next-steps"></a>Sonraki adımlar

[Site Kurtarma dahili izleme](site-recovery-monitor-and-troubleshoot.md)veya Azure [Monitörü](monitor-log-analytics.md)ile nasıl izleyeceğinizi öğrenin.


