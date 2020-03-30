---
title: Azure SSIS Tümleştirme Çalışma Süresi için AAD'yi etkinleştirme
description: Bu makalede, Azure Veri Fabrikası'nın yönetilen kimliğiyle Azure-SSIS Tümleştirme Çalışma Zamanı'nı oluşturmak için Azure Etkin Dizin kimlik doğrulamasını nasıl etkinleştirin.
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.devlang: powershell
ms.topic: conceptual
author: swinarko
ms.author: sawinark
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 5/14/2019
ms.openlocfilehash: 70367a38fbf7b59486e2eaaf6c05634aa7575869
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79260715"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime için Azure Active Directory kimlik doğrulamasını etkinleştirme

Bu makalede, Azure Veri Fabrikanız (ADF) için yönetilen kimlikle Azure Etkin Dizin (Azure AD) kimlik doğrulaması nasıl etkinleştirilebilirsiniz ve geleneksel kimlik doğrulama yöntemleri (SQL kimlik doğrulama gibi) yerine nasıl kullanacağınızı gösterir:

- Azure SQL Veritabanı sunucusunda/Yönetilen Örneği'nde sizin adınıza Sırayla SSIS katalog veritabanı (SSISDB) sağlayacak bir Azure-SSIS Tümleştirme Çalışma Süresi (IR) oluşturun.

- Azure-SSIS IR'de SSIS paketlerini çalıştırırken çeşitli Azure kaynaklarına bağlanın.

ADF'nizin yönetilen kimliği hakkında daha fazla bilgi için [Veri Fabrikası için Yönetilen identiy'e](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity)bakın.

> [!NOTE]
>-  Bu senaryoda, ADF'niz için yönetilen kimliği içeren Azure AD kimlik doğrulaması yalnızca SSIS IR'nizin oluşturulmasında ve sonraki başlangıç işlemlerinde kullanılır ve bu da ssisdb'ye bağlanır. SSIS paket yürütmeleri için, SSIS IR'niz SSISDB sağlama sırasında oluşturulan tam olarak yönetilen hesaplarla SQL kimlik doğrulamasını kullanarak SSISDB'ye bağlanmaya devam eder.
>-  SSIS IR'nizi SQL kimlik doğrulaması kullanarak zaten oluşturduysanız, şu anda PowerShell üzerinden Azure AD kimlik doğrulamasını kullanacak şekilde yeniden yapılandıramazsınız, ancak bunu Azure portalı/ADF uygulaması üzerinden yapabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL Veritabanı'nda Azure AD'yi etkinleştirme

Azure SQL Veritabanı sunucusu, Azure AD kullanıcısıyla veritabanı oluşturmayı destekler. İlk olarak, üye olarak ADF'niz için yönetilen kimliğe sahip bir Azure REKLAM grubu oluşturmanız gerekir. Ardından, Azure SQL Veritabanı sunucunuz için Bir Azure AD kullanıcısını Active Directory yöneticisi olarak ayarlamanız ve ardından bu kullanıcıyı kullanarak SQL Server Management Studio'da (SSMS) bağlamanız gerekir. Son olarak, Azure AD grubunu temsil eden bir kullanıcı oluşturmanız gerekir, böylece ADF'nizin yönetilen kimliği Azure-SSIS IR tarafından sizin adınıza SSISDB oluşturmak için kullanılabilir.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>Üye olarak ADF'niz için yönetilen kimliğe sahip bir Azure REKLAM grubu oluşturma

Varolan bir Azure REKLAM grubunu kullanabilir veya Azure AD PowerShell'i kullanarak yeni bir grup oluşturabilirsiniz.

