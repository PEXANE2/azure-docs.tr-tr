---
title: StorSimple Virtual Array için uyarıları görüntüleme ve yönetme
description: StorSimple Virtual Array uyarı koşullarını ve önem derecesini ve uyarıları yönetmek için StorSimple Manager hizmetini nasıl kullanacağımı açıklar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: 97ee25a1-0ec3-4883-9a0a-54b722598462
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/12/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a7c469be4a1d8aba23857b1ba52ee829c126a431
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267436"
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>StorSimple Virtual Array uyarılarını yönetmek için StorSimple Device Manager'ı kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Device Manager hizmetindeki uyarılar özelliği, StorSimple Virtual Arrays ile ilgili uyarıları gerçek zamanlı olarak gözden geçirmeniz ve temizlemeniz için bir yol sağlar. StorSimple Virtual Arrays'inizin ve genel Microsoft Azure StorSimple çözümünün sistem durumu sorunlarını merkezi olarak izlemek için **Hizmet özeti** bıçaktaki uyarıları kullanabilirsiniz.

Bu öğretici, uyarı bildirimlerinin nasıl yapılandırılabildiğini, yaygın uyarı durumlarını, uyarı önem düzeylerinin nasıl görüntülenebildiğini ve izlerinin nasıl görüntülenebildiğini açıklar. Ayrıca, belirli bir uyarıyı hızlı bir şekilde bulmanızı ve uygun şekilde yanıt vermenizi sağlayan hızlı uyarı başvuru tablolarını içerir.

