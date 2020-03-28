---
title: "Öğretici: Azure Bahar Bulutu'nda bir uygulamayı ölçeklendirin | Microsoft Dokümanlar"
description: Bu eğitimde, Azure portalında Azure Yay Bulutu ile bir uygulamayı nasıl ölçeklendirdiğinizi öğrenirsiniz
ms.service: spring-cloud
ms.topic: tutorial
ms.author: brendm
author: bmitchell287
ms.date: 10/06/2019
ms.openlocfilehash: f08a3b5d0a03b0e898457bbb783dd5031c4b0f27
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76277473"
---
# <a name="scale-an-application-in-azure-spring-cloud"></a>Azure Bahar Bulutu'nda bir uygulamayı ölçeklendirin

Bu öğretici, Azure portalındaki Azure Bahar Bulutu panosunu kullanarak herhangi bir mikro hizmet uygulamasının nasıl ölçeklendirilebildiğini gösterir.

Sanal CPU'ların (vCPUs) sayısını ve bellek miktarını değiştirerek uygulamanızı yukarı ve aşağı ölçeklendirin. Uygulama örneklerinin sayısını değiştirerek uygulamanızı ölçeklendirin.

Bitirdikten sonra, hizmetinizdeki her uygulamada hızlı manuel değişiklikler yapmayı öğreneceksiniz. Ölçekleme saniyeler içinde etkinleşir ve herhangi bir kod değişikliği veya yeniden dağıtım gerektirmez.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 
* Dağıtılmış bir Azure İlkbahar Bulutu hizmeti örneği.  Başlamak [için Azure CLI üzerinden bir uygulamayı dağıtmaya hızlı](spring-cloud-quickstart-launch-app-cli.md) bir şekilde başlayalım.
* Hizmet örneğinizde zaten oluşturulmuş en az bir uygulama.

## <a name="navigate-to-the-scale-page-in-the-azure-portal"></a>Azure portalındaki Ölçek sayfasına gidin

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure Bahar Bulutuna **Genel Bakış** sayfanıza gidin.

1. Hizmetinizi içeren kaynak grubunu seçin.

1. Sayfanın sol tarafındaki menüdeki **Ayarlar'ın** altındaki **Uygulamalar** sekmesini seçin.

1. Ölçeklendirmek istediğiniz uygulamayı seçin. Bu örnekte, hesap **hizmeti**adlı uygulamayı seçin. Daha sonra uygulamanın **Genel Bakış** sayfasını görmeniz gerekir.

1. Sayfanın sol tarafındaki menüdeki **Ayarlar** altındaki **Ölçek** sekmesine gidin. Aşağıdaki bölümde gösterilen öznitelikleri ölçekleme için seçenekleri görmeniz gerekir.

## <a name="scale-your-application"></a>Başvurunuzu ölçeklendirin

Ölçekleme özniteliklerini değiştirirseniz, aşağıdaki notları aklınızda bulundurun:

* **CPU'lar**: Uygulama başına en fazla CPU sayısı dörttür. Bir uygulamanın toplam CPU sayısı, burada belirlenen değerin uygulama örneklerinin sayısıyla çarpılmasıdır.

* **Bellek/GB**: Uygulama başına en fazla bellek miktarı 8 GB'dır. Bir uygulama için toplam bellek miktarı burada ayarlanan değerdir uygulama örneklerinin sayısı ile çarpılır.

* **Uygulama örneği sayısı**: Standart katmanda en fazla 20 örnek ekleyebilirsiniz. Bu değer, microservice uygulamasının ayrı çalışan örneklerinin sayısını değiştirir.

Ölçekleme ayarlarınızı uygulamak için **Kaydet'i** seçtiğinizden emin olun.

![Azure portalında Ölçek hizmeti](media/spring-cloud-tutorial-scale-manual/scale-up-out.png)

Birkaç saniye sonra yaptığınız değişiklikler **Genel Bakış** sayfasında görüntülenir ve Uygulama **örnekleri** sekmesinde daha fazla ayrıntı bulunur. Ölçekleme herhangi bir kod değişikliği veya yeniden dağıtım gerektirmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure İlkbahar Bulutuygulamalarınızı el ile nasıl ölçeklendireceklerinizi öğrendiniz. Uygulamanızı nasıl izleyeceğinizi öğrenmek için bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Bir uygulamayı nasıl izleyeceğizi öğrenin](spring-cloud-tutorial-distributed-tracing.md)
