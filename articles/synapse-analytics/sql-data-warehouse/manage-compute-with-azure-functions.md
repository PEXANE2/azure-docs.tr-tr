---
title: 'Öğretici: Azure İşlevleriyle hesaplamayı yönetme'
description: Azure Synapse Analytics'teki SQL havuzunuzun bilgi işlemini yönetmek için Azure işlevlerini kullanma.
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/27/2018
ms.author: jrasnick
ms.reviewer: igorstan
ms.custom: seo-lt-2019, azure-synapse
ms.openlocfilehash: e0317b3a3e7ab13a78a5d1fe3672d664030436ab
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346651"
---
# <a name="use-azure-functions-to-manage-compute-resources-in-azure-synapse-analytics-sql-pool"></a>Azure Synapse Analytics SQL havuzunda bilgi işlem kaynaklarını yönetmek için Azure Işlevlerini kullanın

Bu öğretici, Azure Synapse Analytics'teki bir SQL havuzunun bilgi işlem kaynaklarını yönetmek için Azure İşlevlerini kullanır.

SQL havuzu ile Azure İşlev Uygulaması'nı kullanmak için, SQL havuz örneğinizle aynı abonelik altında katılımcı erişimine sahip bir [Hizmet Sorumlusu Hesabı](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) oluşturmanız gerekir. 

## <a name="deploy-timer-based-scaling-with-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonuyla zamanlayıcı tabanlı ölçekleme dağıtma

Şablonu dağıtmak için aşağıdaki bilgilere ihtiyacınız var:

- SQL havuz örneğinizin içinde olduğu kaynak grubunun adı
- SQL havuz örneğinizin içinde olduğu mantıksal sunucunun adı
- SQL havuz örneğinizin adı
- Azure Active Directory'nizin Kiracı Kimliği (Dizin Kimliği)
- Abonelik Kimliği 
- Hizmet Sorumlusu Uygulama Kimliği
- Hizmet Sorumlusu Gizli Anahtarı

Önceki bilgilere sahip olduktan sonra, bu şablonu dağıtın:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fsql-data-warehouse-samples%2Fmaster%2Farm-templates%2FsqlDwTimerScaler%2Fazuredeploy.json" target="_blank">
<img src="https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/1-CONTRIBUTION-GUIDE/images/deploytoazure.png"/>
</a>

Şablonu dağıttıktan sonra üç yeni kaynak bulmanız gerekir: ücretsiz Azure Uygulama Hizmet Planı, tüketim tabanlı İşlev Uygulaması planı ve günlüğe kaydetme ve işlem sırasını işleyen bir depolama hesabı. Dağıtılan işlevlerin gereksinimlerinize uyacak şekilde nasıl değiştirileceğini görmek için okumaya devam edin.

## <a name="change-the-compute-level"></a>İşlem düzeyini değiştirme

1. İşlev Uygulaması hizmetinize gidin. Şablonu varsayılan değerlerle dağıttıysanız, bu hizmetin adı *DWOperations* olacaktır. İşlev Uygulamanız açıldığında, İşlev Uygulaması Hizmetinize beş işlevin dağıtılmış olduğunu görürsünüz. 

   ![Şablonla dağıtılan işlevler](./media/manage-compute-with-azure-functions/five-functions.png)

