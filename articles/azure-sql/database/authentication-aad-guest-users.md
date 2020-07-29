---
title: Azure AD Konuk kullanıcıları oluşturma
description: Azure SQL veritabanı, Azure SQL yönetilen örneği ve Azure SYNAPSE Analytics 'te Azure AD gruplarını kullanmadan Azure AD Konuk kullanıcıları oluşturma ve bunları Azure AD yöneticisi olarak ayarlama
ms.service: sql-db-mi
ms.subservice: security
ms.custom: azure-synapse
ms.topic: conceptual
author: shohamMSFT
ms.author: shohamd
ms.reviewer: vanto
ms.date: 07/27/2020
ms.openlocfilehash: b3abda9787654734b24cc27987921ef40d8070fc
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294462"
---
# <a name="create-azure-ad-guest-users-and-set-as-an-azure-ad-admin"></a>Azure AD konuk kullanıcıları oluşturma ve bir Azure AD yöneticisi olarak ayarlama

[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

> [!NOTE]
> Bu makale **genel önizlemede**.

Azure Active Directory (Azure AD) Konuk kullanıcılar, mevcut Azure AD 'ye diğer Azure Active Directory 'lerden veya bunun dışından aktarılmış kullanıcılardır. Örneğin, Konuk kullanıcılar diğer Azure Active Directory 'lerden veya * \@ Outlook.com*, * \@ hotmail.com*, * \@ Live.com*veya * \@ Gmail.com*gibi hesaplardan kullanıcıları içerebilir. Bu makalede, bir Azure AD Konuk kullanıcısının nasıl oluşturulacağı ve bu kullanıcının Azure SQL mantıksal sunucusu için Azure AD yöneticisi olarak nasıl ayarlanacağı, Konuk kullanıcının Azure AD içindeki bir grubun parçası olması gerekmeden nasıl yapılacağı gösterilir.

## <a name="feature-description"></a>Özellik açıklaması

Bu özellik, yalnızca konuk kullanıcıların Azure AD 'de oluşturulan bir grubun üyesi olduklarında Azure SQL veritabanı, SQL yönetilen örneği veya Azure SYNAPSE Analytics 'e bağlanmasına izin veren geçerli sınırlamayı durdurur. Belirli bir veritabanında [Kullanıcı oluşturma (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql) ifadesini kullanarak el ile bir kullanıcıya eşlenmesi gerekir. Konuk kullanıcıyı içeren Azure AD grubu için bir veritabanı kullanıcısı oluşturulduktan sonra, Konuk Kullanıcı, MFA kimlik doğrulamasıyla Azure Active Directory kullanarak veritabanında oturum açabilir. Bu **genel önizlemenin**bir parçası olarak, Konuk KULLANıCıLAR doğrudan SQL VERITABANı, SQL yönetilen örneği veya Azure SYNAPSE 'e, önce bunları BIR Azure AD grubuna ekleme ve ardından bu Azure AD grubu için bir veritabanı kullanıcısı oluşturma gerekliliği olmadan oluşturulabilir ve doğrudan bağlanabilir.

Bu özelliğin bir parçası olarak, Azure AD Konuk kullanıcısını doğrudan Azure SQL mantıksal sunucusu için bir AD yöneticisi olarak ayarlama olanağınız da vardır. Konuk kullanıcının bir Azure AD grubunun parçası olduğu mevcut işlevler ve bu grup daha sonra Azure SQL mantıksal sunucusu için Azure AD yöneticisi etkilenmediğinden ayarlanabilir. Bir Azure AD grubunun parçası olan veritabanındaki Konuk kullanıcılar da bu değişiklikten etkilenmez.

Azure AD gruplarını kullanarak Konuk kullanıcılar için mevcut destek hakkında daha fazla bilgi için bkz. [multi-factor Azure Active Directory kimlik doğrulaması kullanma](authentication-mfa-ssms-overview.md).

## <a name="prerequisite"></a>Önkoşul

- Azure SQL mantıksal sunucusu için bir konuk kullanıcıyı Azure AD yöneticisi olarak ayarlamak üzere PowerShell kullanılırken, [az. SQL 2.9.0](https://www.powershellgallery.com/packages/Az.Sql/2.9.0) modülü veya üzeri gereklidir.

## <a name="create-database-user-for-azure-ad-guest-user"></a>Azure AD Konuk Kullanıcı için veritabanı kullanıcısı oluşturma 

Bir Azure AD Konuk kullanıcısını kullanarak veritabanı kullanıcısı oluşturmak için bu adımları izleyin.

### <a name="create-guest-user-in-sql-database-and-azure-synapse"></a>SQL veritabanı ve Azure SYNAPSE 'de Konuk Kullanıcı oluşturma

1. Konuk kullanıcının (örneğin, `user1@gmail.com` ) Azure AD 'nize zaten eklendiğinden ve veritabanı sunucusu için bir Azure AD yöneticisinin ayarlandığından emin olun. Azure Active Directory kimlik doğrulaması için bir Azure AD yöneticisinin olması gerekir.

1. Azure AD yöneticisi veya Kullanıcı oluşturmak için yeterli SQL izinlerine sahip bir Azure AD kullanıcısı olarak SQL veritabanına bağlanın ve Konuk kullanıcının eklenmesi gereken veritabanında aşağıdaki komutu çalıştırın:

    ```sql
    CREATE USER [user1@gmail.com] FROM EXTERNAL PROVIDER
    ```

1. Artık Konuk Kullanıcı için oluşturulmuş bir veritabanı kullanıcısı olmalıdır `user1@gmail.com` .

1. Veritabanı kullanıcısının başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın:

    ```sql
    SELECT * FROM sys.database_principals
    ```

1. `user1@gmail.com` **MFA ile Azure Active Directory**kimlik doğrulama yöntemini kullanarak veritabanını [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Konuk Kullanıcı olarak açın ve oturum açın. Daha fazla bilgi için bkz. [multi-factor Azure Active Directory kimlik doğrulaması kullanma](authentication-mfa-ssms-overview.md).

### <a name="create-guest-user-in-sql-managed-instance"></a>SQL yönetilen örneği 'nde Konuk kullanıcı oluştur

> [!NOTE]
> SQL yönetilen örneği, Azure AD kullanıcıları için oturum açma işlemlerini ve ayrıca Azure AD dahil veritabanı kullanıcılarını destekler. Aşağıdaki adımlarda, SQL yönetilen örneği 'nde bir Azure AD Konuk kullanıcısı için oturum açma ve Kullanıcı oluşturma işlemleri gösterilmektedir. SQL yönetilen örneği 'nde, [SQL veritabanı ve Azure SYNAPSE 'de Konuk Kullanıcı oluşturma](#create-guest-user-in-sql-database-and-azure-synapse) bölümünde yer alan yöntemi kullanarak [içerilen bir veritabanı kullanıcısı](https://docs.microsoft.com/sql/relational-databases/security/contained-database-users-making-your-database-portable) oluşturmayı da tercih edebilirsiniz.

1. Konuk kullanıcının (örneğin, `user1@gmail.com` ) Azure AD 'nize zaten eklendiğinden ve SQL yönetilen örnek sunucusu için bir Azure AD yöneticisinin ayarlandığından emin olun. Azure Active Directory kimlik doğrulaması için bir Azure AD yöneticisinin olması gerekir.

1. Azure AD yöneticisi veya Kullanıcı oluşturmak için yeterli SQL izinlerine sahip bir Azure AD kullanıcısı olarak SQL yönetilen örnek sunucusuna bağlanın ve `master` Konuk Kullanıcı için bir oturum açma oluşturmak üzere veritabanında aşağıdaki komutu çalıştırın:

    ```sql
    CREATE LOGIN [user1@gmail.com] FROM EXTERNAL PROVIDER
    ```

1. Artık veritabanında Konuk Kullanıcı için oluşturulmuş bir oturum açma bilgisi olmalıdır `user1@gmail.com` `master` .

1. Oturum açmanın başarıyla oluşturulduğunu doğrulamak için aşağıdaki komutu çalıştırın:

    ```sql
    SELECT * FROM sys.server_principals
    ```

1. Konuk kullanıcının eklenmesi gereken veritabanında aşağıdaki komutu çalıştırın: 

    ```sql
    CREATE USER [user1@gmail.com] FROM LOGIN [user1@gmail.com]
    ```

1. Artık Konuk Kullanıcı için oluşturulmuş bir veritabanı kullanıcısı olmalıdır `user1@gmail.com` .

1. `user1@gmail.com` **MFA ile Azure Active Directory**kimlik doğrulama yöntemini kullanarak veritabanını [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) kullanarak Konuk Kullanıcı olarak açın ve oturum açın. Daha fazla bilgi için bkz. [multi-factor Azure Active Directory kimlik doğrulaması kullanma](authentication-mfa-ssms-overview.md).

## <a name="setting-a-guest-user-as-an-azure-ad-admin"></a>Konuk kullanıcıyı Azure AD yöneticisi olarak ayarlama

Azure AD Konuk kullanıcısını SQL mantıksal sunucusu için Azure AD yöneticisi olarak ayarlamak için aşağıdaki adımları izleyin.

### <a name="set-azure-ad-admin-for-sql-database-and-azure-synapse"></a>SQL veritabanı ve Azure SYNAPSE için Azure AD yöneticisi 'ni ayarlama

1. Konuk kullanıcının (örneğin, `user1@gmail.com` ) Azure AD 'nize zaten eklendiğinden emin olun.

1. Konuk kullanıcıyı Azure SQL mantıksal sunucunuza yönelik Azure AD yöneticisi olarak eklemek için aşağıdaki PowerShell komutunu çalıştırın:

    - `<ResourceGroupName>`Azure SQL mantıksal sunucusu Içeren Azure Kaynak grubu adınızla değiştirin.
    - `<ServerName>`Azure SQL mantıksal sunucu adınızla değiştirin. Sunucunuzun adı ise `myserver.database.windows.net` `<Server Name>` ile değiştirin `myserver` .
    - `<DisplayNameOfGuestUser>`Konuk Kullanıcı adınızla değiştirin.

    ```powershell
    Set-AzSqlServerActiveDirectoryAdministrator -ResourceGroupName <ResourceGroupName> -ServerName <ServerName> -DisplayName <DisplayNameOfGuestUser>
    ```

    Konuk kullanıcıyı Azure SQL mantıksal sunucunuz için bir Azure AD yöneticisi olarak ayarlamak üzere [az SQL Server ad-admin](https://docs.microsoft.com/cli/azure/sql/server/ad-admin) Azure CLI komutunu da kullanabilirsiniz.

### <a name="set-azure-ad-admin-for-sql-managed-instance"></a>SQL yönetilen örneği için Azure AD yöneticisi 'ni ayarlama

1. Konuk kullanıcının (örneğin, `user1@gmail.com` ) Azure AD 'nize zaten eklendiğinden emin olun.

1. [Azure Portal](https://portal.azure.com)gidin ve **Azure Active Directory** kaynağına gidin. **Yönet**altında, **Kullanıcılar** bölmesine gidin. Konuk Kullanıcı ' yı seçin ve kaydedin `Object ID` . 

1. Konuk kullanıcıyı SQL yönetilen örneğiniz için Azure AD yöneticisi olarak eklemek için aşağıdaki PowerShell komutunu çalıştırın:

    - `<ResourceGroupName>`SQL yönetilen örneğini Içeren Azure Kaynak grubu adınızla değiştirin.
    - `<ManagedInstanceName>`SQL yönetilen örnek adınızla değiştirin.
    - `<DisplayNameOfGuestUser>`Konuk Kullanıcı adınızla değiştirin.
    - `<AADObjectIDOfGuestUser>` `Object ID` Daha önce toplanan ile değiştirin.

    ```powershell
    Set-AzSqlInstanceActiveDirectoryAdministrator -ResourceGroupName <ResourceGroupName> -InstanceName "<ManagedInstanceName>" -DisplayName <DisplayNameOfGuestUser> -ObjectId <AADObjectIDOfGuestUser>
    ```

    Konuk kullanıcıyı SQL yönetilen örneğiniz için bir Azure AD yöneticisi olarak ayarlamak üzere [az SQL mı ad-admin](https://docs.microsoft.com/cli/azure/sql/mi/ad-admin) Azure CLI komutunu da kullanabilirsiniz.

## <a name="limitations"></a>Sınırlamalar

Azure portal, SQL veritabanı, SQL yönetilen örneği ve Azure SYNAPSE için Azure AD yöneticisi olarak bir Azure AD Konuk kullanıcısının seçilmesine engel olan bir sınırlama vardır. * \@ Outlook.com*, * \@ hotmail.com*, * \@ Live.com*veya * \@ Gmail.com*gibi Azure AD 'niz dışındaki Konuk hesaplarında, ad yönetici Seçicisi bu hesapları gösterir, ancak gri renkte görünür ve seçilemez. Azure AD yöneticisi 'ni ayarlamak için yukarıda listelenen [PowerShell veya CLI komutlarını](#setting-a-guest-user-as-an-azure-ad-admin) kullanın. Alternatif olarak, Konuk kullanıcıyı içeren bir Azure AD grubu, SQL mantıksal sunucusu için Azure AD yöneticisi olarak ayarlanabilir.

Bu özelliğin genel kullanıma sunulmasından önce SQL veritabanı ve Azure SYNAPSE için bu işlev etkinleştirilecek.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL ile Azure AD kimlik doğrulamasını yapılandırma ve yönetme](authentication-aad-configure.md)
- [Multi-Factor Azure Active Directory kimlik doğrulamasını kullanma](authentication-mfa-ssms-overview.md)
- [Kullanıcı oluşturma (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql)
