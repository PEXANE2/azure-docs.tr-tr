---
title: Microsoft Tehdit Modelleme Aracı özelliğine genel bakış - Azure
description: Tehdit Modelleme Aracı'nda bulunan tüm özellikler hakkında bilgi edinin
author: jegeib
ms.author: jegeib
ms.service: security
ms.subservice: security-develop
ms.topic: article
ms.date: 08/17/2017
ms.openlocfilehash: 88eea0eb2bceb0f6322381a94d4339cbdfa662c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552126"
---
# <a name="threat-modeling-tool-feature-overview"></a>Tehdit Modelleme Aracı özelliğine genel bakış

Tehdit Modelleme Aracı, tehdit modelleme ihtiyaçlarınız konusunda size yardımcı olabilir. Araca temel bir giriş için [bkz.](threat-modeling-tool-getting-started.md)

> [!NOTE]
>Tehdit Modelleme Aracı sık sık güncellenir, bu nedenle en son özelliklerimizi ve iyileştirmelerimizi görmek için bu kılavuzu sık sık kontrol edin.

Boş bir sayfa açmak için **Model Oluştur'u**seçin.

![Boş sayfa](./media/threat-modeling-tool-feature-overview/tmtstart.png)

Araçta şu anda mevcut olan özellikleri görmek [için, Başlat ı](threat-modeling-tool-getting-started.md) örneğinde ekibimiz tarafından oluşturulan tehdit modelini kullanın.

![Temel tehdit modeli](./media/threat-modeling-tool-feature-overview/basictmt.png)

## <a name="navigation"></a>Gezinti

Yerleşik özellikleri tartışmadan önce, araçta bulunan ana bileşenleri gözden geçirelim.

### <a name="menu-items"></a>Menü öğeleri

Bu deneyim diğer Microsoft ürünlerine benzer. Üst düzey menü öğelerini gözden geçirelim.

![Menü öğeleri](./media/threat-modeling-tool-feature-overview/menuitems.png)

| Etiketle                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Dosya** | <ul><li>Dosyaları açma, kaydetme ve kapatma</li><li>OneDrive hesaplarında oturum açın ve oturum açın.</li><li>Bağlantıları paylaşın (görüntüle ve edin).</li><li>Dosya bilgilerini görüntüleyin.</li><li>Varolan modellere yeni bir şablon uygulayın.</li></ul> |
| **Düzenle** | Eylemleri geri alave, kopyala ve silmek gibi eylemleri geri ala. |
| **Görünüm** | <ul><li>**Çözümleme** ve **Tasarım** görünümleri arasında geçiş yapın.</li><li>Kapalı pencereleri açın (örneğin, şablonlar, öğe özellikleri ve iletiler).</li><li>Düzeni varsayılan ayarlara sıfırlayın.</li></ul> |
| **Diyagram** | Diyagramlar ekleyin ve silin ve diyagramsekseklerinde hareket ettirin. |
| **Raporlar** | Başkalarıyla paylaşmak için HTML raporları oluşturun. |
| **Yardım** | Aracı kullanmanıza yardımcı olacak kılavuzları bulun. |

Semboller üst düzey menüler için kısayollar:

| Sembol                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Aç** | Yeni bir dosya açar. |
| **Kaydet** | Geçerli dosyayı kaydeder. |
| **Tasarım** | Modeller oluşturabileceğiniz **Tasarım** görünümünü açar. |
| **Çözümleme** | Oluşturulan tehditleri ve bunların özelliklerini gösterir. |
| **Diyagram ekle** | Yeni bir diyagram ekler (Excel'deki yeni sekmelere benzer). |
| **Diyagramı sil** | Geçerli diyagramı siler. |
| **Kopyala/Kes/Yapıştır** | Öğeleri kopyalar, keser ve yapıştırır. |
| **Geri/Yeniden Yap** | Eylemleri geri alır ve yeniden yapar. |
| **Yakınlaştır/Uzaklaştır** | Daha iyi bir görünüm için diyagramı yakınlaştırır ve uzaklaştırın. |
| **Geri bildirim** | MSDN Forumu'nu açar. |

### <a name="canvas"></a>Tuval

Tuval, öğeleri sürükleyip düşürtüğüğün alandır. Sürükle ve bırak modelleri oluşturmak için en hızlı ve en verimli yoldur. Ayrıca, öğelerin genel sürümlerini eklemek için menüden öğeleri sağ tıklayabilir ve seçebilirsiniz:

#### <a name="drop-the-stencil-on-the-canvas"></a>Şablonu tuvale bırak.

![Tuval damla](./media/threat-modeling-tool-feature-overview/canvasdrop1.png)

#### <a name="select-the-stencil"></a>Şablonu seçin

![Öğe özellikleri](./media/threat-modeling-tool-feature-overview/canvasdrop2.png)

### <a name="stencils"></a>Şablonlar

Seçtiğiniz şablona bağlı olarak, kullanabileceğiniz tüm şablonları bulabilirsiniz. Doğru öğeleri bulamıyorsanız, başka bir şablon kullanın. Veya gereksinimlerinize uyacak şekilde bir şablon değiştirebilirsiniz. Genellikle, aşağıdaki gibi kategorilerin bir birleşimini bulabilirsiniz:

| Şablon adı                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **İşleme** | Uygulamalar, tarayıcı eklentileri, iş parçacıkları, sanal makineler |
| **Dış etkileç** | Kimlik doğrulama sağlayıcıları, tarayıcılar, kullanıcılar, web uygulamaları |
| **Veri deposu** | Önbellek, depolama, yapılandırma dosyaları, veritabanları, kayıt defteri |
| **Veri akışı** | İkili, ALPC, HTTP, HTTPS/TLS/SSL, IOCTL, IPSec, adlı boru, RPC/DCOM, Kobİ, UDP |
| **Güven çizgisi/Kenarlık sınırı** | Kurumsal ağlar, internet, makine, kum havuzu, kullanıcı/çekirdek modu |

### <a name="notesmessages"></a>Notlar/mesajlar

| Bileşen                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Ileti** | Öğeler arasında veri akışı olmaması gibi bir hata olduğunda kullanıcıları uyaran dahili araç mantığı. |
| **Notlar** | El ile notlar tasarım ve gözden geçirme süreci boyunca mühendislik ekipleri tarafından dosyaya eklenir. |

### <a name="element-properties"></a>Öğe özellikleri

Öğe özellikleri seçtiğiniz öğelere göre değişir. Güven sınırları dışında, diğer tüm öğeler üç genel seçim içerir:

| Öğe özelliği                               | Ayrıntılar      |
| --------------------------------------- | ------------ |
| **Adı** | İşlemlerinizi, depolarınızı, interaktörleri ve akışlarınızı kolayca tanınmaları için adlandırmak için kullanışlıdır. |
| **Kapsam dışı** | Seçilirse, öğe tehdit oluşturma matrisinden çıkarılır (önerilmez). |
| **Kapsam dışı nedeni** | Kullanıcılara kapsam dışında neden seçildiğini bildirmek için yaslama alanı. |

Özellikler her öğe kategorisi altında değiştirilir. Kullanılabilir seçenekleri incelemek için her öğeyi seçin. Veya daha fazla bilgi edinmek için şablonu açabilirsiniz. Özellikleri gözden geçirelim.

## <a name="welcome-screen"></a>Hoş Geldiniz ekranı

Uygulamayı açtığınızda **Hoş Geldiniz** ekranını görürsünüz.

### <a name="open-a-model"></a>Model açma

İki seçeneği ortaya çıkarmak için **Bir Modeli Aç'ın** üzerine yıkın: Bu Bilgisayardan Aç ve **OneDrive'dan Aç**. **Open From This Computer** İlk seçenek **Dosya Aç** ekranını açar. İkinci seçenek, OneDrive için oturum açma işlemini geçirmenizi ister. Başarılı kimlik doğrulamadan sonra klasörleri ve dosyaları seçebilirsiniz.

![Açık model](./media/threat-modeling-tool-feature-overview/openmodel.png)

![Bilgisayardan veya OneDrive'dan aç](./media/threat-modeling-tool-feature-overview/openmodel2.png)

### <a name="feedback-suggestions-and-issues"></a>Geri bildirim, öneri ve sorunlar

**Geri Bildirim, Öneri ve Sorunlar'ı**seçtiğinizde, SDL Araçları için MSDN Forumu'na gidersiniz. Geçici çözüm ve yeni fikirler de dahil olmak üzere diğer kişilerin araç hakkında söylediklerini okuyabilirsiniz.

![Geri Bildirim](./media/threat-modeling-tool-feature-overview/feedback.png)

## <a name="design-view"></a>Tasarım görünümü

Yeni bir model açtığınızda veya oluşturduğunuzda, **Tasarım** görünümü açılır.

### <a name="add-elements"></a>Öğe ekleme

Kılavuzdaki öğeleri iki şekilde ekleyebilirsiniz:

- **Sürükle ve bırak**: İstenilen öğeyi ızgaraya sürükleyin. Ardından, ek bilgi sağlamak için öğe özelliklerini kullanın.
- **Sağ tıklayın**: Izgaranın herhangi bir yerine sağ tıklayın ve açılır menüden öğeleri seçin. Seçtiğiniz öğenin genel bir gösterimi ekranda görüntülenir.

### <a name="connect-elements"></a>Öğeleri bağlayın

Öğeleri iki şekilde bağlayabilirsiniz:

- **Sürükle ve bırak**: İstenilen veri akışını ızgaraya sürükleyin ve her iki ucunu da uygun öğelere bağlayın.
- **+ Shift ' i tıklatın**: İlk öğeyi (veri gönderme) tıklatın, Shift tuşuna basın ve basılı tutun ve ardından ikinci öğeyi (veri alma) seçin. Sağ tıklatın ve **Bağlan'ı**seçin. Çift yönlü veri akışı kullanıyorsanız, sipariş o kadar önemli değildir.

### <a name="properties"></a>Özellikler

 Şablonlarda değiştirilebilen özellikleri görmek için şablonu seçin ve bilgiler buna göre doldurulur. Aşağıdaki örnek, **veritabanı** şablonundan önce ve sonra diyagrama sürülmeden önce ve sonra gösterilmektedir:

#### <a name="before"></a>Önce

![Önce](./media/threat-modeling-tool-feature-overview/properties1.png)

#### <a name="after"></a>Sonra

![Sonra](./media/threat-modeling-tool-feature-overview/properties2.png)

### <a name="messages"></a>İletiler

Bir tehdit modeli oluşturur ve veri akışlarını öğelere bağlamayı unutursanız, bir bildirim alırsınız. İletiyi yoksayabilir veya sorunu gidermek için yönergeleri izleyebilirsiniz. 

![İletiler](./media/threat-modeling-tool-feature-overview/messages.png)

### <a name="notes"></a>Notlar

Diyagramınıza notlar eklemek için **Mesajlar** sekmesinden **Notlar** sekmesine geçin.

## <a name="analysis-view"></a>Analiz görünümü

Diyagramınızı yaptıktan sonra, **Analiz** görünümüne geçmek için kısayollar araç çubuğundaki **Analiz** simgesini (büyüteç) seçin.

![Analiz görünümü](./media/threat-modeling-tool-feature-overview/analysisview.png)

### <a name="generated-threat-selection"></a>Oluşturulan tehdit seçimi

Bir tehdit seçtiğinizde, üç farklı işlev kullanabilirsiniz:

| Özellik                               | Bilgi      |
| --------------------------------------- | ------------ |
| **Okuma göstergesi** | <p>Tehdit okundu olarak işaretlenir, bu da incelediğiniz öğeleri izlemenize yardımcı olur.</p><p>![Okuma/okunmamış gösterge](./media/threat-modeling-tool-feature-overview/readmode.png)</p> |
| **Etkileşim odağı** | <p>Bir tehdide ait diyagramdaki etkileşim vurgulanır.</p><p>![Etkileşim odağı](./media/threat-modeling-tool-feature-overview/interactionfocus.png)</p> |
| **Tehdit özellikleri** | <p>**Tehdit** özellikleri penceresinde tehditle ilgili ek bilgiler görüntülenir.</p><p>![Tehdit özellikleri](./media/threat-modeling-tool-feature-overview/threatproperties.png)</p> |

### <a name="priority-change"></a>Öncelik değişikliği

Oluşturulan her tehdidin öncelik düzeyini değiştirebilirsiniz. Farklı renkler, yüksek, orta ve düşük öncelikli tehditleri belirlemeyi kolaylaştırır.

![Öncelik değişikliği](./media/threat-modeling-tool-feature-overview/prioritychange.png)

### <a name="threat-properties-editable-fields"></a>Tehdit özellikleri editable alanları

Önceki resimde görüldüğü gibi, araç tarafından oluşturulan bilgileri değiştirebilirsiniz. Ayrıca, gerekçelendirme gibi belirli alanlara bilgi de ekleyebilirsiniz. Bu alanlar şablon tarafından oluşturulur. Her tehdit için daha fazla bilgiye ihtiyacınız varsa, değişiklikler yapabilirsiniz.

![Tehdit özellikleri](./media/threat-modeling-tool-feature-overview/threatproperties.png)

## <a name="reports"></a>Reports

Öncelikleri değiştirmeyi ve oluşturulan her tehdidin durumunu güncelleştirmeyi tamamladıktan sonra, dosyayı kaydedebilir ve/veya bir rapor yazdırabilirsiniz. Tam **Rapor Oluştur'u Bildir'e** > **Create Full Report**git. Raporu adlandırın ve aşağıdaki resme benzer bir şey görmeniz gerekir:

![Rapor](./media/threat-modeling-tool-feature-overview/report.png)

## <a name="next-steps"></a>Sonraki adımlar

- Sorularınızı, yorumlarınızı ve tmtextsupport@microsoft.comendişelerinizi . Başlamak için Tehdit Modelleme Aracı'nı **[indirin.](https://aka.ms/threatmodelingtool)**
- Topluluk için bir şablon sağlamak için [GitHub](https://github.com/Microsoft/threat-modeling-templates) sayfamıza gidin.