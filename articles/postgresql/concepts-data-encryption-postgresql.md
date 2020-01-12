---
title: PostgreSQL için Azure veritabanı müşteri tarafından yönetilen anahtarla tek sunuculu veri şifrelemesi
description: PostgreSQL için Azure veritabanı müşteri tarafından yönetilen anahtarla tek sunuculu veri şifrelemesi
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/10/2020
ms.openlocfilehash: df2504b7e65a0087c9a8e20d94c596a19494069c
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903944"
---
# <a name="azure-database-for-postgresql-single-server-data-encryption-with-customer-managed-key"></a>PostgreSQL için Azure veritabanı müşteri tarafından yönetilen anahtarla tek sunuculu veri şifrelemesi

> [!NOTE]
> Şu anda bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için lütfen AskAzureDBforPostgreSQL@service.microsoft.combaşvurun.

PostgreSQL için Azure veritabanı tek sunuculu veri şifrelemesi müşteri tarafından yönetilen anahtarla, bekleyen veri koruması için Kendi Anahtarını Getir (BYOK) sağlar ve kuruluşların anahtar ve veri yönetiminde görevlerin ayrılmasını sağlar. Müşteri tarafından yönetilen şifreleme sayesinde, anahtar yaşam döngüsünün (anahtar oluşturma, karşıya yükleme, döndürme, silme), anahtar kullanım izinlerinin ve anahtarlardaki işlemlerin denetlenmesine yönelik tam bir denetim üzerinden ve bu işlemden sorumlusunuz.

