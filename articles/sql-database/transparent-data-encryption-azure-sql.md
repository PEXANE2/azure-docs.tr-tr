---
title: Saydam veri şifrelemesi
description: Azure Synapse Analytics'te SQL Veritabanı ve Synapse SQL için saydam veri şifrelemesine genel bakış. Belge, hizmet tarafından yönetilen saydam veri şifreleme ve Kendi Anahtarını Getir'i içeren yapılandırma seçeneklerini ve avantajlarını kapsar.
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 02/06/2020
ms.openlocfilehash: 982a59f1eb8717e2fe2d86728cdae731c919aaf0
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476956"
---
# <a name="transparent-data-encryption-for-sql-database-and-azure-synapse"></a>SQL Veritabanı ve Azure Synapse için saydam veri şifreleme

Saydam veri şifreleme (TDE), Azure Synapse Analytics'teki Azure SQL Veritabanı, Azure SQL Yönetilen Örneği ve Synapse SQL'in, verileri istirahatte şifreleyerek kötü amaçlı çevrimdışı etkinlik tehdidine karşı korunmasına yardımcı olur. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez. TDE varsayılan olarak yeni dağıtılan tüm Azure SQL veritabanlarında etkindir. TDE, SQL Veritabanı'ndaki mantıksal **ana** veritabanını şifrelemek için kullanılamaz.  **Ana** veritabanı, kullanıcı veritabanlarında TDE işlemlerini gerçekleştirmek için gereken nesneleri içerir.

Azure SQL Veritabanı, Azure SQL Yönetilen Örneği veya Azure Synapse'nin eski veritabanları için TDE'nin el ile etkinleştirilmesi gerekir.
Kaynak veritabanından devralma şifreleme durumunu geri yükleyerek oluşturulan Yönetilen Örnek veritabanları.

Saydam veri şifreleme, veritabanı şifreleme anahtarı adı verilen simetrik bir anahtar kullanarak tüm veritabanının depolanmasını şifreler. Bu veritabanı şifreleme anahtarı saydam veri şifreleme koruyucusu tarafından korunmaktadır. Koruyucu, hizmet tarafından yönetilen bir sertifika (hizmet tarafından yönetilen saydam veri şifreleme) veya Azure Key Vault'ta (Kendi Anahtarınızı Getir) depolanan asimetrik bir anahtardır. Saydam veri şifreleme koruyucuyu Azure SQL Veritabanı ve Azure Synapse için sunucu düzeyinde ve Azure SQL Yönetilen Örneği için örnek düzeyinde ayarlarsınız. *Sunucu* terimi, farklı belirtilmediği sürece, bu belge boyunca hem sunucuhem de örnek anlamına gelir.

Veritabanı başlatmada, şifrelenmiş veritabanı şifreleme anahtarı şifrelenir ve sql server veritabanı altyapısı işlemindeki veritabanı dosyalarının şifresini çözmek ve yeniden şifrelemek için kullanılır. Saydam veri şifreleme, sayfa düzeyinde gerçek zamanlı G/Ç şifrelemesi ve verilerin şifresini çözme yi gerçekleştirir. Okunarak belleğe alınan her sayfanın şifresi çözülür ve sayfalar diske yazılmadan önce şifrelenir. Saydam veri şifrelemesinin genel açıklaması için [bkz.](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption)

Azure sanal makinesinde çalışan SQL Server, Key Vault'tan asimetrik bir anahtar da kullanabilir. Yapılandırma adımları, SQL Veritabanı ve SQL Yönetilen Örnek'te asimetrik bir anahtar kullanmaktan farklıdır. Daha fazla bilgi için [Azure Key Vault (SQL Server) kullanarak Genişletilebilir anahtar yönetimine](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server)bakın.

## <a name="service-managed-transparent-data-encryption"></a>Hizmet tarafından yönetilen saydam veri şifreleme

