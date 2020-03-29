---
title: Microsoft OneDrive'da dosyalara erişin ve yönetin
description: Azure Logic Apps'ta otomatik iş akışları oluşturarak OneDrive'da dosyaları yükleyin ve yönetin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/18/2016
tags: connectors
ms.openlocfilehash: edfbf090c3409d583cda6fd2c9957c37be5dfb7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75378441"
---
# <a name="access-and-manage-files-in-onedrive-connector-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak OneDrive konektördeki dosyalara erişin ve yönetin

Azure [Logic Apps'ı](../logic-apps/logic-apps-overview.md) ve [OneDrive konektörünü](/connectors/onedriveconnector/)kullanarak, dosyalarınızı yüklemek, almak, silmek ve daha fazlası dahil olmak üzere dosyalarınızı yönetmek için otomatik görevler ve iş akışları oluşturabilirsiniz. OneDrive ile şu görevleri gerçekleştirebilirsiniz:

* Dosyaları OneDrive'da depolayarak iş akışınızı oluşturun veya OneDrive'da varolan dosyaları güncelleştirin. 
* OneDrive'ınızda bir dosya oluşturulduğunda veya güncelleştirildiğinde iş akışınızı başlatmak için tetikleyicileri kullanın.
* Dosya oluşturmak, dosyayı silmek ve daha fazlası için eylemleri kullanın. Örneğin, yeni bir Office 365 e-postası ek (tetikleyici) ile birlikte alındığı zaman, OneDrive'da (bir eylem) yeni bir dosya oluşturun.

Bu makalede, bir mantık uygulamasında OneDrive konektörünü nasıl kullanacağınızı gösterir ve tetikleyicileri ve eylemleri de listeler.

Mantık Uygulamaları hakkında daha fazla bilgi edinmek [için, mantık uygulamaları nedir](../logic-apps/logic-apps-overview.md) ve [bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="connect-to-onedrive"></a>OneDrive’a bağlanma

Mantık uygulamanız herhangi bir hizmete erişemeden önce, önce hizmete bir *bağlantı* oluşturursunuz. Bağlantı, bir mantık uygulaması ile başka bir hizmet arasında bağlantı sağlar. Örneğin, OneDrive'a bağlanmak için öncelikle onedrive *bağlantısına*ihtiyacınız var. Bağlantı oluşturmak için, bağlanmak istediğiniz hizmete erişmek için normalde kullandığınız kimlik bilgilerini girin. Bu nedenle, OneDrive ile bağlantıyı oluşturmak için OneDrive hesabınıza kimlik bilgilerini girin.

### <a name="create-the-connection"></a>Bağlantı oluşturma

[!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]

## <a name="use-a-trigger"></a>Tetikleyici kullanma

Tetikleyici, bir mantık uygulamasında tanımlanan iş akışını başlatmak için kullanılabilecek bir olaydır. Hizmeti istediğiniz aralıkta ve sıklıkta "yoklama"yı tetikler. [Tetikleyiciler hakkında daha fazla bilgi edinin.](../logic-apps/logic-apps-overview.md#logic-app-concepts)

1. Logic App Designer'da `onedrive` tetikleyicilerin listesini almak için yazın:  

   ![](./media/connectors-create-api-onedrive/onedrive-1.png)

2. **Dosya değiştirildiğinde**seçin. Bağlantı zaten varsa, bir klasör seçmek için Seçici'yi Göster düğmesini seçin.

   ![](./media/connectors-create-api-onedrive/sample-folder.png)

   Oturum açmanız istenirse, bağlantıyı oluşturmak için oturum açma ayrıntılarını girin. Bu makalede [bağlantı oluşturma](connectors-create-api-onedrive.md#create-the-connection) adımları listeler.

   Bu örnekte, seçtiğiniz klasördeki bir dosya güncelleştirildiğinde mantık uygulaması çalışır. Bu tetikleyicinin sonuçlarını görmek için, size e-posta gönderen başka bir eylem ekleyin. Örneğin, Office 365 Outlook'u ekleyin Bir dosya güncelleştirildiğinde size *e-posta gönderen bir e-posta* eylemi gönderin.

3. **Edit** düğmesini seçin ve **Sıklık** ve **Aralık** değerlerini ayarlayın. Örneğin, tetikleyicinin her 15 dakikada bir yoklaması yapmak istiyorsanız, **Sıklığı** **Dakikaya**ayarlayın ve **Aralığı** **15**olarak ayarlayın. 

   ![](./media/connectors-create-api-onedrive/trigger-properties.png)

4. Değişikliklerinizi **kaydedin** (araç çubuğunun sol üst köşesinde). Mantık uygulamanız kaydedilir ve otomatik olarak etkinleştirilebilir.

## <a name="use-an-action"></a>Eylem kullanma

Eylem, bir mantık uygulamasında tanımlanan iş akışı tarafından gerçekleştirilen bir işlemdir. [Eylemler hakkında daha fazla bilgi edinin.](../logic-apps/logic-apps-overview.md#logic-app-concepts)

1. Artı işaretini seçin. Birkaç seçenek görürsünüz: **Eylem ekleyin,** **koşul ekleyin**veya **Daha Fazla** seçenekten birini ekleyin.

   ![](./media/connectors-create-api-onedrive/add-action.png)

2. **Eylem ekle'yi**seçin.

3. Arama kutusunda, kullanılabilir `onedrive` tüm eylemlerin listesini almak için yazın.

   ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 

4. Örneğimizde **OneDrive - Create dosyalarını**seçin. Bağlantı zaten varsa, dosyayı koymak için **Klasör Yolunu** seçin, **Dosya Adını**girin ve istediğiniz **Dosya İçeriği'ni** seçin:  

   ![](./media/connectors-create-api-onedrive/sample-action.png)

   Bağlantı bilgileri için [istenirse, bu](#create-the-connection) konuda açıklandığı gibi bağlantıyı oluşturmak için ayrıntıları girin.

   Bu örnekte, OneDrive klasöründe yeni bir dosya oluşturursunuz. OneDrive dosyasını oluşturmak için başka bir tetikleyiciden çıktı kullanabilirsiniz. Örneğin, *yeni bir e-posta* geldiğinde Office 365 Outlook'u ekleyin. Ardından, OneDrive'da yeni dosyayı oluşturmak için ForEach içindeki Ekler ve İçerik Türü alanlarını kullanan OneDrive *Oluştur dosya* eylemini ekleyin.

   ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. Değişikliklerinizi **kaydedin** (araç çubuğunun sol üst köşesinde). Mantık uygulamanız kaydedilir ve otomatik olarak etkinleştirilebilir.

## <a name="connector-specific-details"></a>Bağlayıcıya özel ayrıntılar

Swagger'da tanımlanan tetikleyicileri ve eylemleri görüntüleyin ve [bağlayıcı ayrıntılarında](/connectors/onedriveconnector/)da herhangi bir sınır görün.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps için Bağlayıcılar](apis-list.md)