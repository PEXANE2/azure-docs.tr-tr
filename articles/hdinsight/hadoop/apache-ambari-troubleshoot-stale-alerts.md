---
title: Azure HDInsight 'ta Apache ambarı eski uyarıları
description: HDInsight 'ta eski Apache ambarı uyarıları için olası nedenleri ve çözümleri tartışma ve çözümleme.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: f19d499b5e50fbb5030a0f396296eed46fc6eee3
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722817"
---
# <a name="scenario-apache-ambari-stale-alerts-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache ambarı eski uyarıları

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ambarı kullanıcı arabiriminden aşağıdaki görüntüye benzer bir uyarı görebilirsiniz:

![Apache ambarı eski uyarı örneği](./media/apache-ambari-troubleshoot-stale-alerts/ambari-stale-alerts-example.png)

## <a name="cause"></a>Nedeni

Ambarı aracıları birçok kaynağın sistem durumunu izlemek için sürekli olarak sistem durumu denetimleri yürütür. Her uyarı önceden tanımlanmış zaman aralıklarıyla çalışacak şekilde yapılandırılır. Her uyarının yürütülmesi tamamlandıktan sonra, ambarı aracıları durumu Ambarı sunucusuna geri bildirir. Bu noktada, ambarı sunucusu uyarılardan birinin zamanında çalıştırılmediğini algılarsa, "bir ambarı sunucu uyarıları" tetikler. Bir sistem durumu denetiminin tanımlı aralıkta yürütülmemesinin çeşitli nedenleri vardır:

* Konaklar ağır kullanım altındayken (yüksek CPU), ambarı aracısının uyarıları zamanında yürütmek için yeterli sistem kaynakları sağlayamadığı bir olasılık vardır.

* Küme, ağır yük sırasında çok sayıda işi/hizmeti yürütmekle meşgul.

* Kümedeki birkaç konak birçok bileşeni barındırabilir ve bu nedenle birçok uyarıyı çalıştırmak gerekecektir. Bileşen sayısı büyükse, uyarı işlerinin zamanlanan aralıklarını kaçırabileceği olasıdır

## <a name="resolution"></a>Çözüm

### <a name="increase-alert-interval-time"></a>Uyarı aralığı süresini artır

Tek bir uyarı aralığının değerini, kümenizin yanıt süresine ve yüküne göre artırmayı tercih edebilirsiniz.

1. Apache ambarı kullanıcı arabiriminden **Uyarılar** sekmesini seçin.
1. İstenen uyarı tanımı adını seçin.
1. Tanımdan **Düzenle**' yi seçin.
1. **Denetim aralığı** değerini istediğiniz şekilde değiştirin ve ardından **Kaydet**' i seçin.

### <a name="increase-alert-interval-time-for-ambari-server-alerts"></a>Ambarı sunucu uyarıları için uyarı aralığı süresini artırma

1. Apache ambarı kullanıcı arabiriminden **Uyarılar** sekmesini seçin.
1. **Gruplar** açılan listesinden, **ambarı varsayılanı**' nı seçin.
1. Uyarı **ambarı sunucu uyarıları**' nı seçin.
1. Tanımdan **Düzenle**' yi seçin.
1. **Denetim aralığı** değerini istediğiniz şekilde değiştirin.
1. **Aralık çarpanı** değerini istediğiniz şekilde değiştirin ve ardından **Kaydet**' i seçin.

### <a name="disable-and-enable-the-alert"></a>Uyarıyı devre dışı bırakma ve etkinleştirme

Eski uyarıları atmak için uyarıyı devre dışı bırakıp daha sonra yeniden etkinleştirebilirsiniz.

1. Apache ambarı kullanıcı arabiriminden **Uyarılar** sekmesini seçin.
1. İstenen uyarı tanımı adını seçin.
1. Tanımdan, en sağdaki **etkin** ' i seçin.
1. **Onay** açılır listesinden **devre dışı bırak**' ı seçin.
1. Sayfada gösterilen tüm uyarılar için birkaç saniye bekleyin.
1. Tanımdan, en sağdaki **devre dışı** seçeneğini belirleyin.
1. **Onay** açılır listesinden **Etkinleştir**' i seçin.

### <a name="increase-alert-grace-time"></a>Uyarı yetkisiz kullanım süresini artır

Ambarı Aracısı yapılandırılmış bir uyarının zamanlamasını kaçırdığını raporladığında, bir yetkisiz kullanım süresi uygulanır. Uyarı, zamanlanan süresini kaçırsa ancak uyarı yetkisiz kullanım süresi içinde tetiklense bile eski uyarı tetiklenmez.

Varsayılan `alert_grace_period` değeri 5 saniyedir. Bu `alert_grace_period` ayarı `/etc/ambari-agent/conf/ambari-agent.ini`yapılandırılabilir. Eski uyarıların düzenli aralıklarla tetiklenme ana bilgisayarlar için 10 değerini artırmayı deneyin. Ardından, ambarı aracısını yeniden başlatın

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
