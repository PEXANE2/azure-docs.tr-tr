---
title: Hızlı başlangıç Apache Spark havuzu oluşturma (Önizleme)
description: Bu kılavuzdaki adımları izleyerek bir Azure SYNAPSE Analytics çalışma alanı için yeni bir Apache Spark havuzu oluşturun.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 2501f66c42360d3ec1626f5f4ee42e0212f1967b
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82792250"
---
# <a name="quickstart-create-a-new-apache-spark-pool-preview"></a>Hızlı başlangıç: yeni bir Apache Spark havuzu oluşturma (Önizleme)

SYNAPSE Analytics, verilerinizi almanıza, dönüştürmenizi, modellemenizi, çözümlemenize ve sungetirmenize yardımcı olmak için çeşitli analiz motorları sunmaktadır. Bir Apache Spark havuz, açık kaynak büyük veri işlem yetenekleri sunar. SYNAPSE çalışma alanınızda bir Apache Spark havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve öngörüleri elde etmek için sunulabilir.

Bu hızlı başlangıçta, bir Synapse çalışma alanında bir Apache Spark havuzu oluşturmak için Azure portal nasıl kullanacağınızı öğreneceksiniz.

> [!IMPORTANT]
> Spark örnekleri için faturalandırma, bunları kullanıp kullanmayacağınızı de dakika başına eşit olarak dağıtılır. Kullanmayı bitirdikten sonra Spark örneğinizi kapattığınızdan emin olun veya kısa bir zaman aşımı ayarlayın. Daha fazla bilgi için bu makalenin **Kaynakları temizleme** bölümüne bakın.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)
- [SYNAPSE Analytics çalışma alanı](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın

## <a name="create-new-apache-spark-pool"></a>Yeni Apache Spark Havuzu Oluştur

1. Apache Spark havuzunu oluşturmak istediğiniz SYNAPSE çalışma alanında **yeni Apache Spark havuzu**' na tıklayın.
![Yeni bir Apache Spark havuzu oluşturmak için komutu etrafında kırmızı bir kutu ile SYNAPSE çalışma alanına genel bakış](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-01.png)
2. **Temel bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    |Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **Apache Spark havuzu adı** | Geçerli bir havuz adı | Bu, Apache Spark havuzunun sahip olacağı addır. |
    | **Düğüm boyutu** | Küçük (4 vCPU/32 GB) | Bu hızlı başlangıç için maliyetleri azaltmak üzere en küçük boyuta ayarlayın |
    | **Otomatik Ölçeklendirme** | Etkin | Bu varsayılan ayarı bırak |
    | **Düğüm sayısı** | 3 - 40 | Bu varsayılan ayarı bırak |
    ||||

    ![Apache Spark havuzu akış oluşturma-temel bilgiler sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-02.png)
    > [!IMPORTANT]
    > Apache Spark havuzların kullanabileceği adlarla ilgili özel sınırlamalar olduğunu unutmayın. Adlar yalnızca harf veya sayı içermelidir, 15 veya daha az karakter olmalıdır, bir harfle başlamalı, ayrılmış sözcükler içermemelidir ve çalışma alanında benzersiz olmalıdır.

3. Ileri ' ye tıklayın **: ek ayarlar** ve varsayılan ayarları gözden geçirin. Herhangi bir varsayılan ayarı değiştirmeyin.
![Apache Spark havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03.png)

4. **İleri: Etiketler**' e tıklayın. Herhangi bir etiket eklemeyin.
![Apache Spark havuzu akış oluşturma-ek ayarlar sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03-tags.png)

5. **Gözden geçir ve oluştur**’a tıklayın.

6. Ayrıntıların daha önce girildiği duruma göre doğru göründüğünden emin olun ve **Oluştur**' a tıklayın.
![Apache Spark havuzu akış oluşturma-ayarları gözden geçirme sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-05.png)

7. Bu noktada kaynak sağlama akışı başlar ve Apache Spark havuzun Flow-Resource sağlamasını oluşturma işlemi tamamlanır ![.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-06.png)

8. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek Yeni oluşturulan Apache Spark havuzu için yeni bir giriş gösterecektir.
 ![Apache Spark havuzu Flow-Resource hazırlama oluşturma.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-07.png)

9. Bu noktada, çalışan hiçbir kaynak yoktur, Spark için ücret ödemez, oluşturmak istediğiniz Spark örnekleri hakkında meta veriler oluşturdunuz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Apache Spark havuzunu çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Apache Spark havuzu silindiğinde, analiz altyapısı çalışma alanından kaldırılır. Havuza bağlanmayacak ve bu Apache Spark havuzunu kullanan tüm sorgular, işlem hatları ve Not defterleri artık çalışmayacak.

Apache Spark havuzunu silmek istiyorsanız, aşağıdakileri yapın:

1. Çalışma alanındaki Apache Spark havuzları dikey penceresine gidin.
2. Silinecek Apache havuzunu seçin (Bu durumda, **contosospark**)
3. **Sil**'e basın.
 ![Son oluşturulan havuz seçiliyken Apache Spark havuzlarının listelenmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-08.png)
4. Silmeyi onaylayın ve **Sil** düğmesine basın.
 ![Seçili Apache Spark havuzunu silmek için onay iletişim kutusu.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-10.png)
5. İşlem başarıyla tamamlandığında, Apache Spark havuzu artık çalışma alanı kaynaklarında listelenmeyecektir.

SQL havuzu oluşturulduktan sonra, veri yüklemek, akışları işlemek, Gölü okumak ve bu şekilde çalışma alanında kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [hızlı başlangıç: Apache Spark Not defteri oluşturma](quickstart-apache-spark-notebook.md).
- Bkz. [hızlı başlangıç: Azure Portal kullanarak SYNAPSE SQL havuzu oluşturma](quickstart-create-sql-pool.md).
