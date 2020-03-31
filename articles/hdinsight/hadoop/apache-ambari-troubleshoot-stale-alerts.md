---
title: Azure HDInsight'ta Apache Ambari bayat uyarıları
description: HDInsight'ta Apache Ambari bayat uyarılarıiçin olası nedenlerin ve çözümlerin tartışılması ve analizi.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: f9dfcb930e3fe4f862f9f51ff00270d0eb0c66ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77539119"
---
# <a name="scenario-apache-ambari-stale-alerts-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Ambari bayat uyarıları

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Ambari UI'de aşağıdaki gibi bir uyarı görebilirsiniz:

![Apache Ambari bayat uyarı örneği](./media/apache-ambari-troubleshoot-stale-alerts/ambari-stale-alerts-example.png)

## <a name="cause"></a>Nedeni

Ambari ajanları sürekli olarak birçok kaynağın sağlığını izlerler. *Uyarılar,* belirli küme özelliklerinin önceden belirlenmiş eşikler içinde olup olmadığını bildirmek üzere yapılandırılabilir. Her kaynak denetimi çalıştırdıktan sonra, uyarı koşulu karşılanırsa, Ambari aracıları durumu Ambari sunucusuna bildirir ve bir uyarı tetikler. Uyarı Profilindeki arayla göre bir uyarı işaretlenmezse, sunucu *ambari Server Bayat Uyarıları* uyarısını tetikler.

Sistem durumu denetiminin tanımlanan aralıkta çalışmaması için çeşitli nedenler vardır:

* Ambari aracısının uyarıları zamanında çalıştırmak için yeterli sistem kaynaklarını alayaması için ana bilgisayarlar yoğun kullanım altındadır (yüksek CPU kullanımı).

* Küme, ağır yük süresi boyunca birçok iş veya hizmeti yürütmekle meşgul.

* Kümedeki az sayıda ana bilgisayar birçok bileşeni barındırıyor ve bu nedenle birçok uyarıyı çalıştırmak için gereklidir. Bileşen sayısı büyükse, uyarı işleri zamanlanan aralıklarını kaçırabilir.

## <a name="resolution"></a>Çözüm

Ambari bayat uyarıları ile sorunları çözmek için aşağıdaki yöntemleri deneyin.

### <a name="increase-the-alert-interval-time"></a>Uyarı aralığı süresini artırma

Kümenizin yanıt süresine ve yüküne bağlı olarak tek bir uyarı aralığının değerini artırabilirsiniz:

1. Apache Ambari UI'de **Uyarılar** sekmesini seçin.
1. İstediğiniz uyarı tanım adını seçin.
1. Tanımdan, **Edit'i**seçin.
1. Denetim **Aralığı** değerini artırın ve sonra **Kaydet'i**seçin.

### <a name="increase-the-alert-interval-time-for-ambari-server-alerts"></a>Ambari Sunucu Uyarıları için uyarı aralığı süresini artırın

1. Apache Ambari UI'de **Uyarılar** sekmesini seçin.
1. **Gruplar** açılır listesinden **AMBARI Varsayılan'ı**seçin.
1. **Ambari Server Uyarıları** uyarısını seçin.
1. Tanımdan, **Edit'i**seçin.
1. Denetim **Aralığı** değerini artırın.
1. Aralık **Çarpanı** değerini artırın ve sonra **Kaydet'i**seçin.

### <a name="disable-and-reenable-the-alert"></a>Uyarıyı devre dışı bırakıp yeniden etkinleştirin

Eski bir uyarıyı atmak için devre dışı bırakıp yeniden etkinleştirin:

1. Apache Ambari UI'de **Uyarılar** sekmesini seçin.
1. İstediğiniz uyarı tanım adını seçin.
1. Tanımdan, UI'nin en sağ tarafında **Etkin'i** seçin.
1. **Onay** açılır penceresinde, Devre **Dışı Nı**Onayla'yı seçin.
1. Sayfada gösterilen tüm uyarı "örneklerinin" temizlenmesi için birkaç saniye bekleyin.
1. Tanımdan, UI'nin en sağ tarafında **Devre Dışı'yı** seçin.
1. **Onay** açılır penceresinde, **Etkinleştir'i onayla'yı**seçin.

### <a name="increase-the-alert-grace-period"></a>Uyarı yetkisiz kullanım süresini artırma

Ambari ajanı, yapılandırılmış bir uyarının programını kaçırdığını bildirmeden önce bir yetkisiz kullanım süresi vardır. Uyarı zamanlanan saatini kaçırdıysa ancak yetkisiz kullanım süresi içinde çalıştırılırsa, eski uyarı oluşturulmaz.

Varsayılan `alert_grace_period` değer 5 saniyedir. Bu ayarı /etc/ambari-agent/conf/ambari-agent.ini'de yapılandırabilirsiniz. Eski uyarıların düzenli aralıklarla gerçekleştiği ana bilgisayarlar için değeri 10'a artırmayı deneyin. O zaman Ambari ajanını yeniden başlat.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuz burada belirtilmediyse veya çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği'ndeki](https://azure.microsoft.com/support/community/)Azure uzmanlarından yanıtlar alın.

* Twitter'da bağlantı [@AzureSupport](https://twitter.com/azuresupport) kurun. Bu, müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabıdır. Azure topluluğunu doğru kaynaklara bağlar: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderin. Oraya ulaşmak için portal menüsünden Yardım (**?**) seçeneğini belirleyin veya **Yardım + destek** bölmesini açın. Daha fazla bilgi için azure [destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)bilgisini öğrenin. 

  Abonelik yönetimi ve faturalandırma desteği, Microsoft Azure aboneliğinize dahildir. [Azure Destek Planları](https://azure.microsoft.com/support/plans/)aracılığıyla teknik destek kullanılabilir.
