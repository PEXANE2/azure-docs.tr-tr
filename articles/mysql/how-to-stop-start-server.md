---
title: Durdur/Başlat-Azure portal-MySQL için Azure veritabanı sunucusu
description: Bu makalede, MySQL için Azure veritabanı 'nda işlemleri durdurma/başlatma işlemi açıklanır.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 95be6aa576d9d059ce419443f8c7e32af5ff397a
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91826212"
---
# <a name="stopstart-an-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nı Durdur/Başlat

> [!IMPORTANT]
> MySQL için Azure veritabanı 'nın Durdur/Başlat işlevselliği Şu anda genel önizleme aşamasındadır.

Bu makalede, tek sunucu durdurma ve başlatma işlemleri gerçekleştirmeye yönelik adım adım yordamlar sunulmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

-   MySQL için Azure veritabanı tek sunucu olmalıdır.

> [!NOTE]
> [Durdur/Başlat](concepts-servers.md#limitations-of-stopstart-operation) kullanma sınırına bakın

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nı durdurma/başlatma

### <a name="stop-a-running-server"></a>Çalışan bir sunucuyu durdur

1.  [Azure Portal](https://portal.azure.com/), durdurmak istediğiniz MySQL sunucunuzu seçin.

2.  **Genel bakış** sayfasında, araç çubuğundaki **Durdur** düğmesine tıklayın.

    :::image type="content" source="./media/howto-stop-start-server/mysql-stop-server.png" alt-text="MySQL için Azure veritabanı sunucuyu durdur":::

    > [!NOTE]
    > Sunucu durdurulduktan sonra, tek sunucu için diğer yönetim işlemleri kullanılamaz.

### <a name="start-a-stopped-server"></a>Durdurulmuş bir sunucu başlatma

1.  [Azure Portal](https://portal.azure.com/), başlatmak istediğiniz tek sunucunuzu seçin.

2.  **Genel bakış** sayfasında, araç çubuğundaki **Başlat** düğmesine tıklayın.

    :::image type="content" source="./media/howto-stop-start-server/mysql-start-server.png" alt-text="MySQL için Azure veritabanı sunucuyu durdur":::

    > [!NOTE]
    > Sunucu başlatıldıktan sonra, tüm yönetim işlemleri artık tek bir sunucu için kullanılabilir.

## <a name="how-to-stopstart-the-azure-database-for-mysql-using-cli"></a>CLı kullanarak MySQL için Azure veritabanı 'nı durdurma/başlatma

### <a name="stop-a-running-server"></a>Çalışan bir sunucuyu durdur

1.  [Azure Portal](https://portal.azure.com/), durdurmak istediğiniz MySQL sunucunuzu seçin.

2.  **Genel bakış** sayfasında, araç çubuğundaki **Durdur** düğmesine tıklayın.

    ```azurecli-interactive
    az mysql server stop --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Sunucu durdurulduktan sonra, tek sunucu için diğer yönetim işlemleri kullanılamaz.

### <a name="start-a-stopped-server"></a>Durdurulmuş bir sunucu başlatma

1.  [Azure Portal](https://portal.azure.com/), başlatmak istediğiniz tek sunucunuzu seçin.

2.  **Genel bakış** sayfasında, araç çubuğundaki **Başlat** düğmesine tıklayın.

    ```azurecli-interactive
    az mysql server start --name <server-name> -g <resource-group-name>
    ```
    > [!NOTE]
    > Sunucu başlatıldıktan sonra, tüm yönetim işlemleri artık tek bir sunucu için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Ölçümler üzerinde uyarılar oluşturma](howto-alert-on-metric.md)hakkında bilgi edinin.
