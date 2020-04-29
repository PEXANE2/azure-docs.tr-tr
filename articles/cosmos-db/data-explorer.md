---
title: Verilerinizi yönetmek için Azure Cosmos DB gezginini kullanın
description: Azure Cosmos DB Explorer, Azure Cosmos DB depolanan verileri görüntülemenize ve yönetmenize olanak tanıyan tek başına bir Web tabanlı arabirimdir.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dech
ms.openlocfilehash: 57ba647ade45928f03cd7fb2b037642d5e4d52cc
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79096806"
---
# <a name="work-with-data-using-azure-cosmos-explorer"></a>Azure Cosmos gezginini kullanarak verilerle çalışma 

Azure Cosmos DB Explorer, Azure Cosmos DB depolanan verileri görüntülemenize ve yönetmenize olanak tanıyan tek başına bir Web tabanlı arabirimdir. Azure Cosmos DB gezgin, Azure Cosmos DB bir hesap oluştururken Azure portal kullanılabilen mevcut **Veri Gezgini** sekmeye eşdeğerdir. Azure Cosmos DB Explorer 'ın mevcut veri Gezgini üzerinde başlıca avantajları şunlardır:

* Verilerinizi görüntülemek, sorguları çalıştırmak, saklı yordamları, Tetikleyicileri ve sonuçlarını görüntülemek için gerçek bir tam ekran vardır.  

* Veritabanı hesabınıza ve koleksiyonlarına Azure portal veya aboneliğine erişimi olmayan diğer kullanıcılara geçici veya kalıcı okuma veya okuma erişimi sağlayabilirsiniz.  

* Sorgu sonuçlarını Azure portal veya aboneliğine erişimi olmayan diğer kullanıcılarla paylaşabilirsiniz.  

## <a name="access-azure-cosmos-db-explorer"></a>Azure Cosmos DB Gezgini 'ne erişin

1. [Azure portalında](https://portal.azure.com/)oturum açın. 

2. **Tüm kaynaklardan**Azure Cosmos DB hesabınıza gidin, anahtarlar ' ı seçin ve **birincil bağlantı dizesini**kopyalayın.  

3. Adresine gidin https://cosmos.azure.com/, bağlantı dizesini yapıştırın ve **Bağlan**' ı seçin. Bağlantı dizesini kullanarak, Azure Cosmos DB Gezgini 'ne herhangi bir zaman sınırlaması olmadan erişebilirsiniz.  

   Diğer kullanıcıların Azure Cosmos DB hesabınıza geçici olarak erişmesini sağlamak istiyorsanız, okuma-yazma ve okuma erişimi URL 'Lerini kullanarak bunu yapabilirsiniz. 

4. **Veri Gezgini** dikey penceresini açın, **tam ekran aç**' ı seçin. Açılır iletişim kutusunda, iki erişim URL 'sini görüntüleyebilirsiniz – **okuma-yazma** ve **okuma**. Bu URL 'Ler Azure Cosmos DB hesabınızı diğer kullanıcılarla geçici olarak paylaşmanızı sağlar. Hesap erişimi, yeni bir erişim URL 'SI veya bağlantı dizesi kullanarak yeniden bağlanabildikten sonra 24 saat sonra sona erer. 

   **Okuma-yazma** : okuma-yazma URL 'sini diğer kullanıcılarla paylaştığınızda, bu ilgili hesapla ilişkili veritabanlarını, koleksiyonları, sorguları ve diğer kaynakları görüntüleyebilir ve değiştirebilirler.

   **Okuma** -salt okuma URL 'sini diğer kullanıcılarla paylaştığınızda, bu ilgili hesapla ilişkili veritabanlarını, koleksiyonları, sorguları ve diğer kaynakları görüntüleyebilirler. Örneğin, bir sorgunun sonuçlarını Azure portal veya Azure Cosmos DB hesabınıza erişimi olmayan takım arkadaşlarınızla paylaşmak istiyorsanız, bu URL ile birlikte sağlayabilirsiniz.

   Hesabı açmak istediğiniz erişim türünü seçin ve **Aç**' a tıklayın. Gezgin 'i açtıktan sonra, deneyim, Azure portal Veri Gezgini sekmesi ile aynı olur.   

   ![Azure Cosmos DB Gezginini aç](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Bilinen sorunlar

Şu anda, Azure Cosmos DB Gremlin ve Tablo API'si hesapları için geçici okuma/yazma veya okuma erişimini paylaşmanıza olanak sağlayan **açık tam ekran** deneyimi henüz desteklenmiyor. Bağlantı dizesini Azure Cosmos DB Explorer 'a geçirerek Gremlin ve Tablo API'si hesaplarınızı görüntülemeye devam edebilirsiniz. 

Şu anda, bir UUID içeren belgeleri görüntülemek Veri Gezgini desteklenmez. Bu, yalnızca bu belgeleri içeren tek tek belgeleri veya sorguları görüntülemek için koleksiyonları yüklemeyi etkilemez. Bu belgeleri görüntülemek ve yönetmek için, kullanıcılar başlangıçta bu belgeleri oluşturmak için kullanılan aracı kullanmaya devam etmelidir.

## <a name="next-steps"></a>Sonraki adımlar
Artık verilerinizi yönetmek için Azure Cosmos DB Gezgini 'ni kullanmaya nasıl başladığınıza öğrendiğinize göre şunları yapabilirsiniz:

* SQL söz dizimini kullanarak [sorgu](sql-api-query-reference.md) tanımlamayı başlatın ve saklı yordamları, UDF 'leri, Tetikleyicileri kullanarak [sunucu tarafı programlama](stored-procedures-triggers-udfs.md) işlemi gerçekleştirin. 
