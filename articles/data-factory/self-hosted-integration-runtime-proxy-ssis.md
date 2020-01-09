---
title: Kendi kendine barındırılan tümleştirme çalışma zamanını SSIS için proxy olarak yapılandırma
description: Şirket içinde barındırılan Integration Runtime Azure-SSIS Integration Runtime için proxy olarak yapılandırmayı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 12/23/2019
ms.openlocfilehash: 8754b2ef9727b5526cc9bcf756085f2f05f6a398
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552109"
---
# <a name="configure-self-hosted-ir-as-a-proxy-for-azure-ssis-ir-in-adf"></a>ADF 'de Azure-SSIS IR için şirket içinde barındırılan IR 'yi bir ara sunucu olarak yapılandırma

Bu makalede, proxy olarak yapılandırılan şirket içinde barındırılan IR ile Azure Data Factory (ADF) üzerinde Azure-SSIS Integration Runtime (IR) için SQL Server Integration Services (SSIS) paketlerinin nasıl çalıştırılacağı açıklanmaktadır.  Bu özellik, [Azure-SSIS IR bir sanal ağa katılmadan](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)Şirket içindeki verilere erişmenizi sağlar.  Şirket ağınızda Azure-SSIS IR eklemek için aşırı karmaşık bir yapılandırma/kısıtlayıcı ilke olduğunda faydalıdır.

Bu özellik, şirket içi veri kaynağı ile bir veri akışı görevi içeren paketinizi iki hazırlama görevine bölecektir: şirket içinde barındırılan IR üzerinde çalışan ilk, verileri şirket içi veri kaynağından Azure Blob depolama alanındaki bir hazırlama alanına taşıyacaktır, ancak Azure-SSIS IR ikinci bir işlem, hazırlama alanındaki verileri amaçlanan veri hedefine taşır.

Bu özellik ayrıca, Azure-SSIS IR tarafından henüz desteklenmeyen bölgelerde şirket içinde barındırılan IR sağlamanıza olanak sağladığından diğer avantajlar/özellikler de sağlar, veri kaynaklarınızın güvenlik duvarında şirket içinde barındırılan IR 'nin genel statik IP adresine izin verir, vb.

## <a name="prepare-self-hosted-ir"></a>Şirket içinde barındırılan IR hazırlama

Bu özelliği kullanmak için önce bir ADF oluşturmanız Azure-SSIS IR ve bu işlemi henüz yapmadıysanız, [bir Azure-SSIS IR nasıl](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) sağlayacağınızı takip etmeniz gerekir.

Daha sonra, şirket içinde barındırılan IR 'nin [nasıl](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime) oluşturulacağı ile Azure-SSIS IR aynı ADF altında otomatik olarak barındırılan IR sağlamanız gerekir.

Son olarak, şirket içi makinenize/Azure sanal makinenize (VM) aşağıdaki gibi otomatik olarak barındırılan IR 'nin ve ek sürücü ve çalışma zamanının en son sürümünü indirip yüklemeniz gerekir:
- Şirket içinde barındırılan IR 'nin en son sürümünü [buradan](https://www.microsoft.com/download/details.aspx?id=39717)indirin ve yükleyin.
- Paketlerinizdeki OLEDB bağlayıcıları kullanıyorsanız, zaten yapmadıysanız, ilgili OLEDB sürücülerini şirket içinde barındırılan IR 'nin yüklendiği makinede indirin ve yükleyin.  SQL Server (SQLNCLI) için OLEDB sürücüsünün önceki sürümünü kullanıyorsanız, 64 bitlik sürümü [buradan](https://www.microsoft.com/download/details.aspx?id=50402)indirebilirsiniz.  SQL Server (MSOLEDBSQL) için OLEDB sürücüsünün en son sürümünü kullanıyorsanız, 64 bitlik sürümü [buradan](https://www.microsoft.com/download/details.aspx?id=56730)indirebilirsiniz.  PostgreSQL, MySQL, Oracle vb. gibi diğer veritabanı sistemleri için OLEDB sürücüleri kullanıyorsanız, 64 bitlik sürümü ilgili web sitelerinden indirebilirsiniz.
- Daha önce yapmadıysanız, C++ Visual (VC) çalışma zamanını şirket IÇINDE barındırılan IR 'nin yüklendiği makinede indirin ve yükleyin.  64 bitlik sürümü [buradan](https://www.microsoft.com/download/details.aspx?id=40784)indirebilirsiniz.

## <a name="prepare-azure-blob-storage-linked-service-for-staging"></a>Azure Blob depolama bağlı hizmetini hazırlama için hazırlama

