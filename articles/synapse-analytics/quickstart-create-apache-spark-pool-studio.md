---
title: "Hızlı başlangıç: SYNAPSE Studio 'Yu kullanarak Apache Spark Havuzu (Önizleme) oluşturma"
description: Bu kılavuzdaki adımları izleyerek SYNAPSE Studio kullanarak yeni bir Apache Spark havuzu oluşturun.
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: spark
ms.date: 3/19/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 87fcbef5a3cedab3142fe9ad47c68752515fdd2e
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274348"
---
# <a name="quickstart-create-an-apache-spark-pool-preview-using-synapse-studio"></a>Hızlı başlangıç: SYNAPSE Studio 'Yu kullanarak Apache Spark Havuzu (Önizleme) oluşturma

Azure SYNAPSE Analytics, verilerinizi almanıza, dönüştürmenizi, modellemenizi, çözümlemenize ve sungetirmenize yardımcı olmak için çeşitli analiz motorları sunmaktadır. Apache Spark havuz, açık kaynak büyük veri işlem yetenekleri sunar. SYNAPSE çalışma alanınızda bir Apache Spark havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve öngörüleri elde etmek için sunulabilir.  

Bu hızlı başlangıçta SYNAPSE Studio kullanarak bir Synapse çalışma alanında Apache Spark havuzu oluşturma adımları açıklanmaktadır.

