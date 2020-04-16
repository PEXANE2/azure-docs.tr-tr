---
title: Quickstart - çalışma alanı oluşturma
description: Bu kılavuzdaki adımları izleyerek bir Azure Synapse Analytics çalışma alanı oluşturun.
services: synapse-analytics
author: malvenko
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: josels
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0b6bf4c6a54e2f187a5751d267720f356e0c30ff
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424401"
---
# <a name="quickstart-create-an-azure-synapse-analytics-workspace-preview"></a>Hızlı başlangıç: Azure Synapse Analytics çalışma alanı oluşturma (önizleme)

Bu hızlı başlangıç, Azure portalını kullanarak bir Azure Synapse çalışma alanı oluşturma adımlarını açıklar.

Azure aboneliğiniz yoksa, başlamadan [önce ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

- [Azure Veri Gölü Depolama Gen2 depolama hesabı](../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın

## <a name="create-an-azure-synapse-workspace-using-the-azure-portal"></a>Azure portalını kullanarak Bir Azure Synapse çalışma alanı oluşturma

1. Microsoft Azure arama bölmesinde **Synapse çalışma alanını** girin ve ardından bu hizmeti seçin.
![Azure Synapse çalışma alanlarının yazılmış olduğu](media/quickstart-create-synapse-workspace/workspace-search.png)Azure portal arama çubuğu.
2. **Synapse çalışma alanları** sayfasında **+ Ekle'yi**tıklatın.
![Yeni Azure Synapse çalışma alanı oluşturma komutu vurgulanır. ](media/quickstart-create-synapse-workspace/create-workspace-02.png).
3. **Azure Synapse çalışma alanı** formunu aşağıdaki bilgilerle doldurun:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **Abonelik** | *Aboneliğiniz* | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | *Herhangi bir kaynak grubu* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
    | **Çalışma alanı adı** | mysampleworkspace | Bağlantı uç noktaları için de kullanılacak çalışma alanının adını belirtir.|
    | **Bölge** | Doğu ABD 2 | Çalışma alanının konumunu belirtir.|
    | **Data Lake Storage Gen2** | Hesabı:`storage account name` </br> Dosya sistemi:`root file system to use` | Birincil depolama olarak kullanmak üzere ADLS Gen2 depolama hesap adını ve kullanılacak dosya sistemini belirtir.|
    ||||

    ![Çalışma alanı sağlama akışı - Temeller sekmesi.](media/quickstart-create-synapse-workspace/create-workspace-03.png).

    Depolama hesabı aşağıdakilerden seçilebilir:
    - Aboneliğinizde bulunan ADLS Gen2 hesaplarının listesi
    - Hesap adını kullanarak el ile girilen

    > [!IMPORTANT]
    > Azure Synapse çalışma alanının seçili ADLS Gen2 hesabına okuma ve yazma yapabilmesi gerekir.
    >
    > ADLS Gen2 seçim alanlarının altında, çalışma alanının yönetilen kimliğinin, tam erişim sağlayan seçili Data Lake Storage Gen2 dosya sisteminde **Storaqe Blob Data Contributor** rolüne atanacağını belirten bir not vardır.

4. (İsteğe bağlı) Güvenlik + **ağ varsayılanlarından** herhangi birini değiştirin:
5. (İsteğe bağlı) **Etiketler** sekmesine etiket ekleyin.
6. **Özet** sekmesi, çalışma alanının başarıyla oluşturulabilmesini sağlamak için gerekli doğrulamaları çalıştırılır. Doğrulama başarılı olduktan sonra, **Create** ![Çalışma Alanı sağlama akışını oluştur](media/quickstart-create-synapse-workspace/create-workspace-05.png)- onay sekmesine basın.
7. Kaynak sağlama işlemi başarıyla tamamlandığında, Synapse çalışma alanları listesinde oluşturulan çalışma alanı için bir giriş görürsünüz. ![Yeni sağlanan çalışma alanını gösteren Synapse çalışma alanlarının listesi.](media/quickstart-create-synapse-workspace/create-workspace-07.png).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Synapse çalışma alanını silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Azure Synapse çalışma alanının silinmesi, analiz altyapılarını ve içerdiği SQL havuzlarının ve çalışma alanı meta verilerinin veritabanında depolanan verileri kaldırır. Artık SQL uç noktalarına bağlanmak mümkün olmayacaktır, Apache Spark uç noktaları. Tüm kod yapıları silinir (sorgular, not defterleri, iş tanımları ve ardışık çizgiler).
>
> Çalışma alanını silerken, çalışma alanına bağlı Veri Gölü Deposu Gen2'deki verileri **etkilemez.**

Azure Synapse çalışma alanını silmek istiyorsanız, aşağıdaki adımları tamamlayın:

1. Silmek için Azure Synapse çalışma alanına gidin.
1. Komut çubuğundaki **sil** tuşuna basın.
 ![Azure Synapse çalışma alanına genel bakış - silme komutu vurgulanır.](media/quickstart-create-synapse-workspace/create-workspace-10.png)
1. Silme işlemini onaylayın ve **Sil** düğmesine basın.
 ![Azure Synapse çalışma alanına genel bakış - çalışma alanı onay iletişim kutusunu silin.](media/quickstart-create-synapse-workspace/create-workspace-11.png)
1. İşlem başarıyla tamamlandığında, Azure Synapse çalışma alanı artık çalışma alanları listesinde listelenmez.

## <a name="next-steps"></a>Sonraki adımlar

Ardından, [verilerinizi](quickstart-create-sql-pool.md) çözümlemeye ve keşfetmeye başlamak için SQL havuzları oluşturabilir veya [Apache Spark havuzları oluşturabilirsiniz.](quickstart-create-apache-spark-pool.md)
