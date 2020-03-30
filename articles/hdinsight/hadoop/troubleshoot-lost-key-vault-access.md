---
title: Disk şifrelemeli Azure HDInsight kümeleri Key Vault erişimini kaybeder
description: Azure HDInsight kümeleriyle etkileşimde olurken sorunlar için sorun giderme adımları ve olası çözümler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/30/2020
ms.openlocfilehash: 2ae389be25cd8633a53a49cf000796c1510733a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76965170"
---
# <a name="scenario-azure-hdinsight-clusters-with-disk-encryption-lose-key-vault-access"></a>Senaryo: Disk şifrelemeli Azure HDInsight kümeleri Key Vault erişimini kaybeder

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Kaynak Sağlık Merkezi (RHC) `The HDInsight cluster is unable to access the key for BYOK encryption at rest`uyarısı, küme düğümlerinin müşterilere erişimini kaybettiği Kendi Anahtarını Getir (BYOK) kümeleri için gösterilir Anahtar Kasası (KV). Benzer uyarılar Apache Ambari UI'de de görülebilir.

## <a name="cause"></a>Nedeni

Uyarı, KV'ye küme düğümlerinden erişilebilmesini sağlayarak yönetilen kimlik atanan kullanıcıiçin ağ bağlantısı, KV durumu ve erişim ilkesini sağlar. Bu uyarı, yalnızca sonraki düğüm yeniden başlatmalarında yaklaşan aracı kapatma uyarısıdır, küme düğümleri yeniden başlatılana kadar çalışmaya devam eder.

**Disk Şifreleme Anahtarı Vault Durumu**uyarı hakkında daha fazla bilgi edinmek için Apache Ambari UI gidin. Bu uyarı, doğrulama hatasının nedeni hakkında ayrıntılara sahip olacaktır.

## <a name="resolution"></a>Çözüm

### <a name="kvaad-outage"></a>KV/AAD kesintisi

Daha fazla ayrıntı [için Azure Key Vault kullanılabilirliği ve artıklık](../../key-vault/key-vault-disaster-recovery-guidance.md) ve Azure durum sayfasına bakınhttps://status.azure.com/

### <a name="kv-accidental-deletion"></a>KV yanlışlıkla silme

* Otomatik kurtarma için KV üzerinde silinen anahtarı geri yükleyin. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/keyvault/recoverdeletedkey)
* Yanlışlıkla silmelerden kurtulmak için KV ekibine ulaşın.

### <a name="kv-access-policy-changed"></a>KV erişim ilkesi değiştirildi

KV'ye erişmek için HDI kümesine atanan Yönetilen Kimlik atanan kullanıcının erişim ilkelerini geri yükleyin.

### <a name="key-permitted-operations"></a>İzin verilen anahtar işlemler

KV'deki her anahtar için, izin verilen işlem kümesini seçebilirsiniz. BYOK tuşu için kaydırma ve açma işlemleri etkinleştirildiğinden emin olun

### <a name="expired-key"></a>Süresi dolmuş anahtar

Son kullanma tarihi geçtiyse ve anahtar döndürülmüyorsa, son kullanma tarihini temizlemek için yedekleme HSM'den anahtarı geri yükleyin veya KV ekibine başvurun.

### <a name="kv-firewall-blocking-access"></a>KV güvenlik duvarı erişimi engelliyor

BYOK küme düğümlerinin KV'ye erişmesine izin vermek için KV güvenlik duvarı ayarlarını düzeltin.

### <a name="nsg-rules-on-virtual-network-blocking-access"></a>Sanal ağ engelleme erişimi nsg kuralları

Kümeye bağlı sanal ağla ilişkili NSG kurallarını denetleyin.

## <a name="mitigation-and-prevention-steps"></a>Azaltma ve önleme adımları

### <a name="kv-accidental-deletion"></a>KV yanlışlıkla silme

* [Kaynak Kilidi kümesi](../../azure-resource-manager/management/lock-resources.md)ile Anahtar Kasası yapılandırın.
* Donanım Güvenlik Modülü anahtarları yedek.

### <a name="key-deletion"></a>Anahtar silme

Küme, anahtar silmeden önce silinmelidir.

### <a name="kv-access-policy-changed"></a>KV erişim ilkesi değiştirildi

Erişim ilkelerini düzenli olarak denetle ve test edin.

### <a name="expired-key"></a>Süresi dolmuş anahtar

* HSM tuşlarınızı yedekle.
* Herhangi bir son kullanma kümesi olmadan bir anahtar kullanın.
* Son kullanma tarihi ayarlanması gerekiyorsa, son kullanma tarihinden önce tuşları döndürün.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
