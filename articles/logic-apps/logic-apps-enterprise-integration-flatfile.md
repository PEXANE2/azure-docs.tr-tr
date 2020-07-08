---
title: Düz dosyaları kodla veya kodunu çözün
description: Azure Logic Apps Enterprise Integration Pack kullanarak kurumsal tümleştirme için düz dosyaları kodlama veya kodunu çözme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 05/09/2020
ms.openlocfilehash: 81c1c95e2af7b537a12c8c86245b009005aa0aa2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83005333"
---
# <a name="encode-and-decode-flat-files-in-azure-logic-apps-by-using-the-enterprise-integration-pack"></a>Enterprise Integration Pack'i kullanarak Azure Logic Apps'te düz dosyaları kodlama ve kodunu çözme

İşletmeden işletmeye (B2B) senaryosunda bir iş ortağına XML içeriği göndermeden önce, önce bu içeriği kodlamak isteyebilirsiniz. Bir mantıksal uygulama oluşturarak, düz **Dosya** bağlayıcısını kullanarak düz dosyaları kodlayabilir ve kod çözme yapabilirsiniz. Mantıksal uygulamanız, Istek tetikleyicisi, başka bir uygulama veya [Azure Logic Apps tarafından desteklenen diğer bağlayıcılar](../connectors/apis-list.md)gibi çeşitli kaynaklardan gelen bu XML içeriğini alabilir. Daha fazla bilgi için bkz. [Azure Logic Apps](logic-apps-overview.md)nedir?

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* **Düz dosya** bağlayıcısını ve mantıksal uygulamanızın iş akışını başlatan bir tetikleyiciyi kullanmak istediğiniz mantıksal uygulama. **Düz dosya** Bağlayıcısı, yalnızca Tetikleyiciler değil, yalnızca eylemler sağlar. Kodlama veya kod çözme amacıyla XML içeriğini mantıksal uygulamanıza akışa almak için tetikleyiciyi veya başka bir eylemi kullanabilirsiniz. Logic Apps 'e yeni başladıysanız [hızlı başlangıç: ilk mantıksal uygulamanızı oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md)konusunu inceleyin.

