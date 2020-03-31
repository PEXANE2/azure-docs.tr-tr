---
title: Etkili çapa deneyimleri için yönergeler
description: Azure Uzamsal Bağlantılarını kullanarak çapaları etkili bir şekilde oluşturmak ve bulmak için yönergeler ve göz önünde bulundurulması gereken kurallar.
author: mattwojo
manager: jken
services: azure-spatial-anchors
ms.author: mattwoj
ms.date: 02/24/2019
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.openlocfilehash: 9a24da8d76f401f534eccf33312fbf0c2bee9f5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74270514"
---
# <a name="create-an-effective-anchor-experience-by-using-azure-spatial-anchors"></a>Azure Uzamsal Bağlantılarını kullanarak etkili bir bağlantı deneyimi oluşturun

Bu makalede, Uzamsal Çapalar kullanarak çapaları etkili bir şekilde oluşturmanıza ve bulmanıza yardımcı olacak yönergeler ve hususlar sağlar.

## <a name="good-anchors"></a>İyi çapalar

Uzamsal Çapalar iyi çapalar oluşturmanıza yardımcı olur. İyi çapalar oluşturmak için kullanıcıları kullanıcı deneyiminizde (UX) eğitmek veya yönlendirmek için zaman ayırabilirsiniz. Öndeiyi iyi çapalar oluşturmaya yatırım yaparak, son kullanıcıların güvenilir bir şekilde çapa bulmalarına yardımcı olursunuz:

- Farklı cihazlar arasında.
- Çeşitli zamanlarda.
- Farklı ışık koşullarında.
- Alan içinde istenilen perspektiflerden.

## <a name="static-and-dynamic-locations"></a>Statik ve dinamik konumlar

Çapa deneyimi tasarımı bir parçası yerleri seçmektir. Konumlar statik olacak ve alanın bir yöneticisi tarafından tanımlanır mı? Ya da dinamik ve kullanıcı tarafından tanımlanan olacak?

Bir perakende mağaza yöneticisi, kullanıcıları ziyaret etmeye ikna etmek için statik bir mağaza içi deneyim isteyebilir. Ama bir karışık gerçeklik kurulu oyunu geliştiricisi büyük olasılıkla kullanıcıların nerede oynamak için seçim izin vermek istiyorum.

Statik konumlar için yöneticilere, alanı iyi çapalarla düzenlemek için zaman harcamayı öğretebilirsiniz.

Dinamik konumlar için, iyi çapalar oluşturmak için UX'nizdeki kullanıcıları nasıl öğrettiğinizi veya yönlendirdiğinizi düşünmelisiniz.

## <a name="stable-visual-features"></a>Kararlı görsel özellikler

Karma gerçeklik ve artırılmış gerçeklik cihazlarında kullanılan görsel izleme sistemleri ortamın görsel özelliklerine dayanır. En güvenilir deneyimi elde etmek için:

- Kararlı görsel özelliklere (diğer bir deyişle, sık sık değişmeyen özellikler) konumlarda bağlantı *lar* oluşturun.

- Ayırt edici özellikleri olmayan büyük boş yüzeylerde çapalar *oluşturmayın.*

- Son derece yansıtıcı malzemeler üzerinde çapa *oluşturmayın.*

- Desen tekrarlar yüzeylerde çapa lar *oluşturmayın,* örneğin halı veya duvar kağıdı gibi.

![Çapalar için iyi bir ortam ve çapalar için kötü bir ortam örnekleri](./media/stable-visual.png)

## <a name="various-viewing-perspectives"></a>Çeşitli görüntüleme perspektifleri

Bir çapa oluştururken, daha sonra çapa bulmak için çalışacağız insanlar düşünün.

Örneğin, iki kapısı olan bir odanın ortasında bir çapa düşünün. Büyük olasılıkla kullanıcıların her iki kapıdan da odaya girmesine izin vermek istiyorsunuz. Çapayı oluştururken, konumunu her iki kapıdan da tararsınız. Kullanıcıların her iki kapıdan da çapayı bulabilmeleri için bağlantının çevresindeki ortam verilerini yakalamak için perspektifleri değiştirirsiniz.

