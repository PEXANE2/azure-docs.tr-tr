---
title: SQL ServerWindows kullanıcılarını ve gruplarını T-SQL kullanarak yönetilen örneğe geçirin
description: SQL Server'ı şirket içi Windows kullanıcıları nın ve gruplarının yönetilen örneğe nasıl geçirileceği hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: seo-lt-2019
ms.topic: tutorial
author: GitHubMirek
ms.author: mireks
ms.reviewer: vanto
ms.date: 10/30/2019
ms.openlocfilehash: 2c8d7252b4e4ca8caa465727c0d2328c4aafaefb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74227918"
---
# <a name="tutorial-migrate-sql-server-on-premises-windows-users-and-groups-to-azure-sql-database-managed-instance-using-t-sql-ddl-syntax"></a>Öğretici: SQL Server'ı şirket içi Windows kullanıcılarını ve gruplarını T-SQL DDL sözdizimini kullanarak Azure SQL Veritabanı'na geçirme

> [!NOTE]
> Kullanıcıları ve grupları yönetilen örnek için bu makalede yönetilen lere geçirmek için kullanılan sözdizimi **genel önizlemededir.**

Bu makale, sql server'ınızdaki şirket içi Windows kullanıcılarınızı ve gruplarınızı T-SQL sözdizimini kullanarak varolan bir Azure SQL Veritabanı yönetilen örneğine geçirme işlemine geçebilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> - SQL Server için giriş oluşturma
> - Geçiş için bir test veritabanı oluşturma
> - Oturum açma, kullanıcı ve roller oluşturma
> - Veritabanınızı yönetilen örneğe yedekleme ve geri yükleme (MI)
> - ALTER USER sözdizimini kullanarak kullanıcıları el ile MI'ye geçirin
> - Yeni eşlenen kullanıcılarla kimlik doğrulamayı test etme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşullar geçerlidir:

- Windows etki alanı Azure Etkin Dizin (Azure AD) ile beslenir.
- Kullanıcı/grup oluşturmak için Active Directory'ye erişim.
- Şirket içi ortamınızda varolan bir SQL Server.
- Varolan yönetilen bir örnek. Bkz. [Hızlı Başlangıç: Azure SQL Veritabanı yönetilen örneği oluşturun.](sql-database-managed-instance-get-started.md)
  - Yönetilen `sysadmin` örnekte Bir, Azure AD oturum açmaları oluşturmak için kullanılmalıdır.
