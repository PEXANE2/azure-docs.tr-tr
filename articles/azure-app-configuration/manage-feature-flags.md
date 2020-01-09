---
title: "Öğretici: Özellik bayraklarını yönetmek için Azure Uygulama yapılandırması 'Nı kullanma"
titleSuffix: Azure App Configuration
description: Bu öğreticide, Azure Uygulama yapılandırması 'nı kullanarak özellik bayraklarını uygulamanızdan ayrı olarak yönetmeyi öğreneceksiniz.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 620cd6e1712a89e13bf876310c0d3248c4a4d1e7
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75414003"
---
# <a name="tutorial-manage-feature-flags-in-azure-app-configuration"></a>Öğretici: Azure Uygulama yapılandırmasındaki Özellik bayraklarını yönetme

Tüm özellik bayraklarını Azure Uygulama Yapılandırması ' nda depolayıp tek bir yerden yönetebilirsiniz. Uygulama yapılandırması, özellikle özellik bayrakları için tasarlanan **Özellik Yöneticisi** adlı bir Portal Kullanıcı arabirimine sahiptir. Uygulama yapılandırması ayrıca .NET Core Özellik bayrağı veri şemasını da yerel olarak destekler.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Uygulama yapılandırmasında Özellik bayraklarını tanımlayın ve yönetin.
> * Uygulamanızdan Özellik bayraklarını erişin.

## <a name="create-feature-flags"></a>Özellik bayrakları oluşturma

Uygulama yapılandırması için Azure portal Özellik Yöneticisi, uygulamalarınızda kullandığınız özellik bayraklarını oluşturmak ve yönetmek için bir kullanıcı arabirimi sağlar.

Yeni bir özellik bayrağı eklemek için:

1. Özellik **yöneticisi** >  **+ Ekle** ' yi seçerek Özellik bayrağı ekleyin.

    ![Özellik bayrağı listesi](./media/azure-app-configuration-feature-flags.png)

1. Özellik bayrağı için benzersiz bir anahtar adı girin. Kodunuzda bayrak başvurusunda bulunmak için bu ada ihtiyacınız vardır.

1. İsterseniz, özellik bayrağına bir açıklama verin.

1. Özellik bayrağının başlangıç durumunu ayarlayın. Bu durum genellikle *kapalı* veya *Açık*durumdadır. Özellik bayrağına bir filtre eklerseniz, *on* durumu *koşullu* olarak değişir.

    ![Özellik bayrağı oluşturma](./media/azure-app-configuration-feature-flag-create.png)

1. Durum *Açık*olduğunda, durumu nitelemek üzere ek koşullar belirtmek için **+ Filtre Ekle** ' yi seçin. Yerleşik veya özel bir filtre anahtarı girin ve ardından filtre ile bir veya daha fazla parametreyi ilişkilendirmek için **+ parametre Ekle** ' yi seçin. Yerleşik Filtreler şunları içerir:

    | Anahtar | JSON parametreleri |
    |---|---|
    | Microsoft. Percentage | {"Value":% 0-100} |
    | Microsoft. TimeWindow | {"Başlangıç": UTC saati, "bitiş": UTC saati} |

    ![Özellik bayrağı filtresi](./media/azure-app-configuration-feature-flag-filter.png)

## <a name="update-feature-flag-states"></a>Özellik bayrağı durumlarını Güncelleştir

Bir özellik bayrağının durum değerini değiştirmek için:

1. **Özellik Yöneticisi**' ni seçin.

1. Değiştirmek istediğiniz bir özellik bayrağının sağında, üç nokta ( **...** ) simgesini seçin ve ardından **Düzenle**' yi seçin.

1. Özellik bayrağı için yeni bir durum belirleyin.

## <a name="access-feature-flags"></a>Özellik bayraklarını erişim

Özellik Yöneticisi tarafından oluşturulan özellik bayrakları, normal anahtar değerleri olarak depolanır ve alınır. `.appconfig.featureflag`özel bir ad alanı ön eki altında tutulur. Temel alınan anahtar değerlerini görüntülemek için yapılandırma Gezgini ' ni kullanın. Uygulamanız, uygulama yapılandırma yapılandırma sağlayıcılarını, SDK 'Ları, komut satırı uzantılarını ve REST API 'Leri kullanarak bu değerleri alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, uygulama yapılandırması ' nı kullanarak özellik bayraklarını ve durumlarını yönetmeyi öğrendiniz. Uygulama yapılandırması ve ASP.NET Core özellik yönetimi desteği hakkında daha fazla bilgi için aşağıdaki makaleye bakın:

* [ASP.NET Core uygulamasında Özellik bayraklarını kullanma](./use-feature-flags-dotnet-core.md)
