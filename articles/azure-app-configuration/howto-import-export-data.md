---
title: Azure Uygulama Yapılandırması ile veri alma veya verme
description: Azure Uygulama Yapılandırmasına veya Azure Uygulama Yapılandırmasından veri alma veya verme konusunda öğrenin
services: azure-app-configuration
author: lisaguthrie
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/25/2020
ms.author: lcozzens
ms.openlocfilehash: 5b3d6d0561d7d6d6b23cb4f579b0988850da9771
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80056875"
---
# <a name="import-or-export-configuration-data"></a>Yapılandırma verilerini içeri veya dışarı aktarma

Azure Uygulama Yapılandırması veri alma ve dışa aktarma işlemlerini destekler. Bu işlemleri, yapılandırma verileriyle toplu olarak çalışmak ve Uygulama Yapılandırma mağazanız ile kod projeniz arasında veri alışverişinde bulunmak için kullanın. Örneğin, test için bir Uygulama Yapılandırma deposu ve üretim için başka bir mağaza ayarlayabilirsiniz. İki kez veri girmek zorunda kalmamak için uygulama ayarlarını aralarında kopyalayabilirsiniz.

Bu makalede, Uygulama Yapılandırması ile veri alma ve dışa aktarma için bir kılavuz sağlar. GitHub repo'nuzla devam eden bir eşitleme ayarlamak istiyorsanız, [GitHub Eylemimize](https://aka.ms/azconfig-gha1)bir göz atın.

## <a name="import-data"></a>Veri içeri aktarma

Alma, yapılandırma verilerini varolan bir kaynaktan bir Uygulama Yapılandırma deposuna getirir. Verileri bir Uygulama Yapılandırma deposuna geçirmek veya birden çok kaynaktan veri toplamak için alma işlevini kullanın. Uygulama Yapılandırması, JSON, YAML veya özellikler dosyasından alma yı destekler.

[Azure portalını](https://portal.azure.com) veya [Azure CLI'yi](./scripts/cli-import.md)kullanarak veri alma. Azure portalından aşağıdaki adımları izleyin:

1. Uygulama Yapılandırma mağazanıza göz atın ve **İşlemler** menüsünden **Dışa Aktar/Dışa Aktar'ı** seçin.

1. **İçe Aktar** sekmesinde **Kaynak hizmet** > **Yapılandırma Dosyası'nı**seçin.

1. **Dil için'i** seçin ve istediğiniz giriş türünü seçin.

1. **Klasör** simgesini seçin ve içe aktarılabilmek için dosyaya göz atın.

    ![Dosyayı içe aktarma](./media/import-file.png)

1. Bir **Ayırıcı**seçin ve isteğe bağlı olarak içe aktarılan anahtar adları için kullanmak üzere bir **Önek** girin.

1. İsteğe bağlı olarak, bir **Etiket**seçin.

1. Alma işleminin tamamlanması için **Uygula'yı** seçin.

    ![Dosyayı içe aktarma tamamlandı](./media/import-file-complete.png)

## <a name="export-data"></a>Verileri dışarı aktarma

Dışa aktarma, Uygulama Yapılandırması'nda depolanan yapılandırma verilerini başka bir hedefe yazar. Örneğin, uygulama yapılandırma deposundaki verileri dağıtım sırasında uygulama kodunuza katışdırılmış bir dosyaya kaydetmek için dışa aktarma işlevini kullanın.

[Azure portalını](https://portal.azure.com) veya [Azure CLI'yi](./scripts/cli-export.md)kullanarak veri aktarın. Azure portalından aşağıdaki adımları izleyin:

1. Uygulama Yapılandırma mağazanıza göz atın ve **İçe/Dışa Aktar'ı**seçin.

1. **Dışa Aktarma** sekmesinde, **Hedef hizmeti** > **Yapılandırma Dosyası'nı**seçin.

1. İsteğe bağlı olarak bir **Önek** girin ve bir **Etiket** ve dışa aktarılacak anahtarlar için bir zaman noktası seçin.

1. Dosya **türü** > **Ayırıcı'yı**seçin.

1. Dışa aktarmayı tamamlamak için **Uygula'yı** seçin.

    ![Dışa aktarma dosyası tamamlandı](./media/export-file-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
