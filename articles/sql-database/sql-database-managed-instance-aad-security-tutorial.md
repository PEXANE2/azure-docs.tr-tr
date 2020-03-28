---
title: Azure AD sunucu ilkeleri (oturum açmalar) ile yönetilen örnek güvenliği
description: Azure SQL Veritabanı'nda yönetilen bir örneği güvence altına almak için teknikler ve özellikler hakkında bilgi edinin ve Azure AD sunucu ilkelerini (oturum açma) kullanın
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: GitHubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 11/06/2019
ms.openlocfilehash: bd65a21c2aa21643c76966410931949db7d17ad6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73822799"
---
# <a name="tutorial-managed-instance-security-in-azure-sql-database-using-azure-ad-server-principals-logins"></a>Öğretici: Azure AD sunucu ilkelerini (oturum açmalar) kullanarak Azure SQL Veritabanı'nda yönetilen örnek güvenliği

Yönetilen örnek, en son SQL Server şirket içi (Enterprise Edition) Veritabanı Altyapısının sahip olduğu hemen hemen tüm güvenlik özelliklerini sağlar:

- Yalıtılmış bir ortamda erişimi sınırlama
- Kimlik gerektiren kimlik doğrulama mekanizmalarını kullanma (Azure AD, SQL Kimlik Doğrulama)
- Rol tabanlı üyelikler ve izinlerle yetkilendirme kullanma
- Güvenlik özelliklerini etkinleştirme

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - Yönetilen bir örnek için Azure Active Directory (AD) sunucu ilkesi (oturum açma) oluşturma
> - Yönetilen bir durumda Azure AD sunucu ilkelerine (oturum açma) izin verme
> - Azure AD sunucu ilkelerinden (oturum açma) Azure AD kullanıcıları oluşturma
> - Azure AD kullanıcılarına izin ler atama ve veritabanı güvenliğini yönetme
> - Azure AD kullanıcılarıyla kimliğe bürünme kullanma
> - Azure AD kullanıcılarıyla veritabanı arası sorguları kullanma
> - Tehdit koruması, denetim, veri maskeleme ve şifreleme gibi güvenlik özellikleri hakkında bilgi edinin

Daha fazla bilgi edinmek için [Azure SQL Veritabanı yönetilen örnek özeti](sql-database-managed-instance-index.yml) ve yetenek [makalelerine](sql-database-managed-instance.md) bakın.

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için aşağıdaki ön koşullara sahip olduğunuzdan emin olun:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS)
- Azure SQL Veritabanı yönetilen örneği
  - Bu makaleyi izleyin: [Quickstart: Bir Azure SQL Veritabanı yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md)
- Yönetilen örneğinize erişebilir ve [yönetilen örnek için bir Azure REKLAM yöneticisi sayılabilir.](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance) Daha fazla bilgi için bkz:
    - [Uygulamanızı yönetilen bir örne bağlama](sql-database-managed-instance-connect-app.md) 
    - [Yönetilen örnek bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md)
    - [Azure Active Directory kimlik doğrulamasını SQL ile yapılandırma ve yönetme](sql-database-aad-authentication-configure.md)

## <a name="limiting-access-to-your-managed-instance"></a>Yönetilen örneğinize erişimi sınırlama

Yönetilen örneklere özel bir IP adresi üzerinden erişilebilir. Tıpkı şirket içinde yalıtılmış bir SQL Server ortamı gibi, bir bağlantı kurulabilmesi için uygulamaların veya kullanıcıların yönetilen örnek ağına (VNet) erişilmesi gerekir. Daha fazla bilgi için aşağıdaki makaleye bakın, [Uygulamanızı yönetilen bir örne bağlayın.](sql-database-managed-instance-connect-app.md)

Yönetilen örnekte, azure SQL Veritabanı ile aynı şekilde ortak bağlantılara olanak tanıyan bir hizmet bitiş noktasını yapılandırmak da mümkündür. Daha fazla bilgi için aşağıdaki makaleye bakın, [Azure SQL Veritabanı yönetilen örneğinde ortak bitiş noktasını yapılandırın.](sql-database-managed-instance-public-endpoint-configure.md)

