---
title: U-SQL veritabanını dışa aktarma- Visual Studio için Azure Veri Gölü Araçları
description: Bir U-SQL veritabanını dışa aktarmak ve yerel bir hesaba otomatik olarak aktarmak için Visual Studio için Azure Veri Göl Araçlarını nasıl kullanacağınızı öğrenin.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: conceptual
ms.date: 11/27/2017
ms.openlocfilehash: e5a52de0342e864cb108d8d590583fe64f72e3b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71315790"
---
# <a name="export-a-u-sql-database"></a>U-SQL veritabanı nı dışa aktarma

Bu makalede, Bir U-SQL veritabanını tek bir U-SQL komut dosyası ve indirilen kaynaklar olarak dışa aktarmak [için Visual Studio için Azure Veri Göl Araçları'nı](https://aka.ms/adltoolsvs) nasıl kullanacağınızı öğrenin. Dışa aktarılan veritabanını aynı işlemde yerel bir hesaba aktarabilirsiniz.

Müşteriler genellikle geliştirme, test ve üretim için birden çok ortam tutar. Bu ortamlar hem yerel bir hesapta, hem geliştiricinin yerel bilgisayarında hem de Azure'daki Bir Azure Veri Gölü Analizi hesabında barındırılır. 

Geliştirme ve test ortamlarında U-SQL sorguları geliştirip ayarladığınızda, geliştiricilerin genellikle çalışmalarını bir üretim veritabanında yeniden oluşturmaları gerekir. Veritabanı Dışa Aktarma Sihirbazı bu işlemi hızlandırmaya yardımcı olur. Sihirbazı kullanarak, geliştiriciler varolan veritabanı ortamını klonlayabilir ve verileri diğer Data Lake Analytics hesaplarına örnekleyebilir.

## <a name="export-steps"></a>Dışa aktarma adımları

### <a name="step-1-export-the-database-in-server-explorer"></a>Adım 1: Veritabanını Server Explorer'da dışa aktarma

İzinleri olan tüm Data Lake Analytics hesapları Server Explorer'da listelenir. Veritabanını dışa aktarmak için:

1. Sunucu Gezgini'nde, dışa aktarmak istediğiniz veritabanını içeren hesabı genişletin.
2. Veritabanına sağ tıklayın ve sonra **Dışa Aktar'ı**seçin. 
   
    ![Server Explorer - Veritabanını dışa aktarma](./media/data-lake-analytics-data-lake-tools-export-database/export-database.png)

     **Dışa Aktarma** menüsü yoksa, [aracı son sürümde güncelleştirmeniz](https://aka.ms/adltoolsvs)gerekir.

### <a name="step-2-configure-the-objects-that-you-want-to-export"></a>Adım 2: Dışa aktarmak istediğiniz nesneleri yapılandırma

Büyük bir veritabanının yalnızca küçük bir bölümüne ihtiyacınız varsa, dışa aktarma sihirbazında dışa aktarmak istediğiniz nesnelerin bir alt kümesini yapılandırabilirsiniz. 

Dışa aktarma eylemi bir U-SQL işi çalıştırılarak tamamlanır. Bu nedenle, bir Azure hesabından dışa aktarma bazı maliyetlere neden olabilir.

![Veritabanı Dışa Aktarma Sihirbazı - Dışa aktarma nesnelerini seçin](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard.png)

### <a name="step-3-check-the-objects-list-and-other-configurations"></a>Adım 3: Nesne listesini ve diğer yapılandırmaları denetleyin

Bu adımda, Nesne **liste** kutusunda seçili nesneleri doğrulayabilirsiniz. Herhangi bir hata varsa, geri dönmek ve dışa aktarmak istediğiniz nesneleri doğru şekilde yapılandırmak için **Önceki'yi** seçin.

Dışa aktarma hedefi için diğer ayarları da yapılandırabilirsiniz. Yapılandırma açıklamaları aşağıdaki tabloda listelenir:

|Yapılandırma|Açıklama|
|-------------|-----------|
|Hedef Adı|Bu ad, dışa aktarılan veritabanı kaynaklarını kaydetmek istediğiniz yeri gösterir. Örnekler derlemeler, ek dosyalar ve örnek verilerdir. Yerel veri kökü klasörünüzün altında bu ada sahip bir klasör oluşturulur.|
|Proje Rehberi|Bu yol, dışa aktarılan U-SQL komut dosyasını kaydetmek istediğiniz yeri tanımlar. Tüm veritabanı nesne tanımları bu konumda kaydedilir.|
|Sadece Şema|Bu seçeneği belirlerseniz, yalnızca veritabanı tanımları ve kaynakları (derlemeler ve ek dosyalar gibi) dışa aktarılır.|
|Şema ve Veri|Bu seçeneği seçerseniz, veritabanı tanımları, kaynaklar ve veriler dışa aktarılır. Tabloların üst N satırları dışa aktarılır.|
|Yerel Veritabanına Otomatik Olarak Alma|Bu seçeneği belirlerseniz, dışa aktarılan veritabanı, dışa aktarma tamamlandığında otomatik olarak yerel veritabanınıza aktarılır.|

![Veritabanı Dışa Aktarma Sihirbazı - Dışa aktarma nesneleri listesi ve diğer yapılandırmaları](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-configuration.png)

### <a name="step-4-check-the-export-results"></a>Adım 4: Dışa aktarma sonuçlarını kontrol edin

Dışa aktarma tamamlandığında, dışa aktarılan sonuçları sihirbazdaki günlük penceresinde görüntüleyebilirsiniz. Aşağıdaki örnek, derlemeler, ek dosyalar ve örnek veriler de dahil olmak üzere dışa aktarılan U-SQL komut dosyası ve veritabanı kaynaklarını nasıl bulabileceğinizi gösterir:

![Veritabanı Dışa Aktarma Sihirbazı - Dışa aktarma sonuçları](./media/data-lake-analytics-data-lake-tools-export-database/export-database-wizard-completed.png)

## <a name="import-the-exported-database-to-a-local-account"></a>Dışa aktarılan veritabanını yerel bir hesaba aktarma

Dışa aktarılan veritabanını almanın en uygun yolu, Adım 3'teki dışa aktarma işlemi sırasında **Yerel Veritabanına Otomatik Olarak Alma** onay kutusunu seçmektir. Bu kutuyu işaretlemediyseniz, önce dışa aktarılan U-SQL komut dosyasını dışa aktarma günlüğünde bulun. Ardından, veritabanını yerel hesabınıza almak için U-SQL komut dosyasını yerel olarak çalıştırın.

## <a name="import-the-exported-database-to-a-data-lake-analytics-account"></a>Dışa aktarılan veritabanını veri gölü analizi hesabına aktarma

Veritabanını farklı Data Lake Analytics hesabına almak için:

1. Derlemeler, ek dosyalar ve örnek veriler de dahil olmak üzere dışa aktarılan kaynakları, içe almak istediğiniz Veri Gölü Analytics hesabının varsayılan Azure Veri Gölü Deposu hesabına yükleyin. Dışa aktarılan kaynak klasörünü yerel veri kökü klasörü altında bulabilirsiniz. Klasörün tamamını varsayılan Veri Gölü Deposu hesabının köküne yükleyin.
2. Yükleme tamamlandığında, dışa aktarılan U-SQL komut dosyasını veritabanını almak istediğiniz Data Lake Analytics hesabına gönderin.

## <a name="known-limitations"></a>Bilinen sınırlamalar

Şu anda, Adım 3'te **Şema ve Veri** seçeneğini seçerseniz, araç tablolarda depolanan verileri dışa aktarmak için bir U-SQL işi çalıştırıyor. Bu nedenle, veri verme işlemi yavaş olabilir ve maliyete maruz kalabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

* [U-SQL veritabanları hakkında bilgi edinin](/u-sql/data-definition-language-ddl-statements) 
* [Yerel çalıştırma ve Azure Data Lake U-SQL SDK’sını kullanarak U-SQL işlerini test etme ve hatalarını ayıklama](data-lake-analytics-data-lake-tools-local-run.md)


