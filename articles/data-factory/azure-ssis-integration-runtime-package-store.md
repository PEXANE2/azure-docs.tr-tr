---
title: Paketleri Azure-SSIS Integration Runtime paket deposuyla yönetme
description: Azure-SSIS Integration Runtime paket deposu ile paketlerin nasıl yönetileceğini öğrenin.
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
ms.date: 05/25/2020
ms.openlocfilehash: fa1ab60ae5690bd3782e4c849ca7cb5c29d640c2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84199059"
---
# <a name="manage-packages-with-azure-ssis-integration-runtime-package-store"></a>Paketleri Azure-SSIS Integration Runtime paket deposuyla yönetme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Şirket içi SQL Server Integration Services (SSIS) iş yüklerinizi Buluta taşımak & için, Azure Data Factory (ADF) içinde Azure-SSIS Integration Runtime (IR) sağlayabilirsiniz. Daha fazla bilgi için bkz. [sağlama Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure). Bir Azure-SSIS IR şunları destekler:

- Azure SQL veritabanı sunucusu/yönetilen örneği (proje dağıtım modeli) tarafından barındırılan SSIS kataloğuna (SSıSDB) dağıtılan Paketleri çalıştırma
- Azure SQL yönetilen örneği (paket dağıtım modeli) tarafından barındırılan dosya sistemine, Azure dosyalarına veya SQL Server veritabanına (MSDB) dağıtılan Paketleri çalıştırma

Paket dağıtım modelini kullanırken, Azure SQL yönetilen örneği tarafından barındırılan dosya sisteminin/Azure dosyalarının/MSDB 'nin üzerine bir paket yönetim katmanı sağlayan paket depolarıyla Azure-SSIS IR sağlamak isteyip istemediğinizi seçebilirsiniz. Azure-SSIS IR paket deposu, paketleri içeri/dışarı/dışarı/dışarı/dışarı aktarmanıza/çalıştırmanıza ve [eskı SSIS paket deposuna](https://docs.microsoft.com/sql/integration-services/service/package-management-ssis-service?view=sql-server-2017)benzer SQL Server Management Studio (SSMS) üzerinden çalışan paketlerin izlenmesini/durdurulmasına olanak tanır. 

## <a name="connect-to-azure-ssis-ir"></a>Azure-SSIS IR Bağlan

Azure-SSIS IR sağlandıktan sonra, SSMS 'de paket depolarına gözatabilmeniz için bu dosyaya bağlanabilirsiniz.

![Azure-SSIS IR Bağlan](media/azure-ssis-integration-runtime-package-store/ssms-package-store-connect.png)

SSMS **Nesne Gezgini** penceresinde, **bağlan** açılan menüsünde **Azure-SSIS Integration Runtime** ' i seçin. Ardından, Azure 'da oturum açın ve paket mağazalarınızın eklendiği ilgili aboneliği, ADF 'yi ve Azure-SSIS IR seçin. Azure-SSIS IR, altında **çalışan paketler** ve **depolanmış paketler** düğümleri ile birlikte görüntülenir. Paket mağazalarınızı altında görmek için **saklı paketler** düğümünü genişletin. Altındaki klasörleri ve paketleri görmek için paket mağazalarınızı genişletin. SSMS bunlara otomatik olarak bağlanamıyorsa paket depolarınız için erişim kimlik bilgilerini girmeniz istenebilir. Örneğin, MSDB 'nin üzerinde bir paket deposu genişletirseniz, önce Azure SQL yönetilen örneğine bağlanmanız istenebilir.

