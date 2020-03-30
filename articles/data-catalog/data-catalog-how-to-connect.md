---
title: Azure Veri Kataloğu'ndaki veri kaynaklarına bağlanma
description: Azure Veri Kataloğu ile keşfedilen veri kaynaklarına nasıl bağlanılabildiğini vurgulayan nasıl dır makalesi.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: 1190a0f34206004b72730a6af85bbe5db7d9961a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68976779"
---
# <a name="how-to-connect-to-data-sources"></a>Veri kaynaklarına bağlanma
## <a name="introduction"></a>Giriş
**Microsoft Azure Veri Kataloğu,** kurumsal veri kaynakları için bir kayıt ve keşif sistemi olarak hizmet veren tam olarak yönetilen bir bulut hizmetidir. Başka bir deyişle, **Azure Veri Kataloğu** tamamen insanların veri kaynaklarını keşfetmesine, anlamasına ve kullanmasına ve kuruluşların mevcut verilerinden daha fazla değer elde etmesine yardımcı olmak için önemlidir. Bu senaryonun önemli bir yönü verileri kullanmaktır – bir kullanıcı bir veri kaynağını bulduğunda ve amacını anladıktan sonra, bir sonraki adım verilerini kullanmak için veri kaynağına bağlanmaktır.

## <a name="data-source-locations"></a>Veri kaynağı konumları
Veri kaynağı kaydı sırasında **Azure Veri Kataloğu,** veri kaynağı yla ilgili meta verileri alır. Bu meta veriler, veri kaynağının konumunun ayrıntılarını içerir. Konumun ayrıntıları veri kaynağından veri kaynağına değişir, ancak bağlanmak için gereken bilgileri her zaman içerir. Örneğin, bir SQL Server tablosunun konumu sunucu adını, veritabanı adını, şema adını ve tablo adını içerirken, SQL Server Reporting Services raporunun konumu sunucu adını ve rapora giden yolu içerir. Diğer veri kaynağı türleri, kaynak sistemin yapısını ve yeteneklerini yansıtan konumlara sahip olacaktır.

## <a name="integrated-client-tools"></a>Entegre istemci araçları
Bir veri kaynağına bağlanmanın en basit yolu "Aç" **menüyü Azure Veri Kataloğu** portalında bulabilirsiniz. Bu menü, seçili veri varlığına bağlanmak için seçeneklerin listesini görüntüler.
Varsayılan döşeme görünümünü kullanırken, bu menü her döşemede kullanılabilir.

 ![Veri kıymet döşemesinden Excel'de SQL Server tablosu açma](./media/data-catalog-how-to-connect/data-catalog-how-to-connect1.png)

Liste görünümünü kullanırken, menü portal penceresinin üst kısmındaki arama çubuğunda kullanılabilir.

 ![Rapor Yöneticisi'nde SQL Server Reporting Services raporu açma](./media/data-catalog-how-to-connect/data-catalog-how-to-connect2.png)

## <a name="supported-client-applications"></a>Desteklenen İstemci Uygulamaları
"Aç..." Azure Veri Kataloğu portalındaki veri kaynakları menüsünde, istemci bilgisayara doğru istemci uygulamasının yüklenmesi gerekir.

| Uygulamada açık | Dosya uzantısı / protokol | Desteklenen uygulama sürümleri |
| --- | --- | --- |
| Excel |Odc |Excel 2010 veya sonrası |
| Excel (Top 1000) |Odc |Excel 2010 veya sonrası |
| Power Query |.xlsx |Excel eklentisi için Güç Sorgusu yüklü Excel 2016 veya Excel 2010 veya Excel 2013 |
| Power BI Desktop |.pbix |Power BI Masaüstü Temmuz 2016 veya sonrası |
| SQL Server Veri Araçları |vsweb:// |Visual Studio 2013 Güncelleme 4 veya daha sonra SQL Server araç yüklü |
| Rapor Yöneticisi |http:// |[SQL Server Reporting Services için tarayıcı gereksinimlerine](https://technet.microsoft.com/library/ms156511.aspx) bakın |

## <a name="your-data-your-tools"></a>Verileriniz, araçlarınız
Menüde bulunan seçenekler, şu anda seçili veri kıymetinin türüne bağlıdır. Tabii ki, tüm olası araçlar "Açık içinde dahil edilecektir ..." menü, ancak yine de herhangi bir istemci aracı nı kullanarak veri kaynağına bağlanmak kolaydır. **Azure Veri Kataloğu** portalında bir veri varlığı seçildiğinde, tüm konumu özellikler bölmesinde görüntülenir.

 ![SQL Server tablosunun bağlantı bilgileri](./media/data-catalog-how-to-connect/data-catalog-how-to-connect3.png)

Bağlantı bilgileri ayrıntıları veri kaynağı türünden veri kaynağı türüne farklılık gösterir, ancak portalda yer alan bilgiler herhangi bir istemci aracında veri kaynağına bağlanmak için ihtiyacınız olan her şeyi verir. Kullanıcılar, **Azure Veri Kataloğu'nu**kullanarak buldukları veri kaynaklarının bağlantı ayrıntılarını kopyalayarak seçtikleri araçtaki verilerle çalışmalarını sağlayabilir.

## <a name="connecting-and-data-source-permissions"></a>Bağlanma ve veri kaynağı izinleri
**Azure Veri Kataloğu** veri kaynaklarını bulunabilir hale getirir, ancak verilere erişim veri kaynağı sahibinin veya yöneticisinin denetimi altında kalır. **Azure Veri Kataloğu'nda** bir veri kaynağının bulunması, kullanıcıya veri kaynağına erişme izni vermez.

Bir veri kaynağı bulan ancak verilerine erişim izni olmayan kullanıcıların verilerini daha kolay hale getirmek için, kullanıcılar bir veri kaynağına açıklama yaparken İstek Erişimi özelliğinde bilgi sağlayabilir. Burada sağlanan bilgiler -veri kaynağı erişimi elde etmek için işlem veya iletişim noktası bağlantıları da dahil olmak üzere- portaldaki veri kaynağı konum bilgilerinin yanında sunulur.

 ![Sağlanan istek erişim talimatlarıile bağlantı bilgileri](./media/data-catalog-how-to-connect/data-catalog-how-to-connect4.png)

## <a name="summary"></a>Özet
Bir veri kaynağını **Azure Veri Kataloğu'na** kaydetmek, veri kaynağından katalog hizmetine yapısal ve açıklayıcı meta verileri kopyalayarak bu verileri bulunabilir hale getirir. Bir veri kaynağı kaydedilip keşfedildikten sonra, kullanıcılar **Azure Veri Kataloğu** portalı "Açık..."" veri kaynağına bağlanabilir. menü veya tercih ettikleri veri araçlarını kullanarak.

## <a name="see-also"></a>Ayrıca bkz.
* Veri kaynaklarına nasıl bağlanıla ilgili adım adım ayrıntılar için Azure Veri Kataloğu öğreticisine [başlayın.](data-catalog-get-started.md)
