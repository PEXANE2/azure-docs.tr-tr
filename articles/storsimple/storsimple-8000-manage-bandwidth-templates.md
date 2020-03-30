---
title: StorSimple 8000 serisi için bant genişliği şablonlarını yönetme | Microsoft Dokümanlar
description: Bant genişliği tüketimini denetlemenize olanak tanıyan StorSimple bant genişliği şablonlarının nasıl yönetilebildiğini açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254904"
---
# <a name="use-the-storsimple-device-manager-service-to-manage-storsimple-bandwidth-templates"></a>StorSimple bant genişliği şablonlarını yönetmek için StorSimple Device Manager hizmetini kullanın

## <a name="overview"></a>Genel Bakış

Bant genişliği şablonları, StorSimple aygıtından buluta gelen verileri katmanlamak için ağ bant genişliği kullanımını gün içinde birden çok zaman çizelgesi boyunca yapılandırmanıza olanak tanır.

Bant genişliği azaltma zamanlamaları ile şunları yapabilirsiniz:

* İş yükü ağı kullanımlarına bağlı olarak özelleştirilmiş bant genişliği zamanlamaları belirtin.
* Yönetimi merkezileştirin ve zamanlamaları birden çok cihaz arasında kolay ve sorunsuz bir şekilde yeniden kullanın.

> [!NOTE]
> Bu özellik yalnızca StorSimple fiziksel aygıtları (8100 ve 8600 modelleri) için kullanılabilir ve StorSimple Cloud Appliances (8010 ve 8020 modelleri) için kullanılamaz.


## <a name="the-bandwidth-templates-blade"></a>Bant genişliği şablonları bıçak

**Bant Genişliği şablonları** bıçak bir tabular biçiminde hizmet için tüm bant genişliği şablonları vardır ve aşağıdaki bilgileri içerir:

* **Ad** – Oluşturulduğunda bant genişliği şablonuna atanan benzersiz bir ad.
* **Zamanlama** – Belirli bir bant genişliği şablonunda bulunan zamanlama ların sayısı.
* **Tarafından kullanılır** – bant genişliği şablonlarını kullanan birim sayısı.

Ayrıca, bant genişliği şablonlarını yapılandırmaya yardımcı olacak ek bilgiler de bulabilirsiniz:

