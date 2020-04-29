---
title: Web Hizmeti Girişi/Çıkışı
description: Azure Machine Learning tasarımcısında Web hizmeti modülleri hakkında bilgi edinin (Önizleme)
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: be940e3ba693270707e22ffc7b9377dbea1df5f8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81462093"
---
# <a name="web-service-inputoutput"></a>Web Hizmeti Girişi/Çıkışı

Bu makalede, Azure Machine Learning tasarımcısında **Web hizmeti giriş** modülü ve **Web hizmeti çıkış** modülü (Önizleme) açıklanmaktadır.

**Web hizmeti giriş** modülü, yalnızca **dataframedirectory**türündeki giriş bağlantı noktasına bağlanabilir. Ve **Web hizmeti çıkış** modülü, yalnızca **dataframedirectory**türündeki çıkış bağlantı noktasından bağlanabilir. İki modül, **Web hizmeti** kategorisi altında modül ağacında bulunabilir. 

**Web hizmeti giriş** modülü, Kullanıcı verilerinin ardışık düzen ve **Web hizmeti çıkış** modülüne nereye girdiğini belirtmek için kullanılır ve Kullanıcı verilerinin gerçek zamanlı bir çıkarım ardışık düzeninde döndürülüp döndürülmeyeceğini gösterir.

## <a name="how-to-use-web-service-inputoutput"></a>Web hizmeti giriş/çıkış kullanma

- Eğitim işlem hattınızdan gerçek zamanlı bir çıkarım işlem hattı oluşturduğunuzda, Kullanıcı verilerinin işlem hattına girdiğinde ve verilerin döndürüldüğü yeri göstermek için **Web hizmeti giriş** ve **Web hizmeti çıkış** modülü otomatik olarak eklenir. 

    [Gerçek zamanlı bir çıkarım işlem hattı oluşturma](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#create-a-real-time-inference-pipeline)hakkında daha fazla bilgi edinin.

    > [!NOTE]
    > Gerçek zamanlı çıkarım ardışık düzenini otomatik olarak oluşturmak, kural tabanlı bir en iyi çaba işlemidir; doğruluk açısından garanti yoktur. Gereksinimlerinizi karşılamak için **Web hizmeti giriş/çıkış** modüllerini el ile ekleyebilir veya kaldırabilirsiniz. Gerçek zamanlı çıkarım ardışık düzeninde en az bir **Web hizmeti giriş** modülünün ve bir **Web hizmeti çıkış** modülünün bulunduğundan emin olun. Birden çok **Web hizmeti girişi** veya **Web hizmeti çıkış** modülünüz varsa, modülün sağ panelinde adı girmek için benzersiz adlara sahip olduklarından emin olun.

- Ayrıca, gönderilmemiş işlem hattınızı **Web hizmeti girişi** ve **Web hizmeti çıkış** modüllerini ekleyerek el ile gerçek zamanlı bir çıkarım işlem hattı oluşturabilirsiniz.

    > [!NOTE]
    >  İşlem hattı türü ilk kez gönderdiğinizde belirlenir. Bu nedenle, gerçek zamanlı bir çıkarım işlem hattı oluşturmak istiyorsanız ilk kez göndermeden önce **Web hizmeti girişi** ve **Web hizmeti çıkış** modülünü eklediğinizden emin olun.

   Aşağıdaki örnek, **Python betik** modülünden nasıl el ile gerçek zamanlı çıkarım işlem hattı oluşturulacağını göstermektedir. 

   ![Örnek](media/module/web-service-input-output-example.png)
   
   İşlem hattını gönderdikten ve çalıştırma başarıyla tamamlandıktan sonra gerçek zamanlı uç noktayı dağıtabileceksiniz.
   
   > [!NOTE]
   >  Yukarıdaki örnekte, **verileri el ile** Web hizmeti girişi için veri şeması sağlar ve gerçek zamanlı uç noktanın dağıtımı için gereklidir. Genellikle, veri şemasını sağlamak üzere **Web hizmeti girişinin** bağlı olduğu bağlantı noktasına bir modül veya veri kümesini her zaman bağlamanız gerekir.
   
## <a name="next-steps"></a>Sonraki adımlar
[Gerçek zamanlı uç noktayı dağıtma](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#deploy-the-real-time-endpoint)hakkında daha fazla bilgi edinin.
Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın.