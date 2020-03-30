---
title: Verilerinizi yönetmek için Azure Cosmos DB gezginini kullanın
description: Azure Cosmos DB explorer, Azure Cosmos DB'de depolanan verileri görüntülemenize ve yönetmenize olanak tanıyan bağımsız bir web tabanlı arabirimdir.
author: deborahc
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: dech
ms.openlocfilehash: 57ba647ade45928f03cd7fb2b037642d5e4d52cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096806"
---
# <a name="work-with-data-using-azure-cosmos-explorer"></a>Azure Cosmos gezginini kullanarak verilerle çalışma 

Azure Cosmos DB explorer, Azure Cosmos DB'de depolanan verileri görüntülemenize ve yönetmenize olanak tanıyan bağımsız bir web tabanlı arabirimdir. Azure Cosmos DB gezgini, bir Azure Cosmos DB hesabı oluşturduğunuzda Azure portalında kullanılabilen varolan **Veri Gezgini** sekmesine eşdeğerdir. Azure Cosmos DB explorer'ın varolan Veri gezginine göre en önemli avantajları şunlardır:

* Verilerinizi görüntülemek, sorguları çalıştırmak, depolanan yordamları, tetikleyicileri ve sonuçlarını görüntülemek için tam ekran bir mülküne sahipsiniz.  

* Azure portalına veya aboneliğine erişimi olmayan diğer kullanıcılara veritabanı hesabınıza ve koleksiyonlarına geçici veya kalıcı okuma veya okuma-yazma erişimi sağlayabilirsiniz.  

* Sorgu sonuçlarını Azure portalına veya aboneliğine erişimi olmayan diğer kullanıcılarla paylaşabilirsiniz.  

## <a name="access-azure-cosmos-db-explorer"></a>Azure Cosmos DB gezginine erişin

1. [Azure Portalı'nda](https://portal.azure.com/)oturum açın. 

2. **Tüm kaynaklardan**Azure Cosmos DB hesabınızda bulun ve gidin, Keys'i seçin ve **Birincil Bağlantı Dizesini**kopyalayın.  

3. Git, https://cosmos.azure.com/bağlantı dizesini yapıştır ve **Bağlan'ı**seç. Bağlantı dizesini kullanarak Azure Cosmos DB gezginine herhangi bir zaman sınırı olmadan erişebilirsiniz.  

   Diğer kullanıcılara Azure Cosmos DB hesabınıza geçici erişim sağlamak istiyorsanız, bunu okuma-yazma ve okuma erişim URL'lerini kullanarak yapabilirsiniz. 

4. Data **Explorer** bıçağını açın, **Tam Ekranı Aç'ı**seçin. Açılan iletişim kutusundan iki erişim URL'sini görüntüleyebilirsiniz – **Oku-Yaz** ve **Oku**. Bu URL'ler, Azure Cosmos DB hesabınızı geçici olarak diğer kullanıcılarla paylaşmanızı sağlar. Hesaba erişim, yeni bir erişim URL'si veya bağlantı dizesini kullanarak yeniden bağlanabileceğiniz 24 saat içinde sona erer. 

   **Oku-Yaz – Oku-Yaz** URL'sini diğer kullanıcılarla paylaştığınızda, bu hesapla ilişkili veritabanlarını, koleksiyonları, sorguları ve diğer kaynakları görüntüleyebilir ve değiştirebilirler.

   **Oku** - Salt okunur URL'yi diğer kullanıcılarla paylaştığınızda, bu hesapla ilişkili veritabanlarını, koleksiyonları, sorguları ve diğer kaynakları görüntüleyebilirler. Örneğin, bir sorgunun sonuçlarını Azure portalına veya Azure Cosmos DB hesabınıza erişimi olmayan takım arkadaşlarınızla paylaşmak istiyorsanız, bu URL'yi sağlayabilirsiniz.

   Hesabı açmak istediğiniz erişim türünü seçin ve **Aç'ı**tıklatın. Gezgini açtıktan sonra, azure portalındaki Veri Gezgini sekmesinde yaptığınız deneyimle aynıdır.   

   ![Azure Cosmos DB gezginini aç](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Bilinen sorunlar

Şu anda geçici okuma yazma veya okuma erişimini paylaşmanızı sağlayan **Açık Tam Ekran** deneyimi, Azure Cosmos DB Gremlin ve Table API hesapları için henüz desteklenmez. Bağlantı dizesini Azure Cosmos DB Explorer'a geçirerek Gremlin ve Tablo API hesaplarınızı görüntülemeye devam edebilirsiniz. 

Şu anda, Veri Gezgini'nde UUID içeren belgeleri görüntüleme desteklenmez. Bu, yükleme koleksiyonlarını etkilemez, yalnızca bu belgeleri içeren tek tek belgeleri veya sorguları görüntüler. Bu belgeleri görüntülemek ve yönetmek için, kullanıcıların bu belgeleri oluşturmak için başlangıçta kullanılan aracı kullanmaya devam etmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar
Verilerinizi yönetmek için Azure Cosmos DB explorer'a nasıl başladığınızı öğrendiğinize göre, bir sonraki

* SQL sözdizimini kullanarak [sorguları](sql-api-query-reference.md) tanımlamaya başlayın ve depolanan yordamları, UDF'leri, tetikleyicileri kullanarak [sunucu yan programlamayı](stored-procedures-triggers-udfs.md) gerçekleştirin. 
