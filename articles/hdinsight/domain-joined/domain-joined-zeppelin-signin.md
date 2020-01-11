---
title: Azure HDInsight 'ta Zeppelin 'da oturum açılamıyor
description: Azure HDInsight 'ta Zeppelin 'da oturum açılamıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/12/2019
ms.openlocfilehash: e9a81d458d1bab68bf94e9e9d0ebd87fac4580c8
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75896145"
---
# <a name="scenario-unable-to-sign-in-to-apache-zeppelin-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Zeppelin 'de oturum açılamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Active Directory 'de parola ekleme parolası değiştirildikten sonra Apache Zeppelin 'da oturum açılamıyor.

## <a name="cause"></a>Nedeni

`shiro_ini` dosyasının `activeDirectoryRealm.systemUsername` bahsedilen Kullanıcı Active Directory parolasını değiştirdi.

## <a name="resolution"></a>Çözünürlük

1. `activeDirectoryRealm.systemPassword = <new password>`, ambarı 'nda Zeppelin `shiro_ini` config dosyasına dahil ederek, değiştirilen parolanın kök neden olduğunu doğrulayın. `activeDirectoryRealm.hadoopSecurityCredentialPath` ayarını kaldırın. `shiro ini`konumu aşağıdadır.

    ![Shiro](./media/domain-joined-zeppelin-signin/shiro.png)

1. Kullanıcılar artık 1. adımdan sonra Zeppelin 'da oturum açabilirseniz yeni parolayla yeni bir `jceks` dosyası oluşturun ve `activeDirectoryRealm.hadoopSecurityCredentialPath` yeni dosya ile değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
