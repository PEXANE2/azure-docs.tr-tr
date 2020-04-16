---
title: Azure Synapse Studio'da SQL komut dosyaları (önizleme)
description: Giriş Azure Synapse Studio (önizleme) SQL komut dosyaları
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: 940c6d6d96c5c1aa062397d21ea96dace2c09bae
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431078"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Azure Synapse Studio'da SQL komut dosyalarını kullanma (önizleme)

Azure Synapse Studio (önizleme), SQL sorguları yazmanız için bir SQL komut dosyası web arabirimi sağlar. SQL havuzuna (önizleme) veya isteğe bağlı SQL'e (önizleme) bağlanabilirsiniz. 

## <a name="begin-authoring-in-sql-script"></a>SQL komut dosyasında yazmaya başlayın 

SQL komut dosyasında yazma deneyimini başlatmanın birkaç yolu vardır. Aşağıdaki yöntemlerden biri aracılığıyla yeni bir SQL komut dosyası oluşturabilirsiniz.

1. "+" simgesini seçin ve SQL komut dosyasını seçin.

    > [!div class="mx-imgBorder"] 
    >![newsqlscript](./media/author-sql-script/newsqlscript.png)

2. SQL komut dosyalarını geliştir altındaki Eylemler menüsünden SQL komut dosyalarını geliştirin altındaki "Eylemler" menüsünden "Yeni SQL komut dosyası"nı seçin. 

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript2actions.png)

or 

3. SQL komut dosyalarını geliştir'in altındaki "Eylemler" menüsünden "İçe Aktarma"yı seçin ve yerel depolama alanınızdan varolan bir SQL komut dosyası seçin.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>SQL komut dosyanızı oluşturun

1. "Özellik" düğmesini seçerek ve SQL komut dosyasına atanan varsayılan adı değiştirerek SQL komut dosyanız için bir ad seçin.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscriptrename.png)

1. "Bağlan" açılır menüsünden belirli SQL havuzunu veya isteğe bağlı SQL havuzunu seçin. Veya gerekirse, "Veritabanını Kullan"dan veritabanını seçin.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlchoosepool.png)

1. Intellisense özelliğini kullanarak SQL komut dosyanızı yazmaya başlayın.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>SQL komut dosyanızı çalıştırma

SQL komut dosyanızı çalıştırmak için "Çalıştır" düğmesini seçin. Sonuçlar varsayılan olarak bir tabloda görüntülenir.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Sonuçlarınızı dışa aktarma

"Dışa aktarma sonuçları" seçerek ve uzantıyı seçerek sonuçları farklı biçimlerde (CSV, Excel, JSON, XML dahil) yerel depolama alanınıza dışa aktarabilirsiniz.

Ayrıca"Chart" düğmesini seçerek SQL komut dosyası sonuçlarını grafikte görselleştirebilirsiniz. "Grafik türü" ve "Kategori sütunu" seçeneğini belirleyin. "Görüntü olarak kaydet" seçeneğini seçerek grafiği bir resme dışa aktarabilirsiniz. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Parke dosyasından verileri keşfedin.

Dosya içeriğini önizlemek için SQL komut dosyası kullanarak bir depolama hesabında Parke dosyalarını keşfedebilirsiniz. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL Tablolar, dış tablolar, görünümler

Verilerin altındaki "Eylemler" menüsünü seçerek, "Yeni SQL komut dosyası", "TOP 1000 satırını seçin", "CREATE", "DROP and CREATE" gibi çeşitli eylemleri seçebilirsiniz. SQL havuzu ve isteğe bağlı SQL düğümlerini sağ tıklayarak kullanılabilir hareketi keşfedin.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL komut dosyası yazma hakkında daha fazla bilgi için [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)bölümüne bakın.