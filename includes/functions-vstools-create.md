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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80657593"
---
Visual Studio 'daki Azure Işlevleri proje şablonu, Azure 'da bir işlev uygulamasına yayımlayabilecek bir proje oluşturur. İşlevleri, kaynakların daha kolay yönetilmesi, dağıtılması, ölçeklendirilmesi ve paylaşılması için mantıksal birim olarak gruplamak üzere bir işlev uygulaması kullanabilirsiniz.

1. Visual Studio 'da, **Dosya** menüsünde **Yeni** > **Proje**' yi seçin.

1. **Yeni proje oluştur**' da, arama kutusuna *işlevler* girin ve ardından **Azure işlevleri** şablonunu seçin.

1. **Yeni projenizi yapılandırın**bölümünde projeniz Için bir **Proje adı** girin ve ardından **Oluştur**' u seçin. İşlev uygulamasının adı, bir C# ad alanı olarak geçerli olmalıdır; bu nedenle alt çizgi, kısa çizgi veya alfasayısal olmayan herhangi bir karakter kullanmayın.

1. **Yeni proje- &lt;&gt; proje adı** ayarları için aşağıdaki tablodaki değerleri kullanın:

    | Ayar      | Değer  | Açıklama                      |
    | ------------ |  ------- |----------------------------------------- |
    | **İşlevler çalışma zamanı** | **Azure Işlevleri v2 <br />(.NET Core)** | Bu değer, .NET Core 'u destekleyen Azure Işlevlerinin sürüm 2. x çalışma zamanını kullanan bir işlev projesi oluşturur. Azure İşlevleri 1.x, .NET Framework’ü destekler. Daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümlerine genel bakış](../articles/azure-functions/functions-versions.md).   |
    | **İşlev şablonu** | **HTTP tetikleyicisi** | Bu değer, bir HTTP isteği tarafından tetiklenen bir işlev oluşturur. |
    | **Depolama hesabı**  | **Depolama Öykünücüsü** | Azure Işlevi için bir depolama hesabı gerektiğinden, projenizi Azure 'da yayımladığınızda bir tane atanır veya oluşturulur. HTTP tetikleyicisi, Azure depolama hesabı bağlantı dizesi kullanmaz; diğer tüm tetikleyici türleri için geçerli bir Azure depolama hesabı bağlantı dizesi gerekir.  |
    | **Erişim hakları** | **Anonim** | Oluşturulan işlev, anahtar gerektirmeden herhangi bir istemci tarafından tetiklenebilir. Bu yetkilendirme ayarı yeni işlevinizi test etmenizi kolaylaştırır. Anahtarlar ve yetkilendirme hakkında daha fazla bilgi için bkz. [Yetkilendirme anahtarları](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) ve [http ve Web kancası bağlamaları](../articles/azure-functions/functions-bindings-http-webhook.md). |
    

    
    ![Azure Işlevleri proje ayarları](./media/functions-vs-tools-create/functions-project-settings.png)

    **Erişim haklarını** **anonim**olarak ayarladığınızdan emin olun. Varsayılan **işlev**düzeyini seçerseniz, işlev uç noktanıza erişmek için isteklerde [işlev anahtarı](../articles/azure-functions/functions-bindings-http-webhook-trigger.md#authorization-keys) sunmak gerekir.

1. İşlev projesi ve HTTP ile tetiklenen işlev oluşturmak için **Tamam** ' ı seçin.
