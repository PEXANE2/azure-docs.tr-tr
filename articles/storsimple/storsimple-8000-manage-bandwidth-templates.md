---
title: StorSimple 8000 serisi için bant genişliği şablonlarını yönetme | Microsoft Docs
description: Bant genişliği tüketimini denetlemenizi sağlayan StorSimple bant genişliği şablonlarının nasıl yönetileceğini açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: 13a3e57bb27c075fc045e87790dbe13369ed9f8e
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79254904"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple bant genişliği şablonlarını yönetmek için StorSimple Aygıt Yöneticisi hizmetini kullanın

## <a name="overview"></a>Genel Bakış

Bant genişliği şablonları, StorSimple cihazındaki verileri buluta katman olarak eklemek için birden çok günlük zaman çizelgesi boyunca ağ bant genişliği kullanımını yapılandırmanıza olanak tanır.

Bant genişliği azaltma zamanlamalarıyla şunları yapabilirsiniz:

* İş yükü ağ kullanımlarına bağlı olarak özelleştirilmiş bant genişliği zamanlamaları belirtin.
* Yönetimini merkezileştirmek ve birden çok cihaz genelinde zamanlamaları kolay ve sorunsuz bir şekilde yeniden kullanın.

> [!NOTE]
> Bu özellik, StorSimple bulut gereçlerine (modeller 8010 ve 8020) değil, yalnızca StorSimple fiziksel cihazları (modeller 8100 ve 8600) için kullanılabilir.


## <a name="the-bandwidth-templates-blade"></a>Bant genişliği şablonları dikey penceresi

**Bant genişliği şablonları** dikey penceresinde, hizmetinizdeki tüm bant genişliği şablonları tablosal biçiminde bulunur ve aşağıdaki bilgiler bulunur:

* **Name** : bant genişliği şablonuna atanan benzersiz bir ad.
* **Schedule** : belirli bir bant genişliği şablonunda bulunan zamanlamaların sayısı.
* **Kullanılan** : bant genişliği şablonlarını kullanan birim sayısı.

Ayrıca, ' de bant genişliği şablonlarını yapılandırmaya yardımcı olacak ek bilgiler bulabilirsiniz:

