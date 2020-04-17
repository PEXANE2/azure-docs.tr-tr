---
title: Şirket içi SQL Server Tümleştirme Hizmetleri (SSIS) işlerini Azure Veri Fabrikası'na taşıyın
description: Bu makalede, SQL Server Management Studio'u kullanarak SQL Server Integration Services (SSIS) işlerinin Azure Veri Fabrikası ardışık hatlarına/etkinliklerine/tetikleyicilerine nasıl geçirilen ler açıklanmaktadır.
services: data-factory
documentationcenter: ''
author: chugugrace
ms.author: chugu
ms.reviewer: ''
manager: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 4/7/2020
ms.openlocfilehash: 6e357e98d6c5190c6dfef675dc1ab9cf30a717c1
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81455096"
---
# <a name="migrate-sql-server-agent-jobs-to-adf-with-ssms"></a>SQL Server Agent işlerini SSMS ile ADF'ye geçirin

Şirket [içi SQL Server Integration Services (SSIS) iş yüklerini ADF'de SSIS'e aktarırken,](scenario-ssis-migration-overview.md)SSIS paketleri geçtikten sonra, SQL Server Integration Services Package iş adım türüne sahip SQL Server Agent işlerinin toplu geçişini, SQL Veri Fabrikası (ADF) ardışık hatlarına/etkinlikleri/zamanlama tetikleyicilerine SQL Management Studio (SSMS) **SSIS İş Geçişi Sihirbazı**üzerinden gerçekleştirebilirsiniz.

Genel olarak, geçerli iş adımı türlerine sahip seçili SQL aracısı işleri için **SSIS İş Geçişi Sihirbazı** şunları yapabilir:

- ADF'de SSIS tarafından erişilebilen paketlerin geçirildiği yere şirket içi SSIS paket konumunu haritalandırın.
    > [!NOTE]
    > Dosya Sisteminin paket konumu yalnızca desteklenir.
- aşağıdaki gibi ilgili ADF kaynaklarına uygulanabilir iş adımlarını ile ilgili işleri geçirin:

|SQL Agent iş nesnesi  |ADF kaynağı  |Notlar|
|---------|---------|---------|
|SQL Agent işi|Boru hattı     |Boru hattının adı *iş \<adı>için oluşturulacaktır. * <br> <br> Yerleşik aracı işleri geçerli değildir: <li> SSIS Server Bakım İşi <li> syspolicy_purge_history <li> collection_set_* <li> mdw_purge_data_* <li> sysutility_*|
|SSIS iş adımı|SSIS paket etkinliğini yürütme|<li> Etkinliğin adı adım \<adı> olacaktır. <li> İş adımında kullanılan proxy hesabı, bu etkinliğin Windows kimlik doğrulaması olarak geçirilir. <li> İş adımında tanımlanan *32 bit çalışma süresi dışındaki* yürütme *seçenekleri* geçişte yoksayılır. <li> İş adımında tanımlanan *doğrulama* geçişte yoksayılır.|
|schedule      |zamanlama tetikleyicisi        |Zamanlama tetikleyicisinin adı *zamanlama \<adı>için oluşturulur. * <br> <br> SQL Agent iş zamanlamasındaki aşağıdaki seçenekler geçişte göz ardı edilecektir: <li> İkinci düzey aralık. <li> *SQL Server Agent başlatıldığında otomatik olarak başlatın* <li> *CPU'lar boşta kaldığında başlayın* <li> *hafta içi* ve *hafta sonu günü*<time zone> <br> SQL Agent iş zamanlaması ADF zamanlama tetikleyicisine geçirildikten sonra farklar aşağıda verilmiştir: <li> ADF Zamanlama Tetikleyici sonraki çalıştırma, öncül tetiklenen çalıştırmanın yürütme durumundan bağımsızdır. <li> ADF Schedule Trigger yineleme yapılandırması SQL aracısı işinde Günlük frekanstan farklıdır.|

