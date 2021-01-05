---
title: Rapor oluştur
description: IoT raporlama araçları için Defender 'ı kullanarak ağ etkinliği, riskleri, saldırıları ve eğilimler hakkında öngörüler elde edin.
author: shhazam-ms
manager: rkarlin
ms.author: shhazam
ms.date: 12/17/2020
ms.topic: how-to
ms.service: azure
ms.openlocfilehash: d0c3f531ede0a590a6ba7bb21c6edbd768c08069
ms.sourcegitcommit: 8be279f92d5c07a37adfe766dc40648c673d8aa8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2020
ms.locfileid: "97842562"
---
# <a name="generate-reports"></a>Rapor oluştur

IoT raporlama araçları için Azure Defender 'ı kullanarak ağ etkinliği, riskleri, saldırıları ve eğilimler hakkında öngörüler elde edin. Raporlar oluşturmak için araçlar kullanılabilir:

- Ayrı sensörler tarafından algılanan etkinliğe göre.
- Tüm sensörler tarafından algılanan etkinliğe göre. 

Bu raporlar, şirket içi yönetim konsolundan oluşturulur.

## <a name="reports-for-sensor-risk-assessment"></a>Algılayıcı risk değerlendirmesi raporları

Risk değerlendirmesi raporlaması, her ağ aygıtı için bir güvenlik puanı ve ayrıca genel bir ağ güvenlik puanı oluşturmanıza olanak sağlar. Genel puan yüzde 100 güvenlik yüzdesini temsil eder.

Bu puan, paket incelemesi, davranış modelleme motorları ve bir SCADA özgü durum makinesi tasarımının sonuçlarına dayalıdır. Çeşitli çok çeşitli bilgiler mevcuttur, örneğin:

- Yapılandırma sorunları

- Güvenlik düzeyine göre öncelikli cihaz güvenlik açığı

- Ağ güvenlik sorunları

- Ağ işlem sorunları

- Saldırı vektörleri

- ICS ağlarına bağlantılar

- Internet bağlantıları

- Endüstriyel kötü amaçlı yazılım göstergeleri

- Protokol sorunları

  - Güvenli cihazlar: %90 üzerinde güvenlik puanı olan cihazlar.

- **Güvenli cihazlar**: %90 üzerinde güvenlik puanı olan cihazlar.

- **Savunmasız cihazlar**: güvenlik puanı %70 altında olan cihazlar.

- **Geliştirme gerektiren cihazlar**: yüzde 70 ve %89 arasında güvenlik puanı olan cihazlar.

Bir rapor oluşturmak için:

1. Yan menüde **risk değerlendirmesi** ' ni seçin.
2. **Algılayıcı Seç** açılan listesinden bir algılayıcı seçin.
3. **Rapor oluştur**' u seçin.
4. Arşivlenmiş raporlar bölümünden **İndir** ' i seçin.

:::image type="content" source="media/how-to-generate-reports/risk-assessment.png" alt-text="Risk değerlendirmesi görünümü.":::

Şirket logosunu içeri aktarmak için:

Şirket logosunu içeri aktarmak için:

- **Logoyu Içeri aktar**' ı seçin.

## <a name="reports-for-sensor-attack-vector"></a>Algılayıcı saldırı vektörü raporları

Saldırı vektör raporları, açıktan yararlanılabilecek bir cihaz güvenlik açığı zincirinin grafik gösterimini sağlar. Bu güvenlik açıkları, bir saldırganın ana ağ cihazlarına erişmesini sağlayabilir. Saldırı vektörü simülatörü, saldırı vektörlerini gerçek zamanlı olarak hesaplar ve belirli bir hedefin tüm saldırı vektörlerini analiz eder.

Saldırı vektörü ile çalışma, saldırı sırasındaki risk azaltma etkinliklerinin etkisini değerlendirmenize imkan tanır. Daha sonra, örneğin, bir sistem yükseltmesinin saldırı zincirini bozarak saldırganın yolunu kesintiye uğramadığını veya alternatif bir saldırı yolu kalmadığını belirleyebilirsiniz. Bu bilgiler, düzeltme ve risk azaltma etkinliklerini önceliklendirmenize yardımcı olur.