1.  Azure [AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modüllerini yükleyin.

2.  Oturum aç, `Connect-AzureAD`bir grup oluşturmak için aşağıdaki cmdlet'i çalıştırın ve bir değişkene kaydedin:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Sonuç, değişken değerini de görüntüleyen aşağıdaki örneğe benzer:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  ADF'nizin yönetilen kimliğini gruba ekleyin. Başlıca Yönetilen Kimlik Nesnekimliği 'ni almak [için Veri Fabrikası için Yönetilen identiy](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) makalesini takip edebilirsiniz (örn. 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, ancak bu amaçla Yönetilen Kimlik Uygulama Kimliği kullanmayın).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Daha sonra grup üyeliğini de kontrol edebilirsiniz.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Azure SQL Veritabanı sunucusu için Azure AD kimlik doğrulamasını yapılandırma

Aşağıdaki adımları kullanarak [Azure AD kimlik doğrulamasını SQL ile yapılandırabilir ve yönetebilirsiniz:](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)

1.  Azure portalında, sol daki gezinmeden **Tüm hizmetler** -> **SQL sunucularını** seçin.

2.  Azure AD kimlik doğrulaması ile yapılandırmak üzere Azure SQL Veritabanı sunucunuzu seçin.

3.  Bıçağın **Ayarlar** bölümünde Active **Directory yöneticisini**seçin.

4.  Komut çubuğunda, **yöneticiyi ayarla'yı**seçin.

5.  Sunucunun yöneticisi olmak için bir Azure AD kullanıcı hesabı seçin ve ardından **Seç'i seçin.**

6.  Komut çubuğunda **Kaydet'i seçin.**

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Azure AD grubunu temsil eden Azure SQL Veritabanı sunucusunda bulunan bir kullanıcı oluşturma

Bu sonraki adım için [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) gerekir.

1. SMS'leri başlatın.

2. **Sunucuya Bağlan** iletişim kutusunda, **Sunucu adı** alanına Azure SQL Veritabanı sunucu adınızı girin.

3. Kimlik **Doğrulama** alanında, **MFA desteğiyle Active Directory - Universal'i** seçin (diğer iki Active Directory kimlik doğrulama türünü de kullanabilirsiniz, Azure [AD kimlik doğrulamasını SQL ile yapılandırma ve yönetme'yi](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)görün).

4. Kullanıcı **adı** alanına, sunucu yöneticisi olarak ayarladığınız Azure AD hesabının adını testuser@xxxonline.comgirin, örneğin.

5. **Bağlan'ı** seçin ve oturum açma işlemini tamamlayın.

6. Object **Explorer'da,** **Veritabanları** -> **Sistem Veritabanları** klasörünü genişletin.

7. **Ana** veritabanına sağ tıklayın ve **Yeni sorgu'u**seçin.

8. Sorgu penceresinde, aşağıdaki T-SQL komutunu girin **Execute** ve araç çubuğunda Yürüt'''ün'u seçin.

   ```sql
   CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
   ```

   Komut, grubu temsil edecek bir kullanıcı oluşturarak başarıyla tamamlanmalıdır.

9. Sorgu penceresini temizleyin, aşağıdaki T-SQL **Execute** komutunu girin ve araç çubuğunda Yürüt'''ün'u seçin.

   ```sql
   ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
   ```

   Komut, içerdiği kullanıcıya veritabanı (SSISDB) oluşturma olanağı vererek başarıyla tamamlanmalıdır.

10. SSISDB'niz SQL kimlik doğrulaması kullanılarak oluşturulduysa ve azure-SSIS IR'nizin erişmek için Azure-SSIS kimlik doğrulamasını kullanmak için geçiş yapmak istiyorsanız, **SSISDB** veritabanına sağ tıklayın ve **Yeni sorgu'u**seçin.

11. Sorgu penceresinde, aşağıdaki T-SQL komutunu girin **Execute** ve araç çubuğunda Yürüt'''ün'u seçin.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    Komut, grubu temsil edecek bir kullanıcı oluşturarak başarıyla tamamlanmalıdır.

12. Sorgu penceresini temizleyin, aşağıdaki T-SQL **Execute** komutunu girin ve araç çubuğunda Yürüt'''ün'u seçin.

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    Komut, içerdiği kullanıcıya SSISDB'ye erişme olanağı vererek başarıyla tamamlanmalıdır.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örnek'te Azure AD'yi etkinleştirme

Azure SQL Veritabanı Yönetilen Örneği, Doğrudan ADF'niz için yönetilen kimliğe sahip bir veritabanı oluşturmayı destekler. ADF'niz için yönetilen kimliğe azure reklam grubuna katılmanız veya Yönetilen Örneğinizde bu grubu temsil eden bir kullanıcı oluşturmanız gerekmez.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için Azure AD kimlik doğrulamasını yapılandırma

