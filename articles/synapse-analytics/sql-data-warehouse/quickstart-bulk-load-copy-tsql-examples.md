---
title: COPY ifadesiyle kimlik doğrulama mekanizmaları
description: Verileri toplu olarak yüklemek için kimlik doğrulama mekanizmalarını özetler
services: synapse-analytics
author: kevinvngo
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql-dw
ms.date: 07/10/2020
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 6f54a8993b602110e35c410338b6f0a51109738f
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88603905"
---
# <a name="securely-load-data-using-synapse-sql"></a>SYNAPSE SQL kullanarak güvenli bir şekilde veri yükleme

Bu makalede, [Copy ifadesiyle](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest)ilgili güvenli kimlik doğrulama mekanizmalarına örnekler verilmektedir. COPY yöntemi, SYNAPSE SQL 'de verileri toplu yüklemenin en esnek ve güvenli yoludur.
## <a name="supported-authentication-mechanisms"></a>Desteklenen kimlik doğrulama mekanizmaları

Aşağıdaki matris, her dosya türü ve depolama hesabı için desteklenen kimlik doğrulama yöntemlerini açıklar. Bu, kaynak depolama konumu ve hata dosyası konumu için geçerlidir.

|                          |                CSV                |              Parquet               |                ORC                 |
| :----------------------: | :-------------------------------: | :-------------------------------:  | :-------------------------------:  |
|  **Azure Blob depolama**  | SAS/MSı/HIZMET SORUMLUSU/ANAHTARı/AAD |              SAS/ANAHTAR               |              SAS/ANAHTAR               |
| **Azure Data Lake Gen2** | SAS/MSı/HIZMET SORUMLUSU/ANAHTARı/AAD | SAS (blob uç noktası)/MSI (DFS uç noktası)/SERVICE PRINCIPAL/KEY/AAD | SAS (blob uç noktası)/MSI (DFS uç noktası)/SERVICE PRINCIPAL/KEY/AAD |


## <a name="a-storage-account-key-with-lf-as-the-row-terminator-unix-style-new-line"></a>A. Satır Sonlandırıcı olarak LF ile depolama hesabı anahtarı (UNIX stili yeni satır)


```sql
--Note when specifying the column list, input field numbers start from 1
COPY INTO target_table (Col_one default 'myStringDefault' 1, Col_two default 1 3)
FROM 'https://adlsgen2account.dfs.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV'
    ,CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='<Your_Account_Key>')
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Storage Account Key', SECRET='x6RWv4It5F2msnjelv3H4DA80n0QW0daPdw43jM0nyetx4c6CpDkdj3986DX5AHFMIf/YN4y6kkCnU8lb+Wx0Pj+6MDw=='),
    ,ROWTERMINATOR='0x0A' --0x0A specifies to use the Line Feed character (Unix based systems)
)
```
> [!IMPORTANT]
>
> - Satır besleme/yeni satır karakterini belirtmek için onaltılık değeri (0x0A) kullanın. Note, COPY ifadesinin ' \n ' dizesini ' \r\n ' (satır başı yeni satır) olarak yorumlayacak.

## <a name="b-shared-access-signatures-sas-with-crlf-as-the-row-terminator-windows-style-new-line"></a>B. Satır Sonlandırıcı olarak CRLF ile paylaşılan erişim Imzaları (SAS) (Windows stili yeni satır)
```sql
COPY INTO target_table
FROM 'https://adlsgen2account.dfs.core.windows.net/myblobcontainer/folder1/'
WITH (
    FILE_TYPE = 'CSV'
    ,CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<Your_SAS_Token>')
    --CREDENTIAL should look something like this:
    --CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='?sv=2018-03-28&ss=bfqt&srt=sco&sp=rl&st=2016-10-17T20%3A14%3A55Z&se=2021-10-18T20%3A19%3A00Z&sig=IEoOdmeYnE9%2FKiJDSFSYsz4AkNa%2F%2BTx61FuQ%2FfKHefqoBE%3D'),
    ,ROWTERMINATOR='\n'-- COPY command automatically prefixes the \r character when \n (newline) is specified. This results in carriage return newline (\r\n) for Windows based systems.
)
```

> [!IMPORTANT]
>
> - ' \R\r\n ' olarak yorumlanabilecek ve sorunların ayrıştırmasına neden olabilecek ROWSONLANDıRıCı ' \r\n ' olarak belirtmeyin

## <a name="c-managed-identity"></a>C. Yönetilen Kimlik

Depolama Hesabınız VNet 'e eklendiğinde yönetilen kimlik kimlik doğrulaması gerekir. 

### <a name="prerequisites"></a>Önkoşullar

