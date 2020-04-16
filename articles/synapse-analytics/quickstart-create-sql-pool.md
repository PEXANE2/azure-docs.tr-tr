---
title: Quickstart - Synapse SQL havuzu oluşturma
description: Bu kılavuzdaki adımları izleyerek Azure Synapse Analytics Synapse Workspace için yeni bir Synapse SQL havuzu oluşturun.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: b3cd4ed280529efbde32fb89e6bac20640940fff
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423911"
---
# <a name="quickstart-create-a-synapse-sql-pool"></a>Quickstart: Synapse SQL havuzu oluşturma

Azure Synapse Analytics, verilerinizi yutmanıza, dönüştürmenize, modellemenize, analiz etmenize ve sunmanıza yardımcı olacak çeşitli analiz motorları sunar. SQL havuzu, T-SQL tabanlı işlem ve depolama özellikleri sunar. Synapse Çalışma Alanınızda bir SQL havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve öngörüler elde etmek için hizmet verebilir.

Bu hızlı başlangıçta, Azure portalını kullanarak Synapse Workspace'te bir SQL havuzu oluşturmayı öğrenirsiniz.

Azure aboneliğiniz yoksa, başlamadan [önce ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

[Sinaps Çalışma Alanı](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın

## <a name="create-new-sql-pool"></a>Yeni SQL havuzu oluşturma

1. SQL havuzunu oluşturmak istediğiniz Synapse çalışma alanında, üst çubuktaki **Yeni SQL havuzu** komutunu tıklatın.
![Yeni bir SQL havuzu oluşturmak için komutun etrafında kırmızı bir kutu ile Synapse Çalışma Alanına genel bakış.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-01.png)
2. **Temel bilgiler** sekmesine aşağıdaki ayrıntıları girin:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **SQL havuz adı** | Geçerli bir ad | SQL havuzunun adı. |
    | **Performans düzeyi** | DW100c | Bu hızlı başlangıç için maliyetleri azaltmak için en küçük boyuta ayarlayın |
    ||||
  
    ![SQL havuzu akış oluşturmak - temelleri sekmesi.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-02.png)
    > [!IMPORTANT]
    > SQL havuzlarının kullanabileceği adlar için belirli sınırlamalar olduğunu unutmayın. Adlar özel karakterler içeremez, 15 veya daha az karakter olmalıdır, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

3. **İleri'yi tıklatın: Ek ayarlar.**
4. SQL havuzunu veri olmadan sağlamak için **Yok'u** seçin. Varsayılan harmanlama seçili bırakın.
![SQL havuzu akış oluşturmak - ek ayarlar sekmesi.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-03.png)

5. **Gözden geçir ve oluştur**’a tıklayın.
6. Daha önce girilenlere göre ayrıntıların doğru olduğundan emin olun ve **Oluştur'u**tıklatın.
![SQL havuzu akış oluşturmak - gözden geçirme ayarları sekmesi.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-04.png)

7. Bu noktada, kaynak sağlama akışı başlayacaktır.
 ![SQL havuzu akış oluşturmak - kaynak sağlama.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-06.png)

8. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek yeni oluşturulan SQL havuzu için yeni bir giriş gösterir.
 ![SQL havuzu akış oluşturmak - kaynak sağlama.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-07.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

SQL havuzunu çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> SQL havuzunun silinmesi hem analiz altyapısını hem de silinen SQL havuzunun veritabanında depolanan verileri çalışma alanından kaldırır. Artık SQL havuzuna bağlanmak mümkün olmayacaktır ve bu SQL havuzuna okuyan veya yazan tüm sorgular, ardışık işler ve not defterleri artık çalışmaz.

SQL havuzunu silmek istiyorsanız, aşağıdaki adımları tamamlayın:

1. Çalışma alanı bıçaklarında SQL havuzları na gidin
1. Silinecek SQL havuzunu seçin (bu durumda **contosoedw)**
1. Seçin ve **sil tuşuna**basın.
1. Silme işlemini onaylayın ve **Sil** düğmesine basın.
 ![SQL havuzuna genel bakış - silme onayına dikkat çekerek.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-11.png)
1. İşlem başarıyla tamamlandığında, SQL havuzu artık çalışma alanı kaynaklarında listelenmez.

SQL havuzu oluşturulduktan sonra, veri yükleme, akış işleme, gölden okuma vb. için çalışma alanında kullanılabilir olacaktır.

## <a name="next-steps"></a>Sonraki adımlar

- [Bkz. Quickstart: Synapse Studio'da web araçlarını kullanarak bir Apache Spark havuzu oluşturun.](spark/apache-spark-notebook-create-spark-use-sql.md)
- Bkz. [Hızlı Başlangıç: Azure portalını kullanarak Bir Apache Spark havuzu oluşturun.](quickstart-create-apache-spark-pool.md)