:::image type="content" source="media/how-to-generate-reports/control-center.png" alt-text="Denetim Merkezi 'nde uyarılarınızı görüntüleyin.":::

> [!NOTE]
> Yöneticiler ve Güvenlik analistleri bu bölümde açıklanan yordamları gerçekleştirebilir.

Bir saldırı vektörü simülasyonu oluşturmak için:

1. Simülasyon eklemek için yan menüdeki :::image type="content" source="media/how-to-generate-reports/plus.png" alt-text="artı işaretini":::seçin.

 :::image type="content" source="media/how-to-generate-reports/vector.png" alt-text="Saldırı vektörü benzetimi.":::

2. Simülasyon özelliklerini girin:

   - **Ad**: benzetim adı.

   - **En fazla vektör**: tek bir benzetimde en fazla vektör sayısı.

   - **Cihaz haritasında göster**: saldırı vektörünü cihaz haritasında bir filtre olarak göster.

   - **Tüm kaynak cihazlar**: saldırı vektörü tüm cihazları bir saldırı kaynağı olarak kabul edecektir.

   - **Saldırı kaynağı**: saldırı vektörü yalnızca belirtilen cihazları bir saldırı kaynağı olarak kabul eder.

   - **Tüm hedef cihazlar**: saldırı vektörü tüm cihazları bir saldırı hedefi olarak kabul edecektir.

   - **Saldırı hedefi**: saldırı vektörü yalnızca belirtilen cihazları bir saldırı hedefi olarak kabul eder.

   - **Cihazları hariç tut**: belirtilen cihazlar saldırı vektörü benzetimine dahil edilecek.

   - **Alt ağları hariç tut**: belirtilen alt ağlar saldırı vektörü benzetimine dahil edilmeyecek.

3. **Benzetim Ekle**' yi seçin. Simülasyonu benzetim listesine eklenir.

   :::image type="content" source="media/how-to-generate-reports/new-simulation.png" alt-text="Yeni bir benzetim ekleyin.":::

4. :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false":::Benzetimi düzenlemek istiyorsanız seçin.

   :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false":::Benzetimi silmek istiyorsanız seçin.

   :::image type="icon" source="media/how-to-generate-reports/make-a-favorite-icon.png" border="false":::Simülasyonu bir sık kullanılan olarak işaretlemek istiyorsanız seçin.

5. Saldırı vektörlerine ait bir liste görünür ve vektör puanı (100 dışında), saldırı kaynak cihazı ve saldırı hedef cihazı içerir. Saldırı vektörlerine yönelik grafik gösterimi için belirli bir saldırı seçin.

   :::image type="content" source="media/how-to-generate-reports/sample-attack-vectors.png" alt-text="Saldırı vektörleri.":::

## <a name="sensor-data-mining-queries"></a>Algılayıcı verileri-araştırma sorguları

Veri araştırma araçları, çeşitli katmanlarda ağ cihazlarınızla ilgili kapsamlı ve ayrıntılı bilgiler oluşturmanıza imkan tanır. Örneğin, aşağıdakileri temel alarak bir sorgu oluşturabilirsiniz:

- Zaman dönemleri

- İnternet bağlantıları

- Bağlantı noktaları

- Protokoller

- Bellenim sürümleri

- Programlama komutları

- Cihazın eylemsizlik durumu

Filtreleri temel alarak rapor üzerinde ince ayar yapabilirsiniz. Örneğin, bellenim 'un güncelleştirildiği belirli bir alt ağı sorgulayabilirsiniz.

:::image type="content" source="media/how-to-generate-reports/active-device-list-v2.png" alt-text="Etkin cihazların listesi.":::

Sorguları yönetmek için çeşitli araçlar mevcuttur. Örneğin, dışa aktarabilir ve kaydedebilirsiniz.

> [!NOTE]
> Yöneticiler ve Güvenlik analistlerinin veri araştırma seçeneklerine erişimi vardır.

### <a name="dynamic-updates"></a>Dinamik güncelleştirmeler

Oluşturduğunuz veri araştırma sorguları, her açışınızda dinamik olarak güncelleştirilir. Örnek:

- 1 Haziran 'da cihazlarda bellenim sürümleri için bir rapor oluşturursanız ve raporu 10 Haziran 'da yeniden açarsanız, bu rapor 10 Haziran 'da doğru olan bilgilerle güncelleştirilir.

