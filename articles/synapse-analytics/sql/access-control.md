---
title: Çalışma alanlarına, verilere ve ardışık işlere erişimi yönetme
description: Azure Synapse Analytics çalışma alanında (önizleme) çalışma alanlarına, verilere ve ardışık işlere erişim denetimini nasıl yönettiğinizi öğrenin.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: 89d2105ab080309639c4341072c3f5f36608dfce
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424772"
---
# <a name="manage-access-to-workspaces-data-and-pipelines"></a>Çalışma alanlarına, verilere ve ardışık işlere erişimi yönetme

Azure Synapse Analytics çalışma alanında (önizleme) çalışma alanlarına, verilere ve ardışık işlere erişim denetimini nasıl yönettiğinizi öğrenin.

> [!NOTE]
> GA için RBAC, Synapse'ye özel Azure RBAC rollerinin piyasaya sürülmesiyle daha da geliştirilecektir

## <a name="access-control-for-workspace"></a>Çalışma Alanı için Erişim Denetimi

Bir Azure Synapse çalışma alanına bir üretim dağıtımı için, kullanıcıları ve yöneticileri sağlamayı kolaylaştırmak için ortamınızı düzenlemenizi öneririz.

> [!NOTE]
> Burada alınan yaklaşım, birden çok güvenlik grubu oluşturmak ve daha sonra çalışma alanını bunları tutarlı bir şekilde kullanacak şekilde yapılandırmaktır. Gruplar kurulduktan sonra, bir yöneticinin yalnızca güvenlik grupları içindeki üyeliği yönetmesi gerekir.

### <a name="step-1-set-up-security-groups-with-names-following-this-pattern"></a>Adım 1: Bu deseni izleyen adlarla güvenlik grupları ayarlama

1. Çağrılan güvenlik grubu oluşturma`Synapse_WORKSPACENAME_Users`
2. Çağrılan güvenlik grubu oluşturma`Synapse_WORKSPACENAME_Admins`
3. `ProjectSynapse_WORKSPACENAME_Users` için `Synapse_WORKSPACENAME_Admins` eklendi

### <a name="step-2-prepare-the-default-adls-gen2-account"></a>Adım 2: Varsayılan ADLS Gen2 Hesabını Hazırlama

Çalışma alanınızı sağlarken, çalışma alanının kullanması için bir ADLSGEN2 hesabı ve dosya sistemi için bir kapsayıcı seçmeniz gerekiyordu.

