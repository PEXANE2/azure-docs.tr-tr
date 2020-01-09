---
title: Microsoft OneDrive 'da dosyalara erişin ve dosyaları yönetin
description: Azure Logic Apps içinde otomatik iş akışları oluşturarak OneDrive 'daki dosyaları karşıya yükleme ve yönetme
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/18/2016
tags: connectors
ms.openlocfilehash: edfbf090c3409d583cda6fd2c9957c37be5dfb7a
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75378441"
---
# <a name="access-and-manage-files-in-onedrive-connector-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak OneDrive bağlayıcıdaki dosyalara erişin ve yönetin

[Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [OneDrive bağlayıcısını](/connectors/onedriveconnector/)kullanarak, dosyalarınızı yönetmek için, dosyaları karşıya yükle, al, Sil ve daha fazlasını içeren otomatikleştirilmiş görevler ve iş akışları oluşturabilirsiniz. OneDrive ile, şu görevleri gerçekleştirebilirsiniz:

* OneDrive 'daki dosyaları depolayarak iş akışınızı oluşturun veya OneDrive 'daki mevcut dosyaları güncelleştirin. 
* OneDrive 'da bir dosya oluşturulduğunda veya güncelleştirilirken iş akışınızı başlatmak için Tetikleyicileri kullanın.
* Bir dosya oluşturmak, bir dosyayı silmek ve daha fazlasını yapmak için eylemleri kullanın. Örneğin, yeni bir Office 365 e-postası bir ek (tetikleyici) ile alındığında OneDrive 'da yeni bir dosya oluşturun (bir eylem).

Bu makalede, bir mantıksal uygulamada OneDrive bağlayıcısının nasıl kullanılacağı gösterilir ve ayrıca Tetikleyiciler ve eylemler listelenir.

Logic Apps hakkında daha fazla bilgi edinmek için bkz. [Logic Apps nedir](../logic-apps/logic-apps-overview.md) ve [mantıksal uygulama oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-onedrive"></a>OneDrive’a bağlanma

Mantıksal uygulamanızın herhangi bir hizmete erişebilmesi için önce hizmete bir *bağlantı* oluşturmanız gerekir. Bir bağlantı, mantıksal uygulama ile başka bir hizmet arasında bağlantı sağlar. Örneğin, OneDrive 'a bağlanmak için önce OneDrive *bağlantınız*olması gerekir. Bağlantı oluşturmak için, bağlanmak istediğiniz hizmete erişmek için normalde kullandığınız kimlik bilgilerini girin. Bu nedenle, OneDrive ile bağlantıyı oluşturmak için OneDrive hesabınıza ait kimlik bilgilerini girin.

### <a name="create-the-connection"></a>Bağlantı oluşturma

[!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]

## <a name="use-a-trigger"></a>Tetikleyici kullanma

Tetikleyici, bir mantıksal uygulamada tanımlanan iş akışını başlatmak için kullanılabilen bir olaydır. Hizmeti, istediğiniz zaman aralığında ve sıklıkta "yoklamayı" tetikler. [Tetikleyiciler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Mantıksal uygulama Tasarımcısı ' nda, tetikleyicilerinin bir listesini almak için `onedrive` yazın:  

   ![](./media/connectors-create-api-onedrive/onedrive-1.png)

2. **Bir dosya değiştirildiğinde**seçin. Bir bağlantı zaten varsa, bir klasör seçmek için seçiciyi göster düğmesini seçin.

   ![](./media/connectors-create-api-onedrive/sample-folder.png)

   Oturum açmanız istenirse, bağlantıyı oluşturmak için oturum açma ayrıntılarını girin. Bu makalede [bağlantıyı oluşturma](connectors-create-api-onedrive.md#create-the-connection) adımları listelenir.

   Bu örnekte, mantıksal uygulama, seçtiğiniz klasörde bir dosya güncelleştirildiği zaman çalışır. Bu tetikleyicinin sonuçlarını görmek için size e-posta gönderen başka bir eylem ekleyin. Örneğin, Office 365 Outlook 'a bir dosya güncelleştirildiği sırada size e *-posta gönder* eylemi ekleyin.

3. **Düzenle** düğmesini seçin ve **Sıklık** ve **Aralık** değerlerini ayarlayın. Örneğin, tetikleyicinin 15 dakikada bir yoklamasını istiyorsanız, **sıklığı** **dakika**olarak ayarlayın ve **aralığı** **15**olarak ayarlayın. 

   ![](./media/connectors-create-api-onedrive/trigger-properties.png)

4. Değişikliklerinizi **kaydedin** (araç çubuğunun sol üst köşesi). Mantıksal uygulamanız kaydedilir ve otomatik olarak etkinleştirilebilir.

## <a name="use-an-action"></a>Eylem kullanma

Eylem, mantıksal uygulamada tanımlanan iş akışı tarafından yürütülen bir işlemdir. [Eylemler hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Artı işaretini seçin. Birkaç seçenek görürsünüz: **eylem ekleme**, **koşul ekleme**veya **daha fazla** seçenekten biri.

   ![](./media/connectors-create-api-onedrive/add-action.png)

2. **Eylem Ekle**' yi seçin.

3. Tüm kullanılabilir eylemlerin listesini almak için arama kutusuna `onedrive` yazın.

   ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 

4. Örneğimizde **OneDrive-dosya oluştur**' u seçin. Zaten bir bağlantı varsa, dosyayı yerleştirmek için **klasör yolunu** seçin, **dosya adını**girin ve istediğiniz **dosya içeriğini** seçin:  

   ![](./media/connectors-create-api-onedrive/sample-action.png)

   Bağlantı bilgileri istenirse, bağlantıyı oluşturmak için bu konuda [açıklandığı gibi](#create-the-connection) ayrıntıları girin.

   Bu örnekte, OneDrive klasöründe yeni bir dosya oluşturacaksınız. OneDrive dosyasını oluşturmak için başka bir tetikleyiciden çıkış kullanabilirsiniz. Örneğin, *Yeni bir e-posta geldiğinde* tetikleyiciyi Office 365 Outlook ' u ekleyin. Ardından, OneDrive 'da yeni bir dosya oluşturmak için bir ForEach içindeki ekleri ve Içerik türü alanlarını kullanan OneDrive *dosya oluştur* eylemini ekleyin.

   ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. Değişikliklerinizi **kaydedin** (araç çubuğunun sol üst köşesi). Mantıksal uygulamanız kaydedilir ve otomatik olarak etkinleştirilebilir.

## <a name="connector-specific-details"></a>Bağlayıcıya özgü ayrıntılar

Swagger 'da tanımlanan Tetikleyicileri ve eylemleri görüntüleyin ve ayrıca [bağlayıcı ayrıntılarında](/connectors/onedriveconnector/)tüm limitleri inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps için Bağlayıcılar](apis-list.md)