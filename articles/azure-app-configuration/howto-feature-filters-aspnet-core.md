---
title: Bir kullanıcı alt kümesi için bir özelliği etkinleştirmek için özellik filtrelerini kullanma
titleSuffix: Azure App Configuration
description: Bir kullanıcı alt kümesi için bir özelliği etkinleştirmek için özellik filtrelerini nasıl kullanacağınızı öğrenin
ms.service: azure-app-configuration
author: lisaguthrie
ms.author: lcozzens
ms.topic: conceptual
ms.date: 3/9/2020
ms.openlocfilehash: 181c97615985283011834dcf9145810b1563fb4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80057009"
---
# <a name="use-feature-filters-to-enable-a-feature-for-a-subset-of-users"></a>Bir kullanıcı alt kümesi için bir özelliği etkinleştirmek için özellik filtrelerini kullanma

Özellik bayrakları, uygulamanızdaki işlevselliği etkinleştirmenize veya devre dışı bırakmanıza olanak sağlar. Basit bir özellik bayrağı veya kapalıdır. Uygulama her zaman aynı şekilde olur. Örneğin, bir özellik bayrağının arkasına yeni bir özellik sunabilirsiniz. Özellik bayrağı etkinleştirildiğinde, tüm kullanıcılar yeni özelliği görür. Özellik bayrağını devre dışı bırakmak yeni özelliği gizler.

Buna karşılık, _koşullu özellik bayrağı_ özellik bayrağının etkinleştirilmesini veya dinamik olarak devre dışı bırakıldığını gösterir. Uygulama, özellik bayrağı ölçütleri bağlı olarak farklı davranabilir. Yeni özelliğinizi ilk başta küçük bir kullanıcı alt kümesine göstermek istediğinizi varsayalım. Koşullu özellik bayrağı, bazı kullanıcılar için özellik bayrağını etkinleştirmenizi sağlarken, diğerleri için devre dışı bırakmanızı sağlar. _Özellik filtreleri,_ her değerlendirildiğinde özellik bayrağının durumunu belirler.

Kitaplık `Microsoft.FeatureManagement` iki özellik filtresi içerir:

- `PercentageFilter`bir yüzdeye dayalı özellik bayrağını etkinleştirir.
- `TimeWindowFilter`belirli bir zaman penceresinde özellik bayrağını etkinleştirir.

[Microsoft.FeatureManagement.IFeatureFilter arabirimini](/dotnet/api/microsoft.featuremanagement.ifeaturefilter)uygulayan kendi özellik filtrenizi de oluşturabilirsiniz.

## <a name="registering-a-feature-filter"></a>Özellik filtresi kaydetme

`AddFeatureFilter` Yöntemi çağırarak, özellik filtresinin adını belirterek bir özellik filtresi kaydedersiniz. Örneğin, aşağıdaki kod `PercentageFilter`kaydeder:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();
    services.AddFeatureManagement().AddFeatureFilter<PercentageFilter>();
}
```

## <a name="configuring-a-feature-filter-in-azure-app-configuration"></a>Azure Uygulama Yapılandırmasında özellik filtresini yapılandırma

Bazı özellik filtrelerinin ek ayarları vardır. Örneğin, `PercentageFilter` bir özelliği yüzdeye göre etkinleştirir. Kullanılacak yüzdeyi tanımlayan bir ayarı vardır.

Bu ayarları Azure Uygulama Yapılandırması'nda tanımlanan özellik bayrakları için yapılandırabilirsiniz. Örneğin, bir web uygulamasındaki isteklerin %50'si için özellik bayrağını etkinleştirmek için aşağıdaki `PercentageFilter` adımları izleyin:

1. Quickstart'taki yönergeleri izleyin: Özellik bayrağı olan bir web uygulaması oluşturmak için [ASP.NET Core uygulamasına özellik bayrakları ekleyin.](./quickstart-feature-flag-aspnet-core.md)

1. Azure portalında yapılandırma mağazanıza gidin ve **Özellik Yöneticisi'ni**tıklatın.

1. Hızlı başlangıçta oluşturduğunuz *Beta* özellik bayrağı için bağlam menüsüne tıklayın. **Düzenle**’ye tıklayın.

    > [!div class="mx-imgBorder"]
    > ![Beta özellik bayrağını edin](./media/edit-beta-feature-flag.png)

1. **Edit** ekranında, zaten seçilmemişse **Radyo'da aç** düğmesini seçin. Ardından **Filtre Ekle** düğmesini tıklatın. (Radyo **On** düğmesinin etiketi **Koşullu**olarak okunacak şekilde değişecektir.)

1. **Anahtar** alanına *Microsoft.Percentage*girin.

    > [!div class="mx-imgBorder"]
    > ![Özellik filtresi ekleme](./media/feature-flag-add-filter.png)

1. Özellik filtresi anahtarının yanındaki bağlam menüsünü tıklatın. **Parametreleri Edit'i**tıklatın.

    > [!div class="mx-imgBorder"]
    > ![Özellik filtre parametrelerini edit](./media/feature-flag-edit-filter-parameters.png)

1. Metin kutularının ızgarada görünmesi için **Ad** üstbilgisinin altında gezin. Bir *Değer* **Adı** ve 50 **Değeri** girin. **Değer** alanı, özellik filtresini etkinleştirmek için istek yüzdesini gösterir.

    > [!div class="mx-imgBorder"]
    > ![Özellik filtresi parametrelerini ayarlama](./media/feature-flag-set-filter-parameters.png)

1. **Özellik bayrak** ekranına dönmek için **Uygula'yı** tıklatın. Ardından özellik bayrağı ayarlarını kaydetmek için yeniden **Uygula'yı** tıklatın.

1. Özellik bayrağının **durumu** artık *Koşullu*olarak görünür. Bu durum, özellik filtresi tarafından uygulanan ölçütlere bağlı olarak, özellik bayrağının istek başına etkinleştirileceğini veya devre dışı bırakıldığını gösterir.

    > [!div class="mx-imgBorder"]
    > ![Koşullu özellik bayrağı](./media/feature-flag-filter-enabled.png)

## <a name="feature-filters-in-action"></a>Özellik filtreleri iş başında

Bu özellik bayrağının etkilerini görmek için uygulamayı başlatın ve tarayıcınızdaki **Yenile** düğmesine birden çok kez basın. *Beta* öğesinin araç çubuğunda yaklaşık %50 oranında göründüğünü görürsünüz. Geri kalan zamanda gizlenir, çünkü `PercentageFilter` istekalt kümesi için *Beta* özelliği devre dışı bırakır. Aşağıdaki video, bu davranışı eylem olarak gösterir.

> [!div class="mx-imgBorder"]
> ![YüzdeFiltre iş başında](./media/feature-flags-percentagefilter.gif)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özellik yönetimine genel bakış](./concept-feature-management.md)
