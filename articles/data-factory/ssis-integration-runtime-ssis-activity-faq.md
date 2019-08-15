---
title: SSIS tümleştirme çalışma zamanı 'nda paket yürütmeye sorun giderme | Microsoft Docs
description: Bu makale, SSIS tümleştirme çalışma zamanı 'nda SSIS paketi yürütme sorunlarını giderme kılavuzu sağlar
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/15/2019
author: wenjiefu
ms.author: wenjiefu
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: a7ad0f3be754029c654b04d19750aab7bbcd210d
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933642"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>SSIS tümleştirme çalışma zamanı 'nda paket yürütmeye sorun giderme

Bu makale, SSIS tümleştirme çalışma zamanı 'nda SQL Server Integration Services (SSIS) paketlerini yürütürken bulabileceğiniz en yaygın hataları içerir. Hataları çözmeye yönelik olası nedenleri ve eylemleri açıklar.

## <a name="where-to-find-logs-for-troubleshooting"></a>Sorun giderme için günlüklerin bulunacağı yer

SSIS paketi yürütme etkinliğinin çıkışını denetlemek için Azure Data Factory portalını kullanın. Çıktı, yürütme sonucunu, hata iletilerini ve işlem KIMLIĞINI içerir. Ayrıntılar için bkz. işlem hattını [izleme](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline).

Yürütmenin ayrıntı günlüklerini denetlemek için SSIS kataloğunu (SSıSDB) kullanın. Ayrıntılar için bkz. [çalışan paketleri ve diğer Işlemleri izleme](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017).

## <a name="common-errors-causes-and-solutions"></a>Yaygın hatalar, nedenler ve çözümler

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Hata iletisi: "Bağlantı zaman aşımı süresi doldu" veya "hizmet isteğinizi işlerken bir hatayla karşılaştı. Lütfen yeniden deneyin. "

