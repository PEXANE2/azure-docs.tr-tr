---
title: 'Hızlı başlangıç: SYNAPSE Studio kullanarak adanmış bir adanmış SQL havuzu oluşturma'
description: Bu kılavuzdaki adımları izleyerek, SYNAPSE Studio kullanarak adanmış bir SQL havuzu oluşturun.
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql
ms.date: 10/16/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 6ffa7bb07ddb42946acdcef6ee3b7f2aa804f774
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93324322"
---
# <a name="quickstart-create-a-dedicated-sql-pool-using-synapse-studio"></a>Hızlı başlangıç: SYNAPSE Studio kullanarak adanmış bir SQL havuzu oluşturma

Azure SYNAPSE Analytics, verilerinizi almak, dönüştürmek, modellemek ve analiz etmenize yardımcı olmak için çeşitli analiz motorları sunmaktadır. Adanmış bir SQL havuzu, T-SQL tabanlı işlem ve depolama olanakları sunar. SYNAPSE çalışma alanınızda adanmış bir SQL havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve daha hızlı analitik Öngörüler için teslim edilebilir.

Bu hızlı başlangıçta, SYNAPSE Studio kullanarak bir Synapse çalışma alanında adanmış bir SQL havuzu oluşturma adımları açıklanmaktadır.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).


## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [SYNAPSE çalışma alanı](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalda](https://portal.azure.com/) oturum açma

## <a name="navigate-to-the-synapse-workspace"></a>SYNAPSE çalışma alanına gidin

1. Arama çubuğuna hizmet adı (veya doğrudan kaynak adı) yazarak adanmış SQL havuzunun oluşturulacağı SYNAPSE çalışma alanına gidin.

    ![İçinde SYNAPSE çalışma alanlarıyla yazılan arama çubuğu Azure portal.](media/quickstart-create-sql-pool/create-sql-pool-00a.png)
1. Çalışma alanları listesinden, açmak için çalışma alanının adını (veya adının bir bölümünü) yazın. Bu örnekte, **contosoanalytics** adlı bir çalışma alanı kullanacağız.

    ![Contoso adını içeren bunları göstermek üzere filtrelenmiş SYNAPSE çalışma alanlarının listesi.](media/quickstart-create-sql-pool/create-sql-pool-00b.png)

## <a name="launch-synapse-studio"></a>Synapse Studio'yu başlatma

1. Çalışma alanına genel bakış ' da, SYNAPSE Studio 'Yu başlatmak için **çalışma alanı Web URL** 'sini seçin.

    ![Çalışma alanı Web URL 'SI vurgulanmış şekilde Azure portal SYNAPSE çalışma alanına genel bakış.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-20.png)

## <a name="create-a-dedicated-sql-pool-in-synapse-studio"></a>SYNAPSE Studio 'da adanmış bir SQL havuzu oluşturma

1. SYNAPSE Studio giriş sayfasında, **Yönet** simgesini seçerek sol gezinti bölmesinde **yönetim merkezine** gidin.

    ![Yönetim Merkezi bölümü vurgulanmış olan SYNAPSE Studio giriş sayfası.](media/quickstart-create-apache-spark-pool/create-spark-pool-studio-21.png)

1. Yönetim hub 'ında bir kez, çalışma alanında kullanılabilir olan SQL havuzlarının güncel listesini görmek için **SQL havuzları** bölümüne gidin.

    ![SQL havuzları gezintisi seçiliyken SYNAPSE Studio Yönetim Merkezi](media/quickstart-create-sql-pool/create-sql-pool-studio-22.png)

1. **+ Yeni** komut ' yi seçin ve yeni SQL havuzu oluşturma Sihirbazı görüntülenir. 

    ![SQL havuzlarının SYNAPSE Studio yönetim merkezi listesi.](media/quickstart-create-sql-pool/create-sql-pool-studio-23.png)

1. **Temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **SQL havuzu adı** | contosoedw | Bu, adanmış SQL havuzunun sahip olacağı addır. |
    | **Performans düzeyi** | DW100c | Bu hızlı başlangıç için maliyetleri azaltmak üzere en küçük boyuta ayarlayın |

    ![SQL havuzları akış oluşturma-temel bileşenler sekmesi.](media/quickstart-create-sql-pool/create-sql-pool-studio-24.png)
    > [!IMPORTANT]
    > Adanmış SQL havuzlarının kullanabileceği adlarla ilgili belirli sınırlamalar olduğunu unutmayın. Adlar özel karakterler içeremez, 15 veya daha az karakter olmalı, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

4. Bir sonraki sekmede, **ek ayarlar** ' da, VERI olmadan SQL havuzunu sağlamak için **hiçbiri** ' ni seçin. Varsayılan harmanlamayı seçili olarak bırakın.

    ![SQL havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-sql-pool/create-sql-pool-studio-25.png)

1. Şimdilik hiç etiket eklememiz, daha sonra **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur** sekmesinde, ayrıntıların daha önce girilmiş olan öğesine göre doğru göründüğünden emin olun ve **Oluştur** ' a basın. 

    ![SQL havuzu akış oluşturma-ayarları gözden geçirme sekmesi.](media/quickstart-create-sql-pool/create-sql-pool-studio-26.png)

1. Bu noktada kaynak sağlama akışı başlar.

1. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek Yeni oluşturulan SQL havuzu için yeni bir giriş gösterir.

    ![SQL havuzu akış oluşturma-kaynak sağlama.](media/quickstart-create-sql-pool/create-sql-pool-studio-27.png)

1. Adanmış SQL havuzu oluşturulduktan sonra, veri yükleme, akışları işleme, Gölü okuma vb. için çalışma alanında kullanılabilir olur.

## <a name="clean-up-dedicated-sql-pool-using-synapse-studio"></a>SYNAPSE Studio kullanarak adanmış SQL havuzunu Temizleme    

SYNAPSE Studio kullanarak çalışma alanından adanmış SQL havuzunu silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Adanmış bir SQL havuzunun silinmesi, analiz altyapısını çalışma alanından kaldırır. Havuza bağlanmayacak ve tüm sorgular, işlem hatları, bu adanmış SQL havuzunu kullanan betikler artık çalışmayacak.

Adanmış SQL havuzunu silmek istiyorsanız aşağıdakileri yapın:

1. SYNAPSE Studio 'daki yönetim hub 'ındaki SQL havuzlarına gidin.
1. Özel SQL havuzu için komutları göstermek üzere, silinecek adanmış SQL havuzunda (Bu durumda **contosoedw** ) üç nokta simgesini seçin:

    ![Son oluşturulan havuz seçiliyken SQL havuzlarının listelenmesi.](media/quickstart-create-sql-pool/create-sql-pool-studio-28.png)
1. **Sil** 'e basın.
1. Silmeyi onaylayın ve **Sil** düğmesine basın.
1. İşlem başarıyla tamamlandığında, adanmış SQL havuzu artık çalışma alanı kaynaklarında listelenmez.

## <a name="next-steps"></a>Sonraki adımlar 
- Bkz. [hızlı başlangıç: Apache Spark Not defteri oluşturma](quickstart-apache-spark-notebook.md).
- Bkz. [hızlı başlangıç: Azure Portal kullanarak adanmış BIR SQL havuzu oluşturma](quickstart-create-sql-pool-portal.md).
