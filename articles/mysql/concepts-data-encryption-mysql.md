---
title: Müşteri tarafından yönetilen anahtarla veri şifreleme - MySQL için Azure Veritabanı
description: Müşteri tarafından yönetilen bir anahtara sahip MySQL veri şifrelemesi için Azure Veritabanı, veri koruması için kendi anahtarınızı (BYOK) hazır da getirmenizi sağlar. Ayrıca, kuruluşların anahtar ve veri yönetiminde görev ayrımını uygulamasına da olanak tanır.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: a97fee619858aa024ff208b72d3b2594c30d2fd5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299133"
---
# <a name="azure-database-for-mysql-data-encryption-with-a-customer-managed-key"></a>Müşteri tarafından yönetilen bir anahtarla MySQL veri şifrelemesi için Azure Veritabanı

> [!NOTE]
> Şu anda, bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için, iletişim . AskAzureDBforMySQL@service.microsoft.com

MySQL için Azure Veritabanı için müşteri tarafından yönetilen anahtarlarla veri şifreleme, veri koruması için kendi anahtarınızı (BYOK) geri ödemenize olanak tanır. Ayrıca, kuruluşların anahtar ve veri yönetiminde görev ayrımını uygulamasına da olanak tanır. Müşteri tarafından yönetilen şifreleme ile, anahtarın yaşam döngüsünden, anahtar kullanım izinlerinden ve anahtarlarla ilgili işlemlerin denetleilmesinden ve tam denetiminden siz sorumlusunuz.

MySQL için Azure Veritabanı için müşteri tarafından yönetilen anahtarlarla veri şifreleme, sunucu düzeyinde ayarlanır. Belirli bir sunucu için, hizmet tarafından kullanılan veri şifreleme anahtarını (DEK) şifrelemek için anahtar şifreleme anahtarı (KEK) adı verilen müşteri tarafından yönetilen bir anahtar kullanılır. KEK, müşteri tarafından yönetilen ve müşteri tarafından yönetilen bir [Azure Key Vault](../key-vault/key-Vault-secure-your-key-Vault.md) örneğinde depolanan asimetrik bir anahtardır. Anahtar Şifreleme Anahtarı (KEK) ve Veri Şifreleme Anahtarı (DEK) bu makalenin ilerleyen saatlerinde daha ayrıntılı olarak açıklanmıştır.

Key Vault bulut tabanlı, harici anahtar yönetim sistemidir. Son derece kullanılabilir ve isteğe bağlı olarak FIPS 140-2 Düzey 2 onaylı donanım güvenlik modülleri (HSM) tarafından desteklenen RSA şifreleme anahtarları için ölçeklenebilir, güvenli depolama sağlar. Depolanan bir anahtara doğrudan erişime izin vermez, ancak yetkili kuruluşlara şifreleme ve şifre çözme hizmetleri sağlar. Key Vault anahtarı oluşturabilir, içe aktarabilir veya [şirket içi HSM aygıtından aktarabilir.](../key-vault/key-Vault-hsm-protected-keys.md)

> [!NOTE]
> Bu özellik, MySQL için Azure Veritabanı'nın "Genel Amaç" ve "Bellek Optimize Edilmiş" fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="benefits"></a>Avantajlar

MySQL için Azure Veritabanı için veri şifreleme aşağıdaki avantajları sağlar:

* Veri erişimi, anahtarı kaldırma ve veritabanını erişilemez hale getirme yeteneği tarafından tamamen sizin yeriniztarafından denetlenir 
* Anahtar yaşam döngüsü üzerinde tam kontrol, anahtar döndürme de dahil olmak üzere kurumsal politikalar ile hizalamak için
* Azure Anahtar Kasası'nda anahtarların merkezi yönetimi ve organizasyonu
* Güvenlik görevlileri ile DBA ve sistem yöneticileri arasında görev ayrımını uygulama becerisi


## <a name="terminology-and-description"></a>Terminoloji ve açıklama