- 1 Haziran 'da son 30 gün içinde bulunan yeni cihazları algılamaya yönelik bir rapor oluşturursanız ve raporu 30 Haziran 'da açarsanız, sonuçlar son 30 gün boyunca görüntülenir.

### <a name="data-mining-use-cases"></a>Veri araştırma kullanım örnekleri

Çeşitli güvenlik ekiplerine yönelik kapsamlı güvenlik ihtiyaçlarını karşılamak için sorguları kullanabilirsiniz:

- **SOC olay yanıtı**: anında olay yanıtıyla başa çıkmak için bir raporu gerçek zamanlı olarak oluşturun. Örneğin, düzeltme eki gerektirebilecek cihazların listesi için bir rapor oluşturun.

- **Forensics**: araştırma raporlarının geçmiş verilerini temel alan bir rapor oluşturun.

- **BT ağ bütünlüğü**: genel ağ güvenliğinin artırılmasına yardımcı olan bir rapor oluşturun. Örneğin, zayıf kimlik doğrulama kimlik bilgileri olan cihazları listeleyen bir rapor oluşturun.

- **Görünürlük**: ağınızın tüm temel parametrelerini görüntülemek için tüm sorgu öğelerini içeren bir rapor oluşturun.

### <a name="data-mining-storage"></a>Veri araştırma depolaması

Veri araştırma bilgileri, bir cihazın silindiği durumlar dışında sürekli olarak kaydedilir ve saklanır. Veri araştırma sonuçları, güvenli bir sunucuya aktarılabilir ve dışarıdan depolanabilir. Ayrıca, algılayıcı, verilerin devamlılığını ve korunmasını sağlamak için otomatik günlük yedeklemeler gerçekleştirir.

### <a name="data-mining-and-reports"></a>Veri madenciliği ve raporları 

**Raporlar** seçeneğinden erişilen düzenli raporlar, önceden tanımlanmış veri araştırma raporlarıdır. Veri madenciliği içinde kullanılabilir olduğu gibi dinamik sorgular değildir, ancak veri madenciliği sorgu sonuçlarının statik bir gösterimi olur.

Veri madenciliği sorgu sonuçları, RO kullanıcıları için kullanılamaz. RO kullanıcılarının veri araştırma sorguları tarafından oluşturulan bilgilere erişimi olmasını isteyen yöneticiler ve Güvenlik analistleri, bilgileri rapor olarak kaydetmelidir.

### <a name="predefined-queries"></a>Önceden tanımlanmış sorgular

Aşağıdaki önceden tanımlanmış sorgular kullanılabilir. Bu sorgular gerçek zamanlı olarak oluşturulur.

- **Cdirme**: son 24 saat içinde bilinen güvenlik açıklarına sahip olan cihazların listesi.

- **Dışlanan CCR 'ler**: el ile dışlanan tüm nesnelerin listesi. VA raporlarında ve saldırı vektörlerine daha doğru sonuçlar elde etmek için, Cvileri ekleyerek ve dışlayarak CVE listesini el ile özelleştirebilirsiniz.

- **Internet etkinliği**: internet 'e bağlı cihazlar.

- **Etkin olmayan cihazlar**: son yedi gün için iletilmedi olan cihazlar.

- **Etkin cihazlar**: son 24 saat içindeki etkin ağ cihazları.

- **Uzaktan erişim**: uzak oturum protokolleriyle iletişim kuran cihazlar.

- **Programlama komutları**: Endüstriyel programlama gönderen cihazlar.

Bu raporlara,, RO kullanıcıları ve diğer kullanıcıların bunları görüntüleyebileceği **raporlar** ekranından otomatik olarak erişilebilir. RO kullanıcıları veri araştırma raporlarına erişemez.

:::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Veri araştırma ekranı.":::

### <a name="create-a-data-mining-report"></a>Veri araştırma raporu oluşturma

Veri araştırma raporu oluşturmak için:

1. Yan menüden **veri araştırma** ' yı seçin. Önceden tanımlanmış önerilen raporlar otomatik olarak görünür.

 :::image type="content" source="media/how-to-generate-reports/data-mining-screeshot-v2.png" alt-text="Yan bölmeden veri araştırma ' yı seçin.":::

