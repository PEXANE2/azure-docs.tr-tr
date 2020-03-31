---
title: Azure Veri Kataloğu'nda aramaları kaydetme ve veri varlıklarını sabitleme
description: Veri kaynaklarını ve veri varlıklarını daha sonra kullanmak üzere kaydetmek için Azure Veri Kataloğu'ndaki özellikleri vurgulama makalesi nasıl kullanılır.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: conceptual
ms.date: 08/01/2019
ms.openlocfilehash: a8545939fd027989519e0f7a161f750ec676749a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68976813"
---
# <a name="save-searches-and-pin-data-assets-in-azure-data-catalog"></a>Azure Veri Kataloğu'nda aramaları kaydetme ve veri varlıklarını sabitleme
## <a name="introduction"></a>Giriş
Azure Veri Kataloğu, veri kaynağı bulma özellikleri sağlar. Veri kaynaklarını bulmak ve amaçlanan amaçlarını anlamak için kataloğu hızlı bir şekilde arayabilir ve filtreleyebilir, bu da eldeki iş için doğru verileri bulmayı kolaylaştırabilir.

Ama düzenli olarak aynı verilerle çalışmak gerekiyorsa ne olur? Peki ya siz ve diğer kullanıcılar kataloğun aynı veri kaynaklarına düzenli olarak bilginize katkıda bulunursanız? Bu gibi durumlarda, aynı aramaları tekrar tekrar vermek verimsiz olabilir. Burası, kaydedilen arama ve sabitlenmiş veri varlıklarının yardımcı olabileceği yerdir.

## <a name="saved-searches"></a>Kaydedilen aramalar
Veri Kataloğu'nda kaydedilmiş bir arama, yeniden kullanılabilir, kullanıcı başına arama tanımıdır. Arama terimleri, etiketler ve diğer filtreler de dahil olmak üzere bir arama tanımlayabilir ve sonra kaydedebilirsiniz. Kaydedilen arama tanımını, arama ölçütlerine uyan tüm veri varlıklarını döndürmek için daha sonra yeniden çalıştırabilirsiniz.

### <a name="create-a-saved-search"></a>Kaydedilen arama oluşturma
Kaydedilmiş bir arama oluşturmak için aşağıdakileri yapın:
1. Azure Veri Kataloğu portalında, **Geçerli Arama** penceresinde **Kaydet'i**tıklatın. 

    ![Geçerli Arama ayarları Bağlantıyı kaydet](./media/data-catalog-how-to-save-pin/01-save-option.png) 

2. Yeniden kullanmak istediğiniz arama ölçütlerini girin ve sonra **Kaydet'i**tıklatın.

    ![Geçerli Arama ayarları arama adını kaydetti](./media/data-catalog-how-to-save-pin/02-name.png)

3. Sizden istendiğinde, kaydedilen arama için bir ad girin. Anlamlı olan ve arama tarafından döndürülecek veri varlıklarını açıklayan bir ad seçin.

### <a name="manage-saved-searches"></a>Kaydedilen aramaları yönetme
Bir veya daha fazla arama kaydettikten sonra, **Geçerli Arama** kutusunun altında **Kaydedilmiş Aramalar** seçeneği görüntülenir. Liste genişletildiğinde, kaydedilen tüm aramalar görüntülenir.

 ![Veri Kataloğu - Kaydedilen aramaların listesi](./media/data-catalog-how-to-save-pin/03-list.png)

Aşağıdakilerden birini yapın:

* Bir aramayı yürütmek için listede kayıtlı bir arama seçin.

* Kaydedilmiş bir arama için yönetim seçenekleri listesini görüntülemek için, arama adının yanındaki aşağı ok'u tıklatın.

    ![Kaydedilen aramaları yönetme seçenekleri](./media/data-catalog-how-to-save-pin/04-managing.png)

* Kaydedilen arama için yeni bir ad girmek için **Yeniden Adlandır'ı**seçin. Arama tanımı değiştirilmez.

* Kayıtlı aramayı listenizden kaldırmak için **Sil'i**seçin ve silme işlemini onaylayın.

* Kaydedilen aramayı varsayılan aramanız olarak işaretlemek için **Varsayılan Olarak Kaydet'i**seçin. Azure Veri Kataloğu ana sayfasından "boş" arama yaparsanız, varsayılan aramanız yürütülür. Ayrıca, varsayılan arama olarak işaretlenmiş arama, **Kayıtlı Aramalar** listesinin en üstünde görüntülenir.

### <a name="organizational-saved-searches"></a>Kuruluş kaydedilmiş aramalar
Kuruluşunuzdaki tüm kullanıcı aramaları kendi kullanımları için kaydedebilir. Veri Kataloğu yöneticileri, kuruluştaki tüm kullanıcılar için aramaları da kaydedebilir. Yöneticiler bir aramayı kaydettiğinde, şirket **içinde paylaşım** seçeneği sunulur. Bu seçeneği n için seçerek, kuruluştaki tüm kullanıcılar için kaydedilen aramayı paylaşır.

 ![Veri Kataloğu - Kuruluş tarafından kaydedilen aramalar](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Sabitlenmiş veri varlıkları
Kaydedilen aramalarla arama tanımlarını kaydedebilir ve yeniden kullanabilirsiniz. Aramalar tarafından döndürülen veri varlıkları, kataloğun içeriği değiştikçe zaman içinde değişebilir. Veri varlıklarını sabitlediğinizde, arama kullanmaya gerek kalmadan erişimi kolaylaştırmak için belirli veri varlıklarını açıkça tanımlayabilirsiniz.

Bir veri kıymetini sabitlemek kolaydır. Veri varlığını sabitlenmiş listenize eklemek için **pin** simgesini tıklatmanız yeterlidir. Simge, döşeme görünümünde varlık döşemesinin köşesinde ve Azure Veri Kataloğu portalındaki liste görünümündeki en sol sütunda görüntülenir.

![Veri Kataloğu - Veri varlık pin simgesi](./media/data-catalog-how-to-save-pin/05-pinning.png)

Bir veri varlığının ppinning eşit derecede basittir. Seçili varlığın ayarını ayarlamak için **unpin** simgesini tıklatmanız yeterlidir.

![Veri Kataloğu - Veri kıymeti unpin simgesi](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="the-my-assets-section"></a>Varlıklarım bölümü
Veri Kataloğu portalı ana sayfasında, geçerli kullanıcının ilgilendiği varlıkları görüntüleyen **Varlıklarım** bölümü bulunur. Bu bölümde hem sabitlenmiş varlıklar hem de kaydedilmiş aramalar yer alıyor.

![Ana sayfadaki Varlıklarım bölümü](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Özet
Azure Veri Kataloğu, ihtiyacınız olan veri kaynaklarını keşfetmeyi kolaylaştıran özellikler sağlar, böylece siz ve diğer kuruluş üyeleri verileri aramak için daha az zaman harcayabilir ve bu kaynaklarla çalışmak için daha fazla zaman harcayabilirsiniz. Kaydedilen aramalar ve sabitlenmiş veri varlıkları, kullanıcıların tekrar tekrar çalıştıkları veri kaynaklarını kolayca tanımlayabilmeleri için bu temel özellikleri temel olarak oluşturur.
