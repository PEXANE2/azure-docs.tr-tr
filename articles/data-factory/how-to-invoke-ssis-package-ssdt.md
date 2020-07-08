---
title: SSIS paketlerini SSDT 'den yürütme
description: SSDT 'den Azure 'da SSIS paketlerini yürütmeyi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.author: sawinark
author: swinarko
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 07/31/2019
ms.openlocfilehash: 1d8261d05f59c7f40ba6b1e2d59d2b15ad56de95
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84424591"
---
# <a name="execute-ssis-packages-in-azure-from-ssdt"></a>SSDT 'den Azure 'da SSIS paketlerini yürütme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, SQL Server Veri Araçları (SSDT) üzerindeki Azure özellikli SQL Server Integration Services (SSIS) projelerinin özelliği açıklanmaktadır. Bu, paketleri Azure Data Factory (ADF) üzerinde Azure-SSIS Integration Runtime (IR) üzerinde çalıştırmanızı sağlar.  Bu özelliği kullanarak mevcut SSIS paketlerinizi, Azure 'a geçiş yapmak veya Azure 'a geçirmek ya da Azure 'da çalıştırmak üzere yeni SSIS paketleri geliştirmek için kullanabilirsiniz &.

Bu özellikle, yeni bir Azure-SSIS IR oluşturabilir veya var olan bir dosyayı SSIS projelerine iliştirebilir ve sonra paketlerinizi yürütebilirsiniz.  Proje dağıtım modelindeki SSIS kataloğuna (SSıSDB) dağıtılacak paketleri ve paket dağıtım modelindeki dosya sistemlerine/dosya paylaşımlarına/Azure dosyalarına dağıtılmasını destekliyoruz. 

## <a name="prerequisites"></a>Ön koşullar
Bu özelliği kullanmak için lütfen Visual Studio için SSIS projeleri uzantısı 'nı [buradan veya buradan](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) tek başına bir yükleyici olarak indirip [yükleyin.](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)

## <a name="azure-enable-ssis-projects"></a>Azure-SSIS projelerini etkinleştir
### <a name="create-new-azure-enabled-ssis-projects"></a>Yeni Azure özellikli SSIS projeleri oluşturma
SSDT 'de **Integration Services projesi (Azure-etkin)** şablonunu kullanarak yeni Azure özellikli SSIS projeleri oluşturabilirsiniz.

   ![Yeni Azure özellikli SSIS projesi](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-new-project.png)

Azure etkin proje oluşturulduktan sonra, Azure Data Factory SSIS 'e bağlanmanız istenir.

   ![Connect Azure-SSIS IR Istemi](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-integration-runtime-connect-prompt.png)

