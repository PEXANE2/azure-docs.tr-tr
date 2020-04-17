---
title: Web Hizmet Girişi/Çıktısı
description: Azure Machine Learning tasarımcısındaki web hizmeti modülleri hakkında bilgi edinin (önizleme)
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: be940e3ba693270707e22ffc7b9377dbea1df5f8
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81462093"
---
# <a name="web-service-inputoutput"></a>Web Hizmet Girişi/Çıktısı

Bu makalede, Azure Machine Learning tasarımcısında **Web Hizmeti Giriş** modülü ve Web Hizmeti **Çıktısı** modülü açıklanmaktadır (önizleme).

**Web Hizmeti Giriş** modülü yalnızca **DataFrameDirectory**türü ile giriş noktasına bağlanabilir. Ve **Web Hizmeti Çıktı** modülü yalnızca veri **çerçevesi dizin**türüne sahip çıkış bağlantı noktasından bağlanabilir. İki modül, **Web Hizmeti** kategorisi altında modül ağacında bulunabilir. 

**Web Hizmeti Giriş** modülü, kullanıcı verilerinin ardışık ardışık ardışık alt ekime nerede girdiğini belirtmek için kullanılır ve **Web Hizmeti Çıktımodülü,** kullanıcı verilerinin gerçek zamanlı çıkarım ardışık bir şekilde nerede döndürülür olduğunu belirtmek için kullanılır.

## <a name="how-to-use-web-service-inputoutput"></a>Web Hizmeti Girişi/Çıktısı nasıl kullanılır?

- Eğitim ardışık ardışık bilgisayarınızdan gerçek zamanlı bir çıkarım ardışık ardışık kaynak oluşturduğunuzda, kullanıcı verilerinin ardışık ardışık duruma nerede girdiğini ve verilerin nerede döndürüleceğini göstermek için **Web Hizmeti Girişi** ve Web Hizmeti **Çıktısı** modülü otomatik olarak eklenir. 

    [Gerçek zamanlı çıkarım ardışık bir yol oluşturma](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#create-a-real-time-inference-pipeline)hakkında daha fazla bilgi edinin.

    > [!NOTE]
    > Otomatik olarak gerçek zamanlı çıkarım boru hattı üreten bir kural tabanlı en iyi çaba sürecidir, doğruluk için hiçbir garanti yoktur. Gereksinimlerinizi karşılamak için **Web Hizmeti Giriş/Çıkış** modüllerini el ile ekleyebilir veya kaldırabilirsiniz. Gerçek zamanlı çıkarım ardışık alanınızda en az bir **Web Hizmeti Giriş** modülü ve bir Web Hizmeti **Çıktı** modülü olduğundan emin olun. Birden çok **Web Hizmeti Girişi** veya **Web Hizmeti Çıktımodülü** varsa, modülün doğru paneline adı girinebileceğiniz benzersiz adlara sahip olduğundan emin olun.

- Ayrıca, teslim edilmemiş ardışık ardınıza Web **Hizmeti Girişi** ve **Web Hizmeti Çıktısı** modülleri ekleyerek gerçek zamanlı bir çıkarım ardışık

    > [!NOTE]
    >  Boru hattı türü, ilk gönderdiğiniz anda belirlenir. Bu nedenle, gerçek zamanlı bir çıkarım ardışık nokta oluşturmak istiyorsanız, ilk kez göndermeden önce **Web Hizmet Girişi** ve Web Hizmeti **Çıktısı** modüllerini eklediğinizden emin olun.

   Aşağıdaki örnek, **Execute Python Script** modülünden gerçek zamanlı çıkarım ardışık hattının nasıl oluşturulurunu gösterir. 

   ![Örnek](media/module/web-service-input-output-example.png)
   
   Ardışık hattı gönderdikten ve çalıştırma başarıyla tamamlandıktan sonra, gerçek zamanlı bitiş noktasını dağıtabilirsiniz.
   
   > [!NOTE]
   >  Yukarıdaki örnekte, **Veri Girin el ile** web hizmeti girişi için veri şema sağlar ve gerçek zamanlı bitiş noktasını dağıtmak için gereklidir. Genellikle, veri şemasını sağlamak için web hizmeti **girişinin** bağlı olduğu bağlantı noktasına her zaman bir modül veya veri kümesi bağlamanız gerekir.
   
## <a name="next-steps"></a>Sonraki adımlar
[Gerçek zamanlı bitiş noktasını dağıtma](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#deploy-the-real-time-endpoint)hakkında daha fazla bilgi edinin.
Azure Machine Learning için [kullanılabilen modül ler kümesine](module-reference.md) bakın.