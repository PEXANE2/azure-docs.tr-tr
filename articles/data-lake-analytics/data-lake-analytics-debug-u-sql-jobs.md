---
title: Azure Veri Gölü U-SQL işleri için Hata Ayıklama C# kodu
description: Bu makalede, Visual Studio için Azure Veri Gölü Araçları'nı kullanarak U-SQL başarısız bir tepe noktasının nasıl hata ayıklanması açıklanmaktadır.
services: data-lake-analytics
ms.service: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: bcd0b01e-1755-4112-8e8a-a5cabdca4df2
ms.topic: conceptual
ms.date: 11/30/2017
ms.openlocfilehash: 72239fc1679d2ebbfd9c9b5be6b79b58efb760cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71315810"
---
# <a name="debug-user-defined-c-code-for-failed-u-sql-jobs"></a>Başarısız U-SQL işleri için kullanıcı tanımlı C# kodu hata ayıklama

U-SQL C# kullanarak genişletilebilirlik modeli sağlar. U-SQL komut dosyalarında C# işlevlerini çağırmak ve SQL benzeri bildirimdilinin desteklemediği analitik işlevleri gerçekleştirmek kolaydır. U-SQL genişletilebilirliği hakkında daha fazla bilgi edinmek için [U-SQL programlanabilirlik kılavuzuna](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-u-sql-programmability-guide#use-user-defined-functions-udf)bakın. 

