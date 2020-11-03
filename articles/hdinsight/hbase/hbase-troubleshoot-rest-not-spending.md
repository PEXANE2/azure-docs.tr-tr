---
title: Apache HBase REST, Azure HDInsight 'taki isteklere yanıt vermiyor
description: Azure HDInsight 'taki isteklere yanıt vermeyen Apache HBase REST ile ilgili sorunu çözün.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: c4a0ef82b951fa43eb4c58050d3148fd2d695026
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93286977"
---
# <a name="scenario-apache-hbase-rest-not-responding-to-requests-in-azure-hdinsight"></a>Senaryo: Apache HBase REST, Azure HDInsight 'taki isteklere yanıt vermiyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase REST hizmeti, Azure HDInsight 'taki isteklere yanıt vermez.

## <a name="cause"></a>Nedeni

Buradaki olası neden Apache HBase REST hizmeti, hizmet uzun bir süre (örneğin, aylar) üzerinde çalışırken yaygın olan yuvalara sızıyor olabilir. İstemci SDK 'Sı ' nden şuna benzer bir hata iletisi görebilirsiniz:

```
System.Net.WebException : Unable to connect to the remote server --->
System.Net.Sockets.SocketException : A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond 10.0.0.19:8090
```

## <a name="resolution"></a>Çözüm

Ana bilgisayara Ssholduktan sonra aşağıdaki komutu kullanarak HBase REST 'i yeniden başlatın. Bu hizmeti tüm çalışan düğümlerinde yeniden başlatmak için betik eylemlerini de kullanabilirsiniz:

```bash
sudo service hdinsight-hbrest restart
```

Bu komut, aynı konaktaki HBase bölge sunucusunu durdurur. HBase bölge sunucusunu ambarı aracılığıyla el ile başlatabilir veya ambarı otomatik yeniden başlatma işlevselliğinin HBase bölge sunucusunu otomatik olarak kurtarmasına izin verebilirsiniz.

Sorun devam ederse, aşağıdaki risk azaltma betiğini her çalışan düğümünde 5 dakikada bir çalışan bir CRON işi olarak yükleyebilirsiniz. Bu risk azaltma betiği REST hizmetine ping atar ve REST hizmetinin yanıt vermediği durumlarda yeniden başlatır.

```bash
#!/bin/bash
nc localhost 8090 < /dev/null
if [ $? -ne 0 ]
    then
    echo "RESTServer is not responding. Restarting"
    sudo /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh restart rest
fi
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]