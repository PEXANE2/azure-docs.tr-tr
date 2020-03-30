---
title: Microsoft Azure StorBasit Veri Yöneticisi'ne genel bakış | Microsoft Dokümanlar
description: StorSimple Data Manager hizmetine genel bakış sağlar
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/21/2018
ms.author: alkohli
ms.openlocfilehash: 2ffe17bf7ef4f01c18d2c26f4a045add7302272d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67876114"
---
# <a name="storsimple-data-manager-solution-overview"></a>StorSimple Data Manager çözümüne genel bakış

## <a name="overview"></a>Genel Bakış

Microsoft Azure StorSimple, şirket içi çözümün bir uzantısı olarak bulut depolamayı kullanır ve verileri şirket içi depolama ve bulut arasında otomatik olarak katmanlar. Veriler, maksimum verimlilik ve maliyetleri düşürmek için bulutta sıkıştırılmış ve sıkıştırılmış bir biçimde depolanır. Veriler StorSimple biçiminde depolandıkça, kullanmak isteyebileceğiniz diğer bulut uygulamaları tarafından kolayca tüketilemez.

StorSimple Veri Yöneticisi, buluttaki StorSimple format verilerine sorunsuz bir şekilde erişmenizi ve bunları kullanmanızı sağlar. Bunu, Azure Medya Hizmetleri, Azure HDInsights ve Azure Machine Learning gibi diğer hizmetlerle kullanabileceğiniz StorSimple biçimini yerel blob'lara ve dosyalara dönüştürerek yapar.

Bu makalede, StorSimple Data Manager çözümüne genel bir bakış sağlanma sağlar. Ayrıca, bulutta StorSimple verilerini ve diğer Azure hizmetlerini kullanan uygulamaları yazmak için bu hizmeti nasıl kullanabileceğinizi de açıklar.

## <a name="how-it-works"></a>Nasıl çalışıyor?

StorSimple Data Manager hizmeti, storsimple 8000 serisi şirket içi bir cihazdan buluttaki StorSimple verilerini tanımlar. Buluttaki StorSimple verileri deduped, sıkıştırılmış StorSimple biçimi. Veri Yöneticisi hizmeti, StorSimple biçim verilerini ayıklamak ve Azure lekeleri ve Azure Dosyaları gibi diğer biçimlere dönüştürmek için API'ler sağlar. Bu dönüştürülmüş veriler azure HDInsight ve Azure Media hizmetleri tarafından kolayca tüketilir. Veri dönüşümü böylece bu hizmetlerin StorSimple 8000 serisi şirket içi cihazdan dönüştürülmüş StorSimple verileri üzerinde çalışmasını sağlar. Bu akış aşağıdaki diyagramda gösterilmiştir.

![Üst düzey diyagram](./media/storsimple-data-manager-overview/storsimple-data-manager-overview2.png)


## <a name="data-manager-use-cases"></a>Veri Yöneticisi kullanım örnekleri

Azure İşlevlerine sahip Veri Yöneticisini, Azure Otomasyonu ve Azure Veri Fabrikası'nı kullanarak verilerinizde iş akışlarının StorSimple'a girerken çalışmasını sağlayabilirsiniz. Azure Medya Hizmetleri ile StorSimple'da depoladığınız medya içeriğinizi işlemek veya bu veriler üzerinde bir Machine Learning algoritması çalıştırmak veya StorSimple'da depoladığınız verileri çözümlemek için bir Hadoop kümesi oluşturmak isteyebilirsiniz. Azure'da bulunan geniş hizmet yelpazesi ve StorSimple'daki verilerle birlikte verilerinizin kilidini açabilirsiniz.


## <a name="region-availability"></a>Bölge kullanılabilirliği

StorSimple Veri Yöneticisi aşağıdaki 7 bölgede kullanılabilir:

 - Güneydoğu Asya
 - Doğu ABD
 - Batı ABD
 - Batı ABD 2
 - Orta Batı ABD
 - Kuzey Avrupa
 - Batı Avrupa

Ancak, StorSimple Veri Yöneticisi aşağıdaki bölgelerdeki verileri dönüştürmek için kullanılabilir. 

![Veriler için kullanılabilen bölgeler](./media/storsimple-data-manager-overview/data-manager-job-definition-different-regions-m.png)

Yukarıdaki bölgelerden herhangi birinde kaynak dağıtımı aşağıdaki bölgelerde dönüşüm işlemini gündeme getirebildiği için bu küme daha büyüktür. Bu nedenle, verileriniz 19 bölgeden herhangi birinde bulunduğu sürece, bu hizmeti kullanarak verilerinizi dönüştürebilirsiniz.


