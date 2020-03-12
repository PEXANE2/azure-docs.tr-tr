---
title: Verilerinizi yönetmek için Azure Cosmos DB Gezgini'ni kullanma
description: Azure Cosmos DB Gezgini'ni görüntülemek ve Azure Cosmos DB'de depolanan verileri yönetmenize olanak sağlayan bir tek başına web tabanlı arabirimidir.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dech
ms.openlocfilehash: 57ba647ade45928f03cd7fb2b037642d5e4d52cc
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79096806"
---
# <a name="work-with-data-using-azure-cosmos-explorer"></a>Azure Cosmos gezginini kullanarak verilerle çalışma 

Azure Cosmos DB Gezgini'ni görüntülemek ve Azure Cosmos DB'de depolanan verileri yönetmenize olanak sağlayan bir tek başına web tabanlı arabirimidir. Azure Cosmos DB gezgin, Azure Cosmos DB bir hesap oluştururken Azure portal kullanılabilen mevcut **Veri Gezgini** sekmeye eşdeğerdir. Mevcut Veri Gezgini üzerinden Azure Cosmos DB explorer'ın önemli avantajlar şunlardır:

* Bir tam veri, sorgu çalıştırma, saklı yordamlar, Tetikleyiciler görüntülemek için ekrandan sahip ve sonuçları görüntüleyin.  

* Veritabanı hesabınız ve Azure portalı veya abonelik erişimi olmayan diğer kullanıcılara koleksiyonları için geçici veya kalıcı okuma veya okuma-yazma erişimi sağlar.  

* Sorgu sonuçları, Azure portalı veya abonelik erişimi olmayan diğer kullanıcılarla paylaşabilirsiniz.  

## <a name="access-azure-cosmos-db-explorer"></a>Azure Cosmos DB Gezgini'ni erişim

1. [Azure portalında](https://portal.azure.com/)oturum açın. 

2. **Tüm kaynaklardan**Azure Cosmos DB hesabınıza gidin, anahtarlar ' ı seçin ve **birincil bağlantı dizesini**kopyalayın.  

3. https://cosmos.azure.com/gidin, bağlantı dizesini yapıştırın ve **Bağlan**' ı seçin. Bağlantı dizesi kullanarak Azure Cosmos DB Gezgini'ni hiçbir zaman Kısıtlamasız erişebilir.  

   Diğer kullanıcıların Azure Cosmos DB hesabınız geçici erişim sağlamak istiyorsanız, okuma-yazma ve okuma erişimi URL'leri kullanarak bunu yapabilirsiniz. 

4. **Veri Gezgini** dikey penceresini açın, **tam ekran aç**' ı seçin. Açılır iletişim kutusunda, iki erişim URL 'sini görüntüleyebilirsiniz – **okuma-yazma** ve **okuma**. Bu URL'leri, Azure Cosmos DB hesabınız geçici olarak diğer kullanıcılarla paylaşmanıza olanak sağlar. Hesabına erişimi süre sonu 24 saat sonra yeni bir erişim URL'si veya bağlantı dizesi kullanarak bağlanabilirsiniz. 

   **Okuma-yazma** : okuma-yazma URL 'sini diğer kullanıcılarla paylaştığınızda, bu ilgili hesapla ilişkili veritabanlarını, koleksiyonları, sorguları ve diğer kaynakları görüntüleyebilir ve değiştirebilirler.

   **Okuma** -salt okuma URL 'sini diğer kullanıcılarla paylaştığınızda, bu ilgili hesapla ilişkili veritabanlarını, koleksiyonları, sorguları ve diğer kaynakları görüntüleyebilirler. Örneğin, bir sorgunun sonuçlarını Azure portal veya Azure Cosmos DB hesabınıza erişimi olmayan arkadaşlarınızla paylaşmak istiyorsanız, bunları bu URL ile sağlayabilir.

   Hesabı açmak istediğiniz erişim türünü seçin ve **Aç**' a tıklayın. Explorer'ı açın, sonra Azure portalında Veri Gezgini sekmesi olduğu gibi aynı deneyimidir.   

   ![Azure Cosmos DB Gezgini'ni Aç](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Bilinen sorunlar

Şu anda, Azure Cosmos DB Gremlin ve Tablo API'si hesapları için geçici okuma/yazma veya okuma erişimini paylaşmanıza olanak sağlayan **açık tam ekran** deneyimi henüz desteklenmiyor. Azure Cosmos DB Gezgini'ni için bağlantı dizesi geçirerek, Gremlin ve tablo API'si hesapları daha da görüntüleyebilirsiniz. 

Şu anda, bir UUID içeren belgeleri görüntülemek Veri Gezgini desteklenmez. Bu, yalnızca bu belgeleri içeren tek tek belgeleri veya sorguları görüntülemek için koleksiyonları yüklemeyi etkilemez. Bu belgeleri görüntülemek ve yönetmek için, kullanıcılar başlangıçta bu belgeleri oluşturmak için kullanılan aracı kullanmaya devam etmelidir.

## <a name="next-steps"></a>Sonraki adımlar
Sonraki verilerinizi yönetmek için Azure Cosmos DB Gezgini ile çalışmaya başlama gerçekleştirmeyi öğrendiniz, şunları yapabilirsiniz:

* SQL söz dizimini kullanarak [sorgu](sql-api-query-reference.md) tanımlamayı başlatın ve saklı yordamları, UDF 'leri, Tetikleyicileri kullanarak [sunucu tarafı programlama](stored-procedures-triggers-udfs.md) işlemi gerçekleştirin. 
