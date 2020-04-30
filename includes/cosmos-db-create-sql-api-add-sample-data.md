---
title: include dosyası
description: include dosyası
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 08/07/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: f80efbac256871af073354f23317c447d6a85f1e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "70020157"
---
Artık Veri Gezgini kullanarak yeni kapsayıcınıza veri ekleyebilirsiniz.

1. **Veri Gezgini**, **Görevler** veritabanını genişletin, **öğeler** kapsayıcısını genişletin. **Öğeler**' i ve sonra **Yeni öğe**' yi seçin.

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-new-document.png)
  
2. Şimdi aşağıdaki yapıyla kapsayıcıya bir belge ekleyin.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. JSON 'u **Belgeler** sekmesine ekledikten sonra **Kaydet**' i seçin.

    ![JSON verilerini kopyalayın ve Azure portal Veri Gezgini Kaydet ' i seçin](./media/cosmos-db-create-sql-api-add-sample-data/azure-cosmosdb-data-explorer-save-document.png)

4.  `id` özelliği için benzersiz bir değer eklediğiniz yerde bir veya daha fazla belge oluşturun ve kaydedin ve diğer özellikleri uygun şekilde değiştirin. Azure Cosmos DB, verilerinizin bir şemaya uygun olmasını şart koşmadığı için yeni belgelerinizin yapısını istediğiniz şekilde oluşturabilirsiniz.