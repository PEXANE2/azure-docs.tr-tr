---
title: SSIS paketlerini Azure SQL Yönetilen Örnek Aracısı ile yürütün
description: SSIS paketlerini Azure SQL Yönetilen Örnek Aracısı ile nasıl yürütüleceklerini öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: lle
author: lle
ms.date: 04/14/2020
ms.openlocfilehash: b3b7a25149a9d075c81b30307ade2beb71907637
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81394728"
---
# <a name="execute-ssis-packages-by-azure-sql-managed-instance-agent"></a>SSIS paketlerini Azure SQL Yönetilen Örnek Aracısı ile yürütün
Bu makalede, Azure SQL Yönetilen Örnek Aracı'yı kullanarak bir SQL Server Integration Services (SSIS) paketinin nasıl çalıştırılacak işletileceği açıklanmaktadır. Bu özellik, tıpkı ssis paketlerini sql server agent tarafından prem ortamınızda zamanlarken olduğu gibi benzer davranışlar sağlar.

Bu özellik sayesinde, Azure SQL Yönetilen Örneği'nin SSISDB'sinde veya Azure Dosyaları gibi Dosya Sistemi'nde depolanan SSIS paketlerini çalıştırabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Bu özelliği kullanmak için, sürüm 18.5 veya sonraki sürüm olan SSMS'in en son sürümünü indirin ve yükleyin. [Bu web sitesinden](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017)indirin.

Azure Veri Fabrikası'nda, Azure SQL Yönetilen Örneği'ni bitiş noktası sunucusu olarak kullanan bir Azure-SSIS Tümleştirme Çalışma Süresi sağlamanız gerekir. Zaten sağlamadıysanız, öğreticideki talimatları izleyerek bunu [sağedin.](tutorial-create-azure-ssis-runtime-portal.md) 

## <a name="run-ssis-packages-in-ssisdb-by-azure-sql-managed-instance-agent"></a>SSIS paketlerini Azure SQL Yönetilen Örnek Aracısı ile SSISDB'de çalıştırma
Bu adımda, Azure SQL Yönetilen Örnek'te SSISDB'de depolanan SSIS paketlerini çağırmak için Azure SQL Yönetilen Örnek Aracı'yı kullanırsınız.
1. SSMS'in en son sürümünde Azure SQL Yönetilen Örneği'ne bağlanın.
2. Yeni bir Ajan İş ve yeni bir İş adımı oluşturun.

