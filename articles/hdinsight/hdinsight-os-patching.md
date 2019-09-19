---
title: Linux tabanlı HDInsight kümeleri için işletim sistemi düzeltme eki uygulama zamanlamasını Yapılandırma-Azure
description: Linux tabanlı HDInsight kümeleri için işletim sistemi düzeltme eki uygulama zamanlamasını yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/01/2019
ms.openlocfilehash: 06111ec35a127cf17fdcc77ff717de7a4bc7299f
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71076852"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Linux tabanlı HDInsight kümeleri için işletim sistemi düzeltme eki uygulama zamanlamasını yapılandırma 

> [!IMPORTANT]
> Ubuntu görüntüleri, yayımlanmakta olan üç ay içinde yeni Azure HDInsight kümesi oluşturma için kullanılabilir hale gelir. 2019 Ocak itibariyle çalıştırılan kümeler otomatik düzeltme eki olmaz. Müşterilerin çalışan bir kümeyi yaması için betik eylemleri veya diğer mekanizmaları kullanması gerekir. Yeni oluşturulan kümeler en son güvenlik düzeltme ekleri dahil olmak üzere her zaman en son kullanılabilir güncelleştirmelere sahip olur.

Bazen önemli güvenlik düzeltme eklerini yüklemek için bir HDInsight kümesinde sanal makineleri (VM) yeniden başlatmanız gerekir.

Bu makalede açıklanan betik eylemlerini kullanarak, işletim sistemi düzeltme eki uygulama zamanlamasını aşağıdaki şekilde değiştirebilirsiniz:

1. Tüm güncelleştirmeleri yükler veya yalnızca çekirdek + güvenlik güncelleştirmelerini veya çekirdek güncelleştirmelerini yükler.
2. Anında yeniden başlatma yapın veya VM 'de yeniden başlatma zamanlayın.

> [!NOTE]  
> Bu makalede açıklanan betik eylemleri yalnızca 1 Ağustos 2016 ' den sonra oluşturulan Linux tabanlı HDInsight kümeleriyle çalışır. Düzeltme ekleri yalnızca VM 'Ler yeniden başlatıldıktan sonra geçerlidir.
> Betik eylemleri, gelecekteki tüm güncelleştirme döngüleri için güncelleştirmeleri otomatik olarak uygulamaz. Güncelleştirmeleri yüklemek için yeni güncelleştirmelerin uygulanması gereken her seferinde komut dosyalarını çalıştırın ve ardından sanal makineyi yeniden başlatın.

## <a name="add-information-to-the-script"></a>Betiğe bilgi ekleme

Komut dosyası kullanmak aşağıdaki bilgileri gerektirir:

- Install-Updates-Schedule-yeniden başlatmalar betik konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh.
    
   HDInsight bu URI 'yi kümedeki tüm VM 'lerde betiği bulmak ve çalıştırmak için kullanır. Bu betik, güncelleştirmeleri yüklemek ve VM 'yi yeniden başlatmak için seçenekler sağlar.
  
- Zamanlama-yeniden başlatmalar betiği konumu: https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh.
    
   HDInsight bu URI 'yi kümedeki tüm VM 'lerde betiği bulmak ve çalıştırmak için kullanır. Bu betik VM 'yi yeniden başlatır.
  
- Betiğin uygulandığı küme düğümü türleri; headnode, workernode ve Zookeeper. Betiği kümedeki tüm düğüm türlerine uygulayın. Betik bir düğüm türüne uygulanmadıysa, bu düğüm türü için VM 'Ler güncellenmez veya yeniden başlatılmaz.

- Install-Updates-Schedule-yeniden başlatmalar betiği iki sayısal parametreyi kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Yalnızca çekirdek güncelleştirmelerini yükler/tüm güncelleştirmeleri yükler/yalnızca çekirdek + güvenlik güncelleştirmelerini yükler|0, 1 veya 2. 0 değeri yalnızca çekirdek güncelleştirmelerini yüklüyor. 1 değeri tüm güncelleştirmeleri yüklüyor ve 2 yalnızca çekirdek + güvenlik güncelleştirmelerini yüklüyor. Hiçbir parametre sağlanmazsa varsayılan değer 0 ' dır. |
    | Yeniden başlatma/etkinleştirme zamanlamasını başlatma/hemen yeniden başlatmaya Izin verme |0, 1 veya 2. 0 değeri yeniden başlatmayı devre dışı bırakır. 1 değeri, zamanlamayı yeniden başlatmaya izin verebilir ve 2 hemen yeniden başlatmaya izin vermez. Hiçbir parametre sağlanmazsa varsayılan değer 0 ' dır. Kullanıcı giriş parametresi 1 ' i giriş parametresi 2 ' ye değiştirmesi gerekir. |
   
 - Zamanlama-yeniden başlatmalar betiği bir sayısal parametreyi kabul eder:

    | Parametre | Tanım |
    | --- | --- |
    | Yeniden başlatma zamanlamasını etkinleştir/hemen yeniden başlatmayı etkinleştir |1 veya 2. 1 değeri, zamanlamayı yeniden başlatmayı (12-24 saat içinde zamanlanmış) mümkün. 2 değeri hemen yeniden başlatmaya (5 dakika içinde) izin vermez. Hiçbir parametre verilmezse varsayılan değer 1 ' dir. |  

> [!NOTE]
> Mevcut bir kümeye uyguladıktan sonra bir betiği kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçeklendirme işlemleri aracılığıyla oluşturulan tüm yeni düğümler varsayılan düzeltme eki uygulama zamanlamasını kullanır. Betiği küme oluşturma işleminin parçası olarak uygularsanız, otomatik olarak kalıcı hale getirilir.


## <a name="next-steps"></a>Sonraki adımlar

Betik eylemlerini kullanmayla ilgili belirli adımlar için, [betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md)bölümünde yer alan aşağıdaki bölümlere bakın:

* [Küme oluşturma sırasında betik eylemi kullanma](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation)
* [Çalışan bir kümeye betik eylemi uygulama](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster)
