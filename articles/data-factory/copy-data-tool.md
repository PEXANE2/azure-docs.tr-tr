---
title: Veri kopyalama aracı Azure Veri Fabrikası
description: Azure Veri Fabrikası Kullanıcı Arabirimi'nde Veri Kopyalama aracı hakkında bilgi sağlar
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/18/2018
ms.author: yexu
ms.openlocfilehash: df078673aed60086a88961ff64f9bfa596d96346
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414096"
---
# <a name="copy-data-tool-in-azure-data-factory"></a>Azure Data Factory'deki Veri Kopyalama aracı
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Veri Fabrikası Kopyalama Verileri aracı, genellikle uçtan uca veri tümleştirme senaryosunun ilk adımı olan verileri veri gölüne alma işlemini kolaylaştırır ve optimize eder.  Özellikle bir veri kaynağından ilk kez veri almak için Azure Veri Fabrikası'nı kullandığınızda zamandan tasarruf sağlar. Bu aracı kullanmanın avantajlarından bazıları şunlardır:

- Azure Veri Fabrikası Kopyalama Veri aracını kullanırken, bağlantılı hizmetler, veri kümeleri, ardışık hatlar, etkinlikler ve tetikleyiciler için Veri Fabrikası tanımlarını anlamanız gerekmez. 
- Veri Kopyalama aracının akışı, verileri bir veri gölüne yüklemek için sezgiseldir. Araç, seçili kaynak veri deposundan seçili hedef/lavabo veri deposuna veri kopyalamak için gerekli tüm Veri Fabrikası kaynaklarını otomatik olarak oluşturur. 
- Verileri Kopyala aracı, yazarlık sırasında yutulan verileri doğrulamanıza yardımcı olur ve bu da başlangıçta olası hataları önlemenize yardımcı olur.
- Verileri bir veri gölüne yüklemek için karmaşık iş mantığı uygulamanız gerekiyorsa, Veri Fabrikası Web'de yazma başına yazma yı kullanarak Veri Veri aracı tarafından oluşturulan Veri Fabrikası kaynaklarını yine de düzenlemeniz gerekir. 

Aşağıdaki tablo, Veri Fabrikası Kullanıcı Arabirimi'nde yazma ile Etkinlik Başına Kopyalama aracının ne zaman kullanılacağı na ilişkin kılavuz sağlar: 

| Veri Kopyalama aracı | Etkinlik başına (Kopyalama etkinliği) yazma |
| -------------- | -------------------------------------- |
| Azure Veri Fabrikası varlıkları (bağlantılı hizmetler, veri kümeleri, boru hatları, vb.) hakkında bilgi edinmeden kolayca veri yükleme görevi oluşturmak istiyorsunuz | Verileri göle yüklemek için karmaşık ve esnek bir mantık uygulamak istiyorsunuz. |
| Çok sayıda veri yapısını bir veri gölüne hızla yüklemek istiyorsunuz. | Verileri temizlemek veya işlemek için sonraki etkinliklerle Kopyalama etkinliğini zincirlemek istiyorsunuz. |

Veri Kopyala aracını başlatmak için, veri fabrikanızın ana sayfasındaki **Veri Kopyala'yı** tıklatın.

![Başlangıç sayfasına geçin - Verileri Kopyala aracına bağlantı](./media/copy-data-tool/get-started-page.png)


## <a name="intuitive-flow-for-loading-data-into-a-data-lake"></a>Veri gölüne veri yüklemek için sezgisel akış
Bu araç, sezgisel bir akışla çok çeşitli kaynaklardan belirli bilgileri dakikalar içinde varış noktalarına kolayca taşımanızı sağlar:  

1. **Kaynak**için ayarları yapılandırın.
2. **Hedef**için ayarları yapılandırın. 
3. Sütun eşleme, performans ayarları ve hata toleransı ayarları gibi kopyalama işlemi için **gelişmiş ayarları** yapılandırın. 
4. Veri yükleme görevi için bir **zamanlama** belirtin. 
5. Oluşturulacak Veri Fabrikası varlıklarının **özetini** gözden geçirin. 
6. Kopya etkinliği ayarlarını gerektiği gibi güncelleştirmek için ardışık hattı **edin.** 

   Araç, farklı veri ve nesne türleri için destek ile, başından itibaren akılda büyük veri ile tasarlanmıştır. Yüzlerce klasör, dosya veya tablotaşımak için kullanabilirsiniz. Araç otomatik veri önizleme, şema yakalama ve otomatik haritalama ve veri filtreleme de destekler.

![Veri Kopyalama aracı](./media/copy-data-tool/copy-data-tool.png)

## <a name="automatic-data-preview"></a>Otomatik veri önizleme
Verilerin bir kısmını, kopyalanan verileri doğrulamanızı sağlayan seçili kaynak veri deposundan önizleyebilirsiniz. Ayrıca, kaynak veriler bir metin dosyasındaysa, Veri Kopyala aracı metin dosyasını ayrıştırır ve satır ve sütun sınırlayıcıları ve şemayı otomatik olarak algılar.

![Dosya ayarları](./media/copy-data-tool/file-format-settings.png)

Tespitten sonra:

![Algılanan dosya ayarları ve önizleme](./media/copy-data-tool/after-detection.png)

