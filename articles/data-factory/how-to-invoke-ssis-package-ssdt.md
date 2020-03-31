---
title: SSDT'den SSIS paketlerini çalıştırın
description: SSDT'den Azure'da SSIS paketlerini nasıl yürüteriz öğrenin.
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
ms.openlocfilehash: 5f21623af9b89bbb020063dfb72f7b60e65a6ebe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74927717"
---
# <a name="execute-ssis-packages-in-azure-from-ssdt"></a>SSDT'den Azure'daki SSIS paketlerini yürütme
Bu makalede, SQL Server Veri Araçları 'ndaki (SSDT) Azure özellikli SQL Server Tümleştirme Hizmetleri (SSIS) projelerinin özelliği açıklanmaktadır ve bu da paketleri Azure Veri Fabrikası'nda (ADF) Azure-SSIS Tümleştirme Çalışma Süresi 'nde (IR) çalıştırmanıza olanak tanır.  Bu özelliği, & kaydırmadan/Azure'a geçirmeden önce varolan SSIS paketlerinizi test etmek veya Azure'da çalışacak yeni SSIS paketleri geliştirmek için kullanabilirsiniz.

Bu özellik sayesinde, yeni bir Azure-SSIS IR oluşturabilir veya SSIS projelerine varolan bir tane ekleyebilir ve paketlerinizi bu projede yürütebilirsiniz.  Proje Dağıtım Modeli'nde SSIS kataloğuna (SSISDB) ve Paket Dağıtım Modeli'nde dosya sistemlerine/dosya paylaşımlarına/Azure Dosyalarına dağıtılacak paketleri çalıştırmayı destekliyoruz. 

## <a name="prerequisites"></a>Ön koşullar
Bu özelliği kullanmak için, visual studio için SSIS Projects uzantısı ile en son SSDT'yi [buradan](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) veya [buradan](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer)bağımsız bir yükleyici olarak indirin ve kurun.

## <a name="azure-enable-ssis-projects"></a>Azure etkin SSIS projeleri
SSDT'de, **Tümleştirme Hizmetleri Projesi (Azure Özellikli)** şablonunu kullanarak Yeni Azure özellikli SSIS projeleri oluşturabilirsiniz.

![Yeni Azure Özellikli SSIS Projesi](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-new-project.png)

Alternatif olarak, bir menü açmak ve ardından **Azure Özellikli** menü öğesini seçmek için SSDT'nin Solution Explorer panelindeki proje düğümüne sağ tıklayarak mevcut SSIS projelerinizi Azure'da etkinleştirebilirsiniz.

![Azure-Enable Existing SSIS Projesi](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-existing-project.png)

Mevcut SSIS projelerinizi azure'a etkinleştirmek, hedef sunucu sürümünü, şu anda **SQL Server 2017**olan Azure-SSIS IR tarafından desteklenen en son sürüm olarak ayarlamanızı gerektirir, bu nedenle bunu zaten yapmadıysanız, bunu yapmak için bir iletişim penceresi açılır.

## <a name="connect-azure-enabled-projects-to-ssis-in-azure-data-factory"></a>Azure özellikli projeleri Azure Veri Fabrikası'nda SSIS'e bağlayın
Azure özellikli projelerinizi ADF'de SSIS'e bağlayarak, paketlerinizi Azure Dosyalarına yükleyebilir ve Azure-SSIS IR'de çalıştırabilirsiniz.  Bunu yapmak için lütfen aşağıdaki adımları izleyin:

1. Bir menü açmak ve **ADF Bağlantı Sihirbazı'nda SSIS'i**başlatmak için **Azure Veri Fabrikası'nda SSIS'e Bağlan'ı** seçmek için SSDT'nin Solution Explorer panelinde projeye veya Bağlantılı Azure **Kaynakları** düğümüne sağ tıklayın.

   ![ADF'de SSIS'e bağlanın](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-existing-project2.png)

2. ADF Giriş sayfasında **SSIS'te** giriş sayfasını inceleyin ve devam etmek için **Sonraki** düğmesine tıklayın.

   ![ADF Girişte SSIS](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard.png)

