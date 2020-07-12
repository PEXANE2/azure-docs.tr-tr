---
title: 'Hızlı başlangıç: Azure portal kullanarak bir Synapse SQL Havuzu (Önizleme) oluşturma'
description: Bu kılavuzdaki adımları izleyerek Azure portal kullanarak yeni bir Synapse SQL havuzu oluşturun.
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 768b559ddaf7364726cb26a34d12f357d4126426
ms.sourcegitcommit: 0b2367b4a9171cac4a706ae9f516e108e25db30c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86274263"
---
# <a name="quickstart-create-a-synapse-sql-pool-preview-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Synapse SQL Havuzu (Önizleme) oluşturma

Azure SYNAPSE Analytics, verilerinizi almak, dönüştürmek, modellemek ve analiz etmenize yardımcı olmak için çeşitli analiz motorları sunmaktadır. SQL havuzu, T-SQL tabanlı işlem ve depolama olanakları sunar. SYNAPSE çalışma alanınızda bir SQL havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve daha hızlı analitik Öngörüler için teslim edilebilir.

Bu hızlı başlangıçta, Azure portal kullanarak bir Synapse çalışma alanında SQL havuzu oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [SYNAPSE çalışma alanı](./quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın

## <a name="navigate-to-the-synapse-workspace"></a>SYNAPSE çalışma alanına gidin

1. Arama çubuğuna hizmet adı (veya doğrudan kaynak adı) yazarak SQL havuzunun oluşturulacağı SYNAPSE çalışma alanına gidin.
![İçinde SYNAPSE çalışma alanları içeren arama çubuğu Azure portal. ](media/quickstart-create-sql-pool/create-sql-pool-00a.png) . 
1. Çalışma alanları listesinden, açmak için çalışma alanının adını (veya adının bir bölümünü) yazın. Bu örnekte, **contosoanalytics**adlı bir çalışma alanı kullanacağız.
![Contoso adını içeren bunları göstermek üzere filtrelenmiş SYNAPSE çalışma alanlarının listesi.](media/quickstart-create-sql-pool/create-sql-pool-00b.png)

## <a name="create-new-sql-pool"></a>Yeni SQL Havuzu Oluştur

1. SQL havuzunu oluşturmak istediğiniz SYNAPSE çalışma alanında, üstteki çubukta **yenı SQL havuzu** komutu ' nu seçin.
![Yeni bir SQL havuzu oluşturmak için komutu etrafında kırmızı bir kutu ile SYNAPSE çalışma alanına genel bakış.](media/quickstart-create-sql-pool/create-sql-pool-portal-01.png)
2. **Temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **SQL havuzu adı** | Geçerli bir ad | SQL havuzunun adı. |
    | **Performans düzeyi** | DW100c | Bu hızlı başlangıç için maliyetleri azaltmak üzere en küçük boyuta ayarla |

  
    ![SQL havuzu akış oluşturma-temel kavramlar sekmesi.](media/quickstart-create-sql-pool/create-sql-pool-portal-02.png)
    > [!IMPORTANT]
    > SQL havuzlarının kullanabileceği adlarla ilgili belirli sınırlamalar olduğunu unutmayın. Adlar özel karakterler içeremez, 15 veya daha az karakter olmalı, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

3. **İleri ' yi seçin: ek ayarlar**.
4. SQL havuzunu veri olmadan sağlamak için **hiçbiri** ' ni seçin. Varsayılan harmanlamayı seçili bırakın.
![SQL havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-sql-pool/create-sql-pool-portal-03.png)

5. **Gözden geçir ve oluştur**’u seçin.
6. Ayrıntıların daha önce girildiği duruma göre doğru göründüğünden emin olun. **Oluştur**’u seçin.
![SQL havuzu akış oluşturma-ayarları gözden geçirme sekmesi.](media/quickstart-create-sql-pool/create-sql-pool-portal-04.png)

7. Bu noktada kaynak sağlama akışı başlar.
 ![SQL havuzu akış oluşturma-kaynak sağlama.](media/quickstart-create-sql-pool/create-sql-pool-portal-06.png)

8. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek Yeni oluşturulan SQL havuzu için yeni bir giriş gösterir.
 ![SQL havuzu akış oluşturma-kaynak sağlama.](media/quickstart-create-sql-pool/create-sql-pool-portal-07.png)


SQL havuzu oluşturulduktan sonra, veri yükleme, akışları işleme, Gölü okuma vb. için çalışma alanında kullanılabilir olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

SQL havuzunu çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Bir SQL havuzunun silinmesi, her ikisi de analiz altyapısını ve silinen SQL havuzunun veritabanında depolanan verileri çalışma alanından kaldırır. Artık SQL havuzuna bağlanmak mümkün olmayacaktır ve bu SQL havuzunu okuyan veya yazan tüm sorgular, işlem hatları ve Not defterleri artık çalışmayacaktır.

SQL havuzunu silmek istiyorsanız aşağıdaki adımları izleyin:

1. Çalışma alanı Dikey penceresinde SQL havuzları dikey penceresine gidin
1. Silinecek SQL havuzunu seçin (Bu durumda **contosowdw**)
1. Seçildikten sonra **Sil** 'e basın
1. Silmeyi onaylayın ve **Sil** düğmesine basın ![ SQL havuzuna genel bakış-silme onayını vurgulama.](media/quickstart-create-sql-pool/create-sql-pool-portal-11.png)
1. İşlem başarıyla tamamlandığında, SQL havuzu artık çalışma alanı kaynaklarında listelenmeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [hızlı başlangıç: Web araçlarını kullanarak SYNAPSE Studio 'da Apache Spark havuzu oluşturma](quickstart-apache-spark-notebook.md).
- Bkz. [hızlı başlangıç: Azure Portal kullanarak Apache Spark havuzu oluşturma](quickstart-create-apache-spark-pool-portal.md).
