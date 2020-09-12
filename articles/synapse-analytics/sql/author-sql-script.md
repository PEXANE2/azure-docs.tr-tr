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
ms.openlocfilehash: 9d130c2a2db9ccead7180b6248398a84fcb34c3f
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89441247"
---
# <a name="using-sql-script-in-azure-synapse-studio-preview"></a>Azure SYNAPSE Studio 'da SQL betiği kullanma (Önizleme)

Azure SYNAPSE Studio (Önizleme), SQL sorgularını yazmak için bir SQL komut dosyası Web arabirimi sağlar. SQL Havuzu (Önizleme) veya SQL isteğe bağlı (Önizleme) bağlantısı yapabilirsiniz. 

## <a name="begin-authoring-in-sql-script"></a>SQL betikte yazmayı Başlat 

SQL komut dosyasında yazma deneyimini başlatmak için birkaç yol vardır. Aşağıdaki yöntemlerden birini kullanarak yeni bir SQL betiği oluşturabilirsiniz.

1. Geliştir menüsünde, **"+"** simgesini seçin ve **SQL betiği**' ni seçin.

![Yeni SQL betiği](media/author-sql-script/newsqlscript.png)

2. **Eylemler** menüsünden **Yeni SQL betiği**' ni seçin.
> [!div class="mx-imgBorder"]
> ![Yeni SQL betiği 2 eylemleri](media/author-sql-script/newsqlscript2actions.png)

Alternatif olarak şunları da yapabilirsiniz: 

3. SQL betikleri geliştirme altındaki **Eylemler** menüsünden **içeri aktar** ' ı seçin. Yerel depoınızdan mevcut bir SQL betiğini seçin.
![Yeni SQL betiği 3 eylemleri](media/author-sql-script/newsqlscript3actions.png)

## <a name="create-your-sql-script"></a>SQL komut dosyanızı oluşturma

1. **Özellik** düğmesini seçerek ve SQL betiğine atanan varsayılan adı değiştirerek SQL komutlarınız için bir ad seçin. 
![Yeni SQL betiği yeniden adlandırma](media/author-sql-script/newsqlscriptrename.png)

2. **Bağlan** açılan menüsünde belirli SQL havuzunu veya Isteğe bağlı SQL ' i seçin. Ya da gerekirse veritabanını **kullan**' ı seçin. 
![Yeni SQL seçme havuzu](media/author-sql-script/newsqlchoosepool.png)

3. IntelliSense özelliğini kullanarak SQL komut dosyanızı yazmaya başlayın.
![Yeni SQL IntelliSense](media/author-sql-script/newsqlintellisense.png)

## <a name="run-your-sql-script"></a>SQL komut dosyanızı çalıştırın

SQL betiğinizi yürütmek için **Çalıştır** düğmesini seçin. Sonuçlar bir tabloda varsayılan olarak görüntülenir.

![Yeni SQL betiği sonuçları tablosu](media/author-sql-script/newsqlscriptresultstable.png)

## <a name="export-your-results"></a>Sonuçlarınızı dışarı aktarın

"Sonuçları dışarı aktar" seçeneğini belirleyerek ve uzantıyı seçerek, sonuçları yerel depolama verilerinize farklı biçimlerde (CSV, Excel, JSON, XML dahil) aktarabilirsiniz.

Ayrıca, **grafik** düğmesini seçerek BIR grafikteki SQL betiği sonuçlarını görselleştirebilirsiniz. "Grafik türü" ve **Kategori sütununu**seçin. Resim **olarak kaydet**' i seçerek grafiği bir resme dışarı aktarabilirsiniz. 

![Yeni SQL betiği sonuçları grafiği](media/author-sql-script/newsqlscriptresultschart.png)

## <a name="explore-data-from-a-parquet-file"></a>Bir Parquet dosyasından verileri araştırma

Dosya içeriğini önizlemek için SQL betiği kullanarak bir depolama hesabındaki Parquet dosyalarını inceleyebilirsiniz.

![Yeni betik sqlod Parquet](media/author-sql-script/newscriptsqlodparquet.png)

## <a name="sql-tables-external-tables-views"></a>SQL tabloları, dış tablolar, görünümler

Veriler ' in altındaki **Eylemler** menüsünü seçerek, şöyle çeşitli eylemler seçebilirsiniz:

- Yeni SQL betiği
- En üstteki 1000 satırları seçin
- CREATE
- BıRAKMA ve oluşturma 
 
SQL havuzunun düğümlerine ve isteğe bağlı SQL 'e sağ tıklayıp kullanılabilir hareketi araştırın.
 
![Yeni betik veritabanı](media/author-sql-script/newscriptdatabase.png)

## <a name="next-steps"></a>Sonraki adımlar

SQL betiği yazma hakkında daha fazla bilgi için bkz. [Azure SYNAPSE Analytics](https://docs.microsoft.com/azure/synapse-analytics).
