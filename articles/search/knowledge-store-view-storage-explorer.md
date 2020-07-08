---
title: Depolama Gezgini bir bilgi deposu görüntüleme
titleSuffix: Azure Cognitive Search
description: Azure portal Depolama Gezgini bir Azure Bilişsel Arama bilgi deposu görüntüleyin ve çözümleyin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: ba0b02067c032f9038051c169866588ded44af73
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85566029"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Depolama Gezgini bir bilgi deposu görüntüleme

Bu makalede, Azure portal Depolama Gezgini kullanarak bir bilgi deposuna nasıl bağlanacağınızı ve keşfedeceğinizi öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

+ Bu kılavuzda kullanılan örnek bilgi deposunu oluşturmak için [Azure Portal bilgi deposu oluşturma](knowledge-store-create-portal.md) bölümündeki adımları izleyin.

+ Ayrıca, bilgi deposunu oluşturmak için kullandığınız Azure depolama hesabının adına, Azure portal erişim anahtarıyla birlikte ihtiyacınız olacaktır.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Depolama Gezgini bir bilgi deposunu görüntüleyin, düzenleyin ve sorgulayın

1. Azure portal, bilgi deposunu oluşturmak için kullandığınız [Depolama hesabını açın](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) .

1. Depolama hesabının sol gezinti bölmesinde **Depolama Gezgini**' ye tıklayın.

1. Otelinizde **veri alma** Sihirbazı 'nı çalıştırdığınızda oluşturulan Azure Tablo projeksiyonlarını bir liste olarak görmek için **Tablolar** listesini genişletin, örnek verileri gözden geçirir.

Anahtar ifadeler ve yaklaşım puanları dahil olmak üzere zenginleştirilmiş verileri görüntülemek için herhangi bir tablo seçin.

   ![Depolama Gezgini tabloları görüntüleme](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Depolama Gezgini tabloları görüntüleme")

Herhangi bir tablo değeri için veri türünü değiştirmek veya tablonuzdaki tek tek değerleri değiştirmek için, **Düzenle**' ye tıklayın. Bir tablo satırındaki herhangi bir sütunun veri türünü değiştirdiğinizde, tüm satırlara uygulanır.

   ![Depolama Gezgini tabloyu Düzenle](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Depolama Gezgini tabloyu Düzenle")

Sorguları çalıştırmak için Komut çubuğundaki **sorgu** ' ya tıklayın ve koşullarınızı girin.  

   ![Depolama Gezgini sorgu tablosu](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Depolama Gezgini sorgu tablosu")

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha derin analizler için bu bilgi deposunu Power BI bağlayın veya farklı bir bilgi deposu oluşturmak için REST API ve Postman kullanarak kodla ilerle ilerleyin.

> [!div class="nextstepaction"]
> [Power BI bağlanma](knowledge-store-connect-power-bi.md) 
>  [Rest 'te bilgi deposu oluşturma](knowledge-store-create-rest.md)
