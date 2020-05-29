---
title: Azure SQL yönetilen örnek Aracısı 'nı kullanarak SSIS paketlerini çalıştırma
description: Azure SQL veritabanı yönetilen örnek Aracısı 'nı kullanarak SSIS paketlerini çalıştırmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: fe32d67dd5d15ccf2f889cf40b79d35c890d5313
ms.sourcegitcommit: 2721b8d1ffe203226829958bee5c52699e1d2116
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84148302"
---
# <a name="run-ssis-packages-by-using-azure-sql-managed-instance-agent"></a>Azure SQL yönetilen örnek Aracısı 'nı kullanarak SSIS paketlerini çalıştırma

Bu makalede, Azure SQL veritabanı yönetilen örnek Aracısı kullanılarak bir SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacağı açıklanmaktadır. Bu özellik, SSIS paketlerini şirket içi ortamınızda SQL Server Agent kullanarak zamanladığınızda benzer davranışlar sağlar.

Bu özellikle, SSıSDB 'de depolanan SSIS paketlerini bir Azure SQL veritabanı yönetilen örneğinde veya Azure dosyaları gibi bir dosya sisteminde çalıştırabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu özelliği kullanmak için sürüm 18,5 olan SQL Server Management Studio (SSMS) en son sürümünü [indirip](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) yükleyin.

Ayrıca, Azure Data Factory 'de [bir Azure-SSIS tümleştirme çalışma zamanı](tutorial-create-azure-ssis-runtime-portal.md) sağlamanız gerekir. Uç nokta sunucusu olarak Azure SQL veritabanı yönetilen örneğini kullanır. 

## <a name="run-an-ssis-package-in-ssisdb"></a>SSSıSDB 'de bir SSIS paketi çalıştırma
Bu yordamda, SSıSDB 'de depolanan bir SSIS paketini çağırmak için Azure SQL veritabanı yönetilen örnek Aracısı 'nı kullanırsınız.

