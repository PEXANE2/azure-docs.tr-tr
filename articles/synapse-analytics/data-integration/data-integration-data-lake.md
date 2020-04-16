---
title: Azure Synapse Analytics'te Azure Veri Gölü Depolama Gen2'ye girme
description: Azure Synapse Analytics'te Azure Veri Gölü Depolama Gen2'ye nasıl veri sindirilemeyi öğrenin
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 4d7d7be523749797e5dbce0e50c307fc682974f2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430584"
---
# <a name="ingesting-data-into-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'ye veri alma 

Bu makalede, Azure Synapse Analytics'i kullanarak bir Azure Veri Gölü Gen 2 (Azure Veri Gölü Gen 2) depolama hesabında bir konumdan diğerine verileri nasıl yutacağınız öğrenilir.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa, başlamadan önce ücretsiz bir [Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure Depolama hesabı**: Azure Veri Gölü Gen 2'yi *kaynak* veri deposu olarak kullanıyorsunuz. Depolama hesabınız yoksa, bir depolama hesabı oluşturmak için adımlar için [bir Azure Depolama hesabı oluşturma'ya](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) bakın.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Azure Synapse Analytics'te bağlantılı bir hizmet, bağlantı bilgilerinizi diğer hizmetlerle tanımladığınız yerdir. Bu bölümde, Azure Synapse Analytics ve Azure Data Lake Gen 2'yi bağlantılı hizmetler olarak eklersiniz.

1. Azure Synapse Analytics UX'yi açın ve **Yönet** sekmesine gidin.
1. **Dış bağlantılar**altında, **Bağlantılı hizmetleri**seçin.
1. Bağlantılı bir hizmet eklemek için **Yeni'yi**tıklatın.
1. Listeden Azure Veri Gölü Depolama Gen2 döşemesini seçin ve **Devam et'i**tıklatın.
1. Kimlik doğrulama kimlik bilgilerinizi girin. Hesap anahtarı, hizmet sorumlusu ve yönetilen kimlik şu anda desteklenen kimlik doğrulama türleridir. Kimlik bilgilerinizin doğru luğunu doğrulamak için test bağlantısını tıklatın. 
1. Tamamlandığında **Oluştur'u** tıklatın.

## <a name="create-pipeline"></a>İşlem hattı oluşturma

Bir ardışık iş, bir dizi etkinlik yürütmesi için mantıksal akışı içerir. Bu bölümde, Azure Veri Gölü Gen 2'den sql havuzuna veri yutacak bir kopyalama etkinliği içeren bir ardışık işlem oluşturursunuz.

1. **Düzenleme** sekmesine gidin. Ardışık hatlar üstbilgisinin yanındaki artı simgesine tıklayın ve **Pipeline'ı**seçin.
1. Etkinlikler bölmesinde **Taşı ve Dönüştür** altında, **Kopyala verilerini** boru hattı tuvaline sürükleyin.
1. Kopyalama etkinliğine tıklayın ve **Kaynak** sekmesine gidin. Yeni kaynak veri kümesi oluşturmak için **Yeni'yi** tıklatın.
1. Veri deponuz olarak Azure Veri Gölü Depolama Gen2'yi seçin ve devam et'i tıklatın.
1. Biçiminiz olarak DelimitedText'i seçin ve devam et'i tıklatın.
1. Ayarözellikleri bölmesinde, oluşturduğunuz ADLS bağlantılı hizmeti seçin. Kaynak verilerinizin dosya yolunu belirtin ve ilk satırda üstbilgi olup olmadığını belirtin. Şema dosya deposundan veya örnek bir dosyadan içe aktarabilirsiniz. Bittiğinde Tamam'ı tıklatın.
1. Yeni bir lavabo veri kümesi oluşturmak **için** **Lavabo** sekmesine gidin.
1. Veri deponuz olarak Azure Veri Gölü Depolama gen2'yi seçin ve devam et'i tıklatın.
1. Biçiminiz olarak DelimitedText'i seçin ve devam et'i tıklatın.
1. Ayarözellikleri bölmesinde, oluşturduğunuz ADLS bağlantılı hizmeti seçin. Veri yazmak istediğiniz klasörün yolunu belirtin. Bittiğinde Tamam'ı tıklatın.

## <a name="debug-and-publish-pipeline"></a>Hata ayıklama ve yayın boru hattı

Ardışık işlem inizi yapılandırmayı tamamladıktan sonra, her şeyin doğru olduğunu doğrulamak için yapılarınızı yayımlamadan önce bir hata ayıklama çalışması gerçekleştirebilirsiniz.

1. İşlem hattında hata ayıklamak için araç çubuğunda **Hata Ayıkla**'yı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir. 
1. Ardışık hatlar başarılı bir şekilde çalıştırıladıktan sonra, üst araç çubuğunda **Tümünü Yayımla'yı**seçin. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve ardışık yollar) Synapse Analytics hizmetine yayımlar.
1. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sağ üstteki çan düğmesini tıklatın. 


## <a name="trigger-and-monitor-the-pipeline"></a>Boru hattını tetikleme ve izleme

Bu adımda, önceki adımda yayınlanan ardışık hattı el ile tetiklersiniz. 

1. Araç çubuğunda **Tetikleyici Ekle'yi** ve ardından Şimdi **Tetikle'yi**seçin. **İşlem Hattı Çalıştırma** sayfasında **Son**’u seçin.  
1. Sol kenar çubuğunda bulunan **Monitör** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve ardışık hattı yeniden çalıştırmak için **Eylemler** sütunundaki bağlantıları kullanabilirsiniz.
1. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu örnekte, yalnızca bir etkinlik vardır, bu nedenle listede yalnızca bir giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. Pipeline Runs'ı seçerek üstteki **Boru Hattı** Çalışır'ı seçin ve Pipeline Runs görünümüne geri döner. Görünümü yenilemek için **Yenile**’yi seçin.
1. Verilerinizin SQL havuzunda doğru şekilde yazıldığını doğrulayın.


## <a name="next-steps"></a>Sonraki adımlar

Synapse Analytics için veri tümleştirmesi hakkında daha fazla bilgi için, verileri sql havuzu makalesine doğru [sindirin'](data-integration-sql-pool.md) e bakın.