Zaten [BIR ADF bağlantılı hizmet oluşturma](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service) makalesini tamamladıktan sonra, Azure-SSIS IR SAĞLANDıĞı aynı ADF altında Azure Blob depolama bağlı hizmeti oluşturun.  Lütfen şunlardan emin olun:
- **Veri deposu** Için **Azure Blob depolama** seçildi
- **Tümleştirme çalışma zamanı aracılığıyla Bağlan** Için **oto resolveıntegrationruntime** seçildi
- **Kimlik doğrulama yöntemi** için **hesap anahtarı**/**SAS URI**/**hizmet sorumlusu** seçili olmalıdır

>[!TIP]
> **Hizmet sorumlusu** seçildiğinde, en az **Depolama Blobu veri katılımcısı rolüne**izin verin. Daha fazla bilgi için [Azure Blob depolama Bağlayıcısı](connector-azure-blob-storage.md#linked-service-properties)' na bakın.

![Azure Blob depolama bağlı hizmetini hazırlama için hazırlama](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-azure-ssis-ir-with-self-hosted-ir-as-a-proxy"></a>Ara sunucu olarak şirket içinde barındırılan IR ile Azure-SSIS IR yapılandırma

Şirket içinde barındırılan IR ve Azure Blob depolama bağlı hizmetinizi hazırlama için hazırladığınıza göre, yeni/mevcut Azure-SSIS IR, kendiliğinden konak IR ile, ADF portalında/uygulamada bir proxy olarak yapılandırabilirsiniz.  Mevcut Azure-SSIS IR çalışıyorsa, bunu yapmadan önce durdurun ve ardından yeniden başlatın.

1. Tümleştirme çalışma zamanı kurulum panelinde, **İleri** düğmesini seçerek **Genel ayarlar** ve **SQL ayarları** bölümlerinde ilerleyin. 

1. **Gelişmiş ayarlar** bölümünde:

   1. Azure-SSIS Integration Runtime onay kutusu **Için şirket Içinde barındırılan Integration Runtime ara sunucu olarak ayarla** ' yı seçin. 

   1. **Şirket Içinde barındırılan Integration Runtime**, Azure-SSIS IR için mevcut şirket IÇINDE barındırılan IR 'yi bir ara sunucu olarak seçin.

   1. **Hazırlama depolama bağlı hizmeti**Için mevcut Azure Blob depolama bağlı hizmetinizi seçin veya hazırlama için yeni bir tane oluşturun.

   1. **Hazırlama yolu**için, seçili Azure Blob depolama hesabınızda bir blob kapsayıcısı belirtin veya hazırlama için varsayılan bir tane kullanmak üzere boş bırakın.

   1. **Devam**'ı seçin.

   ![Şirket içinde barındırılan IR ile gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>SSIS paketlerini proxy ile bağlanacak şekilde etkinleştir

[Buradan indirilebilen ve buradan](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) indirilebilecek tek başına bir yükleyici olarak, Visual STUDIO için SSIS projeleri Uzantısı ile en son SSDT 'yi [kullanarak, OLEDB](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)/düz dosya bağlantı yöneticilerine eklenen yeni bir **connectbyproxy** özelliği bulabilirsiniz.  

Şirket içindeki veritabanlarına/dosyalara erişmek için OLEDB/düz dosya kaynaklarıyla veri akışı görevleri içeren yeni paketler tasarlarken, ilgili bağlantı yöneticilerinin Özellikler panelinde onu **true** olarak ayarlayarak bu özelliği etkinleştirebilirsiniz.

![ConnectByProxy özelliğini etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

Bu özelliği, var olan paketleri bir tane ile tek tek değiştirmek zorunda kalmadan çalıştırırken de etkinleştirebilirsiniz.  İki seçenek vardır:
- Azure-SSIS IR için en son SSDT ile bu paketleri içeren projeyi açma, yeniden oluşturma ve yeniden dağıtma: Bu özellik daha sonra SSMS 'den paket çalıştırırken paket açılır penceresini Çalıştır ' ın **bağlantı yöneticileri** sekmesinde görünen ilgili bağlantı yöneticileri için **true** olarak ayarlanarak etkinleştirilebilir.

  ![ConnectByProxy Property2 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  Özelliği, ADF işlem hatlarında paket çalıştırırken [SSIS paketi yürütme etkinliğinin](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **bağlantı yöneticileri** sekmesinde görünen Ilgili bağlantı yöneticileri için **true** olarak ayarlanarak da etkinleştirilebilir.
  
  ![ConnectByProxy property3 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- SSIS IR 'niz üzerinde çalıştırılacak olan bu paketleri içeren projeyi yeniden dağıtma: özellik, özellik yolu ve `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`sağlayarak etkinleştirilebilir ve paketleri SSMS 'den çalıştırırken paket açılır penceresini Çalıştır **Gelişmiş** sekmesinde bir özellik geçersiz kılma olarak **true** olarak ayarlanarak etkinleştirilebilir.

  ![ConnectByProxy property4 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  Özelliği, Ayrıca, ADF işlem hatlarında paket çalıştırırken [SSIS paketi yürütme etkinliğinin](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **geçersiz kılınmasına** izin vererek özellik yolu, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`ve **true** olarak ayarlanarak etkinleştirilebilir.
  
  ![ConnectByProxy Property5 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>İlk ve ikinci hazırlama görevlerinde hata ayıkla

Şirket içinde barındırılan IR 'de, çalışma zamanı günlüklerini `C:\ProgramData\SSISTelemetry` klasöre ve `C:\ProgramData\SSISTelemetry\ExecutionLog` klasöründeki ilk hazırlama görevlerinin yürütme günlüklerine bulabilirsiniz.  Paketleri SSıSDB veya dosya sistemi/dosya paylaşımları/Azure dosyalarında sakladığınıza bağlı olarak, ikinci hazırlama görevlerinin yürütme günlükleri SSıSDB veya belirtilen günlüğe kaydetme yollarında bulunabilir.  İlk hazırlama görevlerinin benzersiz kimlikleri, ikinci hazırlama görevlerinin yürütme günlüklerinde de bulunabilir, örn. 

![İlk hazırlama görevinin benzersiz KIMLIĞI](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="using-windows-authentication-in-staging-tasks"></a>Hazırlama görevlerinde Windows kimlik doğrulamasını kullanma

Şirket içinde barındırılan IR üzerinde hazırlama görevleriniz Windows kimlik doğrulaması gerektiriyorsa, [SSIS paketlerinizi aynı Windows kimlik doğrulamasını kullanacak şekilde yapılandırmanız](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15)gerekir. Hazırlama görevleriniz, şirket içinde barındırılan IR Hizmeti hesabıyla (varsayılan olarak`NT SERVICE\DIAHostService`) çağrılacaktır ve veri depolarınız Windows kimlik doğrulama hesabıyla birlikte erişilecektir. Her iki hesap de belirli güvenlik ilkelerinin atanmasını gerektirir. Sonuç olarak, şirket içinde barındırılan IR makinesinde, `Local Security Policy` -> `Local Policies` -> `User Rights Assignment` açın ve aşağıdaki adımları izleyin.
- **Bir işlem için bellek kotalarını ayarlama** ve **bir işlem düzeyi belirteç** ILKELERINI şirket içinde barındırılan IR Hizmeti hesabıyla değiştirme. Bu, varsayılan hizmet hesabıyla şirket içinde barındırılan IR 'yi yüklerken otomatik olarak yapılmalıdır. Farklı bir hizmet hesabı kullanıyorsanız, buna aynı ilkeleri atayın.
- Windows kimlik doğrulama hesabına **hizmet olarak oturum** açma ilkesi atayın.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>İlk ve ikinci hazırlama görevleri için faturalandırma

Şirket içinde barındırılan IR üzerinde çalışan ilk hazırlama görevleri, [ADF veri ardışık düzen fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) makalesinde belirtilen şekilde faturalandırılır ve şirket IÇINDE barındırılan IR üzerinde çalışan tüm veri taşıma etkinlikleri aynı şekilde faturalandırılır.

Azure-SSIS IR çalışan ikinci hazırlama görevleri ayrı olarak faturalandırılmaz, ancak çalıştırdığınız Azure-SSIS IR [Azure-SSIS IR fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/ssis/) makalesinde belirtilen şekilde faturalandırılır.

## <a name="current-limitations"></a>Geçerli sınırlamalar

- Yalnızca ODBC/OLEDB/düz dosya bağlantı yöneticileri ve ODBC/OLEDB/düz dosya kaynakları olan veri akışı görevleri şu anda desteklenmektedir. 
- Yalnızca **hesap anahtarı**/**SAS URI**/**hizmet sorumlusu** kimlik doğrulaması ile yapılandırılmış Azure Blob depolama bağlı hizmetleri şu anda destekleniyor.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içinde barındırılan IR 'yi, Azure-SSIS IR için bir ara sunucu olarak yapılandırdıktan sonra, ADF işlem hatlarında SSIS paket etkinliklerini yürütme olarak [SSIS paketlerini](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)yürütme gibi paketleri dağıtabilir