---
title: PostgreSQL için Azure veritabanı müşteri tarafından yönetilen bir anahtarla tek sunuculu veri şifrelemesi
description: PostgreSQL için Azure veritabanı tek sunuculu veri şifreleme, müşteri tarafından yönetilen bir anahtarla, bekleyen veri koruması için Kendi Anahtarını Getir (BYOK) sağlar. Ayrıca, kuruluşların anahtar ve veri yönetiminde görev ayrımı uygulamasına olanak tanır.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 5516bfcb3ed32ba6635943298db2a7773db0a622
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77198709"
---
# <a name="azure-database-for-postgresql-single-server-data-encryption-with-a-customer-managed-key"></a>PostgreSQL için Azure veritabanı müşteri tarafından yönetilen bir anahtarla tek sunuculu veri şifrelemesi

> [!NOTE]
> Şu anda bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için AskAzureDBforPostgreSQL@service.microsoft.combaşvurun.

PostgreSQL için Azure veritabanı için müşteri tarafından yönetilen anahtarlarla veri şifreleme tek sunucu, bekleyen veri koruması için kendi anahtarınızı (BYOK) almanıza olanak sağlar. Ayrıca, kuruluşların anahtar ve veri yönetiminde görev ayrımı uygulamasına olanak tanır. Müşteri tarafından yönetilen şifreleme ile, bir anahtarın yaşam döngüsü, anahtar kullanım izinleri ve anahtarlar üzerindeki işlemlerin denetimi ile ilgili tam bir denetim ile sorumludur.

PostgreSQL için Azure veritabanı için müşteri tarafından yönetilen anahtarlarla veri şifreleme tek sunucu, sunucu düzeyinde ayarlanır. Belirli bir sunucu için, anahtar şifreleme anahtarı (KEK) olarak adlandırılan ve hizmet tarafından kullanılan veri şifreleme anahtarını (DEK) şifrelemek için kullanılan müşteri tarafından yönetilen bir anahtar kullanılır. KEK, müşterinin sahip olduğu ve müşteri tarafından yönetilen [Azure Key Vault](../key-vault/key-Vault-secure-your-key-Vault.md) örneğinde depolanan bir asimetrik anahtardır. Anahtar şifreleme anahtarı (KEK) ve veri şifreleme anahtarı (DEK), bu makalenin ilerleyen kısımlarında daha ayrıntılı olarak açıklanmıştır.