3. **ADF'deki SSIS IR seç'te,** paketleri çalıştırmak veya yoksa yenilerini oluşturmak için mevcut ADF ve Azure-SSIS IR'nizi seçin.
   - Mevcut Azure-SSIS IR'nizi seçmek için önce ilgili Azure aboneliğini ve ADF'yi seçin.
   - Herhangi bir Azure-SSIS IR'si olmayan mevcut ADF'nizi seçerseniz, ADF portalında/uygulamasında yeni bir tane oluşturmak için **SSIS IR oluştur** düğmesini tıklatın.
   - ADF'si olmayan mevcut Azure aboneliğinizi seçerseniz, Aşağıdaki desende adı geçen sizin adınıza otomatik olarak yeni bir Azure Kaynak Grubu, Veri Fabrikası ve SSIS IR oluşturabilmeniz için konumu ve önek girebileceğiniz **Tümleştirme Çalışma Zamanı Oluşturma**Sihirbazı'nı başlatmak için **SSIS IR** oluştur düğmesini **tıklatın.**
   
   ![ADF'de SSIS IR'yi seçin](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard2.png)

4. Azure **Depolama'yı Seç** sayfasında, paketleri Azure Dosyalarına yüklemek için mevcut Azure Depolama hesabınızı seçin veya yoksa yeni bir tane oluşturun.
   - Mevcut Azure Depolama hesabınızı seçmek için önce ilgili Azure aboneliğini seçin.
   - Azure-SSIS IR hesabınızda herhangi bir Azure Depolama hesabı olmayan aynı Azure aboneliğini seçerseniz, Azure-SSIS IR adınızın önekivesinin bir önekini ve oluşturma tarihini birleştirerek adını taşıyan Azure-SSIS IR'niz ile aynı konumda sizin adınıza otomatik olarak yeni bir tane oluşturmamız için **Azure Depolama** Oluştur düğmesini tıklatın.
   - Azure Depolama hesabı olmayan farklı bir Azure aboneliği seçerseniz, Azure portalında yeni bir tane oluşturmak için **Azure Depolama Oluştur** düğmesini tıklatın.
   
   ![Azure Depolama Alanı'nı seçin](media/how-to-invoke-ssis-package-ssdt/ssis-in-adf-connection-wizard3.png)

5. Bağlantınızı tamamlamak için **Bağlan** düğmesine tıklayın.  Seçili Azure-SSIS IR ve Azure Depolama hesabınızı SSDT'nin Solution Explorer panelinde **Bağlantılı Azure Kaynakları** düğümü altında görüntüleyeceğiz.  Ayrıca Azure-SSIS IR'nizin durumunu yenilerken, bir menü açmak için düğümüne sağ tıklayarak ve ardından sizi ADF portalına/uygulamasına götüren **Başlat\Durdur\Yönet** menü öğesini seçerek yönetebilirsiniz.

## <a name="execute-ssis-packages-in-azure"></a>Azure'da SSIS paketlerini yürütün
### <a name="starting-package-executions"></a>Paket yürütmelerini başlatma
Projelerinizi ADF'de SSIS'e bağladıktan sonra, Azure-SSIS IR'de paketleri çalıştırabilirsiniz.  Paket yürütmeleri başlatmak için iki seçeneğiniz vardır:
-  Menüyü düşürmek için SSDT araç çubuğundaki **Başlat** düğmesine tıklayın ve Azure menü **öğesinde Yürüt'ünü** seçin 

   ![Azure'da yürütme](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package.png)

-  Bir menü açmak ve Azure menü öğesinde **Yürüt'ü** seçmek için SSDT'nin Solution Explorer panelindeki paket düğümüne sağ tıklayın.

   ![Paketi Azure'da Yürüt](media/how-to-invoke-ssis-package-ssdt/ssdt-azure-enabled-execute-package2.png)

> [!NOTE]
> Paketlerinizi Azure'da yürütmek için çalışan bir Azure-SSIS IR'niz olması gerekir, bu nedenle Azure-SSIS IR'niz durdurulursa, başlatmak için bir iletişim penceresi açılır.  Herhangi bir özel kurulum süresi hariç, bu işlem 5 dakika içinde tamamlanmalıdır, ancak Azure-SSIS IR'nin sanal ağa katılması yaklaşık 20 -30 dakika sürebilir.  Paketlerinizi Azure'da çalıştırdıktan sonra, bir menü açmak için SSDT'nin Solution Explorer panelindeki düğüme sağ tıklayarak ve ardından sizi ADF portalına/uygulamasına götüren **Başlat\Stop\Yönet** menü öğesini seçerek çalışma maliyetini yönetmek için Azure-SSIS IR'nizi durdurabilirsiniz.

### <a name="checking-package-execution-logs"></a>Paket yürütme günlüklerini denetleme
Paket yürütmenizi başlattığınızda, oturumunu SSDT'nin İlerleme penceresinde biçimlendirecek ve görüntüleceğiz.  Uzun süren bir paket için, günlüklerini düzenli olarak tutanaklarla güncelleştireceğiz.  Hemen iptal edecek SSDT araç çubuğundaki **Durdur** düğmesine tıklayarak paket yürütmenizi durdurabilirsiniz.  Ayrıca, günlük ham verilerini Evrensel Adlandırma Sözleşmesi (UNC) `\\<YourConnectedAzureStorage>.file.core.windows.net\ssdtexecution\<YourProjectName-FirstConnectTime>\<YourPackageName-tmp-ExecutionTime>\logs`yolunda geçici olarak bulabilirsiniz: , ancak bir gün sonra temizleyeceğiz.

### <a name="switching-package-protection-level"></a>Paket koruma düzeyini değiştirme
Azure'da SSIS paketlerinin yürütülmesi **EncryptSensitiveWithUserKey**/**EncryptAllWithUserKey** koruma düzeylerini desteklemez.  Sonuç olarak, paketleriniz bunlarla yapılandırılırsa, paketlerinizi Azure-SSIS IR'nizde yürütülecek şekilde Azure Dosyalarına yüklediğimizde, paketlerinizi rastgele oluşturulan parolalarla sırasıyla **EncryptSensitiveWithPassword**/**EncryptAllWithPassword'e**geçici olarak dönüştürüruz.

> [!NOTE]
> Paketleriniz **EncryptSensitiveWithUserKey**/**EncryptWithUserKey** koruma düzeyleri ile yapılandırılan diğer paketlere atıfta bulunan Yürüt Paket Görevlerini içeriyorsa, paketlerinizi yürütmeden önce sırasıyla **EncryptSensitiveWithPassword**/**EncryptAllWithPassword'i**kullanmak üzere bu diğer paketleri el ile yeniden yapılandırmanız gerekir.

Paketleriniz zaten **EncryptSensitiveWithPassword**/**EncryptAllWithPassword** koruma düzeyleriyle yapılandırıldıysa, bunları değişmeden tutacağız, ancak paketlerinizi Azure-SSIS IR'nizde yürütülecek şekilde Azure Dosyalarına yüklediğimizde rasgele oluşturulan parolaları kullanmaya devam edeceğiz.

### <a name="using-package-configuration-file"></a>Paket yapılandırma dosyalarını kullanma
Paket Dağıtım Modeli'ndeki paket yapılandırma dosyalarını çalışma zamanında değişken değerlerini değiştirmek için kullanırsanız, bu dosyaları otomatik olarak Azure-SSIS IR'nizde yürütülmesi için paketleriniz ile Birlikte Azure Dosyalarına yükleriz.

### <a name="using-execute-package-task"></a>Paket Görevini Yürütme
Paketleriniz, yerel dosya sistemlerinde depolanan diğer paketlere atıfta bulunan Yürüt Paket Görevlerini içeriyorsa, aşağıdaki ek kurulumları gerçekleştirmeniz gerekir:

1. Diğer paketleri projelerinize bağlı aynı Azure Depolama hesabı altında Azure Dosyalarına yükleyin ve yeni UNC rotalarını alın( örn.`\\test.file.core.windows.net\ssdtexecution\Package1.dtsx`

2. Yürüt Paket Görevlerini Yürüt'ün Dosya Bağlantı Yöneticisi'ndeki diğer paketlerin dosya yolunu yeni UNC yolu ile değiştirme
   - SSDT çalıştıran makineniz yeni UNC yoluna erişemiyorsa, Dosya Bağlantısı Yöneticisi'nin Özellikler panelindeki dosya yolunu değiştirebilirsiniz
   - Alternatif olarak, dosya yolu için bir değişken ilerler zaman doğru değeri atamak için kullanabilirsiniz

Paketleriniz, aynı projedeki diğer paketlere atıfta bulunan Yürüt Paket Görevlerini içeriyorsa, ek kurulum gerekmez.

## <a name="next-steps"></a>Sonraki adımlar
Paketlerinizi Azure'da SSDT'den çalıştırmayı tatmin ettikten sonra, bunları ADF ardışık hatlarında SSIS Paketi etkinliklerini yürütebilir ve çalıştırabilirsiniz, [SSIS paketlerini ADF ardışık hatlarında Yürütülür olarak Çalıştır'a](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)bakın.