> [!IMPORTANT]
> Spark örnekleri için faturalandırma, bunları kullanıp kullanmayacağınızı de dakika başına eşit olarak dağıtılır. Kullanmayı bitirdikten sonra Spark örneğinizi kapattığınızdan emin olun veya kısa bir zaman aşımı ayarlayın. Daha fazla bilgi için bu makalenin **Kaynakları temizleme** bölümüne bakın.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [SYNAPSE çalışma alanı](./quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın

## <a name="navigate-to-the-synapse-workspace"></a>SYNAPSE çalışma alanına gidin

1. Arama çubuğuna hizmet adı (veya doğrudan kaynak adı) yazarak Apache Spark havuzunun oluşturulacağı SYNAPSE çalışma alanına gidin.
![İçinde SYNAPSE çalışma alanlarıyla yazılan arama çubuğu Azure portal.](media/quickstart-create-sql-pool/create-sql-pool-00a.png)
1. Çalışma alanları listesinden, açmak için çalışma alanının adını (veya adının bir bölümünü) yazın. Bu örnekte, **contosoanalytics**adlı bir çalışma alanı kullanacağız.
![Contoso adını içeren bunları göstermek üzere filtrelenmiş SYNAPSE çalışma alanlarının listesi.](media/quickstart-create-sql-pool/create-sql-pool-00b.png)

## <a name="launch-synapse-studio"></a>Synapse Studio'yu başlatma 

1. Çalışma alanına genel bakış ' da, Apache Spark havuzunun oluşturulacağı konumu açmak için **SYNAPSE Studio 'Yu Başlat** ' ı seçin. Hizmet adını veya kaynak adını doğrudan arama çubuğuna yazın.
![Azure portal SYNAPSE çalışma alanına genel bakış ve Launch SYNAPSE Studio vurgulanmış.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-20.png)

## <a name="create-the-apache-spark-pool-in-synapse-studio"></a>SYNAPSE Studio 'da Apache Spark havuzunu oluşturma

1. SYNAPSE Studio giriş sayfasında, **Yönet** simgesini seçerek sol gezinti bölmesinde **yönetim merkezine** gidin.
![Yönetim Merkezi bölümü vurgulanmış olan SYNAPSE Studio giriş sayfası.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-21.png)

1. Yönetim hub 'ında bir kez, çalışma alanında kullanılabilen Apache Spark havuzlarının geçerli listesini görmek için **Apache Spark havuzları** bölümüne gidin.
![Apache Spark havuzları gezintisi seçiliyken SYNAPSE Studio Yönetim Merkezi](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-22.png)

1. **+ Yeni** ' yi seçin ve yeni Apache Spark havuzu oluşturma Sihirbazı görüntülenir. 
![Spark havuzlarının SYNAPSE Studio yönetim merkezi listesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-23.png)

1. **Temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **Apache Spark havuzu adı** | contosospark | Bu, Apache Spark havuzunun sahip olacağı addır. |
    | **Düğüm boyutu** | Küçük (4 vCPU/32 GB) | Bu hızlı başlangıç için maliyetleri azaltmak üzere en küçük boyuta ayarlayın |
    | **Otomatik Ölçeklendirme** | Devre dışı | Bu hızlı başlangıçta otomatik ölçeklendirme gerekmiyor |
    | **Düğüm sayısı** | 8 | Bu hızlı başlangıçta maliyetleri sınırlandırmak için küçük bir boyut kullanın|
    
    ![SYNAPSE Studio yeni Apache Spark havuz formu.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-24.png)
    > [!IMPORTANT]
    > Apache Spark havuzların kullanabileceği adlarla ilgili özel sınırlamalar olduğunu unutmayın. Adlar yalnızca harf veya sayı içermelidir, 15 veya daha az karakter olmalıdır, bir harfle başlamalı, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

1. Sonraki sekmede (ek ayarlar), tüm varsayılan değerleri bırakın ve **gözden geçir + oluştur** ' a basın (herhangi bir etiket eklemeiyoruz).
 ![SYNAPSE Studio yeni Apache Spark havuz formu.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-25.png)

1. Şimdilik hiç etiket eklememiz, bu nedenle **gözden geçir + oluştur**' u seçin.

1. **Gözden geçir + oluştur** sekmesinde, ayrıntıların daha önce girilmiş olan öğesine göre doğru göründüğünden emin olun ve **Oluştur**' a basın. 
 ![SYNAPSE Studio yeni Apache Spark havuz formu.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-26.png)

1. Apache Spark havuz, sağlama işlemini başlatacak.
![SYNAPSE Studio yeni Apache Spark havuz formu.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-27.png)

1. Sağlama tamamlandıktan sonra yeni Apache Spark havuzu listede görüntülenir.
![SYNAPSE Studio yeni Apache Spark havuz formu.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-28.png)

## <a name="clean-up-apache-spark-pool-resources-using-synapse-studio"></a>SYNAPSE Studio 'Yu kullanarak Apache Spark havuzu kaynaklarını Temizleme

Apache Spark havuzunu SYNAPSE Studio kullanarak çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Spark havuzunu silme, analiz altyapısını çalışma alanından kaldırır. Havuza bağlanmayacak ve bu Spark havuzunu kullanan tüm sorgular, işlem hatları ve Not defterleri artık çalışmayacak.

Apache Spark havuzunu silmek istiyorsanız, aşağıdakileri yapın:

1. SYNAPSE Studio 'daki yönetim hub 'ındaki Apache Spark havuzlarına gidin.
1. Apache Spark havuzunun komutlarını göstermek için, silinecek Apache havuzunun yanındaki üç nokta simgesini (Bu örnekte, **contosospark**) seçin.
![Son oluşturulan havuz seçiliyken Apache Spark havuzlarının listelenmesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-29.png)
1. **Sil**'e basın.
1. Silmeyi onaylayın ve **Sil** düğmesine basın.
 ![Seçili Apache Spark havuzunu silmek için onay iletişim kutusu.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-30.png)
1. İşlem başarıyla tamamlandığında, Apache Spark havuzu artık çalışma alanı kaynaklarında listelenmeyecektir. 

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [hızlı başlangıç: Web araçlarını kullanarak SYNAPSE Studio 'da Apache Spark havuzu oluşturma](quickstart-apache-spark-notebook.md).
- Bkz. [hızlı başlangıç: Azure Portal kullanarak Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md).