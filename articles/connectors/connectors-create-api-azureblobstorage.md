---
title: Azure Blob depolama 'ya bağlanma-Azure Logic Apps
description: Azure Logic Apps ile Azure depolama 'da Bloblar oluşturma ve yönetme
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
manager: carmonm
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 06/20/2019
tags: connectors
ms.openlocfilehash: d57ea1a881980203b1c8f216239b27b64f0d71cd
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051045"
---
# <a name="create-and-manage-blobs-in-azure-blob-storage-with-azure-logic-apps"></a>Azure Logic Apps ile Azure Blob depolamada Bloblar oluşturma ve yönetme

Bu makalede, Azure Blob depolama Bağlayıcısı ile bir mantık uygulamasının içinden Azure depolama hesabınızda blob olarak depolanan dosyaları nasıl erişebileceğiniz ve yönetebileceğiniz gösterilmektedir. Bu şekilde, dosyalarınızı yönetmek için görevleri ve iş akışlarını otomatikleştiren mantıksal uygulamalar oluşturabilirsiniz. Örneğin, depolama hesabınızda dosya oluşturan, alan güncelleştiren, güncelleştiren ve silen mantıksal uygulamalar oluşturabilirsiniz.

Bir Azure Web sitesinde güncelleştirilmiş bir aracınız olduğunu varsayalım. mantıksal uygulamanız için tetikleyici işlevi görür. Bu olay gerçekleştiğinde, mantıksal uygulamanızdaki bir işlem olan BLOB depolama kapsayıcısında bir dosya olan bir dosya, mantıksal uygulamanızın güncelleştirilmesini sağlayabilirsiniz.

> [!NOTE]
> Logic Apps, Azure Storage hesaplarına güvenlik duvarları aracılığıyla doğrudan bağlanmayı desteklemez. Bu depolama hesaplarına erişmek için şu seçeneklerden birini kullanın:
>
> * Bir Azure sanal ağındaki kaynaklara bağlanabilecek bir [tümleştirme hizmeti ortamı](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)oluşturun.
>
> * Zaten API Management kullanıyorsanız bu hizmeti bu senaryo için kullanabilirsiniz. Daha fazla bilgi için bkz. [basit kurumsal tümleştirme mimarisi](https://aka.ms/aisarch).

Logic Apps 'e yeni başladıysanız [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç ' ı inceleyin: İlk mantıksal uygulamanızı](../logic-apps/quickstart-create-first-logic-app-workflow.md)oluşturun. Bağlayıcıya özgü teknik bilgiler için bkz. [Azure Blob depolama Bağlayıcısı başvurusu](/connectors/azureblobconnector/).

## <a name="limits"></a>Sınırlar

* Varsayılan olarak, Azure Blob depolama eylemleri *50 MB veya daha küçük*olan dosyaları okuyabilir veya yazabilir. 50 MB 'den büyük ancak 1024 MB 'a kadar olan dosyaları işlemek için Azure Blob depolama eylemleri [ileti parçalama](../logic-apps/logic-apps-handle-large-messages.md)desteği sağlar. **BLOB Içeriğini al** eylemi, örtük olarak parçalama kullanır.

* Azure Blob depolama Tetikleyicileri parçalama desteği vermez. Dosya içeriği istenirken Tetikleyiciler yalnızca 50 MB veya daha küçük olan dosyaları seçer. 50 MB 'tan büyük dosyaları almak için şu modele uyun:

  * **BLOB eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** dosya özellikleri döndüren bir Azure Blob depolama tetikleyicisi kullanın.

  * Tam dosyayı okuyan ve örtük olarak parçalama kullanan Azure Blob depolama **BLOB Içeriğini al** eylemini kullanarak tetikleyiciyi izleyin.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Bir [Azure depolama hesabı ve depolama kapsayıcısı](../storage/blobs/storage-quickstart-blobs-portal.md)

* Azure Blob depolama hesabınıza erişmeniz gereken mantıksal uygulama. Mantıksal uygulamanızı bir Azure Blob depolama tetikleyicisi ile başlatmak için [boş bir mantıksal uygulama](../logic-apps/quickstart-create-first-logic-app-workflow.md)gerekir.

<a name="add-trigger"></a>

## <a name="add-blob-storage-trigger"></a>BLOB depolama tetikleyicisi Ekle