Key Vault, bulut tabanlı, dış anahtar yönetim sistemidir. Bu yüksek oranda kullanılabilir ve isteğe bağlı olarak FIPS 140-2 düzey 2 tarafından doğrulanan donanım güvenlik modülleri (HSM 'ler) tarafından desteklenen RSA şifreleme anahtarlarına yönelik ölçeklenebilir, güvenli depolama alanı sağlar. Depolanan bir anahtara doğrudan erişime izin vermez, ancak yetkili varlıklara şifreleme ve şifre çözme hizmetleri sağlar. Key Vault anahtarı oluşturabilir, içeri aktarılabilir veya [Şirket içi BIR HSM cihazından aktarılmasını](../key-vault/key-Vault-hsm-protected-keys.md)sağlayabilir.

> [!NOTE]
> Bu özellik, PostgreSQL için Azure veritabanı 'nın tek sunuculu "Genel Amaçlı" ve "bellek için Iyileştirilmiş" fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="benefits"></a>Avantajlar

PostgreSQL için Azure veritabanı için veri şifreleme tek sunucu aşağıdaki avantajları sağlar:

* Veri erişimi, anahtarı kaldırabilme ve veritabanını erişilemez hale getirme özelliği tarafından tam olarak denetlenir 
*   Anahtar yaşam döngüsü üzerinde, kurumsal ilkelerle hizalamak için anahtar dönüşü dahil tam denetim
*   Azure Key Vault içinde anahtarların merkezi yönetimi ve organizasyonu
*   Güvenlik ofisler ve DBA ve sistem yöneticileri arasında görev ayrımı uygulama yeteneği

## <a name="terminology-and-description"></a>Terminoloji ve açıklama

**Veri şifreleme anahtarı (dek)** : bir bölümü veya veri bloğunu şifrelemek için kullanılan BIR simetrik AES256 anahtarı. Farklı bir anahtarla her bir veri bloğunu şifrelemek, şifre çözümleme saldırılarını daha zor hale getirir. Belirli bir bloğu şifreleyen ve şifresini çözen kaynak sağlayıcı veya uygulama örneği, DEKs 'e erişim için gereklidir. Bir DEK 'ı yeni bir anahtarla değiştirdiğinizde, yalnızca ilişkili bloğundaki verilerin yeni anahtarla yeniden şifrelenmesi gerekir.

**Anahtar şifreleme anahtarı (kek)** : Deks 'leri şifrelemek için kullanılan bir şifreleme anahtarı. Key Vault hiçbir şekilde ayrılmayacak olan bir KEK, bunların şifreli ve denetimli olmasını sağlar. KEK 'e erişimi olan varlık, DEK gerektiren varlıktan farklı olabilir. KEK 'in şifresini çözmek için gerekli olduğundan, KEK, KEK silinerek etkin bir şekilde silinebilen tek bir noktasıdır.

KEKs ile şifrelenen DEKs 'ler ayrı olarak depolanır. Yalnızca KEK erişimi olan bir varlık bu DEKs 'in şifresini çözebilir. Daha fazla bilgi için bkz. [rest 'de şifrelemede güvenlik](../security/fundamentals/encryption-atrest.md).

## <a name="how-data-encryption-with-a-customer-managed-key-works"></a>Müşteri tarafından yönetilen bir anahtarla veri şifrelemenin çalışması

![Kendi Anahtarını Getir bir genel bakış gösteren diyagram](media/concepts-data-access-and-security-data-encryption/postgresql-data-encryption-overview.png)

Bir PostgreSQL sunucusu için Key Vault ' de depolanan müşteri tarafından yönetilen anahtarları kullanması için, bir Key Vault Yöneticisi sunucuya aşağıdaki erişim haklarını verir:

* **Get**: anahtar kasasındaki anahtarın ortak bölümünü ve özelliklerini almak için.
* **wrapKey**: dek ' i şifreleyebilmek için.
* **unwrapKey**: dek ' nin şifresini çözebilmek için.

Anahtar Kasası Yöneticisi ayrıca [Key Vault denetim olaylarının günlüğe kaydedilmesini etkinleştirerek](../azure-monitor/insights/azure-key-vault.md)daha sonra denetlenebilir.

Sunucu, anahtar kasasında depolanan müşteri tarafından yönetilen anahtarı kullanacak şekilde yapılandırıldığında, sunucu DEK şifrelemeleri anahtar kasasına gönderir. Key Vault, Kullanıcı veritabanında depolanan şifreli DEK döndürür. Benzer şekilde, gerektiğinde sunucu, şifre çözme için korunan DEK anahtar kasasına gönderilir. Denetçiler, günlüğe kaydetme etkinse Key Vault denetim olay günlüklerini gözden geçirmek için Azure Izleyici 'yi kullanabilir.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı için veri şifrelemeyi yapılandırma gereksinimleri tek sunucu

Key Vault yapılandırmaya yönelik gereksinimler aşağıda verilmiştir:

* Key Vault ve PostgreSQL için Azure veritabanı tek sunuculu aynı Azure Active Directory (Azure AD) kiracısına ait olmalıdır. Çapraz kiracı Key Vault ve sunucu etkileşimleri desteklenmez. Kaynakları taşımak daha sonra veri şifrelemeyi yeniden yapılandırmanız gerekir.
* Yanlışlıkla bir anahtar (veya Key Vault) silme işlemi gerçekleştiğinde, anahtar kasasında geçici silme özelliğini etkinleştirmeniz gerekir. Kullanıcı bu sırada kurtarmadığı veya silmediği takdirde, geçici olarak silinen kaynaklar 90 gün boyunca tutulur. Kurtarma ve Temizleme eylemlerinin Key Vault erişim ilkesiyle ilişkili kendi izinleri vardır. Geçici silme özelliği varsayılan olarak kapalıdır, ancak PowerShell veya Azure CLı aracılığıyla etkinleştirebilir (Azure portal aracılığıyla etkinleştirebileceğinizi unutmayın).
* PostgreSQL için Azure veritabanı 'na, benzersiz yönetilen kimliğini kullanarak Get, wrapKey ve unwrapKey izinleriyle anahtar kasasına tek sunuculu erişim izni verin. Azure portal, benzersiz kimlik, PostgreSQL tek sunucusunda veri şifrelemesi etkinleştirildiğinde otomatik olarak oluşturulur. Azure portal kullanırken Azure portal ayrıntılı, adım adım yönergeler için bkz. [PostgreSQL Için Azure veritabanı Için veri şifreleme tek sunucu](howto-data-encryption-portal.md) .

* Key Vault bir güvenlik duvarı kullanırken, **Güvenilen Microsoft hizmetlerinin güvenlik duvarını atlamasına Izin ver**seçeneğini etkinleştirmeniz gerekir.

Aşağıda, müşteri tarafından yönetilen anahtarı yapılandırmaya yönelik gereksinimler verilmiştir:

* DEK şifrelemek için kullanılan müşteri tarafından yönetilen anahtar yalnızca asimetrik, RSA 2028 olabilir.
* Anahtar etkinleştirme tarihi (ayarlandıysa), geçmişteki bir tarih ve saat olmalıdır. Sona erme tarihi (ayarlandıysa) gelecekteki bir tarih ve saat olmalıdır.
* Anahtar *etkin* durumda olmalıdır.
* Anahtar kasasında mevcut bir anahtarı içeri aktarıyorsanız, bunu desteklenen dosya biçimlerinde (`.pfx`, `.byok`, `.backup`) sağladığınızdan emin olun.

## <a name="recommendations"></a>Öneriler

Müşteri tarafından yönetilen bir anahtar kullanarak veri şifrelemeyi kullanırken, Key Vault yapılandırmaya yönelik öneriler aşağıda verilmiştir:

* Bu kritik kaynağı kimlerin silebilen ve yanlışlıkla veya yetkisiz silme işlemini engelleyebilecek kişileri denetlemek için Key Vault bir kaynak kilidi ayarlayın.
* Tüm şifreleme anahtarlarında denetim ve raporlamayı etkinleştirin. Key Vault, diğer güvenlik bilgilerine ve olay yönetim araçlarına eklemek kolay olan Günlükler sağlar. Azure Izleyici Log Analytics, zaten tümleştirilmiş bir hizmetin bir örneğidir.

* PostgreSQL için Key Vault ve Azure veritabanı 'nın aynı bölgede bulunduğundan emin olun. Bu, DEK sarmalama ve sarmalama işlemlerine daha hızlı erişim sağlamak için.

Müşteri tarafından yönetilen anahtarı yapılandırmaya yönelik öneriler aşağıda verilmiştir:

* Müşterinin yönettiği anahtarın bir kopyasını güvenli bir yerde tutun veya Emanet hizmetine sağlayın.

* Key Vault anahtarı oluşturursa, anahtarı ilk kez kullanmadan önce anahtar yedeklemesi oluşturun. Key Vault için yalnızca yedeklemeyi geri yükleyebilirsiniz. Backup komutu hakkında daha fazla bilgi için bkz. [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyVault/backup-azkeyVaultkey).

## <a name="inaccessible-customer-managed-key-condition"></a>Erişilemeyen müşteri tarafından yönetilen anahtar koşulu

Key Vault, müşteri tarafından yönetilen bir anahtarla veri şifrelemeyi yapılandırdığınızda, sunucunun çevrimiçi kalması için bu anahtara sürekli erişim gerekir. Sunucu, Key Vault müşterinin yönettiği anahtara erişimi kaybederse, sunucu 10 dakikalık bir süre içinde tüm bağlantıları reddetmeye başlar. Sunucu karşılık gelen bir hata iletisi yayınlar ve sunucu durumunu *erişilemez*olarak değiştirir. Bu durumdaki bir veritabanında izin verilen tek eylem, silme işlemini siliyor.

### <a name="accidental-key-access-revocation-from-key-vault"></a>Key Vault 'den yanlışlıkla anahtar erişimi iptali

Key Vault için yeterli erişim haklarına sahip birinin, yanlışlıkla anahtara sunucu erişimini devre dışı bırakmasından kaynaklanabilir:

* Anahtar kasasının Get, wrapKey ve unwrapKey izinleri sunucudan iptal ediliyor.
* Anahtar siliniyor.
* Anahtar Kasası siliniyor.
* Anahtar kasasının güvenlik duvarı kurallarını değiştirme.

* Azure AD 'de sunucunun yönetilen kimliğini silme.

## <a name="monitor-the-customer-managed-key-in-key-vault"></a>Key Vault 'de müşteri tarafından yönetilen anahtarı izleyin

Veritabanı durumunu izlemek ve saydam veri şifreleme koruyucusu erişiminin kaybedilmesi için uyarı etkinleştirmek üzere aşağıdaki Azure özelliklerini yapılandırın:

* [Azure Kaynak durumu](../service-health/resource-health-overview.md): müşteri anahtarına erişimi kesilen erişilemeyen bir veritabanı, veritabanına ilk bağlantı reddedildikten sonra "erişilemez" olarak gösterilir.
* [Etkinlik günlüğü](../service-health/alerts-activity-log-service-notifications.md): müşteri tarafından yönetilen Key Vault müşteri anahtarına erişim başarısız olursa, girdiler etkinlik günlüğüne eklenir. Bu olaylar için uyarı oluşturursanız, erişimi mümkün olan en kısa sürede yeniden yükleyebilirsiniz.

* [Eylem grupları](../azure-monitor/platform/action-groups.md): tercihlerinizi temel alarak size bildirim ve uyarı göndermek için bunları tanımlayın.

## <a name="restore-and-replicate-with-a-customers-managed-key-in-key-vault"></a>Key Vault bir müşterinin yönetilen anahtarıyla geri yükleme ve çoğaltma

PostgreSQL için Azure veritabanı tek sunucu, Key Vault ' de depolanan bir müşterinin yönetilen anahtarıyla şifrelendikten sonra, sunucunun yeni oluşturulan kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemiyle ya da okuma çoğaltmaları aracılığıyla yapabilirsiniz. Ancak, kopya yeni bir müşterinin şifreleme için yönetilen anahtarını yansıtacak şekilde değiştirilebilir. Müşteri tarafından yönetilen anahtar değiştirildiğinde, sunucunun eski yedeklemeleri en son anahtarı kullanmaya başlar.

Geri yükleme veya okuma çoğaltması oluşturma sırasında müşteri tarafından yönetilen veri şifrelemesini ayarlarken oluşan sorunlardan kaçınmak için, ana ve geri yüklenen/çoğaltma sunucularındaki bu adımları izlemeniz önemlidir:

* PostgreSQL için ana Azure veritabanından tek sunucu için geri yükleme veya okuma çoğaltması oluşturma sürecini başlatın.
* Yeni oluşturulan sunucuyu (geri yüklenmiş/çoğaltma) erişilemez durumda tutun, çünkü benzersiz kimliği henüz Key Vault izinler olarak verilmemiştir.
* Geri yüklenen/çoğaltma sunucusunda, veri şifreleme ayarları 'nda müşteri tarafından yönetilen anahtarı yeniden doğrulayın. Bu, yeni oluşturulan sunucuya Key Vault ' de depolanan anahtara sarmalama ve sarmalama izinleri verilmesini sağlar.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Portal kullanarak, PostgreSQL Için Azure veritabanınız için müşteri tarafından yönetilen bir anahtarla veri şifrelemeyi ayarlamayı](howto-data-encryption-portal.md)öğrenin.

