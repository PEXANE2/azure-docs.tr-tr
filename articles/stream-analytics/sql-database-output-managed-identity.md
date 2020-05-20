---
title: Azure SQL veritabanı 'na erişmek için Yönetilen kimlikler kullanma-Azure Stream Analytics
description: Bu makalede, Azure SQL DB çıktısında Azure Stream Analytics işinizin kimliğini doğrulamak için yönetilen kimliklerin nasıl kullanılacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: a8699b3942fe3a4b23f1d72036b7364cdab36f8e
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83651955"
---
# <a name="use-managed-identities-to-access-azure-sql-database-from-an-azure-stream-analytics-job-preview"></a>Azure Stream Analytics işinden Azure SQL veritabanına erişmek için Yönetilen kimlikler kullanma (Önizleme)

Azure Stream Analytics, Azure SQL veritabanı çıkış havuzları için [yönetilen kimlik kimlik doğrulamasını](../active-directory/managed-identities-azure-resources/overview.md) destekler. Yönetilen kimlikler, Kullanıcı tabanlı kimlik doğrulama yöntemlerinin sınırlamalarını ortadan kaldırarak, parola değişikliklerinden veya her 90 günde bir gerçekleşen Kullanıcı belirteci süre sonu nedeniyle yeniden kimlik doğrulaması yapmanız gerekir. El ile kimlik doğrulaması gereksinimini kaldırdığınızda Stream Analytics dağıtımlarınız tamamen otomatikleştirilebilir.

Yönetilen kimlik, belirli bir Stream Analytics işini temsil eden Azure Active Directory kayıtlı yönetilen bir uygulamadır. Yönetilen uygulama, hedeflenen bir kaynağın kimliğini doğrulamak için kullanılır. Bu makalede, Azure portal aracılığıyla bir Stream Analytics işinin Azure SQL veritabanı çıkışları için yönetilen kimliğin nasıl etkinleştirileceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu özellik için aşağıdakiler gereklidir:

- Azure Stream Analytics işi.

- Azure SQL veritabanı kaynağı.

## <a name="create-a-managed-identity"></a>Yönetilen kimlik oluşturma

İlk olarak, Azure Stream Analytics işiniz için yönetilen bir kimlik oluşturursunuz.