> [!NOTE] 
> Hizmet bitiş noktaları etkin olsa bile, [SQL Veritabanı güvenlik duvarı kuralları](sql-database-firewall-configure.md) geçerli değildir. Yönetilen örnek, bağlantıyı yönetmek için kendi [yerleşik güvenlik duvarı](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md) vardır.

## <a name="create-an-azure-ad-server-principal-login-for-a-managed-instance-using-ssms"></a>SSMS kullanarak yönetilen bir örnek için bir Azure AD sunucu ilkesi (oturum açma) oluşturma

İlk Azure AD sunucu yöneticisi (oturum açma), sağlama işlemi sırasında oluşturulan yönetilen örnek `sysadmin`için standart SQL Server hesabı (azure olmayan AD) veya Azure AD yöneticisi tarafından oluşturulabilir. Daha fazla bilgi için, [yönetilen örneğiniz için Azure Etkin Dizin yöneticisi sağlama](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)konusuna bakın. Bu işlev, [Azure AD sunucu ilkelerinin GA'sı'ndan](sql-database-aad-authentication-configure.md#new-azure-ad-admin-functionality-for-mi)bu yana değişti.

Yönetilen örneğinize bağlanma örnekleri için aşağıdaki makalelere bakın:

- [Hızlı başlatma: Yönetilen bir örne bağlanmak için Azure VM'yi yapılandırın](sql-database-managed-instance-configure-vm.md)
- [Quickstart: Şirket içinde yönetilen bir örneğe noktadan siteye bağlantıyı yapılandırma](sql-database-managed-instance-configure-p2s.md)

1. SQL Server Management `sysadmin` [Studio'u](sql-database-managed-instance-configure-p2s.md#use-ssms-to-connect-to-the-managed-instance)kullanarak MI için azure AD yöneticisi olan standart bir SQL Server hesabı (azure olmayan AD) kullanarak yönetilen örneğinize giriş yapın.

2. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.

3. Sorgu penceresinde, yerel bir Azure REKLAM hesabı için oturum açmak için aşağıdaki sözdizimini kullanın:

    ```sql
    USE master
    GO
    CREATE LOGIN login_name FROM EXTERNAL PROVIDER
    GO
    ```

    Bu örnek, hesap nativeuser@aadsqlmi.onmicrosoft.comiçin bir giriş oluşturur.

    ```sql
    USE master
    GO
    CREATE LOGIN [nativeuser@aadsqlmi.onmicrosoft.com] FROM EXTERNAL PROVIDER
    GO
    ```

4. Araç çubuğunda, **Execute** oturum açma oluşturmak için Yürüt'''ün

5. Aşağıdaki T-SQL komutunu çalıştırarak yeni eklenen girişi kontrol edin:

    ```sql
    SELECT *  
    FROM sys.server_principals;  
    GO
    ```

    ![yerli-login.png](media/sql-database-managed-instance-security-tutorial/native-login.png)

Daha fazla bilgi için CREATE [LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current)'e bakın.

## <a name="granting-permissions-to-allow-the-creation-of-managed-instance-logins"></a>Yönetilen örnek girişleri oluşturulmasına izin vermek için izin verme

Diğer Azure AD sunucu ilkelerini (oturum açmalar) oluşturmak için, SQL Server rolleri veya izinlerinin ana müdüre (SQL veya Azure AD) verilmesi gerekir.

### <a name="sql-authentication"></a>SQL kimlik doğrulaması

- Oturum açma bir SQL Sorumlusuysa, yalnızca rolün `sysadmin` bir parçası olan oturum açmalar, bir Azure REKLAM hesabı için oturum açma oluşturmak için oluştur komutunu kullanabilir.

### <a name="azure-ad-authentication"></a>Azure AD kimlik doğrulaması

- Yeni oluşturulan Azure AD sunucu sorumlusunun (oturum açma) diğer Azure AD kullanıcıları, grupları veya uygulamaları için başka `sysadmin` `securityadmin` oturum açmalar oluşturmasına izin vermek için oturum açma veya sunucu rolünü verir. 
- En **azından, ALTER ANY Gİrİş** İzNİ Diğer Azure AD sunucu ilkelerini (oturum açma) oluşturmak için Azure AD sunucu ilkesine (oturum açma) verilmelidir. 
- Varsayılan olarak, yeni oluşturulan Azure AD sunucu ilkelerine (giriş ler) ana olarak verilen standart izin şudur: **CONNECT SQL** ve VIEW **ANY DATABASE**.
- Sunucu `sysadmin` rolü, yönetilen bir örnekte birçok Azure AD sunucu ilkesine (oturum açma) verilebilir.

Girişi sunucu rolüne `sysadmin` eklemek için:

1. Yönetilen örneğe yeniden giriş yapın veya Azure AD yöneticisi veya SQL `sysadmin`Sorumlusu olan varolan bağlantıyı kullanın.

1. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.

1. Aşağıdaki T-SQL sözdizimini kullanarak `sysadmin` Azure AD sunucu sorumlusuna (oturum açma) sunucu rolünü ver:

    ```sql
    ALTER SERVER ROLE sysadmin ADD MEMBER login_name
    GO
    ```

    Aşağıdaki örnek, `sysadmin` oturum açma için sunucu rolünü verirnativeuser@aadsqlmi.onmicrosoft.com

    ```sql
    ALTER SERVER ROLE sysadmin ADD MEMBER [nativeuser@aadsqlmi.onmicrosoft.com]
    GO
    ```

## <a name="create-additional-azure-ad-server-principals-logins-using-ssms"></a>SSMS kullanarak ek Azure AD sunucu ilkeleri (oturum açmalar) oluşturun

Azure AD sunucu sorumlusu (oturum açma) oluşturulduktan `sysadmin` ve ayrıcalıklar sağlandıktan sonra, bu oturum **açma, CREATE LOGIN**ile **FROM EXTERNAL Provider** yan tümcesini kullanarak ek oturum açmalar oluşturabilir.

1. SQL Server Management Studio'yu kullanarak Azure AD sunucu ilkesi (oturum açma) ile yönetilen örne bağlanın. Yönetilen örnek ana bilgisayar adınızı girin. SSMS'te Kimlik Doğrulama için, bir Azure AD hesabıyla oturum açarken aralarından seçim yapabileceğiniz üç seçenek vardır:

   - Active Directory - MFA desteği ile Universal
   - Active Directory - Şifre
   - Aktif Dizin - Entegre </br>

     ![ssms-giriş-prompt.png](media/sql-database-managed-instance-security-tutorial/ssms-login-prompt.png)

     Daha fazla bilgi için aşağıdaki makaleye bakın: [SQL Veritabanı ve SQL Veri Ambarı ile Evrensel Kimlik Doğrulama (MFA için SSMS desteği)](sql-database-ssms-mfa-authentication.md)

1. **MFA desteği ile Active Directory - Universal'ı**seçin. Bu, Çok Faktörlü Kimlik Doğrulama (MFA) giriş pencerelerini getirir. Azure AD şifrenizle oturum açın.

    ![mfa-giriş-prompt.png](media/sql-database-managed-instance-security-tutorial/mfa-login-prompt.png)

1. SSMS **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.
1. Sorgu penceresinde, başka bir Azure REKLAM hesabı için oturum açmak için aşağıdaki sözdizimini kullanın:

    ```sql
    USE master
    GO
    CREATE LOGIN login_name FROM EXTERNAL PROVIDER
    GO
    ```

    Bu örnek, etki alanı aadsqlmi.net Azure bob@aadsqlmi.netAD aadsqlmi.onmicrosoft.com ile federe olan Azure AD kullanıcısı için bir giriş oluşturur.

    Aşağıdaki T-SQL komutunu çalıştırın. Federe Azure AD hesapları, şirket içi Windows oturum açmaları ve kullanıcıları için yönetilen örnek değiştirmelerdir.

    ```sql
    USE master
    GO
    CREATE LOGIN [bob@aadsqlmi.net] FROM EXTERNAL PROVIDER
    GO
    ```

1. CREATE DATABASE sözdizimini kullanarak yönetilen örnekte bir [veritabanı oluşturun.](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-mi-current) Bu veritabanı sonraki bölümde kullanıcı girişlerini test etmek için kullanılacaktır.
    1. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.
    1. Sorgu penceresinde, **MyMITestDB**adlı bir veritabanı oluşturmak için aşağıdaki sözdizimini kullanın.

        ```sql
        CREATE DATABASE MyMITestDB;
        GO
        ```

1. Azure AD'deki bir grup için yönetilen bir örnek girişi oluşturun. Oturumu yönetilen örneğe eklemeden önce grubun Azure AD'de bulunması gerekir. Bkz. [Azure Etkin Dizini'ni kullanarak temel bir grup oluşturun ve üye ekleyin.](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md) Grubum _mygroup_ oluştur ve bu gruba üye ekleyin.

1. SQL Server Management Studio'da yeni bir sorgu penceresi açın.

    Bu örnek, Azure AD'de _mygroup_ adında bir grup olduğunu varsayar. Aşağıdaki komutu yürütün:

    ```sql
    USE master
    GO
    CREATE LOGIN [mygroup] FROM EXTERNAL PROVIDER
    GO
    ```

1. Test olarak, yeni oluşturulan giriş veya grupla yönetilen örneğe giriş yapın. Yönetilen örneğe yeni bir bağlantı açın ve kimlik doğrulaması yaparken yeni girişi kullanın.
1. **Object Explorer'da**sunucuyu sağ tıklatın ve yeni bağlantı için **Yeni Sorgu'yu** seçin.
1. Aşağıdaki komutu çalıştırarak yeni oluşturulan Azure AD sunucu sorumlusu (oturum açma) için sunucu izinlerini denetleyin:

      ```sql
      SELECT * FROM sys.fn_my_permissions (NULL, 'DATABASE')
      GO
      ```

> [!NOTE]
> Azure AD konuk kullanıcıları, yalnızca bir Azure REKLAM Grubu'nun parçası olarak eklendiğinde yönetilen örnek girişleri için desteklenir. Azure AD konuk kullanıcısı, yönetilen örneğin ait olduğu Azure AD'ye başka bir Azure REKLAMından davet edilen bir hesaptır. Örneğin, joe@contoso.com (Azure AD Hesabı) veya steve@outlook.com (MSA Hesabı) Azure AD aadsqlmi'deki bir gruba eklenebilir. Kullanıcılar bir gruba eklendikten sonra, **CREATE Gİrİş** sözdizimini kullanarak grup için yönetilen örnek **ana** veritabanında bir giriş oluşturulabilir. Bu grubun üyesi olan konuk kullanıcılar, geçerli girişlerini kullanarak yönetilen örneğe joe@contoso.com steve@outlook.combağlanabilir (Örneğin, veya).

## <a name="create-an-azure-ad-user-from-the-azure-ad-server-principal-login-and-give-permissions"></a>Azure AD sunucu sorumlusundan (oturum açma) bir Azure REKLAM kullanıcısı oluşturun ve izin verin

Tek tek veritabanlarına yetkilendirme, yönetilen örnekte, SQL Server'da olduğu gibi şirket içinde de çok aynı şekilde çalışır. Bir kullanıcı, veritabanındaki varolan bir oturum açma dan oluşturulabilir ve bu veritabanında izinlerle sağlanabilir veya veritabanı rolüne eklenebilir.

Şimdi **MyMITestDB**adlı bir veritabanı oluşturduk ve yalnızca varsayılan izinleri olan bir giriş, bir sonraki adım bu oturum açma bir kullanıcı oluşturmaktır. Şu anda, oturum açma yönetilen örne bağlanabilir ve tüm veritabanlarını görebilir, ancak veritabanlarıyla etkileşim kuramıyor. Varsayılan izinlere sahip Azure AD hesabında oturum açtıysanız ve yeni oluşturulan veritabanını genişletmeye çalışırsanız, aşağıdaki hatayı görürsünüz:

![ssms-db-ulaşılmaz.png](media/sql-database-managed-instance-security-tutorial/ssms-db-not-accessible.png)

Veritabanı izinleri verme hakkında daha fazla bilgi için veritabanı [altyapısı izinleriyle başlarken](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions)bkz.

### <a name="create-an-azure-ad-user-and-create-a-sample-table"></a>Azure AD kullanıcısı oluşturma ve örnek tablo oluşturma

1. SQL Server Management Studio'u kullanarak yönetilen `sysadmin` örneğinize giriş yapın.
1. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.
1. Sorgu penceresinde, azure AD sunucu sorumlusundan (oturum açma) bir Azure REKLAM kullanıcısı oluşturmak için aşağıdaki sözdizimini kullanın:

    ```sql
    USE <Database Name> -- provide your database name
    GO
    CREATE USER user_name FROM LOGIN login_name
    GO
    ```

    Aşağıdaki örnek, oturum bob@aadsqlmi.net açma dan bir kullanıcı oluştururbob@aadsqlmi.net:

    ```sql
    USE MyMITestDB
    GO
    CREATE USER [bob@aadsqlmi.net] FROM LOGIN [bob@aadsqlmi.net]
    GO
    ```

1. Ayrıca, bir grup olan bir Azure AD sunucu sorumlusundan (oturum açma) bir Azure AD kullanıcısı oluşturmak için de desteklenir.

    Aşağıdaki örnek, Azure AD _grubum_ için Azure AD'nizde bulunan bir oturum açma oluşturur.

    ```sql
    USE MyMITestDB
    GO
    CREATE USER [mygroup] FROM LOGIN [mygroup]
    GO
    ```

    **Grubuma** ait tüm kullanıcılar **MyMITestDB** veritabanına erişebilir.

    > [!IMPORTANT]
    > Bir Azure AD sunucu sorumlusundan (oturum açma) bir **KULLANICI** oluştururken, **user_name Gİrİş'ten**aynı login_name olarak belirtin.

    Daha fazla bilgi için create [USER](/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current)'a bakın.

1. Yeni bir sorgu penceresinde, aşağıdaki T-SQL komutunu kullanarak bir test tablosu oluşturun:

    ```sql
    USE MyMITestDB
    GO
    CREATE TABLE TestTable
    (
    AccountNum varchar(10),
    City varchar(255),
    Name varchar(255),
    State varchar(2)
    );
    ```

1. Oluşturulan kullanıcı ile SSMS bir bağlantı oluşturun. `sysadmin` Önceki tablo tarafından oluşturulan table **TestTable** göremediğinizi fark edeceksiniz. Kullanıcıya veritabanından verileri okuma izni sağlamamız gerekir.

1. Aşağıdaki komutu çalıştırarak kullanıcının geçerli iznini denetleyebilirsiniz:

    ```sql
    SELECT * FROM sys.fn_my_permissions('MyMITestDB','DATABASE')
    GO
    ```

### <a name="add-users-to-database-level-roles"></a>Kullanıcıları veritabanı düzeyindeki rollere ekleme

Kullanıcının veritabanındaki verileri görmesi için, kullanıcıya [veritabanı düzeyinde roller](/sql/relational-databases/security/authentication-access/database-level-roles) sağlayabiliriz.

1. SQL Server Management Studio'u kullanarak yönetilen `sysadmin` örneğinize giriş yapın.

1. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.

1. Aşağıdaki T-SQL `db_datareader` sözdizimini kullanarak Azure AD kullanıcısına veritabanı rolünü ver:

    ```sql
    Use <Database Name> -- provide your database name
    ALTER ROLE db_datareader ADD MEMBER user_name
    GO
    ```

    Aşağıdaki örnek, kullanıcıya bob@aadsqlmi.net ve grup `db_datareader` _mygroup'a_ **MyMITestDB** veritabanında izinler sağlar:

    ```sql
    USE MyMITestDB
    GO
    ALTER ROLE db_datareader ADD MEMBER [bob@aadsqlmi.net]
    GO
    ALTER ROLE db_datareader ADD MEMBER [mygroup]
    GO
    ```

1. Veritabanında oluşturulan Azure AD kullanıcısını aşağıdaki komutu çalıştırarak var olup olmadığını denetleyin:

    ```sql
    SELECT * FROM sys.database_principals
    GO
    ```

1. `db_datareader` Role eklenen kullanıcıyla yönetilen örneğe yeni bir bağlantı oluşturun.
1. Tabloyu görmek için **Object Explorer'daki** veritabanını genişletin.

    ![ssms-test-table.png](media/sql-database-managed-instance-security-tutorial/ssms-test-table.png)

1. Yeni bir sorgu penceresi açın ve aşağıdaki SELECT deyimini çalıştırın:

    ```sql
    SELECT *
    FROM TestTable
    ```

    Tablodaki verileri görebiliyor musunuz? Döndürülen sütunları görmelisiniz.

    ![ssms-test-tablo-query.png](media/sql-database-managed-instance-security-tutorial/ssms-test-table-query.png)

## <a name="impersonating-azure-ad-server-level-principals-logins"></a>Azure AD sunucu düzeyindeki ilkelerin (oturum açmalar) kimliği

Yönetilen örnek, Azure AD sunucu düzeyindeki ilkelerin (oturum açmalar) kimliğe bürünmesini destekler.

### <a name="test-impersonation"></a>Test kimliğe bürünme

1. SQL Server Management Studio'u kullanarak yönetilen `sysadmin` örneğinize giriş yapın.

1. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.

1. Sorgu penceresinde, yeni bir depolanmış yordam oluşturmak için aşağıdaki komutu kullanın:

    ```sql
    USE MyMITestDB
    GO  
    CREATE PROCEDURE dbo.usp_Demo  
    WITH EXECUTE AS 'bob@aadsqlmi.net'  
    AS  
    SELECT user_name();  
    GO
    ```

1. Depolanan yordamı yürüdürken taklit ettiğiniz kullanıcının **bob\@aadsqlmi.net**olduğunu görmek için aşağıdaki komutu kullanın.

    ```sql
    Exec dbo.usp_Demo
    ```

1. EXECUTE Gİrİş İfadeSI olarak kullanılarak kimliğe bürünme testi:

    ```sql
    EXECUTE AS LOGIN = 'bob@aadsqlmi.net'
    GO
    SELECT SUSER_SNAME()
    REVERT
    GO
    ```

> [!NOTE]
> Yalnızca `sysadmin` rolün bir parçası olan SQL sunucu düzeyindeki ilkeler (oturum açmalar), Azure AD ilkelerini hedefleyen aşağıdaki işlemleri gerçekleştirebilir: 
> - KULLANıCı OLARAK ÇALıŞTıRın
> - Gİrİş OLARAK YÜRÜT

## <a name="using-cross-database-queries-in-managed-instances"></a>Yönetilen örneklerde veritabanı sorgularını kullanma

Azure AD sunucu ilkeleri (oturum açmalar) ile Azure AD hesapları için veritabanı arası sorgular desteklenir. Bir Azure AD grubuyla veritabanı arası sorguyu sınamak için başka bir veritabanı ve tablo oluşturmamız gerekir. Zaten varsa başka bir veritabanı ve tablo oluşturmayı atlayabilirsiniz.

1. SQL Server Management Studio'u kullanarak yönetilen `sysadmin` örneğinize giriş yapın.
1. **Object Explorer'da**sunucuyu sağ tıklatın ve **Yeni Sorgu'yu**seçin.
1. Sorgu penceresinde, **MyMITestDB2** adlı bir veritabanı ve **TestTable2**adlı tablo oluşturmak için aşağıdaki komutu kullanın:

    ```sql
    CREATE DATABASE MyMITestDB2;
    GO
    USE MyMITestDB2
    GO
    CREATE TABLE TestTable2
    (
    EmpId varchar(10),
    FirstName varchar(255),
    LastName varchar(255),
    Status varchar(10)
    );
    ```

1. Yeni bir sorgu penceresinde, yeni veritabanı **MyMITestDB2**kullanıcı _mygroup_ oluşturmak için aşağıdaki komutu yürütmek ve _mygroup_için bu veritabanında SELECT izinleri vermek:

    ```sql
    USE MyMITestDB2
    GO
    CREATE USER [mygroup] FROM LOGIN [mygroup]
    GO
    GRANT SELECT TO [mygroup]
    GO
    ```

1. Azure AD _grubu mygroup'un_bir üyesi olarak SQL Server Management Studio'yu kullanarak yönetilen örneğe oturum açın. Yeni bir sorgu penceresi açın ve veritabanı arası SELECT deyimini çalıştırın:

    ```sql
    USE MyMITestDB
    SELECT * FROM MyMITestDB2..TestTable2
    GO
    ```

    **TestTable2**tablo sonuçlarını görmelisiniz.

## <a name="additional-scenarios-supported-for-azure-ad-server-principals-logins"></a>Azure AD sunucu ilkeleri (oturum açmalar) için desteklenen ek senaryolar

- SQL Agent yönetimi ve iş yürütmeleri Azure AD sunucu ilkeleri (oturum açmalar) için desteklenir.
- Veritabanı yedekleme ve geri yükleme işlemleri Azure AD sunucu ilkeleri (oturum açmalar) tarafından yürütülebilir.
- Azure AD sunucu ilkeleri (oturum açmalar) ve kimlik doğrulama olayları ile ilgili tüm deyimlerin [denetlenmesi.](sql-database-managed-instance-auditing.md)
- Sunucu rolünün `sysadmin` üyeleri olan Azure AD sunucu ilkeleri (oturum açmalar) için özel yönetici bağlantısı.
- Azure AD sunucu ilkeleri (oturum açmalar) [sqlcmd Utility](/sql/tools/sqlcmd-utility) ve [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms) aracı kullanılarak desteklenir.
- Oturum açma tetikleyicileri, Azure AD sunucu ilkelerinden (oturum açma) gelen oturum açma etkinlikleri için desteklenir.
- Service Broker ve DB postası Azure AD sunucu ilkeleri (oturum açmalar) kullanılarak ayarlanabilir.


## <a name="next-steps"></a>Sonraki adımlar

### <a name="enable-security-features"></a>Güvenlik özelliklerini etkinleştirme

Veritabanınızı korumanın kapsamlı bir listesi için aşağıdaki [yönetilen örnek özellikleri güvenlik özellikleri](sql-database-managed-instance.md#azure-sql-database-security-features) makalesine bakın. Aşağıdaki güvenlik özellikleri tartışılır:

- [Yönetilen örnek denetimi](sql-database-managed-instance-auditing.md) 
- [Her zaman şifrelenir](/sql/relational-databases/security/encryption/always-encrypted-database-engine)
- [Tehdit algılama](sql-database-managed-instance-threat-detection.md) 
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking)
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) 
- [Saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)

### <a name="managed-instance-capabilities"></a>Yönetilen örnek özellikleri

Yönetilen örnek özelliklerine tam bir genel bakış için bkz:

> [!div class="nextstepaction"]
> [Yönetilen örnek özellikleri](sql-database-managed-instance.md)
