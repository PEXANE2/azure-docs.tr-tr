---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a27d44a7684b4ad3d39d7fba50ca52e08e932971
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75769786"
---
Visual Studio'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlevleri, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için mantıksal birim olarak gruplamak üzere bir işlev uygulaması kullanabilirsiniz.

1. Visual Studio 'da, **Dosya** menüsünde **Yeni** > **projesi**' ni seçin.

1. **Yeni proje oluştur** iletişim kutusunda `functions`arayın, **Azure işlevleri** şablonunu seçin ve **İleri**' yi seçin.

1. Projeniz için bir ad girin ve **Oluştur**' u seçin. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

1. **Yeni bir Azure işlevleri uygulaması oluştur**bölümünde aşağıdaki seçenekleri kullanın:

    + **Azure Işlevleri v2 (.NET Core)**
    + **HTTP tetikleyicisi**
    + **Depolama hesabı**: **depolama öykünücüsü**
    + **Yetkilendirme düzeyi**: **anonim** 

    | Seçenek      | Önerilen değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **İşlevler çalışma zamanı** | **Azure Işlevleri 2. x <br />(.NET Core)** | Bu ayar, .NET Core 'u destekleyen Azure Işlevlerinin sürüm 2. x çalışma zamanını kullanan bir işlev projesi oluşturur. Azure İşlevleri 1.x, .NET Framework’ü destekler. Daha fazla bilgi için bkz. [hedef Azure işlevleri çalışma zamanı sürümü](../articles/azure-functions/functions-versions.md).   |
    | **İşlev şablonu** | **HTTP tetikleyicisi** | Bu ayar, bir HTTP isteği tarafından tetiklenen bir işlev oluşturur. |
    | **Depolama Hesabı**  | **Depolama öykünücüsü** | HTTP tetikleyicisi, Azure depolama hesabı bağlantısını kullanmaz. Diğer tüm tetikleyici türleri için geçerli bir Depolama hesabı bağlantı dizesi gerekir. Işlevler bir depolama hesabı gerektirdiğinden, projenizi Azure 'da yayımladığınızda bir tane atanır veya oluşturulur. |
    | **Yetkilendirme düzeyi** | **Deðeri** | Oluşturulan işlev, anahtar gerektirmeden herhangi bir istemci tarafından tetiklenebilir. Bu yetkilendirme ayarı yeni işlevinizi test etmenizi kolaylaştırır. Anahtarlar ve yetkilendirme hakkında daha fazla bilgi için, [HTTP ve web kancası bağlamaları](../articles/azure-functions/functions-bindings-http-webhook.md) altında [Yetkilendirme anahtarları](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) bölümüne bakın. |
    
    > [!NOTE]
    > **Yetkilendirme düzeyini** `Anonymous`olarak ayarladığınızdan emin olun. Varsayılan `Function`düzeyini seçerseniz, işlev uç noktanıza erişmek için isteklerde [işlev anahtarı](../articles/azure-functions/functions-bindings-http-webhook.md#authorization-keys) sunmak gerekir.
    
4. İşlev projesi ve HTTP ile tetiklenen işlev oluşturmak için **Oluştur** ' u seçin.
