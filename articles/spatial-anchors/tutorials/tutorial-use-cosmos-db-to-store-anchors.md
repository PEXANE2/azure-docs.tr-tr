---
title: 'Öğretici: Azure Cosmos DB bağlayıcı paylaşma'
description: Bu öğreticide, bir arka uç hizmetiyle ve Azure Cosmos DB Unity 'de Android/iOS cihazlarında Azure uzamsal bağlayıcı tanımlayıcılarını paylaşmayı öğreneceksiniz.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 07/31/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 8ff431c27dd53e82f9003b658c82ceb3efb5d320
ms.sourcegitcommit: 85eb6e79599a78573db2082fe6f3beee497ad316
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810029"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Öğretici: Azure uzamsal bağlayıcılarını Azure Cosmos DB arka ucu ile oturumlar ve cihazlar arasında paylaşma

Bu öğretici, [Azure uzamsal Tutturucuların oturumlar ve cihazlar arasında paylaşılmasının](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md) bir devamıdır. Azure Cosmos DB, oturum ve cihazlarda Azure uzamsal bağlayıcıları paylaşırken arka uç depolama alanı olarak sunmak için daha fazla özellik ekleme sürecinde size kılavuzluk eder.

![Nesne kalıcılığını gösteren GIF](./media/persistence.gif)

Bu öğreticide Unity ve Azure Cosmos DB kullanacağınızı, ancak bu öğreticide yalnızca uzamsal bağlayıcı tanımlayıcılarının nasıl paylaşılacağını gösteren bir örnek sunacağınızı unutmayın. Aynı amaca ulaşmak için diğer dillere ve arka uç teknolojilerine kullanıcı ekleyebilirsiniz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Daha önce oluşturduğunuz kaynak grubuna bir Azure Cosmos veritabanı ekleyin.

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

İhtiyaç duyacağınız için öğesini kopyalayın `Connection String` .

## <a name="make-minor-changes-to-the-sharingservice-files"></a>SharingService dosyalarında küçük değişiklikler yapın

**Çözüm Gezgini**' de, öğesini açın `SharingService\Startup.cs` .

`#define INMEMORY_DEMO`Dosyanın en üstünde bulun ve bu çizginin dışına yorum yapın. Dosyayı kaydedin.

**Çözüm Gezgini**' de, öğesini açın `SharingService\appsettings.json` .

Özelliğini bulun `StorageConnectionString` ve değeri `Connection String` [veritabanı hesabı oluştur adımında](#create-a-database-account)kopyaladığınız değerle aynı olacak şekilde ayarlayın. Dosyayı kaydedin.

Paylaşım hizmetini yeniden yayımlayabilir ve örnek uygulamayı çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazlarda bağlantı tanımlayıcılarını paylaşmak için Azure Cosmos DB kullandınız. Azure uzamsal çıpalarını yeni bir Unity HoloLens uygulamasında kullanma hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yeni bir Unity HoloLens uygulaması başlatma](./tutorial-new-unity-hololens-app.md)
