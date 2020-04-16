---
title: Azure Synapse Studio (önizleme) dizüstü bilgisayarları oluşturun, geliştirin ve koruyun
description: Bu makalede, veri hazırlama ve görselleştirme yapmak için Azure Synapse Studio (önizleme) dizüstü bilgisayarlarını nasıl oluşturup geliştireceklerini öğrenirsiniz.
services: synapse analytics
author: ruixinxu
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: ruxu
ms.reviewer: ''
ms.openlocfilehash: 506339cefa90fb17bedfc946f70cb4d7d8047cf2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430233"
---
# <a name="create-develop-and-maintain-azure-synapse-studio-preview-notebooks"></a>Azure Synapse Studio (önizleme) dizüstü bilgisayarları oluşturun, geliştirin ve koruyun

Azure Synapse Studio (önizleme) dizüstü bilgisayarı, canlı kod, görselleştirmeler ve anlatı metni içeren dosyalar oluşturmanız için bir web arabirimidir. Dizüstü bilgisayarlar, fikirleri doğrulamak ve verilerinizden öngörüler almak için hızlı denemeler kullanmak için iyi bir yerdir. Dizüstü bilgisayarlar ayrıca veri hazırlama, veri görselleştirme, makine öğrenimi ve diğer Büyük Veri senaryolarında da yaygın olarak kullanılır.

Azure Synapse Studio dizüstü bilgisayar ile şunları yapabilirsiniz:

* Sıfır kurulum çabasıyla başlayın.
* Yerleşik kurumsal güvenlik özellikleriyle verileri güvende tutun.
* Verileri ham biçimler (CSV, txt, JSON, vb.), işlenmiş dosya biçimleri (parke, Delta Gölü, ORC, vb.) ve Sql tabular veri dosyaları arasında Analiz edin.
* Gelişmiş yazma özellikleri ve yerleşik veri görselleştirme ile üretken olun.

Bu makalede, Azure Synapse Studio'da not defterlerinin nasıl kullanılacağı açıklanmaktadır.

## <a name="create-a-notebook"></a>Not defteri oluşturma

Not defteri oluşturmanın iki yolu vardır. **Nesne**Gezgini'nden yeni bir not defteri oluşturabilir veya varolan bir not defterini Azure Synapse çalışma alanına aktarabilirsiniz. Azure Synapse Studio dizüstü bilgisayarlar standart Jupyter Notebook IPYNB dosyalarını tanıyabilir.

![synaps-oluşturma-alma-not defteri](./media/apache-spark-development-using-notebooks/synapse-create-import-notebook.png)

## <a name="develop-notebooks"></a>Dizüstü bilgisayar geliştirme

Not defterleri, bağımsız olarak veya grup olarak çalıştırılabilen tek tek kod veya metin blokları olan hücrelerden oluşur.

### <a name="add-a-cell"></a>Hücre ekleme

Not defterinize yeni bir hücre eklemenin birden çok yolu vardır.

1. Sol üst **+ Hücre** düğmesini genişletin ve kod hücresi ekle veya **metin hücresi ekle'yi**seçin. **Add code cell**

    ![hücre ile hücre ekle düğmesi](./media/apache-spark-development-using-notebooks/synapse-add-cell-1.png)

2. İki hücre arasındaki boşluğun üzerine girin ve **kod ekle** veya metin **ekle'yi**seçin.

    ![ara hücre ekleme](./media/apache-spark-development-using-notebooks/synapse-add-cell-2.png)

