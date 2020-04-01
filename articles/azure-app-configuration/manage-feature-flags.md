---
title: 'Öğretici: Özellik bayraklarını yönetmek için Azure Uygulama Yapılandırmasını kullanın'
titleSuffix: Azure App Configuration
description: Bu eğitimde, Azure Uygulama Yapılandırması'nı kullanarak özellik bayraklarını uygulamanızdan ayrı olarak nasıl yöneteceklerinizi öğrenirsiniz.
services: azure-app-configuration
documentationcenter: ''
author: lisaguthrie
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: lcozzens
ms.custom: mvc
ms.openlocfilehash: ccab8014000f9f684249bf2c1f800f74c92e7369
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76899374"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Öğretici: Azure Uygulama Yapılandırmasında özellik bayraklarını yönetme

Tüm özellik bayraklarını Azure Uygulama Yapılandırmasında saklayabilir ve tek bir yerden yönetebilirsiniz. Uygulama Yapılandırması özellik bayrakları için özel olarak tasarlanmış **Özellik Yöneticisi** adlı bir portal UI'ye sahiptir. Uygulama Yapılandırması da yerel olarak .NET Core özellik bayrak lı veri şemasını destekler.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Uygulama Yapılandırması'nda özellik bayraklarını tanımlayın ve yönetin.
> * Uygulamanızdan özellik bayraklarına erişin.

## <a name="create-feature-flags"></a>Özellik bayrakları oluşturma

Uygulama Yapılandırması için Azure portalındaki Özellik Yöneticisi, uygulamalarınızda kullandığınız özellik bayraklarını oluşturmak ve yönetmek için bir kullanıcı arabirimi sağlar.

Yeni bir özellik bayrağı eklemek için:

1. Özellik bayrağı eklemek için **Özellik Yöneticisi** > **+Ekle'yi** seçin.

    ![Özellik bayrak listesi](./media/azure-app-configuration-feature-flags.png)

1. Özellik bayrağı için benzersiz bir anahtar adı girin. Kodunuzdaki bayrağa başvurmak için bu ada ihtiyacınız vardır.

1. İstersen, özellik bayrağına bir açıklama verin.

1. Özellik bayrağının ilk durumunu ayarlayın. Bu durum genellikle *Kapalı* veya *Açıktır.* Özellik bayrağına bir filtre *eklerseniz, On* durumu *Koşullu* olarak değişir.

    ![Özellik bayrağı oluşturma](./media/azure-app-configuration-feature-flag-create.png)

1. Durum *Açık*olduğunda , durumu nitelemek için ek koşullar belirtmek için **+Ekle filtresi'ni** seçin. Yerleşik veya özel filtre anahtarı girin ve ardından filtreyle bir veya daha fazla parametreyi ilişkilendirmek için **+Ekle parametresini** seçin. Yerleşik filtreler şunlardır:

    | Anahtar | JSON parametreleri |
    |---|---|
    | Microsoft.Percentage | {"Değer": Yüzde 0-100} |
    | Microsoft.TimeWindow | {"Başlat": UTC saati, "End": UTC zamanlı} |

    ![Özellik bayrağı filtresi](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Özellik bayrak durumlarını güncelleştirme

Özellik bayrağının durum değerini değiştirmek için:

1. **Özellik Yöneticisi'ni**seçin.

1. Değiştirmek istediğiniz özellik bayrağının sağında, elipsis **(...**) seçeneğini seçin ve ardından **Edit'i**seçin.

1. Özellik bayrağı için yeni bir durum ayarlayın.

## <a name="access-feature-flags"></a>Erişim özelliği bayrakları

Özellik Yöneticisi tarafından oluşturulan özellik bayrakları depolanır ve normal anahtar değerleri olarak alınır. Özel bir ad alanı öneki `.appconfig.featureflag`altında tutulurlar. Temel anahtar değerlerini görüntülemek için Configuration Explorer'ı kullanın. Uygulamanız, Uygulama Yapılandırma yapılandırma sağlayıcıları, SDK'lar, komut satırı uzantıları ve REST API'lerini kullanarak bu değerleri alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, App Configuration'ı kullanarak özellik bayraklarını ve durumlarını nasıl yöneteceklerini öğrendiniz. App Configuration ve ASP.NET Core'da özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

* [ASP.NET Core uygulamasında özellik bayraklarını kullanma](./use-feature-flags-dotnet-core.md)
