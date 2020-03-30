---
title: Power BI Microsoft Azure ile verileri görselleştirin
description: Power BI ile SQL Data Warehouse verilerini görselleştirme
services: synapse-analytics
author: mlee3gsd
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 7e764eeab6f681d90e1a602f02cdb03330d6fd3d
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80350940"
---
# <a name="visualize-data-with-power-bi"></a>Power BI ile verileri görselleştirme
Bu öğreticide SQL Data Warehouse'a bağlanmak ve birkaç temel görselleştirme oluşturmak üzere Power BI'ı nasıl kullanacağınız gösterilmiştir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Data-Warehouse-Sample-Data-and-PowerBI/player]
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticide ilerleyebilmeniz için şunlar gereklidir:

* AdventureWorksDW veritabanı önceden yüklenmiş bir SQL Data Warehouse. Veri ambarı sağlamak için [bir SQL Veri Ambarı Oluştur'a](create-data-warehouse-portal.md) bakın ve örnek verileri yüklemeyi seçin. Zaten bir veri ambarı var ama örnek veri yoksa, [WideWorldImportersDW yükleyebilirsiniz.](load-data-wideworldimportersdw.md)

## <a name="1-connect-to-your-database"></a>1. Veritabanınıza bağlanın
Power BI'ı açmak ve AdventureWorksDW veritabanınıza bağlanmak için şunları yapın:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **SQL veritabanları** seçeneğine tıklayın ve AdventureWorks SQL Data Warehouse veritabanınızı seçin.
   
    ![Veritabanınızı bulma](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png)
3. "Power BI'da aç" düğmesine tıklayın.
   
    ![Power BI düğmesi](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png)
4. Bu işlemin ardından veritabanınızın web adresinin görüntülendiği SQL Data Warehouse bağlantı sayfası açılır. İleri'ye tıklayın.
   
    ![Power BI bağlantısı](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png)
5. Azure SQL sunucu kullanıcı adınızı ve parolanızı girin.
   
    ![Güç BI oturum açma](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png)
6. Veritabanını açmak için sol bıçaktaki AdventureWorksDW veri kümesini tıklatın.
   
    ![Power BI AdventureWorksDW'yi açma](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png)

## <a name="2-create-a-report"></a>2. Rapor oluşturma
Artık AdventureWorksDW örnek verilerinizi Power BI kullanarak çözümlemeye hazırsınız. AdventureWorksDW'de çözümlemenin gerçekleştirileceği AggregateSales adlı bir görünüm vardır. Bu görünüm, şirket satışlarının çözümlenmesine yönelik ana ölçümlerden birkaçını içerir.

1. Posta koduna göre satış tutarlarının bir haritasını oluşturmak için sağ taraftaki alanlar bölmesinde AggregateSales görünümüne tıklayıp görünümü genişletin. PostalCode ve SalesAmount sütünlarını tıklayarak seçin.
   
    ![Power BI AggregateSales'i seçer](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png)
   
    Power BI coğrafi verileri otomatik olarak tanıdı ve sizin için bir haritaya koydu.
   
    ![Power BI haritası](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png)

2. Bu adımda müşteri geliri başına satış tutarını gösteren bir çubuk grafiği oluşturulur. Çubuk grafiği oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount alanına tıklayın. Müşteri Geliri alanını sola sürükleyip Eksene bırakın.
   
    ![Power BI ekseni seçer](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png)
   
    Soldaki çubuk grafik.
   
    ![Power BI çubuğu](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png)
3. Bu adımda, sipariş tarihi başına satış tutarını gösteren bir çizgi grafiği oluşturulur. Satır grafiğini oluşturmak için genişletilmiş AggregateSales görünümüne gidin. SalesAmount ve OrderDate öğelerine tıklayın Görseller sütununda, görselleştirmeler altındaki ikinci satırdaki ilk simge olan Çizgi Grafiği simgesini tıklatın.
   
    ![Power BI çizgi grafiğini seçer](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png)
   
    Artık verilerin üç farklı görselleştirmesini gösteren bir raporunuz var.
   
    ![Power BI çizgi grafiği](./media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png)

**Dosya**'ya tıklayıp **Kaydet**'i seçerek ilerleme durumunuzu istediğiniz zaman kaydedebilirsiniz.

## <a name="using-direct-connect"></a>Doğrudan Bağlanma'yı kullanma
SQL Data Warehouse Direct Connect, Azure SQL Veritabanında olduğu gibi, Power BI'nin analitik yeteneklerinin yanı sıra mantıksal pushdown'a da izin verir. Direct Connect ile, verileri keşfederken sorgular Azure SQL Veri Ambarınıza gerçek zamanlı olarak geri gönderilir.  Bu özellik, SQL Veri Ambarı ölçeğiyle birleştiğinde, terabaytlarzaman ait verilere karşı dakikalar içinde dinamik raporlar oluşturmanıza olanak tanır. Buna ek olarak, Power BI'de Aç düğmesinin piyasaya sürülmesi, kullanıcıların Azure'un diğer bölümlerinden bilgi toplamadan Power BI'yi DOĞRUDAN SQL Veri Ambarı'na bağlamalarına olanak tanır.

Direct Connect kullanırken:

* Bağlanırken tam nitelikli sunucu adını belirtin.
* Veritabanı için güvenlik duvarı kurallarının Azure hizmetlerine erişime izin verecek şekilde yapılandırıldığından emin olun.
* Sütun seçmek veya filtre eklemek gibi her eylem veri ambarını doğrudan sorgular.
* Fayans otomatik olarak ve yaklaşık her 15 dakikada bir yenilenir.
* Q&A, Direct Connect veri kümeleri için kullanılamaz.
* Şema değişiklikleri otomatik olarak dahil edilir.
* Tüm Direct Connect sorguları 2 dakika sonra zaman ları dolacak.

Deneyimler geliştikçe bu kısıtlamalar ve notlar değişebilir.

## <a name="next-steps"></a>Sonraki adımlar
Şimdi size örnek verilerle ısınmanız için biraz zaman verdiğimize göre, nasıl [geliştirileceğiniz](sql-data-warehouse-overview-develop.md) veya [yükleyin.](design-elt-data-loading.md) Veya [Power BI web sitesi](https://www.powerbi.com/)'ni ziyaret edin.
