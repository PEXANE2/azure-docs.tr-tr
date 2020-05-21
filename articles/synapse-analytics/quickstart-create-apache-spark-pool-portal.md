---
title: Hızlı başlangıç-Azure portal kullanarak Apache Spark Havuzu (Önizleme) oluşturma
description: Bu kılavuzdaki adımları izleyerek Azure portal kullanarak yeni bir Apache Spark havuzu oluşturun.
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 04a413ab171ea65c72182928ad4897530cb00e15
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83666333"
---
# <a name="quickstart-create-a-new-apache-spark-pool-preview-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak yeni bir Apache Spark Havuzu (Önizleme) oluşturma

Azure SYNAPSE Analytics, verilerinizi alma, dönüştürme, modellemeyle, analiz etmenize ve dağıtmanıza yardımcı olmak için çeşitli analiz motorları sunmaktadır. Apache Spark havuzu, açık kaynak büyük veri işlem özellikleri sağlar. SYNAPSE çalışma alanınızda bir Apache Spark havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve daha hızlı analitik Öngörüler için dağıtılabilir.

Bu hızlı başlangıçta, bir Synapse çalışma alanında bir Apache Spark havuzu oluşturmak için Azure portal nasıl kullanacağınızı öğreneceksiniz.

> [!IMPORTANT]
> Spark örnekleri için faturalandırma, bunları kullanıp kullanmayacağınızı de dakika başına eşit olarak dağıtılır. Kullanmayı bitirdikten sonra Spark örneğinizi kapattığınızdan emin olun veya kısa bir zaman aşımı ayarlayın. Daha fazla bilgi için bu makalenin **Kaynakları temizleme** bölümüne bakın.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [SYNAPSE Analytics çalışma alanı](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın

## <a name="navigate-to-the-synapse-workspace"></a>SYNAPSE çalışma alanına gidin 
1. Arama çubuğuna hizmet adı (veya doğrudan kaynak adı) yazarak Apache Spark havuzunun oluşturulacağı SYNAPSE çalışma alanına gidin.
![İçinde SYNAPSE çalışma alanlarıyla yazılan arama çubuğu Azure portal.](media/quickstart-create-sql-pool/create-sql-pool-00a.png)
1. Çalışma alanları listesinden, açmak için çalışma alanının adını (veya adının bir bölümünü) yazın. Bu örnekte, **contosoanalytics**adlı bir çalışma alanı kullanacağız.
![Contoso adını içeren bunları göstermek üzere filtrelenmiş SYNAPSE çalışma alanlarının listesi.](media/quickstart-create-sql-pool/create-sql-pool-00b.png)


## <a name="create-new-apache-spark-pool"></a>Yeni Apache Spark Havuzu Oluştur

1. Apache Spark havuzunu oluşturmak istediğiniz SYNAPSE çalışma alanında **yeni Apache Spark havuzu**' nu seçin.
    ![Yeni bir Apache Spark havuzu oluşturmak için komutu etrafında kırmızı bir kutu ile SYNAPSE çalışma alanına genel bakış](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-01.png)
2. **Temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    |Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **Apache Spark havuzu adı** | Geçerli bir havuz adı | Bu, Apache Spark havuzunun sahip olacağı addır. |
    | **Düğüm boyutu** | Küçük (4 vCPU/32 GB) | Bu hızlı başlangıç için maliyetleri azaltmak üzere en küçük boyuta ayarlayın |
    | **Otomatik Ölçeklendirme** | Devre dışı | Bu hızlı başlangıçta otomatik ölçeklendirme gerekmiyor |
    | **Düğüm sayısı** | 5 | Bu hızlı başlangıç için maliyetleri sınırlamak üzere küçük bir boyut kullanın |


    ![Apache Spark havuzu akış oluşturma-temel bilgiler sekmesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-02.png)
    > [!IMPORTANT]
    > Apache Spark havuzların kullanabileceği adlarla ilgili özel sınırlamalar olduğunu unutmayın. Adlar yalnızca harf veya sayı içermelidir, 15 veya daha az karakter olmalıdır, bir harfle başlamalı, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

3. **İleri ' yi seçin: ek ayarlar** ve varsayılan ayarları gözden geçirin. Herhangi bir varsayılan ayarı değiştirmeyin.
    ![Apache Spark havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-03.png)

4. **Sonraki: Etiketler**' i seçin. Etiket eklemeyin.
    ![Apache Spark havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-03-tags.png)

5. **İncele ve oluştur**’u seçin.

6. Ayrıntıların daha önce girildiği duruma göre doğru göründüğünden emin olun ve **Oluştur**' u seçin.
    ![Apache Spark havuzu akış oluşturma-ayarları gözden geçirme sekmesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-05.png)

7. Bu noktada, kaynak sağlama akışı başlar ve bu işlem tamamlandıktan sonra görüntülenir.
    ![Apache Spark havuzu Flow-Resource hazırlama oluşturma.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-06.png)

8. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek Yeni oluşturulan Apache Spark havuzu için yeni bir giriş gösterecektir.
    ![Apache Spark havuzu Flow-Resource hazırlama oluşturma.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-07.png)

9. Bu noktada, çalışan hiçbir kaynak yoktur, Spark için ücret ödemez, oluşturmak istediğiniz Spark örnekleri hakkında meta veriler oluşturdunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Apache Spark havuzunu çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Apache Spark havuzu silindiğinde, analiz altyapısı çalışma alanından kaldırılır. Havuza bağlanmayacak ve bu Apache Spark havuzunu kullanan tüm sorgular, işlem hatları ve Not defterleri artık çalışmayacak.

Apache Spark havuzunu silmek istiyorsanız, aşağıdakileri yapın:

1. Çalışma alanındaki Apache Spark havuzları dikey penceresine gidin.
2. Silinecek Apache Spark havuzunu seçin (Bu durumda, **contosospark**).
3. **Sil**'e basın.
 ![Son oluşturulan havuz seçiliyken Apache Spark havuzlarının listelenmesi.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-08.png)
4. Silmeyi onaylayın ve **Sil** düğmesine basın.
 ![Seçili Apache Spark havuzunu silmek için onay iletişim kutusu.](media/quickstart-create-apache-spark-pool/create-spark-pool-portal-10.png)
5. İşlem başarıyla tamamlandığında, Apache Spark havuzu artık çalışma alanı kaynaklarında listelenmeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [hızlı başlangıç: Apache Spark Not defteri oluşturma](quickstart-apache-spark-notebook.md).
- Bkz. [hızlı başlangıç: Azure Portal kullanarak SYNAPSE SQL havuzu oluşturma](quickstart-create-sql-pool-portal.md).