2. :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: öğesini seçin.

3. **Yeni rapor** ' ı seçin ve raporu tanımlayın.

   :::image type="content" source="media/how-to-generate-reports/create-new-report-screen.png" alt-text="Bu ekranı doldurarak yeni bir rapor oluşturun.":::

   Aşağıdaki parametreler kullanılabilir:

   - Bir rapor adı ve açıklama sağlayın.

   - Kategoriler için şunlardan birini seçin:

      - Ağınızdaki tüm cihazlarla ilgili tüm rapor sonuçlarını görüntülemek için **Kategoriler (tümü)** .

      - Standart kategorileri seçmek için **genel** ' i seçin.

   - İlgilendiğiniz belirli rapor parametrelerini seçin.

   - Bir sıralama düzeni (**order by**) seçin. Sonuçları etkinliğe veya kategoriye göre sıralayın.

   - **Rapor sonuçlarını rapor sayfasından erişilebilen** bir rapor olarak kaydetmek Için rapor **sayfalarına kaydet** ' i seçin. Bu, RO kullanıcılarının oluşturduğunuz raporu çalıştırmasını sağlar.

   - Daha fazla filtre eklemek için **filtreler (Ekle)** seçeneğini belirleyin. Joker karakter istekleri desteklenir.

   - Bir cihaz grubu belirtin (cihaz eşlemesinde tanımlanmıştır).

   - Bir IP adresi belirtin.

   - Bir bağlantı noktası belirtin.

   - Bir MAC adresi belirtin.

4. **Kaydet**’i seçin. Rapor sonuçları **veri araştırma** sayfasında açılır.

:::image type="content" source="media/how-to-generate-reports/data-mining-page.png" alt-text="Veri araştırma sayfasında görülen sonuçları raporla.":::

### <a name="manage-data-mining-reports"></a>Veri araştırma raporlarını yönetme

Aşağıdaki tabloda veri madenciliği için yönetim seçenekleri açıklanmaktadır:

| Simge görüntüsü | Açıklama |
|--|--|
| :::image type="icon" source="media/how-to-generate-reports/edit-a-simulation-icon.png" border="false"::: | Rapor parametrelerini düzenleyin. |
| :::image type="icon" source="media/how-to-generate-reports/export-as-pdf-icon.png" border="false"::: | PDF olarak dışa aktar. |
| :::image type="icon" source="media/how-to-generate-reports/csv-export-icon.png" border="false"::: |CSV olarak dışa aktarın. |
| :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: | Son değiştirme tarihi gibi ek bilgileri göster. Bir sorgu sonucu anlık görüntüsü oluşturmak için bu özelliği kullanın. Örneğin, ekip liderleriyle veya SOC analistleriyle ilgili daha fazla araştırma için bunu yapmanız gerekebilir. |
| :::image type="icon" source="media/how-to-generate-reports/pin-icon.png" border="false"::: | **Raporlar** sayfasında görüntüleyin veya **raporlar** sayfasında gizleyin. :::image type="content" source="media/how-to-generate-reports/hide-reports-page.png" alt-text="Raporlarınızı gizleyin veya ortaya çıkar."::: |
| :::image type="icon" source="media/how-to-generate-reports/delete-simulation-icon.png" border="false"::: | Raporu silin. |

#### <a name="create-customized-directories"></a>Özelleştirilmiş dizinler oluşturma 

Kategoriler için dizinler oluşturarak veri madenciliği sorguları için kapsamlı bilgiler düzenleyebilirsiniz. Örneğin, protokoller veya konumlar için dizinler oluşturabilirsiniz.

Yeni bir dizin oluşturmak için:

1. :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false":::Yeni bir dizin eklemek için seçin.

2. Yeni Dizin formunu göstermek için **Yeni Dizin** ' i seçin.

3. Yeni dizini adlandırın.

4. Gerekli raporları yeni dizine sürükleyin. İstediğiniz zaman raporu, ana görünüme geri sürükleyebilirsiniz.

5. Açmak, düzenlemek veya silmek için yeni dizine sağ tıklayın.

