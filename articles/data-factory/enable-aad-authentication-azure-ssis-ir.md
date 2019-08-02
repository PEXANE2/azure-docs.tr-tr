---
title: Azure-SSIS Integration Runtime için Azure Active Directory kimlik doğrulamasını etkinleştirme | Microsoft Docs
description: Bu makalede, Azure-SSIS Integration Runtime oluşturmak için Azure Data Factory yönetilen kimlik ile Azure Active Directory kimlik doğrulamasının nasıl etkinleştirileceği açıklanır.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 5/14/2019
author: swinarko
ms.author: sawinark
manager: craigg
ms.openlocfilehash: 1e55d1878b1a5616d467f2fa27b1b20132d5e77c
ms.sourcegitcommit: f5cc71cbb9969c681a991aa4a39f1120571a6c2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68516996"
---
# <a name="enable-azure-active-directory-authentication-for-azure-ssis-integration-runtime"></a>Azure-SSIS Integration Runtime için Azure Active Directory kimlik doğrulamasını etkinleştirme

Bu makalede, Azure Data Factory (ADF) için yönetilen kimlikle Azure Active Directory (Azure AD) kimlik doğrulamasının nasıl etkinleştirileceği ve SQL kimlik doğrulaması yerine bunu kullanarak, sağlaması yapılacak bir Azure-SSIS Integration Runtime (IR) oluşturma işlemi gösterilmektedir Azure SQL veritabanı sunucusunda/yönetilen örnekte, SSIS Katalog veritabanı (SSıSDB).

ADF 'nizin yönetilen kimliği hakkında daha fazla bilgi için bkz. [Managed identiy for Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
>-  Bu senaryoda, ADF 'niz için yönetilen kimliğe sahip Azure AD kimlik doğrulaması yalnızca, SSIS IR 'nizin oluşturulması ve sonraki başlangıç işlemlerinde ve SSıSDB 'nin sağlaması ve SSD 'ye bağlanması için kullanılır. SSIS paketi yürütmeleri için SSIS IR 'niz, SSıSDB sağlama sırasında oluşturulan tam olarak yönetilen hesaplarla SQL kimlik doğrulaması kullanarak SSSıSDB 'ye bağlanmaya devam edecektir.
>-  SSIS IR 'nizi SQL kimlik doğrulaması kullanarak zaten oluşturduysanız, bunu şu anda PowerShell aracılığıyla Azure AD kimlik doğrulaması kullanacak şekilde yeniden yapılandıramazsınız, ancak bunu Azure portal/ADF uygulaması aracılığıyla yapabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure SQL veritabanı 'nda Azure AD 'yi etkinleştirme

Azure SQL veritabanı sunucusu, bir Azure AD kullanıcısı ile veritabanı oluşturmayı destekler. İlk olarak, ADF 'niz için yönetilen kimliğe sahip bir Azure AD grubu oluşturmanız gerekir. Daha sonra, Azure SQL veritabanı sunucunuz için Active Directory yöneticisi olarak bir Azure AD kullanıcısı ayarlamanız ve bu kullanıcıyı kullanarak SQL Server Management Studio (SSMS) üzerinde bu sunucuya bağlanmanız gerekir. Son olarak, Azure AD grubunu temsil eden bir kapsanan Kullanıcı oluşturmanız gerekir, bu nedenle ADF 'niz için yönetilen kimlik sizin adınıza SSıSDB oluşturmak için Azure-SSIS IR tarafından kullanılabilir.

### <a name="create-an-azure-ad-group-with-the-managed-identity-for-your-adf-as-a-member"></a>ADF 'niz için yönetilen kimlikle bir üye olarak bir Azure AD grubu oluşturun

Azure AD PowerShell kullanarak mevcut bir Azure AD grubunu kullanabilir veya yeni bir tane oluşturabilirsiniz.

1.  Yükleme [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) modülü.

2.  Kullanarak `Connect-AzureAD`oturum açın, bir grup oluşturmak ve bir değişkende kaydetmek için aşağıdaki cmdlet 'i çalıştırın:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    Sonuç aşağıdaki örneğe benzer şekilde görünür ve aynı zamanda değişken değerini de görüntüler:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  ADF 'niz için yönetilen kimliği gruba ekleyin. Asıl yönetilen kimlik nesne KIMLIĞINI (ör. 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc) almak için [Data Factory yönetilen identiy](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) makalesini takip edebilirsiniz.

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Grup üyeliğini daha sonra da denetleyebilirsiniz.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="configure-azure-ad-authentication-for-azure-sql-database-server"></a>Azure SQL veritabanı sunucusu için Azure AD kimlik doğrulamasını yapılandırma

