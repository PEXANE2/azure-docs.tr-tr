---
title: Azure Depolama hizmetlerini Depolama için Azure Monitörü ile izleyin (önizleme)| Microsoft Dokümanlar
description: Bu makalede, depolama yöneticilerine Azure Depolama hesaplarıyla ilgili performans ve kullanım sorunlarını hızlı bir şekilde anlama sağlayan Azure Depolama Denetimi özelliği açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/15/2019
ms.openlocfilehash: f23be7e764ad180a23c76abb7f9bb2218fd61e4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77662528"
---
# <a name="monitoring-your-storage-service-with-azure-monitor-for-storage-preview"></a>Depolama için Azure Monitörü ile depolama hizmetinizi izleme (önizleme)

Azure Depolama Için Monitör (önizleme), Azure Depolama hizmetleri performansınızın, kapasitenizin ve kullanılabilirliğinizin birleşik bir görünümünü sunarak Azure Depolama hesaplarınızın kapsamlı bir şekilde izlenmesini sağlar. Depolama kapasitesini ve performansı iki şekilde gözlemleyebilir, depolama hesabı gruplarını görmek için doğrudan bir depolama hesabından görüntüleyebilir veya Azure Monitor'dan görüntüleyebilirsiniz. 

Bu makale, Azure Monitor for Storage (önizleme) etkin noktalara odaklanma ve gecikme gecikmesini tanılama, azaltma, ölçek olarak Depolama hesaplarının sistem durumu ve performansı hakkında işlem uygulanabilir bilgiler elde etmek için sunduğu deneyimi anlamanıza yardımcı olacaktır. ve kullanılabilirlik sorunları.

## <a name="introduction-to-azure-monitor-for-storage-preview"></a>Depolama için Azure Monitöre Giriş (önizleme)

Deneyime dalmadan önce, bilgiyi nasıl sunduğunu ve görselleştirdiğini anlamalısınız. Depolama özelliğini doğrudan bir depolama hesabından veya Azure Monitör'den seçin, Azure Monitörü tutarlı bir deneyim sunar. 

Kombine sunar:

* **Depolama** hizmetinin veya API işleminin durumuna göre kullanılabilirliklerinin anlık görüntüsünü gösteren ölçek perspektifinde, depolama hizmetinin aldığı toplam istek sayısını gösteren kullanım ve depolama hizmetinin veya API işlem türünün istekleri işlemek için ortalama süresini gösteren gecikme. Kapasiteyi blob, dosya, tablo ve sıraya göre de görüntüleyebilirsiniz.

* Sorunları tanılamaya veya kategoriye göre ayrıntılı analizler gerçekleştirmeye yardımcı olmak için belirli bir depolama hesabının **analizini ayrıntılı** olarak analiz edin - kullanılabilirlik, performans, hatalar ve kapasite. Bu seçeneklerden herhangi birini seçmek, ölçümlerin ayrıntılı bir görünümünü sağlar.  

* Görmek istediğiniz ölçümleri değiştirebileceğiniz, değiştirebileceğiniz veya sınırlarınızla uyumlu eşikleri ayarlayabildiğiniz ve kendi çalışma kitabınız olarak kaydedebileceğiniz **özelleştirilebilir.** Çalışma kitabındaki grafikler Azure panosuna sabitlenebilir.  

Bu özellik herhangi bir şeyi etkinleştirmenizi veya yapılandırmanızı gerektirmez, depolama hesaplarınızdaki depolama ölçümleri varsayılan olarak toplanır. Azure Depolama'da bulunan ölçümleri bilmiyorsanız, [Azure depolama ölçümlerini](../../storage/common/storage-metrics-in-azure-monitor.md)inceleyerek Azure Depolama ölçümlerinde açıklamayı ve tanımı görüntüleyin.

>[!NOTE]
>Bu özelliğe erişmek için herhangi bir ücret alınmaz ve yalnızca Azure Monitor [fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/monitor/) sayfasında açıklandığı gibi, yapılandırmanız veya etkinleştirdiğiniz Azure Monitor temel özellikleri için ücretlendirilirsiniz.

