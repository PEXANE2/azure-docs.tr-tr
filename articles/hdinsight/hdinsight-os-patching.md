---
title: Azure HDInsight kümeleri için işletim sistemi yama zamanlamasını yapılandırma
description: Linux tabanlı HDInsight kümeleri için işletim sistemi yama zamanlamasını nasıl yapılandırılabildiğini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/21/2020
ms.openlocfilehash: f8e694f658d6e9de04c92001214ecd5c32ff7753
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206869"
---
# <a name="configure-the-os-patching-schedule-for-linux-based-hdinsight-clusters"></a>Linux tabanlı HDInsight kümeleri için işletim sistemi yama zamanlamasını yapılandırın

> [!IMPORTANT]
> Ubuntu görüntüleri, yayımlandıktan sonraki üç ay içinde yeni Azure HDInsight küme oluşturma için kullanılabilir hale gelir. Ocak 2019 itibariyle, çalışan kümeler otomatik yamalı değildir. Müşteriler çalışan bir kümeyi yamalamak için komut dosyası eylemlerini veya diğer mekanizmaları kullanmalıdır. Yeni oluşturulan kümeler, en son güvenlik düzeltme eki de dahil olmak üzere her zaman kullanılabilir en son güncelleştirmelere sahip olur.

HDInsight, işletim sistemi yamaları, güvenlik güncelleştirmeleri ve yeniden başlatma düğümleri yükleme gibi kümenizde ortak görevleri gerçekleştirmeniz için destek sağlar. Bu görevler, [komut dosyası eylemleri](hdinsight-hadoop-customize-cluster-linux.md)olarak çalıştırılabilen ve parametrelerle yapılandırılabilen aşağıdaki iki komut dosyası kullanılarak gerçekleştirilir:

- `schedule-reboots.sh`- Hemen yeniden başlatın veya küme düğümlerinde yeniden başlatma zamanlayın.
- `install-updates-schedule-reboots.sh`- Tüm güncelleştirmeleri, yalnızca çekirdek + güvenlik güncelleştirmelerini veya yalnızca çekirdek güncelleştirmelerini yükleyin.

> [!NOTE]  
> Komut dosyası eylemleri, gelecekteki tüm güncelleştirme döngüleri için güncelleştirmeleri otomatik olarak uygulamaz. Güncelleştirmeleri yüklemek için her yeni güncelleştirmenin uygulanması gerektiğinde komut dosyalarını çalıştırın ve ardından VM'yi yeniden başlatın.

## <a name="preparation"></a>Hazırlık

Üretime dağıtımdan önce temsili bir üretim dışı ortama yama. Gerçek düzeltme önce sisteminizi yeterince test etmek için bir plan geliştirin.

Zaman zaman, kümeile bir ssh oturumundan, bir yükseltme kullanılabilir bir ileti alabilirsiniz. İleti aşağıdaki gibi görünebilir:

```
New release '18.04.3 LTS' available.
Run 'do-release-upgrade' to upgrade it
```

Yama isteğe bağlıdır ve sizin takdirinize bağlıdır.

## <a name="restart-nodes"></a>Düğümleri yeniden başlat
  
Komut dosyası [zamanlaması yeniden başlatma,](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/schedule-reboots.sh)kümedeki makinelerde gerçekleştirilecek yeniden başlatma türünü ayarlar. Komut dosyası eylemini gönderirken, üç düğüm türüne de uygulanacak şekilde ayarlayın: baş düğümü, işçi düğümü ve zookeeper. Komut dosyası bir düğüm türüne uygulanmazsa, bu düğüm türüne ait VM'ler güncelleştirilmez veya yeniden başlatılamayacaktır.

Bir `schedule-reboots script` sayısal parametre kabul eder:

| Parametre | Kabul edilen değerler | Tanım |
| --- | --- | --- |
| Gerçekleştirmek için yeniden başlatma türü | 1 veya 2 | 1 değeri zamanlamayeniden (12-24 saat içinde zamanlanan) sağlar. 2 değeri hemen yeniden başlatmayı sağlar (5 dakika içinde). Parametre verilmezse, varsayılan değer 1'dir. |  

## <a name="install-updates-and-restart-nodes"></a>Güncelleştirmeleri yükleme ve düğümleri yeniden başlatma

Komut dosyası [install-updates-schedule-reboots.sh,](https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/install-updates-schedule-reboots.sh) farklı güncelleştirme türlerini yüklemek ve VM'yi yeniden başlatmak için seçenekler sunar.

Komut `install-updates-schedule-reboots` dosyası, aşağıdaki tabloda açıklandığı gibi iki sayısal parametre kabul eder:

| Parametre | Kabul edilen değerler | Tanım |
| --- | --- | --- |
| Yüklenmesi gereken güncelleştirme türü | 0, 1 veya 2 | 0 değeri yalnızca çekirdek güncelleştirmelerini yükler. 1 değeri tüm güncelleştirmeleri yükler ve 2 yalnızca çekirdek + güvenlik güncelleştirmelerini yükler. Parametre sağlanmadıysa, varsayılan değer 0'dır. |
| Gerçekleştirmek için yeniden başlatma türü | 0, 1 veya 2 | 0 devre dışı kaldırma değeri yeniden başlatılır. 1 değeri zamanlamanın yeniden başlatılmasını ve 2'nin hemen yeniden başlatılmasını sağlar. Parametre sağlanmadıysa, varsayılan değer 0'dır. Kullanıcı giriş parametresi 1'i giriş parametresi 2 olarak değiştirmelidir. |

> [!NOTE]
> Bir komut dosyasını varolan bir kümeye uyguladıktan sonra kalıcı olarak işaretlemeniz gerekir. Aksi takdirde, ölçekleme işlemleri yoluyla oluşturulan tüm yeni düğümler varsayılan düzeltme çizelgesini kullanır. Komut dosyasını küme oluşturma işleminin bir parçası olarak uygularsanız, komut dosyası otomatik olarak devam eder.

## <a name="next-steps"></a>Sonraki adımlar

Komut dosyası eylemlerini kullanmayla ilgili belirli adımlar için, [komut dosyası eylemini kullanarak Linux tabanlı HDInsight kümelerini özelleştir'deki](hdinsight-hadoop-customize-cluster-linux.md)aşağıdaki bölümlere bakın:

- [Küme oluşturma sırasında komut dosyası eylemi kullanma](hdinsight-hadoop-customize-cluster-linux.md#script-action-during-cluster-creation)
- [Çalışan kümeye komut dosyası eylemi uygulama](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster)
