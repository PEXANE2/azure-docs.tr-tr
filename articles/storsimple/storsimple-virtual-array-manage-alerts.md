---
title: StorSimple Sanal dizisi için uyarıları görüntüleyin ve yönetin
description: StorSimple Sanal dizisi uyarı koşullarını ve önem derecesini ve bu bildirimleri yönetmek için StorSimple Yöneticisi hizmetini kullanmayı açıklar.
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
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78365745"
---
# <a name="use-storsimple-device-manager-to-manage-alerts-for-the-storsimple-virtual-array"></a>StorSimple Sanal dizisine yönelik uyarıları yönetmek için StorSimple Aygıt Yöneticisi kullanma

## <a name="overview"></a>Genel Bakış

StorSimple Aygıt Yöneticisi hizmetindeki uyarılar özelliği, StorSimple Sanal dizileri ile ilgili uyarıları gerçek zamanlı olarak gözden geçirmeniz ve temizlemeniz için bir yol sağlar. StorSimple Sanal dizilerinizdeki sistem durumu sorunlarını ve genel Microsoft Azure StorSimple çözümünü merkezi olarak izlemek için **hizmet Özeti** dikey penceresindeki uyarıları kullanabilirsiniz.

Bu öğreticide, uyarı bildirimlerinin, ortak uyarı koşullarının, uyarı önem düzeylerinin ve uyarıların nasıl görüntüleneceği ve izleneceği açıklanır. Ayrıca, belirli bir uyarıyı hızlıca bulmanıza ve uygun şekilde yanıt vermenize olanak sağlayan uyarı hızlı başvuru tabloları da dahildir.

![Uyarılar sayfası](./media/storsimple-virtual-array-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Uyarı ayarlarını yapılandırma

StorSimple Sanal dizilerinizdeki her biri için uyarı koşullarının e-postayla bildirilmesini isteyip istemediğinizi seçebilirsiniz. Ayrıca, diğer uyarı bildirimi alıcılarını, e-posta adreslerini, noktalı virgülle ayırarak **ek e-posta alıcıları** kutusuna girerek de tanımlayabilirsiniz.

> [!NOTE]
> Her sanal dizi için en fazla 20 e-posta adresi girebilirsiniz.

Bir sanal dizi için e-posta bildirimini etkinleştirdikten sonra, bildirim listesinin üyeleri kritik bir uyarı oluştuğunda bir e-posta iletisi alır. Mesajlar *StorSimple-Alerts-noreply\@mail.windowsazure.com* 'dan gönderilir ve uyarı koşulunu anlatmaktadır. Alıcılar e-posta bildirim listesinden kaldırmak için **abonelik kaldırma** ' ya tıklayabilir.

#### <a name="to-enable-email-notification-for-alerts"></a>Uyarılar için e-posta bildirimini etkinleştirmek için

1. StorSimple Aygıt Yöneticisi hizmetinize gidin ve **Yönetim** bölümünde **cihazlar**' ı seçin ve tıklatın. Görüntülenmiş cihaz listesinden cihazınızı seçin ve tıklayın.
   
    ![Uyarı ayarları](./media/storsimple-virtual-array-manage-alerts/alerts2.png)
2. Bu, **Ayarlar** dikey penceresini açar. **Cihaz ayarları** bölümünde **genel**' i seçin. Bu, **Genel ayarlar** dikey penceresini açar.
   
    ![Uyarılar bildirim yapılandırması](./media/storsimple-virtual-array-manage-alerts/alerts4.png)
3. **Genel ayarlar** dikey penceresinde **Uyarı ayarları** bölümüne gidin ve şunları ayarlayın:
   
   1. **E-posta bildirimini etkinleştir** alanında **Evet**' i seçin.
   2. Hizmet yöneticisinin ve tüm ortak yöneticilerin uyarı bildirimlerini almasını istiyorsanız **e-posta hizmeti yöneticileri** alanında **Evet** ' i seçin.
   3. **Ek e-posta alıcıları** alanına, uyarı bildirimlerini alması gereken diğer tüm alıcıların e-posta adreslerini girin. Adları *birisi\@Somewhere.com*biçiminde girin. E-posta adreslerini ayırmak için noktalı virgül kullanın. Sanal cihaz başına en fazla 20 e-posta adresi yapılandırabilirsiniz.
      
       ![Uyarılar bildirim yapılandırması](./media/storsimple-virtual-array-manage-alerts/alerts6.png)
   4. Sınama e-postası bildirimi göndermek için **Test e-postası gönder ' e**tıklayın. StorSimple Aygıt Yöneticisi hizmeti, test bildirimini ileten durum iletilerini görüntüler.
      
       ![Uyarı sınama bildirimi e-postası gönderildi](./media/storsimple-virtual-array-manage-alerts/alerts7.png)
      
      > [!NOTE]
      > Test bildirim iletisi gönderilemezse, StorSimple Aygıt Yöneticisi hizmetinde uygun bir ileti görüntülenir. **Tamam**' a tıklayın, birkaç dakika bekleyin ve ardından test bildirim iletinizi yeniden göndermeye çalışın.
      >
      >
   5. Yapılandırmanızı kaydetmek için sayfanın alt kısmındaki **Kaydet** ' e tıklayın. Onayınız istendiğinde **Evet**’e tıklayın.
      
      ![Uyarı sınama bildirimi e-postası gönderildi](./media/storsimple-virtual-array-manage-alerts/alerts10.png)

## <a name="common-alert-conditions"></a>Ortak uyarı koşulları

StorSimple Sanal diziniz, çeşitli koşullara yanıt olarak uyarılar oluşturur. En yaygın uyarı koşulları türleri şunlardır:

* **Bağlantı sorunları** : Bu uyarılar, verileri aktarma konusunda zorluk oluşması durumunda oluşur. Azure depolama hesabına veri aktarımı sırasında veya sanal cihazlarla StorSimple Aygıt Yöneticisi hizmeti arasında bağlantı olmaması nedeniyle iletişim sorunları oluşabilir. Çok sayıda hata noktası olduğundan, iletişim sorunları düzeltilemeyecek bir kısmı. Daha gelişmiş sorun giderme işlemlerine devam etmeden önce ağ bağlantısı ve Internet erişiminin kullanılabilir olduğunu her zaman ilk önce doğrulamanız gerekir. Bağlantı noktaları ve güvenlik duvarı ayarları hakkında bilgi için, [StorSimple Sanal dizi sistem gereksinimleri](storsimple-ova-system-requirements.md)' ne gidin. Sorun gidermeye yönelik yardım için, [Test-Connection cmdlet 'iyle sorun giderme](storsimple-troubleshoot-deployment.md)bölümüne gidin.
* **Performans sorunları** : Bu uyarılar, sisteminiz ağır bir yük altında olduğunda olduğu gibi en iyi şekilde gerçekleştirimediğinde oluşur.

