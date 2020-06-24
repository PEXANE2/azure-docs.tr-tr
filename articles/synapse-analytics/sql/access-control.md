---
title: Çalışma alanları, veriler ve işlem hatları erişimini yönetme
description: Bir Azure SYNAPSE Analytics çalışma alanında (Önizleme) çalışma alanları, veriler ve işlem hatları için erişim denetimini yönetmeyi öğrenin.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: e5db52d1e28a7db5594b3b2a16bc145d0a50e2e3
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84765089"
---
# <a name="manage-access-to-workspaces-data-and-pipelines"></a>Çalışma alanları, veriler ve işlem hatları erişimini yönetme

Bir Azure SYNAPSE Analytics çalışma alanında (Önizleme) çalışma alanları, veriler ve işlem hatları için erişim denetimini yönetmeyi öğrenin.

> [!NOTE]
> GA için, SYNAPSE özgü Azure RBAC rollerinin tanıtımı aracılığıyla RBAC daha fazla geliştirilecektir.

## <a name="access-control-for-workspace"></a>Çalışma alanı için Access Control

Azure SYNAPSE çalışma alanına bir üretim dağıtımı için, ortamınızı, kullanıcıların ve yöneticilerin sağlanması kolay hale getirmek üzere düzenlemeyi öneririz.

> [!NOTE]
> Burada gerçekleştirilen yaklaşım birkaç güvenlik grubu oluşturmak ve ardından çalışma alanını tutarlı bir şekilde kullanacak şekilde yapılandırmaktır. Gruplar ayarlandıktan sonra bir yöneticinin yalnızca güvenlik grupları içindeki üyeliği yönetmesi gerekir.

### <a name="step-1-set-up-security-groups-with-names-following-this-pattern"></a>1. Adım: bu kalıbı izleyen adlarla güvenlik grupları ayarlama

1. Adlı güvenlik grubu oluştur`Synapse_WORKSPACENAME_Users`
2. Adlı güvenlik grubu oluştur`Synapse_WORKSPACENAME_Admins`
3. `Synapse_WORKSPACENAME_Users` için `Synapse_WORKSPACENAME_Admins` eklendi

> [!NOTE]
> [Bu makalede](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal)bir güvenlik grubu oluşturmayı öğrenin.
>
> [Bu makaledeki](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-membership-azure-portal)başka bir güvenlik grubundan bir güvenlik grubu eklemeyi öğrenin.
>
> ÇALıŞMAALANıADı-bu bölümü gerçek çalışma alanı adınızla değiştirmelisiniz.

### <a name="step-2-prepare-the-default-adls-gen2-account"></a>2. Adım: varsayılan ADLS 2. hesabını hazırlama

Çalışma alanınızı sağladığınızda, çalışma alanının kullanması için bir [Azure Data Lake Storage 2.](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-introduction) hesabı ve FileSystem için bir kapsayıcı seçmeniz gerekiyordu.

