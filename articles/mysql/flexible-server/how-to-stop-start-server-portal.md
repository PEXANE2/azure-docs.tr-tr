---
title: Durdur/Başlat-Azure portal-MySQL için Azure veritabanı esnek sunucu
description: Bu makalede, Azure portal aracılığıyla MySQL için Azure veritabanı 'nda işlemleri durdurma/başlatma işlemi açıklanır.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/29/2020
ms.openlocfilehash: 15b08ea67afe0d307470b5a4fb0f7d26e0f4ea82
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93241931"
---
# <a name="stopstart-an-azure-database-for-mysql---flexible-server-preview"></a>MySQL için Azure veritabanını durdurma/başlatma-esnek sunucu (Önizleme)

> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

Bu makalede esnek sunucu durdurma ve başlatma işlemleri için adım adım yordamlar sunulmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

-   MySQL için Azure veritabanı esnek sunucusu olmalıdır.

## <a name="stop-a-running-server"></a>Çalışan bir sunucuyu durdur

1.  [Azure Portal](https://portal.azure.com/), durdurmak istediğiniz esnek sunucunuzu seçin.

2.  **Genel bakış** sayfasında, araç çubuğundaki **Durdur** düğmesine tıklayın.
    
    :::image type="content" source="media/how-to-stop-start-server-portal/stop-server.png" alt-text="Esnek sunucuyu durdurun."::: 

3.  Sunucunuzu durdurmayı onaylamak için **Evet** ' e tıklayın.

    :::image type="content" source="media/how-to-stop-start-server-portal/confirm-stop.png" alt-text="Esnek sunucu durdurmayı onaylayın."::: 

> [!NOTE]
> Sunucu durdurulduktan sonra, esnek sunucu için diğer yönetim işlemleri kullanılamaz.

## <a name="start-a-stopped-server"></a>Durdurulmuş bir sunucu başlatma

1.  [Azure Portal](https://portal.azure.com/), başlatmak istediğiniz esnek sunucunuzu seçin.

2.  **Genel bakış** sayfasında, araç çubuğundaki **Başlat** düğmesine tıklayın.

    :::image type="content" source="media/how-to-stop-start-server-portal/start-server.png" alt-text="Esnek sunucuyu başlatın.":::  

> [!NOTE]
> Sunucu başlatıldıktan sonra, tüm yönetim işlemleri artık esnek sunucu için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar
- [MySQL Için Azure veritabanı esnek sunucusu 'nda ağ](./concepts-networking.md) hakkında daha fazla bilgi edinin
- [Azure Portal kullanarak MySQL Için Azure veritabanı esnek sunucu sanal ağını oluşturun ve yönetin](./how-to-manage-virtual-network-portal.md).

