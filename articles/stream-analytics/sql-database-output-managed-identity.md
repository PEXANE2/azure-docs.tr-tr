---
title: Azure SQL veritabanı 'na veya Azure SYNAPSE Analytics 'e erişmek için Yönetilen kimlikler kullanma-Azure Stream Analytics
description: Bu makalede, Azure SQL veritabanı veya Azure SYNAPSE Analytics çıkışındaki Azure Stream Analytics işinizin kimliğini doğrulamak için yönetilen kimliklerin nasıl kullanılacağı açıklanır.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: how-to
ms.date: 11/30/2020
ms.openlocfilehash: 7d624f2dd2c0c9b4c7e99d5628a1d47e4303da7f
ms.sourcegitcommit: 6628bce68a5a99f451417a115be4b21d49878bb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2021
ms.locfileid: "98555602"
---
# <a name="use-managed-identities-to-access-azure-sql-database-or-azure-synapse-analytics-from-an-azure-stream-analytics-job-preview"></a>Azure Stream Analytics işinden Azure SQL veritabanı 'na veya Azure SYNAPSE Analytics 'e erişmek için Yönetilen kimlikler kullanma (Önizleme)

Azure Stream Analytics, Azure SQL veritabanı ve Azure SYNAPSE Analytics çıkış havuzları için [yönetilen kimlik kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/overview.md) destekler. Yönetilen kimlikler, Kullanıcı tabanlı kimlik doğrulama yöntemlerinin sınırlamalarını ortadan kaldırarak, parola değişikliklerinden veya her 90 günde bir gerçekleşen Kullanıcı belirteci süre sonu nedeniyle yeniden kimlik doğrulaması yapmanız gerekir. El ile kimlik doğrulaması gereksinimini kaldırdığınızda Stream Analytics dağıtımlarınız tamamen otomatikleştirilebilir.

Yönetilen kimlik, belirli bir Stream Analytics işini temsil eden Azure Active Directory kayıtlı yönetilen bir uygulamadır. Yönetilen uygulama, hedeflenen bir kaynağın kimliğini doğrulamak için kullanılır. Bu makalede, Azure portal aracılığıyla bir Azure SQL veritabanı için yönetilen kimliğin veya bir Stream Analytics işinin Azure SYNAPSE Analytics çıkışının nasıl etkinleştirileceği gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

#### <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/azure-sql)

Bu özelliği kullanmak için aşağıdakiler gereklidir:

- Azure Stream Analytics işi.

- Azure SQL veritabanı kaynağı.

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Bu özelliği kullanmak için aşağıdakiler gereklidir:

- Azure Stream Analytics işi.

- Azure SYNAPSE Analytics SQL Havuzu.

- [Stream Analytics işiniz için yapılandırılmış](azure-synapse-analytics-output.md)bir Azure depolama hesabı.

---

## <a name="create-a-managed-identity"></a>Yönetilen kimlik oluşturma

İlk olarak, Azure Stream Analytics işiniz için yönetilen bir kimlik oluşturursunuz.

