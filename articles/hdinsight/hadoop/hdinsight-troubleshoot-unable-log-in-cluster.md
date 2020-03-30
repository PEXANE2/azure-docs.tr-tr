---
title: Azure HDInsight kümesinde oturum açamıyor
description: Azure HDInsight'ta Neden Apache Hadoop kümesine giriş yapamıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 2099d1b7583017733498946a5866ab37de43ba9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894066"
---
# <a name="scenario-unable-to-log-into-azure-hdinsight-cluster"></a>Senaryo: Azure HDInsight kümesine giriş yapamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight kümesinde oturum açamıyor.

## <a name="cause"></a>Nedeni

Nedenler değişebilir. Kümeveya uygulama panolarında oturum açarken "küme giriş" veya HTTP kimlik bilgilerinizi kullandığınızı unutmayın. Uzaktan bağlanırken Secure Shell (SSH) veya Uzak Masaüstü kimlik bilgilerinizi kullanın.

## <a name="resolution"></a>Çözüm

Sık karşılaşılan sorunları çözmek için aşağıdaki adımlardan birini veya daha fazlasını deneyin.

* Küme panosunu gizlilik modundaki yeni bir tarayıcıda açmayı deneyin.

* SSH kimlik bilgilerinizi geri çağıramıyorsanız, [Ambari UI'deki kimlik bilgilerini](../hdinsight-administer-use-portal-linux.md#change-passwords)sıfırlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