Genel olarak, bir çapa oluştururken, onu bulmaya çalışacak kişilerin bakış açılarından tazyin. Eğer bir açık hava heykeline sanal içerik yerleştiriyorsanız, çapayı oluştururken heykeli tararken etrafında yürümek mantıklıdır. Çapanız bir odanın köşesindeyse, ona yaklaşacak tek bir yön vardır. Bu çapayı oluştururken, sadece bu perspektiften tayabilirsiniz.

## <a name="multiple-anchors"></a>Birden çok çapa

Aydınlatma, bir uygulamanın algıladığını görsel özelliklerde fark yaratabilir. Güçlü doğal ışıkta oluşturulan çapaları yapay ışıkta bulmak zor olabilir, ya da tam tersi.

Bu sorununvarsa, iki bağlantı noktası oluşturmanıza yardımcı olabilir. Aynı noktada, gün ışığında bir çapa ve yapay ışık başka oluşturun. Uygulamanız daha sonra her iki bağlantı için de sorgu yapabilir. Her iki çapa bulunduğunda, uygulama çapa için bir poz alacaktır.

Benzer şekilde, çoğu nesne hareket ettiği için görsel özelliklerin değiştiği ortamlarda, birden çok bağlantı yardımcı olabilir. Bir çapa, çevredeki önemli değişiklikler nedeniyle bulunması çok zorlaştığında, çapayı yenisiyle değiştirebilirsiniz. Bunu, örneğin, düzenin birkaç ayda bir yenilendiği bir perakende satış mağazasında yapabilirsiniz.

## <a name="targets-and-rooms"></a>Hedefler ve odalar

Çoğu durumda, çapa uygulamanızın deneyimine bir giriş noktasıdır. Kullanıcıların deneyiminizi girebilmeleri için bu adımı hızlı ve güvenilir bir şekilde geçmek isteyeceksiniz. Kullanıcıların çapalarınızı nasıl bulacağı konusunda zaman harcamak önemli bir tasarım adımıdır. Bu iki geniş senaryolar açısından çapa bulma hakkında düşünmek yararlıdır: *hedefler* ve *odalar.*

### <a name="targets"></a>Hedefler

Hedef senaryoda, bir çapanın konumu iyi bilinir. Örneğin, kurgusal bir karma gerçeklik boyama uygulamasında, bir kullanıcı duvara sanal bir tuval yerleştirir. Odadaki diğer kullanıcılara, çapayı bulmak ve deneyime başlamak için cihazlarını duvardaki aynı yere doğrultmalarını emreder.

Hedef senaryonun başka bir örneği, bir kafede "Anlaşmaları tarar" yazan bir tabela olabilir. Kahve dükkanı buraya bir çapa yerleştirdi. Kullanıcılar tabelayı tarayıp, kahveyle ilgili fırsatları bulmak için çapayı bulur ve artırılmış gerçeklik deneyimine girerler.

Hedef senaryoda, fotoğraflar yardımcı olabilir. Kullanıcılara aygıtlarında hedeflenen hedefin bir fotoğrafını gösterirseniz, gerçek dünyada ne leri tizlediklerini hızlı bir şekilde belirleyebilirler. Örneğin, kullanıcılarınızın GPS kullanarak hedeflenen hedefin genel alanına ulaşmasına yardımcı olabilirsiniz. Kullanıcı geldiğinde, uygulamanız hedefin bir fotoğrafını gösterir. Kullanıcı uzayın etrafına bakar, hedefi bulur ve çapayı tarar.

![Kullanıcının mobil cihazında hedefin fotoğrafını gösteren çapa çizimi](./media/start-here-edit.png)

### <a name="rooms"></a>Oda -larında

