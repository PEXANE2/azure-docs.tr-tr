---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 034e966d259f1ca5f22eec5935013de32c883b59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80056631"
---
Visual Studio'daki Azure İşlevler proje şablonu, Azure'daki bir işlev uygulamasında yayınlayabileceğiniz bir proje oluşturur. Daha kolay yönetim, dağıtım, ölçekleme ve kaynakların paylaşımı için işlevleri mantıksal bir birim olarak gruplandırmak için bir işlev uygulaması kullanabilirsiniz.

1. Visual Studio'da Dosya **menüsünde** **Yeni** > **Proje'yi**seçin.

1. **Yeni bir proje oluştur'da,** arama kutusuna *işlevleri* girin ve ardından **Azure İşlevler** şablonunu seçin.

1. **Yeni projenizi Yapılandır'da,** projeniz için bir **Proje adı** girin ve ardından **Oluştur'u**seçin. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

1. Proje **adı &lt;&gt; ayarlarınız olan Yeni Proje** için aşağıdaki tablodaki değerleri kullanın:

    | Ayar      | Değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **İşlevler çalışma zamanı** | **Azure Fonksiyonları <br />v2 (.NET Core)** | Bu değer, .NET Core'u destekleyen Azure İşlevlerinin 2.x çalışma saatini kullanan bir işlev projesi oluşturur. Azure İşlevleri 1.x, .NET Framework’ü destekler. Daha fazla bilgi için Azure [İşlevleri çalışma zamanı sürümlerine genel bakış](../articles/azure-functions/functions-versions.md)bakın.   |
    | **İşlev şablonu** | **HTTP tetikleyicisi** | Bu değer, bir HTTP isteği tarafından tetiklenen bir işlev oluşturur. |
    | **Depolama Hesabı**  | **Depolama Öykünücüsü** | Azure İşlevi bir depolama hesabı gerektirdiğinden, projenizi Azure'da yayımladığınızda bir tane atanır veya oluşturulur. BIR HTTP tetikleyicisi Azure Depolama hesabı bağlantı dizesini kullanmaz; diğer tüm tetikleyici türleri geçerli bir Azure Depolama hesabı bağlantı dizesi gerektirir.  |
    | **Erişim hakları** | **Anonim** | Oluşturulan işlev, anahtar gerektirmeden herhangi bir istemci tarafından tetiklenebilir. Bu yetkilendirme ayarı yeni işlevinizi test etmenizi kolaylaştırır. Anahtarlar ve yetkilendirme hakkında daha fazla bilgi için [Yetkilendirme tuşlarına](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) ve [HTTP ve webhook bağlamalarına](../articles/azure-functions/functions-bindings-http-webhook.md)bakın. |
    

    
    ![Azure Fonksiyonları proje ayarları](./media/functions-vs-tools-create/functions-project-settings.png)

    **Erişim haklarını** **Anonymous**olarak ayarladığınızdan emin olun. **Varsayılan Işlev**düzeyini seçerseniz, işlev bitiş noktanıza erişmek için isteklerde [işlev anahtarını](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) sunmanız gerekir.

1. İşlev projesini ve HTTP tetiklenen işlevi oluşturmak için **Tamam'ı** seçin.