- [Yönetilen örnek için bir Azure AD yöneticisi oluşturun.](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance)
- Ağınızda yönetilen örneğinize bağlanabilirsiniz. Ek bilgi için aşağıdaki makalelere bakın: 
    - [Uygulamanızı Azure SQL Veritabanı yönetilen örneğine bağlayın](sql-database-managed-instance-connect-app.md)
    - [Hızlı başlatma: Şirket içinde Azure SQL Veritabanı Yönetilen Örneği'ne yerinde bağlantı oluşturma](sql-database-managed-instance-configure-p2s.md)
    - [Azure SQL Veritabanı yönetilen örneğinde genel uç noktayı yapılandırma](sql-database-managed-instance-public-endpoint-configure.md)

## <a name="t-sql-ddl-syntax"></a>T-SQL DDL sözdizimi

Aşağıda, SQL Server şirket içi Windows kullanıcılarını desteklemek için kullanılan T-SQL DDL sözdizimi ve Azure AD kimlik doğrulaması ile yönetilen örneğe geçişi gruplandırın.

```sql
-- For individual Windows users with logins 
ALTER USER [domainName\userName] WITH LOGIN = [loginName@domainName.com]; 

--For individual groups with logins 
ALTER USER [domainName\groupName] WITH LOGIN=[groupName] 
```

## <a name="arguments"></a>Bağımsız Değişkenler

_Etkialanıadı_</br>
Kullanıcının etki alanı adını belirtir.

_Username_</br>
Veritabanıiçinde tanımlanan kullanıcının adını belirtir.

_= loginName\@domainName.com_</br>
Bir kullanıcıyı Azure AD girişiyle yeniden eşler

_grupAdı_</br>
Veritabanıiçinde tanımlanan grubun adını belirtir.

## <a name="part-1-create-logins-for-sql-server-on-premises-users-and-groups"></a>Bölüm 1: SQL Server şirket içi kullanıcılar ve gruplar için oturum açma oluşturma

> [!IMPORTANT]
> Aşağıdaki sözdizimi, SQL Server'ınızda bir kullanıcı ve grup girişi oluşturur. Aşağıdaki sözdizimini yürütmeden önce kullanıcının ve grubun Etkin Dizin (AD) içinde bulunduğundan emin olmanız gerekir. </br> </br>
> Kullanıcılar: testUser1, testGroupUser </br>
> Grup: geçiş - testGroupUser AD geçiş grubuna ait olması gerekir

Aşağıdaki _örnekte, aadsqlmi_etki alanı altında _testUser1_ adlı bir hesap için SQL Server'da giriş yapılır. 

```sql
-- Sign into SQL Server as a sysadmin or a user that can create logins and databases
 
use master;  
go

-- Create Windows login
create login [aadsqlmi\testUser1] from windows;   
go; 

/** Create a Windows group login which contains one user [aadsqlmi\testGroupUser]. 
testGroupUser will need to be added to the migration group in Active Directory
**/
create login [aadsqlmi\migration] from windows;    
go; 


-- Check logins were created
select * from sys.server_principals; 
go; 
```

Bu sınama için bir veritabanı oluşturun.

```sql
-- Create a database called [migration]
create database migration 
go
```

## <a name="part-2-create-windows-users-and-groups-then-add-roles-and-permissions"></a>Bölüm 2: Windows kullanıcıları ve grupları oluşturun, ardından roller ve izinler ekleyin

Test kullanıcısını oluşturmak için aşağıdaki sözdizimini kullanın.

```sql
use migration;  
go 
 
-- Create Windows user [aadsqlmi\testUser1] with login 
create user [aadsqlmi\testUser1] from login [aadsqlmi\testUser1]; 
go 
```

Kullanıcı izinlerini kontrol edin:

```sql
-- Check the user in the Metadata 
select * from sys.database_principals; 
go 
 
-- Display the permissions – should only have CONNECT permissions
select user_name(grantee_principal_id), * from sys.database_permissions; 
go
```

Bir rol oluşturun ve test kullanıcınızı bu role atayın:

```sql 
-- Create a role with some permissions and assign the user to the role
create role UserMigrationRole; 
go

grant CONNECT, SELECT, View DATABASE STATE, VIEW DEFINITION to UserMigrationRole; 
go 

alter role UserMigrationRole add member [aadsqlmi\testUser1]; 
go 
``` 

Belirli bir role atanan kullanıcı adlarını görüntülemek için aşağıdaki sorguyu kullanın:

```sql
-- Display user name assigned to a specific role 
SELECT DP1.name AS DatabaseRoleName,    
   isnull (DP2.name, 'No members') AS DatabaseUserName    
 FROM sys.database_role_members AS DRM   
 RIGHT OUTER JOIN sys.database_principals AS DP1   
   ON DRM.role_principal_id = DP1.principal_id   
 LEFT OUTER JOIN sys.database_principals AS DP2   
   ON DRM.member_principal_id = DP2.principal_id   
WHERE DP1.type = 'R' 
ORDER BY DP1.name; 
```

Bir grup oluşturmak için aşağıdaki sözdizimini kullanın. Sonra rolü `db_owner`grup ekleyin.

```sql
-- Create Windows group
create user [aadsqlmi\migration] from login [aadsqlmi\migration]; 
go

-- ADD 'db_owner' role to this group
sp_addrolemember 'db_owner', 'aadsqlmi\migration'; 
go 

--Check the db_owner role for 'aadsqlmi\migration' group 
select is_rolemember('db_owner', 'aadsqlmi\migration')   
go 
-- Output  ( 1 means YES) 
```

Bir test tablosu oluşturun ve aşağıdaki sözdizimini kullanarak bazı veriler ekleyin:

```sql
-- Create a table and add data 
create table test ( a int, b int); 
go 

insert into test values (1,10) 
go 

-- Check the table values
select * from test; 
go
```

## <a name="part-3-backup-and-restore-the-individual-user-database-to-managed-instance"></a>Bölüm 3: Tek tek kullanıcı veritabanını yönetilen örneğe yedekleme ve geri yükleme

Yedekleme ve Geri Yükleme ile [Veritabanı kopyala](/sql/relational-databases/databases/copy-databases-with-backup-and-restore)makalesini kullanarak geçiş veritabanının yedek oluşturma sını oluşturun veya aşağıdaki sözdizimini kullanın:

```sql
use master; 
go 
backup database migration to disk = 'C:\Migration\migration.bak'; 
go
```

[Quickstart'ımızı izleyin: Veritabanını yönetilen bir örne geri yükleyin.](sql-database-managed-instance-get-started-restore.md)

## <a name="part-4-migrate-users-to-managed-instance"></a>Bölüm 4: Kullanıcıları yönetilen örnek için geçirin

> [!NOTE]
> Oluşturma değiştirildikten sonra yönetilen örnek işlevselliği için Azure AD yöneticisi. Daha fazla bilgi için [MI için Yeni Azure AD yöneticisi işlevine](sql-database-aad-authentication-configure.md#new-azure-ad-admin-functionality-for-mi)bakın.

Yönetilen örnekte geçiş işlemini tamamlamak için ALTER USER komutunu çalıştırın.

1. Yönetilen örneğini kullanarak yönetilen örneğinizde oturum açın. Ardından, aşağıdaki sözdizimini kullanarak yönetilen örnekte Azure AD oturumunuzu oluşturun. Daha fazla bilgi için [bkz: Azure AD sunucu ilkelerini (oturum açmalar) kullanarak Azure SQL Veritabanı'nda yönetilen örnek güvenliği.](sql-database-managed-instance-aad-security-tutorial.md)

    ```sql
    use master 
    go 
    
    -- Create login for AAD user [testUser1@aadsqlmi.net] 
    create login [testUser1@aadsqlmi.net] from external provider 
    go
    
    -- Create login for the Azure AD group [migration]. This group contains one user [testGroupUser@aadsqlmi.net] 
    create login [migration] from external provider 
    go 
     
    --Check the two new logins 
    select * from sys.server_principals 
    go
    ```

1. Geçişinizi doğru veritabanı, tablo ve ilkeler için denetleyin.

    ```sql
    -- Switch to the database migration that is already restored for MI 
    use migration;  
    go 
     
    --Check if the restored table test exist and contain a row 
    select * from test; 
    go 
     
    -- Check that the SQL on-premises Windows user/group exists  
    select * from sys.database_principals; 
    go 
    -- the old user aadsqlmi\testUser1 should be there 
    -- the old group aadsqlmi\migration should be there
    ```

1. Şirket içi kullanıcıyı Azure AD girişiyle eşlemek için ALTER USER sözdizimini kullanın.

    ```sql
    /** Execute the ALTER USER command to alter the Windows user [aadsqlmi\testUser1]
    to map to the Azure AD user testUser1@aadsqlmi.net
    **/
    alter user [aadsqlmi\testUser1] with login = [testUser1@aadsqlmi.net]; 
    go

    -- Check the principal
    select * from sys.database_principals; 
    go 
    -- New user testUser1@aadsqlmi.net should be there instead 
     
    --Check new user permissions  - should only have CONNECT permissions
    select user_name(grantee_principal_id), * from sys.database_permissions; 
    go

    -- Check a specific role   
    -- Display Db user name assigned to a specific role 
    SELECT DP1.name AS DatabaseRoleName,    
    isnull (DP2.name, 'No members') AS DatabaseUserName    
    FROM sys.database_role_members AS DRM   
    RIGHT OUTER JOIN sys.database_principals AS DP1   
    ON DRM.role_principal_id = DP1.principal_id   
    LEFT OUTER JOIN sys.database_principals AS DP2   
    ON DRM.member_principal_id = DP2.principal_id   
    WHERE DP1.type = 'R' 
    ORDER BY DP1.name;
    ```

1. Şirket içi grubu Azure AD girişiyle eşlemek için ALTER USER sözdizimini kullanın.

    ```sql
    /** Execute ALTER USER command to alter the Windows group [aadsqlmi\migration]
    to the Azure AD group login [migration]
    **/
    alter user [aadsqlmi\migration] with login = [migration]; 
    -- old group migration is changed to Azure AD migration group
    go

    -- Check the principal
    select * from sys.database_principals; 
    go 
     
    --Check the group permission - should only have CONNECT permissions
    select user_name(grantee_principal_id), * from sys.database_permissions; 
    go 
     
    --Check the db_owner role for 'aadsqlmi\migration' user 
    select is_rolemember('db_owner', 'migration')   
    go 
    -- Output 1 means 'YES'
    ```

## <a name="part-5-testing-azure-ad-user-or-group-authentication"></a>Bölüm 5: Azure AD kullanıcılarını veya grup kimlik doğrulamasını sınama

ALTER USER sözdizimini kullanarak daha önce Azure AD girişine eşlenen kullanıcıyı kullanarak yönetilen örneğin kimlik doğrulamasını test edin.
 
1. MI aboneliğinizi kullanarak federe VM'ye giriş yapın`aadsqlmi\testUser1`
1. SQL Server Management Studio 'yı (SSMS) kullanarak, **Active Directory Integrated** kimlik `migration`doğrulamasını kullanarak yönetilen örneğinizde oturum açın ve veritabanına bağlanın.
    1. Ayrıca SSMS seçeneği testUser1@aadsqlmi.net Active Directory ile kimlik bilgilerini kullanarak oturum açabilir **- MFA desteği ile Evrensel**. Ancak, bu durumda, Tek İşaret Açma mekanizmasını kullanamazsınız ve parola yazmanız gerekir. Yönetilen örneğinize giriş yapmak için federe VM kullanmanız gerekmez.
1. Rol üyesi **SELECT'in**bir parçası olarak, `test` tablodan

    ```sql
    Select * from test  --  and see one row (1,10)
    ```


Bir Windows grubunun `migration`üyesini kullanarak yönetilen bir örneğin kimlik doğrulamasını test edin. Kullanıcının `aadsqlmi\testGroupUser` geçişten önce gruba `migration` eklenmesi gerekirdi.

1. MI aboneliğinizi kullanarak federe VM'ye giriş yapın`aadsqlmi\testGroupUser` 
1. **Active Directory Integrated** kimlik doğrulamaile SSMS kullanarak MI sunucusuna ve veritabanına bağlanın`migration`
    1. Ayrıca SSMS seçeneği testGroupUser@aadsqlmi.net Active Directory ile kimlik bilgilerini kullanarak oturum açabilir **- MFA desteği ile Evrensel**. Ancak, bu durumda, Tek İşaret Açma mekanizmasını kullanamazsınız ve parola yazmanız gerekir. Yönetilen örneğinize giriş yapmak için federe VM kullanmanız gerekmez. 
1. Rolün `db_owner` bir parçası olarak, yeni bir tablo oluşturabilirsiniz.

    ```sql
    -- Create table named 'new' with a default schema
    Create table dbo.new ( a int, b int)
    ```
                             
> [!NOTE] 
> Azure SQL DB için bilinen bir tasarım sorunu nedeniyle, bir grubun üyesi olarak yürütülen bir tablo deyimi oluşturmak aşağıdaki hatayla başarısız olur: </br> </br>
> `Msg 2760, Level 16, State 1, Line 4 
The specified schema name "testGroupUser@aadsqlmi.net" either does not exist or you do not have permission to use it.` </br> </br>
> Geçerli geçici çözüm dbo.new> yukarıdaki durumda mevcut bir şema ile <bir tablo oluşturmaktır

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: SQL Server'ı DMS kullanarak Sql Sql Veritabanı yönetilen bir örneğini çevrimdışı olarak geçirin](../dms/tutorial-sql-server-to-managed-instance.md?toc=/azure/sql-database/toc.json)