1. SSMS 'nin en son sürümünde Azure SQL veritabanı yönetilen örneğine bağlanın.
1. Yeni bir aracı işi ve yeni bir iş adımı oluşturun. **SQL Server Agent**altında **işler** klasörüne sağ tıklayın ve ardından **yeni iş**' ı seçin.

   ![Yeni bir aracı işi oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **Yeni Iş adımı** sayfasında tür olarak **SQL Server Integration Services paket** ' i seçin.

   ![Yeni bir SSIS iş adımı oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **Paket** sekmesinde, paket kaynak türü olarak **SSIS Kataloğu** ' nu seçin.
1. SSıSDB bir Azure SQL veritabanı yönetilen örneği içinde olduğundan, kimlik doğrulaması belirtmeniz gerekmez.
1. SSSıSDB 'den bir SSIS paketi belirtin.

   ![Paket kaynak türü için seçimleri olan paket sekmesi](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

1. **Yapılandırma** sekmesinde şunları yapabilirsiniz:
  
   - **Parametreler**altında parametre değerlerini belirtin.
   - **Bağlantı yöneticileri**altındaki değerleri geçersiz kılın.
   - Özelliği geçersiz kılın ve **Gelişmiş**altında günlüğe kaydetme düzeyini seçin.

   ![Paket kaynak türünün seçimleriyle birlikte Yapılandırma sekmesi](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

1. Aracı iş yapılandırmasını kaydetmek için **Tamam ' ı** seçin.
1. SSIS paketini çalıştırmak için aracı işini başlatın.


## <a name="run-an-ssis-package-in-the-file-system"></a>Dosya sisteminde bir SSIS paketi çalıştırma
Bu yordamda, dosya sisteminde depolanan bir SSIS paketini çalıştırmak için Azure SQL veritabanı yönetilen örnek Aracısı 'nı kullanırsınız.

1. SSMS 'nin en son sürümünde Azure SQL veritabanı yönetilen örneğine bağlanın.
1. Yeni bir aracı işi ve yeni bir iş adımı oluşturun. **SQL Server Agent**altında **işler** klasörüne sağ tıklayın ve ardından **yeni iş**' ı seçin.

   ![Yeni bir aracı işi oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **Yeni Iş adımı** sayfasında tür olarak **SQL Server Integration Services paket** ' i seçin.

   ![Yeni bir SSIS iş adımı oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **Paket** sekmesinde:

   1. **Paket kaynağı**için **dosya sistemi**' ni seçin.
   
   1. **Dosya kaynağı türü**için:   

      - Paketiniz Azure dosyalarına yüklenirse **Azure dosya paylaşma**' yı seçin.

        ![Dosya kaynak türü seçenekleri](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)
      
        Paket yolu **`\\<storage account name>.file.core.windows.net\<file share name>\<package name>.dtsx`** .
      
        **Paket dosyası erişimi kimlik bilgileri**altında Azure dosyasına erişmek için Azure dosya hesabı adını ve hesap anahtarını girin. Etki alanı **Azure**olarak ayarlanır.

      - Paketiniz bir ağ paylaşımında karşıya yüklenirse **ağ paylaşma**' yı seçin.
      
        Paket yolu, paket dosyanızın. dtsx uzantısıyla birlikte UNC yoludur.
      
        Ağ paylaşımının paket dosyasına erişmek için karşılık gelen etki alanını, Kullanıcı adını ve parolayı girin.
   1. Paket dosyanız bir parolayla şifrelenirse, **şifreleme parolası** ' nı seçin ve parolayı girin.
1. **Yapılandırmalar** SEKMESINDE, SSIS paketini çalıştırmak için bir yapılandırma dosyası gerekiyorsa yapılandırma dosyası yolunu girin.
   Yapılandırmanızı Azure dosyalarında depolarsanız, yapılandırma yolu olacaktır **`\\<storage account name>.file.core.windows.net\<file share name>\<configuration name>.dtsConfig`** .
1. **Yürütme seçenekleri** SEKMESINDE, SSIS paketini çalıştırmak için **Windows kimlik doğrulaması** mı yoksa **32 bit çalışma zamanı** mi kullanacağınızı seçebilirsiniz.
1. **Günlüğe kaydetme sekmesinde günlüğe** kaydetme yolunu ve günlük dosyalarını depolamak için karşılık gelen günlük erişimi kimlik bilgilerini seçebilirsiniz. 
   Varsayılan olarak, günlük kaydı, paket klasörü yoluyla aynıdır ve günlük erişimi kimlik bilgileri, paket erişimi kimlik bilgileriyle aynıdır.
   Günlüklerinizi Azure dosyalarında depolarsanız, günlük yolunuz olur **`\\<storage account name>.file.core.windows.net\<file share name>\<log folder name>`** .
1. **Değerleri ayarla** sekmesinde, paket özelliklerini geçersiz kılmak için özellik yolunu ve değerini girebilirsiniz.
 
   Örneğin, Kullanıcı değişkeninizin değerini geçersiz kılmak için yolunu aşağıdaki biçimde girin: **`\Package.Variables[User::<variable name>].Value`** .
1. Aracı iş yapılandırmasını kaydetmek için **Tamam ' ı** seçin.
1. SSIS paketini çalıştırmak için aracı işini başlatın.


## <a name="cancel-ssis-package-execution"></a>SSIS paketi yürütmeyi iptal et
Azure SQL veritabanı yönetilen örnek Aracısı işinden paket yürütmeyi iptal etmek için, aracı işini doğrudan durdurmak yerine aşağıdaki adımları uygulayın:

1. **Msdb. dbo. sysjobs**öğesinden SQL Agent iş **kimliği** ' ni bulun.
1. Bu sorguyu kullanarak, iş KIMLIĞINE göre karşılık gelen SSIS **ExecutionID** 'sini bulun:
   ```sql
   select * from '{table for job execution}' where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
   ```
   SSIS paketleriniz SSSıSDB 'leriyse, iş yürütmesi için tablo olarak **SSISDB. Internal. execution_parameter_values** kullanın. SSIS paketleriniz dosya sistemiyse **SSISDB. Internal. execution_parameter_values_noncatalog**kullanın.
1. SSıSDB kataloğuna sağ tıklayın ve ardından **etkin işlemler**' i seçin.

   ![SSSıSDB kataloğunun kısayol menüsünde "Etkin Işlemler"](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

1. **ExecutionID**temelinde ilgili işlemi durdurun.

## <a name="next-steps"></a>Sonraki adımlar
Ayrıca, Azure Data Factory kullanarak SSIS paketlerini de zamanlayabilirsiniz. Adım adım yönergeler için bkz. [Azure Data Factory olay tetikleyicisi](how-to-create-event-trigger.md). 
