---
title: Depolama Gezgini ile bir bilgi deposu (önizleme) görüntüleme
titleSuffix: Azure Cognitive Search
description: Azure portalının Depolama Gezgini ile bir Azure Bilişsel Arama bilgi deposunu görüntüleyin ve analiz edin. bilgi deposu şu anda genel önizlemede.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/30/2019
ms.openlocfilehash: 167316eca1f85530a040d4543f98ae34a9fb93c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75754071"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Depolama Gezgini ile bilgi deposugörüntüleme

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. [REST API sürümü 2019-05-06-Önizleme](search-api-preview.md) önizleme özellikleri sağlar. Şu anda sınırlı portal desteği ve .NET SDK desteği yoktur.

Bu makalede, Azure portalındaki Storage Explorer'ı kullanarak bir bilgi deposuna nasıl bağlanabileceğinizi ve nasıl keşfedeceğinizi örnek olarak öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

+ Bu iznin içinde kullanılan örnek bilgi deposunu oluşturmak için [Azure portalında bir bilgi deposu oluştur'daki](knowledge-store-create-portal.md) adımları izleyin.

+ Ayrıca, bilgi deposunu oluşturmak için kullandığınız Azure depolama hesabının adının yanı sıra Azure portalından erişim anahtarına da ihtiyacınız olacaktır.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Depolama Gezgini'nde bir bilgi deposugörüntüleme, görüntüleme ve sorgula

1. Azure portalında, bilgi deposunu oluşturmak için kullandığınız [Depolama hesabını açın.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/)

1. Depolama hesabının sol gezinti bölmesinde **Depolama Gezgini'ni**tıklatın.

1. Otel incelemeleri örnek verilerinizde **İçe Aktar Veri** sihirbazını çalıştırdığınızda oluşturulan Azure tablo projeksiyonlarının listesini göstermek için **TABLOLAR** listesini genişletin.

Anahtar tümcecikler ve duyarlılık puanları da dahil olmak üzere zenginleştirilmiş verileri görüntülemek için herhangi bir tabloseçin.

   ![Depolama Gezgini'ndeki tabloları görüntüleme](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Depolama Gezgini'ndeki tabloları görüntüleme")

Herhangi bir tablo değeri için veri türünü değiştirmek veya tablonuzdaki tek tek değerleri değiştirmek **için, Edit'i**tıklatın. Bir tablo satırındaki herhangi bir sütunun veri türünü değiştirdiğinizde, tüm satırlara uygulanır.

   ![Depolama Gezgini'nde tabloyu edin](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Depolama Gezgini'nde tabloyu edin")

Sorguları çalıştırmak için **Query** komut çubuğunda Sorgula'yı tıklatın ve koşullarınızı girin.  

   ![Depolama Gezgini'nde sorgu tablosu](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Depolama Gezgini'nde sorgu tablosu")

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu bilgi deposunu daha derin bir analiz için Power BI'ye bağlayın veya farklı bir bilgi deposu oluşturmak için REST API ve Postacı'yı kullanarak kodla ilerleyin.

> [!div class="nextstepaction"]
> [Power BI](knowledge-store-connect-power-bi.md)
> ile[bağlanıN REST'te bir bilgi deposu oluşturun](knowledge-store-create-rest.md)
