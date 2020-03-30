---
title: Azure Veri Kutunuz için güvenlik | Microsoft Dokümanlar
description: Güvenlik kurallarını, yönergeleri ve dikkatleri açıklar ve Azure Veri Kutunuzu güvenli bir şekilde nasıl yükleyip çalıştırılacakaçıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 08/13/2019
ms.author: alkohli
ms.openlocfilehash: db6e2eadbf19d78a203a4d4ba6111ad88430b192
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68985938"
---
# <a name="safely-install-and-operate-your-azure-data-box"></a>Azure Veri Kutunuzu güvenli bir şekilde yükleyin ve çalıştırın
![Uyarı](./media/data-box-safety/warning_icon.png)
![Simgesi Oku](./media/data-box-safety/read_safety_and_health_information_icon.png) Güvenlik Bildirimi Simgesi **OKU GÜVENLİk VE SAĞLIK BİlGİlerİ**

Azure Veri Kutusu'nu kullanmadan önce bu makaledeki tüm güvenlik bilgilerini okuyun. Talimatlara uyulmaması yangına, elektrik çarpmasına veya diğer yaralanmalara veya mülklerinizde hasara neden olabilir.

## <a name="safety-icon-conventions"></a>Güvenlik simgesi kuralları
Veri Kutunuzu kurarken ve çalıştırırken uyulması gereken güvenlik önlemlerini gözden geçirirken bulacağınız simgeler aşağıda verecektir.

| Simge | Açıklama |
|:--- |:--- |
| ![Tehlike](./media/data-box-safety/warning_icon.png) Simgesi **DANGER!** |Kaçınılmaması halinde ölümle veya ciddi yaralanmalara yol açabilecek tehlikeli bir durumu gösterir. Bu sinyal kelimesi en uç durumlarla sınırlı olacaktır. |
| ![Uyarı](./media/data-box-safety/warning_icon.png) Simgesi **UYARI!** |Kaçınılmaması halinde ölüm veya ciddi yaralanmalara yol açabilecek tehlikeli bir durumu gösterir. |
| ![Uyarı](./media/data-box-safety/warning_icon.png) Simgesi **DİkKAT!** |Kaçınılmaması halinde küçük veya orta derecede yaralanmaya neden olabilecek tehlikeli bir durumu gösterir. |
| ![Bildirim](./media/data-box-safety/notice_icon.png) Simgesi **DİkKAT:** |Önemli kabul edilen, ancak tehlikeyle ilgili olmayan bilgileri gösterir. |
| ![Elektrik Şoku Simgesi](./media/data-box-safety/electrical_shock_hazard_icon.png) Elektrik **Şoku Tehlikesi** |Yüksek voltaj. |
| ![Ağır Simge](./media/data-box-safety/heavy_weight_hazard_icon.png) **Ağır Ağırlık** | |
| ![Kullanıcı ServisiNe Tabi](./media/data-box-safety/no_user_serviceable_parts_icon.png) Parçalar Simgesi **Yok Kullanıcı Servis Eki Parçalar** |Düzgün bir şekilde eğitilmedikçe erişmeyin. |
| ![Güvenlik Bildirimi](./media/data-box-safety/read_safety_and_health_information_icon.png) Simgesini Oku **Önce Tüm Talimatları Oku** | |
| ![İpucu Tehlike](./media/data-box-safety/tip_hazard_icon.png) Simgesi **İpucu Tehlike** | |

## <a name="handling-precautions"></a>Taşıma önlemleri

![Uyarı](./media/data-box-safety/warning_icon.png) ![Simgesi Elektrik](./media/data-box-safety/electrical_shock_hazard_icon.png)![ŞokU Simgesi](./media/data-box-safety/no_user_serviceable_parts_icon.png) Kullanıcı Servisi Olmayan Parçalar Simgesi **DİkKAT** 

