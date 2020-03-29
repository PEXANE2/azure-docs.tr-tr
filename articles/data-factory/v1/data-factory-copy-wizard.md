---
title: Kopyalama Sihirbazı ile verileri kolayca kopyalama - Azure
description: Desteklenen veri kaynaklarından lavabolara verileri kopyalamak için Veri Fabrikası Kopyalama Sihirbazı'nı nasıl kullanacağı hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: f904972f-cd33-48db-9755-2b3196ae4168
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 8b872cc30ae66e83274f189138dad6d609e3f536
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74927044"
---
# <a name="copy-or-move-data-easily-with-azure-data-factory-copy-wizard"></a>Azure Veri Fabrikası Kopyalama Sihirbazı ile verileri kolayca kopyalama veya taşıma
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz. [kopyalama etkinliği öğreticisi](../quickstart-create-data-factory-dot-net.md). 


Azure Veri Fabrikası Kopyalama Sihirbazı, genellikle uçtan uca veri tümleştirme senaryosunun ilk adımı olan veri alma işlemini kolaylaştırmak içindir. Azure Veri Fabrikası Kopyalama Sihirbazı'ndan geçerken, bağlantılı hizmetler, veri kümeleri ve ardışık hatlar için json tanımlarını anlamanız gerekmez. Ancak, sihirbazdaki tüm adımları tamamladıktan sonra sihirbaz, seçili veri kaynağından seçili hedefe verileri kopyalamak için otomatik olarak bir ardışık ardışık kaynak oluşturur. Ayrıca, Kopyalama Sihirbazı, yazarlık sırasında yutulan verileri doğrulamanıza yardımcı olur ve bu da özellikle veri kaynağından ilk kez veri sindirirken zamanınızın çoğunu kazandırır. Kopyalama Sihirbazı'nı başlatmak için, veri fabrikanızın ana sayfasındaki **Veri** döşemesini kopyala'yı tıklatın.

![Kopyalama Sihirbazı](./media/data-factory-copy-wizard/copy-data-wizard.png)

## <a name="an-intuitive-wizard-for-copying-data"></a>Verileri kopyalamak için sezgisel bir sihirbaz
Bu sihirbaz, çok çeşitli kaynaklardan gelen verileri dakikalar içinde kolayca hedefe taşımanızı sağlar. Sihirbazdan geçtikten sonra, bağımlı Veri Fabrikası varlıkları (bağlantılı hizmetler ve veri kümeleri) ile birlikte sizin için otomatik olarak kopyalama etkinliği içeren bir ardışık ardışık alan oluşturulur. Ardışık hatlar oluşturmak için ek adım gerekmez.   

![Veri kaynağı seçme](./media/data-factory-copy-wizard/select-data-source-page.png)

> [!NOTE]
> Azure tablosundan Azure SQL Veritabanı tablosuna veri kopyalamak için örnek bir ardışık kaynak oluşturmak için adım adım yönergeleri için [Kopyalama Sihirbazı öğretici](data-factory-copy-data-wizard-tutorial.md) makalesine bakın. 
> 
> 

Sihirbaz başından itibaren büyük veriler göz önünde bulundurularak tasarlanmıştır. Veri Kopyalama sihirbazını kullanarak yüzlerce klasör, dosya veya tablo yutacak veri fabrikası ardışık hatlarını yazar basit ve verimlidir. Sihirbaz aşağıdaki üç özelliği destekler: Otomatik veri önizleme, şema yakalama ve eşleme ve verileri filtreleme. 

## <a name="automatic-data-preview"></a>Otomatik veri önizleme
Kopyalama sihirbazı, verilerin kopyalamak istediğiniz doğru veri olup olmadığını doğrulamanız için seçili veri kaynağından verilerin bir kısmını gözden geçirmenize olanak tanır. Ayrıca, kaynak veriler bir metin dosyasındaysa, kopyalama sihirbazı metin dosyasını satır ve sütun sınırlayıcıları ve şemayı otomatik olarak öğrenmek için ayrıştırır. 

