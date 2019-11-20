---
title: Azure Uygulama yapılandırması ile verileri içeri aktarma veya dışarı aktarma | Microsoft Docs
description: Azure Uygulama yapılandırmasına/içeri veya dışarı veri aktarmayı öğrenin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 9780dc34aa6b146fe62b11586cbab46825e60535
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185166"
---
# <a name="import-or-export-configuration-data"></a>Yapılandırma verilerini içeri veya dışarı aktarma

Azure Uygulama yapılandırması, veri içeri ve dışarı aktarma işlemlerini destekler. Uygulama yapılandırma depem ve kod projeniz arasında toplu ve Exchange verilerinde yapılandırma verileriyle çalışmak için bu işlemleri kullanın. Örneğin, test için bir uygulama yapılandırma deposu ve bir üretim için bir tane oluşturabilirsiniz. Daha sonra, verileri iki kez girmek zorunda kalmaması için uygulama ayarlarını bir dosya aracılığıyla kopyalayabilirsiniz.

Bu makale, uygulama yapılandırmasıyla veri içeri ve dışarı aktarmaya yönelik bir kılavuz sağlar.

## <a name="import-data"></a>Veri içeri aktarma

İçeri aktarma, yapılandırma verilerini el ile girmek yerine mevcut bir kaynaktan bir uygulama yapılandırma deposuna getirir. İçeri aktarma işlevini kullanarak bir uygulama yapılandırma deposuna veri geçirin veya birden çok kaynaktan veri toplayın. Uygulama yapılandırması, JSON, YAML veya özellikler dosyasından içeri aktarmayı destekler.

[Azure Portal](https://portal.azure.com) ya da [Azure CLI](./scripts/cli-import.md)kullanarak verileri içeri aktarın. Azure portal, aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deponuza gidin ve **içeri/dışarı aktar**' ı seçin.

2. **Içeri aktar** sekmesinde **kaynak hizmeti** > **yapılandırma dosyası**' nı seçin.

3. Dil > **dosya türü** **için** seçin.

4. **Klasör** simgesini seçin ve içeri aktarılacak dosyaya gidin.

    ![Dosyayı içeri aktar](./media/import-file.png)

5. Bir **ayırıcı**seçin ve isteğe bağlı olarak içeri aktarılan anahtar adları için kullanılacak bir **ön ek** girin.

6. İsteğe bağlı olarak bir **etiket**seçin.

7. İçeri aktarma işleminin bitmesini sağlamak için **Uygula** ' yı seçin.

    ![Dosya içeri aktarma tamamlandı](./media/import-file-complete.png)

## <a name="export-data"></a>Verileri dışarı aktarma

Uygulama yapılandırmasında depolanan yazma yapılandırması verilerini başka bir hedefe dışarı aktarın. Uygulama yapılandırma deposundaki verileri dağıtım sırasında uygulama kodunuzla birlikte gömülü bir dosyaya kaydetmek için dışarı aktarma işlevini kullanın.

[Azure Portal](https://portal.azure.com) veya [Azure CLI](./scripts/cli-export.md)kullanarak verileri dışarı aktarın. Azure portal, aşağıdaki adımları izleyin:

1. Uygulama yapılandırma deponuza gidin ve **içeri/dışarı aktar**' ı seçin.

2. **Dışarı aktar** sekmesinde **hedef hizmet** > **yapılandırma dosyası**' nı seçin.

3. İsteğe bağlı olarak bir **ön ek** girin ve anahtarlar Için bir **etiket** ve zaman içinde bir nokta seçin.

4.  > **ayırıcı**bir **dosya türü** seçin.

5. Dışarı aktarmayı sona bırakmak için **Uygula** ' yı seçin.

    ![Dışarı aktarma dosyası tamamlandı](./media/export-file-complete.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [ASP.NET Core Web uygulaması oluşturma](./quickstart-aspnet-core-app.md)  
