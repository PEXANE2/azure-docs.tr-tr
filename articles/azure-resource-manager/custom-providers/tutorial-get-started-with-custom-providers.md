---
title: Özel eylemler ve kaynaklar oluşturma
description: Bu öğretici, Azure Kaynak Yöneticisi'nde özel eylemlerin ve kaynakların nasıl oluşturulabildiğini inceler. Ayrıca, Azure Kaynak Yöneticisi Şablonları, Azure CLI, Azure İlkesi ve Azure Etkinlik Günlüğü ile özel iş akışlarının nasıl birlikte çalıştığını da gösterir.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 45f18727b53b802ba746da41b47fe955543ed9d0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75649900"
---
# <a name="create-custom-actions-and-resources-in-azure"></a>Azure'da özel eylemler ve kaynaklar oluşturun

Özel sağlayıcı, Azure ile bitiş noktası arasındaki bir sözleşmedir. Özel sağlayıcılarla, Azure Kaynak Yöneticisi'ne yeni API'ler ekleyerek Azure'daki iş akışlarını değiştirebilirsiniz. Bu özel API'lerle Kaynak Yöneticisi yeni dağıtım ve yönetim özelliklerini kullanabilir.

Bu öğretici, Azure'a yeni eylemler ve kaynakların nasıl eklendirilebildiğini ve bunları nasıl tümleştirileyeceğiniz hakkında basit bir örnekten geçer.

## <a name="set-up-azure-functions-for-azure-custom-providers"></a>Azure Özel Sağlayıcıları için Azure İşlevlerini Ayarlama

Bu öğreticinin birinci bölümünde, özel sağlayıcılarla çalışacak bir Azure işlev uygulamasının nasıl ayarlır:

- [Azure Özel Sağlayıcıları için Azure İşlevlerini Ayarlama](./tutorial-custom-providers-function-setup.md)

Özel sağlayıcılar herhangi bir genel URL ile çalışabilir.

## <a name="author-a-restful-endpoint-for-custom-providers"></a>Özel sağlayıcılar için yeniden bir bitiş noktası yazma

Bu öğreticinin ikinci bölümü, özel sağlayıcılar için yeniden bir bitiş noktasının nasıl yazılabildiğini açıklar:

- [Özel sağlayıcılar için yeniden bir bitiş noktası yazma](./tutorial-custom-providers-function-authoring.md)

## <a name="create-and-use-a-custom-provider"></a>Özel bir sağlayıcı oluşturma ve kullanma

Bu öğreticinin üçüncü bölümünde, özel bir sağlayıcı nın nasıl oluşturulup özel eylemlerini ve kaynaklarını nasıl kullanacağıaçıklanmaktadır:

- [Özel bir sağlayıcı oluşturma ve kullanma](./tutorial-custom-providers-create.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, özel sağlayıcılar ve nasıl bir oluşturmak için öğrendim. Bir sonraki öğreticiye devam etmek [için Bkz. Öğretici: Azure Özel Sağlayıcıları için Azure İşlevlerini ayarlayın.](./tutorial-custom-providers-function-setup.md)

Referanslar veya hızlı bir başlangıç arıyorsanız, bazı yararlı bağlantılar şunlardır:

- [Hızlı başlangıç: Azure özel kaynak sağlayıcısı oluşturun ve özel kaynakları dağıtın](./create-custom-provider.md)
- [Nasıl yapilir: Azure REST API'sine özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapilir: Azure REST API'sine özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
