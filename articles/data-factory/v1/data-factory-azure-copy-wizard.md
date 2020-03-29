---
title: Veri Fabrikası Azure Kopyalama Sihirbazı
description: Desteklenen veri kaynaklarından lavabolara verileri kopyalamak için Veri Fabrikası Azure Kopyalama Sihirbazı'nı nasıl kullanacağınız hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: 0974eb40-db98-4149-a50d-48db46817076
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: fef9059700e2bd94029c40bb819870a7174e0812
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930126"
---
# <a name="azure-data-factory-copy-wizard"></a>Azure Veri Fabrikası Kopyalama Sihirbazı
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. 

Azure Veri Fabrikası Kopyalama Sihirbazı, genellikle uçtan uca veri tümleştirme senaryosunun ilk adımı olan verileri sindirme işlemini kolaylaştırır. Azure Veri Fabrikası Kopyalama Sihirbazı'ndan geçerken, bağlantılı hizmetler, veri kümeleri ve ardışık hatlar için json tanımlarını anlamanız gerekmez. Sihirbaz, seçili veri kaynağından seçili hedefe verileri kopyalamak için otomatik olarak bir ardışık kaynak hattı oluşturur. Ayrıca, Kopyalama Sihirbazı, yazarlık sırasında yutulan verileri doğrulamanıza yardımcı olur. Bu, özellikle veri kaynağından ilk kez veri sindiriyorsanız zaman kazandırır. Kopyalama Sihirbazı'nı başlatmak için, veri fabrikanızın ana sayfasındaki **Veri** döşemesini kopyala'yı tıklatın.