1. Azure [portalını](https://portal.azure.com) açın
2. ADLSGEN2 hesabına gidin
3. Azure Synapse çalışma alanı için seçtiğiniz kapsayıcıya (dosya sistemi) gidin
4. **Erişim Denetimi'ni (IAM)** tıklatın
5. Aşağıdaki rolleri atama:
   1. **Okuyucu** rolü:`Synapse_WORKSPACENAME_Users`
   2. **Depolama Blob Veri Sahibi** rolü:`Synapse_WORKSPACENAME_Admins`
   3. **Depolama Blob Veri Katılımcısı** rolü:`Synapse_WORKSPACENAME_Users`
   4. **Depolama Blob Veri Sahibi** rolü:`WORKSPACENAME`
  
### <a name="step-3-configure-the-workspace-admin-list"></a>Adım 3: Çalışma alanı yönetici listesini yapılandırma

1. Go to the [ **Azure Synapse Web UI**](https://web.azuresynapse.net)
2.  > **Güvenlik****Erişimi denetimini** **yönet'e**  > git
3. **Yönetici Ekle'yi**tıklatın ve`Synapse_WORKSPACENAME_Admins`

### <a name="step-4-configure-sql-admin-access-for-the-workspace"></a>Adım 4: Çalışma alanı için SQL Yönetici Erişimini Yapılandırma

1. [Azure portalına](https://portal.azure.com) gidin
2. Çalışma alanınıza gidin
3. **Ayarlar** > **Etkin Dizin yöneticisine** git
4. **Yönetici Yi Ayarla'yı** tıklatın
5. `Synapse_WORKSPACENAME_Admins` seçeneğini belirleyin
6. **seç'i** tıklatın
7. **Kaydet'i** tıklatın

### <a name="step-5-add-and-remove-users-and-admins-to-security-groups"></a>Adım 5: Güvenlik gruplarına Kullanıcı ve Yönetici Ekleme ve Kaldırma

1. Yönetim erişimine ihtiyaç duyan kullanıcıları ekleme`Synapse_WORKSPACENAME_Admins`
2. Diğer tüm kullanıcıları`Synapse_WORKSPACENAME_Users`

## <a name="access-control-to-data"></a>Verilere Erişim Denetimi

Temel verilere erişim denetimi üç bölüme ayrılır:

- Depolama hesabına veri düzlemi erişimi (yukarıda Adım 2'de yapılandırılmıştır)
- SQL Veritabanlarına veri düzlemi erişimi (hem SQL havuzları hem de isteğe bağlı SQL için)
- Depolama hesabı üzerinden SQL isteğe bağlı veritabanları için bir kimlik bilgisi oluşturma

## <a name="access-control-to-sql-databases"></a>SQL Veritabanlarına erişim denetimi

> [!TIP]
> Tüm SQL veritabanlarına kullanıcı erişimi sağlamak için **her** SQL veritabanı için aşağıdaki adımların çalıştırılması gerekir.

### <a name="sql-on-demand"></a>İsteğe bağlı SQL

Bir kullanıcıya **isteğe** bağlı tek bir SQL veritabanına erişim izni vermek için aşağıdaki örnekteki adımları izleyin:

1. Gİrİş YAP

    ```sql
    use master
    go
    CREATE LOGIN [John.Thomas@microsoft.com] FROM EXTERNAL PROVIDER;
    go
    ```

2. KULLANICI OLUŞTUR

    ```sql
    use yourdb -- Use your DB name
    go
    CREATE USER john FROM LOGIN [John.Thomas@microsoft.com];
    ```

3. Belirtilen rolün üyelerine KULLANICI ekleme

    ```sql
    use yourdb -- Use your DB name
    go
    alter role db_owner Add member john -- Type USER name from step 2
    ```

### <a name="sql-pools"></a>SQL Havuzları

Bir kullanıcıya **tek** bir SQL Veritabanına erişim izni vermek için aşağıdaki adımları izleyin:

1. Bağlam seçicide istenen veritabanını hedefleyen aşağıdaki komutu çalıştırarak veritabanında kullanıcıyı oluşturun (belirli veritabanlarına açılan açılır:

    ```sql
    --Create user in SQL DB
    CREATE USER [<alias@domain.com>] FROM EXTERNAL PROVIDER;
    ```

2. Kullanıcıya veritabanına erişmek için bir rol ver:

    ```sql
    --Create user in SQL DB
    EXEC sp_addrolemember 'db_owner', '<alias@domain.com>';
    ```

> [!IMPORTANT]
> *db_datareader* ve *db_datawriter,* *db_owner* izin verilmesi istiflenmemişse okuma/yazma izinleri için çalışabilir.
> Bir Spark kullanıcısının doğrudan Spark'tan SQL havuzuna/sql havuzundan doğrudan okuma ve yazma izni *db_owner* gereklidir.

Kullanıcıları oluşturduktan sonra, isteğe bağlı SQL'in depolama hesabını sorgulayabileceğini doğrulayın:

- İsteğe bağlı SQL **ana** veritabanını hedefleyen aşağıdaki komutu çalıştırın:

    ```sql
    CREATE CREDENTIAL [https://<storageaccountname>.dfs.core.windows.net]
    WITH IDENTITY='User Identity';
    ```

## <a name="access-control-to-workspace-pipeline-runs"></a>Çalışma alanı ardışık işaklerine erişim denetimi

### <a name="workspace-managed-identity"></a>Çalışma alanı tarafından yönetilen kimlik

> [!IMPORTANT]
> Veri kümelerini veya SQL havuzuna başvuran etkinlikleri içeren ardışık olarak çalıştırmak için çalışma alanı kimliğinin doğrudan SQL havuzuna erişmesi gerekir.

Çalışma alanı yönetilen kimliğin SQL havuz veritabanında ardışık hatlar çalışmasına izin vermek için her SQL havuzunda aşağıdaki komutları çalıştırın:

```sql
--Create user in DB
CREATE USER [<workspacename>] FROM EXTERNAL PROVIDER;

--Granting permission to the identity
GRANT CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;
```

Bu izin, aynı SQL havuzunda aşağıdaki komut dosyası çalıştırılarak kaldırılabilir:

```sql
--Revoking permission to the identity
REVOKE CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;

--Deleting the user in the DB
DROP USER [<workspacename>];
```

## <a name="next-steps"></a>Sonraki adımlar

Synapse SQL'de erişim ve denetime genel bir bakış için [Synapse SQL erişim denetimine](../sql/access-control.md)bakın. Veritabanı ilkeleri hakkında daha fazla bilgi edinmek [için, Bkz. İlkeler.](https://msdn.microsoft.com/library/ms181127.aspx) Veritabanı rolleri hakkında ek bilgiler, [Veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx) makalesinde bulunabilir.
