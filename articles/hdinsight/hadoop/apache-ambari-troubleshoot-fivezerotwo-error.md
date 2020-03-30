---
title: Azure HDInsight'ta Apache Ambari UI 502 hatası
description: Azure HDInsight kümenize erişmeye çalıştığınızda Apache Ambari UI 502 hatası
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: 2b17c2488e47148e8845433f9c7613e1127fbffa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895750"
---
# <a name="scenario-apache-ambari-ui-502-error-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Ambari UI 502 hatası

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

HDInsight kümeniz için Apache Ambari UI'ye erişmeye çalıştığınızda, "502 - Web sunucusu ağ geçidi veya proxy sunucusu gibi hareket ederken geçersiz bir yanıt alır" gibi bir ileti alırsınız.

## <a name="cause"></a>Nedeni

Genel olarak, HTTP 502 durum kodu Ambari sunucusunun etkin başlıküzerinde doğru çalışmadığı anlamına gelir. Birkaç olası kök nedenleri vardır.

## <a name="resolution"></a>Çözüm

Çoğu durumda, sorunu azaltmak için etkin başlık yeniden başlatabilirsiniz. Veya headnode için daha büyük bir VM boyutu seçin.

### <a name="ambari-server-failed-to-start"></a>Ambari sunucusu başlatılamamış

Ambari sunucusunun neden başlatılamadığını öğrenmek için ambari-sunucu günlüklerini kontrol edebilirsiniz. Sık karşılaşılan bir neden veritabanı tutarlılık denetimi hatasıdır. Bunu bu günlük dosyasında bulabilirsiniz: `/var/log/ambari-server/ambari-server-check-database.log`.

Küme düğümünde herhangi bir değişiklik yaptıysanız, lütfen bunları geri getirin. Hadoop/Spark ile ilgili yapılandırmaları değiştirmek için Ambari UI'yi her zaman kullanın.

### <a name="ambari-server-taking-100-cpu-utilization"></a>Ambari sunucu% 100 CPU kullanımı alarak

Nadir durumlarda, ambari-sunucu süreci sürekli% 100 CPU kullanımı yakın olduğunu gördük. Bir azaltma olarak, aktif headnode ssh ve Ambari sunucu işlemini öldürmek ve yeniden başlatabilirsiniz.

```bash
ps -ef | grep AmbariServer
top -p <ambari-server-pid>
kill -9 <ambari-server-pid>
service ambari-server start
```

### <a name="ambari-server-killed-by-oom-killer"></a>Ambari sunucusu oom-katil tarafından öldürüldü

Bazı senaryolarda, kafa düğümünün hafızası biter ve Linux oom-killer öldürme süreçlerini seçmeye başlar. Bu durumu, bulunamaması gereken AmbariServer işlem kimliğini arayarak doğrulayabilirsiniz. O zaman `/var/log/syslog`senin, bakmak ve böyle bir şey arayın:

```
Jul 27 15:29:30 xxx-xxxxxx kernel: [874192.703153] java invoked oom-killer: gfp_mask=0x23201ca, order=0, oom_score_adj=0
```

Sonra hangi süreçlerin anılar alıyor belirlemek ve daha kök neden deneyin.

### <a name="other-issues-with-ambari-server"></a>Ambari sunucusu ile ilgili diğer sorunlar

Nadiren Ambari sunucusu gelen isteği işleyemez, herhangi bir hata için ambari-sunucu günlükleri bakarak daha fazla bilgi bulabilirsiniz. Böyle bir durumda bu gibi bir hatadır:

```
Error Processing URI: /api/v1/clusters/xxxxxx/host_components - (java.lang.OutOfMemoryError) Java heap space
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