1. [Azure Portal](https://portal.azure.com) açın
2. Azure Data Lake Storage 2. hesabına gidin
3. Azure SYNAPSE çalışma alanı için seçtiğiniz kapsayıcıya (dosya sistemi) gidin
4. **Access Control (IAM)** seçeneğine tıklayın
5. Aşağıdaki rolleri atayın:
   1. **Okuyucu** rolü:`Synapse_WORKSPACENAME_Users`
   2. **Depolama Blobu veri sahibi** rolü:`Synapse_WORKSPACENAME_Admins`
   3. **Depolama Blobu veri katılımcısı** rolü:`Synapse_WORKSPACENAME_Users`
   4. **Depolama Blobu veri sahibi** rolü:`WORKSPACENAME`

> [!NOTE]
> ÇALıŞMAALANıADı-bu bölümü gerçek çalışma alanı adınızla değiştirmelisiniz.

### <a name="step-3-configure-the-workspace-admin-list"></a>3. Adım: çalışma alanı yönetici listesini yapılandırma

1. [ **Azure SYNAPSE Web Kullanıcı arabirimine** gidin](https://web.azuresynapse.net)
2. **Manage**   >  **Güvenlik**  >  **erişimi denetimini** Yönet 'e gidin
3. **Yönetici Ekle**' ye tıklayın ve`Synapse_WORKSPACENAME_Admins`

### <a name="step-4-configure-sql-admin-access-for-the-workspace"></a>4. Adım: çalışma alanı için SQL yönetici erişimini yapılandırma

1. [Azure Portal](https://portal.azure.com) git
2. Çalışma alanınıza gidin
3. **Ayarlar**  >  **Active Directory yönetici** 'a git
4. **Yönetici ayarla** öğesine tıklayın
5. `Synapse_WORKSPACENAME_Admins` seçeneğini belirleyin
6. **Seç** 'e tıklayın
7. **Kaydet** 'e tıklayın

> [!NOTE]
> ÇALıŞMAALANıADı-bu bölümü gerçek çalışma alanı adınızla değiştirmelisiniz.

### <a name="step-5-add-and-remove-users-and-admins-to-security-groups"></a>5. Adım: kullanıcıları ve yöneticileri güvenlik gruplarına ekleme ve kaldırma

1. Üzerinde yönetici erişimi olması gereken kullanıcıları ekleyin`Synapse_WORKSPACENAME_Admins`
2. Diğer tüm kullanıcıları buraya ekle`Synapse_WORKSPACENAME_Users`

> [!NOTE]
> [Bu makalede](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal) bir güvenlik grubuna üye olarak Kullanıcı ekleme hakkında bilgi edinin
> 
> ÇALıŞMAALANıADı-bu bölümü gerçek çalışma alanı adınızla değiştirmelisiniz.

## <a name="access-control-to-data"></a>Verilere Access Control

Temel alınan verilere erişim denetimi üç parçaya bölünür:

- Depolama hesabına veri düzlemi erişimi (adım 2 ' de yukarıda zaten yapılandırılmış)
- SQL veritabanlarına veri düzlemi erişimi (SQL havuzları ve isteğe bağlı SQL için)
- Depolama hesabı üzerinden SQL isteğe bağlı veritabanları için kimlik bilgisi oluşturma

## <a name="access-control-to-sql-databases"></a>SQL veritabanlarına erişim denetimi

> [!TIP]
> **Her** SQL veritabanı için, Kullanıcı bir sysadmin rolü atayabileceğiniz bölüm [sunucu düzeyi IZNI](#server-level-permission) hariç tüm SQL veritabanlarına Kullanıcı erişimi vermek için aşağıdaki adımların çalıştırılması gerekir.

### <a name="sql-on-demand"></a>İsteğe bağlı SQL

Bu bölümde, kullanıcıya belirli bir veritabanı veya tam sunucu izinleri için bir izin verme hakkında örnekler bulabilirsiniz.

#### <a name="database-level-permission"></a>Veritabanı düzeyinde izin

Bir kullanıcıya **tek** bir SQL isteğe bağlı veritabanına erişim izni vermek için bu örnekteki adımları izleyin:

1. OTURUM açma oluştur

    ```sql
    use master
    go
    CREATE LOGIN [alias@domain.com] FROM EXTERNAL PROVIDER;
    go
    ```

2. Kullanıcı Oluştur

    ```sql
    use yourdb -- Use your DB name
    go
    CREATE USER alias FROM LOGIN [alias@domain.com];
    ```

3. Belirtilen rolün üyelerine Kullanıcı Ekle

    ```sql
    use yourdb -- Use your DB name
    go
    alter role db_owner Add member alias -- Type USER name from step 2
    ```

> [!NOTE]
> Diğer adı, kullandığınız şirket etki alanı ile erişim ve etki alanı vermek istediğiniz kullanıcının diğer adıyla değiştirin.

#### <a name="server-level-permission"></a>Sunucu düzeyi izni

Bir kullanıcıya **Tüm** SQL isteğe bağlı veritabanlarına tam erişim vermek için bu örnekteki adımı izleyin:

```sql
CREATE LOGIN [alias@domain.com] FROM EXTERNAL PROVIDER;
ALTER SERVER ROLE  sysadmin  ADD MEMBER [alias@domain.com];
```

### <a name="sql-pools"></a>SQL havuzları

Bir kullanıcıya **tek** bir SQL veritabanına erişim izni vermek için şu adımları izleyin:

1. Bağlam seçicide istenen veritabanını hedefleyen aşağıdaki komutu çalıştırarak kullanıcıyı veritabanında oluşturun (veritabanlarını seçmek için açılan menü):

    ```sql
    --Create user in SQL DB
    CREATE USER [<alias@domain.com>] FROM EXTERNAL PROVIDER;
    ```

2. Kullanıcıya veritabanına erişmek için bir rol verin:

    ```sql
    --Create user in SQL DB
    EXEC sp_addrolemember 'db_owner', '<alias@domain.com>';
    ```

> [!IMPORTANT]
> *db_datareader* ve *db_datawriter* , *db_owner* izin verilmesinin istenmeyen olması durumunda okuma/yazma izinleri için çalışabilir.
> Spark kullanıcısının, bir SQL havuzundan doğrudan Spark 'tan/içinden okuması ve yazması için *db_owner* izin gerekir.

Kullanıcıları oluşturduktan sonra, SQL isteğe bağlı, depolama hesabını sorgulayabilecek olduğunu doğrulayın.

## <a name="access-control-to-workspace-pipeline-runs"></a>Çalışma alanı işlem hattı çalıştırmaları için erişim denetimi

### <a name="workspace-managed-identity"></a>Çalışma alanı tarafından yönetilen kimlik

> [!IMPORTANT]
> Bir SQL havuzuna başvuran veri kümelerini veya etkinlikleri içeren işlem hatlarını başarılı bir şekilde çalıştırmak için, çalışma alanı kimliğine doğrudan SQL havuzuna erişim verilmesi gerekir.

Çalışma alanı tarafından yönetilen kimliğin SQL havuzu veritabanında işlem hatlarını çalıştırmasına izin vermek için her bir SQL havuzunda aşağıdaki komutları çalıştırın:

```sql
--Create user in DB
CREATE USER [<workspacename>] FROM EXTERNAL PROVIDER;

--Granting permission to the identity
GRANT CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;
```

Bu izin, aynı SQL havuzunda aşağıdaki betiği çalıştırılarak kaldırılabilir:

```sql
--Revoking permission to the identity
REVOKE CONTROL ON DATABASE::<SQLpoolname> TO <workspacename>;

--Deleting the user in the DB
DROP USER [<workspacename>];
```

## <a name="next-steps"></a>Sonraki adımlar

SYNAPSE çalışma alanı tarafından yönetilen kimliğe genel bakış için bkz. [Azure SYNAPSE çalışma alanı yönetilen kimliği](../security/synapse-workspace-managed-identity.md). Veritabanı sorumluları hakkında daha fazla bilgi edinmek için bkz. [sorumlular](https://msdn.microsoft.com/library/ms181127.aspx). Veritabanı rolleri hakkında ek bilgi, [veritabanı rolleri](https://msdn.microsoft.com/library/ms189121.aspx) makalesinde bulunabilir.