1. [Azure Portal](https://portal.azure.com), Azure Stream Analytics işinizi açın.

1. Sol gezinti menüsünde, **Yapılandır** altında bulunan **yönetilen kimlik** ' i seçin. Sonra, **sistem tarafından atanan yönetilen kimliği kullan** ' ın yanındaki kutuyu Işaretleyin ve **Kaydet**' i seçin.

   ![Sistem tarafından atanan yönetilen kimliği seçin](./media/sql-db-output-managed-identity/system-assigned-managed-identity.png)

   Stream Analytics işinin kimliği için bir hizmet sorumlusu Azure Active Directory oluşturulur. Yeni oluşturulan kimliğin yaşam döngüsü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (hizmet sorumlusu) Azure tarafından otomatik olarak silinir.

1. Yapılandırmayı kaydettiğinizde, hizmet sorumlusunun nesne KIMLIĞI (OID) aşağıda gösterildiği gibi asıl KIMLIK olarak listelenir: 

   ![Asıl KIMLIK olarak gösterilen nesne KIMLIĞI](./media/sql-db-output-managed-identity/principal-id.png)

   Hizmet sorumlusu Stream Analytics işle aynı ada sahiptir. Örneğin, işinizin adı *MyASAJob* ise, hizmet sorumlusu adı da *MyASAJob* olur.

## <a name="select-an-active-directory-admin"></a>Active Directory Yöneticisi seçin

Yönetilen bir kimlik oluşturduktan sonra bir Active Directory Yöneticisi seçersiniz.

1. Azure SQL veritabanınıza veya Azure SYNAPSE Analytics kaynağına gidin ve veritabanının altında bulunduğu SQL Server seçin. Kaynak Genel Bakış sayfasında *sunucu adı* ' nın yanındaki SQL Server adı ' nı bulabilirsiniz.

1. **Ayarlar** altında **Active Directory yönetici** ' yi seçin. Ardından, **yönetici ayarla**' yı seçin.

   ![Active Directory yönetici sayfası](./media/sql-db-output-managed-identity/active-directory-admin-page.png)

1. Active Directory yönetici sayfasında, bir kullanıcı veya grup için SQL Server Yöneticisi olacak şekilde arama yapın ve **Seç**' e tıklayın.

   ![Active Directory Yöneticisi ekleme](./media/sql-db-output-managed-identity/add-admin.png)

   Active Directory yönetici sayfası, Active Directory tüm üyelerini ve gruplarını gösterir. Gri olan kullanıcılar veya gruplar Azure Active Directory yönetici olarak desteklenmediği için seçilemez.  ****    [SQL veritabanı veya Azure Synapse kimlik doğrulaması Için Azure Active Directory kimlik doğrulaması kullan](../azure-sql/database/authentication-aad-overview.md#azure-ad-features-and-limitations)' ın Azure Active Directory Özellikler ve sınırlamalar bölümünde desteklenen Yöneticiler listesine bakın. Azure rol tabanlı erişim denetimi (Azure RBAC) yalnızca portala uygulanır ve SQL Server yayılmaz. Ayrıca, seçilen kullanıcı veya Grup, sonraki bölümde **Kapsanan Veritabanı kullanıcısını** oluşturamayacak Kullanıcı olur.

1. **Active Directory yönetici** sayfasında **Kaydet** ' i seçin. Yönetici değiştirme işlemi birkaç dakika sürer.

   Azure Active Directory Yöneticisi 'ni ayarlarken, yeni yönetici adı (Kullanıcı veya grup), sanal birincil veritabanında bir SQL Server kimlik doğrulama kullanıcısı olarak bulunamaz. Varsa, Azure Active Directory yönetici kurulumu başarısız olur ve bir yöneticinin (ad) zaten var olduğunu belirten oluşturma işlemi geri alınır. SQL Server kimlik doğrulama kullanıcısı Azure Active Directory parçası olmadığından, bu kullanıcı için Azure Active Directory kimlik doğrulaması kullanarak sunucuya bağlanma çabaları vardır. 

## <a name="create-a-contained-database-user"></a>Kapsanan veritabanı kullanıcısı oluşturma

Daha sonra, Azure SQL veya Azure SYNAPSE veritabanınızda Azure Active Directory kimliğiyle eşlenmiş bir kapsanan veritabanı kullanıcısı oluşturun. Kapsanan Veritabanı kullanıcısının birincil veritabanı için bir oturum açma işlemi yoktur, ancak veritabanıyla ilişkili dizindeki bir kimlikle eşlenir. Azure Active Directory kimliği, tek bir kullanıcı hesabı veya grup olabilir. Bu durumda, Stream Analytics işiniz için kapsanan bir veritabanı kullanıcısı oluşturmak istersiniz. 

1. SQL Server Management Studio kullanarak Azure SQL veya Azure SYNAPSE veritabanınıza bağlanın. **Kullanıcı adı** , Kullanıcı **değiştirme** iznine sahip Azure Active Directory bir kullanıcı. SQL Server ayarladığınız yönetici bir örnektir. MFA kimlik doğrulamasıyla **Azure Active Directory – Universal** kullanın. 

   ![SQL Server’a bağlanma](./media/sql-db-output-managed-identity/connect-sql-server.png)

   Sunucu adı `<SQL Server name>.database.windows.net` farklı bölgelerde farklı olabilir. Örneğin, Çin bölgesinin kullanması gerekir `<SQL Server name>.database.chinacloudapi.cn` .
 
   Belirli bir Azure SQL veya Azure SYNAPSE veritabanını, **seçeneklere > bağlantı özellikleri > veritabanına** giderek belirtebilirsiniz.  

   ![SQL Server bağlantısı özellikleri](./media/sql-db-output-managed-identity/sql-server-connection-properties.png)

1. İlk kez bağlandığınızda aşağıdaki pencereyle karşılaşabilirsiniz:

   ![Yeni güvenlik duvarı kuralı penceresi](./media/sql-db-output-managed-identity/new-firewall-rule.png)

   1. Bu durumda, Azure portal SQL Server kaynağına gidin. Güvenlik bölümünde **güvenlik** **duvarları ve sanal ağ** sayfasını açın. 
   1. Kural adı olan yeni bir kural ekleyin.
   1. *Başlangıç IP 'si* Için **yeni güvenlik duvarı kuralı** penceresinde *Kimden* IP adresini kullanın.
   1. *Bitiş IP 'si* Için **yeni güvenlik duvarı kuralı** penceresinde *, IP adresini* kullanın. 
   1. **Kaydet** ' i seçin ve SQL Server Management Studio yeniden bağlanmayı deneyin. 

1. Bağlandıktan sonra kapsanan veritabanı kullanıcısını oluşturun. Aşağıdaki SQL komutu, Stream Analytics işle aynı ada sahip bir kapsanan veritabanı kullanıcısı oluşturur. *ASA_JOB_NAME* etrafına köşeli ayraçları eklediğinizden emin olun. Aşağıdaki T-SQL söz dizimini kullanın ve sorguyu çalıştırın. 

   ```sql
   CREATE USER [ASA_JOB_NAME] FROM EXTERNAL PROVIDER; 
   ```

1. Microsoft 'un Stream Analytics işinin SQL veritabanına erişimi olup olmadığını doğrulamak için Azure Active Directory, veritabanıyla iletişim kurmak için Azure Active Directory izni vermemiz gerekir. Bunu yapmak için, yeniden Azure portal "güvenlik duvarları ve sanal ağ" sayfasına gidin ve "Azure hizmetleri ve kaynaklarının bu sunucuya erişmesine Izin ver" seçeneğini etkinleştirin. 

   ![Güvenlik Duvarı ve sanal ağ](./media/sql-db-output-managed-identity/allow-access.png)

## <a name="grant-stream-analytics-job-permissions"></a>Stream Analytics işi izinleri verme

#### <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/azure-sql)

Kapsanan bir veritabanı kullanıcısı oluşturduktan ve önceki bölümde açıklandığı gibi portalda Azure hizmetlerine erişim izni verildiğinde, Stream Analytics işiniz yönetilen kimlik aracılığıyla Azure SQL veritabanı kaynağına, yönetilen kimlik üzerinden **bağlanmak** için bir izne sahiptir. Stream Analytics iş akışında daha sonra gerekli olacağı için Stream Analytics işine seçme ve ekleme izinleri vermenizi öneririz. **Select** izni, ışın Azure SQL veritabanındaki tabloyla bağlantısını test etmesine izin verir. **Ekleme** izni, bir girişi ve Azure SQL veritabanı çıkışını yapılandırdıktan sonra uçtan uca Stream Analytics sorgularının test edilmesine olanak tanır.

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Kapsanan bir veritabanı kullanıcısı oluşturduktan ve önceki bölümde açıklandığı gibi portalda Azure hizmetlerine erişim izni verildiğinde, Stream Analytics işiniz yönetilen kimlik aracılığıyla Azure SYNAPSE veritabanı kaynaklarına, yönetilen kimlik üzerinden **bağlanmak** için bir izne sahiptir. Stream Analytics iş akışında daha sonra gerekli olacağı için Stream Analytics işine VERITABANı toplu IŞLEMLERINI seçme, ekleme ve yönetme izinleri vermenizi öneririz. **Select** izni, Işin Azure SYNAPSE veritabanındaki tabloyla bağlantısını test etmesine izin verir. VERITABANıNı **Ekle** ve **Yönet toplu işlemler** izinleri, bir girişi ve Azure SYNAPSE veritabanı çıktısını yapılandırdıktan sonra uçtan uca Stream Analytics sorgularının test edilmesine olanak tanır.

VERITABANı yönetme Toplu IŞLEMLERI iznini vermek için, Stream Analytics işine [veritabanı izni tarafından ima](/sql/t-sql/statements/grant-database-permissions-transact-sql?view=azure-sqldw-latest&preserve-view=true#remarks) **altında etiketlenen** tüm izinleri vermeniz gerekir. Stream Analytics işi, [Veritabanını Yönet toplu işlemlerini ve eklemeyi](/sql/t-sql/statements/copy-into-transact-sql)gerektiren Copy ifadesini gerçekleştirdiğinden, bu izne ihtiyacınız vardır.

---

SQL Server Management Studio kullanarak Stream Analytics işine bu izinleri verebilirsiniz. Daha fazla bilgi için bkz. GRANT (Transact-SQL) başvurusu.

Yalnızca veritabanındaki belirli bir tablo veya nesneye izin vermek için aşağıdaki T-SQL söz dizimini kullanın ve sorguyu çalıştırın. 

#### <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/azure-sql)

```sql
GRANT SELECT, INSERT ON OBJECT::TABLE_NAME TO ASA_JOB_NAME;
```

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

```sql
GRANT [PERMISSION NAME] OBJECT::TABLE_NAME TO ASA_JOB_NAME;
```

---

Alternatif olarak, SQL Server Management Studio ' de Azure SQL veya Azure SYNAPSE veritabanınıza sağ tıklayıp **özellikler > izinler**' i seçebilirsiniz. İzinler menüsünde, daha önce eklediğiniz Stream Analytics işi görebilir ve uygun gördüğünüz şekilde izinleri el ile verebilir veya reddedebilirsiniz.

## <a name="create-an-azure-sql-database-or-azure-synapse-output"></a>Azure SQL veritabanı veya Azure SYNAPSE çıkışı oluşturma

#### <a name="azure-sql-database"></a>[Azure SQL Veritabanı](#tab/azure-sql)

Yönetilen kimliğiniz yapılandırıldığına göre Stream Analytics işinize bir Azure SQL veritabanı veya Azure SYNAPSE çıkışı eklemeye hazırsınız demektir.

Uygun çıktı şemasıyla SQL veritabanınızda bir tablo oluşturduğunuzdan emin olun. Bu tablonun adı, SQL veritabanı çıktısını Stream Analytics işine eklediğinizde doldurulması gereken özelliklerden biridir. Ayrıca, bağlantıyı test etmek ve Stream Analytics sorguları çalıştırmak için işin **Select** ve **Insert** izinlerine sahip olduğundan emin olun. Daha önce yapmadıysanız [Stream Analytics işi Izinleri verme](#grant-stream-analytics-job-permissions) bölümüne bakın. 

1. Stream Analytics işinize geri dönün ve **Iş topolojisi** altında **çıktılar** sayfasına gidin. 

1. **> SQL veritabanı Ekle**' yi seçin. SQL veritabanı çıkış havuzunun çıkış özellikleri penceresinde, kimlik doğrulama modu açılır listesinden **yönetilen kimlik** ' i seçin.

1. Kalan özellikleri doldurun. SQL veritabanı çıkışı oluşturma hakkında daha fazla bilgi için, bkz. [Stream Analytics BIR SQL veritabanı çıkışı oluşturma](sql-database-output.md). İşiniz bittiğinde **Kaydet**' i seçin.

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Yönetilen kimliğiniz ve depolama hesabınız yapılandırıldığına göre Stream Analytics işinize bir Azure SQL veritabanı veya Azure SYNAPSE çıkışı eklemeye hazırsınız.

Uygun çıktı şemasıyla Azure SYNAPSE veritabanınızda bir tablo oluşturduğunuzdan emin olun. Bu tablonun adı, Azure SYNAPSE çıkışını Stream Analytics işine eklediğinizde doldurulması gereken özelliklerden biridir. Ayrıca, bağlantıyı test etmek ve Stream Analytics sorguları çalıştırmak için işin **Select** ve **Insert** izinlerine sahip olduğundan emin olun. Daha önce yapmadıysanız [Stream Analytics işi Izinleri verme](#grant-stream-analytics-job-permissions) bölümüne bakın.

1. Stream Analytics işinize geri dönün ve **Iş topolojisi** altında **çıktılar** sayfasına gidin.

1. **> Azure SYNAPSE Analytics Ekle**' yi seçin. SQL veritabanı çıkış havuzunun çıkış özellikleri penceresinde, kimlik doğrulama modu açılır listesinden **yönetilen kimlik** ' i seçin.

1. Kalan özellikleri doldurun. Azure SYNAPSE çıkışı oluşturma hakkında daha fazla bilgi edinmek için [Azure Stream Analytics Azure SYNAPSE Analytics çıkışı](azure-synapse-analytics-output.md)bölümüne bakın. İşiniz bittiğinde **Kaydet**' i seçin.

---

## <a name="remove-managed-identity"></a>Yönetilen kimliği kaldır

Stream Analytics işi için oluşturulan yönetilen kimlik yalnızca iş silindiğinde silinir. İşi silmeden yönetilen kimliği silmenin bir yolu yoktur. Artık yönetilen kimliği kullanmak istemiyorsanız, çıkışın kimlik doğrulama yöntemini değiştirebilirsiniz. Yönetilen kimlik, iş silinene kadar mevcut olmaya devam eder ve yönetilen kimlik kimlik doğrulamasını yeniden kullanmaya karar verirseniz kullanılacaktır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics çıkışlarını anlayın](stream-analytics-define-outputs.md)
* [Azure SQL veritabanı 'na Azure Stream Analytics çıkışı](stream-analytics-sql-output-perf.md)
* [Azure Stream Analytics Azure SYNAPSE Analytics çıkışı](azure-synapse-analytics-output.md)
