---
title: SSIS tümleştirme çalışma zamanında paket yürütme sorun giderme
description: Bu makale, SSIS tümleştirme çalışma zamanında SSIS paket yürütmesi için sorun giderme kılavuzu sağlar
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: wenjiefu
author: wenjiefu
ms.reviewer: sawinark
manager: shwang
ms.custom: seo-lt-2019
ms.date: 04/15/2019
ms.openlocfilehash: 1c2db107302e4851641ef430db61ec9b29ee151f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77187474"
---
# <a name="troubleshoot-package-execution-in-the-ssis-integration-runtime"></a>SSIS tümleştirme çalışma zamanında paket yürütme sorun giderme

Bu makalede, SSIS tümleştirme çalışma zamanında SQL Server Integration Services (SSIS) paketlerini yürülürken bulabileceğiniz en yaygın hatalar yer almaktadır. Hataları çözmek için olası nedenleri ve eylemleri açıklar.

## <a name="where-to-find-logs-for-troubleshooting"></a>Sorun giderme günlüklerini nerede bulabilirsiniz?

SSIS paket yürütme etkinliğinin çıktısını denetlemek için Azure Veri Fabrikası portalını kullanın. Çıktı yürütme sonucu, hata iletileri ve işlem kimliği içerir. Ayrıntılar için [bkz.](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)

Yürütme için ayrıntı günlüklerini denetlemek için SSIS kataloğunu (SSISDB) kullanın. Ayrıntılar için [Bkz. İzleme Çalışma Paketleri ve Diğer İşlemler.](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)

## <a name="common-errors-causes-and-solutions"></a>Yaygın hatalar, nedenleri ve çözümleri

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Hata iletisi: "Bağlantı Süresi Doldu" veya "Hizmet isteğinizi işleyen bir hatayla karşılaştı. Lütfen yeniden deneyin."

