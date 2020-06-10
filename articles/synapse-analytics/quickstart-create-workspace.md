---
title: Hızlı başlangıç-SYNAPSE çalışma alanı oluşturma
description: Bu kılavuzdaki adımları izleyerek bir Synapse çalışma alanı oluşturun.
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 6f72e2ee3b83b693d889e9c43fe42a4677f1666d
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84609480"
---
# <a name="quickstart-create-an-azure-synapse-analytics-workspace-preview"></a>Hızlı başlangıç: Azure SYNAPSE Analytics çalışma alanı oluşturma (Önizleme)

Bu hızlı başlangıçta Azure portal kullanarak bir Azure SYNAPSE çalışma alanı oluşturma adımları açıklanmaktadır.

Azure aboneliğiniz yoksa [başlamadan önce ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

- [Azure Data Lake Storage 2. depolama hesabı](../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/) oturum açın

## <a name="create-an-azure-synapse-workspace-using-the-azure-portal"></a>Azure portal kullanarak bir Azure SYNAPSE çalışma alanı oluşturma

1. Microsoft Azure Arama bölmesinde, **SYNAPSE Workspace** girin ve ardından bu hizmeti seçin.
![İçinde yazılan Azure SYNAPSE çalışma alanları ile arama çubuğu Azure portal.](media/quickstart-create-synapse-workspace/workspace-search.png)
2. **SYNAPSE çalışma alanları** sayfasında **+ Ekle**' ye tıklayın.
![Yeni Azure SYNAPSE çalışma alanı vurgulanmış olan komut.](media/quickstart-create-synapse-workspace/create-workspace-02.png)
3. **Azure SYNAPSE çalışma alanı** formunu aşağıdaki bilgilerle doldurun:

    | Ayar | Önerilen değer | Açıklama |
    | :------ | :-------------- | :---------- |
    | **Abonelik** | *Aboneliğiniz* | Abonelikleriniz hakkında daha ayrıntılı bilgi için bkz. [Abonelikler](https://account.windowsazure.com/Subscriptions). |
    | **Kaynak grubu** | *Herhangi bir kaynak grubu* | Geçerli kaynak grubu adları için bkz. [Adlandırma kuralları ve kısıtlamalar](/azure/architecture/best-practices/resource-naming?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest). |
    | **Çalışma alanı adı** | mysampleworkspace | Bağlantı uç noktaları için de kullanılacak olan çalışma alanının adını belirtir.|
    | **Geli** | Doğu ABD 2 | Çalışma alanının konumunu belirtir.|
    | **Data Lake Storage 2. Nesil** | Hesabı`storage account name` </br> Dosya sistemi:`root file system to use` | Birincil depolama ve kullanılacak dosya sistemi olarak kullanılacak ADLS 2. depolama hesabı adını belirtir.|
    ||||

    ![Çalışma alanı sağlama akışı-temel kavramlar sekmesi.](media/quickstart-create-synapse-workspace/create-workspace-03.png)

    Depolama hesabı şu kaynaktan seçilebilir:
    - Aboneliğinizde kullanılabilen ADLS 2. hesaplarının listesi
    - Hesap adı kullanılarak elle girilen

    > [!IMPORTANT]
    > Azure SYNAPSE çalışma alanının seçili ADLS 2. hesabını okuyup yazabilbilmesi gerekir. Ayrıca, birincil depolama hesabı olarak bağlanan herhangi bir depolama hesabı için depolama hesabı oluşturma sırasında **hiyerarşik ad alanını** etkinleştirmiş olmanız gerekir.
    >
    > ADLS 2. seçim alanlarının altında, çalışma alanının yönetilen kimliğinin seçili Data Lake Storage 2. dosya sisteminde tam erişim sağlayan **Storaqe blob veri katılımcısı** rolüne atandığını belirten bir göz vardır.

4. Seçim **Güvenlik + ağ Varsayılanları** sekmesinden herhangi birini değiştirin:
5. Seçim **Etiketler** sekmesine Etiketler ekleyin.
6. **Özet** sekmesi, çalışma alanının başarıyla oluşturulduğundan emin olmak için gerekli doğrulamaları çalıştırır. Doğrulama başarılı olduktan sonra, **Create** ![ çalışma alanı sağlama akışı-onay sekmesine basın.](media/quickstart-create-synapse-workspace/create-workspace-05.png)
7. Kaynak sağlama işlemi başarıyla tamamlandıktan sonra, SYNAPSE çalışma alanları listesinde oluşturulan çalışma alanı için bir giriş görürsünüz. ![Yeni sağlanan çalışma alanını gösteren SYNAPSE çalışma alanlarının listesi.](media/quickstart-create-synapse-workspace/create-workspace-07.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure SYNAPSE çalışma alanını silmek için aşağıdaki adımları izleyin.
> [!WARNING]
> Azure SYNAPSE çalışma alanını silmek, bulunan SQL havuzlarının ve çalışma alanı meta verilerinin veritabanında depolanan analiz altyapılarını ve verileri kaldırır. Artık uç noktaları Apache Spark SQL uç noktalarına bağlanmak mümkün olmayacaktır. Tüm kod yapıtları silinecek (sorgular, Not defterleri, iş tanımları ve işlem hatları).
>
> Çalışma alanını silmek, çalışma alanına bağlı Data Lake Store Gen2 **verileri etkilemez.**

Azure SYNAPSE çalışma alanını silmek istiyorsanız aşağıdaki adımları izleyin:

1. Silmek için Azure SYNAPSE çalışma alanına gidin.
1. Komut çubuğunda **Sil** ' e basın.
 ![Azure SYNAPSE Workspace genel bakış-Sil komutu vurgulanmış.](media/quickstart-create-synapse-workspace/create-workspace-10.png)
1. Silmeyi onaylayın ve **Sil** düğmesine basın.
 ![Azure SYNAPSE çalışma alanına genel bakış-çalışma alanı onay iletişim kutusunu silin.](media/quickstart-create-synapse-workspace/create-workspace-11.png)
1. İşlem başarıyla tamamlandığında, Azure SYNAPSE çalışma alanı artık çalışma alanları listesinde listelenmeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

Daha sonra, verilerinizi analiz etmeye ve keşfetmeye başlamak için [SQL havuzları oluşturabilir](quickstart-create-sql-pool-studio.md) veya [Apache Spark havuzları oluşturabilirsiniz](quickstart-create-apache-spark-pool-studio.md) .