* Azure aboneliğinizle ilişkili ve **düz dosya** bağlayıcısını kullanmayı planladığınız [mantıksal uygulamayla bağlantılı](logic-apps-enterprise-integration-accounts.md#link-account) bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) . Hem mantıksal uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

* XML içeriğini kodlamada veya kod çözmede tümleştirme hesabınıza yüklediğiniz düz bir dosya [şeması](logic-apps-enterprise-integration-schemas.md)

* Tümleştirme hesabınızda zaten tanımlamış olduğunuz en az iki [ticari iş ortağı](logic-apps-enterprise-integration-partners.md)

## <a name="add-flat-file-encode-action"></a>Düz dosya kodlama eylemi ekleme

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızdaki tetikleyici veya eylem altında **yeni adım**  >  **Eylem Ekle**' yi seçin. Bu örnek, **BIR http isteği alındığında**adlandırılan istek tetikleyicisini kullanır ve mantıksal uygulamanın dışından gelen istekleri işler.

   > [!TIP]
   > JSON şeması sağlamak isteğe bağlıdır. Gelen istekten bir örnek yüküyle karşılaşırsanız, **şema oluşturmak için örnek yük kullan**' ı seçin, örnek yükü girin ve **bitti**' yi seçin. Şema, **Istek GÖVDESI JSON şeması** kutusunda görünür.

1. **Eylem seçin**altında girin `flat file` . Eylemler listesinden şu eylemi seçin: **düz dosya kodlama**

   !["Düz dosya kodlaması" eylemini seçin](./media/logic-apps-enterprise-integration-flatfile/select-flat-file-encoding.png)

1. Dinamik içerik listesinin görünmesi için **içerik** kutusunun içine tıklayın. Listeden, **BIR http isteği alındığında** bölümünde, tetikleyiciden gelen istek gövdesi çıktısını ve kodlanacak Içeriği içeren **Body** özelliğini seçin.

   ![Dinamik içerik listesinden kodlamak için içerik seçin](./media/logic-apps-enterprise-integration-flatfile/select-content-to-encode.png)

   > [!TIP]
   > Dinamik içerik listesinde **Body** özelliğini görmüyorsanız, **bir http isteği alındığında** bölüm etiketi ' nün yanındaki **daha fazla göster** ' i seçin.
   > Ayrıca **içerik** kutusunda kodunu çözecek içeriği doğrudan girebilirsiniz.

1. **Şema adı** listesinden, kodlama için kullanmak üzere bağlantılı tümleştirme hesabınızda bulunan şemayı seçin, örneğin:

   ![Kodlama için kullanılacak şemayı seçin](./media/logic-apps-enterprise-integration-flatfile/select-schema-for-encoding.png)

   > [!NOTE]
   > Listede hiçbir şema görünmüyorsa, tümleştirme hesabınız kodlama için kullanılacak herhangi bir şema dosyası içermez. Kullanmak istediğiniz şemayı tümleştirme hesabınıza yükleyin.

1. Mantıksal uygulamanızı kaydedin. Bağlayıcınızı test etmek için, Istek tetikleyicisinin **http post URL 'si** ÖZELLIĞINDE görüntülenen HTTPS uç noktasına bir istek yapın ve istek gövdesine kodlamak istediğiniz XML içeriğini ekleyin.

Artık düz dosya kodlama eyleminizi ayarlamaya hazırsınız. Gerçek bir dünya uygulamasında, kodlanmış verileri Salesforce gibi bir iş kolu (LOB) uygulamasında depolamak isteyebilirsiniz. Ya da kodlanmış verileri bir ticaret ortağına gönderebilirsiniz. Kodlama eyleminden çıktıyı Salesforce 'a veya ticari iş ortağınıza göndermek için [Azure Logic Apps bulunan diğer bağlayıcıları](../connectors/apis-list.md)kullanın.

## <a name="add-flat-file-decode-action"></a>Düz dosya kod çözme eylemi ekleme

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında mantıksal uygulamanızı açın.

1. Mantıksal uygulamanızdaki tetikleyici veya eylem altında **yeni adım**  >  **Eylem Ekle**' yi seçin. Bu örnek, **BIR http isteği alındığında**adlandırılan istek tetikleyicisini kullanır ve mantıksal uygulamanın dışından gelen istekleri işler.

   > [!TIP]
   > JSON şeması sağlamak isteğe bağlıdır. Gelen istekten bir örnek yüküyle karşılaşırsanız, **şema oluşturmak için örnek yük kullan**' ı seçin, örnek yükü girin ve **bitti**' yi seçin. Şema, **Istek GÖVDESI JSON şeması** kutusunda görünür.

1. **Eylem seçin**altında girin `flat file` . Eylemler listesinden şu eylemi seçin: **düz dosya kod çözme**

   !["Düz dosya kod çözme" eylemini seçin](./media/logic-apps-enterprise-integration-flatfile/select-flat-file-decoding.png)

1. Dinamik içerik listesinin görünmesi için **içerik** kutusunun içine tıklayın. Listeden, **BIR http isteği alındığında** bölümünde, tetikleyiciden gelen istek gövdesi çıktısını ve kodu çözülecek Içeriği içeren **Body** özelliğini seçin.

   ![Dinamik içerik listesinden kod çözmek için içerik seçin](./media/logic-apps-enterprise-integration-flatfile/select-content-to-decode.png)

   > [!TIP]
   > Dinamik içerik listesinde **Body** özelliğini görmüyorsanız, **bir http isteği alındığında** bölüm etiketi ' nün yanındaki **daha fazla göster** ' i seçin. Ayrıca **içerik** kutusunda kodunu çözecek içeriği doğrudan girebilirsiniz.

1. **Şema adı** listesinden, kod çözme için kullanmak üzere bağlantılı tümleştirme hesabınızda bulunan şemayı seçin, örneğin:

   ![Kod çözme için kullanılacak şemayı seçin](./media/logic-apps-enterprise-integration-flatfile/select-schema-for-decoding.png)

   > [!NOTE]
   > Listede hiçbir şema görünmüyorsa, tümleştirme hesabınız kod çözme için kullanılacak herhangi bir şema dosyası içermez. Kullanmak istediğiniz şemayı tümleştirme hesabınıza yükleyin.

1. Mantıksal uygulamanızı kaydedin. Bağlayıcınızı test etmek için, Istek tetikleyicisinin **http post URL 'si** ÖZELLIĞINDE görüntülenen HTTPS uç noktasına bir istek yapın ve istek gövdesinde kodunu çözmek istediğiniz XML içeriğini ekleyin.

Artık düz dosya kod çözme eyleminizi ayarlamaya hazırsınız. Gerçek bir dünya uygulamasında, kodu çözülen verileri Salesforce gibi bir iş kolu (LOB) uygulamasında depolamak isteyebilirsiniz. Ya da kodu çözülen verileri bir ticaret ortağına gönderebilirsiniz. Kod çözme eyleminden çıktıyı Salesforce 'a veya ticari iş ortağınıza göndermek için [Azure Logic Apps bulunan diğer bağlayıcıları](../connectors/apis-list.md)kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md) hakkında daha fazla bilgi edinin