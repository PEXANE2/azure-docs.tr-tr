---
title: SSIS kullanarak Azure Veri Gölü Analizi U-SQL işlerini zamanlama
description: U-SQL işlerini satır içi komut dosyasıyla veya U-SQL sorgu dosyalarından planlamak için SQL Server Tümleştirme Hizmetlerini nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 07/17/2018
ms.openlocfilehash: 0650fcc5023ac57b193fa23b0dedf65113fd64e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71672901"
---
# <a name="schedule-u-sql-jobs-using-sql-server-integration-services-ssis"></a>SQL Server Tümleştirme Hizmetlerini (SSIS) kullanarak U-SQL işlerini zamanlama

Bu belgede, SQL Server Integration Service (SSIS) kullanarak U-SQL işlerini nasıl düzenleyip oluşturabileceğinizi öğrenirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

Azure Veri Gölü Analizi [Hizmeti](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017) için Azure Özellik [Paketi,](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017#scenario-managing-data-in-the-cloud) Azure Veri Gölü Analizi görevini ve Azure Veri Gölü Analizi [Bağlantı Yöneticisi'ni](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager?view=sql-server-2017) sağlar. Bu görevi kullanmak için şunları yüklediğinizden emin olun:

- [Visual Studio için SQL Server Veri Araçları (SSDT) indirin ve yükleyin](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017)
- [Tümleştirme Hizmetleri (SSIS) için Azure Özellik Paketi'ni yükleme](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017)

## <a name="azure-data-lake-analytics-task"></a>Azure Veri Gölü Analizi görevi

Azure Veri Gölü Analizi görevi, kullanıcıların U-SQL işlerini Azure Veri Gölü Analizi hesabına göndermelerine izin verir. 

[Azure Veri Gölü Analizi görevini nasıl yapılandırılatırmayı öğrenin.](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017)

![SSIS'te Azure Veri Gölü Analizi Görevi](./media/data-lake-analytics-schedule-jobs-ssis/data-lake-analytics-azure-data-lake-analytics-task-in-ssis.png)

SSIS yerleşik işlevlerini ve görevlerini kullanarak U-SQL komut dosyasını farklı yerlerden alabilirsiniz, aşağıdaki senaryolar farklı kullanıcı durumları için U-SQL komut dosyalarını nasıl yapılandırabileceğinizi gösterir.

## <a name="scenario-1-use-inline-script-call-tvfs-and-stored-procs"></a>Senaryo 1-Satır içi komut çağrı tvf'leri ve depolanmış proc'ları kullanma

Azure Veri Gölü Analizi Görev Düzenleyicisi'nde **SourceType'ı** **DirectInput**olarak yapılandırın ve U-SQL deyimlerini **USQLStatement'a**koyun.

Kolay bakım ve kod yönetimi için, yalnızca kısa U-SQL komut dosyasını satır içi komut dosyası olarak koyun, örneğin, varolan tablo değerli işlevleri ve u-SQL veritabanlarınızda depolanan yordamları çağırabilirsiniz. 

![SSIS görevinde satır içi U-SQL komut dosyasını düzenle](./media/data-lake-analytics-schedule-jobs-ssis/edit-inline-usql-script-in-ssis.png)

İlgili makale: [Depolanan yordamlara parametre nasıl geçirilir?](#scenario-6-pass-parameters-to-u-sql-script)

## <a name="scenario-2-use-u-sql-files-in-azure-data-lake-store"></a>Senaryo 2-Azure Veri Gölü Deposu'nda U-SQL dosyalarını kullanma

Azure Özellik Paketi'nde **Azure Veri Gölü Deposu'ndaki** U-SQL dosyalarını da kullanabilirsiniz. Bu yaklaşım, bulutta depolanan komut dosyalarını kullanmanıza olanak tanır.

Azure Veri Gölü Deposu Dosya Sistemi Görevi ile Azure Veri Gölü Analizi Görevi arasındaki bağlantıyı kurmak için aşağıdaki adımları izleyin.

### <a name="set-task-control-flow"></a>Görev kontrol akışını ayarlama

SSIS paket tasarım görünümünde, Foreach Loop Kapsayıcısı'na bir **Azure Veri Gölü Deposu Dosya Sistemi Görevi,** **Foreach Loop Kapsayıcısı** ve **Bir Azure Veri Gölü Analizi Görevi** ekleyin. Azure Veri Gölü Deposu Dosya Sistemi Görevi, ADLS hesabınızdaki U-SQL dosyalarını geçici bir klasöre indirmenize yardımcı olur. Foreach Loop Kapsayıcısı ve Azure Veri Gölü Analizi Görevi, geçici klasör altındaki her U-SQL dosyasını Azure Veri Gölü Analytics hesabına U-SQL işi olarak göndermeye yardımcı olur.

![Azure Veri Gölü Deposu'nda U-SQL dosyalarını kullanma](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-data-lake-store.png)

### <a name="configure-azure-data-lake-store-file-system-task"></a>Azure Veri Gölü Depolama Dosya Sistemi Görevini Yapılandırma

1. **İşlemi** **CopyFromADLS**olarak ayarlayın.
2. **AzureDataLakeConnection'ı**kurun, [Azure Veri Gölü Deposu Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager?view=sql-server-2017)hakkında daha fazla bilgi edinin.
3. **AzureDataLakeDirectory'yi**ayarlayın. U-SQL komut dosyalarınızı depolayan klasöre işaret edin. Azure Veri Gölü Deposu hesap kökü klasörüne göregöreli yolu kullanın.
4. **Hedef'i** indirilen U-SQL komut dosyalarını önbelleğe alan bir klasöre ayarlayın. Bu klasör yolu, U-SQL iş gönderimi için Foreach Loop Container'ta kullanılacaktır. 

![Azure Veri Gölü Depolama Dosya Sistemi Görevini Yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-azure-data-lake-store-file-system-task.png)

[Azure Veri Gölü Depolama Dosya Sistemi Görevi hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-store-file-system-task?view=sql-server-2017)

### <a name="configure-foreach-loop-container"></a>Foreach Loop Konteynerini Yapılandır

1. **Toplama** sayfasında, **Enumerator'u** **Foreach File Enumerator**olarak ayarlayın.

2. **Klasörü** **Enumerator yapılandırma** grubu altında indirilen U-SQL komut dosyalarını içeren geçici klasöre ayarlayın.

3. **Dosyaları** **Enumerator yapılandırması** `*.usql` altında, döngü kapsayıcısının yalnızca `.usql`' ile biten dosyaları yakalar şekilde ayarlayın

    ![Foreach Loop Konteynerini Yapılandır](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-collection.png)

4. **Değişken Eşlemeler** sayfasında, her U-SQL dosyası için dosya adını almak için kullanıcı tanımlı bir değişken ekleyin. Dosya adını almak için **Dizin'i** 0 olarak ayarlayın. Bu örnekte, .. `User::FileName` Bu değişken, U-SQL komut dosyası bağlantısını dinamik olarak almak ve Azure Veri Gölü Analizi Görevi'nde U-SQL iş adını ayarlamak için kullanılır.

    ![Dosya adını almak için Foreach Loop Container'ı yapılandır](./media/data-lake-analytics-schedule-jobs-ssis/configure-foreach-loop-container-variable-mapping.png)

### <a name="configure-azure-data-lake-analytics-task"></a>Azure Veri Gölü Analizi Görevini Yapılandırma 

1. **SourceType'ı** **FileConnection'a**ayarlayın.

2. Foreach Loop Kapsayıcısı'ndan döndürülen dosya nesnelerini işaret eden dosya bağlantısına **DosyaBağlantısı'nı** ayarlayın.
    
    Bu dosya bağlantısını oluşturmak için:

   1. FileConnection ayarında ** \<Yeni Bağlantı...>'yi** seçin.
   2. **Kullanım türünü** **Varolan dosyaya**ayarlayın ve **Dosyayı** varolan herhangi bir dosyanın dosya yoluna ayarlayın.

       ![Foreach Loop Konteynerini Yapılandır](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

   3. **Bağlantı Yöneticileri** görünümünde, hemen şimdi oluşturulan dosya bağlantısını sağ tıklatın ve **Özellikler'i**seçin.

   4. **Özellikler** penceresinde, **İfadeleri**genişletin ve **ConnectionString'i** Foreach Loop Kapsayıcısı'nda tanımlanan değişkene ayarlayın, örneğin. `@[User::FileName]`

       ![Foreach Loop Konteynerini Yapılandır](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-property-for-foreach-loop-container.png)

3. İş göndermek istediğiniz Azure Data **LakeAnalyticsBağlantısını** Azure Veri Gölü Analytics hesabına ayarlayın. [Azure Veri Gölü Analytics Bağlantı Yöneticisi](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-analytics-connection-manager?view=sql-server-2017)hakkında daha fazla bilgi edinin.

4. Diğer iş yapılandırmalarını ayarlayın. [Daha fazla bilgi edinin.](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017)

5. U-SQL iş adını dinamik olarak ayarlamak için **İfadeleri** kullanın:

    1. **İfadeler** sayfasında, **JobName**için yeni bir ifade anahtar değeri çifti ekleyin.
    2. JobName değerini Foreach Loop Kapsayıcısı'nda tanımlanan değişkene `@[User::FileName]`ayarlayın, örneğin.
    
        ![U-SQL iş adı için SSIS İfadesini yapılandırma](./media/data-lake-analytics-schedule-jobs-ssis/configure-expression-for-u-sql-job-name.png)

## <a name="scenario-3-use-u-sql-files-in-azure-blob-storage"></a>Senaryo 3-Azure Blob Depolama'da U-SQL dosyalarını kullanma

Azure Özellik Paketi'nde **Azure Blob İndirme Görevi'ni** kullanarak Azure Blob Depolama'daki U-SQL dosyalarını kullanabilirsiniz. Bu yaklaşım, komut dosyalarını bulutta kullanmanızı sağlar.

Adımlar Senaryo 2 ile [benzer: Azure Veri Gölü Deposu'nda U-SQL dosyalarını kullanın.](#scenario-2-use-u-sql-files-in-azure-data-lake-store) Azure Veri Gölü Depolama Dosya Sistemi Görevi'ni Azure Blob İndirme Görevi olarak değiştirin. [Azure Blob İndirme Görevi hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task?view=sql-server-2017)

Kontrol akışı aşağıdaki gibidir.

![Azure Veri Gölü Deposu'nda U-SQL dosyalarını kullanma](./media/data-lake-analytics-schedule-jobs-ssis/use-u-sql-files-in-azure-blob-storage.png)

## <a name="scenario-4-use-u-sql-files-on-the-local-machine"></a>Senaryo 4-Yerel makinede U-SQL dosyalarını kullanma

Bulutta depolanan U-SQL dosyalarını kullanmanın yanı sıra, yerel makinenizdeki dosyaları veya SSIS paketlerinizle dağıtılan dosyaları da kullanabilirsiniz.

1. SSIS projesinde **Bağlantı Yöneticileri'ne** sağ tıklayın ve **Yeni Bağlantı Yöneticisi'ni**seçin.

2. **Dosya** türünü seçin ve **Ekle'yi tıklatın...**

3. **Kullanım türünü** **Varolan dosyaya**ayarlayın ve **Dosyayı** yerel makinede dosyaya ayarlayın.

    ![Yerel dosyaya dosya bağlantısı ekleme](./media/data-lake-analytics-schedule-jobs-ssis/configure-file-connection-for-foreach-loop-container.png)

4. **Azure Veri Gölü Analizi** Görevi ekleyin ve:
    1. **SourceType'ı** **FileConnection'a**ayarlayın.
    2. Hemen şimdi oluşturulan Dosya Bağlantısı'na **Dosya Bağlantısı'nı** ayarlayın.

5. Azure Veri Gölü Analizi Görevi için diğer yapılandırmaları tamamlayın.

## <a name="scenario-5-use-u-sql-statement-in-ssis-variable"></a>Senaryo 5-SSIS değişkeninde U-SQL deyimini kullanın

Bazı durumlarda, U-SQL deyimlerini dinamik olarak oluşturmanız gerekebilir. U-SQL deyimini dinamik olarak oluşturmanıza yardımcı olması için **SSIS** **Değişkeni'ni** ve Komut Dosyası Görevi gibi diğer SSIS görevlerini kullanabilirsiniz.

1. **SSIS > Değişkenler** üst düzey menüsüüzerinden Değişkenler araç pencereyi açın.

2. Bir SSIS Değişkeni ekleyin ve değeri doğrudan ayarlayın veya değeri oluşturmak için **Expression'ı** kullanın.

3. **Azure Veri Gölü Analizi Görevi** ekleyin ve:
    1. **SourceType'ı** **Değişkene**Ayarlayın.
    2. **SourceVariable'i** az şimdi oluşturulan SSIS Değişkenine ayarlayın.

4. Azure Veri Gölü Analizi Görevi için diğer yapılandırmaları tamamlayın.

## <a name="scenario-6-pass-parameters-to-u-sql-script"></a>Senaryo 6-U-SQL komut dosyasına geçme parametreleri

Bazı durumlarda, U-SQL komut dosyasındaki U-SQL değişken değerini dinamik olarak ayarlamak isteyebilirsiniz. Azure Veri Gölü Analizi Görevi'ndeki **Parametre Eşleme** özelliği bu senaryoya yardımcı olur. Genellikle iki tipik kullanıcı durumu vardır:

- Giriş ve çıktı dosyası yolu değişkenlerini geçerli tarih ve saate göre dinamik olarak ayarlayın.
- Depolanan yordamlar için parametreyi ayarlayın.

[U-SQL komut dosyası için parametrelerin nasıl ayarlanılabilenhakkında daha fazla bilgi edinin.](https://docs.microsoft.com/sql/integration-services/control-flow/azure-data-lake-analytics-task?view=sql-server-2017#parameter-mapping-page-configuration)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure’da SSIS paketlerini çalıştırma](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity)
- [Tümleştirme Hizmetleri için Azure Özellik Paketi (SSIS)](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017#scenario-managing-data-in-the-cloud)
- [Azure Veri Fabrikasını kullanarak U-SQL işlerini zamanlama](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics)