![Yeni Ajan İş](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. Yeni **İş Adımı** **sayfasında, SQL Server Tümleştirme Hizmetleri Paketi** türünü seçin.

![Yeni SSIS İş adımı](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. **Paket** sekmesinde, paket kaynağı türü olarak **SSIS Kataloğu'nu** seçin.
5. SSISDB aynı Azure SQL Yönetilen Örneği'nde olduğundan, kimlik doğrulaması belirtmeniz gerekmez.
6. SSISDB'nizden bir SSIS paketi belirtin.

![Paket Kaynak Türü - SSIS Kataloğu](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb.png)

7. **Yapılandırmalar** sekmesinde **parametre** değerlerini belirtebilir, Bağlantı **Yöneticileri'ndeki**değerleri geçersiz kılabilir, **Özelliği** geçersiz kılabilir ve **Günlük düzeyini**seçebilirsiniz.

![Paket Kaynak Türü - SSIS Katalog Yapılandırması](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-ssisdb-configuration.png)

8. Yukarıdaki tüm yapılandırmayı tamamladıktan sonra Aracı İş yapılandırmasını kaydetmek için **Tamam'ı** tıklatın.
9. SSIS paketini çalıştırmak için Ajan İş'i başlatın.


## <a name="run-ssis-packages-in-file-system-by-azure-sql-managed-instance-agent"></a>SSIS paketlerini dosya sisteminde Azure SQL yönetilen örnek aracısı ile çalıştırma
Bu adımda, Dosya Sistemi'nde depolanan SSIS paketlerini çalıştırmak üzere çağırmak için Azure SQL Yönetilen Örnek Aracı'yı kullanırsınız.
1. SSMS'in en son sürümünde Azure SQL Yönetilen Örneği'ne bağlanın.
2. Yeni bir Ajan İş ve yeni bir İş adımı oluşturun.

   ![Yeni Ajan İş](./media/how-to-invoke-ssis-package-managed-instance-agent/new-agent-job.png)

3. Yeni **İş Adımı** **sayfasında, SQL Server Tümleştirme Hizmetleri Paketi** türünü seçin.

   ![Yeni SSIS İş adımı](./media/how-to-invoke-ssis-package-managed-instance-agent/new-ssis-job-step.png)

4. **Paket** sekmesinde, paket kaynağı türü olarak **Dosya sistemini** seçin.

   ![Paket Kaynak Türü - Dosya Sistemi](./media/how-to-invoke-ssis-package-managed-instance-agent/package-source-file-system.png)

   1. Paketiniz Azure Dosyası'na yüklenmişse, dosya kaynağı türü olarak **Azure dosya paylaşımını** seçin.
      - Paket yolu ** \\ <storage account name>.file.core.windows.net\<dosya paylaşım \<adı>paket adı>.dtsx**
      - Azure dosyasına erişmek için Paket **dosyası erişim kimlik bilgisine** Azure dosya hesabı adı ve hesap anahtarı yazın. Etki alanı **Azure**olarak ayarlanır.
   2. Paketiniz bir ağ paylaşımına yüklenmişse, dosya kaynağı türü olarak **Ağ paylaşımını** seçin.
      - Paket yolu, dtsx uzantısı ile paket dosyanızın **UNC yoludur.**
      - Ağ paylaşım paketi dosyasına erişmek için ilgili **etki alanını,** **kullanıcı adını**ve **parolayı** yazın.
   3. Paket dosyanız parolayla şifrelenmişse, **Şifreleme parolasını** seçin ve parolayı yazın.

 5. **Yapılandırmalar** sekmesinde, SSIS paketini yürütmek için bir yapılandırma dosyasına ihtiyacınız varsa **yapılandırma dosya yolunu** yazın.
 6. Yürütme **seçenekleri** sekmesinde, SSIS paketini yürütmek için **windows kimlik doğrulaması** mı yoksa **32 bit çalışma zamanı** mı kullanacağınızı seçebilirsiniz.
 7. **Günlüğe Kaydetme** sekmesinde, günlük dosyalarını depolamak için **günlük yolunu** ve ilgili günlük erişim kimlik bilgisini seçebilirsiniz. Varsayılan olarak, günlük yolu paket klasörü yolu ile aynı olacak ve günlük erişim kimlik bilgileri paket erişim kimlik bilgileriyle aynı olacaktır.
 8. Değerleri **Ayarla** sekmesinde, paket özelliklerini geçersiz kılmak için **Özellik Yolu** ve **Değer'i** yazabilirsiniz.
 Örneğin, kullanıcı değişkeninizin değerini geçersiz kılmak için yolunu aşağıdaki biçimde girin: **<variable name>\Package.Variables[User:: ]. Değer**.
 9. Yukarıdaki tüm yapılandırmayı tamamladıktan sonra Aracı İş yapılandırmasını kaydetmek için **Tamam'ı** tıklatın.
 10. SSIS paketini çalıştırmak için Ajan İş'i başlatın.


 ## <a name="cancel-ssis-package-execution"></a>SSIS paket yürütmeyi iptal etme
 Bir Azure SQL Yönetilen Aracı işinden paket yürütmeyi iptal etmek için aracı işini doğrudan durdurmak yerine aşağıdaki adımları izlemeniz gerekir.
 1. **MSDB.dbo.sysjobs**adresinden SQL aracınızı **jobId'i** bulun.
 2. Aşağıdaki sorguile iş kimliğine dayalı ilgili SSIS **executionId'i** bulun:
    ```sql
    select * from ssisdb.internal.execution_parameter_values_noncatalog where  parameter_value = 'SQL_Agent_Job_{jobId}' order by execution_id desc
    ```
 3. SSIS kataloğu altında **Etkin Operasyonlar'ı** seçin.

    ![Paket Kaynak Türü - Dosya Sistemi](./media/how-to-invoke-ssis-package-managed-instance-agent/catalog-active-operations.png)

 4. ExecutionId'e dayalı ilgili işlemi **durdurun.**

## <a name="next-steps"></a>Sonraki adımlar
 SSIS paketlerini Azure Veri Fabrikası'nı kullanarak da zamanlayabilirsiniz. Adım adım yönergeler için [Azure Veri Fabrikası Olay Tetikleyicisi'ne](how-to-create-event-trigger.md)bakın. 