Aşağıdaki adımları kullanarak, [SQL Ile Azure AD kimlik doğrulamasını yapılandırabilir ve yönetebilirsiniz](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) :

1.  Azure Portal, sol taraftaki gezinmede **tüm hizmetler** -> **SQL Server** ' ı seçin.

2.  Azure AD kimlik doğrulamasıyla yapılandırılacak Azure SQL veritabanı sunucunuzu seçin.

3.  Dikey pencerenin **Ayarlar** bölümünde **yönetici Active Directory**' yi seçin.

4.  Komut çubuğunda **yönetici ayarla**' yı seçin.

5.  Sunucu Yöneticisi yapmak için bir Azure AD Kullanıcı hesabı seçin ve ardından Seç ' i seçin **.**

6.  Komut çubuğunda Kaydet ' i seçin **.**

### <a name="create-a-contained-user-in-azure-sql-database-server-representing-the-azure-ad-group"></a>Azure AD grubunu temsil eden Azure SQL veritabanı sunucusu 'nda kapsanan Kullanıcı oluşturma

Bu sonraki adım için [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) gerekir.

1. SSMS 'yi başlatın.

2. **Sunucuya Bağlan** iletişim kutusunda, **sunucu adı** alanına Azure SQL veritabanı sunucunuzun adını girin.

3. **Kimlik doğrulama** alanında **MFA desteğiyle Active Directory-Universal** ' i seçin (diğer iki Active Directory kimlik doğrulama türünü de KULLANABILIRSINIZ, bkz. [SQL Ile Azure AD kimlik doğrulamasını yapılandırma ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure)).

4. **Kullanıcı adı** alanına, sunucu yöneticisi olarak AYARLADıĞıNıZ Azure AD hesabının adını girin, örneğin testuser@xxxonline.com.

5. **Bağlan** ' ı seçin ve oturum açma işlemini doldurun.

6. **Nesne Gezgini**, **veritabanları** -> **sistem veritabanları** klasörünü genişletin.

7. **Ana** veritabanına sağ tıklayın ve **Yeni sorgu**' yı seçin.

8. Sorgu penceresinde, aşağıdaki T-SQL komutunu girin ve araç çubuğunda **Yürüt** ' ü seçin.

   ```sql
   CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
   ```

   Komutun, grubu temsil eden bir kapsanan Kullanıcı oluşturması başarıyla tamamlanır.

9. Sorgu penceresini temizleyin, aşağıdaki T-SQL komutunu girin ve araç çubuğunda **Yürüt** ' ü seçin.

   ```sql
   ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
   ```

   Komutun, içerilen kullanıcıya bir veritabanı (SSSıSDB) oluşturma özelliği verilerek başarıyla tamamlanmalıdır.

10. SSSıSDB 'niz SQL kimlik doğrulaması kullanılarak oluşturulduysa ve Azure-SSIS IR 'niz için Azure AD kimlik doğrulamasını kullanmak üzere kullanmak istiyorsanız, **SSISDB** veritabanına sağ tıklayıp **Yeni sorgu**' yı seçin.

11. Sorgu penceresinde, aşağıdaki T-SQL komutunu girin ve araç çubuğunda **Yürüt** ' ü seçin.

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    Komutun, grubu temsil eden bir kapsanan Kullanıcı oluşturması başarıyla tamamlanır.

12. Sorgu penceresini temizleyin, aşağıdaki T-SQL komutunu girin ve araç çubuğunda **Yürüt** ' ü seçin.

    ```sql
    ALTER ROLE db_owner ADD MEMBER [SSISIrGroup]
    ```

    Komutun, bulunan kullanıcıya SSSıSDB 'ye erişme özelliği verilerek başarıyla tamamlanmalıdır.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği üzerinde Azure AD 'yi etkinleştirme

Azure SQL veritabanı yönetilen örneği, ADF 'niz için yönetilen kimliğe sahip bir veritabanı oluşturmayı destekler. ADF 'niz için yönetilen kimliğe bir Azure AD grubuna katılmanız veya yönetilen örnekte bu grubu temsil eden kapsanan bir kullanıcı oluşturmanız gerekir.

