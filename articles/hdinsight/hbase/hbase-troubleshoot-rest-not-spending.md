---
title: Apache HBase REST, Azure HDInsight'taki isteklere yanıt vermiyor
description: Azure HDInsight'taki isteklere yanıt vermiyor.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 49b547829a369ea6df35e2f1c2f7d60458e41040
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887181"
---
# <a name="scenario-apache-hbase-rest-not-responding-to-requests-in-azure-hdinsight"></a>Senaryo: Apache HBase REST, Azure HDInsight'taki isteklere yanıt vermiyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase REST hizmeti, Azure HDInsight'taki isteklere yanıt vermez.

## <a name="cause"></a>Nedeni

Burada olası nedeni Apache HBase REST hizmeti, özellikle hizmet (örneğin, ay) çalışıyor yaygın soketleri sızıntı olabilir. İstemci SDK'dan aşağıdakilere benzer bir hata iletisi görebilirsiniz:

```
System.Net.WebException : Unable to connect to the remote server --->
System.Net.Sockets.SocketException : A connection attempt failed because the connected party did not properly respond after a period of time, or established connection failed because connected host has failed to respond 10.0.0.19:8090
```

## <a name="resolution"></a>Çözüm

Ana bilgisayara SSHing sonra aşağıdaki komutu kullanarak HBase REST yeniden başlatın. Bu hizmeti tüm alt düğümlerde yeniden başlatmak için komut dosyası eylemlerini de kullanabilirsiniz:

```bash
sudo service hdinsight-hbrest restart
```

Bu komut, aynı ana bilgisayarda HBase Region Server'ı durdurur. Ambari üzerinden HBase Region Server'ı el ile başlatabilir veya Ambari otomatik yeniden başlatma işlevinin HBase Region Server'ı otomatik olarak kurtarmasına izin verebilirsiniz.

Sorun hala devam ediyorsa, aşağıdaki azaltma komut dosyasını her işçi düğümünde her 5 dakikada bir çalışan bir CRON işi olarak yükleyebilirsiniz. Bu azaltma komut dosyası REST hizmetini pingler ve REST hizmeti yanıt vermez durumda yeniden başlatır.

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

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
