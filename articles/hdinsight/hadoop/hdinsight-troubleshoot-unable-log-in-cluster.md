---
title: Azure HDInsight kümesinde oturum açılamıyor
description: Azure HDInsight 'ta neden Apache Hadoop kümesinde oturum geçemedik sorunlarını giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 059fee89fdb7f0bbc046c98e36c6386d06653fff
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540423"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Senaryo: Azure HDInsight kümesinde oturum açılamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight kümesinde oturum açılamıyor.

## <a name="cause"></a>Nedeni

Nedenler farklılık gösterebilir. Küme veya uygulama panolarında oturum açarken, "küme oturum açma" veya HTTP kimlik bilgilerinizi kullanmayı unutmayın. Uzaktan bağlanırken Secure Shell (SSH) veya Uzak Masaüstü kimlik bilgilerinizi kullanın.

## <a name="resolution"></a>Çözüm

Sık karşılaşılan sorunları çözmek için aşağıdaki adımlardan birini veya daha fazlasını deneyin.

* Küme panosunu gizlilik modundaki yeni bir tarayıcıda açmayı deneyin.

* SSH kimlik bilgilerinizi hatırlayamıyorsanız, [ambarı Kullanıcı arabirimindeki kimlik bilgilerini sıfırlayabilirsiniz](../hdinsight-administer-use-portal-linux.md#change-passwords).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.