#### <a name="create-snapshots-of-report-results"></a>Rapor sonuçlarının anlık görüntülerini oluşturma

Daha fazla araştırma için bazı sorgu sonuçlarını kaydetmeniz gerekebilir. Örneğin, sonuçları çeşitli güvenlik ekipleriyle paylaşmanız gerekebilir.

Anlık görüntüler, rapor sonuçları içine kaydedilir ve dinamik sorgular oluşturmaz.

:::image type="content" source="media/how-to-generate-reports/report-results-report.png" alt-text="Görüntüsünü.":::

Anlık görüntü oluşturmak için:

1. Gerekli raporu açın.

2. Bilgi simgesini seçin :::image type="icon" source="media/how-to-generate-reports/information-icon.png" border="false"::: .

3. **Yeni al**' ı seçin.

4. Anlık görüntü için bir ad girin ve **Kaydet**' i seçin.

:::image type="content" source="media/how-to-generate-reports/take-a-snapshot.png" alt-text="Anlık görüntü alın.":::

#### <a name="customize-the-cve-list"></a>CVE listesini özelleştirme

CVE listesini aşağıdaki şekilde el ile özelleştirebilirsiniz:

  - Cvileri dahil etme/hariç tutma

  - CVE Puanını değiştirme

CVE raporunda el ile değişiklikler yapmak için:

1.  Yan menüden **veri araştırma**' yı seçin.

2. :::image type="icon" source="media/how-to-generate-reports/plus-icon.png" border="false"::: **Veri araştırma** penceresinin sol üst köşesindeki seçimi yapın. Ardından **Yeni rapor**' ı seçin.

   :::image type="content" source="media/how-to-generate-reports/create-a-new-report-screen.png" alt-text="Yeni bir rapor oluşturun.":::

3. Sol bölmeden aşağıdaki seçeneklerden birini seçin:

   - **Bilinen güvenlik açıkları**: her iki seçeneği de seçer ve sonuçları raporun iki tablosuna, biri bir tane, diğeri de hariç tutulan cele alır.

   - **Cves**: tüm cves listesini sunmak için bu seçeneği belirleyin.

   - **Dışlanan CAS**'ler: tüm dışlanan nesnelerin bir listesini temsil etmek için bu seçeneği belirleyin.

4. **Ad** ve **Açıklama** bilgilerini doldurup **Kaydet**' i seçin. Yeni rapor, **veri araştırma** penceresinde görünür.

5. Cvileri dışlamak için, veri araştırma raporunu Cdirme 'Ler için açın. Tüm CVEs 'ler görüntülenir.

   :::image type="content" source="media/how-to-generate-reports/cves.png" alt-text="C V E raporu.":::

6. Listedeki öğeleri seçmeyi etkinleştirmek için :::image type="icon" source="media/how-to-generate-reports/enable-selecting-icon.png" border="false"::: özelleştirmek Istediğiniz Cvileri seçin ve seçin. **İşlemler** çubuğu en altta görünür.

   :::image type="content" source="media/how-to-generate-reports/operations-bar-appears.png" alt-text="Veri madenciliği Işlemler çubuğunun ekran görüntüsü.":::

7. Dışlamak istediğiniz Cvileri seçin ve ardından **kayıtları Sil**' i seçin. Seçtiğiniz canlar, CVEs listesinde görünmez ve bir tane oluşturduğunuzda dışlanan Cdirme listesinde görünür.

8. Dışlanan CCR 'Ler listesine dahil etmek için, hariç tutulan celik 'ler için raporu oluşturun ve bu listeden silmek istediğiniz öğeleri ekleyin.

9. Puanı değiştirmek istediğiniz Ckları seçin ve ardından **CVE Puanını Güncelleştir**' i seçin.

   :::image type="content" source="media/how-to-generate-reports/set-new-score-screen.png" alt-text="CVE Puanını güncelleştirin.":::

10. Yeni puanı girin ve **Tamam**' ı seçin. Güncelleştirilmiş puan, seçtiğiniz Cide görüntülenir.

### <a name="reports-based-on-data-mining"></a>Veri madenciliği temel alan raporlar

