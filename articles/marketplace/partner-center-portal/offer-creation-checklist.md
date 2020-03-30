---
title: SaaS oluşturma kontrol listesi sunuyor - Azure için Ticari Pazar Yeri
description: SaaS teklif oluşturma işleminde sağlayabileceğiniz ayrıntılar. - Azure için Ticari Pazar
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 08/14/2019
ms.openlocfilehash: 60111b6a23c76314383c5f95be3eb4b38f90d3e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281349"
---
# <a name="saas-offer-creation-checklist"></a>SaaS teklifi oluşturma denetim listesi

SaaS teklif oluşturma işlemi birden çok sayfa boyunca götürecektir. Burada her sayfada sağlayabilir ayrıntıları, her öğe hakkında daha fazla bilgi edinmek için bağlantılar vardır.

Sağlamanız veya belirtmeniz gereken öğeler aşağıda belirtilmiştir. Bazı alanlar isteğe bağlıdır veya istediğiniz gibi değiştirebileceğiniz varsayılan değerlere sahiptir. Burada listelenen sırada bu bölümler üzerinde çalışmak zorunda değilsiniz.

| **Öğe**    | **Amaç**  |
| :---------- | :-------------------|
| [**Yeni Teklif Modal**](#new-offer-modal) | Toplar kimlik bilgileri sunar.  |
| [Teklif Kurulum Sayfası](#offer-setup-page) | Temel özellikleri kullanmayı seçmenize ve microsoft aracılığıyla teklifinizi nasıl satabileceğinizi seçmenize olanak tanır.  |
| [Özellikler Sayfası](#properties-page) | Teklifinizi pazarlarda gruplandırmak için kullanılan kategorileri ve sektörleri, teklifinizi destekleyen yasal sözleşmeleri ve uygulama sürümünüz tanımlayın. |
| [Teklif Listeleme Sayfası](#offer-listing-page) | Teklifinizin açıklamaları ve pazarlama varlıklarının açıklamaları da dahil olmak üzere pazarda görüntülenecek teklif ayrıntılarını tanımlayın. |
| [Önizleme Sayfası](#preview-page) | Teklifinizi daha geniş pazar kitlesine (ler) canlı olarak yayınlamadan önce teklifinizi yayınlamak için sınırlı bir Önizleme Hedef Kitlesi tanımlayın. |
| [Teklif Teknik Yapılandırma Sayfası](#technical-configuration-page)  | Yalnızca teklifi Microsoft aracılığıyla satmayı seçerseniz kullanılabilir. Teklifinize bağlanmak için kullanılan teknik ayrıntıları (URL yolu, webhook, kiracı kimliği ve uygulama kimliği) tanımlayın. |
| [**Yeni Plan Modal**](#plan-identity-modal) | Plan kimlik bilgilerini toplar.  |
| [Plan Listeleme Sayfası](#plan-listing-page)  | Yalnızca teklifi Microsoft aracılığıyla satmayı seçerseniz kullanılabilir. Planı pazarda listelemek için kullanılan ayrıntıları tanımlayın.  |
| [Plan Fiyatlandırma & Kullanılabilirlik Sayfası](#plan-pricing--availability-page)  | Yalnızca teklifi Microsoft aracılığıyla satmayı seçerseniz kullanılabilir.  Teklifinizin her planı (sürümü) için iş özelliklerini (fiyatlandırma modeli), hedef kitleyi ve pazar kullanılabilirliğini toplar.  |
| [Test Sürüşü Listeleme Sayfası](#test-drive-listing-page)  | Yalnızca teklifiniz için bir test sürüşü sunmayı seçerseniz kullanılabilir. Test sürüşü pazardaki listelemek için kullanılan ayrıntıları tanımlayın.  |
| Test Sürüşü Teknik Yapılandırma Sayfası  | Yalnızca teklifiniz için bir test sürüşü sunmayı seçerseniz kullanılabilir. Müşterilerin teklifinizi satın almayı taahhüt etmeden önce denemelerine olanak sağlayacak olan gösterinin teknik ayrıntılarını (veya "test sürüşü") tanımlayın.  |
| [İnceleme ve Yayın Sayfası](#review-and-publish-page)  | Yayımlamak istediğiniz değişiklikleri seçin, her sayfanın durumunu görün ve sertifika ekibine notlar sağlayın.  |


## <a name="new-offer-modal"></a>Yeni teklif modal 

Sağlamanız istenecek ilk bilgi, teklifiniz için bir kimlik ve takma addır. 

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Teklif Kimliği  | Gerekli, oluşturulduktan sonra değiştirilemez. Max 50 karakter ve yalnızca küçük, alfasayısal karakterler, tireveya alt çizgilerden oluşmalıdır. |
| Teklif takma adı  | Gereklidir. |

## <a name="offer-setup-page"></a>Teklif kurulum sayfası

Teklif kurulum sayfası, farklı kanallara ve satış hareketlerini seçebileceğiniz ve test sürüşü ve müşteri müşteri adayları gibi temel özelliklerin kullanımını beyan edebileceğiniz sayfadır. 

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------|  
| Microsoft üzerinden satmak ister misiniz?  | Gereklidir. Varsayılan: Evet |
| Potansiyel müşterilerin teklif listesiyle nasıl etkileşimde bulunmasını istiyorsunuz? (Harekete geçme çağrısı)  | Microsoft üzerinden satış değilse gereklidir. Varsayılan: Ücretsiz Deneme, Seçenekler: "Hemen alın", "Ücretsiz Deneme", "Bana ulaşın." |
| Deneme URL'si  | Müşterilerin teklif listesiyle etkileşim edebilmesi gerektiği için "Ücretsiz Deneme" seçilirse gereklidir. |
| Teklif URL'si  | Müşterilerin teklif listesiyle etkileşimde olması gerektiği için "Şimdi Al" seçilirse gerekli |
| Kanallar  | İsteğe bağlı. Varsayılan: CSP (satıcı) kanalına girilmez.  |
| Test Sürüşü | İsteğe bağlı. Varsayılan: Test sürücüsü etkin değil.  |
| Test Sürüşü Türü | Bir test sürüşü etkinleştirilmişse gereklidir. Varsayılan: Hiçbiri seçili değil. Seçenekler: Azure Kaynak Yöneticisi, Business Central için Dynamics 365, Müşteri Etkileşimi için Dynamics 365, Operasyonlar için Dynamics 365, Mantık uygulaması, Power BI.  |
| Müşteri Adayı Yönetimi - CRM sistemine bağlanma | Microsoft üzerinden satış yapılması veya listeleme teklifleri "Benimle iletişime geçin" olarak sunulması gerekir. Varsayılan: bağlı CRM sistemi yok. CRM seçenekleri: Azure tablosu, Azure blob, Dynamics CRM çevrimiçi, HTTP'lerin bitiş noktası, Marketo, Salesforce  |

## <a name="properties-page"></a>Özellikler sayfası

Özellikler sayfası, teklifinizi pazarlarda gruplandırmak için kullanılan kategorileri ve sektörleri, teklifinizi destekleyen yasal sözleşmeleri ve uygulama sürümünü tanımladığınız yerdir. Teklifinizin uygun şekilde görüntülenmesi ve doğru müşteri kümesine sunulması için teklifiniz hakkında eksiksiz ve doğru ayrıntıları verdiğinizden emin olun. 

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------|  
| Kategori ve alt kategori | Gerekli 1 ve max 3. Varsayılan: Hiçbiri seçili değil. |
| Endüstriler ve yan sanayiler | İsteğe bağlı. max 2 L1 Industries ve her L1 sanayi içinde max 2 alt sanayi, Varsayılan: Hiçbiri seçilmiş |
| Uygulama sürümü  | İsteğe bağlı. Varsayılan: Yok. |
| Standart Sözleşme Kullan  | İsteğe bağlı. Varsayılan: seçili değil.  | |
| Kullanım koşulları  | Standart Sözleşme seçili değilse gereklidir.  |

## <a name="offer-listing-page"></a>Teklif listesi sayfası

Giriş sayfası, müşterilerin teklifinizi pazarda listelediğinde gördükleri metin ve resimleri sağladığınız yerdir. 

| **Alan adı**    | **Notlar**   |
| :---------------- | :-----------| 
| Adı  | Gerekli, max 50 chars. |
| Özet  | Gerekli, max 100 chars. | 
| Açıklama  | Gerekli, max 3000 chars. |
| Başlangıç Talimatları  | Gerekli, max 3000 chars. |
| Başlangıç Talimatları  | Gerekli, max 3000 chars. |
| Anahtar kelimeleri ara  | İsteğe bağlı, önerilen, max 3 anahtar kelimeler. |
| Gizlilik ilkesi URL'si  | Gereklidir. |
| CSP Programı Pazarlama Malzemeleri URL  | İsteğe bağlı. |
| Faydalı bağlantılar Başlık + URL  | İsteğe bağlı. |
| Destekleyici Belgeler Başlık + Dosya  | Gerekli, min 1 ve max 3. PDF dosya biçimi olmalıdır. |
| Ekran görüntüleri  | Gerekli, min 1 ekran görüntüsü ve max 5; dört veya daha fazla önerilir. PNG formatında 1280 X 720 olmalıdır. |
| Mağaza logoları (Küçük, Orta, Büyük, Geniş, Kahraman)  | Küçük (48 X 48) ve Büyük (216 X 216) gereklidir; diğer boyutlar isteğe bağlı ama önerilir: Orta (90 x 90), Geniş (255 x 115), Hero (815 x 290). PNG formatında olmalıdır. |
| Video adı + URL + küçük resim  | İsteğe bağlı, tavsiye, max 4 videolar. Küçük resim PNG formatında 1280 x 720 olmalıdır. Video YouTube veya Vimeo'da barındırılmalıdır. |
| İletişim (CSP Programı, Mühendislik, Destek)  | Mühendislik ve Destek iletişimi gereklidir (Ad, e-posta ve telefon numarası); CSP Programı iletişim isteğe bağlı ama önerilir. |
| Destek URL'si  | Gereklidir. |

## <a name="preview-page"></a>Önizleme sayfası

Önizleme sayfası, teklifin yayından önce tüm gereksinimlerinizi karşıladığını doğrulamak için teklif önizlemenize erişecek hedef kitleyi belirttiğiniz sayfadır. 

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| AAD/MSA e-posta + açıklama | Gerekli, min 1 ve max 10 elle girilirse veya bir CSV dosyası yüklerken 20'ye kadar. |

## <a name="technical-configuration-page"></a>Teknik yapılandırma sayfası 

Teknik yapılandırma sayfası, Microsoft'un teklifinize bağlanmak için kullandığı teknik ayrıntıları belirttiğiniz sayfadır. Microsoft üzerinden satmamaya karar verdiyseniz, bu sayfa sizin için görünmez.

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Açılış Sayfası URL'si | Microsoft üzerinden satış yapılması gerekir. |
| Bağlantı webhook | Microsoft üzerinden satış yapılması gerekir. |
| Azure AD kiracı kimliği | Microsoft üzerinden satış yapılması gerekir. |
| Azure AD uygulama kimliği | Microsoft üzerinden satış yapılması gerekir. |

## <a name="plan-identity-modal"></a>Plan kimlik modal

Sağlamanız istenen ilk bilgi, Planınız için bir ad ve kimliktir. Microsoft üzerinden satmamaya karar verdiyseniz, bu sayfa sizin için görünmez.

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Plan Kimliği  | Microsoft üzerinden satış yapılması gerekir. Yaratılıştan sonra değiştirilemez. Max 50 karakter ve yalnızca küçük, alfasayısal karakterler, tireveya alt çizgilerden oluşmalıdır. |
| Plan Adı  | Microsoft üzerinden satış yapılması gerekir. Teklifteki tüm planlar arasında benzersiz olmalıdır. Maksimum 50 karakter. |

## <a name="plan-listing-page"></a>Liste sayfasını planla

Plan listeleme sayfası, müşterilerin planı pazarda görüntülerken görmeleri için metin sağladığınız sayfadır. Microsoft üzerinden satmamaya karar verdiyseniz, bu sayfa sizin için görünmez.

| **Alan adı**    | **Notlar**   |  
| :---------------- | :-----------| 
| Plan Açıklaması   | Microsoft üzerinden satış yapılması gerekir. Maksimum 500 karakter. | |

## <a name="plan-pricing--availability-page"></a>Kullanılabilirlik sayfası & fiyatlandırmayı planlayın

Plan fiyatlandırması ve kullanılabilirlik sayfası, teklifinizin her planı (sürümü) için iş özelliklerini, hedef kitleyi ve pazar kullanılabilirliğini tanımladığınız sayfadır. Microsoft üzerinden satmamaya karar verdiyseniz, bu sayfa sizin için görünmez.

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| Pazar durumu  | Gerekli, min 1 ve max 141. |
| Fiyatlandırma Modeli  | Gereklidir. Varsayılan: Sabit oran. Seçenekler: Kullanıcı başına sabit fiyat. |
| Minimum ve maksimum koltuklar  | İsteğe bağlı, yalnızca koltuk tabanlı fiyatlandırma modeli seçilirse kullanılabilir. |
| FaturaLama Terimi  | Gereklidir. Varsayılan: Aylık. Seçenekler: Aylık, Yıllık. |
| Fiyat  | Aylık fatura dönemi seçilirse, aylık olarak gerekli USD; veya yıllık faturalandırma dönemi seçilirse yıllık USD. |
| Hedef Kitleyi Planla  | İsteğe bağlı. Varsayılan: Genel plan. Seçenekler: Kiracı kimliğine göre genel, özel |
| Kısıtlı Plan Hedef Kitlesi (kiracı kimliği + açıklama)  | Özel plan seçilirse gereklidir. Min 1 ve en fazla 10 kiracı sı, el ile girilirse. CSV dosya alma olursa Max 20000. |

## <a name="test-drive-listing-page"></a>Test Sürüşü giriş sayfası

Yalnızca teklifiniz için bir test sürüşü sunmayı seçerseniz kullanılabilir. Test sürüşü pazardaki listelemek için kullanılan ayrıntıları tanımlayın.

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| Açıklama  | Gereklidir. |
| Kullanım Kılavuzu adı + dosya  | Gerekli, max 1 doc. PDF formatında olmalıdır. |
| Video adı, URL + küçük resim  | İsteğe bağlı, önerilir. Küçük resim JPGP veya PNG formatında 533 x 324 olmalıdır. Video YouTube veya Vimeo'da barındırılmalıdır. |

## <a name="review-and-publish-page"></a>Sayfayı gözden geçirme ve yayımlama

| **Alan adı**    | **Notlar**   | 
| :---------------- | :-----------| 
| Sertifika için notlar  | İsteğe bağlı. |

## <a name="next-steps"></a>Sonraki adımlar

- [Yeni bir SaaS teklifi oluşturma](./create-new-saas-offer.md)