Oda senaryosunda, kullanıcılar sadece burada bir çapa olduğunu bilerek bir alana girerler. Kullanıcılar cihazlarıyla alanı tarayıp hızlı bir şekilde çapayı bulurlar.

Bu deneyim genellikle çeşitli görüntüleme perspektiflerinde anlatıldığı gibi iyi seçilmiş çapalar oluşturmanızı gerektirir. Çapayı oluşturduğunuzda odayı birçok açıdan taradıysanız, kullanıcılar bağlantı noktasını bulmaya çalıştıklarında neredeyse her yeri tayabilirler.

![Bir kullanıcının çapa bulmak için odayı nasıl tayabileceğinin çizimi](./media/scan-room.png)

Esasen, daha sonraki kullanıcıların çapayı hızlı bir şekilde tarayıp bulabilmeleri için çapayı oluşturduğunuzda alanı taramak için daha fazla zaman harcarsınız. Deneyiminizi oluştururken, bu önemli değiş tokuşu göz önünde bulundurmanız gerekir.

Daha önce tartıştığımız karma gerçeklik boyama uygulaması örneği bir oda senaryosu kadar iyi çalışmıyor. Burada, çapayı yerleştiren kullanıcı başkalarının deneyime hızlı bir şekilde katılmasını ister. Kullanıcılar, oda iyi taranana kadar deneyimi başlatmak için beklemek istemezler. Tüm kullanıcılar çapaları bulmak için cihazlarını tam olarak nereye yönlendireceklerini bildiğinden, bu örnek hedef senaryo olarak daha iyi çalışır.

## <a name="anchor-location"></a>Bağlantı konumu

Görsel izleme sistemleri bir ortamdaki görsel özelliklere dayanır. Bir tama ne kadar çok görsel özellik içeriyorsa, çapa bulma olasılığı da o kadar yüksek olabilir.

Ortamın yararlı bir tadına uymayı teşvik eden bir UX oluşturmak için bu bölümdeki genel yönergeleri izleyin.

İlk olarak, kullanıcı birkaç saniye içinde bir bağlantı noktası bulamazsa, uygulama kullanıcıları daha fazla perspektif yakalamak için cihazı hareket ettirmeye teşvik etmelidir. Uygulama ayrıca, kullanıcıların daha fazla perspektiften çapa yı taramaya kendilerini çevrede taşımalarını da teşvik edebilir. Aygıtın gördüğü ne kadar çok özellik perspektifi, o kadar iyi.

Hedef senaryolar için, kullanıcıdan hedefin etrafında hareket etmesini isteyin ve farklı açılardan görüntüleyin. Başka bir deyişle, bağlantı bulunana kadar kullanıcıdan hedefi yeni perspektiflerden yakalamasını isteyin.

Oda senaryoları için, kullanıcıdan odayı yavaşça tetmesini isteyin. Örneğin, kullanıcıdan odanın 180 derece, hatta 360 derecesini yakalamak için dönmesini isteyin. Veya kullanıcıdan odayı yeni bir perspektiften görüntülemesini isteyin.

En anlamlı yöntem oda genelinde taranan etmektir. Oda daki bir taramaya, örneğin yakındaki bir duvarın tadına göre çevrenin daha fazla görsel özelliği yakalar. Yakındaki bir duvarın tkişikinde ortam alabildiği kadar kullanışlı görsel özellik yakalamaz.

Çapa ararken aygıtı tekrar tekrar bir taraftan diğer yana taşımak yararlı değildir. Bu sadece aynı perspektiften aynı noktaları yakalar.

## <a name="experience-tests"></a>Deneyim testleri

Bu makalede, genel yönergeleri tartıştık. Uzamsal Çapalar ile gerçek dünyayla etkileşim edebilen uygulamalar yazabilirsiniz. Bu nedenle, uygulamanızın bağlantı senaryolarını gerçek ortamlarda sınamak için zaman ayırmanız gerekir. Bu, özellikle kullanıcılarınızın uygulamayı kullanmasını beklediğiniz ortamlar için geçerlidir.
