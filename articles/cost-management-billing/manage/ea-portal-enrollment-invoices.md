---
title: Azure Kurumsal kayıt faturaları
description: Bu makalede Azure Kurumsal faturanızı nasıl yöneteceğiniz ve işleme alacağınız açıklanır.
author: bandersmsft
ms.author: banders
ms.date: 08/20/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.subservice: enterprise
ms.reviewer: boalcsva
ms.openlocfilehash: 983cf6e7c9596d40f9db6221739af86175c58e87
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88689873"
---
# <a name="azure-enterprise-enrollment-invoices"></a>Azure Kurumsal kayıt faturaları

Bu makalede Azure Kurumsal Anlaşma (EA) faturanızı nasıl yöneteceğiniz ve işleme alacağınız açıklanır. Faturanız ücretlerinizin gösterimidir. Doğru olup olmadığını gözden geçirmeniz gerekir. Ayrıca faturanızı yönetmek için gerekebilecek diğer görevleri de öğrenmelisiniz.

## <a name="change-a-po-number-for-an-upcoming-overage-invoice"></a>Yaklaşan fazla kullanım faturası için PO numarasını değiştirme

Kuruluş yöneticisi fatura tarihinden önce bir satın alma siparişi (PO) numarası ayarlamadığı sürece Azure Enterprise Portal otomatik olarak bir varsayılan numara oluşturur. Kuruluş yöneticisi, otomatik fatura bildirimi e-postasını aldıktan sonraki yedi gün içinde de PO numarasını güncelleştirebilir.

### <a name="to-update-the-azure-services-purchase-order-number"></a>Azure hizmetleri satın alma siparişi numarasını güncelleştirmek için:

1. Azure Enterprise Portal'da **Rapor** > **Kullanım Özeti**'ni seçin.
1. Sağ üst köşedeki **PO Numaralarını Düzenle**'yi seçin.
1. **Azure Hizmetleri** radyo düğmesini seçin.
1. Tarih aralıkları açılan menüsünden bir **Fatura Dönemi** seçin.

   Fatura bildirimi aldıktan sonraki yedi günlük süre içinde, faturayı ödemeden önce PO numarasını düzenleyebilirsiniz.
1. **PO Numarası** alanına yeni bir PO numarası girin.
1. Yaptığınız değişikliği göndermek için **Kaydet**’i seçin.

### <a name="to-update-the-azure-marketplace-purchase-order-number"></a>Azure Market satın alma siparişi numarasını güncelleştirmek için:

1. Azure Enterprise Portal'da **Rapor** > **Kullanım Özeti**'ni seçin.
1. Sağ üst köşedeki **PO Numaralarını Düzenle**'yi seçin.
1. **Market** radyo düğmesini seçin.
1. Tarih aralıkları açılan menüsünden bir **Fatura Dönemi** seçin.

   Fatura bildirimi aldıktan sonraki yedi günlük süre içinde, faturayı ödemeden önce PO numarasını düzenleyebilirsiniz.
1. **PO Numarası** alanına yeni bir PO numarası girin.
1. Yaptığınız değişikliği göndermek için **Kaydet**’i seçin.

## <a name="azure-enterprise-billing-frequency"></a>Azure kurumsal faturalama sıklığı

Microsoft her yıl kayıt geçerlilik tarihinde Microsoft Azure hizmetlerinin ön ödemeli satın almalarını faturalandırır. Ön ödeme miktarlarını aşan kullanımlar, Microsoft tarafından borç olarak faturalandırılır.

- Ön ödeme ücretleri önceden aylık ücret ve yıllık faturalama temelinde fiyatlandırılır.
- Her ay fazla kullanım ücretleri hesaplanır ve faturalama döneminizin sonunda borç olarak faturalanır.

### <a name="billing-intervals"></a>Faturalama aralıkları

Faturalama aralığınız, ön ödemeli satın alma işlemlerinizi nasıl tamamladığınıza bağlı olarak belirlenir. Yıllık ön ödemeniz şu tarihlerden biri ile uyumlu olacaktır:

