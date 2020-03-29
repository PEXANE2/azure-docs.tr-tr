---
title: Azure HDInsight'taki DomainNotFound hatasıyla küme oluşturma başarısız olur
description: Azure HDInsight kümeleri ile etkileşimde çalışırken sorunlar için sorun giderme adımları ve olası çözümler
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: adcdafbc07fa0a8cc6970ab227b52aee798b084f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76776241"
---
# <a name="scenario-cluster-creation-fails-with-domainnotfound-error-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'taki DomainNotFound hatasıyla küme oluşturma başarısız olur

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

HDI Secure (Kurumsal Güvenlik Paketi) `DomainNotFound` küme oluşturma hata iletisi ile başarısız olur.

## <a name="cause"></a>Nedeni

Yanlış DNS ayarları.

## <a name="resolution"></a>Çözüm

Etki alanı birleştirilmiş kümeler dağıtıldığında, HDI AAD DS'de (her küme için) bir iç kullanıcı adı ve parola oluşturur ve bu etki alanına yönelik tüm küme düğümlerini birleştirir. Etki alanı birleştirme Samba araçları kullanılarak gerçekleştirilir. Aşağıdaki ön koşulların karşılandığından emin olun:

* Etki alanı adı DNS üzerinden çözülmelidir.
* Etki alanı denetleyicilerinin IP adresi, kümenin dağıtıldığı sanal ağ için DNS ayarlarında ayarlanmalıdır.
* Sanal ağ AAD DS sanal ağ ile bakıldı ise, o zaman el ile yapılması gerekir.
* DNS iletmelerini kullanıyorsanız, etki alanı adının sanal ağ içinde doğru şekilde çözülmesi gerekir.
* Güvenlik ilkeleri (NSGs) etki alanı birleştirme engellememelidir.

### <a name="additional-debugging-steps"></a>Ek hata ayıklama adımları

* Aynı alt ağda bir windows VM dağıtmak, etki alanı bir kullanıcı adı ve parola kullanarak makineye katılmak (bu kontrol paneli UI üzerinden yapılabilir), veya

* Aynı alt ağda bir ubuntu VM dağıtın ve makineye katılın
  * Makineye SSH
  * sudo su
  * Komut dosyasını kullanıcı adı ve parolayla çalıştırma
  * Komut dosyası ping, gerekli yapılandırma dosyaları ve daha sonra etki alanı oluşturmak. Başarılı olursa, DNS ayarlarınız iyidir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