PostgreSQL için Azure veritabanı tek sunucu için, veri şifrelemesi sunucu düzeyinde ayarlanır. Bu veri şifreleme biçimi sayesinde, anahtar, bulut tabanlı bir dış anahtar yönetim sistemi olan, müşterinin sahip olduğu ve müşteri tarafından yönetilen [Azure Key Vault (AKV)](https://docs.microsoft.com/azure/key-Vault/key-Vault-secure-your-key-Vault)depolanan, müşteri tarafından yönetilen bir asimetrik anahtar olan veritabanı şifreleme anahtarı (dek) şifrelemesinde kullanılır. AKV yüksek oranda kullanılabilir ve isteğe bağlı olarak FIPS 140-2 düzey 2 tarafından doğrulanan donanım güvenlik modülleri (HSM 'ler) tarafından desteklenen RSA şifreleme anahtarları için ölçeklenebilir güvenli depolama sağlar. Depolanan bir anahtara doğrudan erişime izin vermez, ancak yetkili varlıkların anahtarını kullanarak şifreleme/şifre çözme hizmetleri sağlar. Anahtar, Key Vault tarafından oluşturulabilir, içeri aktarılabilir veya [bir şirket ıçı HSM cihazından Key Vault aktarılabilir](https://docs.microsoft.com/azure/key-Vault/key-Vault-hsm-protected-keys).

> [!NOTE]
> Bu özellik, PostgreSQL için Azure veritabanı 'nın tek sunuculu Genel Amaçlı ve bellek için Iyileştirilmiş fiyatlandırma katmanlarını desteklediği tüm Azure bölgelerinde kullanılabilir.

## <a name="benefits"></a>Avantajlar

PostgreSQL için Azure veritabanı için veri şifreleme tek sunucu aşağıdaki avantajları sağlar:
* Şifreleme anahtarı için daha fazla saydamlık ve ayrıntılı denetim ve yönetim 
* Azure Key Vault içinde barındırarak anahtar merkezi yönetimi ve kuruluşu. 
* Kuruluş içindeki anahtar ve verilerin yönetiminde görevlerin ayrılmasını uygulama yeteneği
* Bir kuruluş içindeki veri yönetiminden önemli yönetimi ayırın, bu nedenle Key Vault yönetici şifreli veritabanını erişilemez hale getirmek için anahtar erişim izinlerini iptal edebilir 
* Azure Key Vault, Microsoft 'un şifreleme anahtarlarını göremediği veya ayıklayamayacağı gibi tasarlandığından, son müşterilerinizden daha fazla güven

## <a name="terminology-and-description"></a>Terminoloji ve açıklama

**Veri şifreleme anahtarı (dek)** – bir bölümü veya veri bloğunu şifrelemek için kullanılan BIR simetrik AES256 anahtarı. Farklı bir anahtarla her bir veri bloğunu şifrelemek, şifre çözümleme saldırılarını daha zor hale getirir. Belirli bir bloğu şifreleyen ve şifresini çözen kaynak sağlayıcı veya uygulama örneği, DEKs 'e erişim için gereklidir. Bir DEK yeni bir anahtarla değiştirildiğinde, yalnızca ilişkili bloğundaki veriler yeni anahtarla yeniden şifrelenmelidir.

**Anahtar şifreleme anahtarı (kek)** -veri şifreleme anahtarlarını şifrelemek için kullanılan bir şifreleme anahtarı. Key Vault hiçbir şekilde ayrılmamış bir anahtar şifreleme anahtarı kullanımı, veri şifreleme anahtarlarının kendilerine şifreli ve denetimli olmasını sağlar. KEK 'e erişimi olan varlık, DEK gerektiren varlıktan farklı olabilir. KEK 'in şifresini çözmek için gerekli olduğundan, KEK, KEK silinerek etkin bir şekilde silinebilen tek bir noktasıdır.

Anahtar şifreleme anahtarlarıyla şifrelenen veri şifreleme anahtarları ayrı olarak saklanır ve yalnızca anahtar şifreleme anahtarına erişimi olan bir varlık bu veri şifreleme anahtarlarının şifresini çözebilir. Ayrıntılar için lütfen [bekleyen şifreleme bölümünde güvenlik](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)bölümüne bakın.

## <a name="how-data-encryption-with-customer-managed-key-works"></a>Müşteri tarafından yönetilen anahtarla veri şifrelemenin çalışması

![Kendi anahtarını getir genel bakış](media/concepts-data-access-and-security-data-encryption/postgresql-data-encryption-overview.png)

Bir PostgreSQL sunucusunun, e-posta şifrelemesi için AKV 'de depolanan müşteri tarafından yönetilen anahtarları kullanabilmesi için, Key Vault yöneticinin benzersiz kimliğini kullanarak sunucuya aşağıdaki erişim haklarını vermesi gerekir:
* Key Vault, anahtarın genel bölümünü ve **özelliklerini alma**
* **wrapKey** -koruyabilmek için (ŞIFRELEME) dek
* **unwrapKey** -korumayı kaldırmak için (şifre çözme) dek

Key Vault Yöneticisi, [Key Vault denetim olaylarının günlüğe kaydedilmesini de etkinleştirerek](https://docs.microsoft.com/azure/azure-monitor/insights/azure-key-Vault)daha sonra denetlenebilir.

Sunucu, Key Vault depolanan müşteri tarafından yönetilen anahtarı kullanacak şekilde yapılandırıldığında, sunucu, bu DEK, şifrelemeleri için bir Key Vault gönderir. Key Vault, Kullanıcı veritabanında depolanan şifreli DEK döndürür. Benzer şekilde, gerektiğinde, sunucu şifre çözme için Key Vault korumalı bir DEK gönderir. Denetçiler, günlük etkinleştirildiyse Key Vault AuditEvent günlüklerini gözden geçirmek için Azure Izleyicisini kullanabilir.

## <a name="requirements-for-configuring-data-encryption-for-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı için veri şifrelemeyi yapılandırma gereksinimleri tek sunucu

### <a name="requirements-for-configuring-akv"></a>AKV 'yi yapılandırma gereksinimleri

* Key Vault ve PostgreSQL için Azure veritabanı tek sunucu aynı Azure Active Directory (AAD) kiracısına ait olmalıdır. Çapraz kiracı Key Vault ve sunucu etkileşimleri desteklenmez. Kaynakları taşımak daha sonra veri şifrelemeyi yeniden yapılandırmanız gerekir. Kaynakları taşıma hakkında daha fazla bilgi edinin.
* Geçici silme özelliği Key Vault, veri kaybını korumak için yanlışlıkla anahtar (veya Key Vault) silme işlemi gerçekleşir. Geçici olarak silinen kaynaklar, bu sırada müşteri tarafından kurtarılmaz veya temizlenmediği takdirde 90 gün boyunca tutulur. Kurtarma ve Temizleme eylemlerinin Key Vault erişim ilkesiyle ilişkili kendi izinleri vardır. Geçici silme özelliği varsayılan olarak kapalıdır ve PowerShell veya CLı aracılığıyla etkinleştirilebilir. Azure portal aracılığıyla etkinleştirilemez.
* PostgreSQL için Azure veritabanı 'na, benzersiz yönetilen kimliğini kullanarak **Get, wrapKey, unwrapKey** izinleriyle Key Vault. Azure portal kullanırken, otomatik olarak, PostgreSQL tek sunucusunda veri şifrelemesi etkinleştirildiğinde benzersiz bir kimlik oluşturulur. Azure portal kullanırken ayrıntılı adım adım yönergeler için bkz. [PostgreSQL Için veri şifrelemeyi yapılandırma](howto-data-encryption-portal.md) .

* AKV ile güvenlik duvarı kullanırken, *Güvenilen Microsoft hizmetlerinin güvenlik duvarını atlamasına Izin ver*seçeneğini etkinleştirmeniz gerekir.

### <a name="requirements-for-configuring-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarı yapılandırma gereksinimleri
* DEK şifrelemek için kullanılan müşteri tarafından yönetilen anahtar yalnızca asimetrik, RSA 2028 olabilir.
* Anahtar etkinleştirme tarihi (ayarlandıysa), geçmişteki bir tarih ve saat olmalıdır. Sona erme tarihi (ayarlandıysa) gelecekteki bir tarih ve saat olmalıdır.
* Anahtar *etkin* durumda olmalıdır.

* Mevcut anahtarı Key Vault içe aktarıyorsanız, bu dosyayı desteklenen dosya biçimlerinde (`.pfx`, `.byok`, `.backup`) sağladığınızdan emin olun.

## <a name="recommendations-when-using-data-encryption-using-customer-managed-key"></a>Müşteri tarafından yönetilen anahtar kullanılarak veri şifrelemesi kullanma önerileri

### <a name="recommendation-for-configuring-akv"></a>AKV 'yi yapılandırma önerisi

* Bu kritik kaynağı kimlerin silebilen ve yanlışlıkla veya yetkisiz silme işlemini engelleyebilecek kişileri denetlemek için Key Vault bir kaynak kilidi ayarlayın. Kaynak kilitleri hakkında daha fazla bilgi edinin.
* Tüm şifreleme anahtarlarında denetim ve raporlamayı etkinleştir: Key Vault, diğer güvenlik bilgilerine ve olay yönetim araçlarına kolayca eklenecek Günlükler sağlar. Azure Izleyici Log Analytics, zaten tümleştirilmiş bir hizmetin bir örneğidir.

* PostgreSQL için tek sunuculu Key Vault ve Azure veritabanı 'nın aynı bölgede bulunduğundan emin olun. Bu, DEK sarmalama/sarmalama işlemlerine daha hızlı erişim sağlamak için. 

### <a name="recommendation-for-configuring-customer-managed-key"></a>Müşteri tarafından yönetilen anahtarı yapılandırmaya yönelik öneri

* Müşterinin yönettiği anahtarın bir kopyasını (KEK) güvenli bir yerde tutun veya Emanet hizmetine bu hizmeti sağlar.

* Anahtar Key Vault oluşturulduysa, ilk kez AKV 'de anahtarı kullanmadan önce anahtar yedeklemesi oluşturun. Yedekleme, yalnızca bir Azure Key Vault geri yüklenebilir. [Backup-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyVault/backup-azkeyVaultkey) komutu hakkında daha fazla bilgi edinin. 

## <a name="inaccessible-customer-managed-key-condition"></a>Erişilemeyen müşteri tarafından yönetilen anahtar koşulu

Veri şifreleme Azure Key Vault (AKV) içinde müşteri tarafından yönetilen anahtarla yapılandırıldığında, sunucunun çevrimiçi kalması için bu anahtara sürekli erişim gerekir. Sunucu, AKV 'de müşteri tarafından yönetilen anahtara erişimi kaybederse 10 dakika içinde, sunucu karşılık gelen hata iletisiyle tüm bağlantıları reddetmeye başlar ve sunucu durumunu **erişilemez**olarak değiştirir. Erişilemeyen durumdaki bir veritabanında izin verilen tek eylem, onu siliyor.

### <a name="accidental-key-access-revocation-from-the-azure-key-vault-akv"></a>Azure Key Vault (AKV) 'den yanlışlıkla anahtar erişimi iptali

Key Vault için yeterli erişim haklarına sahip birisi yanlışlıkla anahtara sunucu erişimini devre dışı bırakır:
* Key Vault sunucudan Get, wrapKey, unwrapKey izinleri iptal ediliyor
* anahtar siliniyor
* Key Vault silme
* Key Vault güvenlik duvarı kurallarını değiştirme

* Azure Active Directory içindeki sunucunun yönetilen kimliğini silme

## <a name="monitoring-of-the-customer-managed-key-in-the-key-vault"></a>Key Vault müşterinin yönettiği anahtarın izlenmesi

Veritabanı durumunu izlemek ve TDE koruyucu erişimi kaybı nedeniyle uyarı etkinleştirmek için aşağıdaki Azure özelliklerini yapılandırın:

* [Azure Kaynak durumu](../service-health/resource-health-overview.md) -veritabanına ilk bağlantı reddedildikten sonra, müşteri anahtarına erişimi kesilen erişilemeyen bir veritabanı "erişilemez" olarak görünür.
* [Etkinlik günlüğü](../service-health/alerts-activity-log-service-notifications.md) -müşteri tarafından yönetilen Key Vault müşteri anahtarına erişim başarısız olursa, girdiler etkinlik günlüğüne eklenir. Bu olaylar için uyarı oluşturulması mümkün olan en kısa sürede erişimi yeniden etkinleştirmenizi sağlar.

* [Eylem grupları](../azure-monitor/platform/action-groups.md) , tercihlerinize, örn. e-posta/SMS/Push/Voice, Logic App, Web KANCASı, ISM veya Otomasyon Runbook 'una göre bildirim ve uyarı göndermek için tanımlanabilir.

## <a name="restore-and-replica-with-customers-managed-key-in-the-key-vault"></a>Key Vault müşterinin yönetilen anahtarıyla geri yükleme ve çoğaltma

PostgreSQL için Azure veritabanı tek sunuculu, Key Vault depolanan müşterinin yönetilen anahtarı ile şifrelendikten sonra, sunucunun yeni oluşturulmuş bir kopyası yerel veya coğrafi geri yükleme işlemi ya da okuma çoğaltmaları aracılığıyla aynı müşteri tarafından da şifrelenir yönetilen anahtar. Ancak, bu kullanıcılar, şifreleme için yeni müşterinin yönetilen anahtarını yansıtacak şekilde değiştirilebilir. Müşteri tarafından yönetilen anahtar değiştirildiğinde, sunucunun eski yedekleri en son anahtarı kullanmaya başlar.

Geri yükleme veya okuma çoğaltması oluşturma sırasında müşteri tarafından yönetilen veri şifrelemeyi ayarlamaya çalışırken sorunlardan kaçınmak için, ana ve geri yükleme/Çoğaltma sunucusunda bu adımları izlemeniz önemlidir:

* PostgreSQL için ana Azure veritabanından tek sunucu için geri yükleme veya okuma çoğaltması oluşturma sürecini başlatın.
* Yeni oluşturulan sunucu (geri yüklenmiş/çoğaltma), benzersiz kimliği henüz Azure Key Vault izin verilmediği için erişilemeyen bir durum tutulur (AKV)
* Geri yüklenen/çoğaltma sunucusunda, yeni oluşturulan sunucuya, AKV 'de depolanan anahtara sarmalama/sarmalama izinleri verildiğinden emin olmak için veri şifreleme ayarlarındaki müşteri tarafından yönetilen anahtarı yeniden doğrulayın.

* Veri şifrelemenin ana bilgisayarda ve geri yüklenmiş/Çoğaltma sunucusunda korunduğundan emin olmak için yukarıdaki adımların gerçekleştirilmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Portal](howto-data-encryption-portal.md)kullanarak, PostgreSQL için Azure veritabanınız için müşteri tarafından yönetilen anahtarla veri şifrelemeyi ayarlamayı öğrenin.