* [Bant genişliği şablonları ile ilgili sorular ve yanıtlar](#questions-and-answers-about-bandwidth-templates)
* [Bant genişliği şablonları için en iyi uygulamalar](#best-practices-for-bandwidth-templates)

## <a name="add-a-bandwidth-template"></a>Bant genişliği şablonu ekleme

Yeni bir bant genişliği şablonu oluşturmak için aşağıdaki adımları gerçekleştirin.

#### <a name="to-add-a-bandwidth-template"></a>Bant genişliği şablonu eklemek için

1. StorSimple Device Manager hizmetinize gidin, **Bant Genişliği şablonlarını** tıklatın ve sonra **+ Bant Genişliği Ekle şablonunu**tıklatın.

    ![+ Bant genişliği şablonu ekle'yi tıklatın](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp1.png)

2. Bant **genişliği ekle şablon** u, aşağıdaki adımları yapın:
   
    1. Bant genişliği şablonunuz için benzersiz bir ad belirtin.
    2. Bant genişliği çizelgesi tanımlayın. Zamanlama oluşturmak için:
   
        1. Açılan listeden, zamanlamanın yapılandırıldırıldığı haftanın **Günlerini** seçin. Birden çok gün seçebilirsiniz.        
        
        2. _Hh:mm_ formatında **bir Başlangıç Saati** girin. Bu, zamanlamanın başlayacağı zamandır.

        3. _hh:mm_ formatında bir **Bitiş Saati** girin. Bu, programın duracağı zaman.
      
           > [!NOTE]
           > Çakışan zamanlamalara izin verilmez. Başlangıç ve bitiş saatleri çakışan bir zamanlamayla sonuçlanırsa, bu yönde bir hata iletisi görürsünüz.

        4. Bant **Genişliği Hızını**belirtin. Bu, StorSimple cihazınızın bulutu (hem yüklemehem de indirme) içeren işlemlerde kullandığı saniyede Megabit (Mbps) bant genişliğidir. Bu alana 1 ile 1000 arasında bir sayı girin.

            ![Bant genişliği zamanlamasını tanımlama](./media/storsimple-8000-manage-bandwidth-templates/addbwtemp2.png)
         
            İşinizi bitirene kadar şablonunuz için birden çok zamanlama tanımlamak için yukarıdaki adımları yineleyin.

        5. Bant genişliği şablonu oluşturmaya başlamak için **Ekle'yi** tıklatın. Oluşturulan şablon bant genişliği şablonları listesine eklenir.
      

## <a name="edit-a-bandwidth-template"></a>Bant genişliği şablonu oluşturma

Bant genişliği şablonu gerçekleştirmek için aşağıdaki adımları gerçekleştirin.

### <a name="to-edit-a-bandwidth-template"></a>Bant genişliği şablonu için

1. StorSimple Device Manager hizmetinize gidin ve **Bant Genişliği şablonlarını**tıklatın.
2. Bant genişliği şablonları listesinde, silmek istediğiniz şablonu seçin. Sağ tıklayın ve bağlam menüsünden **Sil'i**seçin.
3. Onay istendiğinde **Tamam'ı**tıklatın. Bu, bant genişliği şablonu siler. 
4. Silme işlemini yansıtacak bant genişliği şablonları listesi güncelleştirmeleri.

> [!NOTE]
> Düzenlenen zamanlama, değiştirdiğiniz bant genişliği şablonundaki varolan bir zamanlamayla çakışıyorsa değişikliklerinizi kaydedemezsiniz.

## <a name="delete-a-bandwidth-template"></a>Bant genişliği şablonu silme

Bant genişliği şablonu silmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-delete-a-bandwidth-template"></a>Bant genişliği şablonu silmek için

1. StorSimple Device Manager hizmetinize gidin ve **Bant Genişliği şablonlarını**tıklatın.
2. Bant genişliği şablonları listesinde, silmek istediğiniz şablonu seçin. Sağ tıklatın ve bağlam menüsünden Sil'i seçin.
3. Onay istendiğinde **Tamam'ı**tıklatın. Bu, bant genişliği şablonu siler.
4. Silme işlemini yansıtacak bant genişliği şablonları listesi güncelleştirmeleri.

Şablon herhangi bir birim(ler) tarafından kullanılıyorsa, şablonu silmenize izin verilmez. Şablonun kullanımda olduğunu belirten bir hata iletisi görürsünüz. Şablona yapılan tüm başvuruların kaldırılmasını tavsiye eden bir hata iletisi iletişim kutusu görüntülenir.

**Birim Kapsayıcılar** sayfasına erişerek ve bu şablonu kullanan birim kapsayıcılarını başka bir şablon kullanacak veya özel veya sınırsız bant genişliği ayarı kullanacak şekilde değiştirerek şablona yapılan tüm başvuruları silebilirsiniz. Tüm başvurular kaldırıldığında, şablonu silebilirsiniz.

## <a name="use-a-default-bandwidth-template"></a>Varsayılan bant genişliği şablonu kullanma

Varsayılan bant genişliği şablonu sağlanır ve buluta erişirken bant genişliği denetimlerini zorlamak için varsayılan olarak birim kapsayıcılar tarafından kullanılır. Varsayılan şablon, kendi şablonlarını oluşturan kullanıcılar için de hazır bir başvuru görevi göremez. Bu varsayılan şablonun ayrıntıları şunlardır:

* **İsim** – Sınırsız gece ve hafta sonları
* **Zamanlama** – Pazartesi'den Cuma'ya kadar 08:00 ile 17:00 arasında 1 Mbps bant genişliği oranı uygulayan tek bir zamanlama. Bant genişliği haftanın geri kalanı için Sınırsız olarak ayarlanır.

Varsayılan şablon düzenlenebilir. Bu şablonun kullanımı (düzenlenmiş sürümler dahil) izlenir.

## <a name="create-an-all-day-bandwidth-template-that-starts-at-a-specified-time"></a>Belirli bir anda başlayan tüm gün bant genişliği şablonu oluşturma

Belirli bir zamanda başlayan ve tüm gün çalışan bir zamanlama oluşturmak için bu yordamı izleyin. Örnekte, zamanlama sabah 9'da başlar ve ertesi sabah 09:00'a kadar çalışır. Belirli bir zamanlamanın başlangıç ve bitiş saatlerinin aynı 24 saatlik zamanlamada yer alması ve birden çok güne yayılamayacağı unutulmamalıdır. Birden çok güne yayılan bant genişliği şablonları ayarlamanız gerekiyorsa, birden çok zamanlama kullanmanız gerekir (örnekte gösterildiği gibi).

#### <a name="to-create-an-all-day-bandwidth-template"></a>Tüm gün bant genişliği şablonu oluşturmak için

1. Sabah 9'da başlayan ve gece yarısına kadar çalışan bir zamanlama oluşturun.
2. Başka bir zamanlama ekleyin. İkinci zamanlamayı gece yarısından sabah 9'a kadar çalışacak şekilde yapılandırın.
3. Bant genişliği şablonu kaydedin.

Bileşik zamanlama daha sonra seçtiğiniz bir zamanda başlar ve tüm gün çalışır.

## <a name="questions-and-answers-about-bandwidth-templates"></a>Bant genişliği şablonları ile ilgili sorular ve yanıtlar

**S**. Zamanlamalar arasında olduğunuzda bant genişliği denetimlerine ne olur? (Bir zamanlama sona erdi ve başka bir tane henüz başlamadı.)

**A.** Bu gibi durumlarda, hiçbir bant genişliği denetimleri kullanılacaktır. Bu, aygıtın verileri buluta katmanlarken sınırsız bant genişliği kullanabileceği anlamına gelir.

**S**. Çevrimdışı bir aygıtta bant genişliği şablonlarını değiştirebilir misiniz?

**A.** İlgili aygıt çevrimdışıysa, birim kapsayıcılarında bant genişliği şablonlarını değiştiremesiniz.

**S**. İlişkili birimler çevrimdışı olduğunda birim kapsayıcısıyla ilişkili bir bant genişliği şablonunu düzenleyebilir misiniz?

**A.** Birimleri çevrimdışı olan bir birim kapsayıcısıyla ilişkili bant genişliği şablonunu değiştirebilirsiniz. Birimler çevrimdışı olduğunda, aygıttan buluta hiçbir veri katmanlandırılmayacak.

**S**. Varsayılan bir şablonu silebilir misiniz?

**A.** Varsayılan bir şablonu silseniz de, bunu yapmak iyi bir fikir değildir. Düzenlenen sürümler de dahil olmak üzere varsayılan bir şablonun kullanımı izlenir. İzleme verileri analiz edilir ve zaman içinde varsayılan şablonu geliştirmek için kullanılır.

**S**. Bant genişliği şablonlarınızın değiştirilmesi gerektiğini nasıl belirlersiniz?

**A.** Bant genişliği şablonlarını değiştirmek için gereken işaretlerden biri, ağın bir günde birden çok kez yavaşladığını veya tıkandığını görmeye başladığınız dır. Bu durumda, G/Ç Performansı ve Ağ İş İlerletme grafiklerini inceleyerek depolama ve kullanım ağını izleyin.

Ağ iş hacmi verilerinden, günün saatini ve ağ darboğazının oluştuğu birim kapsayıcılarını tanımlayın. Bu durum buluta katmanlandırıldığında gerçekleşirse (aygıttan buluta tüm birim kapsayıcıları için Bu Bilgileri G/Ç performansından alın), ses kapsayıcılarınızla ilişkili bant genişliği şablonlarını değiştirmeniz gerekir.

Değiştirilen şablonlar kullanımda olduktan sonra, önemli gecikmeler için ağı yeniden izlemeniz gerekir. Bunlar hala varsa, bant genişliği şablonlarınızı yeniden ziyaret etmeniz gerekir.

**S**. Aygıtımdaki birden çok birim kapsayıcıda çakışan ancak her biri için farklı sınırlar uygulanan zamanlamalar varsa ne olur?

**A.** 3 birim lik kapsayıcılı bir aygıtınız olduğunu varsayalım. Bu kapsayıcılarla ilişkili zamanlamalar tamamen çakışıyor. Bu kapsayıcıların her biri için kullanılan bant genişliği sınırları sırasıyla 5, 10 ve 15 Mbps'dir. Bu kapsayıcıların tümlerinde aynı anda G/Ç oluştuğunda, en az 3 bant genişliği sınırı uygulanabilir: bu durumda, bu giden G/Ç istekleri aynı sırayı paylaştığından 5 Mbps.

## <a name="best-practices-for-bandwidth-templates"></a>Bant genişliği şablonları için en iyi uygulamalar

StorSimple cihazınız için aşağıdaki en iyi uygulamaları izleyin:

* Günün farklı saatlerinde ağ çıktısının aygıt tarafından değişken şekilde daraltılmasını sağlamak için aygıtınızdaki bant genişliği şablonlarını yapılandırın. Yedekleme zamanlamaları ile kullanıldığında bu bant genişliği şablonları, yoğun olmayan saatlerde bulut işlemleri için ek ağ bant genişliğinden etkili bir şekilde yararlanabilir.
* Dağıtımın boyutuna ve gerekli kurtarma süresi hedefine (RTO) bağlı olarak belirli bir dağıtım için gereken gerçek bant genişliğini hesaplayın.

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

