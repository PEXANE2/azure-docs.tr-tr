---
title: Çok aşamalı web testleriyle izleme - Azure Uygulama Öngörüleri
description: Azure Application Insights ile web uygulamalarınızı izlemek için çok aşamalı web testleri ayarlayın
ms.topic: conceptual
ms.date: 10/23/2019
ms.reviewer: sdash
ms.openlocfilehash: 3b8baad127b16a1bd9d071d0c3d4df68da8c3304
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655949"
---
# <a name="multi-step-web-tests"></a>Çok adımlı web testleri

Çok adımlı web testleri aracılığıyla bir web sitesiyle kaydedilen URL dizisini ve web sitesiyle etkileşimleri izleyebilirsiniz. Bu makale, Visual Studio Enterprise ile çok adımlı bir web testi oluşturma sürecinde size yol verecektir.

> [!NOTE]
> Çok aşamalı web testleri Visual Studio webtest dosyalarına bağlıdır. Visual Studio 2019'un webtest işlevselliği ile son sürüleceği [duyuruldu.](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/) Yeni özellikler eklenmemekle birlikte Visual Studio 2019'daki webtest işlevselliğinin halen desteklenmeye devam ettiğini ve ürünün destek yaşam döngüsü boyunca desteklenmeye devam edeceğini anlamak önemlidir. Azure Monitor ürün [ekibi, burada](https://github.com/MicrosoftDocs/azure-docs/issues/26050#issuecomment-468814101)çok aşamalı kullanılabilirlik testlerinin geleceğiyle ilgili soruları ele almıştır.  

## <a name="pre-requisites"></a>Ön koşullar

* Visual Studio 2017 Enterprise veya daha büyük.
* Visual Studio web performansı ve yük test araçları.

Test araçlarını önceden bulmak için gereklidir. Başlat **Visual Studio Installer** > **Bireysel bileşenleri** > **Hata ayıklama ve Web**performansı ve yük test > **araçları**test.

![Web performansı ve yük test araçları için öğenin yanındaki onay kutusuyla seçilen tek tek bileşenlerle Visual Studio yükleyici arabirimi ekran görüntüsü](./media/availability-multistep/web-performance-load-testing.png)

> [!NOTE]
> Çok adımlı web testlerinin bunlarla ilişkili ek maliyetleri vardır. Daha fazla bilgi için [resmi fiyatlandırma kılavuzuna başvurun.](https://azure.microsoft.com/pricing/details/application-insights/)

## <a name="record-a-multi-step-web-test"></a>Çok adımlı web testi kaydetme 

> [!WARNING]
> Artık çok adımlı kaydediciyi kullanmanızı önermiyoruz. Kaydedici temel etkileşimleri ile statik HTML sayfaları için geliştirilmiştir ve modern web sayfaları için işlevsel bir deneyim sağlamaz.

Visual Studio web testleri oluşturma kılavuzu için [resmi Visual Studio 2019 belgelerine](https://docs.microsoft.com/visualstudio/test/how-to-create-a-web-service-test?view=vs-2019)başvurun.

## <a name="upload-the-web-test"></a>Web testini yükleme

1. Kullanılabilirlik bölmesindeki Application Insights portalında > Test türü**Çok adımlı web testini****Test type** >  **oluştur'u**seçin.

2. Test konumlarını, sıklığını ve uyarı parametrelerini ayarlayın.

### <a name="frequency--location"></a>Sıklık & konumu

|Ayar| Açıklama
|----|----|----|
|**Test frekansı**| Testin her test konumundan ne sıklıkta çalıştırıldığını ayarlar. Beş dakikalık varsayılan sıklıkta ve beş test konumuyla, siteniz ortalama olarak dakikada bir test edilir.|
|**Test konumları**| Sunucularımızın URL'nize web istekleri gönderdiği yerlerdir. Web sitenizdeki sorunları ağ sorunlarından ayırt edebileceğinizden emin olmak için **önerilen minimum test yeri sayısı beştir.** En fazla 16 konum seçebilirsiniz.

### <a name="success-criteria"></a>Başarı kriterleri

|Ayar| Açıklama
|----|----|----|
| **Test zaman** |Yavaş yanıtlar hakkında uyarılmak için bu değeri azaltın. Yanıtlar sitenizden bu süre içinde alınmadıysa test başarısız sayılır. **Ayrıştırık bağımlı isteklerini**seçtiyseniz, bu dönemde tüm resimler, stil dosyaları, komut dosyaları ve diğer bağımlı kaynaklar alınmış olmalıdır.|
| **HTTP yanıtı** | Başarı olarak sayılan döndürülen durum kodu. 200, normal web sayfası döndürüldüğünü belirten koddur.|
| **İçerik eşleşmesi** | "Hoş geldin!" gibi bir ip. Her yanıtta büyük küçük harfe duyarlı bir tam eşleşme oluştuğunu test edebiliriz. Joker karakter bulunmayan düz bir dize olmalıdır. Sayfanızın içeriği değişirse bunu güncelleştirmeniz gerektiğini unutmayın. **Yalnızca İngilizce karakterler içerik eşleştirmesi ile desteklenir** |

### <a name="alerts"></a>Uyarılar

|Ayar| Açıklama
|----|----|----|
|**Gerçek zamanlı ya da yakın (Önizleme)** | Yakın zamanlı uyarıları kullanmanızı öneririz. Bu tür bir uyarının yapılandırılması, kullanılabilirlik testiniz oluşturulduktan sonra yapılır.  |
|**Klasik** | Artık yeni kullanılabilirlik testleri için klasik uyarıları kullanmamızı önermiyoruz.|
|**Uyarı konum eşiği**|En az 3/5 konum öneririz. Uyarı konum eşiği ile test konumlarının sayısı arasındaki en uygun ilişki, en az beş test konumuna sahip test **konumlarının uyarı konum eşik** = **sayısıdır - 2.**|

## <a name="configuration"></a>Yapılandırma

### <a name="plugging-time-and-random-numbers-into-your-test"></a>Testinize zaman ve rasgele sayılar takma

Dış bir kaynağa ait stoklar gibi zamana bağımlı veriler alan bir aracı test ettiğinizi varsayalım. Web testinizi kaydettiğinizde, belirli zamanları kullanmanız gerekse de, bunları testin parametreleri (StartTime ve EndTime) olarak ayarlarsınız.

![Benim awesome stok uygulaması ekran görüntüsü](./media/availability-multistep/app-insights-72webtest-parameters.png)

Testi çalıştırdığınızda, EndTime her zaman geçerli zaman, StartTime da 15 dakika öncesi olmalıdır.

Web Test Tarihi Saat Eklentisi, parametreleme sürelerini işlemenin yolunu sağlar.

1. İstediğiniz her değişken parametre değeri için bir web testi eklentisi ekleyin. Web testi araç çubuğunda, **Web Testi Eklentisi Ekle**’yi seçin.
    
    ![Web Testi Eklentisi Ekle](./media/availability-multistep/app-insights-72webtest-plugin-name.png)
    
    Bu örnekte, Tarih Saat Eklentisinin iki örneğini kullanacağız. Bir örnek "15 dakika önce" için, bir örnek de "şimdi" için.

2. Her eklentinin özelliklerini açın. Buna bir ad verip geçerli saat olarak kullanılmak üzere ayarlayın. Bunlardan birini Dakika Ekle = 15 olarak ayarlayın.

    ![Bağlam Parametreleri](./media/availability-multistep/app-insights-72webtest-plugin-parameters.png)

3. Web testi parametrelerinde, eklenti adına başvurmak için {{plug-in name}} kullanın.

    ![StartTime](./media/availability-multistep/app-insights-72webtest-plugins.png)

Artık testi portala yükleyin. Testin her çalıştırılışında dinamik değerler kullanacaktır.

### <a name="dealing-with-sign-in"></a>Oturum açmayla ilgilenme

Kullanıcılarınız uygulamanızda oturum açarsa, oturum açma benzetimi için bir dizi seçeneğiniz vardır; böylece, oturum açmanın ötesinde sayfaları test edebilirsiniz. Kullandığınız yaklaşım, uygulamanın sağladığı güvenlik türüne bağlıdır.

Her durumda, uygulamanızda yalnızca test amacıyla bir hesap oluşturmalısınız. Mümkünse, web testlerinin gerçek kullanıcıları etkileme olasılığını önlemek için test hesabının izinlerini kısıtlayın.

**Basit kullanıcı adı ve şifre** Her zamanki gibi bir web testi kaydedin. Önce tanımlama bilgilerini silin.

**SAML kimlik doğrulaması**

|Özellik adı| Açıklama|
|----|-----|
| İzleyici Uri | SAML belirteci için seyirci URI.  Bu, ACS ad alanı ve ana bilgisayar adı da dahil olmak üzere Access Control Service (ACS) için URI'dir. |
| Sertifika Şifresi | Gömülü özel anahtara erişim sağlayacak istemci sertifikasının parolası. |
| Müşteri Sertifikası  | Base64 kodlanmış formatında özel anahtarlı istemci sertifikası değeri. |
| Ad Tanımlayıcısı | Belirteci için ad tanımlayıcısı |
| Sonra Değil | Belirteç geçerli olacak zaman ası.  Varsayılan değer 5 dakikadır. |
| Önce Değil | Geçmişte oluşturulan bir belirteç için zaman dilimi geçerli olacaktır (zaman eğriliklerini ele almak için).  Varsayılan değer (negatif) 5 dakikadır. |
| Hedef Bağlam Parametre Adı | Oluşturulan iddiayı alacak bağlam parametresi. |


**İstemci sırrı** Uygulamanızda istemci sırrı içeren bir oturum açma rotası varsa, bu rotayı kullanın. Azure Active Directory (AAD), gizli anahtarla oturum açmayı sağlayan bir hizmet örneğidir. AAD’de gizli anahtar, Uygulama Anahtarı’dır.

Aşağıda uygulama anahtarı kullanan bir Azure web uygulaması için web testi örneği verilmiştir:

![Örnek ekran görüntüsü](./media/availability-multistep/client-secret.png)

Gizli anahtar (AppKey) kullanarak AAD’den belirteç alın.
Yanıttan taşıyıcı belirteci ayıklayın.
Yetkilendirme üst bilgisinde taşıyıcı belirteç kullanarak API çağırın.
Web testinin gerçek bir istemci olduğundan emin olun - yani AAD'de kendi uygulaması vardır - ve clientId + uygulama anahtarını kullanın. Test altındaki hizmetinizin AAD'de de kendi uygulaması vardır: bu uygulamanın appID URI'si kaynak alanındaki web testine yansıtılır.

### <a name="open-authentication"></a>Açık Kimlik Doğrulaması
Microsoft veya Google hesabınızla oturum açma, bir açık kimlik doğrulaması örneğidir. OAuth kullanan çok sayıda uygulama, alternatif gizli anahtar da sağlar; bu nedenle ilk taktiğiniz bu olasılığın incelenmesi olmalıdır.

Testinizde OAuth kullanılarak oturum açılması gerekiyorsa, genel yaklaşım şöyledir:

Web tarayıcınız, kimlik doğrulama sitesi ve uygulamanız arasındaki trafiği incelemek için Fiddler gibi bir araç kullanın.
Farklı makineler veya tarayıcılar kullanarak veya uzun aralıklarla (süresi dolacak şekilde belirteçleri izin vermek için) iki veya daha fazla oturum açın.
Farklı oturumları karşılaştırarak, kimlik doğrulama sitesinden geri geçirilen belirteci tanımlayın; başka bir deyişle oturum açıldıktan sonra uygulama sunucunuza geçirilen belirteç.
Visual Studio’yu kullanarak web testini kaydetme
Belirteçleri parametreleyin; belirteç kimlik doğrulayıcıdan döndürüldüğünde ve sitede sorgu sırasında kullanıldığında parametre ayarı. (Visual Studio testi parametrelemeyi dener, ancak belirteçleri doğru parametrelemez.)

## <a name="troubleshooting"></a>Sorun giderme

Özel [sorun giderme makalesi](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanılabilirlik Uyarıları](availability-alerts.md)
* [Url ping web testleri](monitor-web-app-availability.md)