- Kayıt yıl dönümü tarihiniz
- Bir yıllık Değişiklik Aboneliğinizin geçerlilik tarihi.

Fazla kullanım faturanızın gönderileceği tarih, kayıt başlangıç ve kurulum tarihinize göre belirlenir:

- **Başlangıç tarihi, 1 Mayıs 2018 tarihinden önce olan doğrudan kayıtlar**:
  - Doğrudan Kurumsal Azure (EA) müşterisiyseniz Azure Market hizmetleri hariç olmak üzere Azure hizmetleri için yıllık faturalama dönemine tabi olursunuz. Faturalama döneminiz, yıl dönümü tarihinize (anlaşmanızın yürürlüğe girdiği tarihe) göre belirlenir.
  - Azure EA Ön Ödeme eşiğinizi %150 oranında aşmanız halinde, otomatik olarak yıl dönümü tarihinize göre belirlenen üç aylık faturalama dönemine geçersiniz. Ayrıca bir Azure hizmet fazla kullanımı faturası da gönderilir.
  - Azure Ön Ödeme eşiğinizi %150 oranında aşmazsanız kaydınız yıllık faturalama döneminde kalır. Fazla kullanım faturası, Ön ödeme yılının sonunda gönderilir.

- **Başlangıç tarihi, 1 Mayıs 2018 tarihinden sonra olan doğrudan kayıtlar**:
  - Azure tüketiminiz ve ücretleriniz için aylık faturalama dönemi kullanılarak ayrı faturalar düzenlenir.
  - Azure Ön Ödeme kapsamına girmeyen ücretler, fazla kullanım ödemesi olarak değerlendirilir.  

- **Kayıt tarihi 1 Mayıs 2018 tarihinden önce olan dolaylı kayıtlar**:

  Başlangıç tarihi 1 Mayıs 2018 öncesinde olan dolaylı bir Kurumsal Anlaşma (EA) müşterisiyseniz üç aylık faturalama dönemini kullanırsınız. Kanal iş ortağı (CP) size doğrudan fatura düzenler.  

- **Başlangıç tarihi, 1 Mayıs 2018 tarihinden sonra olan dolaylı kayıtlar**:

  Aylık faturalama dönemini kullanırsınız.  

### <a name="increase-your-azure-prepayment"></a>Azure Ön Ödemenizi artırma

Ön ödemenizi dilediğiniz zaman artırabilirsiniz. Artırdığınızda, içinde bulunulan yılın Ön ödeme süresinde kalan ay sayısına göre faturalandırılırsınız. Örneğin bir yıllık Değişiklik Aboneliği için kaydolur ve Ön ödemenizi altıncı ayda artırırsanız söz konusu dönemin kalan altı aylık bölümü için faturalandırılırsınız. Bu durumda, Ön ödeme döneminizin son altı ayına yönelik Ön ödeme miktarlarınız güncelleştirilir. Fazla kullanım ücretlerinin belirlenmesi için bu yeni miktarlar kullanılır.

### <a name="overage"></a>Kapasite Aşımı

Fazla kullanım olduğunda, faturalama döneminde Ön ödemenizi aşan kullanımlar veya rezervasyonlar için faturalandırılırsınız. Fazla kullanım miktarlarının öğelere göre hesaplama ayrıntılarını görüntülemek için kullanım özeti raporu sayfasını inceleyin veya kanal iş ortağınızla iletişim geçin.

Faturadaki her bir öğe için şu bilgileri göreceksiniz:

- **Genişletilmiş Tutar**: Ücretlerin toplamı
- **Ön Ödeme Kullanımı**: Ücretleri karşılamak için kullanılan Ön ödeme miktarı
- **Net Tutar**: Ön ödemenizi aşan ücretler

Geçerli vergiler yalnızca Ön ödemenizi aşan net tutar üzerinden hesaplanır.

Fazla kullanım faturalama süreci otomatiktir. Bildirimlerin ve faturaların gönderim tarihi, faturalama dönemi bitiş tarihinize göre belirlenir.