## <a name="choosing-a-region"></a>Bölge seçme

Şunları öneririz:
 - Kaynak depolama hesabınız (StorSimple aygıtınızla ilişkili olan) ve hedef depolama hesabınız (verilerin yerel biçimde olmasını istediğiniz yer) aynı Azure bölgesinde olmalıdır.
 - StorSimple depolama hesabını içeren bölgede Veri Yöneticinizi ve iş tanımınızı getirirsiniz. Bu mümkün değilse, en yakın Azure bölgesindeki Veri Yöneticisi'ni getirin ve ardından StorSimple depolama hesabınızla aynı bölgede İş Tanımı'nı oluşturun. 

    StorSimple depolama hesabınız iş tanımı oluşturmayı destekleyen 26 bölgede değilse, uzun gecikmeler ve olası çıkış ücretleri gördüğünüz için StorSimple Data Manager'ı çalıştırmamanızı öneririz.
    
Microsoft, Azure hizmetlerinin tüm bölgelerde her zaman kullanılabilir olmasını sağlamaya çalışır. Ancak, belirli bir bölgede kısa süreler için planlanmamış hizmet kesintileri oluşabilir. Bu gibi durumlarda, kesintiden etkilenmeyen bir bölgede bir Veri Yöneticisi ve iş tanımı getirebilir ve dönüşüm işini çalıştırabilirsiniz. Böyle bir senaryoda bazı ek gecikme karşılaşabilirsiniz, ancak bu bölgesel bir kesinti nadir durumda kurtarma stratejisi olabilir.

## <a name="security-considerations"></a>Güvenlik konuları

StorSimple Veri Yöneticisi'nin, StorSimple biçiminden yerel biçime dönüşmesi için hizmet veri şifreleme anahtarına ihtiyacı vardır. İlk aygıt StorSimple hizmetine kaydolduğunda hizmet veri şifreleme anahtarı oluşturulur. Bu anahtar hakkında daha fazla bilgi için [StorSimple security'ye](storsimple-8000-security.md)gidin.

Giriş olarak sağlanan hizmet veri şifreleme anahtarı, Veri Yöneticisi oluşturduğunuzda oluşturulan bir anahtar kasasında depolanır. Kasa, StorSimple Veri Yöneticinizle aynı Azure bölgesinde bulunur. Veri Yöneticisi hizmetinizi sildiğinizde bu anahtar silinir.

Bu anahtar dönüştürme gerçekleştirmek için işlem kaynakları tarafından kullanılır. Bu hesaplama kaynakları, iş tanımınızla aynı Azure bölgesinde bulunur. Bu bölge, Veri Yöneticinizi getirdiğiniz bölgeyle aynı olabilir veya olmayabilir.

Veri Yöneticisi bölgeniz iş tanımı bölgenizden farklıysa, bu bölgelerin her birinde hangi verilerin/meta verilerin bulunduğunu anlamanız önemlidir. Aşağıdaki diyagram, Veri Yöneticisi ve iş tanımı için farklı bölgelere sahip olmanın etkisini göstermektedir.

![Farklı bölgelerde hizmet ve iş tanımı](./media/storsimple-data-manager-overview/data-manager-job-different-regions.png)

## <a name="managing-personal-information"></a>Kişisel bilgilerin yönetimi

StorSimple Veri Yöneticisi herhangi bir kişisel bilgi toplamaz veya görüntülemez. Daha fazla bilgi için, [Güven Merkezi](https://www.microsoft.com/trustcenter)’nde Microsoft Gizlilik ilkesini gözden geçirin.

## <a name="known-limitations"></a>Bilinen Sınırlamalar

Hizmet şu anda aşağıdaki sınırlamaları vardır:
- StorSimple Veri Yöneticisi şu anda bitlocker şifreli birimleri ile çalışmıyor. Hizmeti şifreli bir sürücüyle çalıştırmayı denerseniz, iş hataları görürsünüz.
- Dosyaların bazı meta verileri (ALA'lar dahil) dönüştürülmüş verilerde tutulmaz.
- Bu hizmet yalnızca NTFS birimleriyle çalışır.
- Dosya yolu uzunlukları 256 karakterden az olması gerekir başka iş başarısız olur.

## <a name="next-steps"></a>Sonraki adımlar

[Verilerinizi dönüştürmek için StorSimple Data Manager Kullanıcı UI'ı kullanın.](storsimple-data-manager-ui.md)
