---
title: BindException-adres Azure HDInsight 'ta zaten kullanılıyor
description: BindException-adres Azure HDInsight 'ta zaten kullanılıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/08/2019
ms.openlocfilehash: 8851a4dfb7deafab7ad77ef80619dd49ca46ed71
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947857"
---
# <a name="scenario-bindexception---address-already-in-use-in-azure-hdinsight"></a>Senaryo: BindException-adres Azure HDInsight 'ta zaten kullanılıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase bölge sunucusu üzerinde yeniden başlatma işlemi tamamlanamadı. Bölge sunucusunun başlayacağı `/var/log/hbase` çalışan düğümlerinde bulunandizininde,aşağıdakinebenzerbirhatailetisigörebilirsiniz:`region-server.log`

```
Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
...

Caused by: java.net.BindException: Address already in use
...
```

## <a name="cause"></a>Nedeni

Ağır iş yükü etkinliği sırasında HBase bölge sunucuları yeniden başlatılıyor. Bir Kullanıcı, HBase bölge sunucusunda, ambarı kullanıcı arabiriminden yeniden başlatma işlemini başlattığında, arka planda bu durum aşağıda verilmiştir:

1. Ambarı Aracısı, bölge sunucusuna bir durdurma isteği gönderir.

1. Daha sonra, ambarı Aracısı bölge sunucusunun düzgün şekilde kapanması için 30 saniye bekler.

1. Uygulama bölge sunucusuna bağlanmaya devam ederse, bu, hemen kapanmaz ve 30 saniyelik zaman aşımı süresi daha önce sona erer.

1. 30 saniyelik süre dolduktan sonra, ambarı Aracısı bölge sunucusuna bir zorla KILL (Kill-9) gönderir.

1. Bu ani kapatmayla kapatılmış olsa da, bölge sunucusu işlemi sonlandırısa da işlemle ilişkili bağlantı noktası yayımlanmayabilir, sonunda buna yol açar `AddressBindException`.

## <a name="resolution"></a>Çözüm

Yeniden başlatma başlatmadan önce HBase bölge sunucularındaki yükü azaltın.

Alternatif olarak, aşağıdaki komutları kullanarak çalışan düğümlerinde bölge sunucularını deneyin ve el ile yeniden başlatın:

```bash
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