Azure-SSIS IR hemen bağlanmak istiyorsanız, Ayrıntılar için lütfen [Azure-SSIS IR Connect](#irconnect) bölümüne bakın. Ayrıca, bir menü açmak için SSDT Çözüm Gezgini paneli ' ndeki proje düğümüne sağ tıklayıp, sonra da SSIS **Azure Data Factory** alt menüsünde ssın altındakı **Azure Data Factory SSIS 'e Bağlan** menü öğesini seçerek daha sonra bağlanabilirsiniz.

### <a name="azure-enable-existing-ssis-projects"></a>Azure-var olan SSIS projelerini etkinleştirin
Mevcut SSIS projeleri için, şu adımları izleyerek Azure 'u etkinleştirebilirsiniz:

1. Bir menü açmak için SSDT Çözüm Gezgini panelinde proje düğümüne sağ tıklayın ve ardından **Azure etkin proje Sihirbazı 'nı**başlatmak Için, **SSIS Azure Data Factory** alt menüsünde bulunan **Azure etkin proje** menü öğesini seçin.

   ![Azure-var olan SSIS projesini etkinleştir](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-for-existing-project.png)

2. **Visual Studio yapılandırması Seç** sayfasında, Azure 'da paket yürütme ayarlarını uygulamak Için Visual Studio yapılandırması ' nı seçin. Bulut ve Azure için yeni bir Visual Studio yapılandırması oluşturmak iyi bir uygulamadır; projenizi bulut yapılandırmasına karşı etkinleştirin. Birden çok yapılandırma ile farklı ortamlara (yerel olarak veya Azure 'da) göre parametrelerinizi farklı değerler atayabilirsiniz. Daha fazla ayrıntı için lütfen [Bu örneğe](#example) bakın.

   ![Visual Studio yapılandırması seçin](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-select-visual-studio-configurations.png)

3. Azure-mevcut SSIS projelerinizi etkinleştirmek, hedef sunucu sürümünü, şu anda **SQL Server 2017**olan Azure-SSIS IR tarafından desteklenen en son bir sürüm olacak şekilde ayarlamanızı gerektirir. Daha önce yapmadıysanız, paketinizin SQL Server 2017 ' de desteklenmeyen ek bileşenler içerip içermemesi gerektiğini denetlemeniz ve sorun yoksa devam etmek için Ileri düğmesine tıklamasını öneririz.

   ![Hedef sunucu sürümünü değiştir](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-switch-target-server-version-step.png)

4. Azure-SSIS IR bağlantısını gerçekleştirmek için [Azure-SSIS IR bağlanma](#irconnect) bölümüne bakın.

## <a name="connect-azure-enabled-projects-to-ssis-in-azure-data-factory"></a><a name="irconnect"></a>Azure etkin projelerini SSIS 'ye bağlama Azure Data Factory

Azure etkin projelerinizi SSIS 'ye bağlanarak, paketlerinizi Azure dosyalarına yükleyebilir ve Azure-SSIS IR üzerinde çalıştırabilirsiniz. Bunu yapmak için lütfen şu adımları izleyin:

1. **ADF 'de SSIS giriş** sayfasında, giriş sayfasını gözden geçirin ve **İleri** düğmesine tıklayarak devam edin.

   ![ADF 'de SSIS girişi](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard.png)

2. **ADF 'de SSIS IR seçin** sayfasında, mevcut ADF 'yi seçip Azure-SSIS IR paketleri çalıştırın veya yoksa yeni bir tane oluşturun.
   - Mevcut Azure-SSIS IR seçmek için önce ilgili Azure aboneliğini ve sonra ADF 'yi seçin.
   - Azure-SSIS IR olmayan mevcut ADF 'yi seçerseniz, ADF portalında/uygulamada yeni bir tane oluşturmak için **SSIS IR oluştur** düğmesine tıklayın.
   - ADF 'ye sahip olmayan mevcut Azure aboneliğinizi seçerseniz, aşağıdaki düzende adlandırılan, sizin adınıza otomatik olarak yeni bir Azure Kaynak grubu, Data Factory ve SSIS IR oluşturmak üzere **Integration Runtime oluşturma Sihirbazı**'nı başlatmak için **SSIS IR oluştur** düğmesine tıklayın: bızım **ön eki-RG/df/IR-yourcreationtime**.

   ![ADF 'de SSIS IR seçin](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard2.png)

3. **Azure Storage 'ı seçin** sayfasında, mevcut Azure depolama hesabınızı seçerek paketleri Azure dosyalarına yükleyin veya yoksa yeni bir tane oluşturun.
   - Mevcut Azure depolama hesabınızı seçmek için öncelikle ilgili Azure aboneliğini seçin.
   - Azure depolama hesabı olmayan Azure-SSIS IR aynı Azure aboneliğini seçerseniz, Azure-SSIS IR adınızın ve oluşturulma tarihinin bir önekini birleştirerek adlandırılan Azure-SSIS IR aynı konumda sizin adınıza otomatik olarak yeni bir tane oluşturmak için **Azure depolama oluştur** düğmesine tıklayın.
   - Azure depolama hesabı bulunmayan farklı bir Azure aboneliği seçerseniz Azure portal yeni bir tane oluşturmak için **Azure depolama oluştur** düğmesine tıklayın.

   ![Azure Depolama Alanı'nı seçin](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard3.png)

4. Bağlantınızı gerçekleştirmek için **Bağlan** düğmesine tıklayın.  Seçtiğiniz Azure-SSIS IR ve Azure depolama hesabınızı SSDT Çözüm Gezgini panelinde **bağlı Azure kaynakları** düğümü altında görüntüleyeceğiz.  Ayrıca, bir menü açmak ve ardından bunu ADF Portal/uygulama ' ya tıklayarak **Start\stop\yönet** menü öğesini seçmek için, Azure-SSIS IR durumunu yenileyecektir.

## <a name="execute-ssis-packages-in-azure"></a>Azure 'da SSIS paketlerini yürütme
### <a name="azure-enabled-setting"></a>Azure etkin ayarı
Paketlerinizi Azure 'da yürütmeden önce, yürütme ayarlarını yapılandırmayı seçebilirsiniz. SSIS paketleriniz için Windows kimlik doğrulamasını etkinleştirmek istiyorsanız lütfen aşağıdaki adımları izleyin:

1. Bir menü açmak için SSDT Çözüm Gezgini panelinde proje düğümüne sağ tıklayın ve sonra Azure Data Factory alt menüsünde **SSIS altında bulunan** **Azure etkin ayarlar** menü öğesini seçin.

   ![Azure etkin ayarları](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-azure-enabled-setting.png)

2. **Windows kimlik doğrulamasını etkinleştir** açılan listesine tıklayın ve **doğru**öğesini seçin. Ardından, kimlik bilgilerini girmek için **Windows kimlik doğrulaması kimlik bilgileri** için Düzenle düğmesine tıklayın.

   ![Windows kimlik doğrulamasını etkinleştir](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-windows-authentication-open.png)

3. **Windows kimlik doğrulama kimlik bilgileri** Düzenleyicisi 'nde kimlik bilgilerini sağlayın.

   ![Windows kimlik doğrulama kimlik bilgileri](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-windows-authentication-credential.png)

### <a name="starting-package-executions"></a>Paket yürütmeleri başlatılıyor
Projelerinizi ADF 'de SSIS 'e bağladıktan sonra, Azure-SSIS IR paketleri çalıştırabilirsiniz.  Paket yürütmelerini başlatmak için iki seçeneğiniz vardır:
-  Bir menü açmak ve **Azure 'Da Yürüt** menü öğesini seçmek için SSDT araç çubuğundaki **Başlat** düğmesine tıklayın 

   ![Azure 'da Yürüt](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package.png)

-  Bir menü açmak için SSDT Çözüm Gezgini panelinde paket düğümüne sağ tıklayın ve **paketi Azure** menü öğesinde Yürüt ' ü seçin.

   ![Paketi Azure 'da Yürüt](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package2.png)

> [!NOTE]
> Paketlerinizi Azure 'da çalıştırmak için çalışan bir Azure-SSIS IR olması gerekir, bu nedenle Azure-SSIS IR durdurulmuşsa, bir iletişim kutusu penceresi başlatılır.  Özel kurulum süresini dışlayarak, bu işlem 5 dakika içinde tamamlanmalıdır, ancak sanal bir ağa katılmak Azure-SSIS IR için yaklaşık 20-30 dakika sürebilir.  Paketlerinizi Azure 'da yürüttükten sonra, bir menü açmak ve ardından bunu yapmak için ADF Portal/uygulama ' ya tıklayarak **STARTDT** ' nin Çözüm Gezgini panelinde, çalışma maliyetini yönetmek için Azure-SSIS IR durdurabilirsiniz.

### <a name="checking-package-execution-logs"></a>Paket yürütme günlükleri denetleniyor
Paket yürütmeyi başlattığınızda, SSDT Ilerleme penceresinde günlüğünü biçimlendireceğiz ve görüntüleriz.  Uzun süre çalışan bir paket için günlük dosyasını dakikalar içinde düzenli olarak güncelleştireceğiz.  SSDT araç çubuğundaki **Durdur** düğmesine tıklayarak ve hemen iptal edecek şekilde paket yürütmeyi durdurabilirsiniz.  Günlük ham verilerini evrensel adlandırma kuralı (UNC) yolunda da geçici olarak bulabilirsiniz `\\<YourConnectedAzureStorage>.file.core.windows.net\ssdtexecution\<YourProjectName-FirstConnectTime>\<YourPackageName-tmp-ExecutionTime>\logs` , ancak bir gün sonra temizleyeceğiz.

### <a name="switching-package-protection-level"></a>Paket koruma düzeyini değiştirme
Azure 'da SSIS paketlerini yürütme, **EncryptSensitiveWithUserKey** / **EncryptAllWithUserKey** koruma düzeylerini desteklemez.  Sonuç olarak, paketleriniz bunlarla yapılandırıldıysa, **EncryptSensitiveWithPassword** / paketlerinizi Azure-SSIS IR yürütmek üzere Azure dosyalarına karşıya yüklerken rastgele oluşturulan parolalarla, bunları sırasıyla EncryptSensitiveWithPassword**EncryptAllWithPassword**' ye geçeceğiz.

> [!NOTE]
> Paketleriniz, **EncryptSensitiveWithUserKey** / **EncryptAllWithUserKey** koruma düzeyleriyle yapılandırılmış diğer paketlere başvuran paket yürütme görevleri içeriyorsa, paketlerinizi yürütmeden önce, bu diğer paketleri sırasıyla **EncryptSensitiveWithPassword**EncryptAllWithPassword kullanacak şekilde el ile yeniden yapılandırmanız gerekir / **EncryptAllWithPassword**.

Paketleriniz zaten **EncryptSensitiveWithPassword** / **EncryptAllWithPassword** koruma düzeyiyle yapılandırıldıysa, bunları değişmeden tutacağız, ancak paketlerinizi Azure-SSIS IR yürütmek üzere Azure dosyalarına yüklerken rastgele oluşturulan parolalar kullanmaya devam edecektir.

### <a name="using-package-configuration-file"></a>Paket yapılandırma dosyasını kullanma
Çalışma zamanında değişken değerlerini değiştirmek için paket dağıtım modelinde paket yapılandırma dosyalarını kullanırsanız, bu dosyaları paketlerinizle birlikte Azure-SSIS IR yürütmek üzere Azure dosyalarına otomatik olarak yükleyeceğiz.

### <a name="using-execute-package-task"></a>Paket yürütme görevini kullanma
Paketleriniz yerel dosya sistemlerinde depolanan diğer paketlere başvuran paket yürütme görevleri içeriyorsa, aşağıdaki ek kurulumları gerçekleştirmeniz gerekir:

1. Diğer paketleri, projelerinize bağlı aynı Azure depolama hesabı altında Azure dosyalarına yükleyin ve örneğin yeni UNC yollarını alın.`\\test.file.core.windows.net\ssdtexecution\Package1.dtsx`

2. Paket görevlerinin dosya Bağlantı Yöneticisinde bu diğer paketlerin dosya yolunu yeni UNC yollarıyla değiştirin
   - SSDT çalıştıran makineniz yeni UNC yoluna erişe, dosya Bağlantı Yöneticisi 'nin Özellikler panelinde dosya yolunu değiştirebilirsiniz
   - Alternatif olarak, çalışma zamanında doğru değeri atamak için dosya yolu için bir değişken kullanabilirsiniz

Paketleriniz aynı projedeki diğer paketlere başvuran paket yürütme görevlerini içeriyorsa, ek bir kurulum gerekmez.

## <a name="switching-package-execution-environments-with-azure-enabled-projects"></a><a name="example"></a>Azure özellikli projelerle paket yürütme ortamlarını değiştirme

Paket yürütme ortamlarını Azure etkinleştirilmiş projelerle değiştirmek için, ortama özgü parametreler için farklı değerler uygulamak üzere birden çok Visual Studio yapılandırması oluşturabilirsiniz. Örneğin, belirtilen dosyanın özniteliklerini ayarlayan bir **dosya sistemi görevi** olan basıt bir SSIS paketi vardır ve aşağıdaki adımları kullanarak buluta geçiş yapabilirsiniz:

1. Hedef dosyanın dosya yolu olan dize türünde bir parametre **FilePath** 'i tanımlayın.

   ![Paket parametresini tanımla](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-define-parameters.png)

2. **Kaynak bağlantıyı** Bu parametre ile ilişkilendirin. 

   ![Kaynak bağlantısını Güncelleştir](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-update-task-with-parameters.png)

3. Visual Studio Configuration Manager 'de bulut için yeni Visual Studio yapılandırması oluşturun.

4. Her Visual Studio yapılandırmasına karşı bu parametre için değerleri tanımlayın.

   ![Parametre değerlerini geçersiz kıl](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-override-parameter.png)

5. Bu SSIS projesi, bulut için Visual Studio yapılandırmasına karşı etkinleştirilmiştir.

6. Bu paketi Azure 'da yürütün. Geçerli Visual Studio yapılandırmasını değiştirerek ortamı kolayca yerel bir şekilde değiştirebilirsiniz.

   ![Visual Studio yapılandırmasını değiştirme](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-example-switch-configurations.png)

## <a name="next-steps"></a>Sonraki adımlar
SSDT 'den Azure 'da paketlerinizi çalıştırmaya memnun olduktan sonra, ADF 'yi ADF Işlem hatlarında SSIS paket etkinliklerini Yürüt olarak [çalıştırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)ve çalıştırma konusuna bakın.
