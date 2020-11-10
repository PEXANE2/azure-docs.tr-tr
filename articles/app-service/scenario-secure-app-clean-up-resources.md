---
title: Öğretici-Kaynakları Temizleme | Mavisi
description: Bu öğreticide, Web uygulamasını oluştururken ayrılan Azure kaynaklarını temizlemeyi öğreneceksiniz.
services: storage, app-service-web
author: rwike77
manager: CelesteDG
ms.service: app-service-web
ms.topic: tutorial
ms.workload: identity
ms.date: 10/27/2020
ms.author: ryanwi
ms.reviewer: stsoneff
ms.openlocfilehash: ab91ea7aa6e621dabc5cac83fe818dbf175214b6
ms.sourcegitcommit: 0dcafc8436a0fe3ba12cb82384d6b69c9a6b9536
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94428967"
---
# <a name="tutorial-clean-up-resources"></a>Öğretici: Kaynakları Temizleme

Bu çok parçalı öğreticideki tüm adımları tamamladıysanız, bir kaynak grubunda bir App Service, App Service barındırma planı ve bir depolama hesabı oluşturdunuz.  Ayrıca, Azure AD 'de bir uygulama kaydı oluşturmuş olursunuz.  Artık gerekli değilse, ücretleri tahakkuk ettirmek için bu kaynakları ve uygulama kaydını silin.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Öğreticiyi izleyerek oluşturulan Azure kaynaklarını silme

## <a name="delete-the-resource-group"></a>Kaynak grubunu silme
[Azure Portal](https://portal.azure.com), Portal menüsünden **kaynak grupları** ' nı seçin ve App Service ve App Service planınızı içeren kaynak grubunu seçin.

Kaynak grubunu ve tüm kaynakları silmek için **kaynak grubunu sil** ' i seçin.

:::image type="content" alt-text="Kaynak grubunu silme" source="./media/scenario-secure-app-clean-up-resources/delete-resource-group.png":::

Bu komutun çalışması birkaç dakika sürebilir.

## <a name="delete-the-app-registration"></a>Uygulama kaydını silme
Portal menüsünde **Azure Active Directory** ' yi seçin ve ardından **uygulama kayıtları** , sonra oluşturduğunuz uygulamayı seçin.
:::image type="content" alt-text="Uygulama kaydı seçin" source="./media/scenario-secure-app-clean-up-resources/select-app-registration.png":::

Uygulama kaydına genel bakış ' da **Sil** ' i seçin.
:::image type="content" alt-text="Uygulama kaydını sil" source="./media/scenario-secure-app-clean-up-resources/delete-app-registration.png":::

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
>
> * Öğreticiyi izleyerek oluşturulan Azure kaynaklarını silme

Bir [.NET Core uygulamasını](tutorial-dotnetcore-sqldb-app.md), [Python uygulamasını](tutorial-python-postgresql-app.md), [Java uygulamasını](tutorial-java-spring-cosmosdb.md)veya [Node.js uygulamasını](tutorial-nodejs-mongodb-app.md) bir veritabanına bağlamayı öğrenin.