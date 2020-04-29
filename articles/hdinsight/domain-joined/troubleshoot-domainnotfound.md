---
title: Azure HDInsight 'ta küme oluşturma, DomainNotFound hatasıyla başarısız oluyor
description: Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: adcdafbc07fa0a8cc6970ab227b52aee798b084f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76776241"
---
# <a name="scenario-cluster-creation-fails-with-domainnotfound-error-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta küme oluşturma, DomainNotFound hatasıyla başarısız oluyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

HDI Secure (Kurumsal Güvenlik Paketi) kümesi oluşturma işlemi `DomainNotFound` hata iletisiyle başarısız oluyor.

## <a name="cause"></a>Nedeni

Yanlış DNS ayarları.

## <a name="resolution"></a>Çözüm

Etki alanına katılmış kümeler dağıtıldığında HDI, AAD DS 'de (her küme için) bir iç Kullanıcı adı ve parola oluşturur ve tüm küme düğümlerini bu etki alanına birleştirir. Etki alanına ekleme işlemi Samba araçları kullanılarak gerçekleştirilir. Aşağıdaki önkoşulların karşılandığından emin olun:

* Etki alanı adı DNS aracılığıyla çözümlenmelidir.
* Etki alanı denetleyicilerinin IP adresi, kümenin dağıtıldığı sanal ağ için DNS ayarlarında ayarlanmalıdır.
* Sanal ağa AAD DS 'nin sanal ağı eşlenirse, el ile yapılması gerekir.
* DNS ileticileri kullanıyorsanız, etki alanı adı sanal ağ içinde doğru şekilde çözümlenmelidir.
* Güvenlik ilkeleri (NSG 'ler), etki alanına katılımı engellemez.

### <a name="additional-debugging-steps"></a>Ek hata ayıklama adımları

* Aynı alt ağda bir Windows sanal makinesi dağıtın, etki alanına bir Kullanıcı adı ve parola kullanarak makineye katılarak (Bu denetim masası kullanıcı arabiriminden yapılabilir) veya

* Aynı alt ağda bir Ubuntu VM dağıtma ve makineye katma etki alanı
  * Makineye SSH
  * sudo su
  * Betiği Kullanıcı adı ve parolayla Çalıştır
  * Betik ping yapar, gerekli yapılandırma dosyalarını ve ardından etki alanını oluşturun. Başarılı olursa DNS ayarlarınız iyidir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
