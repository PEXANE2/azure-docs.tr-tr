---
title: Kişiselleştirme nedir?
titleSuffix: Azure Cognitive Services
description: Kişiselleştirici, gerçek zamanlı davranışlarından öğrenerek kullanıcılarınıza gösterilecek en iyi deneyimi seçmenize olanak tanıyan bulut tabanlı bir API hizmetidir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 99750971e11171c0b315cac38089c216d42c7ba6
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663659"
---
# <a name="what-is-personalizer"></a>Kişiselleştirme nedir?

Azure Kişiselleştirme, kullanıcıların gerçek zamanlı davranışlarından öğrenerek onlara gösterilebilecek en iyi deneyimi seçmenizi sağlayan bulut tabanlı bir API’dir.

* Kullanıcılarınız ve içeriğiniz hakkında bilgi sağlayın ve kullanıcılarınızı göstermek için en iyi eylemi alın. 
* Kişiselleştirici kullanılmadan önce verilerin temizlenmesi ve etiketlenmesi gerekmez.
* Sizin için uygun olduğunda Kişiselleştiriciye geri bildirim sağlayın. 
* Gerçek zamanlı analizi görüntüleyin. 
* Mevcut denemeleri doğrulamak için daha büyük veri bilimi çabalarının bir parçası olarak kişiselleştirici kullanın.

## <a name="how-does-personalizer-work"></a>Kişiselleştirici nasıl çalışır?

Kişiselleştirici, bir bağlamda en yüksek düzeyde sıralama eylemini saptamak için makine öğrenimi modellerini kullanır. İstemci uygulamanız, olası eylemlerin bir listesini, bunlarla ilgili bilgilerle birlikte sağlar; ve Kullanıcı, cihaz vb. hakkında bilgi içerebilen bağlam hakkındaki bilgileri içerir. Kişiselleştirici gerçekleştirilecek eylemi belirler. İstemci uygulamanız seçilen eylemi kullandığında, bir ödül puanı biçiminde Kişiselleştiriciye geri bildirim sağlar. Geri bildirim alındıktan sonra, kişiselleştirici, gelecekteki derecelendirmeler için kullanılan kendi modelini otomatik olarak güncelleştirir.

## <a name="how-do-i-use-the-personalizer"></a>Kişiselleştirici kullanmak Nasıl yaparım? mı?

![Bir kullanıcıya hangi videonun gösterileceğini seçmek için kişiselleştirici kullanma](media/what-is-personalizer/personalizer-example-highlevel.png)

1. Uygulamanızda kişiselleştirmek için bir deneyim seçin.
1. Azure portal kişiselleştirme hizmeti örneğini oluşturun ve yapılandırın. Her örnek bir kişiselleştirici döngüdür.
1. Kullanıcılarınız hakkında bilgiler (_Özellikler_) ve içerik (_Eylemler_) ile KIŞISELLEŞTIRICI çağırmak için SDK 'yı kullanın. Kişiselleştirici kullanılmadan önce temiz, etiketli veriler sağlamanız gerekmez. 
1. İstemci uygulamasında, kullanıcıya kişiselleştirici tarafından seçilen eylemi gösterin.
1. Kullanıcının kişiselleştirici eylemi seçtiği belirlenen Kişiselleştiriciye geri bildirim sağlamak için SDK 'Yı kullanın. Bu, genellikle-1 ile 1 arasında bir _ödül puandır_.
1. Sistemin nasıl çalıştığını ve verilerinizin kişiselleştirmeye nasıl yardımcı olduğunu değerlendirmek için Azure portal analizlerini görüntüleyin.

## <a name="where-can-i-use-personalizer"></a>Kişiselleştiriciye nereden kullanabilirim?

Örneğin, istemci uygulamanız aşağıdakileri öğesine kişiselleştirici ekleyebilir:

* Bir haber web sitesinde vurgulanan makaleyi kişiselleştirin.    
* Web sitesinde ad yerleşimini iyileştirin.
* Bir alışveriş web sitesinde kişiselleştirilmiş bir "önerilen öğe" görüntüleyin.
* Belirli bir fotoğrafta uygulanacak filtreler gibi kullanıcı arabirimi öğelerini önerin.
* Kullanıcı amacını açıklamak veya bir eylem önermek için bir sohbet bot 'un yanıtını seçin.
* Bir kullanıcının iş sürecinde bir sonraki adım olarak ne yapması gerektiğine ilişkin önerilere öncelik verin.

## <a name="personalization-for-developers"></a>Geliştiriciler için kişiselleştirme

Kişiselleştirici hizmeti iki API 'ye sahiptir:

* Kullanıcılarınız ve kişiselleştirilmesi için içerik (_Eylemler_) hakkındaki bilgileri (_özellikleri_) gönderin. Kişiselleştirici, en üstteki eylemle yanıt verir.
* Sıralamanın genellikle 0 ile 1 arasında bir sayı olarak ne kadar iyi çalıştığı hakkında kişiselleştirmeden geri bildirim gönderin (önceki bölüm, 1 ve 1 ' i ifade etti). 

![Kişiselleştirmeye yönelik temel olay sırası](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı Başlangıç: İçinde geri bildirim döngüsü oluşturunC#](csharp-quickstart-commandline-feedback-loop.md)
* [Etkileşimli tanıtımı kullanma](https://personalizationdemo.azurewebsites.net/)