3. [Komut modu altında Kısayol tuşlarını](#shortcut-keys-under-command-mode)kullanın. Geçerli hücrenin üzerine bir hücre eklemek için **A** tuşuna basın. Geçerli hücrenin altına bir hücre eklemek için **B** tuşuna basın.

### <a name="set-a-primary-language"></a>Birincil dil ayarlama

Azure Synapse Studio dizüstü bilgisayarlar dört kıvılcım dili destekler:

* pyspark (piton)
* kıvılcım (Scala)
* sparkSQL
* Spark.NET (C#)

Üst komut çubuğundaki açılır listeden yeni eklenen hücreler için birincil dili ayarlayabilirsiniz.

   ![varsayılan-sinaps dili](./media/apache-spark-development-using-notebooks/synapse-default-language.png)

### <a name="use-multiple-languages"></a>Birden çok dil kullanma

Bir hücrenin başında doğru dil sihirli komutunu belirterek tek bir not defterinde birden çok dil kullanabilirsiniz. Aşağıdaki tabloda hücre dillerinde geçiş yapmak için sihirli komutlar listelenilmektedir.

|Sihirli komut |Dil | Açıklama |  
|---|------|-----|
|%pyspark| Python | Spark Context'a karşı bir **Python** sorgusu yürütün.  |
|%kıvılcım| Scala | Kıvılcım Bağlamı'na karşı bir **Scala** sorgusu yürütün.  |  
|%sql| SparkSQL | Spark Context'a karşı bir **SparkSQL** sorgusu yürütün.  |
|%csharp % | Spark.NET C # | Kıvılcım Bağlamı'na karşı **Spark.NET c#** sorgusu yürütün. |

Aşağıdaki resim, **%%pyspark** sihirli komutunu veya **Spark(Scala)** not defterindeki **%sql** sihirli komutuyla Bir SparkSQL sorgusunu kullanarak pyspark sorgusunu nasıl yazabileceğinize bir örnektir. Not defterinin ana dilinin Scala olarak ayarlı olduğuna dikkat edin.

   ![sinaps-kıvılcım-büyü](./media/apache-spark-development-using-notebooks/synapse-spark-magics.png)

### <a name="use-temp-tables-to-reference-data-across-languages"></a>Diller arasında veri referansı için geçici tabloları kullanma

Synapse Studio not defterinde verilere veya değişkenlere doğrudan farklı dillerde başvuru yapamazsınız. Spark'ta, diller arasında geçici bir tabloya başvurulabilir. Burada, geçici çözüm olarak `Scala` Bir DataFrame'in içinde `PySpark` ve `SparkSQL` spark geçici tablosunda nasıl okunduğuna bir örnek verilmiştir.

1. Hücre 1'de, Scala kullanarak SQL havuz bağlayıcısından bir DataFrame okuyun ve geçici bir tablo oluşturun.

   ```scala
   %%scala
   val scalaDataFrame = spark.read.option("format", "DW connector predefined type")
   scalaDataFrame.registerTempTable( "mydataframetable" )
   ```

2. Hücre 2'de, Verileri Spark SQL kullanarak sorgula.
   
   ```sql
   %%sql
   SELECT * FROM mydataframetable
   ```

3. Hücre 3'te, PySpark'taki verileri kullanın.

   ```python
   %%pyspark
   myNewPythonDataFrame = spark.sql("SELECT * FROM mydataframetable")
   ```

### <a name="ide-style-intellisense"></a>IDE tarzı IntelliSense

Azure Synapse Studio dizüstü bilgisayarlar, IDE tarzı IntelliSense'i hücre düzenleyicisine getirmek için Monako düzenleyicisiyle entegre edilmiştir. Sözdizimi vurgulanması, hata oluşturucu ve otomatik kod tamamlamaları kod yazmanıza ve sorunları daha hızlı belirlemenize yardımcı olur.

IntelliSense özellikleri farklı diller için farklı olgunluk düzeylerindedir. Desteklenenleri görmek için aşağıdaki tabloyu kullanın.

|Diller| Sözdizimi Vurgulama | Sözdizimi Hata İşaretleyici  | Sözdizimi Kodu Tamamlama | Değişken Kod Tamamlama| Sistem İşlev Kodu Tamamlama| Kullanıcı İşlev Kodu Tamamlama| Akıllı Girinti | Kod Katlama|
|--|--|--|--|--|--|--|--|--|
|PySpark (Python)|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Kıvılcım (Scala)|Evet|Evet|Evet|Evet|-|-|-|Evet|
|SparkSQL|Evet|Evet|-|-|-|-|-|-|
|Spark.NET (C#)|Evet|-|-|-|-|-|-|-|

### <a name="format-text-cell-with-toolbar-buttons"></a>Metin hücreni araç çubuğu düğmeleriyle biçimlendirme

Ortak işaretleme eylemleri yapmak için metin hücreleri araç çubuğundaki biçim düğmelerini kullanabilirsiniz. Kalın yazı, metni italikleştirme, kod parçacıkları ekleme, sıralanmamış liste ekleme, sıralı liste ekleme ve URL'den resim ekleme içerir.

  ![sinaps-metin-hücre-araç çubuğu](./media/apache-spark-development-using-notebooks/synapse-text-cell-toolbar.png)

### <a name="undo-cell-operations"></a>Hücre işlemlerini geri ala
En son hücre işlemini iptal etmek için **geri alma** düğmesini tıklatın veya **Ctrl+Z** tuşuna basın. Şimdi en son 20 tarihsel hücre eylemleri kadar geri alabilirsiniz. 

   ![sinaps-geri-hücreler](./media/apache-spark-development-using-notebooks/synapse-undo-cells.png)

### <a name="move-a-cell"></a>Hücreyi taşıma

En sağdaki ek hücre eylemleri menüsüne erişmek için elipsleri (...) seçin. Ardından, geçerli hücreyi taşımak için **hücreyi yukarı veya** **aşağı taşı'yı** seçin. 

Komut modu [altında kısayol tuşlarını](#shortcut-keys-under-command-mode)da kullanabilirsiniz. Geçerli hücreyi yukarı taşımak için **Ctrl+Alt+↑** tuşuna basın. Geçerli hücreyi aşağı taşımak için **Ctrl+Alt+.** tuşuna basın.

   ![hareket-a-hücre](./media/apache-spark-development-using-notebooks/synapse-move-cells.png)

### <a name="delete-a-cell"></a>Hücreyi silme

Bir hücreyi silmek için, en sağdaki ek hücre eylemleri menüsüne erişmek için elipsleri (...) seçin ve hücreyi **sil'i**seçin. 

Komut modu [altında kısayol tuşlarını](#shortcut-keys-under-command-mode)da kullanabilirsiniz. Geçerli hücreyi silmek için **D,D** tuşuna basın.
  
   ![delete-a-cell](./media/apache-spark-development-using-notebooks/synapse-delete-cell.png)

### <a name="collapse-a-cell-input"></a>Hücre girişini daraltma
Daraltmak için geçerli hücrenin altındaki ok düğmesini tıklatın. Genişletmek için, hücre daraltılırken ok düğmesini tıklatın.

   ![çökme hücre girişi](./media/apache-spark-development-using-notebooks/synapse-collapse-cell-input.gif)

### <a name="collapse-a-cell-output"></a>Hücre çıktısını daraltma

Daraltmak için geçerli hücre çıkışının sol üst kısmındaki **daraltma çıkış** düğmesini tıklatın. Genişletmek için, hücre çıkışı daraltılırken **hücre çıktısını göster'i** tıklatın.

   ![çökme hücre çıkışı](./media/apache-spark-development-using-notebooks/synapse-collapse-cell-output.gif)

## <a name="run-notebooks"></a>Not defterlerini çalıştırma

Not defterinizdeki kod hücrelerini tek tek veya hepsini aynı anda çalıştırabilirsiniz. Her hücrenin durumu ve ilerlemesi not defterinde temsil edilir.

### <a name="run-a-cell"></a>Hücre çalıştırma

Bir hücrede kodu çalıştırmanın birkaç yolu vardır.

1. Çalıştırmak istediğiniz hücreye girin ve **Hücre çalıştır** düğmesini seçin veya **Ctrl+Enter**tuşuna basın.

   ![run-cell-1](./media/apache-spark-development-using-notebooks/synapse-run-cell.png)


2. En sağdaki ek hücre eylemleri menüsüne erişmek için elipsleri (**...**) seçin. Ardından, **Hücreyi Çalıştır'ı**seçin.

   ![run-cell-2](./media/apache-spark-development-using-notebooks/synapse-run-cell-2.png)
   
3. [Komut modu altında Kısayol tuşlarını](#shortcut-keys-under-command-mode)kullanın. Geçerli hücreyi çalıştırmak için **Shift+Enter** tuşuna basın ve aşağıdaki hücreyi seçin. Geçerli hücreyi çalıştırmak için **Alt+Enter** tuşuna basın ve aşağıdaki yeni bir hücre ekleyin.


### <a name="run-all-cells"></a>Tüm hücreleri çalıştırın
Geçerli not defterindeki tüm hücreleri sırayla çalıştırmak için **Tümlerini Çalıştır** düğmesini tıklatın.

   ![run-all-cells](./media/apache-spark-development-using-notebooks/synapse-run-all.png)

### <a name="run-all-cells-above-or-below"></a>Tüm hücreleri yukarıda veya aşağıda çalıştırın

En sağdaki ek hücre eylemleri menüsüne erişmek için elipsleri (**...**) seçin. Ardından, akımın üzerindeki tüm hücreleri sırayla çalıştırmak için **yukarıdaki hücreleri çalıştır'ı** seçin. Akımın altındaki tüm hücreleri sırayla çalıştırmak için **aşağıdaki hücreleri Çalıştır'ı** seçin.

   ![run-cells-above-or-below](./media/apache-spark-development-using-notebooks/synapse-run-cells-above-or-below.png)


### <a name="cell-status-indicator"></a>Hücre durum göstergesi

Geçerli ilerlemesini görmenize yardımcı olmak için hücrenin altında adım adım hücre yürütme durumu görüntülenir. Hücre çalışması tamamlandıktan sonra, toplam süre ve bitiş saatini içeren bir yürütme özeti gösterilir ve gelecekteki başvuru için orada tutulur.

![hücre durumu](./media/apache-spark-development-using-notebooks/synapse-cell-status.png)

### <a name="spark-progress-indicator"></a>Kıvılcım ilerleme göstergesi

Azure Synapse Studio dizüstü bilgisayar tamamen Spark tabanlıdır. Kod hücreleri Spark havuzunda uzaktan yürütülür. Bir Spark iş ilerleme göstergesi, iş yürütme durumunu anlamanıza yardımcı olacak bir gerçek zamanlı ilerleme çubuğuyla sağlanır.


![kıvılcım-ilerleme göstergesi](./media/apache-spark-development-using-notebooks/synapse-spark-progress-indicator.png)

### <a name="spark-session-config"></a>Kıvılcım oturumu config

Yapılandırma oturumunda geçerli Spark oturumuna verecek zaman aşım süresini, sayısını ve yürütücülerin boyutunu **belirtebilirsiniz.** Spark oturumunu yeniden başlatma, yapılandırma değişikliklerinin etkili olması içindir. Önbelleğe alınmış tüm not defteri değişkenleri temizlenir.

![oturum-mgmt](./media/apache-spark-development-using-notebooks/synapse-spark-session-mgmt.png)


## <a name="bring-data-to-a-notebook"></a>Not defterine veri getirme

Aşağıdaki kod örneklerinde gösterildiği gibi Azure Blob Depolama, Azure Veri Gölü Deposu Gen 2 ve SQL havuzundan veri yükleyebilirsiniz.

### <a name="read-a-csv-from-azure-data-lake-store-gen2-as-a-spark-dataframe"></a>Azure Veri Gölü Deposu Gen2'den Bir CSV'yi Spark DataFrame olarak okuyun

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import *
account_name = "Your account name"
container_name = "Your container name"
relative_path = "Your path"
adls_path = 'abfss://%s@%s.dfs.core.windows.net/%s' % (blob_container_name, blob_account_name,  blob_relative_path)

spark.conf.set("fs.azure.account.auth.type.%s.dfs.core.windows.net" %account_name, "SharedKey")
spark.conf.set("fs.azure.account.key.%s.dfs.core.windows.net" %account_name ,"Your ADLSg2 Primary Key")

df1 = spark.read.option('header', 'true') \
                .option('delimiter', ',') \
                .csv(adls_path + '/Testfile.csv')

```

#### <a name="read-a-csv-from-azure-blob-storage-as-a-spark-dataframe"></a>Azure Blob Depolama'dan Bir CSV'yi Kıvılcım Veri Çerçevesi olarak okuyun

```python

from pyspark.sql import SparkSession
from pyspark.sql.types import *

blob_account_name = "Your blob account name"
blob_container_name = "Your blob container name"
blob_relative_path = "Your blob relative path"
blob_sas_token = "Your blob sas token"

wasbs_path = 'wasbs://%s@%s.blob.core.windows.net/%s' % (blob_container_name, blob_account_name, blob_relative_path)
spark.conf.set('fs.azure.sas.%s.%s.blob.core.windows.net' % (blob_container_name, blob_account_name), blob_sas_token)

df = spark.read.option("header", "true") \
            .option("delimiter","|") \
            .schema(schema) \
            .csv(wasbs_path)

```

### <a name="read-data-from-the-primary-storage-account"></a>Birincil depolama hesabından verileri okuma

Birincil depolama hesabındaki verilere doğrudan erişebilirsiniz. Gizli anahtarları vermeye gerek yok. Veri Gezgini'nde, bir dosyaya sağ tıklayın ve veri ayıklayıcıotomatik olarak oluşturulmuş yeni bir dizüstü bilgisayar görmek için **Yeni not defterini** seçin.

![veri-hücre](./media/apache-spark-development-using-notebooks/synapse-data-to-cell.png)

## <a name="visualize-data-in-a-notebook"></a>Not defterindeki verileri görselleştirin

### <a name="display"></a>Görüntüle()

Bir çubuk grafik, çizgi grafiği, pasta grafiği, dağılım grafiği ve alan grafiği oluşturma seçeneği yle bir tablo sonuçları görünümü sağlanır. Kod yazmak zorunda kalmadan verilerinizi görselleştirebilirsiniz. Grafikler **Grafik Seçenekleri'nde**özelleştirilebilir. 

**%sql** magic komutlarının çıktısı varsayılan olarak işlenen tablo görünümünde görünür. İşlenen tablo görünümünü oluşturmak için Spark DataFrames veya Esnek Dağıtılmış Veri Kümeleri (RDD) işlevinde **display()`<DataFrame name>`** olarak adlandırabilirsiniz.

   ![yerleşik grafikler](./media/apache-spark-development-using-notebooks/synapse-builtin-charts.png)

### <a name="displayhtml"></a>DisplayHTML()

HTML veya etkileşimli kitaplıklar işleyebilir, **bokeh**gibi, **displayHTML()** kullanarak.

Aşağıdaki resim **bokeh**kullanarak bir harita üzerinde glifler ihya bir örnektir.

   ![bokeh örnek](./media/apache-spark-development-using-notebooks/synapse-bokeh-image.png)
   

Yukarıdaki resmi çizmek için aşağıdaki örnek kodu çalıştırın.

```python
from bokeh.plotting import figure, output_file
from bokeh.tile_providers import get_provider, Vendors
from bokeh.embed import file_html
from bokeh.resources import CDN
from bokeh.models import ColumnDataSource

tile_provider = get_provider(Vendors.CARTODBPOSITRON)

# range bounds supplied in web mercator coordinates
p = figure(x_range=(-9000000,-8000000), y_range=(4000000,5000000),
           x_axis_type="mercator", y_axis_type="mercator")
p.add_tile(tile_provider)

# plot datapoints on the map
source = ColumnDataSource(
    data=dict(x=[ -8800000, -8500000 , -8800000],
              y=[4200000, 4500000, 4900000])
)

p.circle(x="x", y="y", size=15, fill_color="blue", fill_alpha=0.8, source=source)

# create an html document that embeds the Bokeh plot
html = file_html(p, CDN, "my plot1")

# display this html
displayHTML(html)

```

## <a name="save-notebooks"></a>Not defterlerini kaydet

Çalışma alanınızda tek bir not defterini veya tüm dizüstü bilgisayarları kaydedebilirsiniz.

1. Tek bir not defterine yaptığınız değişiklikleri kaydetmek için, not defteri komut çubuğundaki **Yayımla** düğmesini seçin.

   ![yayımlama-defter](./media/apache-spark-development-using-notebooks/synapse-publish-notebook.png)

2. Çalışma alanınızdaki tüm not defterlerini kaydetmek için, çalışma alanı komut çubuğundaki **tümünü yayımla** düğmesini seçin. 

   ![yayımlama-tüm](./media/apache-spark-development-using-notebooks/synapse-publish-all.png)

Not defteri özelliklerinde, tasarruf yaparken hücre çıktısını dahil edip etmeyeceğini yapılandırabilirsiniz.

   ![not defteri özellikleri](./media/apache-spark-development-using-notebooks/synapse-notebook-properties.png)

## <a name="magic-commands"></a>Sihirli komutlar
Azure Synapse Studio dizüstü bilgisayarlarında tanıdık Jupyter sihirli komutlarınızı kullanabilirsiniz. Mevcut sihirli komutları olarak aşağıdaki listeyi kontrol edin. İhtiyaçlarınızı karşılamak için daha fazla sihirli komut oluşturmaya devam edebilmemiz için GitHub'daki kullanım durumlarınızı bize bildirin.

Mevcut hat büyüleri: [%lsmagic](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-lsmagic), [%zaman](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [%timeit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit)

Kullanılabilir hücre büyüleri: [%%zaman](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time), [%%zamanit](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), [%yakalama](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-capture), [%%writefile](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-writefile), [%sql](#use-multiple-languages), [%%pyspark](#use-multiple-languages), [%kıvılcım](#use-multiple-languages), [%csharp](#use-multiple-languages)

## <a name="shortcut-keys"></a>Kısayol tuşları

Jupyter Notebook'lara benzer şekilde Azure Synapse Studio dizüstü bilgisayarlarda da modal kullanıcı arabirimi bulunur. Klavye, dizüstü bilgisayar hücresinin hangi modda olduğuna bağlı olarak farklı şeyler yapar. Synapse Studio dizüstü bilgisayarlar belirli bir kod hücresi için aşağıdaki iki modu destekler: komut modu ve edit modu.

1. Bir hücre, yazmanızı isteyen metin imleci olmadığında komut modundadır. Bir hücre Komut modundayken, not defterini bir bütün olarak ancak tek tek hücrelere yazmama olarak demezsiniz. Bir hücrenin `ESC` düzenleyici alanının dışına tıklamak için fareye basarak veya kullanarak komut moduna girin.

   ![komut modu](./media/apache-spark-development-using-notebooks/synapse-command-mode2.png)

2. Düzenleme modu, düzenleyici alanına yazmanızı isteyen bir metin imleci yle gösterilir. Bir hücre edit modundayken, hücreye yazamazsınız. Bir hücrenin düzenleyici `Enter` alanına tıklamak için fareye basarak veya kullanarak düzenleme moduna girin.
   
   ![düzenleme modu](./media/apache-spark-development-using-notebooks/synapse-edit-mode2.png)

### <a name="shortcut-keys-under-command-mode"></a>Komut modu altında kısayol tuşları

Aşağıdaki tuş vuruşu kısayollarını kullanarak Azure Synapse dizüstü bilgisayarlarında kodda daha kolay gezinebilir ve kod çalıştırabilirsiniz.

| Eylem |Synapse Studio dizüstü bilgisayar Kısayolları  |
|--|--|
|Geçerli hücreyi çalıştırın ve aşağıdakileri seçin | Shift+Enter |
|Geçerli hücreyi çalıştırın ve aşağıdaki ekleme | Alt+Enter |
|Yukarıdaki hücreyi seçin| Yukarı |
|Aşağıdaki hücreyi seçin| Aşağı |
|Yukarıdaki hücre ekleme| A |
|Aşağıdaki hücreekleme| B |
|Seçili hücreleri yukarıda genişletme| Shift+Up |
|Seçili hücreleri aşağıda genişletme| Shift+Down|
|Hücreyi yukarı taşıma| Ctrl+Alt+↑ |
|Hücreyi aşağı taşıma| Ctrl+Alt+. |
|Seçili hücreleri silme| D, D |
|Edit moduna geçme| Enter |

### <a name="shortcut-keys-under-edit-mode"></a>Edit modu altında kısayol tuşları

Aşağıdaki tuş vuruşu kısayollarını kullanarak, Azure Synapse not defterlerinde kodu edit modundayken daha kolay gezinebilir ve çalıştırabilirsiniz.

| Eylem |Synapse Studio dizüstü bilgisayar kısayolları  |
|--|--|
|İmleci yukarı taşıma | Yukarı |
|İmleci aşağı taşı|Aşağı|
|Geri al|Ctrl + Z|
|Yinele|Ctrl + Y|
|Yorum Yap/Yorum yap|Ctrl + /|
|Önce sözcüğü silme|Ctrl + Arka Boşluk|
|Sözcüğü sonra silme|Ctrl + Sil|
|Hücre başlangıcına git|Ctrl + Ana Sayfa|
|Hücre sonuna gitme |Ctrl + Bitiş|
|Bir kelime sola git|Ctrl + Sol|
|Bir kelime sağa git|Ctrl + Sağ|
|Tümünü seç|Ctrl + A|
|Girinti| Ctrl + ]|
|Dedent|Ctrl + [|
|Komut moduna geçme| Esc |

## <a name="next-steps"></a>Sonraki adımlar

- [.NET Apache Spark belgeleri için](/dotnet/spark?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
- [Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics)
