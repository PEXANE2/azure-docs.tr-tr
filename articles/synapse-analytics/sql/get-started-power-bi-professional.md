---
title: Power BI Professional'a bağlanın
description: Bu eğitimde, Power BI masaüstünü isteğe bağlı SQL'e (önizleme) nasıl bağlayacak adımlardan geçeceğiz.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: tutorial
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 0ce8f3a447f1896ae6d96d343782f8cdb44d4c6f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81422567"
---
# <a name="connect-to-synapse-sql-with-power-bi-professional"></a>Power BI Professional ile Synapse SQL'e bağlanın

> [!div class="op_single_selector"]
>
> - [Azure Data Studio](get-started-azure-data-studio.md)
> - [Power BI](get-started-power-bi-professional.md)
> - [Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)
> - [Sqlcmd](../sql/get-started-connect-sqlcmd.md)
> - [SSMS](get-started-ssms.md)

Bu eğitimde, Power BI masaüstünü isteğe bağlı SQL'e (önizleme) nasıl bağlayacak adımlardan geçeceğiz.

## <a name="prerequisites"></a>Ön koşullar

Sorgu verme aracı:

- Seçtiğiniz SQL istemcisi:

  - Azure Data Studio
  - SQL Server Management Studio

- Güç BI masaüstü yüklü

Parametreler:

| Parametre                                 | Açıklama                                                   |
| ----------------------------------------- | ------------------------------------------------------------- |
| SQL isteğe bağlı hizmet bitiş noktası adresi    | Sunucu adı olarak kullanılacak                                   |
| SQL isteğe bağlı hizmet bitiş noktası bölgesi     | Örneklerde hangi depolama alanını kullanacağımızı belirlemek için kullanılacaktır |
| Uç nokta erişimi için kullanıcı adı ve parola | Uç noktaya erişmek için kullanılacak                               |
| Görünüm oluşturmak için kullanacağınız veritabanı     | Bu veritabanı örneklerde başlangıç noktası olarak kullanılacaktır       |

## <a name="first-time-setup"></a>İlk kez kurulum

Örnekleri kullanmadan önce iki adım vardır:

1. Görünümleriniz için veritabanı oluşturma
2. Depolama daki dosyalara erişmek için isteğe bağlı OLARAK SQL tarafından kullanılacak kimlik bilgileri oluşturma

### <a name="create-database"></a>Veritabanı oluşturma

Demo ortamını kullanacağınız için, demo amacıyla kendi veritabanınızı oluşturmalısınız. Veritabanı içinde görünümler oluşturmak için gereklidir. Bu veritabanını, bu belgedeki bazı örnek sorgularda kullanırsınız.

> [!NOTE]
> Veritabanlarının gerçek veriler için değil, yalnızca görüntüleme meta verileri için kullanıldığını unutmayın.
>
> Kullandığınız veritabanı adını yazın, daha sonra ihtiyacınız olacak.

```sql
DROP DATABASE IF EXISTS demo;
```

### <a name="create-credentials"></a>Kimlik bilgileri oluşturma

Sorguları çalıştırabilmeniz için kimlik bilgisi oluşturmamız gerekir. Bu kimlik bilgisi, depolama daki dosyalara erişmek için SQL on-demand hizmeti tarafından kullanılacaktır.

> [!NOTE]
> Depolama hesabına erişmek için kimlik bilgisi oluşturmanız gerektiğini unutmayın. İsteğe bağlı SQL farklı bölgelerden depolama lara erişebilse de, aynı bölgede depolama ve Azure Synapse çalışma alanına sahip olmak daha iyi performans deneyimi sağlar.

**Nüfus Sayımı veri kapsayıcıları için kimlik bilgileri oluşturmak için nasıl kod snippet**, çalıştırın:

```sql
IF EXISTS (SELECT * FROM sys.credentials WHERE name = 'https://azureopendatastorage.blob.core.windows.net/censusdatacontainer')
DROP CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer];
GO

-- Create credentials for Census Data container which resides in a azure open data storage account
-- There is no secret. We are using public storage account which doesn't need secret
CREATE CREDENTIAL [https://azureopendatastorage.blob.core.windows.net/censusdatacontainer]  
WITH IDENTITY='SHARED ACCESS SIGNATURE',  
SECRET = '';
GO
```

## <a name="creating-power-bi-desktop-report"></a>Power BI masaüstü raporu oluşturma

Power BI masaüstü uygulamasını açın ve "Veri al" seçeneğini belirleyin.
![Power BI masaüstü uygulamasını açın ve veri almayı seçin.](./media/get-started-power-bi-professional/step-0-open-powerbi.png)

### <a name="step-1---select-data-source"></a>Adım 1 - Veri kaynağını seçin

Menüde "Azure" ve ardından "Azure SQL Veritabanı"nı seçin.
![Veri kaynağını seçin.](./media/get-started-power-bi-professional/step-1-select-data-source.png)

### <a name="step-2---select-database"></a>Adım 2 - Veritabanını seçin

Veritabanı için URL yazın ve görünümün bulunduğu veritabanının adı.
![Bitiş noktasında veritabanını seçin.](./media/get-started-power-bi-professional/step-2-db.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure Data Studio'yu kullanarak isteğe bağlı SQL'e nasıl bağlanıp bağlanılabildiğini öğrenmek için [Sorgu depolama dosyalarına](get-started-azure-data-studio.md) ilerleyin.
 