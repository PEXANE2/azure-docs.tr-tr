---
title: "Öğretici: Azure Spring Cloud 'da bir uygulamayı ölçeklendirme | Microsoft Docs"
description: Azure portal Azure Spring Cloud ile uygulama ölçeklendirmeyi öğrenin
ms.service: spring-cloud
ms.topic: tutorial
ms.author: brendm
author: bmitchell287
ms.date: 10/06/2019
ms.custom: devx-track-java
ms.openlocfilehash: c3c660c5e301081ee82a12275b6e9125f0fc781b
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89289826"
---
# <a name="tutorial-scale-an-application-in-azure-spring-cloud"></a>Öğretici: Azure Spring Cloud 'da bir uygulamayı ölçeklendirme

Bu öğreticide, Azure portal Azure Spring Cloud Panosu kullanılarak herhangi bir mikro hizmet uygulamasının nasıl ölçeklendirileceğini gösterilmektedir.

Sanal CPU (vCPU) sayısını ve bellek miktarını değiştirerek uygulamanızı yukarı ve aşağı ölçeklendirin. Uygulama örneklerinin sayısını değiştirerek uygulamanızı içinde ve dışarı ölçeklendirin.

İşlemi tamamladıktan sonra, hizmetinizdeki her uygulamada hızlı bir şekilde değişiklik yapmayı öğrenirsiniz. Ölçeklendirme, saniyeler içinde etkili olur ve herhangi bir kod değişikliği veya yeniden dağıtım gerektirmez.

## <a name="prerequisites"></a>Ön koşullar

Bu yordamları izlemek için şunlar gerekir:

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
* Dağıtılmış bir Azure yay bulut hizmeti örneği.  Kullanmaya başlamak için [Azure CLI aracılığıyla uygulama dağıtma hızlı](spring-cloud-quickstart.md) başlangıcını izleyin.
* Hizmet örneğiniz için en az bir uygulama zaten oluşturulmuş.

## <a name="navigate-to-the-scale-page-in-the-azure-portal"></a>Azure portal ölçek sayfasına gidin

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Azure yay bulutuna **genel bakış** sayfasına gidin.

1. Hizmetinizi içeren kaynak grubunu seçin.

1. Sayfanın sol tarafındaki menüde **Ayarlar** ' ın altında **uygulamalar** sekmesini seçin.

1. Ölçeklendirmek istediğiniz uygulamayı seçin. Bu örnekte, **Hesap-hizmet**adlı uygulamayı seçin. Daha sonra uygulamanın **genel bakış** sayfasını görmeniz gerekir.

1. Sayfanın sol tarafındaki menüde bulunan **Ayarlar** ' ın altındaki **Ölçek** sekmesine gidin. Aşağıdaki bölümde gösterilen özniteliklerin ölçeklendirilmesine yönelik seçenekleri görmeniz gerekir.

## <a name="scale-your-application"></a>Uygulamanızı ölçeklendirin

Ölçeklendirme özniteliklerini değiştirirseniz aşağıdaki notları aklınızda bulundurun:

* **CPU 'lar**: uygulama örneği başına en fazla CPU sayısı dördü. Bir uygulama için toplam CPU sayısı, burada ayarlanan değer, uygulama örneklerinin sayısıyla çarpılır.

* **Bellek/GB**: uygulama örneği başına en fazla bellek MIKTARı 8 GB 'dir. Bir uygulama için toplam bellek miktarı, burada ayarlanan değer, uygulama örneklerinin sayısıyla çarpılır.

* **Uygulama örneği sayısı**: Standart katmanda, en fazla 20 örneğe ölçeklendirebilirsiniz. Bu değer, mikro hizmet uygulamasının ayrı çalışan örneklerinin sayısını değiştirir.

Ölçeklendirme ayarlarınızı uygulamak için **Kaydet** ' i seçtiğinizden emin olun.

![Azure portal ölçek hizmeti](media/spring-cloud-tutorial-scale-manual/scale-up-out.png)

Birkaç saniye sonra, yaptığınız değişiklikler, **genel bakış** sayfasında, daha fazla ayrıntı için **uygulama örnekleri** sekmesinde görüntülenir. Ölçeklendirme, herhangi bir kod değişikliği veya yeniden dağıtım gerektirmez.

## <a name="upgrade-to-the-standard-tier"></a>Standart katmana yükseltme
Temel katmandadır ve bu [limitlerden](spring-cloud-quotas.md)biri veya daha fazlası ile kısıtlanıyor ise Standart katmana yükseltebilirsiniz. Bunu yapmak için, önce Standart katman sütununu ve **Yükselt** düğmesine tıklayarak fiyatlandırma katmanı menüsüne gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure yay bulutu uygulamasını el ile ölçeklendirmenin nasıl yapılacağını öğrendiniz. Uyarıları ayarlayarak bir uygulamayı izlemeyi öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Uyarıları ayarlamayı öğrenin](spring-cloud-tutorial-alerts-action-groups.md)