[Yönetilen Örneğiniz için Azure Etkin Dizin yöneticisi sağlama'daki](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance)adımları izleyin.

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği'nde kullanıcı olarak ADF'niz için yönetilen kimliği ekleme

Bu sonraki adım için [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) gerekir.

1.  SMS'leri başlatın.

2.  **Sysadmin**olan bir SQL Server hesabı kullanarak Yönetilen Örneğinize bağlanın. Bu, Azure SQL Veritabanı Yönetilen Örneği için Azure AD sunucu ilkeleri (oturum açmalar) GA olduğunda kaldırılacak geçici bir sınırlamadır. Oturum açmak için bir Azure AD yöneticisi hesabı kullanmaya çalışırsanız aşağıdaki hatayı görürsünüz: Msg 15247, Düzey 16, Durum 1, Satır 1 Kullanıcının bu eylemi gerçekleştirme izni yoktur.

3.  Object **Explorer'da,** **Veritabanları** -> **Sistem Veritabanları** klasörünü genişletin.

4.  **Ana** veritabanına sağ tıklayın ve **Yeni sorgu'u**seçin.

5.  Sorgu penceresinde, adf'nizin yönetilen kimliğini kullanıcı olarak eklemek için aşağıdaki T-SQL komut dosyasını çalıştırın

    ```sql
    CREATE LOGIN [{your ADF name}] FROM EXTERNAL PROVIDER
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{your ADF name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{your ADF name}]
    ```
    
    Komut, ADF'niz için yönetilen kimliğe veritabanı (SSISDB) oluşturma olanağı vererek başarıyla tamamlanmalıdır.

6.  SSISDB'niz SQL kimlik doğrulaması kullanılarak oluşturulduysa ve azure-SSIS IR'nizin erişmek için Azure-SSIS kimlik doğrulamasını kullanmak için geçiş yapmak istiyorsanız, **SSISDB** veritabanına sağ tıklayın ve **Yeni sorgu'u**seçin.

7.  Sorgu penceresinde, aşağıdaki T-SQL komutunu girin **Execute** ve araç çubuğunda Yürüt'''ün'u seçin.

    ```sql
    CREATE USER [{your ADF name}] FOR LOGIN [{your ADF name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{your ADF name}]
    ```

    Komut, ADF'niz için yönetilen kimliğe SSISDB'ye erişme olanağı vererek başarıyla tamamlanmalıdır.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Azure portalı/ADF uygulamasında Azure-SSIS IR sağlama

Azure-SSIS IR'nizi Azure portalı/ADF **uygulamasında, SQL Ayarları** sayfasında sizlerle diğinizde, **ADF seçeneğiniz için yönetilen kimlikle AAD kimlik doğrulamasını kullan'ı** seçin. Aşağıdaki ekran görüntüsü, SSISDB barındırma Azure SQL Veritabanı sunucusu ile IR ayarlarını gösterir. Yönetilen Örnek barındırma SSISDB ile IR için Katalog **Veritabanı Hizmet Katmanı** ve Azure hizmetlerinin erişim **ayarlarına izin** verme geçerli değildir, diğer ayarlar ise aynıdır.

Azure-SSIS IR nasıl oluşturulacak hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)

![Azure-SSIS tümleştirme çalışma zamanı için ayarlar](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>PowerShell ile Azure-SSIS IR Sağlama

Azure-SSIS IR'nizi PowerShell ile birlikte sağlamak için aşağıdaki leri yapın:

1.  [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) modüllerini yükleyin.

2.  Komut dosyanızda parametre `CatalogAdminCredential` ayarlamayın. Örnek:

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
    ```

## <a name="run-ssis-packages-with-managed-identity-authentication"></a>Yönetilen Kimlik Kimlik Doğrulaması ile SSIS Paketlerini Çalıştırın

Azure-SSIS IR'de SSIS paketlerini çalıştırdığınızda, çeşitli Azure kaynaklarına bağlanmak için yönetilen kimlik doğrulamasını kullanabilirsiniz. Şu anda, aşağıdaki bağlantı yöneticilerinde yönetilen kimlik doğrulamasını zaten destekledik.

- [OLE DB Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/ole-db-connection-manager#managed-identities-for-azure-resources-authentication)

- [ADO.NET Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/ado-net-connection-manager#managed-identities-for-azure-resources-authentication)

- [Azure Depolama Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-storage-connection-manager#managed-identities-for-azure-resources-authentication)