**Veri şifreleme anahtarı (DEK)**: Bir veri bölmesini veya bloğunu şifrelemek için kullanılan simetrik Bir AES256 anahtarıdır. Her veri bloğunu farklı bir anahtarla şifrelemek, kripto analizi saldırılarını daha da zorlaştırır. DEK'lere erişim, belirli bir bloğu şifreleyen ve şifresini çözen kaynak sağlayıcı veya uygulama örneği tarafından gereklidir. Bir DEK'i yeni bir anahtarla değiştirdiğinizde, yalnızca ilişkili bloğundaki veriler yeni anahtarla yeniden şifrelenmelidir.

**Anahtar şifreleme anahtarı (KEK)**: DEK'leri şifrelemek için kullanılan şifreleme anahtarı. Key Vault'u hiç terk etmeyen bir KEK, DEK'lerin şifrelenmesini ve kontrol edilmesine izin verir. KEK erişimi olan varlık DEK gerektiren varlık farklı olabilir. KEK'in DEK'lerin şifresini çözmesi gerektiğinden, KEK, KEK'in silinmesi ile DEK'lerin etkin bir şekilde silinebileceği tek bir noktadır.

KEK'ler ile şifrelenen DEK'ler ayrı olarak saklanır. Bu DEK'lerin şifresini yalnızca KEK erişimi olan bir kuruluş çözebilir. Daha fazla bilgi için, [güvenlik şifreleme sinde istirahat](../security/fundamentals/encryption-atrest.md)edin.

## <a name="how-data-encryption-with-a-customer-managed-key-works"></a>Müşteri tarafından yönetilen bir anahtarla veri şifreleme nasıl çalışır?