1. Bu [kılavuzu](/powershell/azure/install-az-ps?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) kullanarak Azure PowerShell'i yükleyin.
2. Genel amaçlı v1 veya blob depolama hesabınız varsa öncelikle bu [kılavuzda](../../storage/common/storage-account-upgrade.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) yer alan adımları izleyerek genel amaçlı v2 sürümüne yükseltmeniz gerekir.
3. Azure depolama hesabı **güvenlik duvarları ve sanal ağlar** ayarları menüsünde **Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin vermeniz** gerekir. Daha fazla bilgi için bu [kılavuza](../../storage/common/storage-network-security.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#exceptions) bakın.
#### <a name="steps"></a>Adımlar

1. PowerShell 'de **SQL Server** 'ı Azure ACTIVE DIRECTORY (AAD) ile kaydedin:

   ```powershell
   Connect-AzAccount
   Select-AzSubscription -SubscriptionId your-subscriptionId
   Set-AzSqlServer -ResourceGroupName your-database-server-resourceGroup -ServerName your-database-servername -AssignIdentity
   ```

2. Bu [Kılavuzu](../../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)kullanarak **genel amaçlı v2 depolama hesabı** oluşturun.

   > [!NOTE]
   > Genel amaçlı bir v1 veya blob depolama hesabınız varsa, önce bu [Kılavuzu](../../storage/common/storage-account-upgrade.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)kullanarak **v2 'ye yükseltmeniz** gerekir.

3. Depolama hesabınız altında **Access Control (IAM)** bölümüne gidin ve **rol ataması Ekle**' yi seçin. SQL Server 'a **Depolama Blobu veri sahibi, katkıda bulunan veya okuyucu** Azure rolü atayın.

   > [!NOTE]
   > Yalnızca sahibi ayrıcalığına sahip Üyeler bu adımı gerçekleştirebilir. Çeşitli Azure yerleşik rolleri için bu [kılavuza](../../role-based-access-control/built-in-roles.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)bakın.
   
    > [!IMPORTANT]
    > **Depolama** **blobu veri** sahibini, katkıda bulunan veya okuyucu Azure rolünü belirtin. Bu roller, sahip, katkıda bulunan ve okuyucunuzun Azure yerleşik rollerinin farklıdır. 

    ![Yük için RBAC izni veriliyor](./media/quickstart-bulk-load-copy-tsql-examples/rbac-load-permissions.png)

4. Şimdi "yönetilen kimlik" belirten kopyalama ifadesini çalıştırabilirsiniz:

    ```sql
    COPY INTO dbo.target_table
    FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.txt'
    WITH (
        FILE_TYPE = 'CSV',
        CREDENTIAL = (IDENTITY = 'Managed Identity'),
    )
    ```

## <a name="d-azure-active-directory-authentication-aad"></a>D. Azure Active Directory kimlik doğrulaması (AAD)
#### <a name="steps"></a>Adımlar

1. Depolama hesabınız altında **Access Control (IAM)** bölümüne gidin ve **rol ataması Ekle**' yi seçin. AAD kullanıcısına **Depolama Blobu veri sahibi, katkıda bulunan veya okuyucu** Azure rolü atayın. 

    > [!IMPORTANT]
    > **Depolama** **blobu veri** sahibini, katkıda bulunan veya okuyucu Azure rolünü belirtin. Bu roller, sahip, katkıda bulunan ve okuyucunuzun Azure yerleşik rollerinin farklıdır.

    ![Yük için RBAC izni veriliyor](./media/quickstart-bulk-load-copy-tsql-examples/rbac-load-permissions.png)

2. Aşağıdaki [belgelere](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure?tabs=azure-powershell#create-an-azure-ad-administrator-for-azure-sql-server)gıderek Azure AD kimlik doğrulamasını yapılandırın. 

3. Artık herhangi bir kimlik bilgisi belirtmeden COPY ifadesini çalıştırabileceğiniz Active Directory kullanarak SQL havuzunuza bağlanın:

    ```sql
    COPY INTO dbo.target_table
    FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder1/*.txt'
    WITH (
        FILE_TYPE = 'CSV'
    )
    ```


## <a name="e-service-principal-authentication"></a>E. Hizmet Sorumlusu Kimlik Doğrulaması
#### <a name="steps"></a>Adımlar

1. [Azure Active Directory (AAD) uygulaması oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application)
2. [Uygulama KIMLIĞINI al](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)
3. [Kimlik doğrulama anahtarını al](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret)
4. [V1 OAuth 2,0 belirteç uç noktasını alın](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#step-4-get-the-oauth-20-token-endpoint-only-for-java-based-applications)
5. Depolama hesabınızdaki [AAD uygulamanıza okuma, yazma ve yürütme Izinleri atama](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-service-to-service-authenticate-using-active-directory?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#step-3-assign-the-azure-ad-application-to-the-azure-data-lake-storage-gen1-account-file-or-folder)
6. Şimdi COPY ifadesini çalıştırabilirsiniz:

    ```sql
    COPY INTO dbo.target_table
    FROM 'https://myaccount.blob.core.windows.net/myblobcontainer/folder0/*.txt'
    WITH (
        FILE_TYPE = 'CSV'
        ,CREDENTIAL=(IDENTITY= '<application_ID>@<OAuth_2.0_Token_EndPoint>' , SECRET= '<authentication_key>')
        --CREDENTIAL should look something like this:
        --,CREDENTIAL=(IDENTITY= '92761aac-12a9-4ec3-89b8-7149aef4c35b@https://login.microsoftonline.com/72f714bf-86f1-41af-91ab-2d7cd011db47/oauth2/token', SECRET='juXi12sZ6gse]woKQNgqwSywYv]7A.M')
    )
    ```

> [!IMPORTANT]
>
> - OAuth 2,0 belirteci uç noktasının **v1** sürümünü kullanın

## <a name="next-steps"></a>Sonraki adımlar

- Ayrıntılı sözdizimi için [kopyalama ekstresi makale](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest#syntax) makalesini denetleyin
- En iyi yöntemleri yüklemek için [veri yüklemeye genel bakış](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/design-elt-data-loading#what-is-elt) makalesini denetleyin