Olası nedenler ve önerilen eylemler şunlardır:
* Veri kaynağı veya hedef aşırı yüklendi. Veri kaynağınızdaki veya hedefteki yükü denetleyin ve yeterli kapasiteye sahip olup olmadığını görün. Örneğin, Azure SQL veritabanı 'nı kullandıysanız, veritabanının zaman aşımına uğrar olması durumunda ölçeklendirmeyi değerlendirin.
* SSIS tümleştirme çalışma zamanı ile veri kaynağı veya hedef arasındaki ağ, özellikle bağlantı çapraz bölgedeyse ve şirket içi ile Azure arasında kararsız hale geldi. Aşağıdaki adımları izleyerek, SSIS paketine yeniden deneme modelini uygulayın:
  * SSIS paketlerinizin, yan etkileri olmadan hata durumunda yeniden çalıştığından emin olun (örneğin, veri kaybı veya veri çoğaltma).
  * **Genel** SEKMESINDE, **SSIS paketi yürütme** etkinliğinin **yeniden denenme** ve **yeniden deneme aralığını** yapılandırın. ![Genel sekmesindeki özellikleri ayarla](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * Bir ADO.NET ve OLE DB kaynak veya hedef bileşeni için SSIS paketi veya SSIS etkinliğinde bağlantı Yöneticisi 'nde **ConnectRetryCount** ve **ConnectRetryInterval** ayarını yapın.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Hata iletisi: "ADO NET kaynağı '... ' bağlantısını alamadı "SQL Server bağlantı kurulurken ağla ilgili veya örneğe özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir durumda değil. "

Bu sorun genellikle veri kaynağına veya hedefe, SSIS tümleştirme çalışma zamanından erişilemediği anlamına gelir. Nedenler farklılık gösterebilir. Şu eylemleri deneyin:
* Veri kaynağını veya hedef adını/IP 'yi doğru geçirdiğinizden emin olun.
* Güvenlik duvarının doğru ayarlandığından emin olun.
* Veri kaynağınız veya hedefi şirket içi ise sanal ağınızın düzgün şekilde yapılandırıldığından emin olun:
  * Aynı sanal ağ içinde bir Azure VM sağlayarak sorunun sanal ağ yapılandırmasından olup olmadığını doğrulayabilirsiniz. Ardından, veri kaynağına veya hedefine Azure VM 'den erişip erişemeyeceğini kontrol edin.
  * Bir [Azure-SSIS tümleştirme çalışma zamanına bir sanal ağa katılarak](join-azure-ssis-integration-runtime-virtual-network.md)bir SSIS tümleştirme çalışma zamanı ile sanal ağ kullanma hakkında daha fazla bilgi edinebilirsiniz.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Hata iletisi: "ADO NET kaynağı '... ' bağlantısını alamadı "yönetilen bağlantı Yöneticisi oluşturulamadı."

Olası nedeni, pakette kullanılan ADO.NET sağlayıcısının SSIS tümleştirme çalışma zamanına yüklenmesidir. Sağlayıcıyı özel bir kurulum kullanarak yükleyebilirsiniz. [Azure-SSIS tümleştirme çalışma zamanı için kurulumu Özelleştir](how-to-configure-azure-ssis-ir-custom-setup.md)bölümünde özel kurulumla ilgili daha fazla ayrıntı bulabilirsiniz.

### <a name="error-message-the-connection--is-not-found"></a>Hata iletisi: "Bağlantı '... ' bulunamadı "

Daha eski SQL Server Management Studio (SSMS) sürümlerindeki bilinen bir sorun bu hataya neden olabilir. Paket, dağıtımı yapmak için SQL Server Management Studio’nun kullanıldığı makinede yüklü olmayan özel bir bileşen (örneğin, SQL Server Integration Services Azure Özellik Paketi veya iş ortağı bileşenleri) içeriyorsa SQL Server Management Studio, bileşeni kaldırır ve hataya neden olur. [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) 'yi sorunu düzeltilen en son sürüme yükseltin.

### <a name="error-messagessis-executor-exit-code--1073741819"></a>Hata iletisi: "SSIS yürütücü çıkış kodu:-1073741819."

* Olası neden & önerilen eylem:
  * Birden çok iş parçacığında paralel olarak birden fazla Excel kaynağı veya hedefi çalıştırıldığında, bu hata Excel kaynağı ve hedefi kısıtlamasından dolayı olabilir. Excel bileşenlerinizi sırayla yürütmek üzere değiştirerek veya bunları farklı paketlere ayırarak ve ExecuteOutOfProcess özelliği true olarak ayarlanmış şekilde "paket görevini Yürüt" aracılığıyla tetikleyerek bu sınırlamaya geçici çözüm verebilirsiniz.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Hata iletisi: "Diskte yeterli alan yok"

Bu hata, yerel diskin SSIS tümleştirme çalışma zamanı düğümünde kullanıldığı anlamına gelir. Paketinizin veya özel kurulumun çok miktarda disk alanı kullanıp kullanmadığını denetleyin:
* Disk paketiniz tarafından tüketilmişse, paket yürütme tamamlandıktan sonra serbest bırakılır.
* Disk özel kurulumlarınız tarafından tüketilmişse, SSIS tümleştirme çalışma zamanını durdurmanız, komut dosyanızı değiştirmeniz ve tümleştirme çalışma zamanını yeniden başlatmanız gerekir. Özel kurulum için belirttiğiniz tüm Azure Blob kapsayıcısı SSIS tümleştirme çalışma zamanı düğümüne kopyalanır, bu nedenle o kapsayıcı altında gereksiz içerik olup olmadığını kontrol edin.

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>Hata iletisi: "Ana sunucudan kaynak alınamadı. Microsoft. SqlServer. IntegrationServices. Scale. ScaleoutContract. Common. MasterResponseFailedException: Kod: 300004. Açıklama: "* * *" dosyasını yükleme başarısız. "

* Olası neden & önerilen eylem:
  * SSIS etkinliği dosya sisteminden (paket dosyası veya proje dosyası) paketi yürütüp, bu hata oluşur çünkü proje, paket veya yapılandırma dosyasına SSIS etkinliğinde verdiğiniz paket erişim kimlik bilgileri ile erişilebilir değilse
    * Azure dosyası kullanıyorsanız:
      * Dosya \\yolu, \<depolama hesabı adıyla \\\>başlamalıdır.\\File.Core.Windows.net\<dosya paylaşma yolu\>
      * Etki alanı "Azure" olmalıdır
      * Kullanıcı adının depolama hesabı \<adı olması gerekir\>
      * Parola, depolama erişim \<anahtarı olmalıdır\>
    * Şirket içi dosya kullanıyorsanız, Azure-SSIS tümleştirme çalışma zamanının şirket içi dosya paylaşımınıza erişebilmesi için lütfen VNet, paket erişimi kimlik bilgileri ve izninin doğru yapılandırılıp yapılandırılmadığını denetleyin.

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>Hata iletisi: "Dosya adı '... ' bağlantıda belirtilen geçerli değildi "

* Olası neden & önerilen eylem:
  * Geçersiz bir dosya adı belirtildi
  * Bağlantı yöneticinizin kısa süre yerine FQDN 'yi (tam etki alanı adı) kullandığınızdan emin olun

### <a name="error-message-cannot-open-file-"></a>Hata iletisi: "Dosya açılamıyor... '"

Bu hata, paket yürütmesi SSIS tümleştirme çalışma zamanı 'nda yerel diskte bir dosya bulamadığında oluşur. Şu eylemleri deneyin:
* SSIS tümleştirme çalışma zamanı 'nda yürütülen pakette mutlak yolu kullanmayın. Geçerli yürütme çalışma dizinini (.) veya Temp klasörünü (% TEMP%) kullanın yerine.
* Bazı dosyaları SSIS tümleştirme çalışma zamanı düğümlerinde kalıcı hale getirmeniz gerekiyorsa, dosyaları [kurulumu Özelleştir](how-to-configure-azure-ssis-ir-custom-setup.md)bölümünde açıklandığı gibi hazırlayın. Çalışma dizinindeki tüm dosyalar yürütme tamamlandıktan sonra temizlenir.
* Dosyayı SSIS tümleştirme çalışma zamanı düğümünde depolamak yerine Azure dosyaları 'nı kullanın. Ayrıntılar için bkz. [Azure dosya paylaşımlarını kullanma](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Hata iletisi: "' SSSıSDB ' veritabanı boyut kotasına ulaştı"

Bunun olası bir nedeni, Azure SQL veritabanında oluşturulan SSISDB veritabanının veya siz SSIS tümleştirme çalışma zamanını oluştururken yönetilen bir örneğin kotasına ulaşması olabilir. Şu eylemleri deneyin:
* Veritabanınızın DTU değerini artırın. [Azure SQL Veritabanı sunucusu için SQL Veritabanı kaynağı sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) bölümünde ayrıntıları bulabilirsiniz.
* Paketinizin çok sayıda günlük oluşturup oluşturmayacağını denetleyin. Bu durumda, bu günlükleri temizlemek için elastik bir iş yapılandırabilirsiniz. Ayrıntılar için bkz. [Azure Elastik Veritabanı işleri ile SSISDB günlüklerini temizleme](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Hata iletisi: "Veritabanı için istek sınırı... ve bu sınıra ulaşıldı. "

SSIS tümleştirme çalışma zamanı 'nda çok sayıda paket paralel çalışıyorsa, SSıSDB 'nin istek sınırına ulaştığından bu hata ortaya çıkabilir. Bu sorunu çözmek için SSıSDB DTC 'YI artırmayı düşünün. [Azure SQL Veritabanı sunucusu için SQL Veritabanı kaynağı sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) bölümünde ayrıntıları bulabilirsiniz.

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Hata iletisi: "SSIS Işlemi beklenmeyen işlem durumuyla başarısız oldu:..."

Hata genellikle geçici bir sorundan kaynaklanır, bu nedenle paket yürütmeyi yeniden çalıştırmayı deneyin. Aşağıdaki adımları izleyerek, SSIS paketine yeniden deneme modelini uygulayın:

* SSIS paketlerinizin, yan etkileri olmadan hata durumunda yeniden çalıştığından emin olun (örneğin, veri kaybı veya veri çoğaltma).
* **Genel** SEKMESINDE, **SSIS paketi yürütme** etkinliğinin **yeniden denenme** ve **yeniden deneme aralığını** yapılandırın. ![Genel sekmesindeki özellikleri ayarla](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* Bir ADO.NET ve OLE DB kaynak veya hedef bileşeni için SSIS paketi veya SSIS etkinliğinde bağlantı Yöneticisi 'nde **ConnectRetryCount** ve **ConnectRetryInterval** ayarını yapın.

### <a name="error-message-there-is-no-active-worker"></a>Hata iletisi: "Etkin çalışan yok."

Bu hata genellikle SSIS tümleştirme çalışma zamanının sağlıksız bir duruma sahip olduğu anlamına gelir. Durum ve ayrıntılı hatalar için Azure portal denetleyin. Daha fazla bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanı](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime).

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Hata iletisi: "Özel kurulum için verdiğiniz Azure Blob kapsayıcısına erişemedik, tümleştirme çalışma zamanı yükseltilemiyor ve sonunda çalışmayı durduracak."

Bu hata, SSIS tümleştirme çalışma zamanı özel kurulum için yapılandırılmış depolamaya erişemediğinde oluşur. Belirttiğiniz paylaşılan erişim imzası (SAS) URI 'sinin geçerli olup olmadığını ve süresinin dolmadığını denetleyin.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Hata iletisi: Analysis Services için Microsoft OLE DB sağlayıcısı. HRESULT 0x80004005 açıklaması: ' COM hatası: COM hatası: mscorlib; Bir çağrının hedefi tarafından özel durum oluşturuldu "

Olası bir neden, Azure Multi-Factor Authentication özellikli Kullanıcı adının veya parolanın Azure Analysis Services kimlik doğrulaması için yapılandırılmış olması olabilir. Bu kimlik doğrulaması, SSIS tümleştirme çalışma zamanı 'nda desteklenmez. Azure Analysis Services kimlik doğrulaması için bir hizmet sorumlusu kullanmayı deneyin:
1. Hizmet sorumlusu [Ile Otomasyon](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal)bölümünde açıklandığı gibi bir hizmet sorumlusu hazırlayın.
2. Bağlantı Yöneticisi 'nde **belirli bir Kullanıcı adı ve parola kullan**: **AppID** 'yi parola olarak Kullanıcı adı ve **ClientSecret** olarak ayarlayın.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Hata iletisi: "ADONET kaynağı {GUID} bağlantısını şu hata iletisiyle alamadı: Yönetilen bir kimlik kullanılırken ' NT AUTHORıTY\ANONYMOUS LOGON ' ' kullanıcısı için oturum açma başarısız oldu

Bağlantı Yöneticisi 'nin kimlik doğrulama yöntemini, *Connectusingmanagedıdentity* parametresi **true**olduğunda **Active Directory parola kimlik doğrulaması** olarak yapılandırmadığınızdan emin olun. Bunun yerine **SQL kimlik doğrulaması** olarak yapılandırabilirsiniz, bu, *Connectusingmanagedıdentity* ayarlandıysa yok sayılır.

### <a name="multiple-package-executions-are-triggered-unexpectedly"></a>Birden çok paket yürütmesi beklenmedik şekilde tetikleniyor

* Olası neden & önerilen eylem:
  * ADF saklı yordam etkinliği, SSIS paketi yürütmesini tetiklemek için kullanılır. T-SQL komutu geçici bir sorunla karşılaşabilir ve çoklu paket yürütmelerinin oluşmasına neden olacak şekilde yeniden çalıştırma tetikleyebilir.
  * Kullanıcı yeniden deneme sayısını etkinlikte bir şekilde ayarlamadığınız takdirde paket yürütmenin yeniden çalıştırılmamasını sağlayan Executessıspackage etkinliğini kullanın. Ayrıntı şurada bulunabilir:[https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)

### <a name="package-execution-takes-too-long"></a>Paket yürütmesi çok uzun sürüyor

Olası nedenler ve önerilen eylemler şunlardır:

* SSIS tümleştirme çalışma zamanı üzerinde çok fazla paket yürütmesi zamanlandı. Tüm bu yürütmeler, kendi kapamaları için bir kuyrukta beklemeye alınır.
  * Bu formülü kullanarak en yüksek değeri belirle:

    IR başına en fazla paralel yürütme sayısı = düğüm sayısı * düğüm başına en fazla paralel yürütme
  * Düğüm sayısı ve düğüm başına en fazla paralel yürütme ayarlama hakkında bilgi edinmek için bkz. [Azure Data Factory bir Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md).
* SSIS tümleştirme çalışma zamanı durdurulur veya sağlıksız durumda olur. SSIS tümleştirme çalışma zamanı durumunu ve hatalarını denetleme hakkında bilgi edinmek için bkz. [Azure-SSIS tümleştirme çalışma zamanı](monitor-integration-runtime.md#azure-ssis-integration-runtime).

Ayrıca, **genel** sekmesinde bir zaman aşımı ayarlamanızı öneririz: ![Genel sekmesinde](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)özellikleri ayarlayın.

### <a name="poor-performance-in-package-execution"></a>Paket yürütmede kötü performans

Şu eylemleri deneyin:

* SSIS tümleştirme çalışma zamanının veri kaynağı ve hedefle aynı bölgede olduğundan emin olun.

* Yürütme sırasında her bir bileşen için Duration bilgilerini toplamak üzere paket yürütmenin günlüğe kaydetme düzeyini **performans** olarak ayarlayın. Ayrıntılar için bkz. [Integration Services (SSIS) günlüğü](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging).

* Azure portal IR düğüm performansını kontrol edin:
  * SSIS tümleştirme çalışma zamanının nasıl izleneceği hakkında bilgi için bkz. [Azure-SSIS tümleştirme çalışma zamanı](monitor-integration-runtime.md#azure-ssis-integration-runtime).
  * Azure portal veri fabrikasının ölçümlerini görüntüleyerek SSIS tümleştirme çalışma zamanı için CPU/bellek geçmişini bulabilirsiniz.
    ![SSIS tümleştirme çalışma zamanının ölçümlerini izleyin](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