![Kopyalama Sihirbazı](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="designed-for-big-data"></a>Büyük veriler için tasarlanmıştır
Bu sihirbaz, çok çeşitli kaynaklardan gelen verileri dakikalar içinde kolayca hedefe taşımanızı sağlar. Sihirbazdan geçtikten sonra, bağımlı Veri Fabrikası varlıkları (bağlantılı hizmetler ve veri kümeleri) ile birlikte sizin için otomatik olarak kopyalama etkinliği içeren bir ardışık ardışık alan oluşturulur. Ardışık hatlar oluşturmak için ek adım gerekmez.   

![Veri kaynağı seçme](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Azure blob'undaki verileri Azure SQL Veritabanı tablosuna kopyalamak için örnek bir ardışık kaynak oluşturmak için adım adım yönergeleri için [Kopya Sihirbazı öğreticisine](data-factory-copy-data-wizard-tutorial.md)bakın.
>
>

Sihirbaz, farklı veri ve nesne türleri için destek ile, başından beri göz önünde bulundurularak büyük veri ile tasarlanmıştır. Yüzlerce klasör, dosya veya tablo hareket ettiren Veri Fabrikası ardışık hatlar yazabilirsiniz. Sihirbaz otomatik veri önizlemesini, şema yakalamayı ve eşlemesini ve veri filtrelemi destekler.

## <a name="automatic-data-preview"></a>Otomatik veri önizleme
Verilerin kopyalamak istediğiniz şey olup olmadığını doğrulamak için verilerin bir kısmını seçili veri kaynağından önizleyebilirsiniz. Ayrıca, kaynak veriler bir metin dosyasındaysa, Kopya Sihirbazı satır ve sütun sınırlayıcıları ve şemayı otomatik olarak öğrenmek için metin dosyasını ayrıştırır.

![Dosya biçimi ayarları](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Şema yakalama ve haritalama
Giriş verilerinin şeması, bazı durumlarda çıktı verilerinin şema ile eşleşmeyebilir. Bu senaryoda, sütunları kaynak şemadaki sütunları hedef şemadaki sütunlara eşlemeniz gerekir.

> [!TIP]
> SQL Server veya Azure SQL Veritabanı'ndan Verileri Azure SQL Veri Ambarı'na kopyalarken, tablo hedef deposunda yoksa, Veri Fabrikası kaynak şemasını kullanarak otomatik tablo oluşturmayı destekler. [Azure Veri Fabrikası'nı kullanarak Verileri Azure SQL Veri Ambarı'na taşıyıp diğer inden](./data-factory-azure-sql-data-warehouse-connector.md)daha fazla bilgi edinin.
>

Hedef şemadaki bir sütuna eşlemek için kaynak şemadan bir sütun seçmek için açılır liste kullanın. Kopya Sihirbazı, sütun eşleme için deseninizi anlamaya çalışır. Şema eşleciliğini tamamlamak için sütunların her birini tek tek seçmeniz gerekmeden, sütunların geri kalanına aynı deseni uygular. İsterseniz, sütunları tek tek eşlemek için açılır listeleri kullanarak bu eşlemeleri geçersiz kılabilirsiniz. Daha fazla sütunharitasını açtıkça desen daha doğru hale gelir. Kopya Sihirbazı deseni sürekli olarak güncelleştirir ve sonuçta ulaşmak istediğiniz sütun eşlemi için doğru desene ulaşır.     

![Şema haritalama](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Verileri filtreleme
Yalnızca lavabo veri deposuna kopyalanması gereken verileri seçmek için kaynak verilerine filtre uygulayabilirsiniz. Filtreleme, lavabo veri deposuna kopyalanacak verilerin hacmini azaltır ve bu nedenle kopyalama işleminin veri hacmini artırır. SQL sorgu dilini kullanarak ilişkisel bir veritabanındaki verileri filtrelemek için esnek bir yol sağlar veya [Veri Fabrikası işlevlerini ve değişkenlerini](data-factory-functions-variables.md)kullanarak Azure blob klasöründeki dosyaları.   

### <a name="filtering-of-data-in-a-database"></a>Veritabanındaki verilerin filtrelemi
Aşağıdaki ekran görüntüsü, işlev `Text.Format` ve `WindowStart` değişkeni kullanarak bir SQL sorgusu nu gösterir.

![İfadeleri doğrulama](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Azure blob klasöründeki verilerin filtrelemi
[Sistem değişkenlerine](data-factory-functions-variables.md#data-factory-system-variables)dayalı olarak çalışma zamanında belirlenen bir klasörden verileri kopyalamak için klasör yolundaki değişkenleri kullanabilirsiniz. Desteklenen değişkenler şunlardır: **{year}**, **{ay}**, **{gün}**, **{hour}**, **{dakika}** ve **{özel}**. Örneğin: inputfolder/{year}/{month}/{day}.

Aşağıdaki biçimde giriş klasörleri olduğunu varsayalım:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

**Dosya veya klasör**için **Gözat** düğmesini tıklatın, bu klasörlerden birine göz atın (örneğin, 2016->03->01->02) ve **Seç'i**tıklatın. Metin kutusunda `2016/03/01/02` görmelisin. Şimdi, **2016'yı** **{year}**, **03** ile **{month}**, **01'i** **{day}** ve **02'yi** **{hour}** ile değiştirin ve **Sekme** tuşuna basın. Bu dört değişkenin biçimini seçmek için açılır listeler görmelisiniz:

![Sistem değişkenlerini kullanma](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Aşağıdaki ekran görüntüsünde gösterildiği gibi, **özel** bir değişken ve [desteklenen biçim dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx)de kullanabilirsiniz. Bu yapıya sahip bir klasör seçmek için önce **Gözat** düğmesini kullanın. Ardından bir değeri **{custom}** ile değiştirin ve biçim dizesini yazabileceğiniz metin kutusunu görmek için **Sekme** tuşuna basın.     

![Özel değişken kullanma](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="scheduling-options"></a>Zamanlama seçenekleri
Kopyalama işlemini bir kez veya bir zamanlamada (saatlik, günlük vb.) çalıştırabilirsiniz. Bu seçeneklerin her ikisi de, şirket içi, bulut ve yerel masaüstü kopyası da dahil olmak üzere ortamlar arasında konektörlerin genişliği için kullanılabilir.

Tek seferlik kopyalama işlemi, veri hareketinin bir kaynaktan hedefe yalnızca bir kez taşınmasını sağlar. Herhangi bir boyuttave desteklenen biçimdeki veriler için geçerlidir. Zamanlanan kopya, verileri öngörülen yinelemede kopyalamanıza olanak tanır. Zamanlanan kopyayı yapılandırmak için zengin ayarları (yeniden deneme, zaman sonu ve uyarılar gibi) kullanabilirsiniz.

![Zamanlama özellikleri](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopya Etkinliği ile bir ardışık hatlar oluşturmak için Veri Fabrikası Kopyalama Sihirbazı'nı kullanmanın hızlı bir şekilde gözden geçmesi için [Bkz. Öğretici: Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)
