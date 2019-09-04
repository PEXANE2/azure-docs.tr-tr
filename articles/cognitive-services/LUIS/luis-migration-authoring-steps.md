---
title: Azure yazma kaynağına geçiş
titleSuffix: Azure Cognitive Services
description: Bir Azure yazma kaynağına geçiş yapın.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/02/2019
ms.author: diberry
ms.openlocfilehash: 13eb301daadb70456c0c0d7db8991d7a695b1de9
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70259689"
---
# <a name="steps-to-migrate-to-the-azure-authoring-resource"></a>Azure yazma kaynağına geçiş adımları

Language Understanding (LUSıS) portalından, Azure yazma kaynağını kullanmak için sahip olduğunuz tüm uygulamaları geçirin.

## <a name="prerequisites"></a>Önkoşullar

* **Isteğe bağlı**olarak, her uygulamayı dışarı aktararak ve dışarı aktarma [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)'sini kullanarak, bu uygulamaları lusıs portalının uygulamalar listesinden yedekleyin.
* **Isteğe bağlı**olarak, her bir uygulamanın ortak kayıt listesini kaydedin. Bu e-posta listesi, geçiş işleminin bir parçası olarak sağlanır.
* **Gerekli**, bir [Azure aboneliğinizin](https://azure.microsoft.com/free/)olması gerekir. Abonelik işleminin bir parçası faturalandırma bilgileri gerektirir. Ancak, LUSıS kullandığınızda ücretsiz (F0) fiyatlandırma katmanlarını kullanabilirsiniz. Kullanımınız arttıkça ücretli bir katmana ihtiyacınız olduğunu fark edebilirsiniz. 

Azure aboneliğiniz yoksa [kaydolun](https://azure.microsoft.com/free/). 

## <a name="access-the-migration-process"></a>Geçiş işlemine erişin

Haftalık olarak, uygulamalarınızı geçirmeniz istenir. Bu pencereyi geçirmeden iptal edebilirsiniz. Bir sonraki zamanlanan dönemden önce geçiş yapmak istiyorsanız, HALU portalının üst araç çubuğundaki **kilit** simgesine geçiş işlemini başlatabilirsiniz. 

## <a name="app-owner-begins-the-migration-process"></a>Uygulama sahibi geçiş işlemini başlatır

Herhangi bir LUO uygulama sahibiyseniz, geçiş işlemi kullanılabilir. 

1. 1. [Luo portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul edin.
1. Geçiş açılır penceresi geçişe devam etmenize veya daha sonra geçiş yapmanıza olanak sağlar. **Şimdi geçir**' i seçin. Daha sonra geçirmeyi seçerseniz, Azure 'daki yeni yazma anahtarına geçiş yapmak için 9 ay daha vardır.

    ![Geçiş işlemindeki ilk açılan pencere, şimdi geçir ' i seçin.](./media/migrate-authoring-key/migrate-now.png)

1. Uygulamalarınızdan herhangi birinde ortak çalışanlar varsa, geçiş hakkında bilgi veren **bir e-posta göndermeniz** istenir. Bu isteğe bağlı bir adımdır. Varsayılan e-posta uygulaması, hafif biçimli bir e-posta ile açılır. 

    ![Geçiş hakkında ortak çalışan e-postası gönder](./media/migrate-authoring-key/send-collaborators-email.png)

1. **Uygulamalarınızı geçirmek için bir yazma kaynağı oluşturarak Başlat**' ı seçerek bir lusıs yazma kaynağı oluşturmayı seçin. 

    ![Yazma kaynağı oluşturma](./media/migrate-authoring-key/choose-authoring-resource.png)

1. Sonraki pencerede, kaynak anahtarı bilgilerinizi girin. Bilgileri girdikten sonra **kaynak oluştur**' u seçin. 

    ![Yazma kaynağı oluşturma](./media/migrate-authoring-key/choose-authoring-resource-form.png)

    **Yeni bir yazma kaynağı oluştururken**aşağıdaki bilgileri sağlayın: 

    * **Kaynak adı** -seçtiğiniz özel bir ad, yazma ve tahmin uç noktası sorgularınız için URL 'nin bir parçası olarak kullanılır.
    * **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı. 
    * **Abonelik adı** -kaynak için faturalandırılacak abonelik.
    * **Kaynak grubu** -seçtiğiniz veya oluşturduğunuz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır. 
    * **Konum** -konum seçimi, **kaynak grubu** seçimini temel alır.
    * **Fiyatlandırma katmanı** -fiyatlandırma katmanı, saniye başına en fazla işlemi ve ayı belirler. 

1. Yazma kaynağı oluşturulduğunda, başarı iletisi gösterilir. Açılır pencereyi kapatmak için **Kapat** ' ı seçin.

    ![Yazma kaynağınız başarıyla oluşturuldu.](./media/migrate-authoring-key/migration-success.png)

    **Uygulamalarım** listesinde, yeni yazma kaynağına geçirilen uygulamalar gösterilir. 

    Uygulamalarınızı LUE portalında düzenlemeyle çalışmaya devam etmek için yazma kaynağının anahtarını bilmeniz gerekmez. Uygulamalarınızı programlama yoluyla düzenlemeyi planlıyorsanız, yazma anahtarı değerleri gerekir. Bu değerler, Lua portalındaki **Manage-> Azure kaynakları** sayfasında görüntülenir ve ayrıca kaynağın **anahtarlar** sayfasındaki Azure Portal de mevcuttur.  

## <a name="after-the-migration-process-add-contributors-to-your-authoring-resource"></a>Geçiş işleminden sonra, yazma kaynağına katkıda bulunanlar ekleyin

[!INCLUDE [Manage contributors for the Azure authoring resource for language understanding](./includes/manage-contributors-authoring-resource.md)]

[Katkıda bulunanlar eklemeyi](luis-how-to-collaborate.md)öğrenin. 

## <a name="next-steps"></a>Sonraki adımlar


* Yazma ve çalışma zamanı anahtarları hakkındaki [kavramları](luis-concept-keys.md) gözden geçirin
* [Anahtar atamayı](luis-how-to-azure-subscription.md) ve [katkıda bulunanlar](luis-how-to-collaborate.md) eklemeyi inceleyin
