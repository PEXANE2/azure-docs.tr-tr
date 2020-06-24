---
title: 'Hızlı başlangıç: iş yükü sınıflandırıcı Oluşturma-Portal'
description: Yüksek öneme sahip bir iş yükü Sınıflandırıcısı oluşturmak için Azure portal kullanın.
services: synapse-analytics
author: ronortloff
manager: craigg
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql-dw
ms.date: 05/04/2020
ms.author: rortloff
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 691cdcb525f8e9e3d1fb914372b9f62366f4bfba
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85213032"
---
# <a name="quickstart-create-a-synapse-sql-pool-workload-classifier-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak SYNAPSE SQL havuzu iş yükü Sınıflandırıcısı oluşturma

Bu hızlı başlangıçta, bir iş yükü grubuna sorgu atamak için bir [iş yükü Sınıflandırıcısı](sql-data-warehouse-workload-classification.md) oluşturacaksınız.  Sınıflandırıcı, `ELTLogin` SQL kullanıcısının `DataLoads` iş yükü grubuna istek atayacaktır.   [Hızlı başlangıç: iş yükü yalıtımını yapılandırma](quickstart-configure-workload-isolation-portal.md) öğreticisini izleyin `DataLoads` .  Bu öğreticide, istekleri doğru şekilde sınıflandırmasına yardımcı olmak için WLM_LABEL seçeneği ile bir iş yükü Sınıflandırıcısı oluşturulur.  Sınıflandırıcı, `HIGH` Bu isteklere de [iş yükü önem derecesini](sql-data-warehouse-workload-importance.md) atayacaktır.


Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.


## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın.

> [!NOTE]
> Azure SYNAPSE Analytics 'te bir SQL havuzu örneği oluşturmak, yeni bir faturalanabilir hizmetle sonuçlanabilir.  Daha fazla bilgi için bkz. [Azure SYNAPSE Analytics fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç, SYNAPSE SQL 'de zaten bir SQL havuzu örneğiniz olduğunu ve DENETIM VERITABANı izinlerine sahip olduğunuzu varsayar. Gerekiyorsa **mySampleDataWarehouse** adlı bir veri ambarı oluşturmak için [Oluşturma ve Bağlanma - portal](create-data-warehouse-portal.md) bölümünü kullanabilirsiniz.
<br><br>
Bir iş yükü grubu `DataLoads` var.  İş yükü grubunu oluşturmak için [hızlı başlangıç: iş yükü yalıtımı yapılandırma](quickstart-configure-workload-isolation-portal.md) öğreticisini inceleyin.
<br><br>
>[!IMPORTANT] 
>İş yükü yönetimini yapılandırmak için SQL havuzunuzun çevrimiçi olması gerekir. 


## <a name="create-a-login-for-eltlogin"></a>ELTLogin için oturum açma oluşturma

`master`IÇIN [oturum açma oluştur](/sql/t-sql/statements/create-login-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) kullanarak veritabanında SQL Server kimlik doğrulaması oturumu oluşturun `ELTLogin` .

```sql
IF NOT EXISTS (SELECT * FROM sys.sql_logins WHERE name = 'ELTLogin')
BEGIN
CREATE LOGIN [ELTLogin] WITH PASSWORD='<strongpassword>'
END
;
```

## <a name="create-user-and-grant-permissions"></a>Kullanıcı oluşturma ve izin verme

Oturum açma oluşturulduktan sonra, veritabanında bir kullanıcının oluşturulması gerekir.  Mysampledatawarehouse içinde SQL kullanıcısını oluşturmak için [Kullanıcı oluşturma](/sql/t-sql/statements/create-user-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) ' yı kullanın `ELTRole` . **mySampleDataWarehouse**  Bu öğretici sırasında sınıflandırmayı test edeceğiz, `ELTLogin` **Mysampledatawarehouse**için izin verin. 

```sql
IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = 'ELTLogin')
BEGIN
CREATE USER [ELTLogin] FOR LOGIN [ELTLogin]
GRANT CONTROL ON DATABASE::mySampleDataWarehouse TO ELTLogin 
END
;
```

## <a name="configure-workload-classification"></a>İş yükü sınıflandırmasını yapılandırma
Sınıflandırma, istekleri bir dizi kurala göre iş yükü grubuna yönlendirmenize olanak tanır.  [Hızlı başlangıç: iş yükü yalıtımı yapılandırma](quickstart-configure-workload-isolation-portal.md) öğreticisini `DataLoads` iş yükü grubunu oluşturduk.  Artık sorguları iş yükü grubuna yönlendirmek için bir iş yükü Sınıflandırıcısı oluşturacaksınız `DataLoads` .


1.  Azure portal sol sayfasında **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** seçeneğine tıklayın.
2.  **Azure SYNAPSE Analytics (eski ADıYLA SQL DW)** sayfasından **mysampledatawarehouse** öğesini seçin. SQL Havuzu açılır.
3.  **Iş yükü yönetimi**' ne tıklayın.

    ![Tıklama menüsü](./media/quickstart-create-a-workload-classifier-portal/menu.png)

4.  İş yükü grubunun sağ tarafındaki **ayarlar & sınıflandırıcılar** ' ne tıklayın `DataLoads` .

    ![Oluştur’a tıklayın](./media/quickstart-create-a-workload-classifier-portal/settings-classifiers.png)

5. **Sınıflandırıcılar**' ne tıklayın.
6. **Sınıflandırıcı Ekle**' ye tıklayın.

    ![Ekle'ye tıklayın.](./media/quickstart-create-a-workload-classifier-portal/add-wc.png)

7.  `ELTLoginDataLoads` **Ad**için girin.
8.  `ELTLogin` **Üye**için girin.
9.  `High` **İstek önemi**için seçin.  *Isteğe bağlı*, normal önem varsayılandır.
10. `fact_loads` **Etiket**için girin.
11. **Ekle**'ye tıklayın.
12. **Kaydet**’e tıklayın.

    ![Yapılandırma ' ya tıklayın](./media/quickstart-create-a-workload-classifier-portal/config-wc.png)

## <a name="verify-and-test-classification"></a>Doğrulama ve test sınıflandırması
Sınıflandırıcının varlığını doğrulamak için [sys. workload_management_workload_classifiers](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifiers-transact-sql?view=azure-sqldw-latest) katalog görünümünü denetleyin `ELTLoginDataLoads` .

```sql
SELECT * FROM sys.workload_management_workload_classifiers WHERE name = 'ELTLoginDataLoads'
```

Sınıflandırıcı ayrıntılarını doğrulamak için [sys. workload_management_workload_classifier_details](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest) katalog görünümünü denetleyin.

```sql
SELECT c.[name], c.group_name, c.importance, cd.classifier_type, cd.classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ELTLoginDataLoads'
```

Sınıflandırmayı test etmek için aşağıdaki deyimlerini çalıştırın.  Olarak bağlı olduğunuzdan ``ELTLogin`` ve ``Label`` sorgusunda kullanıldığından emin olun.
```sql
CREATE TABLE factstaging (ColA int)
INSERT INTO factstaging VALUES(0)
INSERT INTO factstaging VALUES(1)
INSERT INTO factstaging VALUES(2)
GO

CREATE TABLE testclassifierfact WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT * FROM factstaging
OPTION (LABEL='fact_loads')
```

İş yükü `CREATE TABLE` `DataLoads` sınıflandırıcısını kullanarak iş yükü grubuna sınıflandırılan ifadeyi doğrulayın `ELTLoginDataLoads` .
```sql 
SELECT TOP 1 request_id, classifier_name, group_name, resource_allocation_percentage, submit_time, [status], [label], command 
FROM sys.dm_pdw_exec_requests 
WHERE [label] = 'fact_loads'
ORDER BY submit_time DESC
```



## <a name="clean-up-resources"></a>Kaynakları temizleme

`ELTLoginDataLoads`Bu öğreticide oluşturulan iş yükü sınıflandırıcısını silmek için:

1. İş yükü grubunun sağ tarafında bulunan **1 sınıflandırıcıya** tıklayın `DataLoads` .

    ![Sil 'e tıklayın](./media/quickstart-create-a-workload-classifier-portal/delete-wc.png)

2. **Sınıflandırıcılar**' ne tıklayın.
3. **`...`** `ELTLoginDataLoads` İş yükü sınıflandırıcıya sağ tarafındaki öğesine tıklayın.
4. **Sil**' e tıklayın.
5. **Kaydet**'e tıklayın.

    ![Kaydet’e tıklayın.](./media/quickstart-create-a-workload-classifier-portal/delete-save-wc.png)

Veri ambarı birimleri ve veri Ambarınızda depolanan veriler için ücret ödersiniz. Bu işlem ve depolama alanı kaynakları ayrı ayrı faturalandırılır.

- Verileri depoda tutmak istiyorsanız, veri ambarını kullanmadığınız zamanlarda işlemi duraklatabilirsiniz. İşlem duraklatıldığında yalnızca veri depolama alanı için ücret ödersiniz. Verilerle çalışmaya hazırsanız, işlem işlemini sürdürmeniz gerekir.
- Gelecekteki ücretlendirmeleri kaldırmak istiyorsanız, veri ambarını silebilirsiniz.

Kaynakları temizlemek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com)oturum açın, veri Ambarınızda öğesini seçin.

    ![Kaynakları temizleme](./media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

2. İşlem duraklatmak için **Duraklat** düğmesini seçin. Veri ambarı duraklatıldığında, bir **Başlat** düğmesi görürsünüz.  İşlem işlemini sürdürmesini sağlamak için **Başlat**' ı seçin.

3. İşlem veya depolama için ücretlendirilmemek üzere veri ambarını kaldırmak için **Sil**' i seçin.

4. Oluşturduğunuz SQL Server 'ı kaldırmak için önceki görüntüde **sqlpoolservername.Database.Windows.net** ' ı seçin ve **Sil**' i seçin.  Sunucuyu silmek sunucuyla ilişkili tüm veritabanlarını da sileceğinden bu silme işlemini gerçekleştirirken dikkatli olun.

5. Kaynak grubunu kaldırmak için **Myresourcegroup**' ı seçin ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Azure portal izleme ölçümlerini kullanarak iş yükünüzü izleyin.  Ayrıntılar için bkz. [Iş yükü yönetimini yönetme ve izleme](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md) .