* [Bant genişliği şablonları hakkında sorular ve yanıtlar](#questions-and-answers-about-bandwidth-templates)
* [Bant genişliği şablonları için en iyi yöntemler](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Bant genişliği şablonu ekleme

Yeni bir bant genişliği şablonu oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-a-bandwidth-template"></a>Bir bant genişliği şablonu eklemek için

1. StorSimple Aygıt Yöneticisi hizmetinize gidin, **bant genişliği şablonları** ' na tıklayın ve **+ bant genişliği şablonu Ekle**' ye tıklayın.

    ![\+ Bant genişliği şablonu Ekle ' ye tıklayın](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. **Bant genişliği şablonu Ekle** dikey penceresinde aşağıdaki adımları uygulayın:
   
    1. Bant genişliği şablonunuz için benzersiz bir ad belirtin.
    2. Bant genişliği zamanlaması tanımlayın. Bir zamanlama oluşturmak için:
   
        1. Aşağı açılan listeden, zamanlamanın yapılandırıldığı haftanın **günlerini** seçin. Birden çok gün seçebilirsiniz.        
        
        2. _SS: DD_ biçiminde bir **Başlangıç saati** girin. Bu, zamanlamanın başlayacağı zaman olur.

        3. _SS: DD_ biçiminde bir **bitiş saati** girin. Bu, zamanlamanın durması durumunda olur.
      
           > [!NOTE]
           > Çakışan zamanlamalara izin verilmez. Başlangıç ve bitiş zamanları çakışan bir zamanlamaya neden olursa, bu etkiye yönelik bir hata iletisi görürsünüz.

        4. **Bant genişliği hızını**belirtin. Bu, StorSimple cihazınız tarafından bulut (yüklemeler ve İndirmeler) ile ilgili işlemlerde kullanılan bit genişliği (MB/sn) cinsinden bant genişliğidir. Bu alana 1 ile 1000 arasında bir sayı girin.

            ![Bant genişliği zamanlamasını tanımlama](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            Siz tamamlanana kadar şablonunuz için birden çok zamanlama tanımlamak üzere yukarıdaki adımları tekrarlayın.

        5. Bir bant genişliği şablonu oluşturmaya başlamak için **Ekle** ' ye tıklayın. Oluşturulan şablon, bant genişliği şablonları listesine eklenir.
      

## <a name="edit-a-bandwidth-template"></a>Bant genişliği şablonunu düzenleme

Bir bant genişliği şablonunu düzenlemek için aşağıdaki adımları gerçekleştirin.

### <a name="to-edit-a-bandwidth-template"></a>Bir bant genişliği şablonunu düzenlemek için

1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve **bant genişliği şablonları**' na tıklayın.
2. Bant genişliği şablonları listesinde, silmek istediğiniz şablonu seçin. Sağ tıklayıp bağlam menüsünden **Sil**' i seçin.
3. Onay istendiğinde **Tamam**' a tıklayın. Bu, bant genişliği şablonunu silmelidir. 
4. Bant genişliği şablonlarının listesi, silme işlemini yansıtacak şekilde güncelleştirilir.

> [!NOTE]
> Düzenlenen zamanlama, değiştirdiğiniz bant genişliği şablonunda varolan bir zamanlamaya göre çakışırsa değişikliklerinizi kaydedemezsiniz.

## <a name="delete-a-bandwidth-template"></a>Bant genişliği şablonunu silme

Bir bant genişliği şablonunu silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-bandwidth-template"></a>Bir bant genişliği şablonunu silmek için

1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve **bant genişliği şablonları**' na tıklayın.
2. Bant genişliği şablonları listesinde, silmek istediğiniz şablonu seçin. Sağ tıklayıp bağlam menüsünden Sil ' i seçin.
3. Onay istendiğinde **Tamam**' a tıklayın. Bu, bant genişliği şablonunu silmelidir.
4. Bant genişliği şablonlarının listesi, silme işlemini yansıtacak şekilde güncelleştirilir.

Şablon herhangi bir birim (ler) tarafından kullanılıyorsa, bu şablonu silme izniniz olmayacaktır. Şablonun kullanımda olduğunu belirten bir hata iletisi görürsünüz. Şablona yönelik tüm başvuruların kaldırılması gerektiğini bildiren bir hata mesajı iletişim kutusu görüntülenir.

**Birim kapsayıcıları** sayfasına erişerek şablona yapılan tüm başvuruları silebilir ve bu şablonu kullanan birim kapsayıcılarını değiştirerek, başka bir şablon kullanır veya özel veya sınırsız bir bant genişliği ayarı kullanın. Tüm başvurular kaldırıldığında, şablonu silebilirsiniz.

## <a name="use-a-default-bandwidth-template"></a>Varsayılan bir bant genişliği şablonu kullan

Varsayılan olarak, bir bant genişliği şablonu sağlanır ve buluta erişirken bant genişliği denetimlerini zorlamak için varsayılan olarak birim kapsayıcıları tarafından kullanılır. Varsayılan şablon Ayrıca kendi şablonlarını oluşturan kullanıcılar için hazır bir başvuru işlevi görür. Bu varsayılan şablonun ayrıntıları şunlardır:

* **Ad** – sınırsız gece ve hafta sonları
* **Schedule** : Pazartesi 'den Cuma 'ya kadar, 8 Ila 5 pm cihaz süresi arasında 1 Mbps 'lik bir bant genişliği oranı uygulayan tek bir zamanlama. Bant genişliği, haftanın geri kalanı için sınırsız olarak ayarlanır.

Varsayılan şablon düzenlenebilir. Bu şablonun kullanımı (düzenlenmiş sürümler dahil) izlenir.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Belirli bir zamanda başlayan bir günlük bant genişliği şablonu oluşturun

Belirli bir zamanda başlayan ve tüm gün çalışan bir zamanlama oluşturmak için bu yordamı izleyin. Örnekte, zamanlama sabah 9 ' da başlar ve sonraki sabah 9 ' a kadar çalışır. Belirli bir zamanlamanın başlangıç ve bitiş sürelerinin her ikisi de aynı 24 saat zamanlamasında bulunmalıdır ve birden çok güne yayılamaz. Birden çok güne yayılan bant genişliği şablonları ayarlamanız gerekiyorsa birden çok zamanlama kullanmanız gerekir (örnekte gösterildiği gibi).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Tüm gün bant genişliği şablonu oluşturmak için

1. Sabah 00 ' da başlayan ve gece yarısına kadar çalışan bir zamanlama oluşturun.
2. Başka bir zamanlama ekleyin. İkinci zamanlamayı sabah 9 ' a kadar gece yarısından itibaren çalışacak şekilde yapılandırın.
3. Bant genişliği şablonunu kaydedin.

Bileşik zamanlama daha sonra, seçtiğiniz ve her gün çalıştırılan bir zamanda başlar.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Bant genişliği şablonları hakkında sorular ve yanıtlar

**Soru-cevap** Zamanlamalar arasında çalışırken bant genişliği denetimlerine ne olur? (Bir zamanlama sona erdi ve başka bir tane henüz başlatılmamış.)

**A**. Böyle durumlarda, bant genişliği denetimleri işe alınmaz. Bu, cihazın verileri bulutta kullanırken sınırsız bant genişliği kullanabileceği anlamına gelir.

**Soru-cevap** Çevrimdışı bir cihazdaki bant genişliği şablonlarını değiştirebilir misiniz?

**A**. Karşılık gelen cihaz çevrimdışı ise, birimler kapsayıcılarındaki bant genişliği şablonlarını değiştiremeyeceksiniz.

**Soru-cevap** İlişkili birimler çevrimdışıyken bir birim kapsayıcısı ile ilişkili bir bant genişliği şablonunu düzenleyebilir miyim?

**A**. Birimleri çevrimdışı olan bir birim kapsayıcısı ile ilişkili bir bant genişliği şablonunu değiştirebilirsiniz. Birimler çevrimdışıyken, cihazdan buluta hiçbir veri katmanlanmadığını unutmayın.

**Soru-cevap** Varsayılan bir şablonu silebilir misiniz?

**A**. Varsayılan bir şablonu silebilseniz de bunu yapmak iyi bir fikir değildir. Düzenlenmiş sürümler dahil olmak üzere varsayılan şablonun kullanımı izlenir. İzleme verileri analiz edilir ve zaman içinde, varsayılan şablonu geliştirmek için kullanılır.

**Soru-cevap** Bant genişliği şablonlarınızın değiştirilmesi gerektiğini nasıl belirlersiniz?

**A**. Bant genişliği şablonlarını değiştirmek için gereken işaretlerin biri, bir gün içinde birden çok kez ağ yavaşmı veya sıkıştırma Bu durumda, g/ç performansı ve ağ Işleme grafiklerine bakarak depolama ve kullanım ağını izleyin.

Ağ işleme verilerinden, günün saatini ve ağ tıkanıklarının gerçekleştiği birim kapsayıcıları ' nı tespit edin. Bu durum, veriler buluta katmanlanmakta olduğunda (cihazdan buluta kadar tüm birim kapsayıcıları için g/ç performansından bu bilgileri al), birim kapsayıcılarınızla ilişkili bant genişliği şablonlarını değiştirmeniz gerekecektir.

Değiştirilen Şablonlar kullanımda olduktan sonra, önemli gecikme süreleri için ağı yeniden izlemeniz gerekir. Bunlar hala varsa, bant genişliği şablonlarınızı geri yüklemeniz gerekecektir.

**Soru-cevap** Cihazımın birden çok birim kapsayıcısı, çakışan ve farklı limitlerin her biri için geçerlidir durumunda ne olur?

**A**. 3 birim kapsayıcısı olan bir cihazınız olduğunu varsayalım. Bu kapsayıcılarla ilişkili zamanlamalar tamamen çakışıyor. Bu kapsayıcıların her biri için, kullanılan bant genişliği sınırları sırasıyla 5, 10 ve 15 Mbps 'Dir. G/ç, bu kapsayıcıların tümünde aynı anda gerçekleşirken, en az 3 bant genişliği sınırı uygulanabilir: Bu durumda, bu giden g/ç isteklerinin aynı kuyruğu paylaştığı 5 Mbps olur.

## <a name="best-practices-for-bandwidth-templates"></a>Bant genişliği şablonları için en iyi yöntemler

StorSimple cihazınız için en iyi uygulamaları izleyin:

* Cihazdaki bant genişliği şablonlarını, günün farklı saatlerinde cihaz tarafından ağ aktarım hızı için değişken azaltmayı etkinleştirmek üzere yapılandırın. Bu bant genişliği şablonları yedekleme zamanlamalarıyla birlikte kullanıldığında, bulut işlemleri için yoğun olmayan saatlerde ek ağ bant genişliğinden etkili bir şekilde yararlanabilir.
* Dağıtım ve gerekli kurtarma süresi hedefi (RTO) temelinde belirli bir dağıtım için gerekli olan gerçek bant genişliğini hesaplayın.

## <a name="next-steps"></a>Sonraki adımlar

StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