Raporlar, veri madenciliği sorgu sonuçları tarafından oluşturulan bilgileri yansıtır. Bu, raporlar görünümünde kullanılabilen varsayılan veri araştırma raporlarını içerir. Yönetici ve Güvenlik analistleri Ayrıca özel veri araştırma sorguları oluşturabilir ve bunları rapor olarak kaydedebilir. Bu raporlar Ayrıca, RO kullanıcıları için de kullanılabilir.

Bir rapor oluşturmak için:

1. Yan menüdeki **raporlar** ' ı seçin.

2. Görüntülenecek gerekli raporu seçin. Seçim, **programlama komutları** ve **Uzaktan erişim** gibi **özel** veya **otomatik üretilmiş** raporlar olabilir.

3. Ekranın sağ üst köşesindeki simgelerden birini seçerek raporu dışarı aktarabilirsiniz:

   :::image type="icon" source="media/how-to-generate-reports/export-to-pdf-icon.png" border="false"::: PDF dosyasına dışarı aktarın.

   :::image type="icon" source="media/how-to-generate-reports/export-to-csv-icon.png" border="false"::: Bir CSV dosyasına aktarın.

> [!NOTE] 
> RO kullanıcısı yalnızca kendileri için oluşturulan raporları görebilir.

:::image type="content" source="media/how-to-generate-reports/select-a-report-screen.png" alt-text="Oluşturulacak raporu seçin.":::

:::image type="content" source="media/how-to-generate-reports/remote-access-report.png" alt-text="Uzaktan erişim raporu oluşturuldu.":::

## <a name="reports-for-sensor-trends-and-statistics"></a>Algılayıcı eğilimleri ve istatistikleri için raporlar

Ağ eğilimleri ve istatistikleriyle ilgili bilgi edinmek için pencere öğesi grafikleri ve pasta grafikleri oluşturabilirsiniz. Pencere öğeleri, Kullanıcı tanımlı panolar altında gruplandırılabilir.

> [!NOTE]
> Bu bölümdeki yordamları yalnızca Yöneticiler ve Güvenlik analistleri gerçekleştirebilir.

Panoları ve pencere öğelerini görmek için kenar menüsünde **eğilim & istatistik** ' i seçin.

:::image type="content" source="media/how-to-generate-reports/investigation-screenshot.png" alt-text="Bir araştırmanın ekran görüntüsü.":::

Pano, aşağıdaki bilgi türlerini grafiksel olarak tanımlayan pencere öğeleri içerir:

- Bağlantı noktasına göre trafik
- Kanal bant genişliği
- Toplam bant genişliği
- Etkin TCP bağlantısı
- Cihazlarınız
  - Yeni cihazlar
  - Meşgul cihazlar
  - Satıcıya göre cihazlar
  - İşletim sistemine göre cihazlar
  - Bağlantısı kesilen cihazlar
- Saate göre bağlantı bırakma
- Türe göre olay uyarıları
- Veritabanı tablosu erişimi
- Protokol açıklaması pencere öğeleri
- Ethernet ve IP adresi:
  - CıP hizmetine göre Ethernet ve IP adresi trafiği
  - CıP sınıfına göre Ethernet ve IP adresi trafiği
  - Komuta göre Ethernet ve IP adresi trafiği
- OPC
  - OPC üst yönetim işlemleri
  - OPC Top g/ç işlemleri
- Siemens S7:
  - Denetim işlevine göre S7 trafiği
  - Alt işleve göre S7 trafiği
- SRTP:
  - Hizmet koduna göre SRTP trafiği
  - Güne göre SRTP hataları
- SuiteLink:
  - SuiteLink üst sorgulanan Etiketler
  - SuiteLink sayısal etiket davranışı
- IEC-ASDU 'e göre 60870 trafiği
- İşleve göre DNP3 trafiği
- Hizmete göre MMS trafiği
- İşleve göre Modbus trafiği
- Hizmete göre OPC-UA trafiği

> [!NOTE]
>  Pencere öğelerinin zaman algılayıcısı, algılayıcı zamanına göre ayarlanır.

## <a name="reports-for-the-on-premises-management-console"></a>Şirket içi yönetim konsolu raporları

Şirket içi yönetim konsolu, kendisine bağlı her bir algılayıcı için raporlar oluşturmanıza olanak sağlar. Raporlar, gerçekleştirilen algılayıcı veri araştırma sorgularını temel alır.

