---
title: Azure SQL veritabanı 'na erişmek için Yönetilen kimlikler kullanma-Azure Stream Analytics
description: Bu makalede, Azure SQL DB çıktısında Azure Stream Analytics işinizin kimliğini doğrulamak için yönetilen kimliklerin nasıl kullanılacağı açıklanır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/08/2020
ms.openlocfilehash: 70ad69c1a34f656347b0cf53b28a1c35ac6ad043
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83598244"
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

1. **Active Directory yönetici** sayfasında **Kaydet** ' i seçin. Yönetici değiştirme işlemi birkaç dakika sürer.  

## <a name="create-a-database-user"></a>Veritabanı kullanıcısı oluşturma

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

## <a name="grant-stream-analytics-job-permissions"></a>Stream Analytics işi izinleri verme

Stream Analytics işin, SQL veritabanı kaynağına **bağlanmak** Için yönetilen kimliğin izni vardır. Büyük olasılıkla, Stream Analytics işin **Select**gibi komutları çalıştırmasına izin vermek verimli olabilir. SQL Server Management Studio kullanarak Stream Analytics işine bu izinleri verebilirsiniz. Daha fazla bilgi için bkz. [Grant (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/grant-transact-sql?view=sql-server-ver15) başvurusu.

Alternatif olarak, SQL Server Management Studio ' de SQL veritabanınıza sağ tıklayıp **özellikler > izinler**' i seçebilirsiniz. İzinler menüsünde, daha önce eklediğiniz Stream Analytics işi görebilir ve uygun gördüğünüz şekilde izinleri el ile verebilir veya reddedebilirsiniz.

## <a name="create-an-azure-sql-database-output"></a>Azure SQL veritabanı çıkışı oluşturma

Yönetilen kimliğiniz yapılandırıldığına göre, Azure SQL veritabanını Stream Analytics işinize çıktı olarak eklemeye hazırsınız.

1. Stream Analytics işinize geri dönün ve **Iş topolojisi**altında **çıktılar** sayfasına gidin. 

1. **> SQL veritabanı Ekle**' yi seçin. SQL veritabanı çıkış havuzunun çıkış özellikleri penceresinde, kimlik doğrulama modu açılır listesinden **yönetilen kimlik** ' i seçin.

1. Kalan özellikleri doldurun. SQL veritabanı çıkışı oluşturma hakkında daha fazla bilgi için, bkz. [Stream Analytics BIR SQL veritabanı çıkışı oluşturma](stream-analytics-define-outputs.md#sql-database). İşiniz bittiğinde **Kaydet**' i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics çıkışlarını anlayın](stream-analytics-define-outputs.md)
* [Azure SQL veritabanı 'na Azure Stream Analytics çıkışı](stream-analytics-sql-output-perf.md)