Ayrıca, güvenlik, güncelleştirmeler veya iş hatalarıyla ilgili uyarıları görebilirsiniz.

## <a name="alert-severity-levels"></a>Uyarı önem düzeyleri

Uyarılar, uyarı durumunun sahip olacağı etkiye ve uyarıya yanıt gereksinimine bağlı olarak farklı önem düzeylerine sahiptir. Önem düzeyleri şunlardır:

* **Kritik** – bu uyarı, sisteminizin başarılı performansını etkileyen bir koşula yanıt olarak yapılır. StorSimple hizmetinin kesilmediğinden emin olmak için eylem gerekir.
* **Uyarı** – bu durum çözümlenmezse kritik hale gelebilir. Durumu araştırmalı ve sorunu temizlemek için gereken tüm işlemleri gerçekleştirmeniz gerekir.
* **Bilgi** – bu uyarı, sisteminizi izlemek ve yönetmek için yararlı olabilecek bilgiler içerir.

## <a name="view-and-track-alerts"></a>Uyarıları görüntüle ve izle

StorSimple Aygıt Yöneticisi hizmeti Özet dikey penceresi, sanal cihazlarınızda önem düzeyine göre düzenlenmiş uyarı sayısına hızlı bir bakış sağlar.

![Uyarılar panosu](./media/storsimple-virtual-array-manage-alerts/alerts14.png)

Önem düzeyi ' ne tıklayarak **Uyarılar** dikey penceresini açın. Sonuçlar yalnızca bu önem düzeyiyle eşleşen uyarıları içerir.

![Uyarı türüne göre kapsamlandırılan uyarılar raporu](./media/storsimple-virtual-array-manage-alerts/alerts15.png)

Uyarının en son ne zaman bildirildiğini, cihazdaki uyarının oluşum sayısını ve uyarıyı çözümlemek için önerilen eylemi içeren uyarı hakkında ek ayrıntılar almak için listedeki bir uyarıya tıklayın.

![Uyarı listesi ve Ayrıntılar](./media/storsimple-virtual-array-manage-alerts/alerts16.png)

Bilgileri Microsoft Desteği göndermeniz gerekiyorsa, uyarı ayrıntılarını bir metin dosyasına kopyalayabilirsiniz. Öneriyi uyguladıktan ve şirket içi uyarı koşulunu çözümledikten sonra, listeden uyarıyı temizlemeniz gerekir. Listeden uyarıyı seçin ve ardından **Temizle**' ye tıklayın. Birden çok uyarıyı temizlemek için, her bir uyarıyı seçin, **Uyarı** sütunu dışında herhangi bir sütuna tıklayın ve sonra temizlenecek tüm uyarıları seçtikten sonra **Temizle** ' ye tıklayın.