Aşağıdaki raporları oluşturabilirsiniz:

- **Etkin cihazlar (son 24 saat)**: 24 saat içinde ağ etkinliğini gösteren cihazların listesini gösterir.

- **Etkin olmayan cihazlar (son 7 gün)**: son yedi gün içinde ağ etkinliği olmadığını gösteren cihazların listesini gösterir.

- **Programlama komutları**: son 24 saat içinde programlama komutları gönderen cihazların listesini gösterir.

- **Uzaktan erişim**: uzak kaynaklara son 24 saat içinde eriştiği cihazların listesini gösterir.

:::image type="content" source="media/how-to-generate-reports/reports-view.png" alt-text="Raporlar görünümünün ekran görüntüsü.":::

Şirket içi yönetim konsolundan algılayıcıyı seçtiğinizde, bu algılayıcı üzerinde yapılandırılan tüm özel raporlar rapor listesinde görüntülenir. Her algılayıcı için, bu algılayıcı üzerinde yapılandırılmış bir varsayılan rapor veya özel rapor oluşturabilirsiniz.

Bir rapor oluşturmak için:

1. Sol bölmede **raporlar**' ı seçin. **Raporlar** penceresi görüntülenir.

2. **Algılayıcılar** açılan listesinden, raporu oluşturmak istediğiniz algılayıcıyı seçin.

   :::image type="content" source="media/how-to-generate-reports/sensor-drop-down-list.png" alt-text="Algılayıcılar görünümünün ekran görüntüsü.":::

3. Sağ açılan listeden, oluşturmak istediğiniz raporu seçin.

4. Rapor sonuçlarının bir PDF 'sini oluşturmak için öğesini seçin :::image type="icon" source="media/how-to-generate-reports/pdf-report-icon.png" border="false"::: .

## <a name="risk-assessment-reports-for-the-on-premises-management-console"></a>Şirket içi yönetim konsolu için risk değerlendirmesi raporları

Şirket içi yönetim konsolunuza bağlı her bir algılayıcı için bir risk değerlendirmesi raporu oluşturun. Bu rapor, sensörlerinizin izlediği ağların her birine ilişkin öngörüler sağlar.

Risk değerlendirmesi raporları her ağ aygıtı için bir güvenlik puanı ve genel bir ağ güvenlik puanı oluşturmanıza olanak sağlar. Genel puan, derin paket incelemesi, davranış modelleme motorları ve SCADA özgü durum makinesi tasarımını temel alır. Çeşitli çok sayıda bilgi mevcuttur. Örnek:

- Yapılandırma sorunları

- Güvenlik düzeyine göre öncelikli cihaz güvenlik açığı

- Ağ güvenlik sorunları

- Ağ işlem sorunları

- Saldırı vektörleri

- ICS ağlarına bağlantılar

- Internet bağlantıları

- Endüstriyel kötü amaçlı yazılım göstergeleri

- Protokol sorunları

Rapor, güvenlik puanınızı iyileştirmenize yardımcı olacak risk azaltma önerileri sağlar.

- Güvenli cihazlar: %90 üzerinde güvenlik puanı olan cihazlar.

- **Savunmasız cihazlar**: güvenlik puanı %70 altında olan cihazlar.

- **Geliştirme gerektiren cihazlar**: yüzde 70 ve %89 arasında güvenlik puanı olan cihazlar.

Bir rapor oluşturmak için:

1. Yan menüde **risk değerlendirmesi** ' ni seçin.

2. **Algılayıcı Seç** açılan listesinden bir algılayıcı seçin.

3. **Rapor oluştur**' u seçin.

4. **Arşivlenmiş raporlar** bölümünden **İndir** ' i seçin.

Şirket logosunu içeri aktarmak için:

- **Logoyu Içeri aktar**' ı seçin.

:::image type="content" source="media/how-to-generate-reports/import-logo-screenshot.png" alt-text="Risk değerlendirmesi görünümü aracılığıyla logonuzu içeri aktarın.":::

## <a name="see-also"></a>Ayrıca bkz.
[Site Haritası görünümleriyle çalışma](how-to-gain-insight-into-global-regional-and-local-threats.md#work-with-site-map-views)
