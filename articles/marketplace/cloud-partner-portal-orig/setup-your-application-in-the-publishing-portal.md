---
title: Uygulamanızı Yayın Portalı'nda kur
description: Uygulamanızı Bulut Yayıncılık Portalı'nda nasıl kuracağınıza ilgili talimatlar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 2676dbf72309eeb51be1f08e7bae2c1502cc671e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280210"
---
# <a name="setup-your-application-in-the-publishing-portal"></a>Uygulamanızı Yayın Portalı'nda kur

Artık uygulamanızı yayın portalında ayarlamaya hazırsınız.

## <a name="login-and-create-a-new-offer"></a>Giriş ve Yeni Teklif Oluştur

1. Bulut İş [Ortağı Portalı'nda](https://cloudpartner.azure.com/)oturum açın.
2. Soldaki gezinti çubuğundan "+ Yeni teklif"e tıklayın ve "Müşteri Etkileşimi için Dynamics 365"i seçin.

![Yeni bir teklif seçme](./media/CRMScreenShot14.png)

1. Yeni bir teklif "Editör" görünümü şimdi sizin için açıldı ve biz yazmaya başlamak için hazırız.

![Yeni Teklif ekranı](./media/CRMScreenShot15.png)

1. Doldurulması gereken "formlar" "Editör" görünümünde solda görünür. Her "form" doldurulması gereken bir alan kümesinden oluşur. Gerekli alanlar kırmızı yıldız işaretiyle\*işaretlenir ( ).

Müşteri Katılımı teklifi için Dynamics 365 yazmanın dört ana biçimi vardır

* Teklif Ayarları
* Teknik Bilgiler
* Storefront Ayrıntılar
* Kişiler

## <a name="fill-out-the-offer-settings-form"></a>Teklif Ayarları formunu doldurun

Teklif ayarları formu, teklif ayarlarını belirtmek için temel bir formdur. Farklı alanlar aşağıda açıklanmıştır.

### <a name="offer-id"></a>Teklif Kimliği

Bu, bir yayımcı profilindeki teklif için benzersiz bir tanımlayıcıdır. Bu kimlik ürün URL'lerinde görünür olacaktır. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir. Kimlik tire ile sonlanamaz ve en fazla 50 karaktere sahip olabilir. Bir teklif yayına girdiğinde bu alan kilitlenir.

örneğin, bir yayıncı **"contoso"** yayıncılar teklif kimliği **"örnek-WebApp"** ile bir teklif oluşturursa, AppSource'da "https:\//appsource.microsoft.com/marketplace/apps/contoso.sample-WebApp?tab=Overview" olarak gösterecektir.

### <a name="publisher-id"></a>Yayıncı Kimliği

Bu açılır bilgi, bu teklifi altında yayınlamak istediğiniz yayımcı profilini seçmenize olanak tanır. Bir teklif yayına girdiğinde bu alan kilitlenir.

### <a name="name"></a>Adı

Bu teklifinizin görüntü adıdır. Bu [AppSource](https://appsource.microsoft.com/)gösterilecek adıdır. En fazla 50 karakter olabilir.

İlerlemenizi kaydetmek için "Kaydet"e tıklayın. Bir sonraki adım teklifiniz için Teknik Bilgi eklemek olacaktır.

## <a name="fill-out-the-technical-info-form"></a>Teknik Bilgi formunu doldurun


Teknik bilgi formu, Müşteri Katılımı çözümü için Dynamics 365'inize özel bilgileri dolduracağınız yerdir. Üzerinde gezinen daha fazla bilgi sunacak. Aşağıdaki örneğe bakın.

![Teknik bilgi ekranı](./media/CRMScreenShot16.png)

### <a name="application-info"></a>Başvuru Bilgileri

Çoğu yayıncı, yukarıdaki ekran görüntüsüne göre bu alanları varsayılan değerler, Kullanıcı, Hayır, Hayır ve boş bir Uygulama YapılandırmaSı URL'si ile bırakır.

### <a name="crm-package"></a>CRM Paketi

![CRM paket bilgileri](./media/CRMScreenShot17.png)

Bu alanların bir açıklaması aşağıda veda edilebaçıktır:

* Paketinizin dosya adı: CRM AppSource Paketiniz olan zip dosyasını oluştururken yukarıdaki adımda oluşturduğunuz dosya adı. Yukarıdaki örnekte, "Microsoft\_SamplePackage.zip"tir.
* Paket konumunuzurli: Azure Depolama hesabının URL'si, yukarıda belirtilen paket dosya adını içerir. Yukarıdaki bölümün Adım 9'da oluşturulan URL'sidir.
* Paket dosyanızda birden fazla crm paketi var mı: CrM'nin birden fazla versiyonunu farklı paketlerle **destekliyorsanız** Sadece Evet'i seçin. Çoğu ortak için bu "Hayır" olacaktır. Evet'i seçerseniz, çözümünüzün her sürümü için AppSource Paketleri oluşturmanız gerekir. _Not: Bu birden çok **zip** dosyaları olup olmadığını soran DeğİlDir. Birden çok solution.zip dosyanız varsa ancak yalnızca bir sürümünüz varsa, yine de "Hayır"ı seçmeniz gerekir. Paketleme aleti bunları otomatik olarak bir araya getirir._

### <a name="crm-package-availability"></a>CRM Paketi Kullanılabilirliği

Bu bölümde, paketinizin hangi CRM bölgelerine sunulacağını seçin. Hangi CRM bölgelerinin hangi ülkelere/bölgelere hizmet ettiği hakkında bilgi için lütfen aşağıdaki bağlantıya bakın:[https://o365datacentermap.azurewebsites.net/](https://o365datacentermap.azurewebsites.net/)

Not: Almanya'ya "Egemen ve ABD Gov Cloud" Sovereign'e konuşlandırılması, SERTIfikasyon SıRASıNDA ÖZEL İzİn VE ONAY GEREKTIRIR

## <a name="storefront-details"></a>Storefront Ayrıntılar

### <a name="offer-summary"></a>Teklif Özeti

Bu, teklifinizin değer teklifinin bir özetidir. Teklifinizin arama sayfasında görünür. En fazla 100 karakter olmalıdır.

### <a name="offer-description"></a>Teklif Açıklaması

Bu, uygulama ayrıntı sayfanızda görünecek açıklamadır. İzin verilen maksimum 1300 karakter

### <a name="industries"></a>Sektörler

Uygulamanızın en iyi hizalanmış olduğu sektörü seçin. Uygulamanızın birden çok sektörle ilgili olması durumunda, bunu boş bırakabilirsiniz.

### <a name="categories"></a>Kategoriler

Uygulamanızla alakalı kategorileri seçin. En fazla 3 seçin.

### <a name="app-type"></a>Uygulama türü

Uygulamanızın AppSource'ta etkinleştireceği deneme türünü seçin. 'Ücretsiz' uygulamanızın ücretsiz olduğu anlamına gelir. 'Deneme' müşterilerin AppSource'da kısa bir süre için uygulamanızı deneyebileceği anlamına gelir. Müşteri Katılımı uygulamaları için Dynamics 365 için 'deneme talebi' desteklenmez. Bu seçeneği seçmeyin.

### <a name="help-link-for-your-app"></a>Uygulamanız için yardım bağlantısı

Uygulamanız için yardım la ilgili bilgileri içeren bir sayfaya URL girin.

### <a name="supported-countriesregions"></a>Desteklenen ülkeler/bölgeler

Bu alan, teklifinizin deneme için hazır olacağı ülkeleri/bölgeleri belirler.

### <a name="supported-languages"></a>Desteklenen diller

Uygulamanızın desteklediği dilleri seçin. Uygulamanız bu listede olmayan ek dilleri destekliyorsa, teklifinizi yayınlamaya devam edin ve bize e-posta ile bildirin: [appsource@microsoft.com](mailto:appsource@microsoft.com) bize bildirin.

### <a name="app-version"></a>Uygulama sürümü

Uygulamanızın sürüm numarasını girin

### <a name="app-release-date"></a>Uygulama Yayın Tarihi

Uygulamanızın çıkış tarihini girin

### <a name="products-your-app-works-with-max-3"></a>Uygulamanızın birlikte çalıştığı ürünler (Max 3)

Uygulamanızın birlikte çalıştığı özel ürünleri listele. En fazla üç ürün listeleyebilirsiniz. Bir ürünü listelemek için artı işaretine (yeninin yanında) tıklayın ve uygulamanızın birlikte çalıştığı ürünün adını girmeniz için yeni bir açık metin alanı oluşturulacaktır.

### <a name="search-keywords-max-3"></a>Arama anahtar kelimeleri (Max 3)

AppSource, müşterinin anahtar kelimelere dayalı arama yapmasına olanak tanır. Uygulamanızın müşterilere gösterilecek anahtar kelime kümesini girebilirsiniz.

Örneğin, uygulama "E-posta Hizmetim" E-postaları, Posta, Posta hizmeti bazı anahtar kelimeler olabilir. Kullanıcıların AppSource arama kutusunda uygulamanızı aramak için büyük olasılıkla kullanacağı sözcükleri seçin.

### <a name="hide-key"></a>Anahtarı gizle

Bu, genel görünümden gizlemek için teklif önizleme URL'si ile birleştirilecek bir anahtardır. Bu bir şifre değil. Herhangi bir dizeyi buraya girebilirsiniz.

### <a name="offer-logo-png-format-48x48"></a>Teklif logosu (png formatı, 48x48)

Bu, uygulamanızın arama sayfasında görünür. **Yalnızca png biçimine izin verilir.** 48PX\*48PX çözünürlüğe sahip bir png görüntüsü yükleyin

### <a name="offer-logo-png-format-216x216"></a>Teklif logosu (png formatı, 216x216)

Bu, uygulamanızın ayrıntı sayfasında görünür. **Yalnızca png biçimine izin verilir.** 216PX\*216PX çözünürlüğe sahip bir png görüntüsü yükleyin

### <a name="videos"></a>Videolar

En fazla dört video yükleyebilirsiniz. Yüklemek istediğiniz her video için, videoyla ilişkilendirmek için video adını, URL'yi (yalnızca YouTube veya Vimeo) ve Küçük Resim'i doldurmanız gerekir. Küçük resim png formatında olmalı ve 1280PX\*720PX olmalıdır. Yeni video(lar) eklemek için artı işaretine tıklayın. Videolar küçük resim(ler) uygulamanızın ayrıntı sayfasında görünür.

### <a name="documents"></a>Belgeler

En fazla üç belgeyi PDF formatında yükleyebilirsiniz. Yüklemek istediğiniz her belge için belge adını doldurmanız ve belgeyi yüklemeniz gerekir. Belge pdf formatında olmalıdır.

Yeni belge(ler eklemek için), artı işaretine tıklayın

### <a name="screenshots"></a>Ekran görüntüleri

Bunlar, uygulamanızın AppSource ayrıntı sayfasında görünecek ekran görüntüleridir.

### <a name="privacy-policy"></a>Gizlilik İlkesi

Uygulamanızın gizlilik politikasına URL girin

### <a name="terms-of-use"></a>Kullanım koşulları

Uygulamanızın kullanım koşullarını girin. AppSource müşterilerinin uygulamanızı denemeden önce bu koşulları kabul etmeleri gerekmektedir

### <a name="support-url"></a>Destek URL'si

Uygulamanızın destek URL'sini girin.

### <a name="lead-destination"></a>Müşteri Adayı Hedef

Müşteri adayının depolanacağı bir CRM sistemi seçin. Aşağıdaki CRM sistemlerinden birine sahipseniz burada "Azure Tablosu"nu seçin: Salesforce, Marketo, Microsoft Dynamics CRM. Burada seçtiğiniz CRM sistemi, appsource 'da (müşteri adayları) uygulamanızı deneyen son kullanıcıların ayrıntılarını yazacağız. Seçtiğiniz CRM sistemine bağlı olarak, bir sonraki alan kümesini nasıl tamamlayabileceğiniz hakkında bilgi almak için aşağıdaki ilgili URL'yi tıklatın

* [Azure Tablosu](./cloud-partner-portal-lead-management-instructions-azure-table.md)
* [Marketo](./cloud-partner-portal-lead-management-instructions-marketo.md)
* [Microsoft Dynamics CRM](./cloud-partner-portal-lead-management-instructions-dynamics.md)
* [Salesforce](./cloud-partner-portal-lead-management-instructions-salesforce.md)

## <a name="storefront-details"></a>Storefront Ayrıntılar

İletişim bilgileri yalnızca iş ortağı ve Microsoft arasındaki iç iletişim için kullanılır. Not: Bu alanlarda izlenen bir e-posta adresi kullanmak önemlidir. Bu e-postayı AppSource'a yayınlamadaki ilerlemeniz hakkında sizinle iletişim kurmak için kullanacağız. Müşteriler etekleri yalnızca destek URL'si görünür.