Azure'da saydam veri şifreleme için varsayılan ayar, veritabanı şifreleme anahtarının yerleşik bir sunucu sertifikası tarafından korunmasıdır. Yerleşik sunucu sertifikası her sunucu için benzersizdir ve kullanılan şifreleme algoritması AES 256'dır. Bir veritabanı coğrafi çoğaltma ilişkisindeyse, birincil ve ikincil veritabanı birincil veritabanının ana sunucu anahtarı tarafından korunur. İki veritabanı aynı sunucuya bağlıysa, aynı yerleşik sertifikayı da paylaşırlar.  Microsoft bu sertifikaları iç güvenlik ilkesine uygun olarak otomatik olarak döndürür ve kök anahtar bir Microsoft dahili gizli deposu tarafından korunur.  Müşteriler, [Microsoft Güven Merkezi'nde](https://servicetrust.microsoft.com/)bulunan bağımsız üçüncü taraf denetim raporlarında IÇ güvenlik ilkeleriyle SQL Veritabanı uyumluluğunu doğrulayabilir.

Microsoft ayrıca, coğrafi çoğaltma ve geri yükleme için gerektiğinde anahtarları sorunsuz bir şekilde taşır ve yönetir.

> [!IMPORTANT]
> Yeni oluşturulan tüm SQL veritabanları ve Yönetilen Örnek veritabanları, hizmet tarafından yönetilen saydam veri şifrelemesi kullanılarak varsayılan olarak şifrelenir. Mayıs 2017'den önce oluşturulan varolan SQL veritabanları ve geri yükleme, coğrafi çoğaltma ve veritabanı kopyalama yoluyla oluşturulan SQL veritabanları varsayılan olarak şifrelenmez. Şubat 2019'dan önce oluşturulan Varolan Yönetilen Örnek veritabanları varsayılan olarak şifrelenmez. Kaynaktan devralma şifreleme durumunu geri yükleyerek oluşturulan Yönetilen Örnek veritabanları.

## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Müşteri tarafından yönetilen şeffaf veri şifreleme - Kendi Anahtarınızı Getir

[Azure Key Vault'ta müşteri tarafından yönetilen anahtarlara sahip TDE,](transparent-data-encryption-byok-azure-sql.md) Veritabanı Şifreleme Anahtarını (DEK) TDE Protector adlı müşteri tarafından yönetilen asimetrik anahtarla şifrelemeye olanak tanır.  Bu, genellikle Saydam Veri Şifrelemesi için Kendi Anahtarınızı Getir (BYOK) desteği olarak da adlandırılır. BYOK senaryosunda, TDE Koruyucusu, Azure'un bulut tabanlı dış anahtar yönetim sistemi olan müşteriye ait ve yönetilen [Azure Key Vault'ta](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)depolanır. TDE [Koruyucusu anahtar kasası tarafından oluşturulabilir veya](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys) şirket içi bir HSM cihazından anahtar kasasına aktarılabilir. Bir veritabanının önyükleme sayfasında depolanan TDE DEK, Azure Key Vault'ta depolanan ve anahtar kasasından asla çıkmayan TDE Protector tarafından şifrelenir ve şifresi çözülür.  SQL Veritabanı'na DEK'in şifresini çözmek ve şifrelemek için müşteriye ait anahtar kasasına izin verilmesi gerekir. Mantıksal SQL sunucusunun anahtar kasasına olan izinleri iptal edilirse, veritabanına erişilemez ve tüm veriler şifrelenir. Azure SQL Veritabanı için, TDE koruyucusu mantıksal SQL sunucu düzeyinde ayarlanır ve bu sunucuyla ilişkili tüm veritabanları tarafından devralınır. [Azure SQL Yönetilen Örneği](https://docs.microsoft.com/azure/sql-database/sql-database-howto-managed-instance)için, TDE koruyucusu örnek düzeyinde ayarlanır ve bu örnekteki tüm *şifrelenmiş* veritabanları tarafından devralır. *Sunucu* terimi, farklı belirtilmediği sürece, bu belge boyunca hem sunucuhem de örnek anlamına gelir.

Azure Key Vault entegrasyonu ile TDE ile kullanıcılar, anahtar döndürmeler, anahtar kasa izinleri, anahtar yedeklemeleri gibi önemli yönetim görevlerini denetleyebilir ve Azure Key Vault işlevini kullanarak tüm TDE koruyucularında denetim/raporlama yı etkinleştirebilir. Key Vault merkezi anahtar yönetimi sağlar, sıkı bir şekilde izlenen donanım güvenlik modüllerinden (HSM) yararlanır ve güvenlik ilkeleriyle uyumluluğu karşılamaya yardımcı olmak için anahtarların yönetimi ile veri arasında görev ayrımını sağlar.
Azure SQL Veritabanı, SQL Yönetilen Örneği ve Azure Synapse için Azure Key Vault tümleştirmesi (Kendi Anahtar desteğinizi getir) ile saydam veri şifreleme hakkında daha fazla bilgi edinmek için [Azure Anahtar Kasası tümleştirmesiyle Saydam veri](transparent-data-encryption-byok-azure-sql.md)şifrelemesi'ne bakın.

Azure Key Vault tümleştirmesiyle saydam veri şifrelemekullanmaya başlamak için (Kendi Anahtar Desteğinizi Getirin) [PowerShell'i kullanarak Key Vault'tan kendi anahtarınızı kullanarak saydam veri şifrelemeyi](transparent-data-encryption-byok-azure-sql-configure.md)nasıl açar kılavuzuna bakın.

## <a name="move-a-transparent-data-encryption-protected-database"></a>Saydam veri şifreleme korumalı veritabanını taşıma

Azure'daki işlemler için veritabanlarının şifresini çözmeniz gerekmez. Kaynak veritabanındaki veya birincil veritabanındaki saydam veri şifreleme ayarları, hedefte saydam olarak devralınır. Dahil edilen işlemler şunlardır:

- Coğrafi Geri Yükleme
- Self servis nokta-in-time geri yükleme
- Silinen bir veritabanının geri yüklemesi
- Etkin coğrafi çoğaltma
- Veritabanı kopyasının oluşturulması
- Yedekleme dosyasını Azure SQL Yönetilen Örneği'ne geri yükleme

> [!IMPORTANT]
> Şifreleme için kullanılan sertifikaya erişilemediğinden, hizmet tarafından yönetilen TDE tarafından şifrelenmiş bir veritabanının manuel COPY-ONLY yedeklemesini almak Azure SQL Yönetilen Örneği'nde izin verilmez. Bu veritabanı türünü başka bir Yönetilen Örnek'e taşımak için zaman içinde geri yükleme özelliğini kullanın.

Saydam bir veri şifreleme korumalı veritabanı dışa aktardığınızda, veritabanının dışa aktarılan içeriği şifrelenmez. Bu dışa aktarılan içerik, şifrelenmemiş BACPAC dosyalarında depolanır. BACPAC dosyalarını uygun şekilde koruduğumdan ve yeni veritabanının aktarılması tamamlandıktan sonra saydam veri şifrelemesini etkinleştirdiğinden emin olun.

Örneğin, BACPAC dosyası şirket içi BIR SQL Server örneğinden dışa aktarılırsa, yeni veritabanının içe aktarılan içeriği otomatik olarak şifrelenmez. Aynı şekilde, BACPAC dosyası şirket içi BIR SQL Server örneğine dışa aktarılırsa, yeni veritabanı da otomatik olarak şifrelenmez.

Bunun tek istisnası, sql veritabanına ve sql veritabanına dışa aktarma nızdır. Yeni veritabanında saydam veri şifreleme etkindir, ancak BACPAC dosyasının kendisi hala şifrelenmemiştir.


## <a name="manage-transparent-data-encryption"></a>Saydam veri şifrelemeyi yönetme
# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalında saydam veri şifrelemeyi yönetin.

Azure portalı üzerinden saydam veri şifrelemesi yapılandırmak için Azure Sahibi, Katılımcı veya SQL Güvenlik Yöneticisi olarak bağlanmanız gerekir.

Saydam veri şifrelemesini veritabanı düzeyinde açıp kapatırsınız. Bir veritabanında saydam veri şifrelemesi etkinleştirmek için [Azure portalına](https://portal.azure.com) gidin ve Azure Yöneticisi veya Katılımcı hesabınızla oturum açın. Kullanıcı veritabanınızın altındaki saydam veri şifreleme ayarlarını bulun. Varsayılan olarak, hizmet tarafından yönetilen saydam veri şifreleme kullanılır. Veritabanını içeren sunucu için saydam bir veri şifreleme sertifikası otomatik olarak oluşturulur. Azure SQL Yönetilen Örnek için, saydam veri şifrelemesini bir veritabanında açıp kapatmak için T-SQL'i kullanın.

![Hizmet tarafından yönetilen saydam veri şifreleme](./media/transparent-data-encryption-azure-sql/service-managed-transparent-data-encryption.png)

Saydam veri şifreleme ana anahtarını sunucu düzeyinde saydam veri şifreleme koruyucusu olarak da bilinirsiniz. Kendi Anahtarınızı Getir desteği yle saydam veri şifrelemesi kullanmak ve veritabanlarınızı Key Vault'tan bir anahtarla korumak için sunucunuzun altındaki saydam veri şifreleme ayarlarını açın.

![Bring Your Own Key desteği ile saydam veri şifreleme](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell'i kullanarak saydam veri şifrelemeyi yönetin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

PowerShell aracılığıyla saydam veri şifrelemesini yapılandırmak için Azure Sahibi, Katılımcı veya SQL Güvenlik Yöneticisi olarak bağlanmanız gerekir.

### <a name="cmdlets-for-azure-sql-database-and-azure-synapse"></a>Azure SQL Veritabanı ve Azure Synapse için Cmdlets

Azure SQL Veritabanı ve Azure Synapse için aşağıdaki cmdlets'i kullanın:

| Cmdlet | Açıklama |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |Veritabanı için saydam veri şifrelemesini etkinleştirir veya devre dışı|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |Veritabanı için saydam veri şifreleme durumunu alır |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |Veritabanı için şifreleme ilerlemesini denetler |
| [Ekle-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |SQL Server örneğine Anahtar Kasa sı ekleniyor |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |Azure SQL Veritabanı sunucusunun Anahtar Kasası anahtarlarını alır  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |SQL Server örneği için saydam veri şifreleme koruyucusu ayarlar |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |Saydam veri şifreleme koruyucusu alır |
| [Remove-azsqlserverkeyvaultkey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |SQL Server örneğinden Key Vault anahtarını kaldırır |
|  | |

> [!IMPORTANT]
> Azure SQL Yönetilen Örnek için, veritabanı düzeyinde saydam veri şifrelemesini açıp kapatmak için T-SQL [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) komutunu kullanın ve örnek düzeyinde saydam veri şifrelemesini yönetmek için [örnek PowerShell komut dosyasını](transparent-data-encryption-byok-azure-sql-configure.md) denetleyin.

# <a name="transact-sql"></a>[Transact-SQL](#tab/azure-TransactSQL)
Transact-SQL kullanarak saydam veri şifrelemeyi yönetin.

Ana veritabanındaki **dbmanager** rolünün yöneticisi veya üyesi olan bir giriş kullanarak veritabanına bağlanın.

| Komut | Açıklama |
| --- | --- |
| [ALTER DATABASE (Azure SQL Veritabanı)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | ŞIFRELEME A/KAPAMA'YI AYARLA |
| [_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Veritabanının şifreleme durumu ve ilişkili veritabanı şifreleme anahtarları hakkında bilgi verir |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Her veri ambarı düğümünün şifreleme durumu ve ilişkili veritabanı şifreleme anahtarları hakkında bilgi verir |
|  | |

Transact-SQL kullanarak saydam veri şifreleme koruyucuyu Key Vault'tan bir anahtara çeviremezsiniz. PowerShell'i veya Azure portalını kullanın.

# <a name="rest-api"></a>[REST API](#tab/azure-RESTAPI)
REST API'sini kullanarak saydam veri şifrelemeyi yönetin.

Saydam veri şifrelemesini REST API'si aracılığıyla yapılandırmak için Azure Sahibi, Katılımcı veya SQL Güvenlik Yöneticisi olarak bağlanmanız gerekir.
Azure SQL Veritabanı ve Azure Synapse için aşağıdaki komut kümesini kullanın:

| Komut | Açıklama |
| --- | --- |
|[Sunucu Oluşturma veya Güncelleme](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|SQL Server örneğine Azure Active Directory kimliği ekler (Key Vault'a erişim sağlamak için kullanılır)|
|[Sunucu Anahtarı Oluşturma veya Güncelleme](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|SQL Server örneğine Anahtar Kasa sı ekleniyor|
|[Sunucu Anahtarını Sil](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|SQL Server örneğinden Key Vault anahtarını kaldırır|
|[Sunucu Anahtarlarını Al](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|SQL Server örneğinden belirli bir Key Vault anahtarı nı alır|
|[Sunucu Anahtarlarını Sunucuya Göre Listele](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|SQL Server örneği için Anahtar Kasası tuşlarını alır |
|[Şifreleme Koruyucusu Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|SQL Server örneği için saydam veri şifreleme koruyucusu ayarlar|
|[Şifreleme Koruyucusu Alın](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|SQL Server örneği için saydam veri şifreleme koruyucusu alır|
|[Şifreleme Koruyucularını Sunucuya Göre Listele](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|SQL Server örneği için saydam veri şifreleme koruyucularını alır |
|[Saydam Veri Şifreleme Yapılandırması Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Veritabanı için saydam veri şifrelemesini etkinleştirir veya devre dışı|
|[Saydam Veri Şifreleme Yapılandırması Alın](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Veritabanı için saydam veri şifreleme yapılandırmasını alır|
|[Şeffaf Veri Şifreleme Yapılandırma Sonuçlarını Listele](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Veritabanı için şifreleme sonucunu alır|

## <a name="next-steps"></a>Sonraki adımlar

- Saydam veri şifrelemesinin genel açıklaması için [bkz.](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption)
- Azure SQL Veritabanı, Azure SQL Yönetilen Örneği ve Azure Synapse için Kendi Anahtar desteğinizi getir ile saydam veri şifreleme hakkında daha fazla bilgi edinmek [için, Kendi Anahtarınızı Getir desteğiyle Saydam veri](transparent-data-encryption-byok-azure-sql.md)şifrelemesi'ne bakın.
- Kendi Anahtarınızı Getir desteğiyle saydam veri şifrelemekullanmaya başlamak için [PowerShell'i kullanarak Key Vault'tan kendi anahtarınızı kullanarak saydam veri şifrelemeyi](transparent-data-encryption-byok-azure-sql-configure.md)nasıl aç'ı kılavuzuna bakın.
- Key Vault hakkında daha fazla bilgi için [Key Vault belgeleri sayfasına](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)bakın.