Azure Logic Apps, her mantıksal uygulama, belirli bir olay gerçekleştiğinde [](../logic-apps/logic-apps-overview.md#logic-app-concepts)veya belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her tetiklendiğinde Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

Bu örnek, depolama kapsayıcıınızda bir blob 'un özellikleri eklendiğinde veya güncelleştirilirken bir **BLOB eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)** tetikleyicisiyle bir mantıksal uygulama iş akışını nasıl başlatakullanabileceğinizi gösterir.

1. [Azure Portal](https://portal.azure.com) veya Visual Studio 'Da mantıksal uygulama Tasarımcısı ' nı açan boş bir mantıksal uygulama oluşturun. Bu örnek Azure portal kullanır.

2. Arama kutusuna filtreniz olarak "Azure Blob" yazın. Tetikleyiciler listesinden istediğiniz tetikleyiciyi seçin.

   Bu örnek, bu tetikleyiciyi kullanır: **Blob eklendiğinde veya değiştirildiğinde (yalnızca Özellikler)**

   ![Tetikleyici seçin](./media/connectors-create-api-azureblobstorage/azure-blob-trigger.png)

3. Bağlantı ayrıntıları istenirse, [BLOB depolama bağlantınızı şimdi oluşturun](#create-connection). Ya da bağlantınız zaten varsa, tetikleyici için gerekli bilgileri sağlayın.

   Bu örnek için, izlemek istediğiniz kapsayıcıyı ve klasörü seçin.

   1. **Kapsayıcı** kutusunda klasör simgesini seçin.

   2. Klasör listesinde, sağ açılı köşeli ayracı ( **>** ) seçin ve istediğiniz klasörü bulup seçinceye kadar gözatın.

      ![Klasör seçin](./media/connectors-create-api-azureblobstorage/trigger-select-folder.png)

   3. Tetikleyicinin klasör değişikliklerini ne sıklıkta denetlemesini istediğinizi belirlemek için aralığı ve sıklığı seçin.

4. İşiniz bittiğinde, Tasarımcı araç çubuğunda **Kaydet**' i seçin.

5. Şimdi, tetikleyici sonuçlarıyla gerçekleştirmek istediğiniz görevler için mantıksal uygulamanıza bir veya daha fazla eylem eklemeye devam edin.

<a name="add-action"></a>

## <a name="add-blob-storage-action"></a>BLOB depolama eylemi ekleme

Azure Logic Apps bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) , iş akışınızda bir tetikleyiciyi veya başka bir eylemi izleyen bir adımdır. Bu örnekte, mantıksal uygulama [yinelenme tetikleyicisiyle](../connectors/connectors-native-recurrence.md)başlar.

1. [Azure Portal](https://portal.azure.com) veya Visual Studio 'Da mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın. Bu örnek Azure portal kullanır.

2. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici veya eylem altında **yeni adım**' ı seçin.

   ![Eylem ekle](./media/connectors-create-api-azureblobstorage/add-action.png) 

   Varolan adımlar arasında bir eylem eklemek için farenizi bağlantı oku üzerine taşıyın. Görüntülenen artı işaretini ( **+** ) seçin ve **Eylem Ekle**' yi seçin.

3. Arama kutusuna filtreniz olarak "Azure Blob" yazın. Eylemler listesinden istediğiniz eylemi seçin.

   Bu örnek, bu eylemi kullanır: **Blob içeriğini al**

   ![Eylem Seç](./media/connectors-create-api-azureblobstorage/azure-blob-action.png)

4. Bağlantı ayrıntıları istenirse, [Azure Blob depolama bağlantınızı hemen oluşturun](#create-connection).
Ya da bağlantınız zaten varsa, eylem için gerekli bilgileri sağlayın.

   Bu örnek için istediğiniz dosyayı seçin.

   1. **BLOB** kutusundan klasör simgesini seçin.
  
      ![Klasör seçin](./media/connectors-create-api-azureblobstorage/action-select-folder.png)

   2. Blob 'un **kimlik** numarasına göre istediğiniz dosyayı bulun ve seçin. Bu **kimlik** numarasını, daha önce açıklanan BLOB depolama tetikleyicisi tarafından döndürülen Blobun meta verilerinde bulabilirsiniz.

5. İşiniz bittiğinde, Tasarımcı araç çubuğunda **Kaydet**' i seçin.
Mantıksal uygulamanızı test etmek için, seçili klasörde bir blob bulunduğundan emin olun.

Bu örnek yalnızca bir Blobun içeriğini alır. İçeriği görüntülemek için başka bir bağlayıcı kullanarak blob ile dosya oluşturan başka bir eylem ekleyin. Örneğin, blob içeriklerine dayalı bir dosya oluşturan OneDrive eylemi ekleyin.

<a name="create-connection"></a>

## <a name="connect-to-storage-account"></a>Depolama hesabına Bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

[!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının Open API (eski adıyla Swagger) dosyasında açıklandığı gibi Tetikleyiciler, Eylemler ve sınırlar gibi teknik ayrıntılar için [bağlayıcının başvuru sayfasına](/connectors/azureblobconnector/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