- Yerel çıktı klasöründe Azure Kaynak Yöneticisi (ARM) şablonları oluşturun ve doğrudan veya daha sonra el ile veri fabrikasına dağıtın. ADF Kaynak Yöneticisi şablonları hakkında daha fazla bilgi için [Microsoft.DataFactory kaynak türlerine](https://docs.microsoft.com/azure/templates/microsoft.datafactory/allversions)bakın.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan özellik, SQL Server Management Studio sürüm 18.5 veya üzeri gerektirir. SSMS'in en son sürümünü almak için SQL [Server Management Studio 'yı (SSMS) indirin](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)bölümüne bakın.

## <a name="migrate-ssis-jobs-to-adf"></a>SSIS işlerini ADF'ye geçirin

1. SSMS'te, Object Explorer'da SQL Server Agent'ı seçin, İşler'i seçin, ardından **SSIS İşlerini ADF'ye**geçir'i sağ tıklatın.
![Menü](media/how-to-migrate-ssis-job-ssms/menu.png)

1. Azure'da Oturum Aç, Azure Aboneliği, Veri Fabrikası ve Tümleştirme Çalışma Zamanı'nı seçin. Azure Depolama isteğe bağlıdır ve paket konum eşleme adımında kullanılır, eğer ssis işleri taşınırsa SSIS Dosya Sistemi paketleri vardır.
![Menü](media/how-to-migrate-ssis-job-ssms/step1.png)

1. SSIS işlerinden sis paketlerinin ve yapılandırma dosyalarının yollarını, geçirilen ardışık yolların erişebileceği hedef yollara eşleyin. Bu eşleme adımında şunları yapabilirsiniz:

    1. Bir kaynak klasör seçin, ardından **Eşleme Ekle.**
    1. Kaynak klasör yolunu güncelleştirin. Geçerli yollar klasör yolları veya paketlerin üst klasör yollarıdır.
    1. Hedef klasör yolunu güncelleştirin. Varsayılan değer, adım 1'de seçilen varsayılan Depolama hesabına göreli yoldur.
    1. Seçili eşlemi **Sil Eşlemi'ni**sil ile silin.
![adım2](media/how-to-migrate-ssis-job-ssms/step2.png)
![adım2-1](media/how-to-migrate-ssis-job-ssms/step2-1.png)

1. Geçiş yapmak için geçerli işleri seçin ve ilgili *SSIS Paketi etkinliğinin*ayarlarını yapılandırın.

    - *Varsayılan Ayar,* varsayılan olarak seçilen tüm adımlar için geçerlidir. Her özellik hakkında daha fazla bilgi için, paket konumu *Dosya Sistemi (Paket)* olduğunda [SSIS Paketi etkinliğini yürütecek](how-to-invoke-ssis-package-ssis-activity.md) *Ayarlar sekmesine* bakın.
    ![adım3-1](media/how-to-migrate-ssis-job-ssms/step3-1.png)
    - *Adım Ayarı,* seçili bir adım için ayar yapılandırır.
        
        **Varsayılan Ayar Uygula**: varsayılan seçilir. Yalnızca seçili adım için ayar yapılandırmak için seçin.  
        Diğer özellikler hakkında daha fazla bilgi için, paket konumu *Dosya Sistemi (Paket)* olduğunda [SSIS Paketi etkinliğini yürütecek](how-to-invoke-ssis-package-ssis-activity.md) *Ayarlar sekmesine* bakın.
    ![adım3-2](media/how-to-migrate-ssis-job-ssms/step3-2.png)

1. ARM şablonu oluşturun ve dağıtın.
    1. Geçirilen ADF ardışık hatlarının ARM şablonları için çıkış yolunu seçin veya girdi. Klasör yoksa otomatik olarak oluşturulur.
    2. **ARM şablonlarını veri fabrikanıza dağıtma**seçeneğini seçin:
        - Varsayılan, seçili değil. Oluşturulan ARM şablonlarını daha sonra el ile dağıtabilirsiniz.
        - Oluşturulan ARM şablonlarını doğrudan veri fabrikasına dağıtmak için seçin.
    ![adım4](media/how-to-migrate-ssis-job-ssms/step4.png)

1. Geçirin, sonra sonuçları kontrol edin.
![adım5](media/how-to-migrate-ssis-job-ssms/step5.png)

## <a name="next-steps"></a>Sonraki adımlar

[Boru hattını çalıştırma ve izleme](how-to-invoke-ssis-package-ssis-activity.md)
