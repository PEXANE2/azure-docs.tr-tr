---
title: "Öğretici: Azure Spring Cloud 'da bir uygulamayı ölçeklendirme | Microsoft Docs"
description: Bu öğreticide, Azure portal Azure Spring Cloud ile bir uygulamayı nasıl ölçeklentireceğinizi öğreneceksiniz
ms.service: spring-cloud
ms.topic: tutorial
ms.author: jeconnoc
author: jpconnock
ms.date: 10/06/2019
ms.openlocfilehash: eaf7e7ec39555e5b933020835f3bb96429e3aa81
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75461423"
---
# <a name="scale-an-application-in-azure-spring-cloud"></a>Azure Spring Cloud 'da bir uygulamayı ölçeklendirme

Bu öğreticide, Azure portal Azure Spring Cloud Panosu kullanılarak herhangi bir mikro hizmet uygulamasının nasıl ölçeklendirileceğini gösterilmektedir.

Sanal CPU (vCPU) sayısını ve bellek miktarını değiştirerek uygulamanızı yukarı ve aşağı ölçeklendirin. Uygulama örneklerinin sayısını değiştirerek uygulamanızı içinde ve dışarı ölçeklendirin.

İşlemi tamamladıktan sonra, hizmetinizdeki her uygulamada hızlı bir şekilde değişiklik yapmayı öğrenirsiniz. Ölçeklendirme, saniyeler içinde etkili olur ve herhangi bir kod değişikliği veya yeniden dağıtım gerektirmez.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
* Dağıtılmış bir Azure yay bulut hizmeti örneği.  Kullanmaya başlamak için [Azure CLI aracılığıyla uygulama dağıtmaya yönelik hızlı başlangıç hızımızı](spring-cloud-quickstart-launch-app-cli.md) izleyin.
* Hizmet örneğiniz için en az bir uygulama zaten oluşturulmuş.

## <a name="navigate-to-the-scale-page-in-the-azure-portal"></a>Azure portal ölçek sayfasına gidin

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

1. Azure yay bulutuna **genel bakış** sayfasına gidin.

1. Hizmetinizi içeren kaynak grubunu seçin.

1. Sayfanın sol tarafındaki menüde **Ayarlar** ' ın altında **uygulamalar** sekmesini seçin.

1. Ölçeklendirmek istediğiniz uygulamayı seçin. Bu örnekte, **Hesap-hizmet**adlı uygulamayı seçin. Daha sonra uygulamanın **genel bakış** sayfasını görmeniz gerekir.

1. Sayfanın sol tarafındaki menüde bulunan **Ayarlar** ' ın altındaki **Ölçek** sekmesine gidin. Aşağıdaki bölümde gösterilen özniteliklerin ölçeklendirilmesine yönelik seçenekleri görmeniz gerekir.

## <a name="scale-your-application"></a>Uygulamanızı ölçeklendirme

Ölçeklendirme özniteliklerini değiştirirseniz aşağıdaki notları aklınızda bulundurun:

* **CPU 'lar**: uygulama örneği başına en fazla CPU sayısı dördü. Bir uygulama için toplam CPU sayısı, burada ayarlanan değer, uygulama örneklerinin sayısıyla çarpılır.

* **Bellek/GB**: uygulama örneği başına en fazla bellek MIKTARı 8 GB 'dir. Bir uygulama için toplam bellek miktarı, burada ayarlanan değer, uygulama örneklerinin sayısıyla çarpılır.

* **Uygulama örneği sayısı**: Standart katmanda, en fazla 20 örneğe ölçeklendirebilirsiniz. Bu değer, mikro hizmet uygulamasının ayrı çalışan örneklerinin sayısını değiştirir.

Ölçeklendirme ayarlarınızı uygulamak için **Kaydet** ' i seçtiğinizden emin olun.

![Azure portal ölçek hizmeti](media/spring-cloud-tutorial-scale-manual/scale-up-out.png)

Birkaç saniye sonra, yaptığınız değişiklikler, **uygulama örnekleri** sekmesinde daha fazla ayrıntı Ile **genel bakış** sayfasında görüntülenir. ölçeklendirme, herhangi bir kod değişikliği veya yeniden dağıtım gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Spring Cloud uygulamalarınızı el ile ölçeklendirmenizi öğrendiniz. Uygulamanızı nasıl izleyeceğinizi öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Bir uygulamayı izlemeyi öğrenin](spring-cloud-tutorial-distributed-tracing.md)
