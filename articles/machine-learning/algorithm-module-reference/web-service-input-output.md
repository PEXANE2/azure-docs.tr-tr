---
title: 'Web hizmeti giriş/çıkış: modül başvurusu'
description: Azure Machine Learning tasarımcısında Web hizmeti modülleri hakkında bilgi edinin
titleSuffix: Azure Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 04/13/2020
ms.openlocfilehash: a62f8aee0bd0a0d2b7009a48e9d5f00ea3c5155f
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90883218"
---
# <a name="web-service-input-and-web-service-output-modules"></a>Web hizmeti giriş ve Web hizmeti çıkış modülleri

Bu makalede, Azure Machine Learning tasarımcısında Web hizmeti giriş ve Web hizmeti çıkış modülleri açıklanmaktadır.

Web hizmeti giriş modülü yalnızca **Dataframedirectory**türünde bir giriş bağlantı noktasına bağlanabilir. Web hizmeti çıkış modülü yalnızca **Dataframedirectory**türünde bir çıkış bağlantı noktasından bağlanabilir. Modül ağacında, **Web hizmeti** kategorisi altında iki modülü bulabilirsiniz. 

Web hizmeti giriş modülü, Kullanıcı verilerinin işlem hattına girdiği yeri gösterir. Web hizmeti çıkış modülü, Kullanıcı verilerinin gerçek zamanlı bir çıkarım ardışık düzeninde döndürüldüğü yeri gösterir.

## <a name="how-to-use-web-service-input-and-output"></a>Web hizmeti girişi ve çıkışı nasıl kullanılır

Eğitim işlem hattınızdan [gerçek zamanlı bir çıkarım işlem hattı oluşturduğunuzda](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#create-a-real-time-inference-pipeline) , Web hizmeti giriş ve Web hizmeti çıkış modülleri, Kullanıcı verilerinin ardışık düzene girdiği ve verilerin döndürüldüğü yeri göstermek için otomatik olarak eklenir. 

> [!NOTE]
> Gerçek zamanlı çıkarım işlem hattını otomatik olarak oluşturmak, kural tabanlı ve en iyi çaba bir işlemdir. Doğruluk garantisi yoktur. 

Gereksinimlerinizi karşılamak için Web hizmeti giriş ve Web hizmeti çıkış modüllerini el ile ekleyebilir veya kaldırabilirsiniz. Gerçek zamanlı çıkarım işlem hattının en az bir Web hizmeti giriş modülüne ve bir Web hizmeti çıkış modülüne sahip olduğundan emin olun. Birden çok Web hizmeti girişi veya Web hizmeti çıkış modülünüz varsa, benzersiz adlara sahip olduklarından emin olun. Modülün sağ paneline adını girebilirsiniz.

Ayrıca, gönderilmemiş işlem hattınızı Web hizmeti girişi ve Web hizmeti çıkış modüllerini ekleyerek el ile gerçek zamanlı bir çıkarım işlem hattı oluşturabilirsiniz.

> [!NOTE]
> İşlem hattı türü ilk kez gönderdiğinizde belirlenir. İlk kez göndermeden önce Web hizmeti giriş ve Web hizmeti çıkış modüllerini eklediğinizden emin olun.

Aşağıdaki örnek, Python betik modülünden nasıl el ile gerçek zamanlı çıkarım işlem hattı oluşturulacağını göstermektedir. 

![Örnek](media/module/web-service-input-output-example.png)
   
İşlem hattını gönderdikten ve çalıştırma başarılı bir şekilde tamamlandıktan sonra gerçek zamanlı uç noktayı dağıtabilirsiniz.
   
> [!NOTE]
>  Yukarıdaki örnekte, **verileri el ile** Web hizmeti girişi için veri şeması sağlar ve gerçek zamanlı uç noktanın dağıtımı için gereklidir. Genellikle, veri şemasını sağlamak üzere **Web hizmeti girişinin** bağlandığı bağlantı noktasına her zaman bir modül veya veri kümesi bağlamanız gerekir.
   
## <a name="next-steps"></a>Sonraki adımlar
[Gerçek zamanlı uç noktayı dağıtma](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy#deploy-the-real-time-endpoint)hakkında daha fazla bilgi edinin.

Azure Machine Learning için [kullanılabilen modül kümesine](module-reference.md) bakın.