![Kendi Anahtarını Getir'e genel bir bakış gösteren diyagram](media/concepts-data-access-and-security-data-encryption/mysqloverview.png)

Bir MySQL sunucusunun DEK şifrelemesi için Key Vault'ta depolanan müşteri tarafından yönetilen anahtarları kullanması için, Key Vault yöneticisi sunucuya aşağıdaki erişim haklarını verir:

* **get**: Anahtar kasasındaki anahtarın ortak kısmını ve özelliklerini almak için.
* **wrapKey**: DEK'i şifreleyebilmek.
* **unwrapKey**: DEK şifresini çözebilmek için.

Anahtar kasa yöneticisi, daha sonra denetlenebilmeleri için [Key Vault denetim olaylarının günlüğe kaydedilmesini](../azure-monitor/insights/azure-key-vault.md)de etkinleştirebilir.

Sunucu, anahtar kasasında depolanan müşteri tarafından yönetilen anahtarı kullanacak şekilde yapılandırıldığında, sunucu DEK'i şifreleme için anahtar kasasına gönderir. Key Vault, kullanıcı veritabanında depolanan şifrelenmiş DEK'i döndürür. Benzer şekilde, gerektiğinde sunucu korumalı DEK'i şifre çözme için anahtar kasasına gönderir. Tetkikçiler, günlük kullanımı etkinse, Key Vault denetim olay günlüğünü gözden geçirmek için Azure Monitor'u kullanabilir.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure Veritabanı için veri şifrelemesi yapılandırma gereksinimleri

Anahtar Kasası yapılandırma gereksinimleri şunlardır:

* MySQL için Key Vault ve Azure Veritabanı aynı Azure Active Directory (Azure AD) kiracısına ait olmalıdır. Çapraz kiracı Key Vault ve sunucu etkileşimleri desteklenmez. Daha sonra kaynakları taşımak, veri şifrelemesini yeniden yapılandırmanızı gerektirir.
* Yanlışlıkla bir anahtar (veya Key Vault) silme gerçekleşirse veri kaybına karşı korumak için anahtar kasasındaki yumuşak silme özelliğini etkinleştirmelisiniz. Bu arada kullanıcı bunları kurtarmadığı veya temizlemediği sürece, silinen kaynaklar 90 gün boyunca saklanır. Kurtarma ve temizleme eylemlerinin, Key Vault erişim ilkesinde ilişkili kendi izinleri vardır. Yumuşak silme özelliği varsayılan olarak kapalıdır, ancak PowerShell veya Azure CLI (Azure portalı üzerinden etkinleştiremezsiniz unutmayın) aracılığıyla etkinleştirebilirsiniz.
* Benzersiz yönetilen kimliğini kullanarak Get, wrapKey ve UnwrapKey izinleriyle anahtar kasasına MySQL erişimi için Azure Veritabanı'nı ver. Azure portalında, MySQL'de veri şifreleme etkinleştirildiğinde benzersiz kimlik otomatik olarak oluşturulur. Azure portalını kullanırken ayrıntılı, adım adım yönergeler [için MySQL için veri şifrelemesini yapılandır'](howto-data-encryption-portal.md) a bakın.

* Key Vault ile bir güvenlik duvarı kullanıyorsanız, **güvenlik duvarını atlamak için güvenilir Microsoft hizmetlerine İzin ver**seçeneğini etkinleştirmelisiniz.

Müşteri tarafından yönetilen anahtarı yapılandırmak için aşağıdaki gereksinimler şunlardır:

* DEK'i şifrelemek için kullanılacak müşteri tarafından yönetilen anahtar yalnızca asimetrik olabilir, RSA 2028.
* Anahtar etkinleştirme tarihi (eğer ayarlanmışsa) geçmişte bir tarih ve saat olmalıdır. Son kullanma tarihi (ayarlanmışsa) gelecekteki bir tarih ve saat olmalıdır.
* Anahtar *Etkin* durumda olmalıdır.
* Varolan bir anahtarı anahtar kasasına aktarıyorsanız, desteklenen dosya biçimlerinde (`.pfx`, `.byok`, `.backup`, ) sağladığından emin olun.

## <a name="recommendations"></a>Öneriler

Müşteri tarafından yönetilen bir anahtar kullanarak veri şifreleme kullanırken, Key Vault'u yapılandırmaönerileri şunlardır:

* Bu kritik kaynağı kimin silecegini ve kazara veya yetkisiz silmeyi kimin önleyecegini kontrol etmek için Key Vault' a bir kaynak kilidi ayarlayın.
* Tüm şifreleme anahtarlarının denetimini ve raporlamasını etkinleştirin. Key Vault, diğer güvenlik bilgilerine ve olay yönetimi araçlarına kolayca enjekte edilebilen günlükler sağlar. Azure Monitor Log Analytics, zaten entegre edilmiş bir hizmetin bir örneğidir.

* DEK kaydırma ve açma işlemlerine daha hızlı erişim sağlamak için MySQL için Key Vault ve Azure Veritabanı'nın aynı bölgede bulunduğundan emin olun.

Müşteri tarafından yönetilen bir anahtarı yapılandırmak için öneriler şunlardır:

* Müşteri tarafından yönetilen anahtarın bir kopyasını güvenli bir yerde saklayın veya emanet hizmetine emanet edin.

* Key Vault anahtarı oluşturursa, anahtarı ilk kez kullanmadan önce bir anahtar yedeklemesi oluşturun. Yedeği yalnızca Key Vault'a geri yükleyebilirsiniz. Yedekleme komutu hakkında daha fazla bilgi için [Backup-AzKeyVaultKey'e](https://docs.microsoft.com/powershell/module/az.keyVault/backup-azkeyVaultkey)bakın.

## <a name="inaccessible-customer-managed-key-condition"></a>Erişilemeyen müşteri tarafından yönetilen anahtar koşulu

Key Vault'ta müşteri tarafından yönetilen bir anahtarla veri şifrelemeyi yapılandırdığınızda, sunucunun çevrimiçi kalması için bu anahtara sürekli erişim gerekir. Sunucu Key Vault'taki müşteri tarafından yönetilen anahtara erişimini kaybederse, sunucu 10 dakika içinde tüm bağlantıları reddetmeye başlar. Sunucu karşılık gelen bir hata iletisi veriyor ve sunucu durumunu *erişilemeilemeye*göre değiştiriyor. Bu durumda bir veritabanında izin verilen tek eylem onu siler.

### <a name="accidental-key-access-revocation-from-key-vault"></a>Key Vault'tan yanlışlıkla anahtar erişim iptali

Key Vault'a yeterli erişim hakkına sahip olan birinin sunucunun anahtara erişimini yanlışlıkla devre dışı kılmış olması mümkün olabilir:

* Anahtar kasasının al, wrapKey ve sunucudan anahtar izinlerini açma iznini iptal etmek.
* Anahtarı silerken.
* Anahtar kasasını silerken.
* Anahtar kasasının güvenlik duvarı kurallarını değiştiriyorum.

* Azure AD'de sunucunun yönetilen kimliğini silme.

## <a name="monitor-the-customer-managed-key-in-key-vault"></a>Key Vault'ta müşteri tarafından yönetilen anahtarı izleme

Veritabanı durumunu izlemek ve saydam veri şifreleme koruyucusu erişiminin kaybedilmesi konusunda uyarıyı etkinleştirmek için aşağıdaki Azure özelliklerini yapılandırın:

* [Azure Kaynak Durumu](../service-health/resource-health-overview.md): Veritabanına ilk bağlantı reddedildikten sonra müşteri anahtarına erişimini kaybetmiş erişilemeyen bir veritabanı ,"Erişilemez" olarak gösterir.
* [Etkinlik günlüğü](../service-health/alerts-activity-log-service-notifications.md): Müşteri tarafından yönetilen Key Vault'taki müşteri anahtarına erişim başarısız olduğunda, girişler etkinlik günlüğüne eklenir. Bu olaylar için uyarılar oluşturursanız, erişimi mümkün olan en kısa sürede yeniden başlatabilirsiniz.

* [Eylem grupları](../azure-monitor/platform/action-groups.md): Tercihlerinize göre bildirimler ve uyarılar göndermek için bunları tanımlayın.

## <a name="restore-and-replicate-with-a-customers-managed-key-in-key-vault"></a>Key Vault'ta müşterinin yönetilen anahtarıyla geri yükleme ve çoğaltma

MySQL için Azure Veritabanı, Key Vault'ta saklanan bir müşterinin yönetilen anahtarıyla şifrelendikten sonra, sunucunun yeni oluşturulan herhangi bir kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemi veya okuma yinelemeleri aracılığıyla yapabilirsiniz. Ancak, kopya yeni bir müşterinin yönetilen şifreleme anahtarını yansıtacak şekilde değiştirilebilir. Müşteri tarafından yönetilen anahtar değiştirildiğinde, sunucunun eski yedekleri en son anahtarı kullanmaya başlar.

Geri yükleme sırasında müşteri tarafından yönetilen veri şifrelemesini ayarlarken veya yineleme oluşturmayı okurken sorunları önlemek için, ana ve geri yüklenen/çoğaltma sunucularında aşağıdaki adımları izlemen önemlidir:

* MySQL için ana Azure Veritabanından geri yükleme veya okuma yineleme oluşturma işlemini başlatın.
* Benzersiz kimliği henüz Key Vault'a izin verilmediği için, yeni oluşturulan sunucuyu (geri yüklenen/çoğaltma) erişilemez durumda tutun.
* Geri yüklenen/çoğaltma sunucusunda, veri şifreleme ayarlarında müşteri tarafından yönetilen anahtarı yeniden doğrulayın. Bu, yeni oluşturulan sunucuya Anahtar Kasası'nda depolanan anahtara kaydırma ve açma izinleri verilmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Azure portalını kullanarak MySQL için Azure veritabanınız için müşteri tarafından yönetilen bir anahtarla veri şifrelemesini](howto-data-encryption-portal.md)nasıl ayarlayayınız öğrenin.