**Temizle**' ye tıkladığınızda, uyarı ve sorunu çözmek için gerçekleştirdiğiniz adımlar hakkında açıklama sağlama fırsatına sahip olursunuz.

![Uyarı açıklamaları](./media/storsimple-virtual-array-manage-alerts/alerts17.png)

Yeni bilgilerle başka bir olay tetikleniyorsa, bazı olaylar sistem tarafından temizlenir.

## <a name="sort-and-review-alerts"></a>Uyarıları sıralama ve gözden geçirme

**Uyarılar** dikey penceresinde en fazla 250 uyarı görüntülenebilir. Bu uyarı sayısını aştıysanız, varsayılan görünümde tüm uyarılar gösterilmez. Hangi uyarıların görüntülendiğini özelleştirmek için aşağıdaki alanları birleştirebilirsiniz:

* **Durum** – **etkin** ya da **temizlenmiş** uyarıları görüntüleyebilirsiniz. Etkin uyarıların hala sisteminizde tetiklenmesi, temizlenmiş uyarıların bir yönetici tarafından el ile temizlenmesi veya sistem uyarı koşulunu yeni bilgilerle güncelleştirmesi nedeniyle program aracılığıyla temizlenmiş olması gerekir.
* **Önem derecesi** – tüm önem düzeyleri (kritik, uyarı, bilgi) veya yalnızca kritik uyarılar gibi belirli bir önem derecesindeki uyarıları görüntüleyebilirsiniz.
* **Kaynak** : tüm kaynaklardaki uyarıları görüntüleyebilir veya uyarıları hizmetten ya da bir sanal cihazdan gelen olanlarla sınırlayabilirsiniz.
* **Zaman aralığı** : **Kimden** ve **bitiş** tarihleri ve zaman damgaları belirterek, ilgilendiğiniz zaman dilimi sırasında uyarılara bakabilirsiniz.

## <a name="alerts-quick-reference"></a>Uyarılar hızlı başvuru

Aşağıdaki tablolarda, karşılaşabileceğiniz StorSimple uyarılarından bazıları ve kullanılabilir olduğunda ek bilgi ve öneriler listelenmektedir. StorSimple Sanal dizi uyarıları aşağıdaki kategorilerden birine girer:

