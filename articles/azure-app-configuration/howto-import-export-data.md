---
title: Azure Uygulama yapılandırması ile verileri içeri aktarma veya dışarı aktarma
description: Yapılandırma verilerini içeri veya Azure Uygulama yapılandırmasına aktarma hakkında bilgi edinin. Uygulama yapılandırma depem ve kod projeniz arasında veri alışverişi yapın.
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: lcozzens
ms.openlocfilehash: 21eba653bcd853db9550d0d3781aacd281884605
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88588084"
---
# <a name="import-or-export-configuration-data"></a>Yapılandırma verilerini içeri veya dışarı aktarma

Azure Uygulama yapılandırması, veri içeri ve dışarı aktarma işlemlerini destekler. Uygulama yapılandırma depem ve kod projeniz arasında toplu ve Exchange verilerinde yapılandırma verileriyle çalışmak için bu işlemleri kullanın. Örneğin, test için bir uygulama yapılandırma deposu ve bir üretim için bir tane oluşturabilirsiniz. Verileri iki kez girmeniz gerekmiyorsa, bunlar arasında uygulama ayarlarını kopyalayabilirsiniz.

Bu makale, uygulama yapılandırmasıyla veri içeri ve dışarı aktarmaya yönelik bir kılavuz sağlar. GitHub deponuz ile devam eden bir eşitleme kurmak istiyorsanız [GitHub Eylemimize](https://aka.ms/azconfig-gha1)göz atın.

## <a name="import-data"></a>Veri içeri aktarma

İçeri aktarma, yapılandırma verilerini mevcut bir kaynaktan bir uygulama yapılandırma deposuna getirir. İçeri aktarma işlevini kullanarak bir uygulama yapılandırma deposuna veri geçirin veya birden çok kaynaktan veri toplayın. Uygulama yapılandırması, JSON, YAML veya özellikler dosyasından içeri aktarmayı destekler.

[Azure Portal](https://portal.azure.com) ya da [Azure CLI](./scripts/cli-import.md)kullanarak verileri içeri aktarın. Azure portal, aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deponuza gidin ve **işlemler** menüsünden **Içeri/dışarı aktar** ' ı seçin.

1. **İçeri aktar** sekmesinde **kaynak hizmeti**  >  **yapılandırma dosyası**' nı seçin.

1. **Dil için** öğesini seçin ve istediğiniz giriş türünü seçin.

1. **Klasör** simgesini seçin ve içeri aktarılacak dosyaya gidin.

    ![Dosyayı içeri aktar](./media/import-file.png)

1. Bir **ayırıcı**seçin ve isteğe bağlı olarak içeri aktarılan anahtar adları için kullanılacak bir **ön ek** girin.

1. İsteğe bağlı olarak bir **etiket**seçin.

1. İçeri aktarma işleminin bitmesini sağlamak için **Uygula** ' yı seçin.

    ![Dosya içeri aktarma tamamlandı](./media/import-file-complete.png)

## <a name="export-data"></a>Verileri dışarı aktarma

Uygulama yapılandırmasında depolanan yazma yapılandırması verilerini başka bir hedefe dışarı aktarın. Uygulama yapılandırma deposundaki verileri dağıtım sırasında uygulama kodunuzla birlikte gömülü bir dosyaya kaydetmek için dışarı aktarma işlevini kullanın.

[Azure Portal](https://portal.azure.com) veya [Azure CLI](./scripts/cli-export.md)kullanarak verileri dışarı aktarın. Azure portal, aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deponuza gidin ve **içeri/dışarı aktar**' ı seçin.

1. **Dışarı aktar** sekmesinde **hedef hizmet**  >  **yapılandırma dosyası**' nı seçin.

1. İsteğe bağlı olarak bir **ön ek** girin ve anahtarlar Için bir **etiket** ve zaman içinde bir nokta seçin.

1. Bir **dosya türü**  >  **ayırıcısı**seçin.

1. Dışarı aktarmayı sona bırakmak için **Uygula** ' yı seçin.

    ![Dışarı aktarma dosyası tamamlandı](./media/export-file-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