Olası nedenler ve önerilen eylemler şunlardır:
* Veri kaynağı veya hedef aşırı yüklenir. Veri kaynağınızdaki veya hedefinizdeki yükü kontrol edin ve yeterli kapasiteye sahip olup olmadığını görün. Örneğin, Azure SQL Veritabanı'nı kullandıysanız, veritabanının zaman alabına göre ölçekleme yapmayı düşünün.
* SSIS tümleştirme çalışma zamanı ile veri kaynağı veya hedef arasındaki ağ, özellikle bağlantı bölgeler arası veya şirket içi ve Azure arasında olduğunda kararsızdır. Aşağıdaki adımları izleyerek SSIS paketinde yeniden deneme deseni uygulayın:
  * SSIS paketlerinizin yan etkileri olmadan (örneğin, veri kaybı veya veri çoğaltma) başarısızlığa bağlı olarak tekrarlayabilmesini unutmayın.
  * **Genel** sekmesinde **SSIS Paketini Yürüt etkinliği** yeniden **deneme** ![ve yeniden deneme **aralığını** yapılandırın. Genel sekmedeki özellikleri ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
  * bir ADO.NET ve OLE DB kaynak veya hedef bileşeni için, SSIS paketinde veya SSIS etkinliğinde **ConnectRetryCount** ve **ConnectRetryInterval'ı** Bağlantı Yöneticisi'nde ayarlayın.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Hata iletisi: "ADO NET Kaynak '...'" bağlantısını elde edemedi. "SQL Server'a bağlantı kurarken ağla ilgili veya örne özgü bir hata oluştu. Sunucu bulunamadı veya erişilebilir değildi.”

Bu sorun genellikle veri kaynağına veya hedefe, SSIS tümleştirme çalışma zamanından erişilemediği anlamına gelir. Nedenler farklılık gösterebilir. Şu eylemleri deneyin:
* Veri kaynağını veya hedef adını/IP'yi doğru bir şekilde geçtiğinden emin olun.
* Güvenlik duvarının düzgün ayarlandıkdığından emin olun.
* Veri kaynağınız veya hedefiniz şirket içindeyse sanal ağınızın düzgün şekilde yapılandırıldığından emin olun:
  * Aynı sanal ağda bir Azure VM sağlayarak sorunun sanal ağ yapılandırmasından kaynaklanıp kaynaklmadığını doğrulayabilirsiniz. Ardından, veri kaynağına veya hedefe Azure VM'den erişilip erişilemeyeceğini kontrol edin.
  * Sanal ağa [Katılma Azure-SSIS tümleştirme çalışma zamanında](join-azure-ssis-integration-runtime-virtual-network.md)SSIS tümleştirme çalışma süresine sahip sanal ağ kullanma hakkında daha fazla ayrıntı bulabilirsiniz.

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-could-not-create-a-managed-connection-manager"></a>Hata iletisi: "ADO NET Kaynak '...'" bağlantısını elde edemedi. "Yönetilen bir bağlantı yöneticisi oluşturamadı."

Olası nedeni, pakette kullanılan ADO.NET sağlayıcısının SSIS tümleştirme çalışma zamanında yüklü olmamasıdır. Özel bir kurulum kullanarak sağlayıcı yükleyebilirsiniz. [Azure-SSIS tümleştirme çalışma zamanı için](how-to-configure-azure-ssis-ir-custom-setup.md)özel kurulum hakkında daha fazla ayrıntı bulabilirsiniz.

### <a name="error-message-the-connection--is-not-found"></a>Hata iletisi: "Bağlantı '...' bulunamadı"

Daha eski SQL Server Management Studio (SSMS) sürümlerindeki bilinen bir sorun bu hataya neden olabilir. Paket, dağıtımı yapmak için SQL Server Management Studio’nun kullanıldığı makinede yüklü olmayan özel bir bileşen (örneğin, SQL Server Integration Services Azure Özellik Paketi veya iş ortağı bileşenleri) içeriyorsa SQL Server Management Studio, bileşeni kaldırır ve hataya neden olur. [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)'yi sorunun düzeltildiği en son sürüme yükseltin.

### <a name="error-messagessis-executor-exit-code--1073741819"></a>Hata iletisi:"SSIS Yürütme kodu: -1073741819."

* Olası neden ve önerilen eylem:
  * Bu hata, birden çok Excel kaynağı veya hedefi çoklu iş parçacığı paralel olarak yürütülmektedir Excel kaynak ve hedef için sınırlama nedeniyle olabilir. Bu sınırlamayı sırayla yürütmek üzere Excel bileşenlerinizi değiştirerek veya bunları farklı paketlere ayırarak ve True olarak ayarlanmış ExecuteOutOfProcess özelliğiyle "Paket Görevini Yürüt" ile tetikleyerek geçici olarak çözümleyebilirsiniz.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Hata iletisi: "Diskte yeterli alan yok"

Bu hata, yerel diskin SSIS tümleştirme çalışma zamanı düğümünde kullanıldığı anlamına gelir. Paketinizin veya özel kurulumunuzun çok fazla disk alanı tüketip tüketmediğini kontrol edin:
* Disk paketiniz tarafından tüketiliyorsa, paket yürütme sınadı bittikten sonra serbest bırakılır.
* Disk özel kurulumunuz tarafından tüketiliyorsa, SSIS tümleştirme çalışma süresini durdurmanız, komut dosyanızı değiştirmeniz ve tümleştirme çalışma saatini yeniden başlatmanız gerekir. Özel kurulum için belirttiğiniz tüm Azure blob kapsayıcısı SSIS tümleştirme çalışma zamanı düğümüne kopyalanır, bu nedenle bu kapsayıcının altında gereksiz içerik olup olmadığını kontrol edin.

### <a name="error-message-failed-to-retrieve-resource-from-master-microsoftsqlserverintegrationservicesscalescaleoutcontractcommonmasterresponsefailedexception-code300004-descriptionload-file--failed"></a>Hata iletisi: "Ana kaynak almak için başarısız oldu. Microsoft.SqlServer.IntegrationServices.Scale.ScaleoutContract.Common.MasterResponseFailedException: Kod:300004. Açıklama:Yükleme dosyası "***" başarısız oldu."

* Olası neden ve önerilen eylem:
  * SSIS Etkinliği dosya sisteminden (paket dosyası veya proje dosyası) paket yürütüyorsa, proje, paket veya yapılandırma dosyasına SSIS Etkinliği'nde sağladığınız paket erişim kimlik bilgileriyle erişilemiyorsa bu hata oluşur
    * Azure Dosyası kullanıyorsanız:
      * Dosya yolu depolama \\ \\ \<hesabı adı\>.file.core.windows.net\\\<dosya paylaşım yolu ile başlamalıdır\>
      * Etki alanı "Azure" olmalıdır
      * Kullanıcı adı depolama \<hesabı adı olmalıdır\>
      * Parola depolama \<erişim anahtarı olmalıdır\>
    * Şirket içi dosyanız kullanıyorsanız, Azure-SSIS tümleştirme çalışma sürenizin şirket içi dosya paylaşımınıza erişebilmeleri için lütfen VNet, paket erişim kimlik bilgisi ve izinlerin doğru şekilde yapılandırılıp yapılmadığını kontrol edin

### <a name="error-message-the-file-name--specified-in-the-connection-was-not-valid"></a>Hata iletisi: "Dosya adı '...' bağlantıda belirtilen geçerli değildi"

* Olası neden ve önerilen eylem:
  * Geçersiz bir dosya adı belirtilir
  * Bağlantı yöneticinizde kısa bir süre yerine FQDN (Tam Nitelikli Alan Adı) kullandığınızdan emin olun

### <a name="error-message-cannot-open-file-"></a>Hata iletisi: "Dosya '...'" açılmıyor

Bu hata, paket yürütme SSIS tümleştirme çalışma zamanında yerel diskte bir dosya bulamıyordu oluşur. Şu eylemleri deneyin:
* SSIS tümleştirme çalışma zamanında yürütülmekte olan paketteki mutlak yolu kullanmayın. Geçerli yürütme çalışma dizinini (.) veya geçici klasörü (%TEMP%) kullanın Yer -ine.
* SSIS tümleştirme çalışma zamanı düğümlerinde bazı dosyaları ertelemeniz gerekiyorsa, [dosyaları Özelleştir kurulumunda](how-to-configure-azure-ssis-ir-custom-setup.md)açıklandığı şekilde hazırlayın. Çalışma dizinindeki tüm dosyalar yürütme bittikten sonra temizlenir.
* Dosyayı SSIS tümleştirme çalışma zamanı düğümünde depolamak yerine Azure Dosyaları'nı kullanın. Ayrıntılar için Bkz. [Azure dosya paylaşımlarını kullan.](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares)

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Hata iletisi: "Veritabanı 'SSISDB' boyut kotasına ulaştı"

Bunun olası bir nedeni, Azure SQL veritabanında oluşturulan SSISDB veritabanının veya siz SSIS tümleştirme çalışma zamanını oluştururken yönetilen bir örneğin kotasına ulaşması olabilir. Şu eylemleri deneyin:
* Veritabanınızın DTU değerini artırın. [Azure SQL Veritabanı sunucusu için SQL Veritabanı kaynağı sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) bölümünde ayrıntıları bulabilirsiniz.
* Paketinizin çok sayıda günlük oluşturup oluşturmayacağını denetleyin. Bu durumda, bu günlükleri temizlemek için elastik bir iş yapılandırabilirsiniz. Ayrıntılar için bkz. [Azure Elastik Veritabanı işleri ile SSISDB günlüklerini temizleme](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md).

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Hata iletisi: "Veritabanı için istek sınırı ... ve ulaşıldı."

SSIS tümleştirme çalışma zamanında birçok paket paralel olarak çalışıyorsa, SSISDB istek sınırına uymuş olduğundan bu hata oluşabilir. Bu sorunu çözmek için SSISDB'nin DTC'sini artırmayı düşünün. [Azure SQL Veritabanı sunucusu için SQL Veritabanı kaynağı sınırları](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server) bölümünde ayrıntıları bulabilirsiniz.

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Hata iletisi: "SSIS İşlemi beklenmeyen işlem durumu yla başarısız oldu: ..."

Hata çoğunlukla geçici bir sorundan kaynaklanır, bu nedenle paket yürütmeyi yeniden çalıştırmayı deneyin. Aşağıdaki adımları izleyerek SSIS paketinde yeniden deneme deseni uygulayın:

* SSIS paketlerinizin yan etkileri olmadan (örneğin, veri kaybı veya veri çoğaltma) başarısızlığa bağlı olarak tekrarlayabilmesini unutmayın.
* **Genel** sekmesinde **SSIS Paketini Yürüt etkinliği** yeniden **deneme** ![ve yeniden deneme **aralığını** yapılandırın. Genel sekmedeki özellikleri ayarlama](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
* bir ADO.NET ve OLE DB kaynak veya hedef bileşeni için, SSIS paketinde veya SSIS etkinliğinde **ConnectRetryCount** ve **ConnectRetryInterval'ı** Bağlantı Yöneticisi'nde ayarlayın.

### <a name="error-message-there-is-no-active-worker"></a>Hata iletisi: "Etkin bir çalışan yok."

Bu hata genellikle SSIS tümleştirme çalışma zamanının sağlıksız bir duruma sahip olduğu anlamına gelir. Durum ve ayrıntılı hatalar için Azure portalına bakın. Daha fazla bilgi için [Azure-SSIS tümleştirme çalışma zamanı'na](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)bakın.

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Hata iletisi: "Özel kurulum için sağladığınız Azure Blob kapsayıcısına erişemediğimiz için tümleştirme çalışma süreniz yükseltilemez ve sonunda çalışmayı durdurur."

Bu hata, SSIS tümleştirme çalışma zamanı özel kurulum için yapılandırılan depolama alanına erişemediğinde oluşur. Sağladığınız paylaşılan erişim imzasının (SAS) URI'sinin geçerli olup olmadığını ve süresinin dolup olmadığını denetleyin.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Hata iletisi: "Microsoft OLE DB Çözümleme Hizmetleri Sağlayıcısı. 'Hresult: 0x80004005 Açıklama:' COM hatası: COM hatası: mscorlib; İstisna bir çağırma hedefi tarafından atılmıştır"

Olası nedenlerden biri, Azure Çok Faktörlü Kimlik Doğrulama etkinleştirilmiş kullanıcı adı veya parolanın Azure Çözümleme Hizmetleri kimlik doğrulaması için yapılandırılmasıdır. Bu kimlik doğrulaması SSIS tümleştirme çalışma zamanında desteklenmez. Azure Çözümleme Hizmetleri kimlik doğrulaması için bir hizmet ilkesi kullanmaya çalışın:

1. Otomasyon'da açıklandığı gibi [servis müdürü hazırlayın.](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal)
2. Connection Manager'da yapılandırma, **belirli bir kullanıcı adı ve parola kullanın:** **AppID'yi** kullanıcı adı olarak ayarlayın ve parola olarak **clientSecret'** ı ayarlayın.

### <a name="error-message-adonet-source-has-failed-to-acquire-the-connection-guid-with-the-following-error-message-login-failed-for-user-nt-authorityanonymous-logon-when-using-a-managed-identity"></a>Hata iletisi: "ADONET Kaynak aşağıdaki hata iletisi ile {GUID} bağlantısını elde etmek için başarısız oldu: Oturum kullanıcı 'NT AUTHORITY\ANONYMOUS LOGON'" için yönetilen bir kimlik kullanırken başarısız oldu

*ConnectUsingManagedIdentity* parametresi **Doğru**olduğunda, Bağlantı Yöneticisi'nin kimlik doğrulama **yöntemini Active Directory Password Authentication** olarak yapılandırmadığınızdan emin olun. *ConnectUsingManagedIdentity* ayarlanırsa yoksayıldığından, bunun yerine **SQL Authentication** olarak yapılandırabilirsiniz.

### <a name="error-message-0xc020801f-at--odata-source--cannot-acquire-a-managed-connection-from-the-run-time-connection-manager"></a>Hata iletisi: "0xC020801F at ..., OData Source [...]: Çalışma zamanı bağlantı yöneticisinden yönetilen bir bağlantı elde edemez"

Olası nedenlerden biri, OData kaynağınızın gerektirdiği SSIS tümleştirme çalışma zamanında Aktarım Katmanı Güvenliği'nin (TLS) etkinleştirilen olmamasıdır. Kurulumu Özelleştir'i kullanarak SSIS tümleştirme çalışma zamanında TLS'yi etkinleştirebilirsiniz. [SSIS'ten Project Online Odata'yı bağlayama](https://docs.microsoft.com/office365/troubleshoot/cant-connect-project-online-odata-from-ssis) ve [Azure-SSIS tümleştirme çalışma zamanı için kurulumu özelleştir'de](how-to-configure-azure-ssis-ir-custom-setup.md)daha fazla ayrıntı bulabilirsiniz.

### <a name="error-message-request-staging-task-with-operation-guid--fail-since-error-failed-to-dispatch-staging-operation-with-error-message-microsoftsqlserverintegrationservicesaisagentcoreaisagentexception-failed-to-load-data-proxy"></a>Hata iletisi: "İşlem kılavuzlu evreleme görevi isteği... hatadan bu yana başarısız: hata iletisi ile evreleme işlemi göndermek için başarısız: Microsoft.SqlServer.IntegrationServices.AisAgentCore.AisAgentException: Veri proxy yüklemek için başarısız oldu."

Azure-SSIS tümleştirme çalışma sürenizin Self-Hosted tümleştirme çalışma süresiyle yapılandırıldığından emin olun. Daha fazla [ayrıntı, ADF'deki Azure-SSIS IR için bir proxy olarak Kendi Kendine Barındırılan IR'yi Yapılandır'da](self-hosted-integration-runtime-proxy-ssis.md)bulunabilir.

### <a name="error-message-staging-task-status-failed-staging-task-error-errorcode-2010-errormessage-the-self-hosted-integration-runtime--is-offline"></a>Hata iletisi: "Evreleme görev durumu: Başarısız oldu. Evreleme görev hatası: ErrorCode: 2010, ErrorMessage: Self-hosted Integration Runtime ... çevrimdışı"

Self-Hosted tümleştirme çalışma sürenizin yüklü olduğundan ve başlatıldıdığından emin olun. Oluştur'da daha fazla ayrıntı bulunabilir [ve kendi kendine barındırılan tümleştirme çalışma süresini yapılandırın](create-self-hosted-integration-runtime.md)

### <a name="error-message-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-error-the-requested-ole-db-provider--is-not-registered-if-the-64-bit-driver-is-not-installed-run-the-package-in-32-bit-mode"></a>Hata iletisi: "Evreleme görev hatası: ErrorCode: 2906, ErrorMessage: Paket yürütme başarısız oldu., Çıktı: {"OperationErrorMessages": "Hata: İstenen OLE DB sağlayıcısı ... kayıtlı değildir. 64 bit sürücü yüklenmezse, paketi 32 bit modunda çalıştırın..."

Paketinizdeki OLE DB konektörleriniz tarafından kullanılan ilgili sağlayıcının Self-Hosted tümleştirme çalışma makinesine düzgün şekilde yüklendiğinden emin olun. Daha fazla [ayrıntı, ADF'de Azure-SSIS IR için bir proxy olarak Kendi Kendine Barındırılan IR'yi Yapılandır'da](self-hosted-integration-runtime-proxy-ssis.md#prepare-the-self-hosted-ir) bulunabilir

### <a name="error-message-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-error-systemiofileloadexception-could-not-load-file-or-assembly-microsoftwindowsazurestorage-version-cultureneutral-publickeytoken31bf3856ad364e35-or-one-of-its-dependencies-the-located-assemblys-manifest-definition-does-not-match-the-assembly-reference"></a>Hata iletisi: "Evreleme görev hatası: ErrorCode: 2906, ErrorMessage: Paket yürütme başarısız oldu., Çıkış: {"OperationErrorMessages": "Hata: System.IO.FileLoadException: Dosya veya derleme 'Microsoft.WindowsAzure.Storage, Sürüm=..., yükleyemedi Kültür=nötr, PublicKeyToken=31bf3856ad364e35' veya bağımlılıklarından biri. Bulunan derlemenin bildirim tanımı derleme başvurusuyla eşleşmiyor.' ..."

Olası nedenlerden biri, Self-Hosted tümleştirme çalışma sürenizin düzgün şekilde yüklenmemesi veya yükseltilmemesidir. En son Kendi kendine barındırılan tümleştirme çalışma süresini indirmenizi ve yeniden yüklemenizi öneririz. Oluştur'da daha fazla ayrıntı bulunabilir [ve kendi kendine barındırılan tümleştirme çalışma süresini yapılandırın](create-self-hosted-integration-runtime.md#installation-best-practices)

### <a name="error-message-a-connection-is-required-when-requesting-metadata-if-you-are-working-offline-uncheck-work-offline-on-the-ssis-menu-to-enable-the-connection"></a>Hata iletisi: "Meta veri istenirken bir bağlantı gereklidir. Çevrimdışı çalışıyorsanız, bağlantıyı etkinleştirmek için SSIS menüsünde Çevrimdışı Çalışma'nın işaretlerini kaldırın"

* Olası neden ve önerilen eylem:
  * Yürütme günlüğünde "Bileşen, ConnectByProxy değeri ayarı doğru olan bağlantı yöneticisini kullanmayı desteklemez" uyarısı da varsa, bu, henüz "ConnectByProxy"yi desteklemeyen bir bileşenüzerinde bağlantı yöneticisinin kullanıldığı anlamına gelir. Desteklenen bileşenler, [ADF'de Azure-SSIS IR için bir proxy olarak Kendi Kendine Barındırılan IR'yi Yapılandırmada](self-hosted-integration-runtime-proxy-ssis.md#enable-ssis-packages-to-connect-by-proxy) bulunabilir
  * Yürütme günlüğü [SSMS raporunda](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017#reports) veya SSIS paket yürütme etkinliğinde belirttiğiniz günlük klasöründe bulunabilir.
  * vNet, alternatif olarak şirket içi verilere erişmek için de kullanılabilir. [Sanal ağa Azure-SSIS tümleştirme çalışma zamanı katılın'da](join-azure-ssis-integration-runtime-virtual-network.md) daha fazla ayrıntı bulunabilir

### <a name="error-message-staging-task-status-failed-staging-task-error-errorcode-2906-errormessage-package-execution-failed-output-operationerrormessages-ssis-executor-exit-code--1n-loglocation-ssistelemetryexecutionlog-effectiveintegrationruntime--executionduration--durationinqueue--integrationruntimequeue--"></a>Hata iletisi: "Evreleme görev durumu: Başarısız oldu. Evreleme görev hatası: ErrorCode: 2906, ErrorMessage: Paket yürütme başarısız oldu., Çıktı: {"OperationErrorMessages": "SSIS Executor çıkış kodu: -1.\n", "LogLocation": "... \\SSISTelemetry\\ExecutionLog\\...", "effectiveIntegrationRuntime": "...", "executionDuration": ..., "durationInQueue": { "integrationRuntimeQueue": ... }}"

Visual C++ çalışma zamanının Self-Hosted tümleştirme çalışma zamanı makinesine yüklendiğinden emin olun. Daha fazla [ayrıntı, ADF'de Azure-SSIS IR için bir proxy olarak Kendi Kendine Barındırılan IR'yi Yapılandır'da](self-hosted-integration-runtime-proxy-ssis.md#prepare-the-self-hosted-ir) bulunabilir

### <a name="multiple-package-executions-are-triggered-unexpectedly"></a>Birden çok Paket yürütmesi beklenmeyen bir şekilde tetiklenir

* Olası neden ve önerilen eylem:
  * SSIS paket yürütmesini tetiklemek için ADF depolanan yordam etkinliği veya Arama etkinliği kullanılır. T-sql komutu geçici bir soruna neden olabilir ve birden çok paket yürütmesi neden olur yeniden tetikleyebilir.
  * Bunun yerine, kullanıcı kümesi yeniden deneme sayısı olmadıkça paket yürütmenin yeniden çalışmamasını sağlayan ExecuteSSISPackage etkinliğini kullanın. Detay bulunabilir[https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)
  * Bir yürütmezaten tetiklenmiş olup olmadığını kontrol ederek yeniden çalıştırmak mümkün edebilmek için t-sql komutunuzu hassaslaştırın

### <a name="package-execution-takes-too-long"></a>Paket yürütme çok uzun sürüyor

Olası nedenler ve önerilen eylemler şunlardır:

* SSIS tümleştirme çalışma zamanında çok fazla paket yürütme zamanlandı. Tüm bu infazlar sıraları için kuyrukta bekliyor olacak.
  * Bu formülü kullanarak maksimumu belirleyin:

    IR başına Maksimum Paralel Yürütme Sayısı = Düğüm Sayısı * Düğüm Başına Maksimum Paralel Yürütme
  * Düğüm sayısını ve düğüm başına maksimum paralel yürütmeyi nasıl ayarlayınız öğrenmek için Azure [Veri Fabrikası'nda Azure-SSIS tümleştirme çalışma süresi oluşturma](create-azure-ssis-integration-runtime.md)bölümüne bakın.
* SSIS tümleştirme çalışma süresi durdurulur veya sağlıksız bir durum vardır. SSIS tümleştirme çalışma zamanı durumunu ve hatalarını nasıl denetleyeceklerini öğrenmek için [Azure-SSIS tümleştirme çalışma zamanı'na](monitor-integration-runtime.md#azure-ssis-integration-runtime)bakın.

**Ayrıca Genel** sekmesinde bir zaman arası ![ayarlamanızı öneririz:](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)Özellikleri Genel sekmesinde ayarlayın.

### <a name="poor-performance-in-package-execution"></a>Paket yürütmede düşük performans

Şu eylemleri deneyin:

* SSIS tümleştirme çalışma zamanının veri kaynağı ve hedefle aynı bölgede olduğundan emin olun.

* Yürütmedeki her bileşen için süre bilgilerini toplamak için paket yürütmesinin günlüğe kaydetme düzeyini **Performans** olarak ayarlayın. Ayrıntılar [için, Entegrasyon Hizmetleri (SSIS) günlüğü'ne](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging)bakın.

* Azure portalında IR düğümü performansını denetleyin:
  * SSIS tümleştirme çalışma zamanı nasıl izlenir hakkında bilgi için [Azure-SSIS tümleştirme çalışma zamanı'na](monitor-integration-runtime.md#azure-ssis-integration-runtime)bakın.
  * Azure portalındaki veri fabrikasının ölçümlerini görüntüleyerek SSIS tümleştirme çalışma zamanı için CPU/bellek geçmişini bulabilirsiniz.
    ![SSIS tümleştirme çalışma zamanının ölçümlerini izleme](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
