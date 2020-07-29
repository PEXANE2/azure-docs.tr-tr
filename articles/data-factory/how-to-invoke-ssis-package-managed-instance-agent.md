---
title: Azure SQL yönetilen örnek Aracısı 'nı kullanarak SSIS paketlerini çalıştırma
description: Azure SQL yönetilen örnek Aracısı 'nı kullanarak SSIS paketlerini çalıştırmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: cf1bf9e05f83610fd43146cf4c99c5006fdc97b3
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171432"
---
# <a name="run-ssis-packages-by-using-azure-sql-managed-instance-agent"></a>Azure SQL yönetilen örnek Aracısı 'nı kullanarak SSIS paketlerini çalıştırma

Bu makalede, Azure SQL yönetilen örnek Aracısı kullanılarak bir SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacağı açıklanmaktadır. Bu özellik, SSIS paketlerini şirket içi ortamınızda SQL Server Agent kullanarak zamanladığınızda benzer davranışlar sağlar.

Bu özellikle, SSıSDB 'de bulunan SSIS paketlerini bir SQL yönetilen örneğinde, Azure dosyaları gibi bir dosya sistemine veya Azure-SSIS Integration Runtime paket deposuna çalıştırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu özelliği kullanmak için en son SQL Server Management Studio (SSMS) [indirip](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017) yükleyin. Sürüm desteği ayrıntıları aşağıda verilmiştir:

- SSSıSDB veya dosya sisteminde paket çalıştırmak için SSMS sürüm 18,5 veya üstünü çalıştırın.
- Paket deposunda paket çalıştırmak için SSMS sürüm 18,6 veya üstünü yüklemelisiniz.

Ayrıca, Azure Data Factory 'de [bir Azure-SSIS tümleştirme çalışma zamanı](tutorial-create-azure-ssis-runtime-portal.md) sağlamanız gerekir. Uç nokta sunucusu olarak SQL yönetilen örneği kullanır.

## <a name="run-an-ssis-package-in-ssisdb"></a>SSSıSDB 'de bir SSIS paketi çalıştırma

Bu yordamda, SSıSDB 'de depolanan bir SSIS paketini çağırmak için SQL yönetilen örnek Aracısı 'nı kullanırsınız.

1. SSMS 'nin en son sürümünde bir SQL yönetilen örneğine bağlanın.
1. Yeni bir aracı işi ve yeni bir iş adımı oluşturun. **SQL Server Agent**altında **işler** klasörüne sağ tıklayın ve ardından **yeni iş**' ı seçin.

   ![Yeni bir aracı işi oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **Yeni Iş adımı** sayfasında tür olarak **SQL Server Integration Services paket** ' i seçin.

   ![Yeni bir SSIS iş adımı oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **Paket** sekmesinde, paket konumu olarak **SSIS Kataloğu** ' nu seçin.
1. SSıSDB bir SQL yönetilen örneği içinde olduğundan, kimlik doğrulaması belirtmeniz gerekmez.
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

Bu yordamda, dosya sisteminde depolanan bir SSIS paketini çalıştırmak için SQL yönetilen örnek Aracısı 'nı kullanırsınız.

1. SSMS 'nin en son sürümünde bir SQL yönetilen örneğine bağlanın.
1. Yeni bir aracı işi ve yeni bir iş adımı oluşturun. **SQL Server Agent**altında **işler** klasörüne sağ tıklayın ve ardından **yeni iş**' ı seçin.

   ![Yeni bir aracı işi oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **Yeni Iş adımı** sayfasında tür olarak **SQL Server Integration Services paket** ' i seçin.

   ![Yeni bir SSIS iş adımı oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **Paket** sekmesinde:

   1. **Paket konumu**için **dosya sistemi**' ni seçin.

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

## <a name="run-an-ssis-package-in-the-package-store"></a>Paket deposunda bir SSIS paketi çalıştırma

Bu yordamda, Azure-SSIS IR paket deposunda depolanan bir SSIS paketini çalıştırmak için SQL yönetilen örnek Aracısı 'nı kullanırsınız.

1. SSMS 'nin en son sürümünde bir SQL yönetilen örneğine bağlanın.
1. Yeni bir aracı işi ve yeni bir iş adımı oluşturun. **SQL Server Agent**altında **işler** klasörüne sağ tıklayın ve ardından **yeni iş**' ı seçin.

   ![Yeni bir aracı işi oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

1. **Yeni Iş adımı** sayfasında tür olarak **SQL Server Integration Services paket** ' i seçin.

   ![Yeni bir SSIS iş adımı oluşturmaya yönelik seçimler](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

1. **Paket** sekmesinde:

   1. **Paket konumu**Için **paket deposu**' nu seçin.

   1. **Paket yolu**için:

      Paket yolu **`<package store name>\<folder name>\<package name>`** .

      ![Paket deposu türü seçenekleri](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-package-store.png)

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

Bir SQL yönetilen örnek Aracısı işinden paket yürütmeyi iptal etmek için, aracı işini doğrudan durdurmak yerine aşağıdaki adımları uygulayın:

1. **msdb.dbo.sysIŞLERDEN**SQL Agent iş **kimliği** ' ni bulun.
1. Bu sorguyu kullanarak, iş KIMLIĞINE göre karşılık gelen SSIS **ExecutionID** 'sini bulun:
   ```sql
   select * from '{table for job execution}' where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
   ```
   SSIS paketleriniz SSSıSDB 'de ise, iş yürütmesi için tablo olarak **ssisdb.internal.execution_parameter_values** kullanın. SSIS paketleriniz dosya sistemiyse **ssisdb.internal.execution_parameter_values_noncatalog**kullanın.
1. SSıSDB kataloğuna sağ tıklayın ve ardından **etkin işlemler**' i seçin.

   ![SSSıSDB kataloğunun kısayol menüsünde "Etkin Işlemler"](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

1. **ExecutionID**temelinde ilgili işlemi durdurun.

## <a name="next-steps"></a>Sonraki adımlar
Ayrıca, Azure Data Factory kullanarak SSIS paketlerini de zamanlayabilirsiniz. Adım adım yönergeler için bkz. [Azure Data Factory olay tetikleyicisi](how-to-create-event-trigger.md). 