>[!NOTE]
>Depolama için Azure Monitor [genel amaçlı v1 hesaplarını](../../storage/common/storage-account-overview.md#general-purpose-v1-accounts)desteklemez.
>

## <a name="view-from-azure-monitor"></a>Azure Monitör'den Görünüm

Azure Monitor'dan aboneliğinizdeki birden çok depolama hesabından hareket, gecikme ve kapasite ayrıntılarını görüntüleyebilir ve performans, kapasite sorunları ve hataları belirlemenize yardımcı olabilirsiniz.

Tüm aboneliklerinizde depolama hesaplarınızın kullanımını ve kullanılabilirliğini görüntülemek için aşağıdaki adımları gerçekleştirin.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Azure portalındaki sol bölmeden **Monitör'ü** seçin ve **Öngörüler** bölümünde **Depolama Hesapları 'nı (önizleme)** seçin.

    ![Birden çok depolama hesabı görünümü](./media/storage-insights-overview/multiple-storage-accounts-view-01.png)

### <a name="overview-workbook"></a>Genel bakış çalışma kitabı

Seçili abonelik için **Genel Bakış** çalışma kitabında, tablo, abonelik içinde gruplanmış en fazla 10 depolama hesabı için etkileşimli depolama ölçümleri ve hizmet kullanılabilirliği durumunu görüntüler. Sonuçları, aşağıdaki açılır listelerden seçtiğiniz seçeneklere göre filtreleyebilirsiniz:

* **Abonelikler** - yalnızca depolama hesabı olan abonelikler listelenir.  

* **Depolama Hesapları** - varsayılan olarak, 10 depolama hesabı önceden seçilir. Kapsam seçicide tüm ünü veya birden çok depolama hesabını seçerseniz, en fazla 200 depolama hesabı döndürülür. Örneğin, seçtiğiniz üç abonelikte toplam 573 depolama hesabınız varsa, yalnızca 200 hesap görüntülenir. 

* **Zaman Aralığı** - varsayılan olarak, yapılan ilgili seçimlere göre son 4 saatlik bilgileri görüntüler.

Açılan listelerin altındaki sayaç döşemesi, abonelikteki toplam depolama hesabı sayısını kaydeder ve toplamın kaç tanesinin seçildiğini yansıtır. Çalışma kitabında işlem ölçümlerini veya hataları bildiren sütunlar için koşullu renk kodlama veya ısı eşlemleri vardır. En derin renk en yüksek değere sahiptir ve daha açık bir renk en düşük değerlere dayanır. Hata tabanlı sütunlar için değer kırmızı, metrik tabanlı sütunlar için değer mavi renktedir.

Sütunlarda **kullanılabilirlik,** **E2E Gecikme Alanı**, **Sunucu Gecikmesi**ve **işlem hatası türü/Hatalar'da** bir değer seçin ve sizi bu depolama hesabı için seçilen sütunla eşleşen belirli depolama ölçümleri türüne uygun bir rapora yönlendirir. Her kategori için çalışma kitapları hakkında daha fazla bilgi için aşağıdaki [Ayrıntılı depolama çalışma kitapları](#detailed-storage-workbooks) bölümüne bakın. 

>[!NOTE]
>Raporda hangi hataların gösterilebildiği yle ilgili ayrıntılar için [Yanıt Türü şemasına](../../storage/common/storage-metrics-in-azure-monitor.md#metrics-dimensions) bakın ve **ServerOtherError , ClientOtherError**, **ClientThrottlingError**gibi yanıt türlerini arayın. **ClientOtherError** Seçilen depolama hesaplarına bağlı olarak, bildirilen üçten fazla hata türü varsa, diğer tüm hatalar **Diğer**.

Varsayılan **Kullanılabilirlik** eşiği:

* Uyarı - 99%
* Kritik - 90%

Gözlemveya gereksinimlerinizin sonuçlarına göre bir kullanılabilirlik eşiği ayarlamak için, [kullanılabilirlik eşiğini gözden geçirin.](#modify-the-availability-threshold) 

### <a name="capacity-workbook"></a>Kapasite çalışma kitabı

Sayfanın üst kısmında **Kapasite'yi** seçin ve **Kapasite** çalışma kitabı açılır. Hesaptaki her veri hizmeti tarafından kullanılan toplam depolama miktarını ve kullanılan depolamanın üzerinde ve altında belirlemeye yardımcı olmak için kullanılan kapasiteyi gösterir.

![Birden fazla depolama hesabı Kapasite çalışma kitabı](./media/storage-insights-overview/storage-account-capacity-02.png) 

Çalışma kitabında, mavi değeri olan kapasite ölçümlerini bildiren sütunlar için koşullu renk kodlama veya ısı haritaları vardır. En derin renk en yüksek değere sahiptir ve daha açık bir renk en düşük değerlere dayanır.

Çalışma kitabındaki sütunlardan herhangi birinin altında bir değer seçtiğinizde, depolama hesabı için **Kapasite** çalışma kitabına ayrıntılanır. Ayrıntılı [depolama çalışma kitapları](#detailed-storage-workbooks) bölümünde ayrıntılı olarak açıklanmıştır. 

## <a name="view-from-a-storage-account"></a>Depolama hesabından görüntüleme

VM'ler için Azure Monitor'a doğrudan bir depolama hesabından erişmek için:

1. Azure portalında Depolama hesapları'nı seçin.

2. Listeden bir depolama hesabı seçin. İzleme bölümünde, Öngörüler 'i (önizleme) seçin.

    ![Seçili depolama hesabı Genel Bakış sayfası](./media/storage-insights-overview/storage-account-direct-overview-01.png)

Depolama hesabı için **Genel Bakış** çalışma kitabında, hızlı bir şekilde değerlendirmenize yardımcı olan çeşitli depolama performansı ölçümleri gösterir:

* **Denetiminizin** dışındaki bir sorunun, Özet sütununda belirtilen, dağıtıldığı bölgedeki Depolama hizmetini etkileyip etkilemediği ne olduğunu hemen görmek için Depolama hizmetinin sağlık durumu.

* Depolama kapasitesi, kullanılabilirlik, işlemler ve gecikme ile ilgili en önemli ayrıntıları gösteren etkileşimli performans grafikleri.  

* Hizmet kullanılabilirliğini, depolama hizmetine yapılan toplam işlem sayısını, E2E gecikmesini ve sunucu gecikmesini vurgulayan metrik ve durum kutucukları.

**Hatalar,** **Performans,** **Kullanılabilirlik**ve **Kapasite** için düğmelerden herhangi birini seçmek ilgili çalışma kitabını açar. 

![Seçili depolama hesabı Genel Bakış sayfası](./media/storage-insights-overview/storage-account-capacity-01.png)

## <a name="detailed-storage-workbooks"></a>Ayrıntılı depolama çalışma kitapları

Sütunlarda **kullanılabilirlik,** **E2E Gecikme Alanı**, **Sunucu Gecikmesi**ve birden çok depolama hesabı **Genel Bakış** çalışma kitabından işlem hatası **türü/Hatalar** veya belirli bir depolama hesabından **Genel Bakış** çalışma kitabından **Hatalar,** **Performans,** **Kullanılabilirlik**ve **Kapasite** düğmelerinden herhangi birini seçerek, her biri bu kategoriye uygun olarak tasarlanmış bir dizi etkileşimli depolamayla ilgili bilgi sunar.  

* **Kullanılabilirlik,** **Kullanılabilirlik** çalışma kitabını açar. Azure Depolama hizmetinin geçerli sistem durumu durumunu, depolama hesabında tanımlanan veri hizmetitarafından sınıflandırılan her nesnenin kullanılabilir sistem durumunu gösteren bir tabloyu, seçilen zaman aralığını temsil eden bir eğilim çizgisi ve kullanılabilirlik eğilim grafiğini gösterir hesaptaki her veri hizmeti.  

    ![Kullanılabilirlik raporu örneği](./media/storage-insights-overview/storage-account-availability-01.png)

* **E2E Gecikme ve** **Sunucu Gecikmesi** **Performans** çalışma kitabını açar. E2E gecikmesi ve sunucu gecikmesi gösteren bir toplama durumu döşemesi, e2E ve sunucu gecikmesinin performans grafiği ni ve depolama hesabında tanımlanan veri hizmetiyle sınıflandırılan API tarafından başarılı çağrıların gecikmesini kıran bir tablo içerir.

    ![Performans raporu örneği](./media/storage-insights-overview/storage-account-performance-01.png)

* Kılavuzda listelenen hata kategorilerinden herhangi birini **seçmek, Hata** çalışma kitabını açar. Rapor, açıklananlar ve başarılı istekler, istemci azaltma hataları, ClientOtherError özniteliğine özgü hareket **Yanıt Türü** boyut ölçümü için bir performans grafiği ve iki tablo dışında diğer tüm istemci tarafındaki hataların metrik kutucuklarını gösterir ve iki tablo - **Yanıt türüne göre API adına** göre hareketler ve **Hareketler**.

   ![Hata raporu örneği](./media/storage-insights-overview/storage-account-failures-01.png)

* **Kapasite** **çalışma** kitabını açar. Kutucuklarda ve grafikte hesaptaki her depolama veri nesnesi için kullanılan toplam depolama miktarını ve hesapta kaç veri nesnesinin depolanırolduğunu gösterir.  

    ![Seçili depolama hesabı Kapasite sayfası](./media/storage-insights-overview/storage-account-capacity-01.png) 

## <a name="pin-and-export"></a>Pin ve dışa aktarma

Bölümün sağ üst kısmındaki toka simgesini seçerek metrik bölümlerden herhangi birini Azure Panosu'na sabitleyebilirsiniz.

![Pano örneğine metrik kesit pini](./media/storage-insights-overview/workbook-pin-example.png)

Çoklu abonelik ve depolama hesabı **Genel Bakış** veya **Kapasite** çalışma kitapları, pişensimgenin sağındaki aşağı ok simgesini seçerek sonuçları Excel biçiminde dışa aktarmayı destekler.

![Çalışma kitabı ızgara sonuçlarını dışa aktarma örneği](./media/storage-insights-overview/workbook-export-example.png)

## <a name="customize-azure-monitor-for-storage-preview"></a>Depolama için Azure Monitörünü Özelleştir (önizleme)

Bu bölümde, veri analizi gereksinimlerinizi desteklemek üzere özelleştirmek için çalışma kitabını düzenlemek için sık karşılaşılan senaryolar vurgulanır:

* Her zaman belirli bir abonelik veya depolama hesabı(lar) seçmek için çalışma kitabını kapsamın
* Kılavuzdaki ölçümleri değiştirme
* Kullanılabilirlik eşiğini değiştirme
* Renk oluşturmayı değiştirme

Özelleştirmeler, yayınlanan çalışma kitabımızda varsayılan yapılandırmanın üzerine yazılmasını önlemek için özel bir çalışma kitabına kaydedilir. Çalışma kitapları, sizin için özel olan **Raporlarım** bölümünde veya kaynak grubuna erişimi olan herkesin erişebileceği **Paylaşılan Raporlar** bölümünde bir kaynak grubuna kaydedilir. Özel çalışma kitabını kurtardıktan sonra, başlatmak için çalışma kitabı galerisine gitmeniz gerekir.

![Komut çubuğundan çalışma kitabı galerisini başlatın](./media/storage-insights-overview/workbook-command-bar-gallery.png)

### <a name="specifying-a-subscription-or-storage-account"></a>Abonelik veya depolama hesabı belirtme

Çoklu abonelik ve depolama hesabı **Genel Bakış** veya **Kapasite** çalışma kitaplarını her çalıştırmada belirli bir abonelik(ler) veya depolama hesabı(lar) kapsamına göre yapılandırabilir, aşağıdaki adımları gerçekleştirebilirsiniz.

1. Portaldan **Monitör'ü** seçin ve ardından sol bölmeden **Depolama Hesapları'nı (önizleme)** seçin.

2. Genel **Bakış** çalışma kitabında, komut çubuğundan **Edit'i**seçin.

3. Varsayılan olarak **olmasını** istediğiniz bir veya daha fazla abonelik abonelikaçılır listesinden seçim yap. Unutmayın, çalışma kitabı toplam 10 abonelik seçmeyi destekler.  

4. **Depolama Hesapları** açılır listesinden varsayılan olarak olmasını istediğiniz bir veya daha fazla hesabı seçin. Unutmayın, çalışma kitabı toplam 200 depolama hesabı seçmeyi destekler. 

5. Özelleştirilmiş çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **Kaydet'i** seçin ve ardından okuma moduna dönmek için **Bitti düzenlemesi'ni** tıklatın.  

### <a name="modify-metrics-and-colors-in-the-workbook"></a>Çalışma kitabındaki ölçümleri ve renkleri değiştirme

Önceden oluşturulmuş çalışma kitapları metrik veriler içerir ve görselleştirilmiş leştirilmiş lerden herhangi birini değiştirme veya kaldırma ve ekibinizin özel gereksinimlerine göre özelleştirme yeteneğine sahipsiniz.

Örneğimizde, aşağıdakileri göstermek için çoklu abonelik ve depolama hesabı kapasitesi çalışma kitabı yla birlikte çalışıyoruz:

* Bir metrik kaldırma
* Renk oluşturmayı değiştirme

Önceden oluşturulmuş **Hatalar,** **Performans,** **Kullanılabilirlik**ve **Kapasite** çalışma kitaplarından herhangi birine karşı aynı değişiklikleri gerçekleştirebilirsiniz.

1. Portaldan **Monitör'ü** seçin ve ardından sol bölmeden **Depolama Hesapları'nı (önizleme)** seçin.

2. Kapasite çalışma kitabına geçmek için **Kapasite'yi** seçin ve komut çubuğundan komut çubuğundan **Edit'i** seçin.

    ![Çalışma kitabını değiştirmek için edit'i seçme](./media/storage-insights-overview/workbook-edit-workbook.png)

3. Ölçümler bölümünün **yanında, Edit'i**seçin.

    ![Kapasite çalışma kitabı ölçümlerini değiştirmek için Edit'i seçin](./media/storage-insights-overview/edit-metrics-capacity-workbook-01.png)

4. Kullanılan Hesap kullanılan **kapasite zaman çizelgesi** sütunundan kaldıracağız, bu nedenle ölçümler ızgarasında Sütun **Ayarları'nı** seçin.

    ![Sütun ayarlarını değiştir](./media/storage-insights-overview/edit-capacity-workbook-resource-grid.png)

5. Sütun ayarları nı **edin** bölmesinde, **microsoft.storage/storageaccounts-Capacity-UsedCapacity** **Timeline$| Hesap kullanılan kapasite Zaman Çizelgesi$** ve açılan liste **sütun renderer** altında **Gizli**seçin.

6. **Kaydet'i** seçin ve değişikliğinizi gerçekleştirmek için kapatın.

Şimdi, rapordaki kapasite ölçümlerinin renk temasını mavi yerine yeşil kullanmak üzere değiştirelim.

1. Metrikler tablosunda **Sütun Ayarları'nı** seçin.

2. Sütun **ayarları** bölmesinde, **Sütunlar** bölümü altında **microsoft.storage/storageaccounts-Capacity-UsedCapacity$|microsoft.storage/storageaccounts/blobservices-Capacity-BlobCapacity$|microsoft.storage/storageaccounts/fileservices-Capacity-FileCapacity$|microsoft.storage/storageaccounts/queueservices-Capacity-QueueCapacity$|microsoft.storage/storage/tableservices-Capacity-TableCapacity$** seçeneğini belirleyin. Açılan liste Renk **paletinin**altında **Yeşil'i**seçin.

3. **Kaydet'i** seçin ve değişikliğinizi gerçekleştirmek için kapatın.

4. Özelleştirilmiş çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **Kaydet'i** seçin ve ardından okuma moduna dönmek için **Bitti düzenlemesi'ni** tıklatın.  

### <a name="modify-the-availability-threshold"></a>Kullanılabilirlik eşiğini değiştirme

Bu örnekte, depolama hesabı kapasitesi çalışma kitabıyla birlikte çalışıyoruz ve kullanılabilirlik eşiğini nasıl değiştirebileceğimizi gösteriyoruz. Varsayılan olarak, döşeme ve ızgara raporlama yüzdesi kullanılabilirliği en az 90 eşiği ve 99 maksimum eşik ile yapılandırılır. **API ad** ızgarasına göre **Kullanılabilirlik Durumu %'sinin** minimum eşik değerini %85 olarak değiştireceğiz, bu da eşik yüzde 85'ten azsa sağlık durumunun kritik yönde değiştiği anlamına gelir. 

1. Portaldan **Depolama hesaplarını** seçin ve ardından listeden bir depolama hesabı seçin.

2. Sol bölmeden **Öngörüler 'i (önizleme)** seçin.

3. Çalışma kitabında, kullanılabilirlik çalışma kitabına geçmek için **Kullanılabilirlik'i** seçin ve ardından komut çubuğundan **Edit'i** seçin. 

4. Sayfanın altına ve **api** ızgarasının yanındaki sol tarafa doğru ilerleyin, **Edit'i**seçin.

    ![Kullanılabilirliği API Adı Kılavuz ayarlarına göre düzenleme](./media/storage-insights-overview/availability-workbook-avail-by-apiname.png)

5. **Sütun ayarlarını** seçin ve ardından **Sütunları Edit** sütunu bölmesinde, **Sütunlar** bölümünün altında **Kullanılabilirlik 'i seçin (%) (Eşikler + Biçimlendirilmiş)**.

6. **Kritik** sistem durumu değerini **90'dan** **85'e** değiştirin ve sonra **Kaydet ve Kapat'ı**tıklatın.

    ![Kritik durum için kullanılabilirlik eşik değerini değiştirme](./media/storage-insights-overview/edit-column-settings-capacity-workbook-01.png)

7. Özelleştirilmiş çalışma kitabının bir kopyasını kaydetmek için komut çubuğundan **Kaydet'i** seçin ve ardından okuma moduna dönmek için **Bitti düzenlemesi'ni** tıklatın.

## <a name="troubleshooting"></a>Sorun giderme

Bu bölüm, Depolama için Azure Monitörü'nu (önizleme) kullanırken karşılaşabileceğiniz sık karşılaşılan sorunlardan bazılarının tanılanması ve sorun giderme konusunda size yardımcı olacaktır. Özel sorununla ilgili bilgileri bulmak için aşağıdaki listeyi kullanın.

### <a name="resolving-performance-capacity-or-availability-issues"></a>Performans, kapasite veya kullanılabilirlik sorunlarını çözme

Depolama için Azure Monitörü (önizleme) ile tanımladığınız depolamayla ilgili sorunları gidermek için Azure Depolama [sorun giderme kılavuzuna](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance)bakın.  

### <a name="why-can-i-only-see-200-storage-accounts"></a>Neden sadece 200 depolama hesabı görebiliyorum?

Seçili depolama hesabı sayısı, seçilen abonelik sayısına bakılmaksızın 200 sınırı vardır.

### <a name="what-happens-when-i-click-on-a-recently-pinned-tile-in-the-dashboard"></a>Panoda yakın zamanda sabitlenmiş bir döşemeye tıkladığımda ne olur?

* Döşemenin herhangi bir yerine tıklarsanız, sizi döşemenin sabitlendiği sekmeye götürür. Örneğin, bir grafiği "Depolama Hesabına Genel Bakış" sekmesine sabitlerseniz, panodaki o döşemeyi tıklattığınızda varsayılan görünümü açar, ancak kendi kayıtlı kopyanızdan bir grafiği sabitlerseniz, kayıtlı kopyanızın görünümünü açar.
* Başlığın sol üst kısmındaki filtre simgesi "Yapılandırılan döşeme ayarlarını" sekmesini açar.
* Sağ üstteki elips simgesi size "Başlık verilerini özelleştirmek", "özelleştirmek", "yenilemek" ve "panodan kaldırma" seçeneklerini sunar.

### <a name="what-happens-when-i-save-a-workbook"></a>Çalışma kitabını kaydettiğimde ne olur?

* Bir çalışma kitabını kaydettiğinizde, çalışma kitabının yeni bir kopyasını, yaptığınız değişikliklerle oluşturmanıza ve başlığı değiştirmenize olanak tanır. Kaydetme çalışma kitabının üzerine yazmıyor, geçerli çalışma kitabı her zaman varsayılan görünüm olacaktır.
* **Kaydedilmemiş** çalışma kitabı varsayılan görünümdür.


### <a name="why-dont-i-see-all-my-subscriptions-in-the-portal"></a>Neden tüm aboneliklerimi portalda göremiyorum?

Portal, yalnızca portal açılışında seçilen aboneliklerin verilerini gösterir. Hangi aboneliklerin seçili olduğunu değiştirmek için sağ üste gidin ve filtre simgesiyle not defterine tıklayın. Bu, Dizin + abonelikler sekmesini gösterir.

![Dizin + abonelik](./media/storage-insights-overview/fqa3.png)

### <a name="how-to-change-the-coloring-and-threshold-for-availability"></a>Kullanılabilirlik için boyama ve eşik nasıl değiştirilir?

Kullanılabilirlik için renklendirme ve eşikleri değiştirme konusunda ayrıntılı adımlar için [kullanılabilirlik eşiğini değiştir](storage-insights-overview.md#modify-the-availability-threshold) bölümüne bakın.

### <a name="how-to-analyze-and-troubleshoot-the-data-shown-in-azure-monitor-for-storage"></a>Depolama için Azure Monitor'da gösterilen verileri nasıl analiz edin ve sorun giderme?

 Azure Depolama Için Azure Depolama verilerinin nasıl analiz edileceği ve sorun giderilen ayrıntılar için Microsoft Azure Depolama makalesini [tanıla](https://docs.microsoft.com/azure/storage/common/storage-monitoring-diagnosing-troubleshooting) ve sorun gidermeye bakın.

### <a name="why-dont-i-see-all-the-types-of-errors-in-metrics"></a>Neden ölçümlerdeki tüm hata türlerini göremiyorum?

Şu anda, en fazla üç farklı hata türü gösterilir ve hataların geri kalanı tek bir kovada birlikte gruplandırılır. SplitByLimit kullanılarak denetlenir ve değiştirilebilir. Bu özelliği değiştirmek için:

1. Çalışma kitabını edit'e tıklayın.
2. Ölçümlere gidin, edit'e tıklayın ve ardından **Hareketler, Toplam** veya ne olursa olsun ölçümleri seçin.

    ![Ölçümlere gidin ve "İşlemler, Toplamlar" düğmesine tıklayın](./media/storage-insights-overview/fqa7.png)

1. Ardından Bölme Sayısını değiştirin.

    ![Metrik Parametreleri Seçin"](./media/storage-insights-overview/fqa7-2.png)

SplitByLimit'i n+1 olarak belirtmekten farklı hata türlerini görmek istiyorsanız, hataların geri kalanı için 1 ekstra.

###  <a name="i-saved-my-workbook-while-on-some-storage-account-why-cant-i-find-it-now"></a>Bazı Depolama Hesabındayken çalışma kitabımı kaydettim. Neden şimdi bulamıyorum?

Her çalışma kitabı kaydettiğiniz depolama hesabına kaydedilir. Kullanıcının çalışma kitabını kaydettiği belirli Depolama Hesabı'nı bulmaya çalışın. Aksi takdirde, kaynağı (depolama hesabı) bilmeden belirli bir çalışma kitabı nı bulmanın bir yolu yoktur.

### <a name="what-is-time-range"></a>Zaman aralığı nedir?

Zaman aralığı, belirli bir zaman dilimindeki verileri gösterir. Örneğin, zaman aralığı 24 saatse, son 24 saate ait verileri gösteriyor demektir.

### <a name="what-is-time-granularity-time-grain"></a>Zaman tanecikliliği (zaman taneciliği) nedir?

Zaman tanecikliliği, iki veri noktası arasındaki zaman farkıdır. Örneğin, zaman tanesi 1 saniyeolarak ayarlanmışsa, bu ölçümlerin her saniye toplandığı anlamına gelir.

### <a name="what-is-the-time-granularity-once-we-pin-any-part-of-the-workbooks-to-a-dashboard"></a>Çalışma kitaplarının herhangi bir bölümünü panoya sabitledikten sonra zaman ayırılitesi nedir?

Varsayılan zaman parçalı otomatik olarak ayarlanır, şu anda değiştirilemez.

### <a name="how-do-i-change-the-timespan-time-range-of-the-workbook-step-on-my-dashboard"></a>Panomdaki çalışma kitabı adımının zaman aralığını/ zaman aralığını nasıl değiştirebilirim?

Varsayılan olarak, pano kutucunuzdaki zaman aralığı/zaman aralığı 24 saat olarak ayarlanır, sağ üstteki elipslere bu tıklamayı değiştirmek için, **kutucuğu verileri özelleştir'i**seçin, "başlık düzeyindepano zaman ayarlarını geçersiz kıl" kutusunu işaretleyin ve açılır menüyü kullanarak bir zaman aralığı seçin.  

![Döşemenin sağ köşesindeki elipsleri seçin ve bu verileri Özelleştir'i seçin](./media/storage-insights-overview/fqa-data-settings.png)

![Yapılandırılan döşeme ayarlarında zaman aralığı/zaman aralığını değiştirmek için zaman aralığı açılır düşüşünü seçin](./media/storage-insights-overview/fqa-timespan.png)

### <a name="how-do-i-change-the-title-of-the-workbook-or-a-workbook-step-i-pinned-to-a-dashboard"></a>Çalışma kitabının veya panoya sabitlediğim çalışma kitabı adımının başlığını nasıl değiştiririm?

Bir panoya sabitlenen çalışma kitabı veya çalışma kitabı adımının başlığı, çalışma kitabındaki adı korur. Başlığı değiştirmek için çalışma kitabının kendi kopyasını kaydetmeniz gerekir. Daha sonra kaydet tuşuna basmadan önce çalışma kitabına isim verebilirsiniz.

![Çalışma kitabının bir kopyasını kaydetmek ve adını değiştirmek için en üstte kaydet'i seçin](./media/storage-insights-overview/fqa-change-workbook-name.png)

Kayıtlı çalışma kitabınızdaki bir adımın adını değiştirmek için adımın altında edin ve ardından ayarların en altındaki vitesi seçin.

![Ayarları](./media/storage-insights-overview/fqa-edit.png)
![açmak için çalışma kitabı adımının altındaki edit'i seçin Adım adını değiştirmek için en alttaki dişliyi seçin](./media/storage-insights-overview/fqa-change-name.png)

## <a name="next-steps"></a>Sonraki adımlar

* Sorunları algılamaya yardımcı olmak için otomatik uyarı ayarlamak için [metrik uyarıları](../platform/alerts-metric.md) ve hizmet [durumu bildirimlerini](../../service-health/alerts-activity-log-service-notifications.md) yapılandırın.

* Çalışma kitaplarının desteklemek için tasarladığı senaryoları öğrenin, yeni raporları nasıl yazarken ve özelleştirin ve [Azure Monitor çalışma kitaplarıyla etkileşimli raporlar oluştur'u](../app/usage-workbooks.md)gözden geçirerek daha fazlasını öğrenin.

* Azure Depolama ile ilgili sorunları tanımlamak, tanılamak ve sorun gidermek için Depolama Analizi'ni ve diğer araçları kullanma yla ilgili ayrıntılı bir kılavuz için Microsoft [Azure Depolama Depolamasını İzle, tanılama ve sorun giderme](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md)konusuna bakın.