- Fazla kullanım bildirimi genellikle son faturalandırma tarihinizden yedi gün sonra gönderilir.
- Fazla kullanım faturaları, bildirimden sonra yedi ile dokuz gün arasında gönderilir.
- Fazla kullanım bildirimi ile faturalama arasındaki yedi gün boyunca ücretleri gözden geçirebilir ve sistem tarafından oluşturulan PO numaralarını güncelleştirebilirsiniz.

### <a name="azure-marketplace"></a>Azure Market

Tüm Azure ve Azure Market ücretleri iki ayrı fatura yerine tek bir faturada birleştirildiğinden Nisan 2019 faturalama döneminden itibaren geçerli olacak şekilde müşterilere tek bir Azure faturası gönderilecektir. Bu değişiklik Avustralya, Japonya veya Singapur'daki müşteriler için geçerli olmayacaktır.

Birleştirilmiş faturaya geçiş sürecinde kısmi bir Azure Market faturası alacaksınız. Bu son ayrı faturada, fatura güncelleştirme tarihine kadar olan Azure Market ücretleriniz yer alacaktır. Bu tarihten sonraki Azure Market ücretleri, Azure faturanızda görünecektir. Geçiş döneminden sonra tüm Azure ve Azure Market ücretlerini birleştirilmiş tek bir faturada göreceksiniz.  

### <a name="adjust-billing-frequency"></a>Faturalama sıklığını ayarlama

Müşterinin faturalama sıklığı yıllık, çeyreklik veya aylık olur. Faturalama dönemi müşteri sözleşmesini imzalarken belirlenir. Aylık faturalama en kısa faturalama aralığıdır.

- Doğrudan kayıtlarda yıllık faturalama dönemini üç aylık olarak değiştirmek için kuruluş yöneticisinin **onayı** gerekir. Dolaylı kayıtlarda iş ortağı yöneticisinin onayı gereklidir. Değişiklik geçerli faturalama çeyreğinin sonunda geçerlilik kazanır.
- Yıllık veya çeyreklik faturalama dönemini aylık olarak değiştirmek için sözleşmede bir **değişiklik** yapılmalıdır.  Mevcut kaydın faturalama döneminde yapılacak her değişiklik için kuruluş yöneticisinin veya sözleşmenizde "Fatura İlgili Kişisi" olarak belirlenmiş kişinin onayı gerekir.
- Onayınızı [Azure Enterprise Portal destek ekibine](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) **gönderin**. Şu sorun kategorisini seçin: **Ödeme ve Faturalama**.

Değişiklik geçerli faturalama çeyreğinin sonunda geçerlilik kazanır.

Düzeltme M503 imzalanmışsa, her sözleşmeyi herhangi bir sıklıktan aylık faturalamaya geçirebilirsiniz. 

### <a name="request-an-invoice-copy"></a>Fatura kopyası isteme

Faturanızın bir kopyasını istemek için iş ortağınızla iletişim kurun.

## <a name="credits-and-adjustments"></a>Krediler ve düzeltmeler

