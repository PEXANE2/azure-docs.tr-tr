---
title: Uygulamaları en son şemaya geçirin
description: Mantık uygulaması iş akışı JSON tanımlarını en son İş Akışı Tanımı Dili şeması sürümüne nasıl geçirilir?
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: cef0fcb990cd2c5c6583822d4dc4c6993c52eac2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75666797"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Mantık uygulamalarını en son şema sürümüne geçirin

Varolan mantık uygulamalarınızı en yeni şemaya taşımak için aşağıdaki adımları izleyin: 

1. Azure [portalında](https://portal.azure.com)mantık uygulamanızı Logic App Designer'da açın.

2. Mantık uygulamanızın menüsünde **Genel Bakış'ı**seçin. Araç çubuğunda **Schema'yı Güncelleştir'i**seçin.

   > [!NOTE]
   > **Schema'yı Güncelleştir'i**seçtiğinizde, Azure Logic Apps geçiş adımlarını otomatik olarak çalıştırıyor ve kod çıktısını sizin için sağlar. Bu çıktıyı mantık uygulama tanımınızı güncelleştirmek için kullanabilirsiniz. Ancak, aşağıdaki **En İyi Uygulamalar** bölümünde açıklandığı gibi en iyi uygulamaları izlediğinden emin olun.

   ![Şema'yı Güncelleştir](./media/connectors-schema-migration/update-schema.png)

   Schema'yı Güncelleştir sayfası görüntülenir ve yeni şemadaki gelişmeleri açıklayan bir belgenin bağlantısını gösterir.

## <a name="best-practices"></a>En iyi uygulamalar

Mantık uygulamalarınızı en son şema sürümüne geçirmek için en iyi uygulamalar şunlardır:

* Geçirilen komut dosyasını yeni bir mantık uygulamasına kopyalayın. Testinizi tamamlayana ve geçirilen uygulamanızın beklendiği gibi çalıştığını onaylayana kadar eski sürümün üzerine yazmayın.

* Üretime başlamadan **önce** mantık uygulamanızı test edin.

* Geçişi tamamladıktan sonra, mümkün olduğunca [yönetilen API'leri](../connectors/apis-list.md) kullanmak için mantık uygulamalarınızı güncellemeye başlayın. Örneğin, DropBox v1 kullandığınız her yerde Dropbox v2 kullanmaya başlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Mantık uygulamalarınızı el ile nasıl geçirebilirsiniz](../logic-apps/logic-apps-schema-2015-08-01.md) öğrenin
