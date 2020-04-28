---
title: Depolama Gezgini ile bilgi deposunu (Önizleme) görüntüleme
titleSuffix: Azure Cognitive Search
description: Azure portal Depolama Gezgini bir Azure Bilişsel Arama bilgi deposu görüntüleyin ve çözümleyin. bilgi deposu Şu anda genel önizleme aşamasındadır.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/30/2019
ms.openlocfilehash: 167316eca1f85530a040d4543f98ae34a9fb93c6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75754071"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Depolama Gezgini bir bilgi deposu görüntüleme

> [!IMPORTANT] 
> Bilgi deposu Şu anda genel önizleme aşamasındadır. Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). [REST API sürüm 2019-05-06-önizleme](search-api-preview.md) , Önizleme özellikleri sağlar. Şu anda sınırlı sayıda portal desteği var ve .NET SDK desteği yok.

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
> [Connect with Power BI](knowledge-store-connect-power-bi.md)
> [Rest 'te bilgi deposu oluşturmak](knowledge-store-create-rest.md) Power BI bağlanma
