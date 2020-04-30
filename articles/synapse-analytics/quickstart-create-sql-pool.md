---
title: Hızlı başlangıç-SYNAPSE SQL havuzu oluşturma
description: Bu kılavuzdaki adımları izleyerek bir Azure SYNAPSE Analytics SYNAPSE çalışma alanı için yeni bir Synapse SQL havuzu oluşturun.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: a50f203b8057dd4ea51eff1cf09fc97fb598cb3e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82096444"
---
# <a name="quickstart-create-a-synapse-sql-pool"></a>Hızlı başlangıç: SYNAPSE SQL havuzu oluşturma

Azure SYNAPSE Analytics, verilerinizi almanıza, dönüştürmenizi, modellemenizi, çözümlemenize ve sungetirmenize yardımcı olmak için çeşitli analiz motorları sunmaktadır. SQL havuzu, T-SQL tabanlı işlem ve depolama olanakları sunar. SYNAPSE çalışma alanınızda bir SQL havuzu oluşturduktan sonra veriler yüklenebilir, modellenebilir, işlenebilir ve öngörüleri elde etmek için sunulabilir.

Bu hızlı başlangıçta, Azure portal kullanarak bir Synapse çalışma alanında SQL havuzu oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

[SYNAPSE çalışma alanı](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın

## <a name="create-new-sql-pool"></a>Yeni SQL Havuzu Oluştur

1. SQL havuzunu oluşturmak istediğiniz SYNAPSE çalışma alanında, üstteki çubukta **yenı SQL havuzu** komutu ' na tıklayın.
![Yeni bir SQL havuzu oluşturmak için komutu etrafında kırmızı bir kutu ile SYNAPSE çalışma alanına genel bakış.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-01.png)
2. **Temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **SQL havuzu adı** | Geçerli bir ad | SQL havuzunun adı. |
    | **Performans düzeyi** | DW100c | Bu hızlı başlangıç için maliyetleri azaltmak üzere en küçük boyuta ayarla |
    ||||
  
    ![SQL havuzu akış oluşturma-temel kavramlar sekmesi.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-02.png)
    > [!IMPORTANT]
    > SQL havuzlarının kullanabileceği adlarla ilgili belirli sınırlamalar olduğunu unutmayın. Adlar özel karakterler içeremez, 15 veya daha az karakter olmalı, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

3. **İleri**' ye tıklayın.
4. SQL havuzunu veri olmadan sağlamak için **hiçbiri** ' ni seçin. Varsayılan harmanlamayı seçili bırakın.
![SQL havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-03.png)

5. **Gözden geçir ve oluştur**’a tıklayın.
6. Ayrıntıların daha önce girildiği duruma göre doğru göründüğünden emin olun ve **Oluştur**' a tıklayın.
![SQL havuzu akış oluşturma-ayarları gözden geçirme sekmesi.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-04.png)

7. Bu noktada kaynak sağlama akışı başlar.
 ![SQL havuzu akış oluşturma-kaynak sağlama.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-06.png)

8. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek Yeni oluşturulan SQL havuzu için yeni bir giriş gösterir.
 ![SQL havuzu akış oluşturma-kaynak sağlama.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-07.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

SQL havuzunu çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Bir SQL havuzunun silinmesi, her ikisi de analiz altyapısını ve silinen SQL havuzunun veritabanında depolanan verileri çalışma alanından kaldırır. Artık SQL havuzuna bağlanmak mümkün olmayacaktır ve bu SQL havuzunu okuyan veya yazan tüm sorgular, işlem hatları ve Not defterleri artık çalışmayacaktır.

SQL havuzunu silmek istiyorsanız aşağıdaki adımları izleyin:

1. Çalışma alanı Dikey penceresinde SQL havuzları dikey penceresine gidin
1. Silinecek SQL havuzunu seçin (Bu durumda **contosoedw**)
1. Seçin ve **Sil**'e basın.
1. Silmeyi onaylayın ve **Sil** düğmesine basın.
 ![SQL havuzuna genel bakış-silme onayını vurgulama.](media/quickstart-create-sql-pool/quickstart-create-sql-pool-11.png)
1. İşlem başarıyla tamamlandığında, SQL havuzu artık çalışma alanı kaynaklarında listelenmeyecektir.

SQL havuzu oluşturulduktan sonra, veri yükleme, akışları işleme, Gölü okuma vb. için çalışma alanında kullanılabilir olur.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [hızlı başlangıç: Web araçlarını kullanarak SYNAPSE Studio 'da Apache Spark havuzu oluşturma](quickstart-apache-spark-notebook.md).
- Bkz. [hızlı başlangıç: Azure Portal kullanarak Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool.md).
