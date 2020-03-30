---
title: 'Quickstart: İş yükü sınıflandırıcısı oluşturma - T-SQL'
description: Yüksek öneme sahip bir iş yükü sınıflandırıcıoluşturmak için T-SQL'i kullanın.
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
ms.openlocfilehash: 9d41aa3447caebfeeffc05c36b5073f57c3e16cd
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350815"
---
# <a name="quickstart-create-a-workload-classifier-using-t-sql"></a>Quickstart: T-SQL kullanarak iş yükü sınıflandırıcısı oluşturma

Bu hızlı başlangıçta, kuruluşunuzun CEO'su için büyük önem taşıyan bir iş yükü sınıflandırıcısı hızla oluşturursunuz. Bu iş yükü sınıflandırıcısı, CEO sorgularının kuyrukta daha az öneme sahip diğer sorgulardan önce olmasını sağlar.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Azure Synapse Analytics'te BIR SQL Analytics örneği oluşturmak, faturalandırılabilir yeni bir hizmetle sonuçlanabilir.  Daha fazla bilgi için Azure [Synapse Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)bölümüne bakın.
>
>

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlatma, zaten bir SQL Veri Ambarı'nız olduğunu ve CONTROL DATABASE izinlerine sahip olduğunuzu varsayar. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

## <a name="create-login-for-theceo"></a>TheCEO için giriş oluşturma

'TheCEO' için CREATE `master` [LOGIN'i](/sql/t-sql/statements/create-login-transact-sql) kullanarak veritabanında bir SQL Server kimlik doğrulama girişi oluşturun.

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'TheCEO')
BEGIN
CREATE LOGIN [TheCEO] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user"></a>Kullanıcı oluştur

mySampleDataWarehouse'da "TheCEO" [kullanıcısını oluşturun](/sql/t-sql/statements/create-user-transact-sql?view=azure-sqldw-latest)

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'THECEO')
BEGIN
CREATE USER [TheCEO] FOR LOGIN [TheCEO]
END
;
```

## <a name="create-a-workload-classifier"></a>İş yükü sınıflandırıcısı oluşturma

"TheCEO" için büyük öneme sahip bir [iş yükü sınıflandırıcısı](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) oluşturun.

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO];
CREATE WORKLOAD CLASSIFIER [wgcTheCEO]
WITH (WORKLOAD_GROUP = 'xlargerc'
      ,MEMBERNAME = 'TheCEO'
      ,IMPORTANCE = HIGH);
```

## <a name="view-existing-classifiers"></a>Varolan sınıflandırıcıları görüntüleme

```sql
SELECT * FROM sys.workload_management_workload_classifiers
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

```sql
DROP WORKLOAD CLASSIFIER [wgcTheCEO]
DROP USER [TheCEO]
;
```

Veri ambarı birimleri ve veri ambarınızda depolanan veriler için ücretlendirilirsiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depoda tutmak istiyorsanız, veri ambarını kullanmadığınız zamanlarda işlemi duraklatabilirsiniz. İşlemi duraklatarak, yalnızca veri depolama için ücretlendirilirsiniz. Verilerle çalışmaya hazır olduğunuzda, bilgi işlem yapmaya devam edin.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları temizlemek için aşağıdaki adımları izleyin.

1. [Azure portalında](https://portal.azure.com)oturum açın , veri ambarınızda seçin.

    ![Kaynakları temizleme](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlemi duraklatmak için **Duraklat** düğmesini seçin. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşleme devam etmek için **Başlat'ı**seçin.

3. Bilgi işlem veya depolama için ücretlendirilmemek için veri ambarını kaldırmak için **Sil'i**seçin.

4. Oluşturduğunuz SQL sunucusunu kaldırmak için önceki resimde **mynewserver-20180430.database.windows.net'yi** seçin ve sonra **Sil'i**seçin.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için **myResourceGroup'u**seçin ve ardından **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Şimdi bir iş yükü sınıflandırıcı oluşturduk. Nasıl performans gösterdiğini görmek için CEO olarak birkaç sorgu çalıştırın. Sorguları ve atanan önemi görüntülemek için [sys.dm_pdw_exec_requests'a](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) bakın.
- SQL Analytics iş yükü yönetimi hakkında daha fazla bilgi için iş [yükü önemi](sql-data-warehouse-workload-importance.md) ve Iş [Yükü Sınıflandırması'na](sql-data-warehouse-workload-classification.md)bakın.
- [İş Yükü Önemini Yapılandırmak](sql-data-warehouse-how-to-configure-workload-importance.md) ve İş Yükü Yönetimini Nasıl [Yönetecek ve İzleyecek](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)makalelere bakın.
