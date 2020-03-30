---
title: 'Quickstart: İş yükü yalıtımı yapılandırma - T-SQL'
description: İş yükü yalıtımı yapılandırmak için T-SQL'i kullanın.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 02/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 555d437fb0ee898473b37febb1774924b55bfa1d
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350840"
---
# <a name="quickstart-configure-workload-isolation-using-t-sql"></a>Quickstart: T-SQL kullanarak iş yükü yalıtımı yapılandırın

Bu hızlı başlatmada, veri yükleme için kaynak ayıracak bir iş yükü grubu ve sınıflandırıcı hızla oluşturursunuz. İş yükü grubu, sistem kaynaklarının %20'sini veri yüklerine ayırır.  İş yükü sınıflandırıcısı, veri yükleri iş yükü grubuna istekler atayacaktır.  Veri yükleri için %20 izolasyon ile SLA'ları vuracak kaynaklar garanti edilir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Azure Synapse Analytics'te BIR SQL Analytics örneği oluşturmak, faturalandırılabilir yeni bir hizmetle sonuçlanabilir.  Daha fazla bilgi için Azure [Synapse Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)bölümüne bakın.
>
>

## <a name="prerequisites"></a>Ön koşullar
 
Bu hızlı başlatma, Azure Sinaps'ta zaten bir SQL Analytics örneğiniz olduğunu ve CONTROL DATABASE izinlerine sahip olduğunuzu varsayar. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-login-for-dataloads"></a>DataLoads için oturum açma oluşturma

'ELTLogin' için CREATE `master` [LOGIN'i](/sql/t-sql/statements/create-login-transact-sql) kullanarak veritabanında bir SQL Server kimlik doğrulama girişi oluşturun.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'ELTLogin')
BEGIN
CREATE LOGIN [ELTLogin] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Kullanıcı oluştur

mySampleDataWarehouse'da "ELTLogin" [kullanıcısı oluşturun](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest)

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'ELTLogin')
BEGIN
CREATE USER [ELTLogin] FOR LOGIN [ELTLogin]
END
;
```

## <a name="create-a-workload-group"></a>İş yükü grubu oluşturma
DataLoads için %20 yalıtımlı bir [iş yükü grubu](/sql/t-sql/statements/create-workload-group-transact-sql?view=azure-sqldw-latest) oluşturun.
```sql
CREATE WORKLOAD GROUP DataLoads
WITH ( MIN_PERCENTAGE_RESOURCE = 20   
      ,CAP_PERCENTAGE_RESOURCE = 100
      ,REQUEST_MIN_RESOURCE_GRANT_PERCENT = 5) 
;
```

## <a name="create-a-workload-classifier"></a>İş yükü sınıflandırıcısı oluşturma

ELTLogin'i DataLoads iş yükü grubuna eşlemek için bir [iş yükü sınıflandırıcısı](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) oluşturun.

```sql
CREATE WORKLOAD CLASSIFIER [wgcELTLogin]
WITH (WORKLOAD_GROUP = 'DataLoads'
      ,MEMBERNAME = 'ELTLogin')
;
```

## <a name="view-existing-workload-groups-and-classifiers-and-run-time-values"></a>Varolan iş yükü gruplarını ve sınıflandırıcıları ve çalışma zamanı değerlerini görüntüleme

```sql
--Workload groups
SELECT * FROM 
sys.workload_management_workload_groups

--Workload classifiers
SELECT * FROM 
sys.workload_management_workload_classifiers

--Run-time values
SELECT * FROM 
sys.dm_workload_management_workload_groups_stats
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

```sql
DROP WORKLOAD CLASSIFIER [wgcELTLogin]
DROP WORKLOAD GROUP [DataLoads]
DROP USER [ELTLogin]
;
```

Veri ambarı birimleri ve veri ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depolamada tutmak istiyorsanız, SQL havuzunu kullanmadığınızda bilgi işlem'i duraklatabilirsiniz. İşlemi duraklatarak, yalnızca veri depolama için ücretlendirilirsiniz. Verilerle çalışmaya hazır olduğunuzda, bilgi işlem yapmaya devam edin.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları temizlemek için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın , veri ambarınızda seçin.

    ![Kaynakları temizleme](./media/quickstart-configure-workload-isolation-tsql/clean-up-resources.png)

2. İşlemi duraklatmak için **Duraklat** düğmesini seçin. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşleme devam etmek için **Başlat'ı**seçin.

3. Bilgi işlem veya depolama için ücretlendirilmemek için veri ambarını kaldırmak için **Sil'i**seçin.

4. Oluşturduğunuz SQL sunucusunu kaldırmak için önceki resimde **mynewserver-20180430.database.windows.net'yi** seçin ve sonra **Sil'i**seçin.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için **myResourceGroup'u**seçin ve ardından **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Şimdi bir iş yükü grubu oluşturdunuz. Nasıl performans gösterdiğini görmek için ELTLogin olarak birkaç sorgu çalıştırın. Sorguları ve atanan iş yükü grubunu görüntülemek için [sys.dm_pdw_exec_requests'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) bakın.
- SQL Analytics iş yükü yönetimi hakkında daha fazla bilgi için iş [yükü yönetimi](sql-data-warehouse-workload-management.md) ve iş [yükü yalıtımı](sql-data-warehouse-workload-isolation.md)bölümüne bakın.
