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
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: 3132d31e9e45718fa95c39a1b8160ea303ded25d
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70883670"
---
# <a name="what-is-personalizer"></a>Kişiselleştirme nedir?

Azure Kişiselleştirme, kullanıcıların gerçek zamanlı davranışlarından öğrenerek onlara gösterilebilecek en iyi deneyimi seçmenizi sağlayan bulut tabanlı bir API’dir.

* Kullanıcılarınız ve içeriğiniz hakkında bilgi sağlayın ve kullanıcılarınızı göstermek için en iyi eylemi alın. 
* Kişiselleştirici kullanılmadan önce verilerin temizlenmesi ve etiketlenmesi gerekmez.
* Sizin için uygun olduğunda Kişiselleştiriciye geri bildirim sağlayın. 
* Gerçek zamanlı analizi görüntüleyin. 
* Mevcut denemeleri doğrulamak için daha büyük veri bilimi çabalarının bir parçası olarak kişiselleştirici kullanın.

## <a name="how-does-personalizer-work"></a>Kişiselleştirici nasıl çalışır?

Kişiselleştirici, bir bağlamda en yüksek düzeyde sıralama eylemini saptamak için makine öğrenimi modellerini kullanır. İstemci uygulamanız, olası eylemlerin bir listesini, bunlarla ilgili bilgilerle birlikte sağlar; ve Kullanıcı, cihaz vb. hakkında bilgi içerebilen bağlam hakkındaki bilgileri içerir. Kişiselleştirici gerçekleştirilecek eylemi belirler. İstemci uygulamanız seçilen eylemi kullandığında, bir ödül puanı biçiminde Kişiselleştiriciye geri bildirim sağlar. Geri bildirim alındıktan sonra, kişiselleştirici, gelecekteki derecelendirmeler için kullanılan kendi modelini otomatik olarak güncelleştirir. Zaman içinde, kişiselleştirici, özelliklerine göre her bir bağlamda seçmek için en iyi eylemi öneren bir modeli eğitecektir.

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

Kişiselleştirici Kullanıcı profili bilgilerini kalıcı hale getirmek ve yönetmek ya da bireysel kullanıcıların tercihlerini veya geçmişini günlüğe kaydetmek için bir hizmet değildir. Kişiselleştirici, benzer özellikler gerçekleştiğinde, tek bir model bağlam içindeki her etkileşimin özelliklerinden öğrenir. 

## <a name="personalization-for-developers"></a>Geliştiriciler için kişiselleştirme

Kişiselleştirici hizmeti iki API 'ye sahiptir:

* Kullanıcılarınız ve kişiselleştirilmesi için içerik (_Eylemler_) hakkındaki bilgileri (_özellikleri_) gönderin. Kişiselleştirici, en üstteki eylemle yanıt verir.
* Sıralamanın genellikle 0 ile 1 arasında bir sayı olarak ne kadar iyi çalıştığı hakkında kişiselleştirmeden geri bildirim gönderin (önceki bölüm, 1 ve 1 ' i ifade etti). 

![Kişiselleştirmeye yönelik temel olay sırası](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Kişiselleştirici 'daki yenilikler nelerdir?](whats-new.md)
* [Kişiselleştirici nasıl çalışıyor?](how-personalizer-works.md)
* [Pekiştirmeye dayalı Learning nedir?](concepts-reinforcement-learning.md)
* [Sıralama isteğine yönelik özellikler ve eylemler hakkında bilgi edinin](concepts-features.md)
* [Reward isteği için puanı belirleme hakkında bilgi edinin](concept-rewards.md)
* [Etkileşimli tanıtımı kullanma](https://personalizationdemo.azurewebsites.net/)
