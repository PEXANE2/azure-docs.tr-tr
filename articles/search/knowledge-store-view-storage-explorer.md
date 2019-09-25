---
title: Depolama Gezgini Azure Search bir bilgi deposu görüntüleme
description: Azure portal Depolama Gezgini bir Azure Search bilgi deposu görüntüleyin ve çözümleyin.
author: lisaleib
services: search
ms.service: search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: 9ec93bcd2309bb47b24983260c35726ac24b5e95
ms.sourcegitcommit: 3f22ae300425fb30be47992c7e46f0abc2e68478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71265614"
---
# <a name="view-a-knowledge-store-with-storage-explorer"></a>Depolama Gezgini bir bilgi deposu görüntüleme

> [!Note]
> Bilgi deposu önizlemededir ve üretimde kullanılmamalıdır. [Azure Search REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>
Bu makalede, Azure portal Depolama Gezgini kullanarak bir bilgi deposunu nasıl bağlayacağınızı ve keşfedeceğinizi öğreneceksiniz. Bu kılavuzda kullanılan bilgi deposu örneğini oluşturmak için, bkz. [Azure Portal bilgi deposu oluşturma](knowledge-store-create-portal.md).

## <a name="prerequisites"></a>Önkoşullar

+ Bu kılavuzda kullanılan örnek bilgi deposunu oluşturmak için [Azure Portal bilgi deposu oluşturma](knowledge-store-create-portal.md) bölümündeki adımları izleyin.

+ Ayrıca, bilgi deposunu oluşturmak için kullandığınız Azure depolama hesabının adına, Azure portal erişim anahtarıyla birlikte ihtiyacınız olacaktır.

## <a name="view-edit-and-query-a-knowledge-store-in-storage-explorer"></a>Depolama Gezgini bir bilgi deposunu görüntüleyin, düzenleyin ve sorgulayın

1. Azure portal, bilgi deposunu oluşturmak için kullandığınız depolama hesabını açın.

1. Depolama hesabının sol gezinti bölmesinde **Depolama Gezgini**' ye tıklayın.

1. Otelinizde **veri alma** Sihirbazı 'nı çalıştırdığınızda oluşturulan Azure Tablo projeksiyonlarını bir liste olarak görmek için **Tablolar** listesini genişletin, örnek verileri gözden geçirir.

Anahtar tümceleri yaklaşım puanları, enlem ve Boylam konum verileri ve daha fazlası dahil olmak üzere zenginleştirilmiş verileri görüntülemek için herhangi bir tablo seçin.

   ![Depolama Gezgini tabloları görüntüleme](media/knowledge-store-view-storage-explorer/storage-explorer-tables.png "Depolama Gezgini tabloları görüntüleme")

Herhangi bir tablo değeri için veri türünü değiştirmek veya tablonuzdaki tek tek değerleri değiştirmek için, **Düzenle**' ye tıklayın. Bir tablo satırındaki herhangi bir sütunun veri türünü değiştirdiğinizde, tüm satırlara uygulanır.

   ![Depolama Gezgini tabloyu Düzenle](media/knowledge-store-view-storage-explorer/storage-explorer-edit-table.png "Depolama Gezgini tabloyu Düzenle")

Sorguları çalıştırmak için Komut çubuğundaki **sorgu** ' ya tıklayın ve koşullarınızı girin.  

   ![Depolama Gezgini sorgu tablosu](media/knowledge-store-view-storage-explorer/storage-explorer-query-table.png "Depolama Gezgini sorgu tablosu")

## <a name="clean-up"></a>Temizleme

Kendi aboneliğinizde çalışırken, sizin oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek için bir projenin sonunda iyi bir fikir olur. Çalışan kaynaklar sizin için ücret verebilir. Kaynakları tek tek silebilir veya kaynak grubunu silerek tüm kaynak kümesini silebilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu bilgi deposunu Power BI bağlama hakkında bilgi edinmek için aşağıdaki makaleye bakın.

> [!div class="nextstepaction"]
> [Power BI ile bağlanma](knowledge-store-connect-power-bi.md)

REST API 'Leri ve Postman kullanarak bilgi deposu oluşturmayı öğrenmek için aşağıdaki makaleye bakın.  

> [!div class="nextstepaction"]
> [REST 'te bilgi deposu oluşturma](knowledge-store-howto.md)
