---
author: rothja
ms.service: virtual-machines-sql
ms.topic: include
ms.date: 10/26/2018
ms.author: jroth
ms.openlocfilehash: 22f16a7382cb0fe1f3fe2a6ef5e7c00a6989623c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188321"
---
## <a name="next-steps"></a>Sonraki adımlar

Azure Key Vault Tümleştirmesini etkinleştirdikten sonra, SQL VM'nizde SQL Server şifrelemesini etkinleştirebilirsiniz. İlk olarak, anahtar kasanızın içinde asimetrik bir anahtar ve VM'nizde SQL Server içinde simetrik bir anahtar oluşturmanız gerekir. Ardından, veritabanlarınız ve yedeklemeleriniz için şifreleme sağlamak için T-SQL deyimlerini yürütebilirsiniz.

Yararlanabileceğiniz çeşitli şifreleme biçimleri vardır:

* [Saydam Veri Şifrelemesi (TDE)](https://msdn.microsoft.com/library/bb934049.aspx)
* [Şifreli yedeklemeler](https://msdn.microsoft.com/library/dn449489.aspx)
* [Sütun Düzeyi Şifrelemesi (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)

Aşağıdaki Transact-SQL komut dosyaları bu alanların her biri için örnekler sağlar.

### <a name="prerequisites-for-examples"></a>Örnekler için ön koşullar

Her örnek iki ön koşula dayanır: **CONTOSO_KEY** adlı anahtar kasanızdan asimetrik bir anahtar ve **Azure_EKM_TDE_cred**adlı AKV Tümleştirme özelliği tarafından oluşturulan bir kimlik bilgisi. Aşağıdaki Transact-SQL komutları, örnekleri çalıştırmak için bu ön koşulları düzenesin.

``` sql
USE master;
GO

--create credential
--The <<SECRET>> here requires the <Application ID> (without hyphens) and <Secret> to be passed together without a space between them.
CREATE CREDENTIAL sysadmin_ekm_cred
    WITH IDENTITY = 'keytestvault', --keyvault
    SECRET = '<<SECRET>>'
FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov;


--Map the credential to a SQL login that has sysadmin permissions. This allows the SQL login to access the key vault when creating the asymmetric key in the next step.
ALTER LOGIN [SQL_Login]
ADD CREDENTIAL sysadmin_ekm_cred;


CREATE ASYMMETRIC KEY CONTOSO_KEY
FROM PROVIDER [AzureKeyVault_EKM_Prov]
WITH PROVIDER_KEY_NAME = 'KeyName_in_KeyVault',  --The key name here requires the key we created in the key vault
CREATION_DISPOSITION = OPEN_EXISTING;
```

### <a name="transparent-data-encryption-tde"></a>Saydam Veri Şifrelemesi (TDE)

1. TDE veritabanı altyapısı tarafından kullanılmak üzere bir SQL Server girişi oluşturun ve ardından kimlik bilgilerini ekleyin.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it loads a database
   -- encrypted by TDE.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the TDE Login to add the credential for use by the
   -- Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred;
   GO
   ```

1. TDE için kullanılacak veritabanı şifreleme anahtarını oluşturun.

   ``` sql
   USE ContosoDatabase;
   GO

   CREATE DATABASE ENCRYPTION KEY 
   WITH ALGORITHM = AES_128 
   ENCRYPTION BY SERVER ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the database to enable transparent data encryption.
   ALTER DATABASE ContosoDatabase
   SET ENCRYPTION ON;
   GO
   ```

### <a name="encrypted-backups"></a>Şifreli yedeklemeler

1. Yedeklemeleri şifrelemek için Veritabanı Altyapısı tarafından kullanılmak üzere bir SQL Server girişi oluşturun ve kimlik bilgilerini ekleyin.

   ``` sql
   USE master;
   -- Create a SQL Server login associated with the asymmetric key
   -- for the Database engine to use when it is encrypting the backup.
   CREATE LOGIN EKM_Login
   FROM ASYMMETRIC KEY CONTOSO_KEY;
   GO

   -- Alter the Encrypted Backup Login to add the credential for use by
   -- the Database Engine to access the key vault
   ALTER LOGIN EKM_Login
   ADD CREDENTIAL Azure_EKM_cred ;
   GO
   ```

1. Anahtar kasasında depolanan asimetrik anahtarla şifreleme yi belirten veritabanını yedekleyin.

   ``` sql
   USE master;
   BACKUP DATABASE [DATABASE_TO_BACKUP]
   TO DISK = N'[PATH TO BACKUP FILE]'
   WITH FORMAT, INIT, SKIP, NOREWIND, NOUNLOAD,
   ENCRYPTION(ALGORITHM = AES_256, SERVER ASYMMETRIC KEY = [CONTOSO_KEY]);
   GO
   ```

### <a name="column-level-encryption-cle"></a>Sütun Düzeyi Şifrelemesi (CLE)

Bu komut dosyası, anahtar kasasındaki asimetrik anahtar tarafından korunan simetrik bir anahtar oluşturur ve veritabanındaki verileri şifrelemek için simetrik anahtarı kullanır.

``` sql
CREATE SYMMETRIC KEY DATA_ENCRYPTION_KEY
WITH ALGORITHM=AES_256
ENCRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

DECLARE @DATA VARBINARY(MAX);

--Open the symmetric key for use in this session
OPEN SYMMETRIC KEY DATA_ENCRYPTION_KEY
DECRYPTION BY ASYMMETRIC KEY CONTOSO_KEY;

--Encrypt syntax
SELECT @DATA = ENCRYPTBYKEY(KEY_GUID('DATA_ENCRYPTION_KEY'), CONVERT(VARBINARY,'Plain text data to encrypt'));

-- Decrypt syntax
SELECT CONVERT(VARCHAR, DECRYPTBYKEY(@DATA));

--Close the symmetric key
CLOSE SYMMETRIC KEY DATA_ENCRYPTION_KEY;
```

## <a name="additional-resources"></a>Ek kaynaklar

Bu şifreleme özelliklerinin nasıl kullanılacağı hakkında daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/dn198405.aspx#UsesOfEKM)

Bu makaledeki adımların, Azure sanal makinesinde zaten SQL Server'ın çalıştığını varsaydığını unutmayın. Değilse, [Azure'da sql server sanal makine sağlama](../articles/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision.md)konusuna bakın. Azure VM'lerde SQL Server'ı çalıştırmayla ilgili diğer kılavuzlar için [Azure Sanal Makinelere genel bakışta SQL Server'a](../articles/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview.md)bakın.
