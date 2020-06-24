---
title: Azure Blockchain Workbench veritabanı ayrıntılarını alma
description: Azure blok zinciri çalışma ekranı önizleme veritabanı ve veritabanı sunucusu bilgilerini nasıl alabileceğinizi öğrenin.
ms.date: 09/05/2019
ms.topic: how-to
ms.reviewer: mmercuri
ms.openlocfilehash: 898360d8e22803e17ff4da7d3b63f5595cad47ba
ms.sourcegitcommit: bf99428d2562a70f42b5a04021dde6ef26c3ec3a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85254659"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Azure Blockchain Workbench veritabanınızla ilgili bilgi edinin

Bu makalede, Azure blok zinciri çalışma ekranı önizleme veritabanınız hakkında ayrıntılı bilgilerin nasıl alınacağı gösterilmektedir.

## <a name="overview"></a>Genel Bakış

Uygulamalar, iş akışları ve akıllı sözleşme yürütme ile ilgili bilgiler Blockchain Workbench SQL DB’deki veritabanı görünümleri kullanılarak sağlanır. Geliştiriciler Microsoft Excel, Power BI, Visual Studio ve SQL Server Management Studio gibi araçları kullanırken bu bilgileri kullanabilir.

Bir geliştiricinin veritabanına bağlanabilmesi için önce şunlar gerekir:

* Veritabanı güvenlik duvarında dış istemci erişimine izin verilmesi. Veritabanı güvenlik duvarı yapılandırmayla ilgili bu makalede erişime nasıl izin verileceği açıklanır.
* Veritabanı sunucu adı ve veritabanı adı.

## <a name="connect-to-the-blockchain-workbench-database"></a>Blockchain Workbench veritabanına bağlanma

Veritabanına bağlanmak için:

1. Azure blok zinciri çalışma ekranı kaynakları için **sahip** izinlerine sahip bir hesapla Azure Portal oturum açın.
2. Sol gezinti bölmesinden **Kaynak Grupları**'nı seçin.
3. Blockchain Workbench dağıtımınıza yönelik kaynak grubunun adını seçin.
4. Kaynak listesini sıralamak için **Tür**’ü, sonra da **SQL sunucunuzu** seçin. Sonraki ekran yakalamadaki sıralanmış liste, "ana" ve **kaynak ön eki**olarak "lhgn" kullanan iki veritabanını gösterir.

   ![Sıralı Blockchain Workbench kaynak listesi](./media/getdb-details/sorted-workbench-resource-list.png)

5. Blockchain Workbench veritabanıyla ilgili ayrıntılı bilgileri görüntülemek üzere Blockchain Workbench’i dağıtmak için sağladığınız **Kaynak ön ekine** sahip veritabanının bağlantısını seçin.

   ![Veritabanı ayrıntıları](./media/getdb-details/workbench-db-details.png)

Veritabanı sunucu adı ve veritabanı adı, geliştirme ya da raporlama aracınızı kullanarak Blockchain Workbench veritabanına bağlanmanıza imkan tanır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](database-views.md)