Kaydınıza uygulanan tüm kredileri veya düzeltmeleri [Azure Enterprise Portal](https://ea.azure.com)'ın **Raporlar** bölümünde görüntüleyebilirsiniz.

Kredileri görüntülemek için:

1. [Azure Enterprise Portal](https://ea.azure.com)'da **Raporlar** bölümünü seçin.
1. **Kullanım Özeti**'ni seçin.
1. Sağ üst köşeden **M** yerine **C** görünümüne geçiş yapın.
1. Azure hizmet ön ödemesi tablosundaki düzeltme alanını genişletin.
1. Kaydınıza uygulanan kredileri ve kısa açıklamaları görürsünüz. Örneğin: Hizmet Düzeyi Sözleşmesi Kredisi.

## <a name="pay-your-overage-with-your-azure-prepayment"></a>Fazla kullanımınızı Azure Ön Ödemesiyle karşılama

Azure Ön Ödemenizi fazla kullanıma uygulamak için aşağıdaki ölçütlere uygun olmanız gerekir:

- Ödenmemiş fazla kullanım ücretleriniz bulunmalı ve bunlar faturalanan hizmetin son tarihini izleyen bir yıl içinde olmalıdır.
- Kullanılabilir Azure Ön Ödeme tutarınız, geçmişe ait tüm ödenmemiş Azure faturaları da dahil olmak üzere tahakkuk eden ücretlerin tamamını kapsamalıdır.
- Tamamlanmasını istediğiniz faturalama dönemi tamamen kapatılmış olmalıdır. Faturalama her ayın beşinci günü kapatılır.
- Denkleştirilmesini istediğiniz faturalama dönemi tamamen kapatılmış olmalıdır.
- Azure Ön Ödeme İndiriminiz (APD), gerçek yeni Ön ödeme miktarından önceki tüketim için planlanan fonlar çıkarıldığında kalan tutara göre belirlenir. Bu gereksinim yalnızca tahakkuk eden fazla kullanım ücretleri için geçerlidir. Yalnızca Azure Ön Ödeme tüketimi olan hizmetlerde geçerlidir, dolayısıyla Azure Market ücretlerine uygulanamaz. Azure Market ücretleri ayrı faturalanır.

Fazla kullanım denkleştirmesini tamamlamak için sizin veya hesap ekibinizin bir destek isteği açması gerekir. Kuruluş yöneticinizden veya Fatura İlgili Kişisinden e-posta onayı istenecektir.

## <a name="move-charges-to-another-enrollment"></a>Ücretleri başka bir kayda taşıma

Kullanım verileri yalnızca bir aktarım geriye dönük tarihlendiğinde taşınır. Kullanım verilerini bir kayıttan diğerine taşımak için iki seçenek vardır:

- Bir kayıttan başka bir kayda hesap aktarımları
- Bir kayıttan başka bir kayda kayıt aktarımı

Her iki seçenekte de yardım için EA Destek Takımına bir [destek isteği](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) göndermeniz gerekir. 

## <a name="enterprise-agreement-usage-calculations"></a>Kurumsal Anlaşma kullanım hesaplamaları

Hizmetlerle ilgili temel genel fiyatlandırma bilgileri, ölçü birimleri, SSS ve kullanım raporlama yönergeleri için bkz. [Azure hizmetleri](https://azure.microsoft.com/services/) ve [Azure fiyatlandırması](https://azure.microsoft.com/pricing/). EA hesaplamaları hakkında daha fazla bilgi için aşağıdaki bölümlere göz atabilirsiniz.

### <a name="enterprise-agreement-units-of-measure"></a>Kurumsal Anlaşma ölçü birimleri

Kurumsal Anlaşma ölçü birimleri genellikle Microsoft Çevrimiçi Abonelik Sözleşmesi programı (MOSA) gibi programlarda görünenden farklıdır. Başka bir deyişle birden fazla hizmetteki ölçü birimi toplanarak normalleştirilmiş fiyatlandırma elde edilir. Azure Enterprise Portal'ın Kullanım Özeti görünümünde gösterilen ölçü birimi her zaman Enterprise ölçü birimidir. Her hizmetin ölçü biriminin ve dönüşümlerin yer aldığı listeye [Hizmet Kolay Adları](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) Excel dosyasından ulaşabilirsiniz.

### <a name="rounding-rules"></a>Yuvarlama kuralları

Azure Enterprise Portal, IEEE standardı olan Bankacı Yuvarlaması veya Gauss Yuvarlaması mantığı kullanılır. Bu mantık, yarım basamak değerlerini bir sonraki çift basamağa yuvarlar. Daha geleneksel olan Yarımı Yukarı Yuvarlama işlevleri yarım basamakları her zaman bir sonraki en yüksek basamağa yuvarlar. Bu Azure Enterprise Portal yöntemi, standart Excel mantığıyla karşılaştırıldığında grup genelinde daha doğru bir toplam değeri sunar.

Örneğin, kalan ilk basamak 5 olduğunda ve sonrasında basamak olmadığında veya sonraki basamaklar sıfır olduğunda sonraki en yakın çift basamağa yuvarlanır. Örneğin hem 2,315 hem de 2,325, en yakın 100'lük değere yuvarlandığında 2,32 olur.

Aşağıdaki tabloda yuvarlama ve dönüştürme için Azure Enterprise Portal kurallarını modellemek için kullanabileceğiniz Excel formülleri gösterilmiştir:

| Senaryo | Bankacı Mantığı Formülü |
| --- | --- |
| Yuvarlama kullanımı | =MROUND({_kaynak_}, 0,0002) |
| Yuvarlama Fiyatlandırması (2 ondalık basamak) | =MROUND({_kaynak_}, 0,02) |
| Yuvarlama Fiyatlandırması (0 ondalık basamak) | =MROUND({_kaynak_}, 2) |

### <a name="conversion-between-usage-detail-report-and-the-usage-summary-page"></a>Kullanım ayrıntıları raporu ile kullanım özeti sayfası verilerini birbirine dönüştürme

İndirilen kullanım verileri raporunda ham kullanım verileri için altı ondalık basamak bulunur. Ancak Azure Enterprise Portal’da gösterilen veriler, Ön Ödeme birimlerinde kullanımı dört ondalık basamağa yuvarlar, fazla kullanım birimlerinde de tüm ondalık basamakları siler. Ham kullanım verileri, Azure Enterprise Portal'da kullanılan birimlere dönüştürülmeden önce dört basamağa yuvarlanır. Daha sonra dönüştürülen Enterprise birimleri yeniden dört basamağa yuvarlanır. Dönüştürme öncesindeki tüketilen gerçek saat sayısı yalnızca indirilen kullanım raporunda gösterilir, Azure Enterprise Portal'da gösterilmez.

Örneğin: Kullanım raporunda 694,533404 gerçek SQL Server saatlerinin bildirildiğini düşünelim. Bu birimler önce 6,94533404 birim 100 işlem saatine dönüştürülür ve ardından 6,9453 şeklinde yuvarlanır ve Azure Enterprise Portal'da bu şekilde görüntülenir.

- Görüntülenen birimler Ön Ödeme Birimi Fiyatı ile çarpılır ve sonuç iki ondalık basamağa düşürülerek genişletilmiş faturalama tutarı belirlenir. Japon Yeni (JPY) ve Kore Wonu (KRW) için genişletilmiş tutardaki tüm ondalık basamaklar silinir.
- Fazla kullanım için faturalandırma birimleri altı basamak olacak şekilde kısaltıldıktan sonra Fazla Kullanım Fiyatı ile çarpılarak genişletilmiş fatura tutarı belirlenir.
- Yönetilen Hizmet Sağlayıcısı (MSP) faturalandırma sürecinde MSP olarak işaretlenmiş bir bölüm ile ilgili olan tüm kullanımlar, EA ölçü birimine dönüştürüldükten sonra sıfır ondalık basamak olacak şekilde kısaltılır. Sonuç olarak bu kullanımın sonucu, Azure Enterprise Portal'da bildirilen tüm kullanımların toplamından daha düşük olabilir. MSP’nin Azure Ön Ödeme bakiyesinde veya fazla kullanımda olma durumuna göre belirlenir.

### <a name="graduated-pricing"></a>Kademeli fiyatlandırma

Kurumsal Anlaşma fiyatlandırması şu an için kademeli fiyatlandırma özelliğine (kullanım arttıkça birim fiyatın azaldığı fiyatlandırma modeli) sahip değildir. Kademeli fiyatlandırmaya sahip olan bir MOSA programından Kurumsal Anlaşma'ya geçerseniz fiyatlarınız, hizmetin taban katmanından Kurumsal Anlaşma indirimleri için gerekli ayarlamaların çıkarılmasıyla belirlenir.

### <a name="partial-hour-billing"></a>Bir saatten daha kısa süreler için faturalama

Faturalandırılan tüm kullanımlar, tam saatlik artışlar yerine dakikaların kısmi saat cinsine dönüştürülmesiyle hesaplanır. Listedeki saatlik Enterprise ücretleri, toplam saat sayısına ve kısmi saat sayısına uygulanır.

### <a name="average-daily-consumption"></a>Günlük ortalama tüketim

Bazı hizmetler aylık olarak fiyatlandırılır ancak kullanımı günlük olarak bildirilir. Bu gibi durumlarda kullanım günlük olarak hesaplanır, 31'e bölünür ve ilgili faturalama ayındaki gün sayısına göre toplanır. Bu durumda herhangi bir ay için beklenen ücretler yüksek olmaz ancak 31 günden kısa aylar için biraz daha düşük olur.

### <a name="compute-hours-conversion"></a>İşlem saati dönüştürme

28 Ocak 2016 tarihinden önce A0, A2, A3 ve A4 Standart ve Temel Sanal Makineler ve Bulut Hizmetleri için yapılan kullanımlar, A1 Sanal Makineleri ölçüm dakikaları şeklinde gösteriliyordu. A0 VM'ler, A1 VM dakikalarının kesiri olarak sayılıyor; A2, A3 ve A4'ler ise birden fazla örneğe dönüştürülüyordu. Bu ilke müşterilerimiz için kafa karışıklığına neden olduğundan ayrılmış A0, A2, A3 ve A4 ölçümlerine dakika başına kullanım atanacak şekilde bir değişiklik yaptık.

Yeni ölçüm, 27-28 Ocak 2016 tarihinden itibaren kullanılmaya başlandı. Bu geçiş dönemine ait kullanımı gösteren CSV dosyasında şunların ikisini de görebilirsiniz:

- A1 ölçümüne göre hesaplanan kullanım
- Dağıtımınızın boyutuna göre yeni ayrılmış ölçüme göre hesaplanan kullanım.

| **Dağıtım boyutu** | **26 Ocak 2016’ya kadar A1’in katları olarak gösterilen kullanım** | **27 Ocak 2016’dan itibaren ayrılmış ölçüm olarak gösterilen kullanım** |
| --- | --- | --- |
| A0 | 0,25 A1 saati | 1 A0 saati |
| A2 | 2 A1 saati | 1 A2 saati |
| A3 | 4 A1 saati | 1 A3 saati |
| A4 | 8 A1 saati | 1 A4 saati |

### <a name="regions"></a>Bölgeler

Fiyatın alana ve bölgeye göre değiştiği hizmetler için geçerli coğrafi bölge ve bölge haritası aşağıdaki tabloda gösterilmiştir:

| Coğrafi Bölge | Veri Aktarım Bölgeleri | CDN Bölgeleri |
| --- | --- | --- |
| Bölge 1 | Kuzey Avrupa <br> Batı Avrupa <br> ABD Batı <br> ABD Doğu <br> ABD Orta Kuzey <br> ABD Orta Güney <br> ABD Doğu 2 <br> ABD Orta | Kuzey Amerika <br> Avrupa |
| Bölge 2 | Asya Pasifik Doğu <br> Asya Pasifik Güneydoğu <br> Doğu Japonya <br> Batı Japonya <br> Doğu Avustralya <br> Güneydoğu Avustralya | Asya Pasifik <br> Japonya <br> Latin Amerika <br> Orta Doğu/Afrika <br> Doğu Avustralya <br> Güneydoğu Avustralya |
| Bölge 3 | Güney Brezilya |   |

Aynı veri merkezinde barındırılan hizmetler arasındaki veri çıkışından ücret alınmaz. Örneğin Microsoft 365 ve Azure.

### <a name="azure-prepayment-and-unbilled-usage"></a>Azure Ön Ödeme kullanımı ve faturalandırılmamış kullanım

Azure Ön Ödeme, Azure hizmetleri için önceden ödenen tutardır. Hizmetler kullanıldıkça Azure Ön Ödeme tüketilir. Azure Ön Ödeme, birinci taraf Azure hizmetleri için faturalandırılır. Ancak bazı ücretler ayrıca faturalandırılır ve Azure Market hizmetleri Azure Ön Ödemeyi kullanmaz.

### <a name="charges-billed-separately"></a>Ayrı olarak faturalandırılan ücretler

Üçüncü taraf kaynaklar tarafından sunulan bazı ürün ve hizmetler, Azure Ön Ödemesini kullanmaz. Bu öğeler bunun yerine standart faturalama döneminin fazla kullanım faturasında ayrı olarak faturalanır.

Tüm Azure ve Azure Market ücretlerini, kaydın faturalama dönemiyle eşleşen tek bir fatura olacak şekilde birleştirdik. Birleştirilmiş fatura Avustralya, Japonya veya Singapur’daki müşteriler için geçerli değildir.

Birleştirilmiş faturada önce Azure ücretleri, ardından Azure Market ücretleri yer alacaktır. Avustralya, Japonya veya Singapur'daki müşteriler, Azure Market ücretlerini ayrı bir fatura olarak görmeye devam edecektir.

Standart faturalama döneminin sonunda fazla kullanım olmaması durumunda ayrı olarak yansıtılan ücretler ayrıca faturalanacaktır. Bu işlem Avustralya, Japonya ve Singapur'daki müşteriler için geçerlidir.

Aşağıdaki hizmetler ayrıca faturalandırılır:

- Canonical
- Citrix XenApp Essentials
- Citrix XenDesktop Kayıtlı Kullanıcı
- Openlogic
- Uzaktan Erişim Hakları XenApp Essentials Kayıtlı Kullanıcı
- Ubuntu Advantage
- Visual Studio Enterprise (Aylık)
- Visual Studio Enterprise (Yıllık)
- Visual Studio Professional (Aylık)
- Visual Studio Professional (Yıllık)

## <a name="what-to-expect-after-change-of-channel-partner"></a>Kanal iş ortağı değişikliğinden sonra neler beklemelisiniz?

Kanal iş ortağı değişikliği (COCP) ayın ortasında olursa, müşteri önceki ilişkili iş ortağı altındaki kullanımlar için bir fatura, yeni iş ortağı altındaki kullanımlar için de başka bir fatura alır.

Faturalar, fatura dönemi sona erdikten sonraki ay yayınlanır. Faturalama sıklığı aylık ise, Eylül ayının faturası her iki iş ortağı için de Ekim ayında yayınlanır. Fatura dönemi üç aylık veya yıllık ise müşteri, önceki ilişkili iş ortağıyla ilgili dönemdeki kullanımlara yönelik bir fatura alır. Diğer faturalar faturalama sıklığına göre yeni iş ortağı için oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar

- Aşağıdaki Excel dosyaları, Azure hizmetleri hakkında ayrıntılı bilgiler içerir ve her ayın 6. ve 20. gününde güncelleştirilir:

   | Başlık | Açıklama | Dosya adı |
   | --- | --- | --- |
   | [Hizmet Kolay Adları](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) | Tüm etkin hizmetlerin listesidir ve şunları içerir: <br>  <ul><li>hizmet kategorisi</li>   <li>hizmet kolay adı</li>   <li>Ön ödeme adı ve parça numarası</li> <li>tüketim adı ve parça numarası</li>   <li>ölçü birimi</li>   <li>bildirilen kullanım ile görüntülenen Enterprise Portal kullanımı arasındaki dönüştürme faktörleri</li></ul> | Hizmet\_Kolay\_Adları.xlsx |
   | [Hizmet İndirme Alanları](https://azurepricing.blob.core.windows.net/supplemental/Service_Download_Fields.xlsx) | Bu elektronik tabloda Kullanım İndirme Raporundaki hizmetle ilgili alanların tüm olası birleşimlerinin bir listesi bulunur. | Hizmet\_İndirme\_Alanları.xlsx |

- Faturanızı ve ücretlerinizi anlama konusunda bilgi için bkz. [Azure Kurumsal Anlaşma faturanızı anlama](../understand/review-enterprise-agreement-bill.md).
- Azure Enterprise Portal'ı kullanmaya başlamak için bkz. [Azure EA portalını kullanmaya başlama](ea-portal-get-started.md).
