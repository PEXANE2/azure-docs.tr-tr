---
title: Azure Active Directory
description: SQL Veritabanı, Yönetilen Örnek ve Synapse SQL ile kimlik doğrulama için Azure Active Directory'yi nasıl kullanacağınızı öğrenin
services: synapse-analytics
author: vvasic-msft
ms.service: synapse-analytics
ms.topic: overview
ms.date: 04/15/2020
ms.author: vvasic
ms.reviewer: jrasnick
ms.openlocfilehash: db80c11c3b6eab3b7e682878e479729f4787a40b
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086105"
---
# <a name="use-azure-active-directory-authentication-for-authentication-with-synapse-sql"></a>Synapse SQL ile kimlik doğrulama için Azure Active Directory Authentication'ı kullanma

Azure Etkin Dizin kimlik doğrulaması, Azure Etkin Dizin (Azure AD) kimliklerini kullanarak [Azure Synapse Analytics'e](../overview-faq.md) bağlanma mekanizmasıdır.

Azure AD kimlik doğrulaması ile, izin yönetimini basitleştirmek için Azure Synapse erişimi olan kullanıcı kimliklerini merkezi olarak yönetebilirsiniz. Avantajları şunlardır:

- Normal kullanıcı adı ve parola kimlik doğrulamasına alternatif sağlar.
- Veritabanı sunucuları arasında kullanıcı kimliklerinin çoğalmasını durdurmaya yardımcı olur.
- Parola döndürmeyi tek bir yerde sağlar.
- Müşteriler, harici (Azure AD) gruplarını kullanarak izinleri yönetebilir.
- Azure Active Directory tarafından desteklenen tümleşik Windows kimlik doğrulaması ve diğer kimlik doğrulama biçimlerini etkinleştirerek parola depolamayı ortadan kaldırabilir.
- Azure AD, Azure Synapse'ye bağlanan uygulamalar için belirteç tabanlı kimlik doğrulamasını destekler.
- Azure AD kimlik doğrulaması, etki alanı eşitlemesi olmadan yerel bir Azure Etkin Dizini için ADFS (etki alanı federasyonu) veya yerel kullanıcı/parola kimlik doğrulamasını destekler.
- Azure AD, Multi-Factor Authentication (MFA) içeren Active Directory Evrensel Kimlik Doğrulaması'nın kullanıldığı SQL Server Management Studio'dan gelen bağlantıları destekler.  MFA telefon görüşmesi, SMS mesajı, pin ile akıllı kart veya mobil uygulama bildirimi gibi çok sayıda kolay doğrulama seçeneğine sahiptir. Daha fazla bilgi [için Synapse SQL ile Azure AD MFA için SSMS desteğine](mfa-authentication.md)bakın.
- Azure AD, Active Directory Etkileşimli Kimlik Doğrulaması'nın kullanıldığı SQL Server Veri Araçları'ndan (SSDT) gelen benzer bağlantıları destekler. Daha fazla bilgi için bkz. [SQL Server Veri Araçları'na (SSDT) Azure Active Directory desteği](/sql/ssdt/azure-active-directory?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

Yapılandırma adımları, Azure Active Directory kimlik doğrulamasını yapılandırmak ve kullanmak için aşağıdaki yordamları içerir.

1. Azure AD oluşturun ve doldurun.
2. Azure Etkin Dizin kimliği oluşturma
3. Synapse çalışma alanında oluşturulan Azure Etkin Dizin kimliğine rol atama (önizleme)
4. Azure AD kimliklerini kullanarak Synapse Studio'ya bağlanın.

## <a name="aad-pass-through-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te AAD geçişi

Azure Synapse Analytics, Azure Active Directory kimliğinizi kullanarak veri gölündeki verilere erişmenizi sağlar.

Farklı veri motorlarında saygı duyulan dosya ve verilerdeki erişim haklarını tanımlamak, izinlerin birden çok yerde tanımlamak yerine tanımlandığı tek bir yere sahip olarak veri gölü çözümlerinizi basitleştirmenize olanak tanır.

## <a name="trust-architecture"></a>Güven mimarisi

Aşağıdaki üst düzey diyagram, Synapse SQL ile Azure AD kimlik doğrulamasını kullanmanın çözüm mimarisini özetler. Azure AD yerel kullanıcı parolasını desteklemek için yalnızca Bulut bölümü ve Azure AD/Synapse Synapse SQL olarak kabul edilir. Federe kimlik doğrulamasını (veya Windows kimlik bilgileri için kullanıcı/parolayı) desteklemek için ADFS bloğu ile iletişim gereklidir. Oklar iletişim yollarını gösteriyor.

![aad auth diyagramı](./media/aad-authentication/1-active-directory-authentication-diagram.png)

Aşağıdaki diyagram, istemcinin bir belirteç göndererek veritabanına bağlanmasına izin veren federasyon, güven ve barındırma ilişkilerini gösterir. Belirteç bir Azure REKLAMı tarafından doğrulanır ve veritabanı tarafından güvenilir. 

Müşteri 1, yerel kullanıcılarla bir Azure Etkin Dizinini veya federe kullanıcılarla bir Azure REKLAM'ı temsil edebilir. Müşteri 2, içe aktarılan kullanıcılar da dahil olmak üzere olası bir çözümü temsil eder; Bu örnekte, ADFS'nin Azure Active Directory ile senkronize edildiği federe bir Azure Active Directory'sinden gelir. 

Azure AD kimlik doğrulaması kullanarak bir veritabanına erişimin barındırma aboneliğinin Azure AD ile ilişkilendirilmesi gerektiğini anlamak önemlidir. Aynı abonelik, Azure SQL Veritabanı'nı veya SQL havuzunu barındıran SQL Server'ı oluşturmak için kullanılmalıdır.

![abonelik ilişkisi](./media/aad-authentication/2-subscription-relationship.png)

## <a name="administrator-structure"></a>Yönetici yapısı

Azure AD kimlik doğrulaması kullanırken, Synapse SQL için iki Yönetici hesabı vardır; özgün SQL Server yöneticisi ve Azure AD yöneticisi. Yalnızca bir Azure AD hesabına dayalı yönetici, bir kullanıcı veritabanında ilk Azure AD içerdiği veritabanı kullanıcısını oluşturabilir. 

Azure AD yöneticisi girişi bir Azure AD kullanıcısı veya Azure AD grubu olabilir. Yönetici bir grup hesabı olduğunda, synapse SQL örneği için birden çok Azure AD yöneticisine olanak sağlayarak herhangi bir grup üyesi tarafından kullanılabilir. 

Grup hesabını yönetici olarak kullanmak, Synapse Analytics çalışma alanındaki kullanıcıları veya izinleri değiştirmeden Azure AD'deki grup üyelerini merkezi olarak eklemenize ve kaldırmanıza izin vererek yönetilebilirliği artırır. İstediğiniz zaman yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) yapılandırılabilir.

![yönetici yapısı](./media/aad-authentication/3-admin-structure.png)

## <a name="permissions"></a>İzinler

Yeni kullanıcılar oluşturmak için veritabanında izne `ALTER ANY USER` sahip olmalısınız. İzin `ALTER ANY USER` herhangi bir veritabanı kullanıcısına verilebilir. `ALTER ANY USER` İzin, sunucu yöneticisi hesapları ve veritabanı kullanıcıları tarafından `CONTROL ON DATABASE` `ALTER ON DATABASE` bu veritabanı için izin li `db_owner` veya veritabanı rolü üyeleri tarafından da tutulur.

Synapse SQL'de bulunan bir veritabanı kullanıcısı oluşturmak için, azure AD kimliğini kullanarak veritabanına veya örneğe bağlanmanız gerekir. İlk bulunan veritabanı kullanıcısını oluşturmak için, bir Azure AD yöneticisi (veritabanının sahibi) kullanarak veritabanına bağlanmanız gerekir. 

Azure AD kimlik doğrulaması yalnızca Azure AD yöneticisi Synapse SQL için oluşturulduğunda mümkündür. Azure Active Directory yöneticisi sunucudan kaldırıldıysa, Synapse SQL içinde daha önce oluşturulan mevcut Azure Active Directory kullanıcıları, Azure Etkin Dizin kimlik bilgilerini kullanarak veritabanına artık bağlanabilir.
 
## <a name="azure-ad-features-and-limitations"></a>Azure AD özellikleri ve sınırlamaları

- Azure AD'nin aşağıdaki üyeleri Synapse SQL'de kullanılabilir:

  - Yerel üyeler: Azure AD'de yönetilen etki alanında veya müşteri etki alanında oluşturulan üye. Daha fazla bilgi için [bkz.](../../active-directory/fundamentals/add-custom-domain.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
  - Federe etki alanı üyeleri: Azure AD'de federe etki alanı yla oluşturulmuş bir üye. Daha fazla bilgi için Bkz. [Microsoft Azure şimdi Windows Server Active Directory ile federasyonu destekler.](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/)
  - Diğer Azure AD'lerinden, yerel veya federe etki alanı üyesi olan üyeler ithal edilir.
  - Etkin Dizin grupları güvenlik grupları olarak oluşturuldu.

- Sunucu rolü olan bir grubun parçası olan Azure AD kullanıcıları, Synapse SQL'e karşı **[CREATE DATABASE SCOPED KIMLIK](/sql/t-sql/statements/create-database-scoped-credential-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)** sözdizimini kullanamaz. `db_owner` Aşağıdaki hatayı görürsünüz:

    `SQL Error [2760] [S0001]: The specified schema name 'user@mydomain.com' either does not exist or you do not have permission to use it.`

    CREATE `db_owner` **DATABASE SCOPED CREDIAL** sorununu azaltmak için rolü doğrudan tek bir Azure AD kullanıcısına ver.

- Bu sistem işlevleri Azure AD ilkeleri altında yürütüldüğünde NULL değerlerini döndürür:

  - `SUSER_ID()`
  - `SUSER_NAME(<admin ID>)`
  - `SUSER_SNAME(<admin SID>)`
  - `SUSER_ID(<admin name>)`
  - `SUSER_SID(<admin name>)`

## <a name="connecting-using-azure-ad-identities"></a>Azure AD kimliklerini kullanarak bağlanma

Azure Etkin Dizin kimlik doğrulaması, Azure AD kimliklerini kullanarak bir veritabanına bağlanma nın aşağıdaki yöntemlerini destekler:

- Azure Active Directory Password
- Azure Active Directory Entegre
- MFA ile Azure Active Directory Universal
- Uygulama belirteç kimlik doğrulamasını kullanma

Aşağıdaki kimlik doğrulama yöntemleri Azure AD sunucu ilkeleri (oturum açmalar)**(genel önizleme)** için desteklenir:

- Azure Active Directory Password
- Azure Active Directory Entegre
- MFA ile Azure Active Directory Universal

### <a name="additional-considerations"></a>Diğer konular

- Yönetilebilirliği artırmak için, yönetici olarak özel bir Azure REKLAM grubu sağlamanızı öneririz.
- Synapse SQL havuzu için herhangi bir zamanda yalnızca bir Azure AD yöneticisi (kullanıcı veya grup) yapılandırılabilir.
  - Sql isteğe bağlı (önizleme) için Azure AD sunucu ilkelerinin (oturum açmalar) eklenmesi, `sysadmin` role eklenebilecek birden çok Azure AD sunucu ilkesi (oturum açma) oluşturma olanağı sağlar.
- Yalnızca Synapse SQL için bir Azure AD yöneticisi, başlangıçta Bir Azure Etkin Dizin hesabı kullanarak Synapse SQL'e bağlanabilir. Etkin Dizin yöneticisi sonraki Azure AD veritabanı kullanıcılarını yapılandırabilir.
- Bağlantı zaman larını 30 saniyeye ayarlamanızı öneririz.
- VISUAL Studio 2015 için SQL Server 2016 Management Studio ve SQL Server Data Tools (sürüm 14.0.60311.1Nisan 2016 veya sonrası) Azure Active Directory kimlik doğrulamasını destekler. (Azure AD kimlik **doğrulaması, SqlServer için .NET Framework Data Provider**tarafından desteklenir ; en azından sürüm .NET Framework 4.6). Bu nedenle bu araçların en yeni sürümleri ve veri katmanı uygulamaları (DAC ve . BACPAC) Azure AD kimlik doğrulamasını kullanabilir.
- Sürüm 15.0.1 ile başlayarak, [sqlcmd yardımcı programı](/sql/tools/sqlcmd-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) ve [bcp yardımcı programı](/sql/tools/bcp-utility?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) MFA ile Active Directory Interactive kimlik doğrulamasını destekler.
- Visual Studio 2015 için SQL Server Veri Araçları, Veri Araçları'nın en az Nisan 2016 sürümünü (sürüm 14.0.60311.1) gerektirir. Şu anda Azure AD kullanıcıları SSDT Object Explorer'da gösterilmez. Geçici çözüm olarak, [sys.database_principals'deki](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)kullanıcıları görüntüleyin.
- [SQL Server için Microsoft JDBC Driver 6.0](https://www.microsoft.com/download/details.aspx?id=11774) Azure AD kimlik doğrulamasını destekler. Ayrıca, bkz. [Bağlantı Özelliklerini Ayarlama.](/sql/connect/jdbc/setting-the-connection-properties?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)

## <a name="next-steps"></a>Sonraki adımlar

- Synapse SQL'de erişim ve denetime genel bir bakış için [Synapse SQL erişim denetimine](../sql/access-control.md)bakın.
- Veritabanı sorumluları hakkında daha fazla bilgi için bkz. [Sorumlular](/sql/relational-databases/security/authentication-access/principals-database-engine?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).
- Veritabanı rolleri hakkında daha fazla bilgi için bkz. [Veritabanı rolleri](/sql/relational-databases/security/authentication-access/database-level-roles?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

 