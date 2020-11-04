---
title: Azure SYNAPSE Studio 'da SQL betikleri (Önizleme)
description: Azure SYNAPSE Studio 'ya giriş (Önizleme) SQL betikleri
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.subservice: sql
ms.topic: conceptual
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: omafnan
ms.openlocfilehash: 3618ba28403882ee1f949a2bbc97e586674b3772
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93317507"
---
# <a name="using-sql-scripts-in-azure-synapse-studio-preview"></a>Azure SYNAPSE Studio 'da SQL betikleri kullanma (Önizleme)

Azure SYNAPSE Studio (Önizleme), SQL sorgularını yazmak için bir SQL komut dosyası Web arabirimi sağlar. SQL havuzuna (Önizleme) bağlanabilirsiniz. 

## <a name="begin-authoring-in-sql-script"></a>SQL betikte yazmayı Başlat 

SQL komut dosyasında yazma deneyimini başlatmak için birkaç yol vardır. Aşağıdaki yöntemlerden birini kullanarak yeni bir SQL betiği oluşturabilirsiniz.

1. Geliştir menüsünde, **"+"** simgesini seçin ve **SQL betiği** ' ni seçin.

2. **Eylemler** menüsünden **Yeni SQL betiği** ' ni seçin.

3. SQL betikleri geliştirme altındaki **Eylemler** menüsünden **içeri aktar** ' ı seçin. Yerel depoınızdan mevcut bir SQL betiğini seçin.
![Yeni SQL betiği 3 eylemleri](media/author-sql-script/new-sql-script-3-actions.png)

## <a name="create-your-sql-script"></a>SQL komut dosyanızı oluşturma

1. **Özellik** düğmesini seçerek ve SQL betiğine atanan varsayılan adı değiştirerek SQL komutlarınız için bir ad seçin. 
![Yeni SQL betiği yeniden adlandırma](media/author-sql-script/new-sql-script-rename.png)

2. **Bağlan** açılan menüsünden belırlı adanmış SQL havuzunu veya SUNUCUSUZ SQL havuzunu seçin. Ya da gerekirse veritabanını **kullan** ' ı seçin. 
![Yeni SQL seçme havuzu](media/author-sql-script/new-sql-choose-pool.png)

3. IntelliSense özelliğini kullanarak SQL komut dosyanızı yazmaya başlayın.

## <a name="run-your-sql-script"></a>SQL komut dosyanızı çalıştırın

SQL betiğinizi yürütmek için **Çalıştır** düğmesini seçin. Sonuçlar bir tabloda varsayılan olarak görüntülenir.

![Yeni SQL betiği sonuçları tablosu](media/author-sql-script/new-sql-script-results-table.png)

## <a name="export-your-results"></a>Sonuçlarınızı dışarı aktarın

"Sonuçları dışarı aktar" seçeneğini belirleyerek ve uzantıyı seçerek, sonuçları yerel depolama verilerinize farklı biçimlerde (CSV, Excel, JSON, XML dahil) aktarabilirsiniz.

Ayrıca, **grafik** düğmesini seçerek BIR grafikteki SQL betiği sonuçlarını görselleştirebilirsiniz. "Grafik türü" ve **Kategori sütununu** seçin. Resim **olarak kaydet** ' i seçerek grafiği bir resme dışarı aktarabilirsiniz. 

![Yeni SQL betiği sonuçları grafiği](media/author-sql-script/new-sql-script-results-chart.png)

## <a name="explore-data-from-a-parquet-file"></a>Bir Parquet dosyasından verileri araştırma

Dosya içeriğini önizlemek için SQL betiği kullanarak bir depolama hesabındaki Parquet dosyalarını inceleyebilirsiniz.

![Yeni betik sqlod Parquet](media/author-sql-script/new-script-sqlod-parquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL tabloları, dış tablolar, görünümler

Veriler ' in altındaki **Eylemler** menüsünü seçerek, şöyle çeşitli eylemler seçebilirsiniz:

- Yeni SQL betiği
- En üstteki 1000 satırları seçin
- CREATE
- BıRAKMA ve oluşturma 
 
SQL veritabanlarının düğümlerine sağ tıklayarak kullanılabilir hareketi keşfedebilirsiniz.
 
![Yeni betik veritabanı](media/author-sql-script/new-script-database.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL betiği yazma hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Analytics](https://docs.microsoft.com/azure/synapse-analytics).
