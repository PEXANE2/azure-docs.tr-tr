---
title: Düz dosyaları kodla veya kodunu çözün
description: Azure Logic Apps ve Enterprise Integration Pack ile kurumsal tümleştirme için düz dosyaları kodlayın veya kodu çözün
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 07/08/2016
ms.openlocfilehash: 2d6182ba01507c2fb361628e01bb52e1ea821f44
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152661"
---
# <a name="encode-or-decode-flat-files-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Enterprise Integration Pack düz dosyaları kodla veya kodunu çözün

XML içeriğini işletmeden işletmeye (B2B) senaryosundaki bir iş ortağına göndermeden önce kodlamak isteyebilirsiniz. Bir mantıksal uygulamada, bunu yapmak için düz dosya kodlama bağlayıcısını kullanabilirsiniz. Oluşturduğunuz mantıksal uygulama, bir HTTP istek tetikleyicisinden, başka bir uygulamadan veya birçok [bağlayıcıdan](../connectors/apis-list.md)birinden dahil olmak üzere çeşitli kaynaklardan gelen XML içeriğini alabilir. Logic Apps hakkında daha fazla bilgi için bkz. [Logic Apps belgeleri](logic-apps-overview.md "Logic Apps hakkında daha fazla bilgi").  

## <a name="create-the-flat-file-encoding-connector"></a>Düz dosya kodlama bağlayıcısını oluşturma
Mantıksal uygulamanıza düz bir dosya kodlama Bağlayıcısı eklemek için bu adımları izleyin.

1. Bir mantıksal uygulama oluşturun ve [bunu tümleştirme hesabınıza bağlayın](logic-apps-enterprise-integration-accounts.md "Tümleştirme hesabını bir Logic App 'e bağlamayı öğrenin"). Bu hesap, XML verilerini kodlamak için kullanacağınız şemayı içerir.  

1. Mantıksal uygulama Tasarımcısı ' nda, mantıksal uygulamanıza **BIR http isteği alındığında** tetikleyiciyi ekleyin.

1. Düz dosya kodlama eylemini aşağıdaki gibi ekleyin:

   a. **Artı** işaretini seçin.

   b. **Eylem Ekle** bağlantısını seçin (artı işaretini seçtikten sonra görünür).

   c. Kullanmak istediğiniz tüm eylemleri filtrelemek için arama kutusuna *düz* ' i girin.

   d. Listeden **düz dosya kodlama** seçeneğini belirleyin.   

      ![Düz dosya kodlama seçeneğinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   

1. **Düz dosya kodlama** Iletişim kutusunda **içerik** metin kutusunu seçin.  

   ![Içerik metin kutusunun ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  

1. Kodlamak istediğiniz içerik olarak Body etiketini seçin. Body etiketi, içerik alanını dolduracaktır.     

   ![Gövde etiketinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  

1. **Şema adı** liste kutusunu seçin ve giriş içeriğini kodlamak için kullanmak istediğiniz şemayı seçin.    

   ![Şema adı liste kutusunun ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  

1. Çalışmanızı kaydedin.

   ![Kaydet simgesinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Bu noktada, düz dosya kodlama bağlayıcınızı ayarlamayı tamamladınız. Gerçek bir dünya uygulamasında, kodlanmış verileri Salesforce gibi bir iş kolu uygulamasında depolamak isteyebilirsiniz. Ya da bu kodlanmış verileri bir ticaret ortağına gönderebilirsiniz. Kodlama eyleminin çıkışını, sunulan diğer bağlayıcılardan birini kullanarak Salesforce 'a veya ticari iş ortağınıza göndermek için kolayca bir eylem ekleyebilirsiniz.

Artık, HTTP uç noktasına bir istek yaparak ve isteğin gövdesindeki XML içeriğini de ekleyerek bağlayıcınızı test edebilirsiniz.  

## <a name="create-the-flat-file-decoding-connector"></a>Düz dosya kod çözme bağlayıcısını oluşturma

> [!NOTE]
> Bu adımları tamamlayabilmeniz için, tümleştirme hesabınıza önceden yüklenmiş bir şema dosyası olması gerekir.

1. Mantıksal uygulama Tasarımcısı ' nda, mantıksal uygulamanıza **BIR http isteği alındığında** tetikleyiciyi ekleyin.

1. Düz dosya kod çözme eylemini aşağıdaki gibi ekleyin:

   a. **Artı** işaretini seçin.

   b. **Eylem Ekle** bağlantısını seçin (artı işaretini seçtikten sonra görünür).

   c. Kullanmak istediğiniz tüm eylemleri filtrelemek için arama kutusuna *düz* ' i girin.

   d. Listeden **düz dosya kod çözme** seçeneğini belirleyin.   

      ![Düz dosya kod çözme seçeneğinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   

1. **İçerik** denetimini seçin. Bu, kodunu çözecek içerik olarak kullanabileceğiniz önceki adımlardan içerik listesini oluşturur. Gelen HTTP isteğindeki *gövdenin* , kodunu çözecek içerik olarak kullanılabilir olduğunu unutmayın. **İçerik** denetimine doğrudan kod çözme için içerik de girebilirsiniz.     

1. *Gövde* etiketini seçin. Gövde etiketinin şimdi **içerik** denetiminde olduğuna dikkat edin.

1. İçeriğin kodunu çözmek için kullanmak istediğiniz şemanın adını seçin. Aşağıdaki ekran görüntüsünde, *Orderfile* seçili şema adı gösterilmektedir. Bu şema adı daha önce tümleştirme hesabına yüklendi.

   ![Düz dosya kod çözme iletişim kutusunun ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png) 

1. Çalışmanızı kaydedin.  

   ![Kaydet simgesinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Bu noktada, düz dosya kod çözme bağlayıcınızı ayarlamayı tamamladınız. Gerçek bir dünya uygulamasında, kodu çözülen verileri Salesforce gibi bir iş kolu uygulamasında depolamak isteyebilirsiniz. Kod çözme eyleminin çıkışını Salesforce 'a göndermek için kolayca bir eylem ekleyebilirsiniz.

Artık, HTTP uç noktasına bir istek yaparak ve isteğin gövdesinde kodunu çözmek istediğiniz XML içeriğini ekleyerek bağlayıcınızı test edebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar
* [Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin").  