2. Zaman ölçeğini artırmak mı yoksa azaltmak mı istediğinize bağlı olarak, *DWScaleDownTrigger*'ı veya *DWScaleUpTrigger*'ı seçin. Açılan menüde Tümleştir'i seçin.

   ![İşlev için Tümleştir'i seçme](./media/manage-compute-with-azure-functions/select-integrate.png)

3. Şu anda görüntülenen değer *%ScaleDownTime%* veya *%ScaleUpTime%* olmalıdır. Bu değerler, zamanlamanın [Uygulama Ayarları](../../azure-functions/functions-how-to-use-azure-function-app-settings.md) altında tanımlanmış değerleri temel alacağını gösterir. Şimdilik, bu değeri yoksayabilir ve sonraki adımlara bağlı olarak zamanlamayı tercih ettiğiniz zamana değiştirebilirsiniz.

4. Zamanlama alanında, SQL Veri Ambarı'nın ölçeğinin ne sıklıkta artırılmasını istediğinizi yansıtan zaman CRON ifadesini ekleyin. 

   ![İşlev zamanlamasını değiştirme](./media/manage-compute-with-azure-functions/change-schedule.png)

   `schedule` değeri, şu altı alanı içeren bir [CRON ifadesidir](https://en.wikipedia.org/wiki/Cron#CRON_expression): 
   ```json
   {second} {minute} {hour} {day} {month} {day-of-week}
   ```

   Örneğin, *"0 30 9 * * 1-5"* hafta içi her gün saat 09:30'da bir tetikleyiciyi yansıtır. Daha fazla bilgi için Azure İşlevleri[zamanlama örnekleri](../../azure-functions/functions-bindings-timer.md#example) sayfasını ziyaret edin.


## <a name="change-the-time-of-the-scale-operation"></a>Ölçek işleminin saatini değiştirme

1. İşlev Uygulaması hizmetinize gidin. Şablonu varsayılan değerlerle dağıttıysanız, bu hizmetin adı *DWOperations* olacaktır. İşlev Uygulamanız açıldığında, İşlev Uygulaması Hizmetinize beş işlevin dağıtılmış olduğunu görürsünüz. 

2. İşlem değeri ölçeğini artırmak mı yoksa azaltmak mı istediğinize bağlı olarak, *DWScaleDownTrigger*'ı veya *DWScaleUpTrigger*'ı seçin. İşlevleri seçtikten sonra, bölmenizde *index.js* dosyası gösterilmelidir.

   ![İşlev tetikleyicisi işlem düzeyini değiştirme](././media/manage-compute-with-azure-functions/index-js.png)

3. *ServiceLevelObjective*'in değerini istediğiniz düzeyle değiştirin ve Kaydet'e tıklayın. Bu değer, veri ambarı örneğinizin Tümleştirme bölümünde tanımlanan zamanlamayı temel alan ölçeklendireceği bilgi işlem düzeyidir.

## <a name="use-pause-or-resume-instead-of-scale"></a>Ölçek yerine duraklatma veya sürdürme kullanma 

Şu anda, varsayılan olarak açık olan işlevler *DWScaleDownTrigger* ve *DWScaleUpTrigger*'dır. Bunun yerine duraklatma ve sürdürme işlevselliğini kullanmak isterseniz, *DWPauseTrigger* veya *DWResumeTrigger*'ı etkinleştirebilirsiniz.

1. İşlevler bölmesine gidin.

   ![İşlevler bölmesi](./media/manage-compute-with-azure-functions/functions-pane.png)



2. Etkinleştirmek istediğiniz tetikleyicilere karşılık gelen kayan düğmeye tıklayın.

3. Zamanlamalarını değiştirmek için ilgili tetikleyicilerin *Tümleştir* sekmelerine gidin.

   > [!NOTE]
   > Ölçekleme tetikleyicileri ile duraklatma/devam tetikleyicileri arasındaki işlevsel fark, kuyruğa gönderilen iletidir. Daha fazla bilgi için [bkz.](manage-compute-with-azure-functions.md#add-a-new-trigger-function)


## <a name="add-a-new-trigger-function"></a>Yeni tetikleyici işlevi ekleme

Şu anda, şablona dahil edilmiş yalnızca iki ölçeklendirme işlevi vardır. Bu işlevlerle, bir gün boyunca, yalnızca bir kez küçültebilir ve bir kez daha fazla. Günde birden çok kez ölçekleme veya hafta sonları farklı ölçekleme davranışı na sahip olmak gibi daha ayrıntılı denetim için başka bir tetikleyici eklemeniz gerekir.

1. Yeni boş bir işlev oluşturun. İşlev *+* şablonu bölmesini göstermek için Işlevler konumunun yanındaki düğmeyi seçin.

   ![Yeni işlev oluşturma](./media/manage-compute-with-azure-functions/create-new-function.png)

2. Dil'de *Javascript*'i seçin ve sonra da *TimerTrigger*'ı seçin.

   ![Yeni işlev oluşturma](./media/manage-compute-with-azure-functions/timertrigger-js.png)

3. İşlevinizi adlandırın ve zamanlamanızı ayarlayın. Resimde, işlevin her Cumartesi gece yarısı (Cuma gecesi) nasıl tetiklenebileceği gösterilir.

   ![Cumartesi ölçeği azaltma](./media/manage-compute-with-azure-functions/scale-down-saturday.png)

4. Diğer tetikleyici işlevlerinden birinin *index.js* içeriğini kopyalayın.

   ![Index.js'yi kopyalama](././media/manage-compute-with-azure-functions/index-js.png)

5. Operasyon değişkeninizi aşağıdaki gibi istenilen davranışa ayarlayın:

   ```javascript
   // Resume the SQL pool instance
   var operation = {
       "operationType": "ResumeDw"
   }

   // Pause the SQL pool instance
   var operation = {
       "operationType": "PauseDw"
   }

   // Scale the SQL pool instance to DW600
   var operation = {
       "operationType": "ScaleDw",
       "ServiceLevelObjective": "DW600"
   }
   ```


## <a name="complex-scheduling"></a>Karmaşık zamanlama

Bu bölümde, duraklatma, devam etme ve ölçekleme yeteneklerinin daha karmaşık bir şekilde zamanlanması için nelerin gerekli olduğu kısaca gösterin.

### <a name="example-1"></a>Örnek 1:

Gündelik olarak 08:00'da DW600'a ölçeği artırma ve 20:00'da DW200'e ölçeği azaltma.

| İşlev  | Zamanlama     | İşlem                                |
| :-------- | :----------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW600"}` |
| İşlev2 | 0 0 20 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-2"></a>Örnek 2: 

Günlük ölçek 08:00'den DW1000'e kadar ölçeklendirin, 16:00'da DW600'e bir kez küçültün ve 22:00'de DW200'e kadar küçültün.

| İşlev  | Zamanlama     | İşlem                                |
| :-------- | :----------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * *  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| İşlev2 | 0 0 16 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| İşlev3 | 0 0 22 * * * | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW200"}` |

### <a name="example-3"></a>Örnek 3: 

Hafta içi günlerinde 08:00'da DW1000'e ölçeği artırma ve 16:00'da bir kez DW600'e ölçeği azaltma. Cuma 23:00'da duraklatılır, Pazartesi sabahı 07:00'da sürdürülür.

| İşlev  | Zamanlama       | İşlem                                |
| :-------- | :------------- | :--------------------------------------- |
| İşlev1 | 0 0 8 * * 1-5  | `var operation = {"operationType": "ScaleDw",    "ServiceLevelObjective": "DW1000"}` |
| İşlev2 | 0 0 16 * * 1-5 | `var operation = {"operationType": "ScaleDw", "ServiceLevelObjective": "DW600"}` |
| İşlev3 | 0 0 23 * * 5   | `var operation = {"operationType": "PauseDw"}` |
| İşlev4 | 0 0 7 * * 0    | `var operation = {"operationType": "ResumeDw"}` |



## <a name="next-steps"></a>Sonraki adımlar

[Zamanlayıcı tetikleyicisi](../../azure-functions/functions-create-scheduled-function.md) Azure işlevleri hakkında daha fazla bilgi edinin.

SQL havuz [örnekleri deposunu](https://github.com/Microsoft/sql-data-warehouse-samples)kullanıma alıyoruz.

