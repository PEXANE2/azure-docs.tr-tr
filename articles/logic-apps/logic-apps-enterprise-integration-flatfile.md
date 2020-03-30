---
title: Düz dosyaları kodlama veya çözme
description: Azure Logic Apps ve Kurumsal Tümleştirme Paketi ile kurumsal tümleştirme için düz dosyaları kodlama veya çözme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 07/08/2016
ms.openlocfilehash: 2d6182ba01507c2fb361628e01bb52e1ea821f44
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152661"
---
# <a name="encode-or-decode-flat-files-with-azure-logic-apps-and-enterprise-integration-pack"></a>Azure Logic Apps ve Kurumsal Tümleştirme Paketi ile düz dosyaları kodlama veya çözme

XML içeriğini işletmeden işletmeye (B2B) bir senaryoda bir iş ortağına göndermeden önce kodlamak isteyebilirsiniz. Bir mantık uygulamasında, bunu yapmak için düz dosya kodlama konektörünü kullanabilirsiniz. Oluşturduğunuz mantık uygulaması, XML içeriğini bir HTTP istek tetikleyicisi, başka bir uygulamadan veya hatta birçok [bağlayıcıdan](../connectors/apis-list.md)biri de dahil olmak üzere çeşitli kaynaklardan alabilir. Mantık uygulamaları hakkında daha fazla bilgi [için, mantık uygulamaları dokümantasyon](logic-apps-overview.md "Logic uygulamaları hakkında daha fazla bilgi edinin")bakın.  

## <a name="create-the-flat-file-encoding-connector"></a>Düz dosya kodlama konektörünü oluşturma
Mantık uygulamanıza düz dosya kodlama bağlayıcısı eklemek için aşağıdaki adımları izleyin.

1. Bir mantık uygulaması oluşturun ve [tümleştirme hesabınıza bağla.](logic-apps-enterprise-integration-accounts.md "Bir tümleştirme hesabını Bir Mantık uygulamasına bağlamayı öğrenin") Bu hesap, XML verilerini kodlamak için kullanacağınız şemayı içerir.  

1. Logic App Designer'da, mantık uygulamanıza **BIR HTTP isteği geldiğinde** tetikleyiciyi ekleyin.

1. Aşağıdaki gibi düz dosya kodlama eylemi ekleyin:

   a. **Artı** işaretini seçin.

   b. Eylem **ekle** bağlantısını seçin (artı işaretini seçtikten sonra görüntülenir).

   c. Arama kutusunda, tüm eylemleri kullanmak istediğiniz eyleme filtrelemek için *Düz'e* girin.

   d. Listeden **Düz Dosya Kodlama** seçeneğini seçin.   

      ![Düz Dosya Kodlama seçeneğinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   

1. Düz **Dosya Kodlama** iletişim kutusunda **İçerik** metin kutusunu seçin.  

   ![İçerik metin kutusunun ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  

1. Kodlamak istediğiniz içerik olarak gövde etiketini seçin. Gövde etiketi içerik alanını dolduracaktır.     

   ![Gövde etiketinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  

1. **Şema Adı** listesi kutusunu seçin ve giriş içeriğini kodlamak için kullanmak istediğiniz şemayı seçin.    

   ![Şema Adı liste kutusunun ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  

1. Çalışmanızı kaydedin.

   ![Kaydet simgesinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Bu noktada, düz dosya kodlama konektörünü ayarlamayı bitirdiniz. Gerçek bir uygulamada, kodlanmış verileri Salesforce gibi bir iş satırı uygulamasında depolamak isteyebilirsiniz. Ya da bu kodlanmış verileri bir ticaret ortağına gönderebilirsiniz. Kodlama eyleminin çıktısını Salesforce'a veya işlem ortağınıza, sağlanan diğer konektörlerden herhangi birini kullanarak kolayca gönderebilirsiniz.

Artık https://yada, HTTP bitiş noktasına bir istekte bulunarak ve isteğin gövdesine XML içeriğini de ekleyerek bağlayıcınızı test edebilirsiniz.  

## <a name="create-the-flat-file-decoding-connector"></a>Düz dosya kod çözme konektörü oluşturma

> [!NOTE]
> Bu adımları tamamlamak için, bir şema dosyasının zaten tümleştirme hesabınıza yüklenmiş olması gerekir.

1. Logic App Designer'da, mantık uygulamanıza **BIR HTTP isteği geldiğinde** tetikleyiciyi ekleyin.

1. Aşağıdaki gibi düz dosya kod çözme eylemi ekleyin:

   a. **Artı** işaretini seçin.

   b. Eylem **ekle** bağlantısını seçin (artı işaretini seçtikten sonra görüntülenir).

   c. Arama kutusunda, tüm eylemleri kullanmak istediğiniz eyleme filtrelemek için *Düz'e* girin.

   d. Listeden **Düz Dosya Çözme** seçeneğini seçin.   

      ![Düz Dosya Çözme seçeneğinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   

1. **İçerik** denetimini seçin. Bu, kodu çözmek için içerik olarak kullanabileceğiniz önceki adımlardaki içeriğin bir listesini üretir. Gelen HTTP isteğinden *Gövde'nin* kodu çözmek için içerik olarak kullanılabileceğini unutmayın. Doğrudan **İçerik** denetimine şifresini çözmek için içeriği de girebilirsiniz.     

1. *Vücut* etiketini seçin. Gövde etiketinin artık **İçerik** denetiminde olduğuna dikkat edin.

1. İçeriği çözmek için kullanmak istediğiniz şema adını seçin. Aşağıdaki ekran *görüntüsü, OrderFile'ın* seçilen şema adı olduğunu gösterir. Bu şema adı daha önce entegrasyon hesabına yüklenmiş.

   ![Düz Dosya Çözme iletişim kutusunun ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png) 

1. Çalışmanızı kaydedin.  

   ![Kaydet simgesinin ekran görüntüsü](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Bu noktada, düz dosya kod çözme bağlayıcınızı ayarlamayı bitirdiniz. Gerçek bir uygulamada, kodlanmış verileri Salesforce gibi bir iş hattı uygulamasında depolamak isteyebilirsiniz. Kod çözme eyleminin çıktısını Salesforce'a göndermek için kolayca bir eylem ekleyebilirsiniz.

Artık https://rıddamak için http uç noktasına istekte bulunarak ve isteğin gövdesinde deşifre etmek istediğiniz XML içeriğini de dahil ederek konektörünüzü test edebilirsiniz.  

## <a name="next-steps"></a>Sonraki adımlar
* [Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin.](logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin")  

