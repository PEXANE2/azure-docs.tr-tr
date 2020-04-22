---
title: Windows kimlik doğrulamasıyla veri depolarına ve dosya paylaşımlarına erişin
description: Veri depolarına ve dosya paylaşımlarına Windows kimlik doğrulamasıyla erişen paketleri çalıştırmak için Azure SQL Veritabanı'nda SSIS kataloğunu ve Azure Veri Fabrikası'ndaki Azure-SSIS Tümleştirme Çalışma Süresi'ni nasıl yapılandırıştırmayı öğrenin.
ms.date: 3/22/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 0c4cdc3481fb58efd8eaa4cd83e1d6167f203a4e
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81760192"
---
# <a name="access-data-stores-and-file-shares-with-windows-authentication-from-ssis-packages-in-azure"></a>Azure'da SSIS paketlerinden Windows kimlik doğrulaması ile veri depolarına ve dosya paylaşımlarına erişme

Azure Veri Fabrikası'nda (ADF) Azure-SSIS Tümleştirme Çalışma Zamanı (IR) üzerinde çalışan SSIS paketlerinden SQL Sunucuları, dosya paylaşımları, Azure Dosyaları vb. gibi veri depolarına erişmek için Windows kimlik doğrulamasını kullanabilirsiniz. Veri depolarınız şirket içinde olabilir, Azure Sanal Makineleri'nde (VM) barındırılabilir veya yönetilen hizmetler olarak Azure'da çalıştırılabilir. Şirket içindeyseler, azure-SSIS IR'nize şirket içi ağınıza bağlı bir Sanal Ağa (Microsoft Azure Sanal Ağı) katılmanız gerekir, [bkz.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network) Azure-SSIS IR'nizde çalışan SSIS paketlerinden Windows kimlik doğrulaması ile veri depolarına erişmek için dört yöntem vardır:

| Bağlantı yöntemi | Etkin kapsam | Kurulum adımı | Paketlerde erişim yöntemi | Kimlik bilgisi kümelerinin ve bağlı kaynakların sayısı | Bağlı kaynakların türü | 
|---|---|---|---|---|---|
| Etkinlik düzeyinde yürütme bağlamı ayarlama | Execute SSIS Paketi etkinliği başına | **ADF** ardışık hatlarında SSIS Paketi etkinliklerini yürüterken SSIS paketlerini çalıştırırken Windows kimlik doğrulama özelliğini "Yürütme/Çalıştır" bağlamı ayarlamak üzere yapılandırın.<br/><br/> Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) | Örneğin dosya paylaşımlarını veya Azure Dosyalarını kullanıyorsanız, kaynaklara UNC yolu üzerinden doğrudan paketlerde erişin: `\\YourFileShareServerName\YourFolderName` veya`\\YourAzureStorageAccountName.file.core.windows.net\YourFolderName` | Bağlı tüm kaynaklar için yalnızca bir kimlik bilgisi kümesini destekleyin | - Şirket/Azure VM'lerinde dosya paylaşımları<br/><br/> - Azure Dosyaları, [bkz.](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) <br/><br/> - Windows kimlik doğrulaması ile şirket/Azure VM'lerde SQL Sunucuları<br/><br/> - Windows kimlik doğrulaması ile diğer kaynaklar |
| Katalog düzeyinde yürütme bağlamı ayarlama | Azure-SSIS IR'ye göre, ancak etkinlik düzeyinde bir yürütme bağlamı ayarlarken geçersiz kılınır (yukarıya bakın) | "Yürütme/Çalıştırma" `catalog.set_execution_credential` bağlamını ayarlamak için SSISDB depolanan yordamı çalıştırın.<br/><br/> Daha fazla bilgi için, aşağıdaki makalenin geri kalanına bakın. | Örneğin dosya paylaşımlarını veya Azure Dosyalarını kullanıyorsanız, kaynaklara UNC yolu üzerinden doğrudan paketlerde erişin: `\\YourFileShareServerName\YourFolderName` veya`\\YourAzureStorageAccountName.file.core.windows.net\YourFolderName` | Bağlı tüm kaynaklar için yalnızca bir kimlik bilgisi kümesini destekleyin | - Şirket/Azure VM'lerinde dosya paylaşımları<br/><br/> - Azure Dosyaları, [bkz.](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) <br/><br/> - Windows kimlik doğrulaması ile şirket/Azure VM'lerde SQL Sunucuları<br/><br/> - Windows kimlik doğrulaması ile diğer kaynaklar |
| Komut yoluyla `cmdkey` kimlik bilgilerini kalıcı olarak | Azure-SSIS IR'ye göre, ancak bir etkinlik/katalog düzeyi yürütme bağlamı ayarlarken geçersiz kılınır (yukarıya bakın) | Azure-SSIS IR'nizi `cmdkey` sağlarken (`main.cmd`) komutu özel bir kurulum komutdosyasında yürütün , `cmdkey /add:YourFileShareServerName /user:YourDomainName\YourUsername /pass:YourPassword` `cmdkey /add:YourAzureStorageAccountName.file.core.windows.net /user:azure\YourAzureStorageAccountName /pass:YourAccessKey`örneğin dosya paylaşımlarını veya Azure Dosyalarını kullanıyorsanız: veya .<br/><br/> Daha fazla bilgi için [Azure-SSIS IR için kurulumu Özelleştir'e](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)bakın. | Örneğin dosya paylaşımlarını veya Azure Dosyalarını kullanıyorsanız, kaynaklara UNC yolu üzerinden doğrudan paketlerde erişin: `\\YourFileShareServerName\YourFolderName` veya`\\YourAzureStorageAccountName.file.core.windows.net\YourFolderName` | Farklı bağlı kaynaklar için birden çok kimlik bilgisi kümesini destekleme | - Şirket/Azure VM'lerinde dosya paylaşımları<br/><br/> - Azure Dosyaları, [bkz.](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) <br/><br/> - Windows kimlik doğrulaması ile şirket/Azure VM'lerde SQL Sunucuları<br/><br/> - Windows kimlik doğrulaması ile diğer kaynaklar |
| Paket yürütme zamanında sürücüleri montaj (kalıcı olmayan) | Paket başına | Örneğin, paketlerinizdeki kontrol akışının başına eklenen Yürütme İşlemi Görevi'nde komutu yürütün, `net use``net use D: \\YourFileShareServerName\YourFolderName` | Eşlenen sürücüler aracılığıyla dosya paylaşımlarını erişin | Farklı dosya paylaşımları için birden çok sürücüye destek | - Şirket/Azure VM'lerinde dosya paylaşımları<br/><br/> - Azure Dosyaları, [bkz.](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows) |
|||||||

> [!WARNING]
> Windows kimlik doğrulaması ile veri depolarına erişmek için yukarıdaki yöntemlerden herhangi birini kullanmazsanız, Windows kimlik doğrulamasına bağlı olan paketleriniz bunlara erişemez ve çalışma zamanında başarısız olur. 

Bu makalenin geri kalanında, veri depolarına erişmek için Windows kimlik doğrulamasını kullanan Azure-SSIS IR'de paketleri çalıştırmak için Azure SQL Veritabanı sunucusunda barındırılan SSIS kataloğunun (SSISDB) nasıl yapılandırılabildiğini açıklanmaktadır. 

## <a name="you-can-only-use-one-set-of-credentials"></a>Yalnızca bir kimlik bilgileri kümesi kullanabilirsiniz

Windows kimlik doğrulamasını bir SSIS paketinde kullandığınızda, yalnızca bir kimlik bilgileri kümesi kullanabilirsiniz. Bu makaledeki adımları izlediğinizde sağladığınız etki alanı kimlik bilgileri, siz bunları değiştirene veya kaldırana kadar Azure-SSIS IR'nizde etkileşimli veya zamanlanmış tüm paket yürütmeleri için geçerlidir. Paketinizin farklı kimlik bilgilerine sahip birden çok veri deposuna bağlanması gerekiyorsa, yukarıdaki alternatif yöntemleri göz önünde bulundurmanız gerekir.

## <a name="provide-domain-credentials-for-windows-authentication"></a>Windows kimlik doğrulaması için etki alanı kimlik bilgilerini sağlama

Paketlerin şirket teki veri depolarına erişmek için Windows kimlik doğrulamasını kullanmasına izin veren etki alanı kimlik bilgilerini sağlamak için aşağıdaki leri yapın:

1. SQL Server Management Studio (SSMS) veya başka bir araçla, SSISDB'yi barındıran Azure SQL Veritabanı sunucusuna/Yönetilen Örneği'ne bağlanın. Daha fazla bilgi için [Azure'da SSISDB'ye bağlan'a](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)bakın.

2. Geçerli veritabanı olarak SSISDB ile bir sorgu penceresi açın.

3. Aşağıdaki depolanan yordamı çalıştırın ve uygun etki alanı kimlik bilgilerini sağlayın:

   ```sql
   catalog.set_execution_credential @user='<your user name>', @domain='<your domain name>', @password='<your password>'
   ```

4. SSIS paketlerinizi çalıştırın. Paketler, Windows kimlik doğrulaması ile şirket içinde veri depolarına erişmek için sağladığınız kimlik bilgilerini kullanır.

### <a name="view-domain-credentials"></a>Etki alanı kimlik bilgilerini görüntüleme

Etkin etki alanı kimlik bilgilerini görüntülemek için aşağıdaki leri yapın:

1. SSMS veya başka bir araçla, SSISDB'yi barındıran Azure SQL Veritabanı sunucusuna/Yönetilen Örneği'ne bağlanın. Daha fazla bilgi için [Azure'da SSISDB'ye bağlan'a](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)bakın.

2. Geçerli veritabanı olarak SSISDB ile bir sorgu penceresi açın.

3. Aşağıdaki depolanan yordamı çalıştırın ve çıktıyı kontrol edin:

   ```sql
   SELECT * 
   FROM catalog.master_properties
   WHERE property_name = 'EXECUTION_DOMAIN' OR property_name = 'EXECUTION_USER'
   ```

### <a name="clear-domain-credentials"></a>Etki alanı kimlik bilgilerini temizleme
Bu makalede açıklandığı şekilde sağladığınız kimlik bilgilerini temizlemek ve kaldırmak için aşağıdaki leri yapın:

1. SSMS veya başka bir araçla, SSISDB'yi barındıran Azure SQL Veritabanı sunucusuna/Yönetilen Örneği'ne bağlanın. Daha fazla bilgi için [Azure'da SSISDB'ye bağlan'a](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)bakın.

2. Geçerli veritabanı olarak SSISDB ile bir sorgu penceresi açın.

3. Aşağıdaki depolanan yordamı çalıştırın:

   ```sql
   catalog.set_execution_credential @user='', @domain='', @password=''
   ```

## <a name="connect-to-a-sql-server-on-premises"></a>Şirket içinde bir SQL Server'a bağlanma

Bir SQL Server'a şirket içinde bağlanıp bağlanamayacağınızı kontrol etmek için aşağıdaki leri yapın:

1. Bu testi çalıştırmak için etki alanına katılmayan bir bilgisayar bulun.

2. Etki alanına katılmayan bilgisayarda, kullanmak istediğiniz etki alanı kimlik bilgileriyle SSMS başlatmak için aşağıdaki komutu çalıştırın:

   ```cmd
   runas.exe /netonly /user:<domain>\<username> SSMS.exe
   ```

3. SSMS'den, SQL Server'a şirket içinde bağlanıp bağlanamayacağınızı kontrol edin.

### <a name="prerequisites"></a>Ön koşullar

Azure'da çalışan paketlerden bir SQL Server'a şirket içinde erişmek için aşağıdaki leri yapın:

1.  SQL Server Configuration Manager'da TCP/IP protokolünü etkinleştirin.

2. Windows güvenlik duvarından erişime izin verin. Daha fazla bilgi için, [SQL Server'a erişmek için Windows güvenlik duvarLarını Yapılandır'a](https://docs.microsoft.com/sql/sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access)bakın.

3. Azure-SSIS IR'nize, şirket içinde SQL Server'a bağlı bir Microsoft Azure Sanal Ağı'na katılın.  Daha fazla bilgi için bkz: [Azure-SSIS IR'yi bir Microsoft Azure Sanal Ağına Katılın.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

4. Bu makalede açıklandığı `catalog.set_execution_credential` gibi kimlik bilgilerini sağlamak için SSISDB depolanan yordamı kullanın.

## <a name="connect-to-a-file-share-on-premises"></a>Şirket içinde dosya paylaşımına bağlanma

Şirket içinde bir dosya paylaşımına bağlanıp bağlanamayacağınızı denetlemek için aşağıdaki leri yapın:

1. Bu testi çalıştırmak için etki alanına katılmayan bir bilgisayar bulun.

2. Etki alanına katılmayan bilgisayarda aşağıdaki komutları çalıştırın. Bu komutlar, kullanmak istediğiniz etki alanı kimlik bilgilerini içeren bir komut istemi penceresi açar ve ardından bir dizin listesi alarak şirket içindeki dosya paylaşımına bağlantısını sınar.

   ```cmd
   runas.exe /netonly /user:<domain>\<username> cmd.exe
   dir \\fileshare
   ```

3. Dizin listesinin şirket içindeki dosya paylaşımı için döndürülüp döndürülmediğini denetleyin.

### <a name="prerequisites"></a>Ön koşullar

Azure'da çalışan paketlerden şirket içinde bir dosya paylaşımına erişmek için aşağıdaki leri yapın:

1. Windows güvenlik duvarından erişime izin verin.

2. Azure-SSIS IR'nize şirket içinde dosya paylaşımına bağlı bir Microsoft Azure Sanal Ağı'na katılın.  Daha fazla bilgi için bkz: [Azure-SSIS IR'yi bir Microsoft Azure Sanal Ağına Katılın.](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)

3. Bu makalede açıklandığı `catalog.set_execution_credential` gibi kimlik bilgilerini sağlamak için SSISDB depolanan yordamı kullanın.

## <a name="connect-to-a-file-share-on-azure-vm"></a>Azure VM'de dosya paylaşımına bağlanma

Azure'da çalışan paketlerden Azure VM'deki bir dosya paylaşımına erişmek için aşağıdaki leri yapın:

1. SSMS veya başka bir araçla, SSISDB'yi barındıran Azure SQL Veritabanı sunucusuna/Yönetilen Örneği'ne bağlanın. Daha fazla bilgi için [Azure'da SSISDB'ye bağlan'a](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)bakın.

2. Geçerli veritabanı olarak SSISDB ile bir sorgu penceresi açın.

3. Aşağıdaki depolanan yordamı çalıştırın ve uygun etki alanı kimlik bilgilerini sağlayın:

   ```sql
   catalog.set_execution_credential @domain = N'.', @user = N'username of local account on Azure virtual machine', @password = N'password'
   ```

## <a name="connect-to-a-file-share-in-azure-files"></a>Azure Dosyaları'nda dosya paylaşımına bağlanma

Azure Dosyaları hakkında daha fazla bilgi için [Azure Dosyaları'na](https://azure.microsoft.com/services/storage/files/)bakın.

Azure'da çalışan paketlerden Azure Dosyaları'ndaki bir dosya paylaşımına erişmek için aşağıdaki leri yapın:

1. SSMS veya başka bir araçla, SSISDB'yi barındıran Azure SQL Veritabanı sunucusuna/Yönetilen Örneği'ne bağlanın. Daha fazla bilgi için [Azure'da SSISDB'ye bağlan'a](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)bakın.

2. Geçerli veritabanı olarak SSISDB ile bir sorgu penceresi açın.

3. Aşağıdaki depolanan yordamı çalıştırın ve uygun etki alanı kimlik bilgilerini sağlayın:

   ```sql
   catalog.set_execution_credential @domain = N'Azure', @user = N'<storage-account-name>', @password = N'<storage-account-key>'
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Paketlerinizi dağıtın. Daha fazla bilgi için bkz: [SSMS ile Azure'a bir SSIS projesini dağıtın.](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-deploy-ssms)
- Paketlerinizi çalıştırın. Daha fazla bilgi için [Azure'da SSMS ile SSIS paketlerini çalıştır'a](https://docs.microsoft.com/sql/integration-services/ssis-quickstart-run-ssms)bakın.
- Paketlerinizi planlayın. Daha fazla bilgi için [Azure'daki SSIS paketlerini zamanlayın.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages-ssms?view=sql-server-ver15)