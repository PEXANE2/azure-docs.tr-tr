---
title: Azure SYNAPSE Studio 'da SQL betikleri (Önizleme)
description: Giriş Azure SYNAPSE Studio (Önizleme) SQL betikleri
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: 940c6d6d96c5c1aa062397d21ea96dace2c09bae
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81431078"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Azure SYNAPSE Studio 'da SQL betiği kullanma (Önizleme)

Azure SYNAPSE Studio (Önizleme), SQL sorgularını yazmak için bir SQL komut dosyası Web arabirimi sağlar. SQL Havuzu (Önizleme) veya SQL isteğe bağlı (Önizleme) bağlantısı yapabilirsiniz. 

## <a name="begin-authoring-in-sql-script"></a>SQL betikte yazmayı Başlat 

SQL komut dosyasında yazma deneyimini başlatmak için birkaç yol vardır. Aşağıdaki yöntemlerden birini kullanarak yeni bir SQL betiği oluşturabilirsiniz.

1. "+" Simgesini seçin ve SQL betiği ' ni seçin.

    > [!div class="mx-imgBorder"] 
    >![newsqlscript](./media/author-sql-script/newsqlscript.png)

2. SQL betikleri geliştirme altındaki Eylemler menüsünde, SQL betikleri geliştirme altında "eylemler" menüsünden "yeni SQL betiği" seçeneğini belirleyin. 

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript2actions.png)

or 

3. SQL betikleri geliştirme altında "eylemler" menüsünden "Içeri aktar" seçeneğini belirleyin ve yerel depoınızdan mevcut bir SQL betiğini seçin.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>SQL komut dosyanızı oluşturma

1. "Özellik" düğmesini seçerek ve SQL betiğine atanan varsayılan adı değiştirerek SQL komutlarınız için bir ad seçin.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlscriptrename.png)

1. Belirli SQL havuzunu veya "Bağlan" açılan menüsünden SQL isteğe bağlı ' yı seçin. Ya da gerekirse "veritabanını kullan" ' dan veritabanını seçin.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlchoosepool.png)

1. IntelliSense özelliğini kullanarak SQL komut dosyanızı yazmaya başlayın.

    > [!div class="mx-imgBorder"] 
    > ![newsqlscript](./media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>SQL komut dosyanızı çalıştırın

SQL betiğinizi yürütmek için "Çalıştır" düğmesini seçin. Sonuçlar bir tabloda varsayılan olarak görüntülenir.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Sonuçlarınızı dışarı aktarın

"Sonuçları dışarı aktar" seçeneğini belirleyerek ve uzantıyı seçerek, sonuçları yerel depolama verilerinize farklı biçimlerde (CSV, Excel, JSON, XML dahil) aktarabilirsiniz.

Ayrıca, "grafik" düğmesini seçerek SQL betiği sonuçlarını bir grafik ile görselleştirebilirsiniz. "Grafik türü" ve "Kategori sütunu" nı seçin. "Görüntü olarak Kaydet" i seçerek grafiği bir resme dışarı aktarabilirsiniz. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Bir Parquet dosyasından verileri araştırma.

Dosya içeriğini önizlemek için SQL betiği kullanarak bir depolama hesabındaki Parquet dosyalarını inceleyebilirsiniz. 

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL tabloları, dış tablolar, görünümler

Veriler altındaki "eylemler" menüsünü seçerek, "yeni SQL betiği", "en üstteki 1000 satırları Seç", "Oluştur", "bırak ve oluştur" gibi çeşitli eylemleri seçebilirsiniz. SQL havuzunun düğümlerine ve isteğe bağlı SQL 'e sağ tıklayıp kullanılabilir hareketi araştırın.

> [!div class="mx-imgBorder"] 
> ![newsqlscript](./media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL betiği yazma hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Analytics](https://docs.microsoft.com/azure/synapse-analytics).