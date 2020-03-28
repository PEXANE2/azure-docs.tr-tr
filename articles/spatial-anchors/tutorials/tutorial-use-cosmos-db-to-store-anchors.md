---
title: 'Öğretici: Sunucuları Azure Cosmos DB ile paylaşın'
description: Bu eğitimde, Bir arka uç hizmeti ve Azure Cosmos DB ile Birlikte Android/iOS aygıtlarında Azure Uzamsal Bağlantı Tanımlayıcıları'nı nasıl paylaşabileceğinizi öğrenirsiniz.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 71b3027d86400d6921895f86e257ddff2961f91f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77615147"
---
# <a name="tutorial-sharing-azure-spatial-anchors-across-sessions-and-devices-with-an-azure-cosmos-db-back-end"></a>Öğretici: Azure Uzamsal Bağlantılarını oturumlar ve aygıtlar arasında Azure Cosmos DB arka uçla paylaşma

Bu öğretici, [Oturumlar ve aygıtlar arasında Azure Uzamsal Bağlantılarını paylaşmanın](../../../articles/spatial-anchors/tutorials/tutorial-share-anchors-across-devices.md) bir devamıdır. Azure Cosmos DB'nin oturumlar ve aygıtlar arasında azure uzamsal bağlantılarını paylaşırken arka uç depolama alanı olarak hizmet etmesini sağlamak için birkaç özellik daha ekleme sürecinde size yol gösterecektir.

![Nesne kalıcılığını gösteren GIF](./media/persistence.gif)

Bu eğitimde Unity ve Azure Cosmos DB'yi kullanıyor olsanız da, bu yalnızca aygıtlar arasında Uzamsal Çapa tanımlayıcılarını nasıl paylaşacağınız hakkında bir örnek vermek için olduğunu belirtmekte fayda var. Aynı amaca ulaşmak için diğer dilleri ve arka uç teknolojilerini kullanabilirsiniz. Ayrıca, bu eğitimde kullanılan ASP.NET Core web uygulaması .NET Core 2.2 SDK gerektirir. Windows için Web Apps'ta iyi çalışır, ancak şu anda Linux için Web Apps'ta çalışmaz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Daha önce oluşturduğunuz kaynak grubuna bir Azure Cosmos Veritabanı ekleyin.

[!INCLUDE [cosmos-db-create-dbaccount-table](../../../includes/cosmos-db-create-dbaccount-table.md)]

İhtiyacınız `Connection String` olacak diye kopyalayın.

## <a name="make-minor-changes-to-the-sharingservice-files"></a>PaylaşımHizmeti dosyalarında küçük değişiklikler yapma

**Çözüm Gezgini'nde**, açık `SharingService\Startup.cs`.

Dosyanın üst kısmında bulun `#define INMEMORY_DEMO` ve bu satırı dışarı yorum. Dosyayı kaydedin.

**Çözüm Gezgini'nde**, açık `SharingService\appsettings.json`.

Özelliği bulun ve değeri [veritabanı hesabı adımında](#create-a-database-account)kopyaladığınız `Connection String` değerle aynı olacak şekilde ayarlayın. `StorageConnectionString` Dosyayı kaydedin.

Paylaşım Hizmetini yeniden yayımlayabilir ve örnek uygulamayı çalıştırabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="unity-20193"></a>Birlik 2019.3

Kırılan değişiklikler nedeniyle, Unity 2019.3 şu anda desteklenmiyor. Lütfen Unity 2019.1 veya 2019.2'yi kullanın.

[!INCLUDE [Clean-up section](../../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bağlantı tanımlayıcılarını aygıtlar arasında paylaşmak için Azure Cosmos DB'yi kullandınız. Yeni unity HoloLens uygulamasında Azure Uzamsal Çapaları nasıl kullanacağınız hakkında daha fazla bilgi edinmek için bir sonraki eğitime devam edin.

> [!div class="nextstepaction"]
> [Yeni Unity HoloLens uygulaması başlıyor](./tutorial-new-unity-hololens-app.md)