1. [Azure Portal](https://portal.azure.com), Azure Stream Analytics işinizi açın.

1. Sol gezinti menüsünde, **Yapılandır**altında bulunan **yönetilen kimlik** ' i seçin. Sonra, **sistem tarafından atanan yönetilen kimliği kullan** ' ın yanındaki kutuyu Işaretleyin ve **Kaydet**' i seçin.

   ![Sistem tarafından atanan yönetilen kimliği seçin](./media/sql-db-output-managed-identity/system-assigned-managed-identity.png)


   Stream Analytics işinin kimliği için bir hizmet sorumlusu Azure Active Directory oluşturulur. Yeni oluşturulan kimliğin yaşam döngüsü Azure tarafından yönetilir. Stream Analytics işi silindiğinde, ilişkili kimlik (hizmet sorumlusu) Azure tarafından otomatik olarak silinir. 

1. Yapılandırmayı kaydettiğinizde, hizmet sorumlusunun nesne KIMLIĞI (OID) aşağıda gösterildiği gibi asıl KIMLIK olarak listelenir: 

   ![Asıl KIMLIK olarak gösterilen nesne KIMLIĞI](./media/sql-db-output-managed-identity/principal-id.png)

   Hizmet sorumlusu Stream Analytics işle aynı ada sahiptir. Örneğin, işinizin adı *MyASAJob*ise, hizmet sorumlusu adı da *MyASAJob*olur.

## <a name="select-an-active-directory-admin"></a>Active Directory Yöneticisi seçin

Yönetilen bir kimlik oluşturduktan sonra bir Active Directory Yöneticisi seçersiniz.

1. Azure SQL veritabanı kaynağına gidin ve veritabanının altında bulunduğu SQL Server seçin. Kaynak Genel Bakış sayfasında *sunucu adı* ' nın yanındaki SQL Server adı ' nı bulabilirsiniz. 

1. **Ayarlar**altında **Active Directory yönetici** ' yi seçin. Ardından, **yönetici ayarla**' yı seçin. 

   ![Active Directory yönetici sayfası](./media/sql-db-output-managed-identity/active-directory-admin-page.png)
 
1. Active Directory yönetici sayfasında, bir kullanıcı veya grup için SQL Server Yöneticisi olacak şekilde arama yapın ve **Seç**' e tıklayın.

   ![Active Directory Yöneticisi ekleme](./media/sql-db-output-managed-identity/add-admin.png)

   Active Directory yönetici sayfası, Active Directory tüm üyelerini ve gruplarını gösterir. Gri olan kullanıcılar veya gruplar Azure AD yöneticileri olarak desteklenmediği için seçilemez.  **Azure AD Features and Limitations**    [SQL veritabanı veya Azure Synapse kimlik doğrulaması Için Azure Active Directory kimlik doğrulaması kullan](../sql-database/sql-database-aad-authentication.md#azure-ad-features-and-limitations)' ın Azure AD özellikleri ve sınırlamaları bölümünde desteklenen Yöneticiler listesine bakın. Rol tabanlı erişim denetimi (RBAC) yalnızca Portal için geçerlidir ve SQL Server yayılmaz. Ayrıca, seçilen kullanıcı veya Grup, sonraki bölümde **Kapsanan Veritabanı kullanıcısını** oluşturamayacak Kullanıcı olur.

1. **Active Directory yönetici** sayfasında **Kaydet** ' i seçin. Yönetici değiştirme işlemi birkaç dakika sürer.

   Azure AD yöneticisi 'ni ayarlarken, yeni yönetici adı (Kullanıcı veya grup) sanal ana veritabanında SQL Server kimlik doğrulaması kullanıcısı olarak bulunamaz. Varsa, Azure AD yönetici kurulumu başarısız olur ve bir yöneticinin (ad) zaten var olduğunu belirten oluşturma işlemi geri alınır. SQL Server kimlik doğrulaması kullanıcısı Azure AD 'nin bir parçası olmadığından, bu kullanıcı başarısız olduğu için Azure AD kimlik doğrulaması kullanarak sunucuya bağlanma çabaları vardır. 

## <a name="create-a-contained-database-user"></a>Kapsanan veritabanı kullanıcısı oluşturma

Daha sonra, SQL veritabanınızda Azure Active Directory kimliğiyle eşlenmiş bir kapsanan veritabanı kullanıcısı oluşturun. Kapsanan Veritabanı kullanıcısının ana veritabanı için bir oturum açma işlemi yoktur, ancak veritabanıyla ilişkili dizindeki bir kimlikle eşlenir. Azure Active Directory kimliği, tek bir kullanıcı hesabı veya grup olabilir. Bu durumda, Stream Analytics işiniz için kapsanan bir veritabanı kullanıcısı oluşturmak istersiniz. 

1. SQL Server Management Studio kullanarak SQL veritabanına bağlanın. **Kullanıcı adı** , Kullanıcı **değiştirme** iznine sahip Azure Active Directory bir kullanıcı. SQL Server ayarladığınız yönetici bir örnektir. MFA kimlik doğrulamasıyla **Azure Active Directory – Universal** kullanın. 

   ![SQL Server’a bağlanma](./media/sql-db-output-managed-identity/connect-sql-server.png)

   Sunucu adı `<SQL Server name>.database.windows.net` farklı bölgelerde farklı olabilir. Örneğin, Çin bölgesinin kullanması gerekir `<SQL Server name>.database.chinacloudapi.cn` .
 
   Belirli bir SQL veritabanını, **seçeneklere > bağlantı özellikleri ' ne giderek > veritabanına bağlanarak**belirtebilirsiniz.  

   ![SQL Server bağlantısı özellikleri](./media/sql-db-output-managed-identity/sql-server-connection-properties.png)

1. İlk kez bağlandığınızda aşağıdaki pencereyle karşılaşabilirsiniz:

   ![Yeni güvenlik duvarı kuralı penceresi](./media/sql-db-output-managed-identity/new-firewall-rule.png)

   1. Bu durumda, Azure portal SQL Server kaynağına gidin. Güvenlik bölümünde **güvenlik** **duvarları ve sanal ağ** sayfasını açın. 
   1. Kural adı olan yeni bir kural ekleyin.
   1. *Başlangıç IP 'si*Için **yeni güvenlik duvarı kuralı** penceresinde *Kimden* IP adresini kullanın.
   1. *Bitiş IP 'si*Için **yeni güvenlik duvarı kuralı** penceresinde *, IP adresini* kullanın. 
   1. **Kaydet** ' i seçin ve SQL Server Management Studio yeniden bağlanmayı deneyin. 

1. Bağlandıktan sonra kapsanan veritabanı kullanıcısını oluşturun. Aşağıdaki SQL komutu, Stream Analytics işle aynı ada sahip bir kapsanan veritabanı kullanıcısı oluşturur. *ASA_JOB_NAME*etrafına köşeli ayraçları eklediğinizden emin olun. Aşağıdaki T-SQL söz dizimini kullanın ve sorguyu çalıştırın. 

   ```sql
   CREATE USER [ASA_JOB_NAME] FROM EXTERNAL PROVIDER; 
   ```

1. Microsoft 'un Stream Analytics işinin SQL veritabanına erişimi olup olmadığını doğrulamak için Azure Active Directory, veritabanıyla iletişim kurmak için Azure Active Directory izni vermemiz gerekir. Bunu yapmak için, Azure portalındaki "güvenlik duvarları ve sanal ağ" sayfasına yeniden gidin ve "Azure hizmetleri ve kaynaklarının bu sunucuya erişmesine Izin ver" seçeneğini etkinleştirin. 

   ![Güvenlik Duvarı ve sanal ağ](./media/sql-db-output-managed-identity/allow-access.png)

## <a name="grant-stream-analytics-job-permissions"></a>Stream Analytics işi izinleri verme

Bir kapsanan veritabanı kullanıcısı oluşturduktan ve önceki bölümde açıklandığı gibi portalda Azure hizmetlerine erişim izni verildiğinde, Stream Analytics işiniz yönetilen kimlik aracılığıyla SQL veritabanı kaynaklarına, yönetilen kimlik üzerinden **bağlanmak** için bir izne sahiptir. Stream Analytics iş akışında daha sonra gerekli olacağı için Stream Analytics işine seçme ve ekleme izinleri vermenizi öneririz. **Select** izni, Işin SQL veritabanındaki tabloyla bağlantısını test etmesine izin verir. **Ekleme** izni, bir GIRIŞI ve SQL veritabanı çıkışını yapılandırdıktan sonra uçtan uca Stream Analytics sorgularının test edilmesine olanak tanır. SQL Server Management Studio kullanarak Stream Analytics işine bu izinleri verebilirsiniz. Daha fazla bilgi için bkz. [Grant (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql?view=sql-server-ver15) başvurusu.

Yalnızca veritabanındaki belirli bir tablo veya nesneye izin vermek için aşağıdaki T-SQL söz dizimini kullanın ve sorguyu çalıştırın. 

```sql
GRANT SELECT, INSERT ON OBJECT::TABLE_NAME TO ASA_JOB_NAME; 
```

Alternatif olarak, SQL Server Management Studio ' de SQL veritabanınıza sağ tıklayıp **özellikler > izinler**' i seçebilirsiniz. İzinler menüsünde, daha önce eklediğiniz Stream Analytics işi görebilir ve uygun gördüğünüz şekilde izinleri el ile verebilir veya reddedebilirsiniz.

## <a name="create-an-azure-sql-database-output"></a>Azure SQL veritabanı çıkışı oluşturma

Yönetilen kimliğiniz yapılandırıldığına göre, Azure SQL veritabanını Stream Analytics işinize çıktı olarak eklemeye hazırsınız.

Uygun çıktı şemasıyla SQL veritabanınızda bir tablo oluşturduğunuzdan emin olun. Bu tablonun adı, SQL veritabanı çıktısını Stream Analytics işine eklediğinizde doldurulması gereken özelliklerden biridir. Ayrıca, bağlantıyı test etmek ve Stream Analytics sorguları çalıştırmak için işin **Select** ve **Insert** izinlerine sahip olduğundan emin olun. Daha önce yapmadıysanız [Stream Analytics işi Izinleri verme](#grant-stream-analytics-job-permissions) bölümüne bakın. 

1. Stream Analytics işinize geri dönün ve **Iş topolojisi**altında **çıktılar** sayfasına gidin. 

1. **> SQL veritabanı Ekle**' yi seçin. SQL veritabanı çıkış havuzunun çıkış özellikleri penceresinde, kimlik doğrulama modu açılır listesinden **yönetilen kimlik** ' i seçin.

1. Kalan özellikleri doldurun. SQL veritabanı çıkışı oluşturma hakkında daha fazla bilgi için, bkz. [Stream Analytics BIR SQL veritabanı çıkışı oluşturma](stream-analytics-define-outputs.md#sql-database). İşiniz bittiğinde **Kaydet**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics çıkışlarını anlayın](stream-analytics-define-outputs.md)
* [Azure SQL veritabanı 'na Azure Stream Analytics çıkışı](stream-analytics-sql-output-perf.md)