## <a name="schema-capture-and-automatic-mapping"></a>Şema yakalama ve otomatik haritalama
Veri kaynağının şeması birçok durumda veri hedefişemasıyla aynı olmayabilir. Bu senaryoda, sütunları kaynak şemadaki sütunları hedef şemadaki sütunlara eşlemeniz gerekir.

Verileri Kopyala aracı, kaynak ve hedef mağazalar arasında sütunları eşlerken davranışınızı izler ve öğrenir. Kaynak veri deposundan bir veya birkaç sütun seçtikten ve bunları hedef şemaya eşledikten sonra, Veri Kopyala aracı her iki taraftan seçtiğiniz sütun çiftleri için deseni çözümlemeye başlar. Daha sonra, sütunların geri kalanına aynı deseni uygular. Bu nedenle, tüm sütunların hedefe birkaç tıklamadan hemen sonra istediğiniz şekilde eşlendiğini görürsünüz.  Veri Kopyala aracı tarafından sağlanan sütun eşleme seçeneğinden memnun değilseniz, bunu yoksayabilir ve sütunları el ile eşleyerek devam edebilirsiniz. Bu arada, Veri Kopyalama aracı sürekli olarak deseni öğrenir ve günceller ve sonuçta ulaşmak istediğiniz sütun eşleme için doğru desenine ulaşır. 

> [!NOTE]
> SQL Server veya Azure SQL Veritabanı'ndan Azure SQL Veri Ambarı'na veri kopyalanırken, tablo hedef deposunda yoksa, Veri Kopyalama aracı kaynak şemayı kullanarak tablonun oluşturulmasını otomatik olarak destekler. 

## <a name="filter-data"></a>Verileri filtreleme
Yalnızca lavabo veri deposuna kopyalanması gereken verileri seçmek için kaynak verilerine filtre uygulayabilirsiniz. Filtreleme, lavabo veri deposuna kopyalanacak verilerin hacmini azaltır ve bu nedenle kopyalama işleminin veri hacmini artırır. Veri Kopyalama aracı, SQL sorgu dilini veya Azure blob klasöründeki dosyaları kullanarak ilişkisel bir veritabanındaki verileri filtrelemek için esnek bir yol sağlar. 

### <a name="filter-data-in-a-database"></a>Veritabanındaki verileri filtreleme
Aşağıdaki ekran görüntüsü, verileri filtrelemek için bir SQL sorgusu gösterir.

![Veritabanındaki verileri filtreleme](./media/copy-data-tool/filter-data-in-database.png)

### <a name="filter-data-in-an-azure-blob-folder"></a>Azure blob klasöründeki verileri filtreleme
Klasördeki verileri kopyalamak için klasör yolundaki değişkenleri kullanabilirsiniz. Desteklenen değişkenler şunlardır: **{year}**, **{ay}**, **{gün}**, **{hour}** ve **{dakika}**. Örneğin: inputfolder/{year}/{month}/{day}. 

Aşağıdaki biçimde giriş klasörleri olduğunu varsayalım: 

```
2016/03/01/01
2016/03/01/02
2016/03/01/03
...
```

**Dosya veya klasör**için **Gözat** düğmesini tıklatın, bu klasörlerden birine göz atın (örneğin, 2016->03->01->02) ve **Seç'i**tıklatın. 2016/03/01/02 metin kutusunda görmelisiniz. 

Ardından, **2016'yı** **{year}**, **03** ile **{month},** **01'i** **{day}** ve **02'yi** **{hour}** ile değiştirin ve **Sekme** tuşuna basın. Bu dört değişkenin biçimini seçmek için açılır listeler görmelisiniz:

![Filtre dosyası veya klasör](./media/copy-data-tool/filter-file-or-folder.png)

Veri Kopyalama aracı, ardışık düzen oluştururken {year}, {month}, {day}, {hour}ve {minute} ifadelerini, işlevleri ni temsil etmek için kullanılabilecek parametreler oluşturur.

## <a name="scheduling-options"></a>Zamanlama seçenekleri
Kopyalama işlemini bir kez veya bir zamanlamada (saatlik, günlük vb.) çalıştırabilirsiniz. Bu seçenekler, şirket içi, bulut ve yerel masaüstü dahil olmak üzere farklı ortamlardaki bağlayıcılar için kullanılabilir. 

Tek seferlik kopyalama işlemi, veri hareketinin bir kaynaktan hedefe yalnızca bir kez taşınmasını sağlar. Herhangi bir boyuttave desteklenen biçimdeki veriler için geçerlidir. Zamanlanan kopya, belirttiğiniz bir yinelemedeki verileri kopyalamanızı sağlar. Zamanlanan kopyayı yapılandırmak için zengin ayarları (yeniden deneme, zaman sonu ve uyarılar gibi) kullanabilirsiniz.

![Zamanlama seçenekleri](./media/copy-data-tool/scheduling-options.png)


## <a name="next-steps"></a>Sonraki adımlar
Veri Kopyalama aracını kullanan şu öğreticileri deneyin:

- [Quickstart: Veri Kopyalama aracını kullanarak bir veri fabrikası oluşturma](quickstart-create-data-factory-copy-data-tool.md)
- [Öğretici: Verileri Azure'da Kopyala veri aracını kullanarak kopyalama](tutorial-copy-data-tool.md) 
- [Öğretici: Verileri Kopyala aracını kullanarak şirket içi verileri Azure'a kopyalama](tutorial-hybrid-copy-data-tool.md)
