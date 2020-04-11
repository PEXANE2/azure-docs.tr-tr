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
ms.date: 04/10/2020
ms.openlocfilehash: 87abdb37ff7773b0205a2ce3ee21689a10c459d7
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113543"
---
# <a name="transparent-data-encryption-for-sql-database-and-azure-synapse"></a>SQL Veritabanı ve Azure Synapse için saydam veri şifreleme

[Saydam veri şifreleme (TDE),](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) Azure Synapse Analytics'teki Azure SQL Veritabanı, Azure SQL Yönetilen Örneği ve Synapse SQL'in, verileri istirahatte şifreleyerek kötü amaçlı çevrimdışı etkinlik tehdidine karşı korunmasına yardımcı olur. Bu özellik bütün bir veritabanı, yedekleri ve işlem günlüğü dosyaları için gerçek zamanlı şifreleme ve şifre çözme işlemlerini gerçekleştirir ve uygulamada değişiklik yapmayı gerektirmez. Varsayılan olarak, TDE yeni dağıtılan tüm Azure SQL veritabanları için etkinleştirilir ve Azure SQL Veritabanı, Azure SQL Yönetilen Örneği veya Azure Synapse'nin eski veritabanları için el ile etkinleştirilmesi gerekir.

TDE, sayfa düzeyinde verilerin gerçek zamanlı G/Ç şifrelemesini ve şifresini çözmeyi gerçekleştirir. Okunarak belleğe alınan her sayfanın şifresi çözülür ve sayfalar diske yazılmadan önce şifrelenir. TDE, Veritabanı Şifreleme Anahtarı (DEK) adı verilen simetrik bir anahtar kullanarak tüm veritabanının depolanmasını şifreler. Veritabanı nın başlatılmasında, şifrelenmiş DEK şifresi çözülür ve SQL Server Database Engine işlemindeki veritabanı dosyalarının şifresini çözmek ve yeniden şifrelemek için kullanılır. DEK, TDE koruyucusu tarafından korunmaktadır. TDE koruyucusu, hizmet tarafından yönetilen bir sertifika (hizmet tarafından yönetilen saydam veri şifrelemesi) veya [Azure Key Vault'ta](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault) depolanan asimetrik bir anahtardır (müşteri tarafından yönetilen saydam veri şifreleme). 

Azure SQL Veritabanı ve Azure Synapse için, TDE koruyucusu mantıksal SQL sunucu düzeyinde ayarlanır ve bu sunucuyla ilişkili tüm veritabanları tarafından devralınır. Azure SQL Yönetilen Örnek (önizlemedeki BYOK özelliği) için, TDE koruyucusu örnek düzeyinde ayarlanır ve bu örnekteki tüm şifrelenmiş veritabanları tarafından devredilir. *Sunucu* terimi, farklı belirtilmediği sürece, bu belge boyunca hem sunucuhem de örnek anlamına gelir.

> [!IMPORTANT]
> Yeni oluşturulan tüm Azure SQL veritabanları, hizmet tarafından yönetilen saydam veri şifrelemesi kullanılarak varsayılan olarak şifrelenir. Mayıs 2017'den önce oluşturulan varolan SQL veritabanları ve geri yükleme, coğrafi çoğaltma ve veritabanı kopyalama yoluyla oluşturulan SQL veritabanları varsayılan olarak şifrelenmez. Şubat 2019'dan önce oluşturulan Varolan Yönetilen Örnek veritabanları varsayılan olarak şifrelenmez. Kaynaktan devralma şifreleme durumunu geri yükleyerek oluşturulan Yönetilen Örnek veritabanları.

> [!NOTE]
> TDE, SQL Veritabanı'ndaki mantıksal **ana** veritabanını şifrelemek için kullanılamaz.  **Ana** veritabanı, kullanıcı veritabanlarında TDE işlemlerini gerçekleştirmek için gereken nesneleri içerir.


## <a name="service-managed-transparent-data-encryption"></a>Hizmet tarafından yönetilen saydam veri şifreleme

Azure'da, TDE için varsayılan ayar DEK'nin yerleşik bir sunucu sertifikası tarafından korunmasıdır. Yerleşik sunucu sertifikası her sunucu için benzersizdir ve kullanılan şifreleme algoritması AES 256'dır. Bir veritabanı coğrafi çoğaltma ilişkisindeyse, birincil ve ikincil veritabanları birincil veritabanının ana sunucu anahtarı tarafından korunur. İki veritabanı aynı sunucuya bağlıysa, aynı yerleşik sertifikayı da paylaşırlar.  Microsoft bu sertifikaları iç güvenlik ilkesine uygun olarak otomatik olarak döndürür ve kök anahtar bir Microsoft dahili gizli deposu tarafından korunur.  Müşteriler, [Microsoft Güven Merkezi'nde](https://servicetrust.microsoft.com/)bulunan bağımsız üçüncü taraf denetim raporlarında IÇ güvenlik ilkeleriyle SQL Veritabanı uyumluluğunu doğrulayabilir.

Microsoft ayrıca, coğrafi çoğaltma ve geri yükleme için gerektiğinde anahtarları sorunsuz bir şekilde taşır ve yönetir.


## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>Müşteri tarafından yönetilen şeffaf veri şifreleme - Kendi Anahtarınızı Getir

Müşteri tarafından yönetilen TDE, TDE için Bring Your Own Key (BYOK) desteği olarak da adlandırılır. Bu senaryoda, DEK'i şifreleyen TDE Koruyucusu, müşteriye ait ve yönetilen Azure Key Vault'ta (Azure'un bulut tabanlı dış anahtar yönetim sistemi) depolanan ve anahtar kasasından asla çıkmayan müşteri tarafından yönetilen asimetrik bir anahtardır. TDE [Koruyucu, anahtar kasası tarafından oluşturulabilir veya](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) şirket içi donanım güvenlik modülü (HSM) aygıtından anahtar kasasına aktarılabilir. SQL Veritabanı'na DEK'in şifresini çözmek ve şifrelemek için müşteriye ait anahtar kasasına izin verilmesi gerekir. Anahtar kasasına mantıksal SQL sunucusunun izinleri iptal edilirse, veritabanına erişilemez ve tüm veriler şifrelenir

Azure Key Vault entegrasyonu ile TDE ile kullanıcılar, anahtar döndürmeler, anahtar kasa izinleri, anahtar yedeklemeleri gibi önemli yönetim görevlerini denetleyebilir ve Azure Key Vault işlevini kullanarak tüm TDE koruyucularında denetim/raporlama yı etkinleştirebilir. Key Vault merkezi anahtar yönetimi sağlar, sıkı bir şekilde izlenen HSM'lerden yararlanır ve güvenlik ilkelerine uygunluğu karşılamaya yardımcı olmak için anahtarların yönetimi ile verilerin yönetimi arasında görev ayrılmasını sağlar.
Azure SQL Veritabanı ve Azure Synapse için BYOK hakkında daha fazla bilgi edinmek için [Azure Key Vault tümleştirmesiyle Saydam veri](transparent-data-encryption-byok-azure-sql.md)şifrelemesi'ne bakın.

Azure Key Vault tümleştirmesiyle TDE'yi kullanmaya başlamak için [Key Vault'tan kendi anahtarınızı kullanarak saydam veri şifrelemeyi nasıl aç'ı kılavuzuna](transparent-data-encryption-byok-azure-sql-configure.md)bakın.

## <a name="move-a-transparent-data-encryption-protected-database"></a>Saydam veri şifreleme korumalı veritabanını taşıma

Azure'daki işlemler için veritabanlarının şifresini çözmeniz gerekmez. Kaynak veritabanındaki veya birincil veritabanındaki TDE ayarları, hedefte saydam olarak devralınır. Dahil edilen işlemler şunlardır:

- Coğrafi Geri Yükleme
- Self servis nokta-in-time geri yükleme
- Silinen bir veritabanının geri yüklemesi
- Etkin coğrafi çoğaltma
- Veritabanı kopyasının oluşturulması
- Yedekleme dosyasını Azure SQL Yönetilen Örneği'ne geri yükleme

> [!IMPORTANT]
> Şifreleme için kullanılan sertifikaya erişilemediğinden, hizmet tarafından yönetilen TDE tarafından şifrelenmiş bir veritabanının manuel COPY-ONLY yedeklemesini almak Azure SQL Yönetilen Örneği'nde izin verilmez. Bu veritabanı türünü başka bir Yönetilen Örnek'e taşımak için zaman içinde geri yükleme özelliğini kullanın.

TDE korumalı bir veritabanı dışa aktardığınızda, veritabanının dışa aktarılan içeriği şifrelenmez. Bu dışa aktarılan içerik şifrelenmemiş BACPAC dosyalarında depolanır. BACPAC dosyalarını uygun şekilde koruduğumdan ve yeni veritabanının aktarılması tamamlandıktan sonra TDE'yi etkinleştirdiğinden emin olun.

Örneğin, BACPAC dosyası şirket içi BIR SQL Server örneğinden dışa aktarılırsa, yeni veritabanının içe aktarılan içeriği otomatik olarak şifrelenmez. Aynı şekilde, BACPAC dosyası şirket içi BIR SQL Server örneğine dışa aktarılırsa, yeni veritabanı da otomatik olarak şifrelenmez.

Bunun tek istisnası, sql veritabanına ve sql veritabanına dışa aktarma nızdır. TDE yeni veritabanında etkindir, ancak BACPAC dosyasının kendisi hala şifrelenmemiştir.


## <a name="manage-transparent-data-encryption"></a>Saydam veri şifrelemeyi yönetme
# <a name="portal"></a>[Portal](#tab/azure-portal)
Azure portalında TDE'yi yönetin.

Azure portalı üzerinden TDE'yi yapılandırmak için Azure Sahibi, Katılımcı veya SQL Güvenlik Yöneticisi olarak bağlanmanız gerekir.

TDE'yi veritabanı düzeyinde açıp kapatırsınız. Bir veritabanında TDE'yi etkinleştirmek için [Azure portalına](https://portal.azure.com) gidin ve Azure Yöneticisi veya Katılımcı hesabınızla oturum açın. Kullanıcı veritabanınızın altındaki TDE ayarlarını bulun. Varsayılan olarak, hizmet tarafından yönetilen saydam veri şifreleme kullanılır. Veritabanını içeren sunucu için otomatik olarak bir TDE sertifikası oluşturulur. Azure SQL Yönetilen Örnek için TDE'yi bir veritabanında açıp kapatmak için T-SQL'i kullanın.

![Hizmet tarafından yönetilen saydam veri şifreleme](./media/transparent-data-encryption-azure-sql/service-managed-transparent-data-encryption.png)  

TDE koruyucusu olarak bilinen TDE ana anahtarını sunucu düzeyinde ayarlarsınız. BYOK desteği ile TDE'yi kullanmak ve Veritabanınızı Key Vault'tan bir anahtarla korumak için sunucunuzun altındaki TDE ayarlarını açın.

![Bring Your Own Key desteği ile saydam veri şifreleme](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
PowerShell'i kullanarak TDE'yi yönetin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

TDE'yi PowerShell aracılığıyla yapılandırmak için Azure Sahibi, Katılımcı veya SQL Güvenlik Yöneticisi olarak bağlanmanız gerekir.

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
> Azure SQL Yönetilen Örneği için, TDE'yi veritabanı düzeyinde açıp kapatmak için T-SQL [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) komutunu kullanın ve TDE'yi örnek düzeyinde yönetmek için [örnek PowerShell komut dosyasını](transparent-data-encryption-byok-azure-sql-configure.md) denetleyin.

# <a name="transact-sql"></a>[Transact-SQL](#tab/azure-TransactSQL)
Transact-SQL kullanarak TDE'yi yönetin.

Ana veritabanındaki **dbmanager** rolünün yöneticisi veya üyesi olan bir giriş kullanarak veritabanına bağlanın.

| Komut | Açıklama |
| --- | --- |
| [ALTER DATABASE (Azure SQL Veritabanı)](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | ŞIFRELEME A/KAPAMA'YI AYARLA |
| [_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |Veritabanının şifreleme durumu ve ilişkili veritabanı şifreleme anahtarları hakkında bilgi verir |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |Her Azure Synapse düğümünün şifreleme durumu ve ilişkili veritabanı şifreleme anahtarları hakkında bilgi verir |
|  | |

Transact-SQL kullanarak TDE koruyucuyu Key Vault'tan bir anahtara çeviremezsiniz. PowerShell'i veya Azure portalını kullanın.

# <a name="rest-api"></a>[REST API](#tab/azure-RESTAPI)
REST API'sini kullanarak TDE'yi yönetin.

TDE'yi REST API'si aracılığıyla yapılandırmak için Azure Sahibi, Katılımcı veya SQL Güvenlik Yöneticisi olarak bağlanmanız gerekir.
Azure SQL Veritabanı ve Azure Synapse için aşağıdaki komut kümesini kullanın:

| Komut | Açıklama |
| --- | --- |
|[Sunucu Oluşturma veya Güncelleme](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|SQL Server örneğine Azure Active Directory kimliği ekler (Key Vault'a erişim sağlamak için kullanılır)|
|[Sunucu Anahtarı Oluşturma veya Güncelleme](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|SQL Server örneğine Anahtar Kasa sı ekleniyor|
|[Sunucu Anahtarını Sil](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|SQL Server örneğinden Key Vault anahtarını kaldırır|
|[Sunucu Anahtarlarını Al](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|SQL Server örneğinden belirli bir Key Vault anahtarı nı alır|
|[Sunucu Anahtarlarını Sunucuya Göre Listele](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|SQL Server örneği için Anahtar Kasası tuşlarını alır |
|[Şifreleme Koruyucusu Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|Bir SQL Server örneği için TDE koruyucusu ayarlar|
|[Şifreleme Koruyucusu Alın](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|BIR SQL Server örneği için TDE koruyucusu alır|
|[Şifreleme Koruyucularını Sunucuya Göre Listele](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|BIR SQL Server örneği için TDE koruyucularını alır |
|[Saydam Veri Şifreleme Yapılandırması Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|Bir veritabanı için TDE'yi etkinleştirir veya devre dışı|
|[Saydam Veri Şifreleme Yapılandırması Alın](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|Veritabanı için TDE yapılandırmasını alır|
|[Şeffaf Veri Şifreleme Yapılandırma Sonuçlarını Listele](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|Veritabanı için şifreleme sonucunu alır|

## <a name="see-also"></a>Ayrıca Bkz.
- Azure sanal makinesinde çalışan SQL Server, Key Vault'tan asimetrik bir anahtar da kullanabilir. Yapılandırma adımları, SQL Veritabanı ve SQL Yönetilen Örnek'te asimetrik bir anahtar kullanmaktan farklıdır. Daha fazla bilgi için [Azure Key Vault (SQL Server) kullanarak Genişletilebilir anahtar yönetimine](https://docs.microsoft.com/sql/relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server)bakın.
- TDE'nin genel açıklaması için [Bkz. Saydam veri şifreleme.](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption)
- Azure SQL Veritabanı, Azure SQL Yönetilen Örneği ve Azure Synapse için BYOK desteği ile TDE hakkında daha fazla bilgi edinmek için [Kendi Anahtarınızı Getir desteğiyle Saydam veri şifrelemebölümüne](transparent-data-encryption-byok-azure-sql.md)bakın.
- Kendi Anahtarınızı Getir desteğiyle TDE'yi kullanmaya başlamak için, Key [Vault'tan kendi anahtarınızı kullanarak nasıl](transparent-data-encryption-byok-azure-sql-configure.md)yapılacağınız kılavuzuna bakın, saydam veri şifrelemeyi açın.
- Key Vault hakkında daha fazla bilgi için, [bir anahtar kasasına Güvenli erişim](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault)bakın.