![Azure SQL yönetilen örneği 'ne Bağlan](media/azure-ssis-integration-runtime-package-store/ssms-package-store-connect2.png)

## <a name="manage-folders-and-packages"></a>Klasörleri ve paketleri yönetme

SSMS üzerinde Azure-SSIS IR gözatarken, bir menü açmak ve **Yeni klasör**, **paket Içeri**aktar, **paketi dışarı aktar**, **Sil**veya **Yenile**' yi seçmek için herhangi bir paket depolarına/klasöre/paketlerine sağ tıklayabilirsiniz.

   ![Klasörleri ve paketleri yönetme](media/azure-ssis-integration-runtime-package-store/ssms-package-store-manage.png)

   *  İçeri aktarılan paketler için yeni bir klasör oluşturmak üzere **Yeni klasör** ' ü seçin.

   *  Paketleri **dosya sisteminden**, **SQL Server** (msdb) veya eski **SSIS paketi deposundan** paket deponuza içeri aktarmak için **paketi içeri aktar** ' ı seçin.

      ![Paketi içeri aktar](media/azure-ssis-integration-runtime-package-store/ssms-package-store-import.png)

      İçinden içeri aktarılacak **paket konumuna** bağlı olarak, ilgili **sunucu** / **kimlik doğrulaması türünü**seçin, gerekirse erişim kimlik bilgilerini girin, **paket yolunu**seçin ve yeni **paket adını**girin. Paketler içeri aktarılırken, koruma düzeyi değiştirilemez. Bunu değiştirmek için SQL Server Veri Araçları (SSDT) veya `dtutil` komut satırı yardımcı programını kullanın.

   *  Paketleri paket deponuzdan **dosya sistemine**, **SQL Server** (msdb) veya eski **SSIS paket deposuna**dışarı aktarmak için **paketi dışarı aktar** ' ı seçin.

      ![Paketi dışarı aktar](media/azure-ssis-integration-runtime-package-store/ssms-package-store-export.png)

      Uygulamasına verilecek **paket konumuna** bağlı olarak, ilgili **sunucu** / **kimlik doğrulaması türünü**seçin, gerekirse erişim kimlik bilgilerini girin ve **paket yolunu**seçin. Paketler dışa aktarıldığında, önce şifrelerini çözmek için parolaları girin ve ardından koruma düzeyini değiştirebilirsiniz, örneğin hassas verileri kaydetme veya tüm/hassas verileri Kullanıcı anahtarı/parola ile şifrelemek için kullanabilirsiniz.

   *  Paket deponuzdan mevcut klasörleri/paketleri silmek için **Sil** ' i seçin.

   *  Paket deponuzda yeni eklenen klasörleri/paketleri görüntülemek için **Yenile** ' yi seçin.

## <a name="execute-packages"></a>Paketleri Yürüt

SSMS 'de Azure-SSIS IR gözatarken, bir menü açmak ve **paketi Çalıştır**' ı seçmek için saklı paketlere sağ tıklayabilirsiniz.  Bu, ADF işlem hatlarında SSIS paket etkinliklerini yürütme olarak Azure-SSIS IR üzerinde paket yürütmelerini yapılandırabileceğiniz **paket yürütme yardımcı programı** iletişim kutusunu açar.

![Paket Yürütme Yardımcı Programı sayfaları 1 & 2](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute.png)

![Paket Yürütme Yardımcı Programı sayfaları 3 & 4](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute2.png)

**Paket yürütme yardımcı programı** Iletişim kutusunun **genel**, **yapılandırmalar**, **yürütme seçenekleri**ve **günlüğe kaydetme** sayfaları, paketinizin şifreleme parolasını girebileceğiniz SSIS paket etkinliğinin **Ayarlar** sekmesine karşılık gelir, paket yapılandırma dosyanız için erişim bilgileri, paket yürütme kimlik bilgileri/Özellikler ve günlük klasörünüz için erişim bilgileri.  **Paket yürütme yardımcı programı** Iletişim kutusunun **değerleri ayarla** sayfası, geçersiz kılmak için mevcut paket özelliklerinizi girebileceğiniz SSIS paketi yürütme etkinliğinin **geçersiz kılmaları** sekmesine karşılık gelir. Daha fazla bilgi için bkz. [ADF işlem hatları içinde SSIS paketlerini yürütme SSIS paketi etkinlikleri](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

**Paket yürütme yardımcı programı** Iletişim kutusunun **Yürüt** düğmesini seçtiğinizde, SSIS paketi yürütme ETKINLIĞINE sahip yeni bir ADF işlem hattı otomatik olarak oluşturulup tetiklenir. Aynı paket yürütme ayarlarına sahip bir ADF işlem hattı zaten varsa, yeniden çalıştırılır ve yeni bir işlem hattı oluşturulmaz. ADF işlem hattı ve SSIS paketi yürütme etkinliği `Pipeline_SSMS_YourPackageName_HashString` sırasıyla ve olarak adlandırılır `Activity_SSMS_YourPackageName` .

![Paket Yürütme Yardımcı Programı düğmesi](media/azure-ssis-integration-runtime-package-store/ssms-package-store-execute3.png)

![SSIS paketi yürütme etkinliği](media/azure-ssis-integration-runtime-package-store/ssis-activity-package-store.png)

## <a name="monitor-and-stop-running-packages"></a>Çalışan paketleri izleme ve durdurma

SSMS 'de Azure-SSIS IR gözatarken, çalışmakta olan paketleri görmek için **çalışan paketler** düğümünü genişletebilirsiniz.  Bir menü açmak için bunlardan birine sağ tıklayın ve **Durdur** veya **Yenile**' yi seçin.

   ![Çalışan paketleri izleme ve durdurma](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor.png)

   *  Paketi, SSIS paketi yürütme etkinliğini çalıştıran, çalışmakta olan ADF ardışık düzenini iptal etmek için **Durdur** ' u seçin.

   *  Paket mağazalarınızın yeni çalışan paketlerini göstermek için **Yenile** ' yi seçin.

## <a name="monitor-azure-ssis-ir-and-edit-package-stores"></a>Azure-SSIS IR izleme ve paket depolarını düzenleme

SSMS 'de Azure-SSIS IR gözatarken, bir menü açmak için sağ tıklayıp **Azure Data Factory portala git** veya **Yenile**' yi seçebilirsiniz.

   ![ADF portalına git](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor2.png)

   *  Azure-SSIS IR izleyebileceğiniz ADF izleme hub 'ının **tümleştirme çalışma zamanları** sayfasını açmak için **Azure Data Factory portalına git** ' i seçin. **Paket depoları** kutucuğunda, Azure-SSIS IR bağlı paket deposunun sayısını görebilirsiniz.  Bu numarayı seçtiğinizde, paket depolarınız için erişim bilgilerini depolayan ADF bağlantılı hizmetleri düzenleyebileceğiniz bir pencere açılır.

      ![Paket depolarını Düzenle](media/azure-ssis-integration-runtime-package-store/ssms-package-store-monitor3.png)

   *  Paketinizdeki yeni eklenen klasörleri/paketleri göstermek için **Yenile** ' yi seçin ve paketleri paket mağazalarından depolar ve çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Otomatik olarak oluşturulan ADF işlem hatlarını, SSIS paketi etkinliklerini yürütün veya ADF portalında yeni bir tane oluşturabilirsiniz. Daha fazla bilgi için bkz. [ADF işlem hatları içinde SSIS paketlerini yürütme SSIS paketi etkinlikleri](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).