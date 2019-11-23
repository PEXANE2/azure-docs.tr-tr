---
title: Azure Blockchain Workbench verilerini Microsoft Power BI’da kullanma
description: Azure Blockchain Workbench SQL DB verilerini Microsoft Power BI’da yüklemeyi ve görüntülemeyi öğrenin.
ms.date: 05/09/2019
ms.topic: article
ms.reviewer: mmercuri
ms.openlocfilehash: 6e1f160c3563a280548c74ebe84f30bf08945c3f
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74324788"
---
# <a name="using-azure-blockchain-workbench-data-with-microsoft-power-bi"></a>Azure Blockchain Workbench verilerini Microsoft Power BI ile kullanma

Microsoft Power BI, Power BI Desktop uygulamasını kullanarak SQL DB veritabanlarından kolayca güçlü raporlar oluşturma ve bunları [https://www.powerbi.com](https://www.powerbi.com) üzerinde yayımlama olanağı sağlar.

Bu makale, Azure Blockchain Workbench'in SQL Veritabanı’na PowerBI Desktop’ın içinden bağlanma, bir rapor oluşturma ve raporu powerbi.com’a dağıtma sürecine ilişkin adım adım bir kılavuz içerir.

## <a name="prerequisites"></a>Önkoşullar

* Download [Power BI Desktop](https://aka.ms/pbidesktopstore).

## <a name="connecting-power-bi-to-data-in-azure-blockchain-workbench"></a>Connecting Power BI to data in Azure Blockchain Workbench

1.  Power BI Desktop’ı açın.
2.  **Veri Al**’ı seçin.

    ![Verileri alma](./media/data-powerbi/get-data.png)
3.  Veri kaynağı türleri listesinden **SQL Server**’ı seçin.

4.  İletişim kutusunda sunucu ve veritabanı adını belirtin. Veri içeri aktarmak istediğinizi mi yoksa bir **DirectQuery** mi gerçekleştirmek istediğinizi belirtin. **Tamam**’ı seçin.

    ![SQL Server'ı seçin](./media/data-powerbi/select-sql.png)

5.  Azure Blockchain Workbench’e erişmek için veritabanı kimlik bilgilerini sağlayın. **Veritabanı**’nı seçip kimlik bilgilerinizi girin.

    Azure Blockchain Workbench dağıtım işlemi tarafından oluşturulan kimlik bilgilerini kullanıyorsanız kullanıcı adı **dbadmin**, parola ise dağıtım sırasında belirttiğiniz paroladır.

    ![SQL DB ayarları](./media/data-powerbi/db-settings.png)

6.  Veritabanına bağlandığınızda **Gezgin** iletişim kutusunda veritabanındaki tablolar ve görünümler görüntülenir. Görünümler raporlama için tasarlanmıştır ve hepsinin önünde **vw** eki olur.

    ![Gezgin](./media/data-powerbi/navigator.png)

7.  Dahil etmek istediğiniz görünümleri seçin. For demonstration purposes, we include **vwContractAction**, which provides details on the actions that have taken place on a contract.

    ![Görünümleri seçin](./media/data-powerbi/select-views.png)

Artık Power BI ile normalde yaptığınız gibi rapor oluşturup yayımlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’te veritabanı görünümleri](database-views.md)