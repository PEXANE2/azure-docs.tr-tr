---
title: Şirket içinde barındırılan tümleştirme çalışma zamanını SSSıS için proxy olarak Yapılandırma Azure Data Factory | Microsoft Docs
description: Şirket içinde barındırılan Integration Runtime Azure-SSIS Integration Runtime proxy olarak yapılandırma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/01/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 2f65303cd5cda50a95c3563422c059b985ecf28a
ms.sourcegitcommit: c662440cf854139b72c998f854a0b9adcd7158bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68737569"
---
# <a name="configure-self-hosted-ir-as-a-proxy-for-azure-ssis-ir-in-adf"></a>ADF 'de Azure-SSIS IR için ara sunucu olarak şirket içinde barındırılan IR yapılandırma
Bu makalede, proxy olarak yapılandırılan şirket içinde barındırılan IR ile Azure Data Factory (ADF) üzerinde Azure-SSIS Integration Runtime (IR) üzerinde SQL Server Integration Services (SSIS) paketlerinin nasıl çalıştırılacağı açıklanmaktadır.  Bu özellik, [Azure-SSIS IR 'nizi bir sanal ağa](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network)eklemeden Şirket içindeki verilere erişmenizi sağlar.  Bu, şirket ağınızın Azure-SSIS IR 'nizi içine ekleyebilmeniz için aşırı karmaşık bir yapılandırma/kısıtlayıcı ilke olduğunda faydalıdır.

Bu özellik, şirket içi veri kaynağı ile bir veri akışı görevi içeren paketinizi iki hazırlama görevine bölecektir: şirket içinde barındırılan IR üzerinde çalışan ilk, verileri şirket içi veri kaynağından Azure Blob depolama alanındaki bir hazırlama alanına taşıyacaktır, ancak Azure-SSIS IR 'niz üzerinde çalışan ikinci bir adım, verileri hazırlama alanından amaçlanan veri hedefine taşıyacaktır.

Bu özellik ayrıca, Azure-SSIS IR tarafından henüz desteklenmeyen bölgelerde şirket içinde barındırılan IR sağlamanıza olanak tanıyan diğer avantajlar/özellikler de sağlar, veri kaynaklarınızın güvenlik duvarında şirket içinde barındırılan IR 'nin genel statik IP adresine izin verir, vb.

## <a name="prepare-self-hosted-ir"></a>Şirket içinde barındırılan IR hazırlama
Bu özelliği kullanabilmeniz için öncelikle [Azure-SSIS IR sağlama](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) makalesini IZLEYEREK bir ADF oluşturmanız ve bunun altında Azure-SSIS IR 'nizi sağlamanız gerekir.

Daha sonra, şirket içinde barındırılan IR 'yi, Azure-SSIS IR 'nin sağlandığı aynı ADF altında sağlamanız gerekir. [](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime)

Son olarak, şirket içi makinenize/Azure sanal makinenize (VM) aşağıdaki gibi otomatik olarak barındırılan IR 'nin ve ek sürücü ve çalışma zamanının en son sürümünü indirip yüklemeniz gerekir:
- Lütfen şirket içinde barındırılan IR 'nin en son sürümünü [buradan](https://www.microsoft.com/download/details.aspx?id=39717)indirin ve yükleyin.
- Paketlerinizdeki OLEDB bağlayıcıları kullanıyorsanız, lütfen ilgili OLEDB sürücülerini indirip şirket içinde barındırılan IR 'nin yüklendiği makinede yükleyin ve daha önce yapmadıysanız.  SQL Server (SQLNCLI) için OLEDB sürücüsünün önceki sürümünü kullanıyorsanız, 64 bitlik sürümü [buradan](https://www.microsoft.com/download/details.aspx?id=50402)indirebilirsiniz.  SQL Server (MSOLEDBSQL) için OLEDB sürücüsünün en son sürümünü kullanıyorsanız, 64 bitlik sürümü [buradan](https://www.microsoft.com/download/details.aspx?id=56730)indirebilirsiniz.  PostgreSQL, MySQL, Oracle vb. gibi diğer veritabanı sistemleri için OLEDB sürücüleri kullanıyorsanız, 64 bitlik sürümü ilgili web sitelerinden indirebilirsiniz.
- Lütfen daha önce yapmadıysanız, C++ Visual (VC) çalışma zamanını şirket IÇINDE barındırılan IR 'nin yüklendiği makinede indirip yükleyin.  64 bitlik sürümü [buradan](https://www.microsoft.com/download/details.aspx?id=40784)indirebilirsiniz.

## <a name="prepare-azure-blob-storage-linked-service-for-staging"></a>Azure Blob depolama bağlı hizmetini hazırlama için hazırlama
Lütfen [ADF bağlantılı hizmet oluşturma](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service) makalesini izleyerek Azure-SSIS IR 'nizin SAĞLANDıĞı aynı ADF altında Azure Blob depolama bağlı hizmeti oluşturun.  Lütfen şunlardan emin olun:
- **Veri deposu** Için **Azure Blob depolama** seçildi
- **Tümleştirme çalışma zamanı aracılığıyla Bağlan** Için **oto resolveıntegrationruntime** seçildi
- **Kimlik doğrulama yöntemi** için **hesap anahtarı**/**SAS URI**/**hizmet sorumlusu** seçilidir

![Azure Blob depolama bağlı hizmetini hazırlama için hazırlama](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-azure-ssis-ir-with-self-hosted-ir-as-a-proxy"></a>Azure-SSIS IR 'yi, şirket içinde barındırılan IR ile ara sunucu olarak yapılandırma
Şirket içinde barındırılan IR ve Azure Blob depolama bağlı hizmetinizi hazırlama için hazırladığınıza göre, yeni/mevcut Azure-SSIS IR 'yi, ADF portalında/uygulamada bir proxy olarak otomatik barındırılan IR ile yapılandırabilirsiniz.  Mevcut Azure-SSIS IR 'niz çalışıyorsa, bunu yapmadan önce lütfen durdurun ve ardından yeniden başlatın.

**Gelişmiş ayarlar** sayfasında, lütfen **Azure-SSIS Integration Runtime için bir proxy olarak şirket Içinde barındırılan Integration Runtime ayarlama** onay kutusunu Işaretleyin, kendi kendine barındırılan IR ve Azure Blob depolama bağlı hizmetini hazırlama için seçin ve bir blob belirtin isterseniz **hazırlama yolu** için kapsayıcı adı.

![Azure-SSIS IR 'yi, şirket içinde barındırılan IR ile ara sunucu olarak yapılandırma](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-settings-ssisir.png)

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>SSIS paketlerini proxy ile bağlanacak şekilde etkinleştir
Buradan indirilebilen ve buradan indirilebilecek tek başına bir yükleyici olarak, Visual Studio için SSIS [](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) projeleri Uzantısı ile en son SSDT 'yi kullanarak, OLEDB [](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)'ye eklenen yeni bir **connectbyproxy** özelliği bulabilirsiniz/ Düz dosya bağlantı yöneticileri.  

Şirket içindeki veritabanlarına/dosyalara erişmek için OLEDB/düz dosya kaynaklarıyla veri akışı görevleri içeren yeni paketler tasarlarken, ilgili bağlantı yöneticilerinin Özellikler panelinde onu **true** olarak ayarlayarak bu özelliği etkinleştirebilirsiniz.

![ConnectByProxy özelliğini etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

Bu özelliği, var olan paketleri bir tane ile tek tek değiştirmek zorunda kalmadan çalıştırırken de etkinleştirebilirsiniz.  2 seçenek vardır:
- Azure-SSIS IR 'niz üzerinde çalışacak en son SSDT ile bu paketleri içeren projeyi açma, yeniden oluşturma ve yeniden dağıtma: Özellik daha sonra SSMS 'den paket çalıştırırken paket açılır penceresini Çalıştır ' ın **bağlantı yöneticileri** sekmesinde görünen ilgili bağlantı yöneticileri için **true** olarak ayarlanarak etkinleştirilebilir.

  ![ConnectByProxy Property2 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  Özelliği, ADF işlem hatlarında paket çalıştırırken [SSIS paketi yürütme etkinliğinin](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **bağlantı yöneticileri** sekmesinde görünen Ilgili bağlantı yöneticileri için **true** olarak ayarlanarak da etkinleştirilebilir.
  
  ![ConnectByProxy property3 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- SSIS IR 'niz üzerinde çalıştırılacak olan paketleri içeren projeyi yeniden dağıtma: Özelliği daha sonra özellik yolu `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`sağlayarak etkinleştirilebilir ve paketleri SSMS 'den çalıştırırken paket açılır penceresini Çalıştır ' ın **Gelişmiş** sekmesinde bir özellik geçersiz kılma olarak **true** olarak ayarlanır.

  ![ConnectByProxy property4 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  Özelliği, bir özellik yolu `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`sunarak da etkinleştirilebilir ve bu özellik, ADF işlem hatlarında paket çalıştırırken [SSIS paketi yürütme etkinliğinin](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) **geçersiz** kılınması sekmesinde özellik geçersiz kılma olarak ayarlanır.
  
  ![ConnectByProxy Property5 etkinleştir](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>İlk ve ikinci hazırlama görevlerinde hata ayıkla
Şirket içinde barındırılan IR 'de çalışma zamanı günlüklerini `C:\ProgramData\SSISTelemetry` klasöründe ve `C:\ProgramData\SSISTelemetry\ExecutionLog` klasördeki ilk hazırlama görevlerinin yürütme günlüklerinde bulabilirsiniz.  Paketleri SSıSDB veya dosya sistemi/dosya paylaşımları/Azure dosyalarında sakladığınıza bağlı olarak, ikinci hazırlama görevlerinin yürütme günlükleri SSıSDB veya belirtilen günlüğe kaydetme yollarında bulunabilir.  İlk hazırlama görevlerinin benzersiz kimlikleri, ikinci hazırlama görevlerinin yürütme günlüklerinde de bulunabilir, örn. 

![İlk hazırlama görevinin benzersiz KIMLIĞI](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="billing-for-the-first-and-second-staging-tasks"></a>İlk ve ikinci hazırlama görevleri için faturalandırma
Şirket içinde barındırılan IR üzerinde çalışan ilk hazırlama görevleri, [ADF veri ardışık düzen fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/) makalesinde belirtilen şekilde faturalandırılır ve şirket IÇINDE barındırılan IR üzerinde çalışan tüm veri taşıma etkinlikleri aynı şekilde faturalandırılır.

Azure-SSIS IR 'niz üzerinde çalışan ikinci hazırlama görevleri ayrı olarak faturalandırılmaz, ancak çalıştırdığınız Azure-SSIS IR, [Azure-SSIS IR fiyatlandırma](https://azure.microsoft.com/pricing/details/data-factory/ssis/) makalesinde belirtilen şekilde faturalandırılır.

## <a name="current-limitations"></a>Geçerli sınırlamalar
- Şu anda yalnızca OLEDB/düz dosya bağlantı yöneticileri ve OLEDB/düz dosya kaynakları desteklenmektedir.
- Yalnızca **hesap anahtarı**/**SAS URI 'si**/**hizmet sorumlusu** kimlik doğrulaması ile yapılandırılmış Azure Blob depolama bağlı hizmetleri şu anda destekleniyor.
- Azure-SSIS IR 'nin sağlandığı aynı ADF kapsamında sağlanan yalnızca şirket içinde barındırılan IR, şu anda desteklenmektedir.

## <a name="next-steps"></a>Sonraki adımlar
Şirket içinde barındırılan IR 'yi Azure-SSIS IR 'niz için bir proxy olarak yapılandırdıktan sonra, ADF 'yi ADF işlem hatlarında SSIS paket etkinliklerini yürütme olarak çalıştır bölümünde SSIS [paketlerini yürütme, ADF işlem hatları 'nda SSIS paketi ](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).