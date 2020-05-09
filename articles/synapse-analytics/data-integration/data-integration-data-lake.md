---
title: Azure Data Lake Storage 2.
description: Azure SYNAPSE Analytics 'te verileri Azure Data Lake Storage 2. alma hakkında bilgi edinin
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 7b844bcf45417fefc7dd78a26d5dae0b2ce03249
ms.sourcegitcommit: 999ccaf74347605e32505cbcfd6121163560a4ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82982913"
---
# <a name="ingesting-data-into-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. içine veri yerleştirme 

Bu makalede, Azure SYNAPSE Analytics kullanarak bir Azure Data Lake Gen 2 (Azure Data Lake Gen 2) depolama hesabında verileri bir konumdan diğerine alma hakkında bilgi edineceksiniz.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure depolama hesabı**: Azure Data Lake Gen 2 ' nı *kaynak* veri deposu olarak kullanırsınız. Depolama hesabınız yoksa, oluşturma adımları için bkz. [Azure depolama hesabı oluşturma](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) .

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Azure SYNAPSE Analytics 'te, bağlı bir hizmet, bağlantı bilgilerinizi diğer hizmetlere tanımladığınız yerdir. Bu bölümde, Azure SYNAPSE Analytics ve Azure Data Lake Gen 2 ' yi bağlı hizmetler olarak ekleyeceksiniz.

1. Azure SYNAPSE Analytics UX ' i açın ve **Yönet** sekmesine gidin.
1. **Dış bağlantılar**altında **bağlı hizmetler**' i seçin.
1. Bağlı bir hizmet eklemek için **Yeni**' ye tıklayın.
1. Listeden Azure Data Lake Storage 2. kutucuğunu seçin ve **devam**' a tıklayın.
1. Kimlik doğrulama kimlik bilgilerinizi girin. Hesap anahtarı, hizmet sorumlusu ve yönetilen kimlik, şu anda desteklenen kimlik doğrulama türleridir. Kimlik bilgilerinizin doğru olduğunu doğrulamak için Bağlantıyı Sına ' ya tıklayın. 
1. İşiniz bittiğinde **Oluştur** ' a tıklayın.

## <a name="create-pipeline"></a>İşlem hattı oluşturma

İşlem hattı, bir dizi etkinliğin yürütülmesi için mantıksal akışı içerir. Bu bölümde, Azure Data Lake Gen 2 ' den bir SQL havuzuna veri veren bir kopyalama etkinliği içeren bir işlem hattı oluşturacaksınız.

1. Organize **et sekmesine gidin** . ardışık düzen üstbilgisinin yanındaki artı simgesine tıklayın ve Işlem **hattı**' nı seçin.
1. Etkinlikler bölmesinde **taşıma ve dönüştürme** ' nın altında, verileri ardışık düzen tuvaline **Kopyala** ' yı sürükleyin.
1. Kopyalama etkinliğine tıklayın ve **kaynak** sekmesine gidin. yeni bir kaynak veri kümesi oluşturmak Için **Yeni** ' ye tıklayın.
1. Veri depolduğunuz Azure Data Lake Storage 2. seçin ve devam ' a tıklayın.
1. Biçim olarak DelimitedText ' i seçin ve devam ' a tıklayın.
1. Özellikleri ayarla bölmesinde, oluşturduğunuz ADLS bağlı hizmeti ' ni seçin. Kaynak verilerinizin dosya yolunu belirtin ve ilk satırın bir üst bilgisine sahip olup olmadığını belirtin. Şemayı dosya deposundan veya örnek bir dosyadan içeri aktarabilirsiniz. Bittiğinde Tamam ' a tıklayın.
1. **Havuz** sekmesine gidin. yeni bir havuz veri kümesi oluşturmak Için **Yeni** 'yi tıklatın.
1. Veri depolama alanı olarak Azure Data Lake Storage Gen2 seçin ve devam ' a tıklayın.
1. Biçim olarak DelimitedText ' i seçin ve devam ' a tıklayın.
1. Özellikleri ayarla bölmesinde, oluşturduğunuz ADLS bağlı hizmeti ' ni seçin. Veri yazmak istediğiniz klasörün yolunu belirtin. Bittiğinde Tamam ' a tıklayın.

## <a name="debug-and-publish-pipeline"></a>Hata ayıklama ve yayımlama işlem hattı

İşlem hattınızı yapılandırmayı tamamladıktan sonra, yapılarınızı yayımlamadan önce, her şeyin doğru olduğunu doğrulamak için bir hata ayıklama çalıştırması gerçekleştirebilirsiniz.

1. İşlem hattında hata ayıklamak için araç çubuğunda **Hata Ayıkla**'yı seçin. Pencerenin altındaki **Çıkış** sekmesinde işlem hattı çalıştırmasının durumu görüntülenir. 
1. İşlem hattı başarıyla çalıştırıldığında, üstteki araç çubuğunda **Tümünü Yayımla**' yı seçin. Bu eylem, oluşturduğunuz varlıkları (veri kümeleri ve işlem hatları) SYNAPSE Analytics hizmetinde yayımlar.
1. **Başarıyla yayımlandı** iletisini görene kadar bekleyin. Bildirim iletilerini görmek için sağ üst köşedeki zil düğmesine tıklayın. 


## <a name="trigger-and-monitor-the-pipeline"></a>İşlem hattını tetikleme ve izleme

Bu adımda, önceki adımda yayınlanan işlem hattını el ile tetiklersiniz. 

1. Araç çubuğunda **tetikleyici Ekle** ' yi seçin ve sonra **Şimdi Tetikle**' yi seçin. **İşlem Hattı Çalıştırma** sayfasında **Son**’u seçin.  
1. Sol kenar çubuğunda bulunan **izleyici** sekmesine gidin. El ile tetikleme tarafından tetiklenmiş bir işlem hattı çalıştırması görürsünüz. Etkinlik ayrıntılarını görüntülemek ve Işlem hattını yeniden çalıştırmak için **Eylemler** sütunundaki bağlantıları kullanabilirsiniz.
1. İşlem hattı çalıştırmalarıyla ilişkili etkinlik çalıştırmalarını görmek için **Eylemler** sütunundaki **Etkinlik Çalıştırmalarını Görüntüle** bağlantısını seçin. Bu örnekte yalnızca bir etkinlik bulunur, bu nedenle listede yalnızca bir giriş görürsünüz. Kopyalama işlemiyle ilgili ayrıntılar için **Eylemler** sütunundaki **Ayrıntılar** bağlantısını (gözlük simgesi) seçin. İşlem hattı çalıştırmaları görünümüne dönmek için üstteki işlem **hattı çalıştırmalarını** seçin. Görünümü yenilemek için **Yenile**’yi seçin.
1. Verilerinizin SQL havuzunda doğru yazıldığından emin olun.


## <a name="next-steps"></a>Sonraki adımlar

SYNAPSE Analytics için veri tümleştirmesi hakkında daha fazla bilgi için bkz. [verileri BIR SQL havuzuna dönüştürme](data-integration-sql-pool.md) .