![Dosya biçimi ayarları](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Şema yakalama ve haritalama
Giriş verilerinin şeması, bazı durumlarda çıktı verilerinin şema ile eşleşmeyebilir. Bu senaryoda, sütunları kaynak şemadaki sütunları hedef şemadaki sütunlara eşlemeniz gerekir. 

Kopyalama sihirbazı, kaynak şemadaki sütunları hedef şemadaki sütunlarla otomatik olarak eşler. Açılan listeleri (veya) verileri kopyalarken bir sütunun atlanması gerekip gerekmediğini belirterek eşlemeleri geçersiz kılabilirsiniz.   

![Şema haritalama](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Verileri filtreleme
Sihirbaz, yalnızca hedef/lavabo veri deposuna kopyalanması gereken verileri seçmek için kaynak verilere filtre uygulamanızı sağlar. Filtreleme, lavabo veri deposuna kopyalanacak verilerin hacmini azaltır ve bu nedenle kopyalama işleminin veri hacmini artırır. [Veri Fabrikası işlevlerini ve değişkenlerini](data-factory-functions-variables.md)kullanarak Azure blob klasöründeki SQL sorgu dili (veya) dosyalarını kullanarak ilişkisel bir veritabanındaki verileri filtrelemek için esnek bir yol sağlar.   

### <a name="filtering-of-data-in-a-database"></a>Veritabanındaki verilerin filtrelemi
Örnekte, SQL sorgusu işlev `Text.Format` ve `WindowStart` değişken kullanır. 

![İfadeleri doğrulama](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Azure blob klasöründeki verilerin filtrelemi
[Sistem değişkenlerine](data-factory-functions-variables.md#data-factory-system-variables)dayalı olarak çalışma zamanında belirlenen bir klasörden verileri kopyalamak için klasör yolundaki değişkenleri kullanabilirsiniz. Desteklenen değişkenler şunlardır: **{year}**, **{ay}**, **{gün}**, **{hour}**, **{dakika}** ve **{özel}**. Örnek: inputfolder/{year}/{month}/{day}.

Aşağıdaki biçimde giriş klasörleri olduğunu varsayalım:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

**Dosya veya klasör**için **Gözat** düğmesini tıklatın, bu klasörlerden birine göz atın (örneğin, 2016->03->01->02) ve **Seç'i**tıklatın. Metin kutusunda `2016/03/01/02` görmelisin. Şimdi, **2016'yı** **{year}**, **03** ile **{month}**, **01'i** **{day}** ile ve **02'yi** **{hour}** ile değiştirin ve Sekme'ye basın. Bu dört değişkenin biçimini seçmek için açılır listeler görmelisiniz:

![Sistem değişkenlerini kullanma](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Aşağıdaki ekran görüntüsünde gösterildiği gibi, **özel** bir değişken ve [desteklenen biçim dizeleri](https://msdn.microsoft.com/library/8kb3ddd4.aspx)de kullanabilirsiniz. Bu yapıya sahip bir klasör seçmek için önce **Gözat** düğmesini kullanın. Ardından bir değeri **{custom}** ile değiştirin ve biçim dizesini yazabileceğiniz metin kutusunu görmek için Sekme'ye basın.     

![Özel değişken kullanma](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)

## <a name="support-for-diverse-data-and-object-types"></a>Farklı veri ve nesne türleri için destek
Kopya sihirbazını kullanarak yüzlerce klasörü, dosyayı veya tabloyu verimli bir şekilde taşıyabilirsiniz.

![Verileri kopyalamak için tabloları seçin](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Zamanlama seçenekleri
Kopyalama işlemini bir kez veya bir zamanlamada (saatlik, günlük vb.) çalıştırabilirsiniz. Bu seçeneklerin her ikisi de şirket içi, bulut ve yerel masaüstü kopyadaki konektörlerin genişliği için kullanılabilir.

Tek seferlik kopyalama işlemi, veri hareketinin bir kaynaktan hedefe yalnızca bir kez taşınmasını sağlar. Herhangi bir boyuttave desteklenen biçimdeki veriler için geçerlidir. Zamanlanan kopya, verileri öngörülen yinelemede kopyalamanıza olanak tanır. Zamanlanan kopyayı yapılandırmak için zengin ayarları (yeniden deneme, zaman sonu ve uyarılar gibi) kullanabilirsiniz.

![Zamanlama özellikleri](./media/data-factory-copy-wizard/scheduling-properties.png)

## <a name="next-steps"></a>Sonraki adımlar
Kopya Etkinliği ile bir ardışık hatlar oluşturmak için Veri Fabrikası Kopyalama Sihirbazı'nı kullanmanın hızlı bir şekilde gözden geçmesi için [Bkz. Öğretici: Kopya Sihirbazı'nı kullanarak bir ardışık kaynak oluşturun.](data-factory-copy-data-wizard-tutorial.md)

