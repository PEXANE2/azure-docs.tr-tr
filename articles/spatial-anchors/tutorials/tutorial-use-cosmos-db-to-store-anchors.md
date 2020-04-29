---
title: 'Öğretici: Azure Cosmos DB bağlayıcı paylaşma'
description: Bu öğreticide, bir arka uç hizmetiyle ve Azure Cosmos DB Unity 'de Android/iOS cihazlarında Azure uzamsal bağlayıcı tanımlayıcılarını paylaşmayı öğreneceksiniz.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 71b3027d86400d6921895f86e257ddff2961f91f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77615147"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Öğretici: Azure uzamsal bağlayıcılarını Azure Cosmos DB arka ucu ile oturumlar ve cihazlar arasında paylaşma

Bu öğretici, [Azure uzamsal Tutturucuların oturumlar ve cihazlar arasında paylaşılmasının](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md) bir devamıdır. Azure Cosmos DB, oturum ve cihazlarda Azure uzamsal bağlayıcıları paylaşırken arka uç depolama alanı olarak sunmak için daha fazla özellik ekleme sürecinde size kılavuzluk eder.

![Nesne kalıcılığını gösteren GIF](./media/persistence.gif)

Bu öğreticide Unity ve Azure Cosmos DB kullanacağınızı, ancak bu öğreticide yalnızca uzamsal bağlayıcı tanımlayıcılarının nasıl paylaşılacağını gösteren bir örnek sunacağınızı unutmayın. Aynı amaca ulaşmak için diğer dillere ve arka uç teknolojilerine kullanıcı ekleyebilirsiniz. Ayrıca, bu öğreticide kullanılan ASP.NET Core Web uygulaması .NET Core 2,2 SDK 'sını gerektirir. Windows için Web Apps üzerinde ince çalışır, ancak şu anda Linux için Web Apps üzerinde çalışmaz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Daha önce oluşturduğunuz kaynak grubuna bir Azure Cosmos veritabanı ekleyin.

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

İhtiyaç duyacağınız `Connection String` için öğesini kopyalayın.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>SharingService dosyalarında küçük değişiklikler yapın

**Çözüm Gezgini**' de, `SharingService\Startup.cs`öğesini açın.

Dosyanın `#define INMEMORY_DEMO` en üstünde bulun ve bu çizginin dışına yorum yapın. Dosyayı kaydedin.

**Çözüm Gezgini**' de, `SharingService\appsettings.json`öğesini açın.

`StorageConnectionString` Özelliğini bulun ve değeri [veritabanı hesabı oluştur adımında](#create-a-database-account)kopyaladığınız `Connection String` değerle aynı olacak şekilde ayarlayın. Dosyayı kaydedin.

Paylaşım hizmetini yeniden yayımlayabilir ve örnek uygulamayı çalıştırabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unity-20193"></a>Unity 2019,3

Büyük değişiklikler nedeniyle Unity 2019,3 Şu anda desteklenmiyor. Lütfen Unity 2019,1 veya 2019,2 kullanın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, cihazlarda bağlantı tanımlayıcılarını paylaşmak için Azure Cosmos DB kullandınız. Azure uzamsal çıpalarını yeni bir Unity HoloLens uygulamasında kullanma hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Yeni bir Unity HoloLens uygulaması başlatılıyor](./tutorial-new-unity-hololens-app.md)