![Uyarılar sayfası](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandırma

StorSimple Virtual Dizilerinizin her biri için uyarı koşullarının e-posta yoluyla bildirilmesini isteyip istemediğinizi seçebilirsiniz. Ayrıca, diğer uyarı bildirimi alıcılarını, e-posta adreslerini yarı sütunlarla ayrılmış **Ek e-posta alıcıları** kutusuna girerek tanımlayabilirsiniz.

> [!NOTE]
> Sanal dizi başına en fazla 20 e-posta adresi girebilirsiniz.

Sanal bir dizi için e-posta bildirimini etkinleştirdikten sonra, bildirim listesinin üyeleri her kritik uyarı oluştuğunda bir e-posta iletisi alır. İletiler *storsimple-alerts-noreply\@mail.windowsazure.com* gönderilecektir ve uyarı durumunu açıklar. Alıcılar kendilerini e-posta bildirim listesinden kaldırmak için **Abonelikten Çık'ı** tıklatabilir.

#### <a name="to-enable-email-notification-for-alerts"></a>Uyarılar için e-posta bildirimini etkinleştirmek için

1. StorSimple Device Manager hizmetinize gidin ve **Yönetim** bölümünde **Cihazlar'ı**seçin ve tıklatın. Görüntülenen aygıtlar listesinden cihazınızı seçin ve tıklatın.
   
    ![uyarı ayarları](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Bu **Ayarlar** bıçak açılır. Aygıt **ayarları** bölümünde **Genel'i**seçin. Bu, **Genel Ayarlar** bıçağını açar.
   
    ![uyarıları bildirim yapılandırma](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. Genel **ayarlar** bıçağında, **Uyarı ayarları** bölümüne gidin ve aşağıdakileri ayarlayın:
   
   1. **E-posta bildirimini etkinleştir** alanında **EVET'i**seçin.
   2. **E-posta hizmeti yöneticileri** alanında, hizmet yöneticisinin ve tüm yardımcı yöneticilerin uyarı bildirimlerini almasını istiyorsanız **EVET'i** seçin.
   3. Ek **e-posta alıcıları** alanına, uyarı bildirimlerini alması gereken diğer tüm alıcıların e-posta adreslerini girin. *Birisi\@somewhere.com*biçiminde adları girin. E-posta adreslerini ayırmak için yarım sütunlar kullanın. Sanal aygıt başına en fazla 20 e-posta adresi yapılandırabilirsiniz.
      
       ![uyarıları bildirim yapılandırma](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Test e-postası bildirimi göndermek **için test e-postası gönder'i**tıklatın. StorSimple Device Manager hizmeti, test bildirimini ileterken durum iletilerini görüntüler.
      
       ![Uyarılar test bildirim e-postası gönderildi](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Test bildirimi iletisi gönderilemezse, StorSimple Device Manager hizmeti uygun bir ileti görüntüler. **Tamam'ı**tıklatın, birkaç dakika bekleyin ve ardından test bildirim iletinizi yeniden göndermeyi deneyin.
      >
      >
   5. Sayfanın alt kısmında, yapılandırmanızı kaydetmek için **Kaydet'i** tıklatın. Onayınız istendiğinde **Evet**’e tıklayın.
      
      ![Uyarılar test bildirim e-postası gönderildi](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Sık uyarı koşulları

StorSimple Virtual Array'iniz çeşitli koşullara yanıt olarak uyarılar oluşturur. En yaygın uyarı koşulları türleri şunlardır:

* **Bağlantı sorunları** – Bu uyarılar, veri aktarımında güçlük olduğunda oluşur. Azure depolama hesabına veri aktarımı sırasında veya sanal aygıtlar ile StorSimple Device Manager hizmeti arasında bağlantı olmaması nedeniyle iletişim sorunları oluşabilir. İletişim sorunları, düzeltilmesi en zor sorunlardan bazılarıdır, çünkü çok fazla başarısızlık noktası vardır. Daha gelişmiş sorun gidermeye devam etmeden önce her zaman ağ bağlantısının ve Internet erişiminin kullanılabilir olduğunu doğrulamanız gerekir. Bağlantı noktaları ve güvenlik duvarı ayarları hakkında daha fazla bilgi için [StorSimple Virtual Array sistem gereksinimlerine](storsimple-ova-system-requirements.md)gidin. Sorun giderme ile ilgili yardım için [Test-Bağlantı cmdlet ile Sorun](storsimple-troubleshoot-deployment.md)Giderme'ye gidin.
* **Performans sorunları** – Bu uyarılar, sisteminiz ağır bir yük altında yken olduğu gibi en iyi şekilde performans sergilemiyorsa neden olur.

Ayrıca, güvenlik, güncelleştirmeler veya iş hatalarıyla ilgili uyarılar da görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarılar, uyarı durumunun yaratacağı etkiye ve uyarıya yanıt verme gereksinimine bağlı olarak farklı önem düzeylerine sahiptir. Önem dereceleri şunlardır:

* **Kritik** – Bu uyarı, sisteminizin başarılı performansını etkileyen bir duruma yanıt olarak verilir. StorSimple hizmetinin kesintiye uğramadığından emin olmak için eylem gereklidir.
* **Uyarı** – Bu durum çözülmezse kritik hale gelebilir. Durumu araştırmalı ve sorunu gidermek için gerekli her türlü önlemi almalısınız.
* **Bilgi** - Bu uyarı, sisteminizi izleme ve yönetmede yararlı olabilecek bilgiler içerir.

## <a name="view-and-track-alerts"></a>Uyarıları görüntüleme ve izleme

StorSimple Device Manager servis özet bıçağı, önem düzeyine göre düzenlenmiş sanal cihazlarınızdaki uyarı sayısına hızlı bir bakış sağlar.

![Uyarılar panosu](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

**Uyarılar** bıçağını açmak için önem düzeyini tıklatın. Sonuçlar yalnızca bu önem düzeyiyle eşleşen uyarıları içerir.

![Uyarılar, uyarı türüne kapsamlı rapor](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Uyarı için uyarının en son ne zaman bildirilmesi, aygıttaki uyarının oluşum sayısı ve uyarıyı çözmek için önerilen eylem dahil olmak üzere ek ayrıntılar almak için listedeki bir uyarıyı tıklatın.

![Uyarılar listesi ve ayrıntıları](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Bilgileri Microsoft Destek'e göndermeniz gerekiyorsa, uyarı ayrıntılarını bir metin dosyasına kopyalayabilirsiniz. Öneriyi izledikten ve uyarı koşulunu şirket içinde çözdükten sonra, uyarıyı listeden temizlemeniz gerekir. Listeden uyarıyı seçin ve ardından **Temizle'yi**tıklatın. Birden çok uyarıyı temizlemek için her uyarıyı seçin, **Uyarı** sütunu dışında herhangi bir sütunu tıklatın ve ardından temizlenecek tüm uyarıları seçtikten sonra **Temizle'yi** tıklatın.

**Temizle'yi**tıklattığınızda, uyarı ve sorunu gidermek için attığınız adımlar hakkında yorum sağlama fırsatına sahip olursunuz.

![uyarı yorumları](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Başka bir olay yeni bilgilerle tetiklenirse, bazı olaylar sistem tarafından temizlenir.

## <a name="sort-and-review-alerts"></a>Uyarıları sıralama ve inceleme

**Alerts** blade en fazla 250 uyarı görüntüleyebilir. Bu uyarı sayısını aştıysanız, tüm uyarılar varsayılan görünümde görüntülenmez. Hangi uyarıların görüntülendiğini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – **Etkin** veya **Temizlenmiş** uyarıları görüntüleyebilirsiniz. Sisteminizde etkin uyarılar hala tetiklenirken, temizlenen uyarılar bir yönetici tarafından el ile temizlenir veya sistem uyarı koşulunu yeni bilgilerle güncelleştirdiği için programlanabilir şekilde temizlenir.
* **Önem derecesi** – Tüm önem düzeyleri (kritik, uyarı, bilgi) veya yalnızca kritik uyarılar gibi belirli bir önem derecesine ilişkin uyarıları görüntüleyebilirsiniz.
* **Kaynak** – Tüm kaynaklardan gelen uyarıları görüntüleyebilir veya uyarıları hizmetten veya bir veya tüm sanal aygıtlardan gelenlerle sınırlandırabilirsiniz.
* **Zaman aralığı** – **Kimden** ve **To** tarihleri ve zaman damgalarını belirterek, ilgilendiğiniz zaman dilimindeki uyarılara bakabilirsiniz.

## <a name="alerts-quick-reference"></a>Uyarıları hızlı başvuru

Aşağıdaki tablolarda karşılaşabileceğiniz Bazı StorSimple uyarılarının yanı sıra varsa ek bilgi ve öneriler listelenmektedir. StorSimple Virtual Array uyarıları aşağıdaki kategorilerden birine girer:

* [Bulut bağlantısı uyarıları](#cloud-connectivity-alerts)
* [Yapılandırma uyarıları](#configuration-alerts)
* [İş hatası uyarıları](#job-failure-alerts)
* [Performans uyarıları](#performance-alerts)
* [Güvenlik uyarıları](#security-alerts)

### <a name="cloud-connectivity-alerts"></a>Bulut bağlantısı uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Aygıt <*cihaz adı*> buluta bağlı değildir. |Adlandırılmış aygıt buluta bağlanamaz. |Buluta bağlanamadı. Bunun nedeni, aşağıdakilerden biri olabilir:<ul><li>Cihazınızdaki ağ ayarlarıyla ilgili bir sorun olabilir.</li><li>Depolama hesabı kimlik bilgileriyle ilgili bir sorun olabilir.</li></ul>Sorun giderme bağlantısı sorunları hakkında daha fazla bilgi için aygıtın [yerel web kullanıcı arabirimi](storsimple-ova-web-ui-admin.md) adresine gidin. |

### <a name="configuration-alerts"></a>Yapılandırma uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Şirket içi sanal aygıt yapılandırması desteklenmez. |Yavaş performans. |Geçerli yapılandırma performans düşüşüne neden olabilir. Sunucunuzun minimum yapılandırma gereksinimlerini karşıladığından emin olun. Daha fazla bilgi için [StorSimple Sanal Dizi Gereksinimleri'ne](storsimple-ova-system-requirements.md)gidin. |
| *<aygıt adında*\>sağlanan disk alanınız tükeniyor. |Disk alanı uyarısı. |Size sağlanan disk alanı azalıyor. Alanı boşaltmak için iş yüklerini başka bir büyükluğa taşımayı veya verileri paylaşmayı veya silmeyi düşünün. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| <*aygıt adının* \> yedeklemesi tamamlanamadı. |Yedekleme iş hatası. |Yedek oluşturamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Bağlantı sorunları yedekleme işleminin başarıyla tamamlanmasını engelliyor olabilir. Bağlantı sorunları olmadığından emin olun. Sorun giderme bağlantısı sorunları hakkında daha fazla bilgi için sanal aygıtınız için [yerel web kullanıcı arabirimi](storsimple-ova-web-ui-admin.md) adresine gidin.</li><li>Kullanılabilir depolama sınırına ulaştınız. Yer açmak için, artık ihtiyaç duyulmadığı yedeklemeleri silmeyi düşünün.</li></ul> Sorunları çözün, uyarıyı temizleyin ve işlemi yeniden deneyin. |
| <aygıt *adının* \> klonunu tamamlayamadı. |Klon iş başarısızlığı. |Bir klon oluşturamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Yedek listeniz geçerli olmayabilir. Hala geçerli olduğunu doğrulamak için listeyi yenileyin.</li><li>Bağlantı sorunları, klon işleminin başarıyla tamamlanmasını engelliyor olabilir. Bağlantı sorunları olmadığından emin olun.</li><li>Kullanılabilir depolama sınırına ulaştınız. Yer açmak için, artık ihtiyaç duyulmadığı yedeklemeleri silmeyi düşünün.</li></ul>Sorunları çözün, uyarıyı temizleyin ve işlemi yeniden deneyin. |

### <a name="networking-alerts"></a>Ağ uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Kimlik doğrulama hizmetine bağlanamadı. |Datapath hatası |Kimlik doğrulaması için kullanılan URL'ye erişilemez. Güvenlik duvarı kurallarınızın StorSimple aygıtı için belirtilen URL desenlerini içerdiğinden emin olun. Azure portalındaki URL desenleri hakkında daha fazla bilgi için [StorSimple Virtual Array ağ gereksinimlerine](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules)gidin.|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| Veri aktarımında beklenmeyen gecikmeler yaşıyorsunuz. |Yavaş veri aktarımı. |Bir depolama hizmetinin ölçeklenebilirlik hedeflerini aştığınızda azaltma hataları oluşur. Depolama hizmeti, tek bir istemcinin veya kiracının hizmeti başkalarının pahasına kullanamamasını sağlamak için bunu yapar. Azure depolama hesabınızla ilgili daha fazla bilgi için [Microsoft Azure Depolama Alanını İzle, tanı ve sorun giderme](../storage/common/storage-monitoring-diagnosing-troubleshooting.md)'ye gidin. |
| *<aygıt adında*\>yerel rezervasyon disk alanı azalıyor. |Yavaş tepki süresi. |<*aygıt adı* \> için verilen toplam boyutun %10'u yerel aygıtta ayrılmıştır ve artık ayrılmış alan da azalmaktadır. <aygıt *adındaki* \> iş yükü daha yüksek bir karmaşa oranı oluşturuyor veya yakın zamanda büyük miktarda veriyi geçirmiş olabilirsiniz. Bu, performansın düşmesine neden olabilir. Bunu çözmek için aşağıdaki eylemlerden birini göz önünde bulundurun:<ul><li>Bu aygıta bulut bant genişliğini artırın.</li><li>İş yüklerini azaltın veya başka bir büyük hacimveya paylaşıma taşıyın.</li></ul> |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi / önerilen eylemler |
|:--- |:--- |:--- |
| *<cihaz adının* \> parolası <*birkaç* \> gün içinde sona erer. |Parola uyarısı. |Parolanızın süresi <*birkaç* \> gün içinde sona erer. Parolanızı değiştirmeyi düşünün. Daha fazla bilgi için [StorSimple Virtual Array aygıt yöneticisi parolasını değiştir'e](storsimple-virtual-array-change-device-admin-password.md)gidin. |

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple Sanal Dizi hakkında bilgi edinin.](storsimple-ova-overview.md)