### <a name="configure-azure-ad-authentication-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için Azure AD kimlik doğrulamasını yapılandırma

[Yönetilen örneğiniz için Azure Active Directory Yöneticisi sağlama](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-managed-instance)bölümündeki adımları izleyin.

### <a name="add-the-managed-identity-for-your-adf-as-a-user-in-azure-sql-database-managed-instance"></a>ADF 'niz için yönetilen kimliği Azure SQL veritabanı yönetilen örneği 'nde Kullanıcı olarak ekleyin

Bu sonraki adım için [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) gerekir.

1.  SSMS 'yi başlatın.

2.  **Sysadmin**olan SQL Server bir hesabı kullanarak yönetilen örneğe bağlanın. Bu, Azure SQL veritabanı yönetilen örneği için Azure AD Server sorumluları (oturum açmalar) GA olduktan sonra kaldırılacak geçici bir kısıtlamadır. Oturum açmayı oluşturmak için bir Azure AD yönetici hesabı kullanmayı denerseniz aşağıdaki hatayı görürsünüz: Msg 15247, düzey 16, durum 1, satır 1 Kullanıcı bu eylemi gerçekleştirmek için izne sahip değil.

3.  **Nesne Gezgini**, **veritabanları** -> **sistem veritabanları** klasörünü genişletin.

4.  **Ana** veritabanına sağ tıklayın ve **Yeni sorgu**' yı seçin.

5.  Sorgu penceresinde, ADF 'nizin yönetilen kimliğini Kullanıcı olarak eklemek için aşağıdaki T-SQL betiğini yürütün

    ```sql
    CREATE LOGIN [{your ADF name}] FROM EXTERNAL PROVIDER
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{your ADF name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{your ADF name}]
    ```
    
    Komut, ADF 'niz için yönetilen kimliğe (SSSıSDB) bir veritabanı oluşturma özelliği verilerek başarıyla tamamlanır.

6.  SSSıSDB 'niz SQL kimlik doğrulaması kullanılarak oluşturulduysa ve Azure-SSIS IR 'niz için Azure AD kimlik doğrulamasını kullanmak üzere kullanmak istiyorsanız, **SSISDB** veritabanına sağ tıklayıp **Yeni sorgu**' yı seçin.

7.  Sorgu penceresinde, aşağıdaki T-SQL komutunu girin ve araç çubuğunda **Yürüt** ' ü seçin.

    ```sql
    CREATE USER [{your ADF name}] FOR LOGIN [{your ADF name}] WITH DEFAULT_SCHEMA = dbo
    ALTER ROLE db_owner ADD MEMBER [{your ADF name}]
    ```

    Komutun, ADF 'niz için SSıSDB 'ye erişme özelliği için yönetilen kimliğe izin veren başarıyla tamamlanmalıdır.

## <a name="provision-azure-ssis-ir-in-azure-portaladf-app"></a>Azure portal/ADF uygulaması 'nda Azure-SSIS IR sağlama

Azure-SSIS IR 'yi Azure portal/ADF uygulamasında sağladığınızda, **SQL ayarları** SAYFASıNDA, **ADF 'niz IÇIN yönetilen kimlikle AAD kimlik doğrulaması kullan** seçeneğini belirleyin. Aşağıdaki ekran görüntüsünde, SSıSDB barındıran Azure SQL veritabanı sunucusu ile IR ayarları gösterilmektedir. SSSıSDB barındıran yönetilen örnek ile IR için, **Katalog veritabanı hizmet katmanı** ve **Azure hizmetlerinin erişime izin ver** ayarları geçerli değildir.

Azure-SSIS IR oluşturma hakkında daha fazla bilgi için, bkz. [Azure Data Factory Azure-SSIS tümleştirme çalışma zamanı oluşturma](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Azure-SSIS tümleştirme çalışma zamanı ayarları](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-azure-ssis-ir-with-powershell"></a>PowerShell ile Azure-SSIS IR sağlama

Azure-SSIS IR 'nizi PowerShell ile sağlamak için aşağıdaki işlemleri yapın:

1.  [](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018)AzurePowerShell modülünü yükler.

2.  Betiğinizdeki parametre ayarlamayın `CatalogAdminCredential` . Örneğin:

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
