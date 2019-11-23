---
title: Azure Blockchain Workbench veritabanı ayrıntılarını alma
description: Learn how to get Azure Blockchain Workbench Preview database and database server information.
ms.date: 09/05/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: 2b3190a9d042be8ead1ff3d5ef48d4a2a19e8963
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74324686"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Azure Blockchain Workbench veritabanınızla ilgili bilgi edinin

This article shows how to get detailed information about your Azure Blockchain Workbench Preview database.

## <a name="overview"></a>Genel Bakış

Uygulamalar, iş akışları ve akıllı sözleşme yürütme ile ilgili bilgiler Blockchain Workbench SQL DB’deki veritabanı görünümleri kullanılarak sağlanır. Developers can use this information when using tools such as Microsoft Excel, Power BI, Visual Studio, and SQL Server Management Studio.

Bir geliştiricinin veritabanına bağlanabilmesi için önce şunlar gerekir:

* Veritabanı güvenlik duvarında dış istemci erişimine izin verilmesi. Veritabanı güvenlik duvarı yapılandırmayla ilgili bu makalede erişime nasıl izin verileceği açıklanır.
* Veritabanı sunucu adı ve veritabanı adı.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain Workbench veritabanına bağlanma

Veritabanına bağlanmak için:

1. Sign in to the Azure portal with an account that has **Owner** permissions for the Azure Blockchain Workbench resources.
2. Sol gezinti bölmesinden **Kaynak Grupları**'nı seçin.
3. Blockchain Workbench dağıtımınıza yönelik kaynak grubunun adını seçin.
4. Kaynak listesini sıralamak için **Tür**’ü, sonra da **SQL sunucunuzu** seçin. Bir sonraki ekran görüntüsünde yer alan sıralı listede "master" adlı bir veritabanının yanı sıra **Kaynak ön eki** olarak "lhgn" kullanan bir veritabanı şeklinde iki SQL veritabanı gösterilir.

   ![Sıralı Blockchain Workbench kaynak listesi](./media/getdb-details/sorted-workbench-resource-list.png)

5. Blockchain Workbench veritabanıyla ilgili ayrıntılı bilgileri görüntülemek üzere Blockchain Workbench’i dağıtmak için sağladığınız **Kaynak ön ekine** sahip veritabanının bağlantısını seçin.

   ![Veritabanı ayrıntıları](./media/getdb-details/workbench-db-details.png)

Veritabanı sunucu adı ve veritabanı adı, geliştirme ya da raporlama aracınızı kullanarak Blockchain Workbench veritabanına bağlanmanıza imkan tanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](database-views.md)