Uygulamada, herhangi bir kodun hata ayıklama gerekebilir, ancak sınırlı günlük dosyaları ile bulutta özel kod ile dağıtılmış bir iş hata ayıklamak zordur. [Visual Studio için Azure Veri Göl Araçları,](https://aka.ms/adltoolsvs) özel kodunuzda oluşan hataları daha kolay hata ayıklamanıza yardımcı olan **Failed Vertex Hata Ayıklama**adlı bir özellik sağlar. U-SQL işi başarısız olduğunda, hizmet hata durumunu korur ve araç hata ayıklama için bulut hatası ortamını yerel makineye indirmenize yardımcı olur. Yerel indirme, tüm giriş verileri ve kullanıcı kodu da dahil olmak üzere tüm bulut ortamını yakalar.

Aşağıdaki videoda, Visual Studio için Azure Veri Gölü Araçları'nda Başarısız Vertex Hata Ayıklama gösterilmiştir.

> [!VIDEO https://www.youtube.com/embed/3enkNvprfm4]
>

> [!IMPORTANT]
> Visual Studio bu özelliği kullanmak için aşağıdaki iki güncelleştirmeyi gerektirir: [Microsoft Visual C++ 2015 Yeniden Dağıtılabilir Güncelleştirme 3](https://www.microsoft.com/en-us/download/details.aspx?id=53840) ve Windows için Evrensel C Çalışma [Süresi.](https://www.microsoft.com/download/details.aspx?id=50410)
>

## <a name="download-failed-vertex-to-local-machine"></a>Başarısız vertex'i yerel makineye indirin

Visual Studio için Azure Veri Göl Araçları'nda başarısız bir iş açtığınızda, hata sekmesinde ayrıntılı hata iletileri içeren sarı bir uyarı çubuğu görürsünüz.

1. Gerekli tüm kaynakları ve giriş akışlarını indirmek için **İndir'i** tıklatın. İndirme tamamlanmazsa, **Yeniden Deneyin'i**tıklatın.

2. Yerel hata ayıklama ortamı oluşturmak için karşıdan yükleme tamamlandıktan sonra **Aç'ı** tıklatın. Yeni bir hata ayıklama çözümü açılacak ve Visual Studio'da açılmış bir çözüm varsa, lütfen hata ayıklamadan önce bunu kaydettiğinizden ve kapattığınızdan emin olun.

![Azure Veri Gölü Analytics U-SQL hata ayıklama visual studio indir vertex](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

## <a name="configure-the-debugging-environment"></a>Hata ayıklama ortamını yapılandırma

> [!NOTE]
> Hata ayıklamadan önce, Özel Durum Ayarları penceresinde **(Ctrl + Alt + E)** **Ortak Dil Çalışma Zamanı Özel Durumlarını** kontrol ettiğinizden emin olun.

![Azure Veri Gölü Analizi U-SQL hata ayıklama görsel stüdyo ayarı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)

Yeni başlatılan Visual Studio örneğinde, kullanıcı tarafından tanımlanan C# kaynak kodunu bulabiliyor veya bulamayabilirsiniz:

1. [Çözümde kaynak kodumu bulabilirim](#source-code-is-included-in-debugging-solution)

2. [Çözümde kaynak kodumu bulamıyorum](#source-code-is-not-included-in-debugging-solution)

### <a name="source-code-is-included-in-debugging-solution"></a>Kaynak kodu hata ayıklama çözümüne dahildir

C# kaynak kodunun yakalandığı iki durum vardır:

1. Kullanıcı kodu kod arkası dosyasında tanımlanır (genellikle bir U-SQL projesinde adlandırılır). `Script.usql.cs`

2. Kullanıcı kodu U-SQL uygulaması için C# sınıfı kitaplık projesinde tanımlanır ve **hata ayıklama bilgisi**ile derleme olarak kaydedilir.

Kaynak kodu çözüme aktarılırsa, sorunu gidermek için Visual Studio hata ayıklama araçlarını (saat, değişkenler, vb.) kullanabilirsiniz:

1. Hata ayıklamaya başlamak için **F5**'e basın. Kod, bir özel durum tarafından durdurulana kadar çalışır.

2. Kaynak kodu dosyasını açın ve kesme noktaları ayarlayın, ardından kodu adım adım hata ayıklamak için **F5** tuşuna basın.

    ![Azure Veri Gölü Analizi U-SQL hata ayıklama özel durumu](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-exception.png)

### <a name="source-code-is-not-included-in-debugging-solution"></a>Kaynak kodu hata ayıklama çözümüne dahil edilmez

Kullanıcı kodu kod arkası dosyasına dahil edilmediyse veya derlemeyi **hata ayıklama bilgileriyle**kaydetmediyseniz, kaynak kodu hata ayıklama çözümüne otomatik olarak dahil edilmez. Bu durumda, kaynak kodunuzu eklemek için ek adımlar gerekir:

1. Derleme kaynak kodunu bulmak ve projeyi hata ayıklama çözümüne eklemek için Solution **'VertexDebug' > Varolan Proje> ekle...** seçeneğini sağ tıklatın.

    ![Azure Veri Gölü Analizi U-SQL hata ayıklama projesi ekleme](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-add-project-to-debug-solution.png)

2. **FailedVertexDebugHost** projesi için proje klasörü yolunu alın. 

3. **Eklenen derleme kaynak kodu projesi> Özellikleri'ni**sağ tıklatın, solda **Yap** sekmesini seçin ve Çıktı > **Çıkış yolu**olarak \bin\debug ile biten kopyalanan yolu yapıştırın. Son çıkış yolu `<DataLakeTemp path>\fd91dd21-776e-4729-a78b-81ad85a4fba6\loiu0t1y.mfo\FailedVertexDebug\FailedVertexDebugHost\bin\Debug\`gibi.

    ![Azure Veri Gölü Analizi U-SQL hata ayıklama kümesi pdb yolu](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-set-pdb-path.png)

Bu ayarlardan sonra, **F5** ve kesme noktaları ile hata ayıklamaya başlayın. Sorunu gidermek için Visual Studio hata ayıklama araçlarını (saat, değişkenler, vb.) de kullanabilirsiniz.

> [!NOTE]
> Güncelleştirilmiş .pdb dosyaları oluşturmak için kodu değiştirdikten sonra her seferinde derleme kaynak kodu projesini yeniden oluşturun.

## <a name="resubmit-the-job"></a>İşi yeniden gönderme

Hata ayıklamadan sonra, proje başarıyla tamamlanırsa çıktı penceresi aşağıdaki iletiyi gösterir:

    The Program 'LocalVertexHost.exe' has exited with code 0 (0x0).

![Azure Veri Gölü Analizi U-SQL hata ayıklama başarılı](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-debug-succeed.png)

Başarısız işi yeniden göndermek için:

1. Kod arkası çözümleriolan işler için C# kodunu kod arkasındaki kaynak dosyaya kopyalayın `Script.usql.cs`(genellikle).

2. Derlemeli işler için hata ayıklama çözümünde derleme kaynak kodu projesini sağ tıklatın ve güncelleştirilmiş .dll derlemelerini Azure Veri Gölü kataloğunuza kaydedin.

3. U-SQL işini yeniden gönderin.

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL programlanabilirlik kılavuzu](data-lake-analytics-u-sql-programmability-guide.md)
- [Azure Veri Gölü Analizi işleri için U-SQL Kullanıcı tanımlı operatörler geliştirme](data-lake-analytics-u-sql-develop-user-defined-operators.md)
- [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)
- [Anormal yinelenen bir iş sorun giderme](data-lake-analytics-data-lake-tools-debug-recurring-job.md)
