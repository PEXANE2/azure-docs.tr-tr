---
title: Quickstart bir Apache Spark havuzu oluşturma (önizleme)
description: Bu kılavuzdaki adımları izleyerek Azure Synapse Analytics çalışma alanı için yeni bir Apache Spark havuzu oluşturun.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 1457549fef3a78356c8b1af6be620fdf30ddab46
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424436"
---
# <a name="quickstart-create-a-new-apache-spark-pool-preview"></a>Hızlı başlangıç: Yeni bir Apache Spark havuzu oluşturma (önizleme)

Synapse Analytics, verilerinizi yutmanıza, dönüştürmenize, modellemenize, analiz etmenize ve sunmanıza yardımcı olacak çeşitli analiz motorları sunar. Apache Spark havuzu açık kaynak kodlu büyük veri bilgi işlem özellikleri sunar. Synapse çalışma alanınızda bir Apache Spark havuzu oluşturduktan sonra, veriler yüklenebilir, modellenebilir, işlenebilir ve öngörüler elde etmek için hizmet verebilir.

Bu hızlı başlangıçta, Bir Synapse çalışma alanında Apache Spark havuzu oluşturmak için Azure portalını nasıl kullanacağınızı öğrenirsiniz.

> [!IMPORTANT]
> Spark örnekleri için faturalandırma, bunları kullanıyor olun veya kullanmasanız da dakika başına eşit olarak eşitlenir. Spark örneğini kullanmayı bitirdikten sonra kapattığınızdan veya kısa bir zaman aranızdan ayarladığınızdan emin olun. Daha fazla bilgi için bu makalenin **Kaynakları temizleme** bölümüne bakın.

Azure aboneliğiniz yoksa, başlamadan [önce ücretsiz bir hesap oluşturun.](https:/azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https:/azure.microsoft.com/free/)
- [Synapse Analytics çalışma alanı](quickstart-create-workspace.md)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https:/portal.azure.com/) oturum açın

## <a name="create-new-apache-spark-pool"></a>Yeni Apache Spark havuzu oluşturun

1. Apache Spark havuzunu oluşturmak istediğiniz Synapse çalışma alanında **Yeni Apache Spark havuzunu**tıklatın.
![Yeni bir Apache Spark havuzu oluşturmak için komutun etrafında kırmızı bir kutu yla Synapse çalışma alanına genel bakış](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-01.png)
2. **Temel bilgiler** sekmesine aşağıdaki ayrıntıları girin:

    |Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **Apache Spark havuz adı** | Geçerli bir havuz adı | Bu, Apache Spark havuzunun sahip olacağı addır. |
    | **Düğüm boyutu** | Küçük (4 vCPU / 32 GB) | Bu hızlı başlangıç için maliyetleri azaltmak için bunu en küçük boyuta ayarlayın |
    | **Otomatik Ölçeklendirme** | Etkin | Bu varsayılan ayarı bırakın |
    | **Düğüm sayısı** | 3 - 40 | Bu varsayılan ayarı bırakın |
    ||||

    ![Apache Spark havuzu akış oluşturur - temeller sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-02.png)
    > [!IMPORTANT]
    > Apache Spark havuzlarının kullanabileceği adlar için belirli sınırlamalar olduğunu unutmayın. Adlar yalnızca harfleri veya sayıları içermeli, 15 veya daha az karakter olmalıdır, bir harfle başlamalı, ayrılmış sözcükler içermemeli ve çalışma alanında benzersiz olmalıdır.

3. **İleri'yi tıklatın: ek ayarlar** ve varsayılan ayarları gözden geçirin. Varsayılan ayarları değiştirmeyin.
![Apache Spark havuzu akış oluşturur - ek ayarlar sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03.png)

4. **Sonraki'yi tıklatın: etiketler.** Etiket eklemeyin.
![Apache Spark havuzu akış oluşturur - ek ayarlar sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-03-tags.png)

5. **Gözden geçir ve oluştur**’a tıklayın.

6. Daha önce girilenlere göre ayrıntıların doğru olduğundan emin olun ve **Oluştur'u**tıklatın.
![Apache Spark havuzu akış oluşturmak - gözden geçirme ayarları sekmesi.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-05.png)

7. Bu noktada, kaynak sağlama akışı, tam ![Apache Spark havuzu akış - kaynak sağlama oluşturmak bir kez belirten başlayacaktır.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-06.png)

8. Sağlama tamamlandıktan sonra, çalışma alanına geri dönmek yeni oluşturulan Apache Spark havuzu için yeni bir giriş gösterir.
 ![Apache Spark havuzu akış oluşturmak - kaynak sağlama.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-07.png)

9. Bu noktada, çalışan hiçbir kaynak, Spark için hiçbir ücret, oluşturmak istediğiniz Spark örnekleri hakkında meta veri oluşturduk.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Apache Spark havuzunu çalışma alanından silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Apache Spark havuzunu silerken, analiz altyapısı çalışma alanından kaldırılır. Artık havuza bağlanmak mümkün olmayacaktır ve bu Apache Spark havuzunu kullanan tüm sorgular, ardışık işler ve not defterleri artık çalışmaz.

Apache Spark havuzunu silmek istiyorsanız, aşağıdakileri yapın:

1. Çalışma alanındaki Apache Spark havuzları na gidin.
2. Silinecek Apache havuzunu seçin (bu **durumda, contosospark)**
3. **Sil tuşuna**basın.
 ![Apache Spark havuzlarının listesi, yeni oluşturulan havuz seçilir.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-08.png)
4. Silme işlemini onaylayın ve **Sil** düğmesine basın.
 ![Seçili Apache Spark havuzunu silmek için onay iletişim kutusu.](media/quickstart-create-apache-spark-pool/quickstart-create-spark-pool-10.png)
5. İşlem başarıyla tamamlandığında, Apache Spark havuzu artık çalışma alanı kaynaklarında listelenmez.

SQL havuzu oluşturulduktan sonra, veri yüklemek, akışları işlemek, gölden okuma ve böylece için çalışma alanında kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bkz. Quickstart: Synapse Studio'da web araçlarını kullanarak bir Apache Spark havuzu oluşturun.](spark/apache-spark-notebook-create-spark-use-sql.md)
- Bkz. [Hızlı Başlangıç: Azure portalını kullanarak bir Synapse SQL havuzu oluşturun.](quickstart-create-sql-pool.md)