* [Bulut bağlantısı uyarıları](#cloud-connectivity-alerts)
* [Yapılandırma uyarıları](#configuration-alerts)
* [İş hatası uyarıları](#job-failure-alerts)
* [Performans uyarıları](#performance-alerts)
* [Güvenlik uyarıları](#security-alerts)

### <a name="cloud-connectivity-alerts"></a>Bulut bağlantısı uyarıları

| Uyarı metni | Olay | Daha fazla bilgi/Önerilen Eylemler |
|:--- |:--- |:--- |
| > Cihaz < cihaz*adı*buluta bağlı değil. |Adlandırılmış cihaz buluta bağlanamıyor. |Buluta bağlanılamadı. Bunun nedeni, aşağıdakilerden biri olabilir:<ul><li>Cihazınızda ağ ayarlarıyla ilgili bir sorun olabilir.</li><li>Depolama hesabı kimlik bilgileriyle ilgili bir sorun olabilir.</li></ul>Bağlantı sorunlarını giderme hakkında daha fazla bilgi için, cihazın [Yerel Web Kullanıcı arabirimine](storsimple-ova-web-ui-admin.md) gidin. |

### <a name="configuration-alerts"></a>Yapılandırma uyarıları

| Uyarı metni | Olay | Daha fazla bilgi/Önerilen Eylemler |
|:--- |:--- |:--- |
| Şirket içi sanal cihaz yapılandırması desteklenmiyor. |Yavaş performans. |Geçerli yapılandırma performans düşüşüne neden olabilir. Sunucunuzun en düşük yapılandırma gereksinimlerini karşıladığından emin olun. Daha fazla bilgi için, [StorSimple Sanal dizi gereksinimlerine](storsimple-ova-system-requirements.md)gidin. |
| <*Cihaz adı*\>sağlanan disk alanından çalışıyorsunuz. |Disk alanı uyarısı. |Sağlanan disk alanını azalmış olarak çalıştırıyorsunuz. Alan boşaltmak için iş yüklerini başka bir birime taşımayı veya verileri paylaşmayı veya silmeyi düşünün. |

### <a name="job-failure-alerts"></a>İş hatası uyarıları

| Uyarı metni | Olay | Daha fazla bilgi/Önerilen Eylemler |
|:--- |:--- |:--- |
| <*Cihaz adı*\> yedeklemesi tamamlanamadı. |Yedekleme işi hatası. |Yedek oluşturulamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Bağlantı sorunları yedekleme işleminin başarıyla tamamlanmasını engelliyor olabilir. Bağlantı sorunları olmadığından emin olun. Bağlantı sorunlarını giderme hakkında daha fazla bilgi için sanal cihazınızın [Yerel Web Kullanıcı arabirimine](storsimple-ova-web-ui-admin.md) gidin.</li><li>Kullanılabilir depolama sınırına ulaştınız. Alan boşaltmak için artık gerekli olmayan yedeklemeleri silmeyi göz önünde bulundurun.</li></ul> Sorunları çözümleyin, uyarıyı temizleyin ve işlemi yeniden deneyin. |
| <*Cihaz adı*\> kopyası tamamlanamadı. |Kopyalama işi hatası. |Kopya oluşturulamadı. Aşağıdakilerden birini göz önünde bulundurun:<ul><li>Yedekleme listeniz geçerli olmayabilir. Hala geçerli olduğunu doğrulamak için listeyi yenileyin.</li><li>Bağlantı sorunları kopyalama işleminin başarıyla tamamlanmasını engelliyor olabilir. Bağlantı sorunları olmadığından emin olun.</li><li>Kullanılabilir depolama sınırına ulaştınız. Alan boşaltmak için artık gerekli olmayan yedeklemeleri silmeyi göz önünde bulundurun.</li></ul>Sorunları çözümleyin, uyarıyı temizleyin ve işlemi yeniden deneyin. |

### <a name="networking-alerts"></a>Ağ uyarıları

| Uyarı metni | Olay | Daha fazla bilgi/Önerilen Eylemler |
|:--- |:--- |:--- |
| Kimlik doğrulama hizmetine bağlanılamadı. |Veri yolu hatası |Kimlik doğrulaması için kullanılan URL 'ye ulaşılamıyor. Güvenlik Duvarı kurallarınızın StorSimple cihazı için belirtilen URL düzenlerini içerdiğinden emin olun. Azure portal URL desenleri hakkında daha fazla bilgi için, [StorSimple Sanal dizi ağı gereksinimlerine](storsimple-ova-system-requirements.md#url-patterns-for-firewall-rules)gidin.|

### <a name="performance-alerts"></a>Performans uyarıları

| Uyarı metni | Olay | Daha fazla bilgi/Önerilen Eylemler |
|:--- |:--- |:--- |
| Veri aktarımında beklenmeyen gecikmeler yaşanıyor. |Yavaş veri aktarımı. |Bir depolama hizmetinin ölçeklenebilirlik hedeflerini aşarsanız azaltma hataları oluşur. Depolama hizmeti, tek bir istemcinin veya kiracının hizmeti diğer masrafına uygun olmamasını sağlamak için bunu yapar. Azure depolama hesabınızda sorun giderme hakkında daha fazla bilgi için, [izleme, tanılama ve sorun giderme Microsoft Azure depolama](../storage/common/storage-monitoring-diagnosing-troubleshooting.md)gidin. |
| <*Cihaz adı*\>üzerinde yerel ayırma diski alanı azalıyor. |Yavaş yanıt süresi. |<*Cihaz adı*\> için sağlanan toplam boyutun %10 ' unun yerel cihaza ayrılmış olması ve artık ayrılmış alanda azaldınız. <*Cihaz adı*\> iş yükü daha yüksek bir karmaşıklık oranı oluşturuyor veya yakın zamanda büyük miktarda veri geçirdiniz. Bu, performansın düşmesine neden olabilir. Bu sorunu çözmek için aşağıdaki eylemlerden birini göz önünde bulundurun:<ul><li>Bu cihaz için bulut bant genişliğini artırın.</li><li>İş yüklerini başka bir birime veya paylaşıma düşürün veya taşıyın.</li></ul> |

### <a name="security-alerts"></a>Güvenlik uyarıları

| Uyarı metni | Olay | Daha fazla bilgi/Önerilen Eylemler |
|:--- |:--- |:--- |
| <*Cihaz adı*\> parolasının sonunda <*sayı*\> gün sonra dolacak. |Parola uyarısı. |Parolanızın <*sayı*\> gün sonra dolacak. Parolanızı değiştirmeyi göz önünde bulundurun. Daha fazla bilgi için, [StorSimple Sanal dizisi cihaz yönetici parolasını değiştirme](storsimple-virtual-array-change-device-admin-password.md)bölümüne gidin. |

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple Sanal dizisi hakkında bilgi edinin](storsimple-ova-overview.md).