* *Alınan* cihazı hasarlar için inceleyin. Aygıt muhafazası zarar görmüşse, değiştirme almak için [Microsoft Destek'e başvurun.](data-box-disk-contact-microsoft-support.md) Cihazı çalıştırmaya çalışmayın. 
* Cihaz kurcalamaya dayanıklı vidalarla donatılmıştır. Aygıtın arızalı olduğundan şüpheleniyorsanız, yenisini almak için [Microsoft Destek'e başvurun.](data-box-disk-contact-microsoft-support.md) Aygıta hizmet vermeye çalışmayın. 
* Cihaz, kullanıcı tarafından hizmet e-reayi olmayan parçalar içermez. Tehlikeli voltaj, akım ve enerji seviyeleri içeride bulunmaktadır. Açmayın. Servis için aygıtı Microsoft'a iade edin.

![Uyarı](./media/data-box-safety/warning_icon.png) ![Simgesi Ağır](./media/data-box-safety/heavy_weight_hazard_icon.png) Ağırlık Simgesi **UYARI!** 

* Tamamen yapılandırılmış bir muhafaza 22,7 kg(50 lbs) ağırolabilir; tek başınıza kaldırmaya çalışmayın.
* Muhafazayı hareket ettirmeden önce, her zaman ağırlığı işlemek için iki kişinin kullanılabilir olduğundan emin olun. Bu ağırlığı kaldırmaya çalışan bir kişinin yaralanmalara neden olabileceğini unutmayın.


![Uyarı](./media/data-box-safety/warning_icon.png) ![Simgesi İpucu](./media/data-box-safety/tip_hazard_icon.png) Tehlike Simgesi **UYARI!**
* Olası bir uç tehlikesini önlemek için cihazı düz, sert ve kararlı bir yüzeye yerleştirin.
* Rafa monte edilmiş ekipmanlar raf veya çalışma alanı olarak kullanılmamalıdır. Veri Kutusunu rafa monte edilmiş ekipmanların üzerine yerleştirmeyin. Rafa monte edilmiş uzun bir üniteye herhangi bir yük eklemek yaralanmaya, ölüme veya ürün hasarına yol açabilecek potansiyel bir uç tehlikesi oluşturabilir.

![Uyarı](./media/data-box-safety/warning_icon.png) Simgesi **UYARI!**

* Cihazı, cihazın etrafında yeterli hava sirkülasyonuna izin veren bir çalışma alanına kurun.
* Cihazı iletken kirletici maddeler içermeyen sıcaklık kontrollü bir kapalı alana takın ve cihazın etrafında yeterli hava sirkülasyonuna izin verin.
* Cihazı sıvı ve aşırı nemli ortamlardan uzak tutun.


## <a name="electrical-precautions"></a>Elektriksel önlemler

![Uyarı](./media/data-box-safety/warning_icon.png) ![Simgesi Elektrik](./media/data-box-safety/electrical_shock_hazard_icon.png) Şok Ukonu **UYARI!**

* Güç kaynağı kablosuna güvenli bir elektrik dünya bağlantısı sağlayın. AC kablosuüç telli topraklama fişi (topraklama pimi olan bir fiş) vardır. Bu fiş sadece topraklanmış ac prizine uyar. Topraklama piminin amacını yenme.
* Güç besleme kablosunun fişinin ana bağlantı kesme cihazı olduğu göz önüne alındığında, soket prizlerinin cihazın yakınında olduğundan ve kolayca erişilebilir olduğundan emin olun.
* Güç kablosunu çıkarın (fişi çekerek, kabloyu değil) ve aşağıdaki koşullardan herhangi biri varsa tüm kabloları çıkarın:

    - Güç kablosu veya fiş yıpranmış veya başka bir şekilde hasar görür.
    - Cihaz kasasına bir şey döküyorsun.
    - Cihaz yağmura veya aşırı neme maruz kalır.
    - Cihaz düşürüldü ve cihazın gövdesi hasar gördü.
    - Cihazın servise veya onarıma ihtiyacı olduğundan şüpheleniyorsunuz.
* Taşımadan önce veya herhangi bir şekilde hasar gördüğünü düşünüyorsanız, ünitenin fişini kalıcı olarak çıkarın.
* Aşağıdaki güç özelliklerini karşılamak için elektrik aşırı yük koruması ile uygun bir güç kaynağı sağlayın:

    - Gerilim: 100 V AC - 240 V AC
    - Şu anki: 6 A, maksimum
    - Frekans: 50 Hz - 60 Hz

![Uyarı](./media/data-box-safety/warning_icon.png) Simgesi **DİkKAT:**

* Bu cihaz sikke hücresi pilleri içerir. Aygıta hizmet vermeye çalışmayın. Bu cihazdaki piller kullanıcı tarafından servis edilemez. 
* **Yalnızca servis personeli için**: Pil yanlış bir türle değiştirilirse patlama riski. Kullanılmış pilleri talimatlara göre atın.

![Bildirim](./media/data-box-safety/notice_icon.png) Simgesi **DİkKAT:**

Cihazınızın düzgün çalışması ve ürün hasarının önlenmesi için:

* Cihaz çalışırken ön ve arka kapıların tamamen açık olduğundan emin olun.

## <a name="regulatory-information"></a>Mevzuat bilgileri

Bu bölüm, DB010 düzenleyici model numarası Olan Azure Veri Kutusu için düzenleyici bilgiler içerir.

Bu cihaz:

- Tipik bir veri odası ortamında çalışmak üzere tasarlanmış Bilgi Teknolojisi Ekipmanları (ITE) olarak değerlendirilir. Bu ürünün diğer ortamlar için uygunluğu daha fazla değerlendirme gerektirebilir.
- NRTL Listelenen (UL, CSA, ETL, vb)ve IEC/EN 60950-1 veya IEC/EN 62368-1 uyumlu (CE işaretli) Bilgi Teknolojisi ekipmanları ile kullanılmak üzere tasarlanmıştır.
- Aşağıdaki ortamda çalışacak şekilde tasarlanmıştır. 
    - Çalışma sıcaklığı: 50° ila 95° F (10° ila 35° C)
    - Saklama sıcaklığı: -4° ila 122° F (-20° ila 50° C)
    - Bağıl nem: %15 ila %85 (yoğuşmasız) 
    - Çalışma yüksekliği: 6500 feet'e kadar (0 metre ile 2000 metre) kadar test edilmiştir

Elektrik besleme derecelendirmeleri için birimle birlikte sağlanan aygıt derecelendirme etiketine bakın. 

![Bildirim](./media/data-box-safety/notice_icon.png) Simgesi **DİkKAT:** 

Microsoft tarafından açıkça onaylanmayan aygıtta yapılan değişiklikler veya değişiklikler, kullanıcının cihazı çalıştırma yetkisini geçersiz kılabilir.

**KANADA ve ABD:**

![Bildirim](./media/data-box-safety/notice_icon.png) Simgesi **DİkKAT:** 

Bu ekipman test edilmiş ve FCC Kuralları bölüm 15 uyarınca, Bir Sınıf A dijital cihaz için sınırları uygun olduğu tespit edilmiştir. Bu sınırlar, ekipman ticari bir ortamda çalıştırıldığında zararlı müdahalelere karşı makul koruma sağlamak üzere tasarlanmıştır. Bu ekipman radyo frekansı enerjisi üretir, kullanır ve yayabilir ve kullanım kılavuzuna uygun olarak kurulup kullanılmadığı takdirde radyo iletişimine zararlı parazitlere neden olabilir. Bu ekipmanın bir yerleşim bölgesinde çalışması zararlı parazitlere neden olabilir ve bu durumda kullanıcının kendi pahasına paraziti düzeltmesi gerekecektir.

Bu cihaz, FCC Kuralları ve Endüstri Kanada lisanstan muaf RSS standardının (lar) 15. Çalışma aşağıdaki iki koşula tabidir: (1) Bu cihaz zararlı parazite neden olmayabilir ve (2) bu cihaz, cihazın istenmeyen çalışmasına neden olabilecek parazitler de dahil olmak üzere alınan herhangi bir paraziti kabul etmelidir.

![Kanada](./media/data-box-safety/canada.png)

CAN ICES-3(A)/NMB-3(A)

Microsoft Corporation, One Microsoft Way, Redmond, WA 98052, ABD.

Amerika Birleşik Devletleri: (800) 426-9400

Kanada: (800) 933-4750

**AVRUPA BİrLİğİ:**

[AB Uygunluk Beyanı'nın bir kopyasını isteyin.](mailto:CSI_Compliance@microsoft.com)

![Uyarı](./media/data-box-safety/warning_icon.png) Simgesi **UYARI:** 

Bu a sınıfı bir ürün. Bir ev ortamında, bu ürün radyo parazitine neden olabilir ve bu durumda kullanıcının yeterli önlemleri alması gerekebilir.

**Atık pillerin ve elektrikli ve elektronik ekipmanların bertaraf edilmesi:**

![Pil imha simgesi](./media/data-box-safety/battery_disposal_icon.png)

Üründeki veya pilleri veya ambalajı üzerindeki bu sembol, bu ürünün ve içerdiği pillerin evsel atıklarınızla birlikte atılmaması gerektiği anlamına gelir. Bunun yerine, pillerin ve elektrikli ve elektronik ekipmanların geri dönüşümü için bunu geçerli bir toplama noktasına teslim etmek sizin sorumluluğunuzdadır. Bu ayrı toplama ve geri dönüşüm, doğal kaynakların korunmasına ve pillerde ve elektrik ve elektronik ortamda tehlikeli maddelerin bulunması nedeniyle insan sağlığı ve çevre için olası olumsuz sonuçları önlemeye yardımcı olacaktır. uygun olmayan bertaraf neden olabilir ekipman. Pillerinizi ve elektrikli ve elektronik atıklarınızı nereye bırakacağınız hakkında daha fazla bilgi için lütfen yerel şehir/belediye ofisine, evsel atık bertaraf hizmetine veya bu ürünü satın aldığınız mağazaya başvurun. WEEE hakkında daha fazla bilgi için *erecycle\@microsoft.com* ile iletişime geçin.

Bu ürün sikke hücre pil (ies) içerir.

Microsoft İrlanda Sandyford Ind Est Dublin D18 KX32 IRL

Telefon numarası: +353 1 295 3826

Faks numarası: +353 1 706 4110 

**Japonya**

![Japonya](./media/data-box-safety/japan.png)

<!--**South Korea**

![South Korea](./media/data-box-safety/south-korea.png)

**China RoHS Hazardous Substances Table**

![China RoHS hazardous substances](./media/data-box-safety/south-korea.png)

**Taiwan**

![Taiwan](./media/data-box-safety/taiwan.png)-->

Bu güvenlik bildirimlerini inceledikten sonra cihazınızı kurabilir ve kabloyla kablosuz atabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Veri Kutunuzu kablolayın ve bağlayın](data-box-deploy-set-up.md)


