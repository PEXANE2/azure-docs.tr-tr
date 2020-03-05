---
title: Azure Kurumsal kayıt faturaları
description: Bu makalede Azure Kurumsal faturanızı nasıl yöneteceğiniz ve işleme alacağınız açıklanır.
author: bandersmsft
ms.author: banders
ms.date: 03/03/2020
ms.topic: conceptual
ms.service: cost-management-billing
ms.reviewer: boalcsva
ms.openlocfilehash: df2d4eb1b28ac9f13511692ca7588414ea4fdff0
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78272389"
---
# <a name="azure-enterprise-enrollment-invoices"></a>Azure Kurumsal kayıt faturaları

Bu makalede Azure Kurumsal Anlaşma (EA) faturanızı nasıl yöneteceğiniz ve işleme alacağınız açıklanır. Faturanız ücretlerinizin gösterimidir. Doğru olup olmadığını gözden geçirmeniz gerekir. Ayrıca faturanızı yönetmek için gerekebilecek diğer görevleri de öğrenmelisiniz.

## <a name="change-a-po-number-for-an-overage-invoice"></a>Fazla kullanım faturası için PO numarasını değiştirme

Kuruluş yöneticisi fatura tarihinden önce bir satın alma siparişi (PO) numarası ayarlamadığı sürece Azure Enterprise Portal otomatik olarak bir varsayılan numara oluşturur. Kuruluş yöneticisi, otomatik fatura bildirimi e-postasını aldıktan sonraki yedi gün içinde de PO numarasını güncelleştirebilir.

### <a name="to-update-the-azure-services-purchase-order-number"></a>Azure hizmetleri satın alma siparişi numarasını güncelleştirmek için:

1. Azure Enterprise Portal'da **Rapor** > **Kullanım Özeti**'ni seçin.
1. Sağ üst köşedeki **PO Numaralarını Düzenle**'yi seçin.
1. **Azure Hizmetleri** radyo düğmesini seçin.
1. Tarih aralıkları açılan menüsünden bir **Fatura Dönemi** seçin.

   Fatura bildirimi aldıktan sonraki yedi günlük süre içinde, faturayı ödemeden önce PO numarasını düzenleyebilirsiniz.
1.  **PO Numarası** alanına yeni bir PO numarası girin.
1. Yaptığınız değişikliği göndermek için **Kaydet**'i seçin.

### <a name="to-update-the-azure-marketplace-purchase-order-number"></a>Azure Market satın alma siparişi numarasını güncelleştirmek için:

1. Azure Enterprise Portal'da **Rapor** > **Kullanım Özeti**'ni seçin.
1. Sağ üst köşedeki **PO Numaralarını Düzenle**'yi seçin.
1. **Market** radyo düğmesini seçin.
1. Tarih aralıkları açılan menüsünden bir **Fatura Dönemi** seçin.

   Fatura bildirimi aldıktan sonraki yedi günlük süre içinde, faturayı ödemeden önce PO numarasını düzenleyebilirsiniz.
1.  **PO Numarası** alanına yeni bir PO numarası girin.
1. Yaptığınız değişikliği göndermek için **Kaydet**'i seçin.

## <a name="cadence-of-azure-enterprise-billing"></a>Azure Kurumsal faturalama temposu

Microsoft her yıl kayıt geçerlilik tarihinde Microsoft Azure hizmetlerinin satın alma taahhüdünü faturalandırır. Taahhüt miktarlarını aşan kullanımlar, Microsoft tarafından borç olarak faturalandırılır.

- Taahhüt ücretleri önceden aylık ücret ve yıllık faturalama temelinde fiyatlandırılır.
- Her ay fazla kullanım ücretleri hesaplanır ve faturalama döneminizin sonunda borç olarak faturalanır.

### <a name="billing-intervals"></a>Faturalama aralıkları

Faturalama aralığınız, taahhüt satın alma işlemlerinizi nasıl tamamladığınıza bağlı olarak belirlenir. Yıllık taahhüdünüz şu tarihlerden biri ile uyumlu olacaktır:

- Kayıt yıl dönümü tarihiniz
- Bir yıllık Değişiklik Aboneliğinizin geçerlilik tarihi.

Fazla kullanım faturanızın gönderileceği tarih, kayıt başlangıç ve kurulum tarihinize göre belirlenir:

- **Başlangıç tarihi, 1 Mayıs 2018 tarihinden önce olan doğrudan kayıtlar**:
  - Doğrudan Kurumsal Azure (EA) müşterisiyseniz Azure Market hizmetleri hariç olmak üzere Azure hizmetleri için yıllık faturalama dönemine tabi olursunuz. Faturalama döneminiz, yıl dönümü tarihinize (anlaşmanızın yürürlüğe girdiği tarihe) göre belirlenir.
  - EA parasal taahhüt (MC) eşiğinizi %150 oranında aşmanız halinde, otomatik olarak yıl dönümü tarihinize göre belirlenen üç aylık dönem faturalama dönemine geçersiniz. Ayrıca bir Azure hizmet fazla kullanımı faturası da gönderilir.
  - Parasal taahhüt eşiğinizi %150 oranında aşmazsanız kaydınız yıllık faturalama dönemi olarak kalır. Fazla kullanım faturası, taahhüt yılının sonunda gönderilir.

- **Başlangıç tarihi, 1 Mayıs 2018 tarihinden sonra olan doğrudan kayıtlar**:
  - Azure tüketiminiz ve ücretleriniz için aylık faturalama dönemi kullanılarak ayrı faturalar düzenlenir.
  - Parasal taahhüdünüzün kapsamına girmeyen ücretler, fazla kullanım ödemesi olarak değerlendirilir.  

- **Kayıt tarihi 1 Mayıs 2018 tarihinden önce olan dolaylı kayıtlar**:

  Başlangıç tarihi 1 Mayıs 2018 öncesinde olan dolaylı bir Kurumsal Anlaşma (EA) müşterisiyseniz üç aylık faturalama dönemini kullanırsınız. Kanal iş ortağı (CP) size doğrudan fatura düzenler.  

- **Başlangıç tarihi, 1 Mayıs 2018 tarihinden sonra olan dolaylı kayıtlar**:

  Aylık faturalama dönemini kullanırsınız.  

### <a name="increase-your-monetary-commitment"></a>Parasal taahhüdünüzü artırma

Taahhüdünüzü dilediğiniz zaman artırabilirsiniz. O ayın taahhüt süresinde kalan ay sayısı üzerinden faturalandırılırsınız. Örneğin bir yıllık Değişiklik Aboneliği için kaydolur ve taahhüdünüzü altıncı ayda artırırsanız, o dönemin kalan altı aylık bölümü için faturalandırılırsınız. Bu durumda taahhüt miktarlarınız, taahhüt döneminizin son altı ayı için güncelleştirilir. Fazla kullanım ücretlerinin belirlenmesi için bu yeni miktarlar kullanılır.

### <a name="overage"></a>Kapasite Aşımı

Fazla kullanım için faturalama döneminde taahhüdünüzü artan kullanımlar veya rezervasyonlar için faturalandırılırsınız. Fazla kullanım miktarlarının öğelere göre hesaplama ayrıntılarını görüntülemek için kullanım özeti raporu sayfasını inceleyin veya kanal iş ortağınızla iletişim geçin.

Faturadaki her bir öğe için şu bilgileri göreceksiniz:

- **Genişletilmiş Tutar**: Ücretlerin toplamı
- **Taahhüt Kullanımı**: Ücretleri karşılamak için kullanılan taahhüt miktarı
- **Net Tutar**: Taahhüdünüzü aşan ücretler

Geçerli vergiler yalnızca taahhüdünüzü aşan net tutar üzerinden hesaplanır.

Fazla kullanım faturalama süreci otomatiktir. Bildirimlerin ve faturaların gönderim tarihi, faturalama dönemi bitiş tarihinize göre belirlenir.

- Fazla kullanım bildirimi genellikle son faturalandırma tarihinizden yedi gün sonra gönderilir.
- Fazla kullanım faturaları, bildirimden sonra yedi ile dokuz gün arasında gönderilir.
- Fazla kullanım bildirimi ile faturalama arasındaki yedi gün boyunca ücretleri gözden geçirebilir ve sistem tarafından oluşturulan PO numaralarını güncelleştirebilirsiniz.

### <a name="azure-marketplace"></a>Azure Market

Tüm Azure ve Azure Market ücretleri iki ayrı fatura yerine tek bir faturada birleştirildiğinden Nisan 2019 faturalama döneminden itibaren geçerli olacak şekilde müşterilere tek bir Azure faturası gönderilecektir. Bu değişiklik Avustralya, Japonya veya Singapur'daki müşteriler için geçerli olmayacaktır.

Birleştirilmiş faturaya geçiş sürecinde kısmi bir Azure Market faturası alacaksınız. Bu son ayrı faturada, fatura güncelleştirme tarihine kadar olan Azure Market ücretleriniz yer alacaktır. Bu tarihten sonraki Azure Market ücretleri, Azure faturanızda görünecektir. Geçiş döneminden sonra tüm Azure ve Azure Market ücretlerini birleştirilmiş tek bir faturada göreceksiniz.  

## <a name="adjust-billing-frequency"></a>Faturalama sıklığını ayarlama

Müşterinin faturalama sıklığı yıllık, çeyreklik veya aylık olur. Faturalama dönemi müşteri sözleşmesini imzalarken belirlenir. Aylık faturalama en kısa faturalama aralığıdır.

- Doğrudan kayıtlarda yıllık faturalama dönemini üç aylık olarak değiştirmek için kuruluş yöneticisinin **onayı** gerekir. Dolaylı kayıtlarda iş ortağı yöneticisinin onayı gereklidir. Değişiklik geçerli faturalama çeyreğinin sonunda geçerlilik kazanır.
- Yıllık veya çeyreklik faturalama dönemini aylık olarak değiştirmek için sözleşmede bir **değişiklik** yapılmalıdır.  Mevcut kaydın faturalama döneminde yapılacak her değişiklik için kuruluş yöneticisinin veya sözleşmenizde "Fatura İlgili Kişisi" olarak belirlenmiş kişinin onayı gerekir.
- Onayınızı [Azure Enterprise Portal destek ekibine](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) **gönderin**. Şu sorun kategorisini seçin: **Ödeme ve Faturalama**.

Değişiklik geçerli faturalama çeyreğinin sonunda geçerlilik kazanır.

Düzeltme M503 imzalanmışsa, her sözleşmeyi herhangi bir sıklıktan aylık faturalamaya geçirebilirsiniz. 

## <a name="credits-and-adjustments"></a>Krediler ve düzeltmeler

Kaydınıza uygulanan tüm kredileri veya düzeltmeleri [Azure Enterprise Portal](https://ea.azure.com)'ın **Raporlar** bölümünde görüntüleyebilirsiniz.

Kredileri görüntülemek için:

1. [Azure Enterprise Portal](https://ea.azure.com)'da **Raporlar** bölümünü seçin.
1. **Kullanım Özeti**'ni seçin.
1. Sağ üst köşeden **M** yerine **C** görünümüne geçiş yapın.
1. Azure hizmet taahhüdü tablosundaki düzeltme alanını genişletin.
1. Kaydınıza uygulanan kredileri ve kısa açıklamaları görürsünüz. Örneğin: Hizmet Düzeyi Sözleşmesi Kredisi.

## <a name="request-an-invoice-copy"></a>Fatura kopyası isteme

Faturanızın bir kopyasını istemek için iş ortağınızla iletişim kurun.

## <a name="overage-offset"></a>Fazla kullanım denkleştirmesi

Parasal taahhüdünüzü fazla kullanıma uygulamak için aşağıdaki ölçütleri karşılamanız gerekir:

- Ödenmemiş fazla kullanım ücretleriniz bulunmalı ve bunlar faturalanan hizmetin son tarihini izleyen bir yıl içinde olmalıdır.
- Kullanılabilir parasal taahhüt tutarınız, geçmişe ait tüm ödenmemiş Azure faturaları da dahil olmak üzere tahakkuk eden ücretlerin tamamını kapsamalıdır.
- Tamamlanmasını istediğiniz faturalama dönemi tamamen kapatılmış olmalıdır. Faturalama her ayın beşinci günü kapatılır.
- Denkleştirilmesini istediğiniz faturalama dönemi tamamen kapatılmış olmalıdır.
- Azure Taahhüt İndiriminiz (ACD) için gerçek yeni taahhüt eksi önceki tüketim için planlanan fonlar temel alınır. Bu gereksinim yalnızca tahakkuk eden fazla kullanım ücretleri için geçerlidir. Yalnızca parasal taahhüt tüketimi olan hizmetlerde geçerlidir, dolayısıyla Azure Market ücretlerine uygulanamaz. Azure Market ücretleri ayrı faturalanır.

Fazla kullanım denkleştirmesini tamamlamak için sizin veya hesap ekibinizin bir destek isteği açması gerekir. Kuruluş yöneticinizden veya Fatura İlgili Kişisinden e-posta onayı istenecektir.

## <a name="view-price-sheet-information"></a>Fiyat listesi bilgilerini görüntüleme

Kuruluş yöneticileri Azure hizmetleri kayıtlarıyla ilişkili fiyat listesini görüntüleyebilir.

Geçerli fiyat listesini görüntülemek için:

1. Azure Enterprise Portal'da **Raporlar**'ı ve ardından **Fiyat Listesi**'ni seçin.
2. Fiyat listesini görüntüleyin veya **İndir**'i seçin.

![Fiyat listesi bilgilerini gösteren örnek](./media/ea-portal-enrollment-invoices/ea-create-view-price-sheet-information.png)

Geçmiş fiyat listesini indirmek için:

1. Azure Enterprise Portal'da **Raporlar**'ı ve ardından **Kullanımı İndir**'i seçin.
2. Fiyat listesini indirin.

![Eski fiyat listesinin indirileceği yeri gösteren örnek](./media/ea-portal-enrollment-invoices/create-ea-view-price-sheet-download-historical-price-list.png)

Fiyat farklılıklarının nedenlerinden bazıları şunlardır:

- Önceki kayıtla yeni kayıt arasında fiyatlandırma değişmiş olabilir. Fiyat değişiklikleri olmasının nedeni, belirli bir kayıt için fiyatlandırma anlaşmasının, sözleşmenin başlangıç tarihinden bitiş tarihine kadar geçerli olmasıdır.
- Kayıt yeni bir kayda aktarıldığında, yeni sözleşmenin fiyatlandırması geçerli olur. Fiyatlandırma, sizin fiyat listenize göre belirlenir ve bu da yeni kayıtta daha yüksek olabilir.
- Kaydın süresinin uzatılması durumunda da fiyatlandırma değişir. Fiyatlar kullandıkça öde fiyatlarına göre değişir.

## <a name="request-detailed-usage-information"></a>Ayrıntılı kullanım bilgileri isteme

Kuruluş yöneticileri, Azure Enterprise Portal'da portalında kullanım verilerinin özetini, tüketilen parasal taahhüdü ve ek kullanımla ilişkili ücretleri görüntüleyebilir. Ücretler, tüm hesaplar ve abonelikler genelinde özet düzeyinde gösterilir.

Belirli hesaplardaki ayrıntılı kullanımı görüntülemek için **Raporlar** > **Kullanımı İndir**'e gidip kullanım ayrıntıları raporunu indirebilirsiniz.

> [!NOTE]
> Kullanım ayrıntıları raporu hiçbir geçerli vergiyi içermez. Kullanımın tahakkuk etmesiyle rapora yansıtılması arasında sekiz saatlik bir gecikme olabilir.

Dolaylı kayıtlarda, maliyetle ilgili bilgileri görebilmeniz için önce iş ortağınızın ek ücret işlevini etkinleştirmesi gerekir.

## <a name="reports"></a>Raporlar

Kuruluş yöneticileri, Azure Enterprise Portal'da portalında kullanım verilerinin özetini, tüketilen parasal taahhüdü ve ek kullanımla ilişkili ücretleri görüntüleyebilir. Ücretler, tüm hesaplar ve abonelikler genelinde özet düzeyinde gösterilir.

### <a name="azure-enterprise-reports"></a>Azure Kurumsal raporları

- Kullanım özeti ve grafikler
- Hizmet kullanım raporu
- Bakiye ve ücret raporu
- Kullanım ayrıntıları raporu
- Azure Market ücretleri raporu
- Fiyat listesi
- Gelişmiş rapor indirme
- CSV raporunu biçimlendirme

### <a name="to-view-the-usage-summary-reports-and-graphs"></a>Kullanım özeti raporlarını ve grafiklerini görüntülemek için:

1. Azure Enterprise Portal'a gidin.
1. Sol taraftaki bölmeden **Raporlar**'ı seçin.
1. **Kullanım Özeti** sekmesini seçin.
1. Sol üstteki tarihi aralığı menüsünden taahhüt dönemini seçin.
1. Ek ayrıntıları görüntülemek için grafikte bir dönem veya ay seçin.
1. Bu sekmede şunları yapabilirsiniz:
   - Kullanım, hizmet fazla kullanımı, ayrı olarak faturalandırılan ücretler ve Azure Market ücretlerinin dökümünü kapsayan ve aylara göre ayrılmış grafiği görüntüleyin.
   - Grafiğin altından departmanlara, hesaplara ve aboneliklere göre filtreleyin.
   - **Hizmetlere Göre Ücret** ve **Hiyerarşiye Göre Ücret** dökümleri arasında geçiş yapın.
   - Azure hizmetlerinin ayrıntılarını, ayrı olarak faturalandırılan ücretleri ve Azure Market ücretlerini görüntüleyin.

## <a name="service-usage-report"></a>Hizmet kullanım raporu

Hizmet kullanım raporu sayfası, kuruluş yöneticilerinin hizmet kullanım verilerinin özetini görüntülemesine olanak tanır. Kullanım, tüm hesaplar ve abonelikler genelinde özet düzeyinde gösterilir. Ayrıntılı kullanımı görüntülemek için raporu hesaplara veya aboneliklere göre filtreleyebilirsiniz.

> [!NOTE]
> Gerçek kullanım tarihi ile kullanımın bu rapora yansıtıldığı tarih arasında beş güne kadar fark olabilir.

### <a name="to-view-the-report"></a>Raporu görüntülemek için:

1. Azure Enterprise Portal'da oturum açın.
1. Sol taraftaki gezinti menüsünden **Raporlar**'ı seçin.
1. **Kullanım Özeti** sekmesini seçin.
1. Tarih aralığını seçin.
1. Görüntülemek istediğiniz hesapları veya abonelikleri seçin.
1. İsteğe bağlı olarak şunları da yapabilirsiniz:
   - Farklı dökümleri görüntülemek için **Hizmetlere Göre Ücret** ve **Hiyerarşiye Göre Ücret** görünümleri arasında geçiş yapın.
   - Hizmet Adı, Ölçü Birimi, Tüketilen Birimler, Geçerli Ücret ve Genişletilmiş Maliyet gibi ayrıntıları görüntüleyin.

## <a name="download-csv-reports"></a>CSV raporlarını indirme

Aylık rapor indirme sayfası, kuruluş yöneticilerinin birden fazla raporu CSV dosyası olarak indirmesini sağlar. Şu raporlar indirilebilir:

- Bakiye ve ücret raporu
- Kullanım ayrıntıları raporu
- Azure Market ücretleri raporu
- Fiyat listesi

### <a name="to-download-reports"></a>Raporları indirmek için:

1. Azure Enterprise Portal'da **Rapor**'u seçin.
1. Üst şeritten **Kullanımı İndir**'i seçin.
1. İlgili ay raporunun yanındaki **İndir**'i seçin.

### <a name="csv-report-formatting-issues"></a>CSV raporunu biçimlendirme sorunları

Azure Enterprise Portal'ın CSV raporlarını Euro cinsinden görüntüleyen kullanıcılar, virgüller ve noktalarla ilgili biçimlendirme sorunlarıyla karşılaşabilir.

Örneğin şunları görebilirsiniz:

| **ServiceResource** | **ResourceQtyConsumed** | **ResourceRate** | **ExtendedCost** |
| --- | --- | --- | --- |
| Saat | 24 | 0,0535960591133005 | 12,863,054,187,192,100,000,000 |

Şunu görmeniz gerekir:

| ServiceResource | ResourceQtyConsumed | ResourceRate | ExtendedCost |
| --- | --- | --- | --- |
| Saat | 24 | 0,0535960591133005 | 1,2863054187192120000000 |

Bu biçimlendirme sorununun nedeni, Excel'in içeri aktarma işlevindeki varsayılan ayarlardır. Excel, tüm alanları "Genel" metin olarak içeri aktarır ve bunun şu matematik standardında ayrılmış bir sayı olduğunu kabul eder. Örneğin: "1,000.00".

Binlik ayırıcı olarak nokta (.), ondalık ayırıcı olarak da virgül (,) kullanılan bir Avrupa para birimi için değer hatalı bir şekilde görüntülenecektir. Örneğin: "1.000,00". İçeri aktarma sonuçları, bölge ve dil ayarınıza göre değişebilir.

### <a name="to-import-the-csv-file-without-formatting-issues"></a>CSV dosyasını biçimlendirme sorunu yaşamadan içeri aktarmak için:

1. Microsoft Excel'de **Dosya** > **Aç**'ı seçin.
   Metin İçeri Aktarma Sihirbazı görüntülenir.
1. **Özgün Veri Türü** bölümünde **sınırlandırılmış**'ı seçin.  Varsayılan değer **Sabit Genişlik** olarak belirlenmiştir.
1. **İleri**’yi seçin.
1. Sınırlayıcılar bölümünde **Virgül** onay kutusunu seçin. **Sekme** seçiliyse seçimini kaldırın.
1. **İleri**’yi seçin.
1. **ResourceRate** ve **ExtendedCost** sütunlarının üzerine gelin.
1. **ResourceRate** sütununu seçin. Siyah renkte vurgulanmış olarak görünür.
1. **Sütun Veri Biçimi** bölümünde **Genel** yerine **Metin** girişini seçin. Sütun başlığı **Genel** yerine **Metin** olur.
1. 8 ve 9. adımları **ExtendedCost** sütunu için tekrarlayın ve **Son**'u seçin.

> [!TIP]
> CSV dosyalarını otomatik olarak Excel'de açılacak şekilde ayarladıysanız Excel'deki **Aç** işlevini kullanmanız gerekir. Excel'de **Dosya** > **Aç**'ı seçin.

## <a name="balance-and-charge-report"></a>Bakiye ve ücret raporu

Bakiye ve ücret raporu bakiyelere, yeni satın alımlara, Azure Market hizmeti ücretlerine, ayarlamalara ve fazla kullanım ücretlerine ilişkin bilgilerin aylık bir özetini sunar.

Azure Hizmet Taahhüdü özet tablosundaki tüm satırlar, tüm aylarda sabit kalır. Tüm satın alma ve düzeltme bilgileri **Yeni Satın Alma Ayrıntıları** ve **Düzeltme Ayrıntıları** bölümlerinde görüntülenir.

### <a name="download-the-balance-and-charge-report"></a>Bakiye ve ücret raporunu indirme

1. Azure Enterprise Portal'da kuruluş yöneticisi olarak oturum açın.
1. Sol taraftaki bölmeden **Raporlar**'ı seçin.
1. **Rapor İndirme** sekmesini seçin.
1. **Bakiye ve Ücret** sütununun altından uygun ayı seçin ve raporu indirin.

## <a name="usage-detail-report"></a>Kullanım ayrıntıları raporu

Kullanım ayrıntıları raporu bir kayıt tarafından tüketilen hizmetlerin ve miktarların aylık özetini sunar. Buna ölçüm adları, ölçüm türleri ve tüketilen miktarlar hakkında bilgiler dahildir.

### <a name="download-the-usage-detail-report"></a>Kullanım ayrıntısı raporunu indirme

1. Azure Enterprise Portal'da kuruluş yöneticisi olarak oturum açın.
1. Sol taraftaki gezinti menüsünden **Raporlar**'ı seçin.
1. **Kullanımı İndir** sekmesini seçin.
1. **Kullanım Ayrıntısı** sütununun altından uygun ayı seçin ve raporu indirin.

## <a name="azure-marketplace-charges-in-azure-enterprise-portal-reports"></a>Azure Enterprise Portal raporlarındaki Azure Market ücretleri

İki tür Azure Market ücreti vardır:

- **Kullanıma dayalı:** Ürünlerin kullanımı, işlem birimi üzerinden ölçülür.  Örneğin sanal makineler saat, Bing API aramaları ise arama sayısı üzerinden ücretlendirilir.
- **Aylık ücret:** Kullanıma veya erişime göre aylık olarak faturalandırılır.

Azure Market ücretleri hakkında daha fazla bilgi için bkz. [Azure Market Hakkında SSS](https://azure.microsoft.com/marketplace/faq/).

Azure Enterprise Portal'daki farklı ücretleri görüntülemek için:

- **Kullanım özeti raporu**: Kullanıma dayalı **ve** aylık ücret türündeki Azure Market ücretlerini gösterir.
- **Market ücretleri raporu**: **Yalnızca** kullanıma dayalı Azure Market ücretlerini gösterir.  Tek seferlik ücretler gösterilmez.

> [!NOTE]
> Azure Market, MPSA kayıtlarıyla kullanılamaz.

## <a name="advanced-report-download"></a>Gelişmiş rapor indirme

Belirli tarih aralıklarına veya hesaplara göre rapor oluşturmak için gelişmiş rapor indirme işlevini kullanabilirsiniz. Daha geniş kayıt kümelerine yer verebilme amacıyla çıkış dosyasının biçimi 30 Ağustos 2016 tarihinden itibaren CSV olarak değiştirilmiştir.

1. Azure Enterprise Portal'da **Gelişmiş Rapor İndirme**'yi seçin.
1. **Uygun Tarih Aralığı**'nı seçin.
1. **Uygun Hesaplar**'ı seçin.
1. **Kullanım Verilerini İste**'yi seçin.
1. Rapor durumu **İndir** olana kadar **Yenile** düğmesini seçin.
1. Raporu indirin.

## <a name="reporting-for-non-enterprise-administrators"></a>Kuruluş yöneticileri dışındaki kullanıcılar için raporlama

Kuruluş yöneticileri, bir kaydın bölüm yöneticileri (DA) ve hesap sahipleri (AO) için ücretleri görüntüleme erişimi sunabilir. Erişim verilen hesap sahipleri kendi hesaplarına ve aboneliklerine ait CSV raporlarını indirebilir. Bu kişiler bilgileri Azure Enterprise Portal'ın raporlama bölümünde görsel olarak da görüntüleyebilir.

### <a name="to-enable-access"></a>Erişimi etkinleştirmek için:

 1. Azure Enterprise Portal'da kuruluş yöneticisi olarak oturum açın.
 1. Sol gezinti panelinde **Yönet**’i seçin.
 1. **Kayıt** sekmesini seçin.
 1. **Kayıt Ayrıntıları** bölümünde **DA Ücretleri Görüntüleme** veya **AO Ücretleri Görüntüleme** girişinin yanındaki kalem simgesini seçin.
 1. **Etkin**'i seçin.
 1. **Kaydet**’i seçin.

### <a name="to-view-reports"></a>Raporları görüntülemek için:

1. Azure Enterprise Portal'da bölüm yöneticisi veya hesap sahibi olarak oturum açın.
1. Sol taraftaki gezinti menüsünden **Raporlar**'ı seçin.
1. Hesap ve abonelik bilgilerini görsel olarak görüntülemek için **Kullanım Özeti** sekmesini seçin.
1. CSV raporlarını görüntülemek için **Kullanımı İndir**'i seçin.

Bölüm yöneticileri ve hesap sahipleri, ücretleri **Gelişmiş Rapor İndirme** işlevini kullanarak görüntüleyemez. Ücretler $0 olarak görünür.

Hesap sahibine verilen görüntüleme izinleri, hesap sahiplerini ve ilgili aboneliklerde gerekli izinlere sahip olan tüm kullanıcıları kapsar. Dolaylı müşteriyseniz ücret özelliklerinin kanal iş ortağınız tarafından etkinleştirilmesi gerekir.

## <a name="move-usage-data-to-another-enrollment"></a>Kullanım verilerini başka bir kayda taşıma

Kullanım verileri yalnızca bir aktarım geriye dönük tarihlendiğinde taşınır. Kullanım verilerini bir kayıttan diğerine taşımak için iki seçenek vardır:

- Bir kayıttan başka bir kayda hesap aktarımları
- Bir kayıttan başka bir kayda kayıt aktarımı

Her iki seçenekte de yardım için EA Destek Takımına bir [destek isteği](https://support.microsoft.com/supportrequestform/cf791efa-485b-95a3-6fad-3daf9cd4027c) göndermeniz gerekir. 

## <a name="azure-ea-pricing-overview"></a>Azure EA fiyatlandırmasına genel bakış

Bu bölümde, kullanımın nasıl hesaplandığı hakkında ayrıntılı bilgi verilmektedir. Kurumsal Anlaşma'da hesaplaması karmaşık olan çeşitli Azure hizmetleriyle ilgili sık sorulan soruların yanıtları sağlanmaktadır.

### <a name="price-protection"></a>Fiyat koruması

Müşteri veya kanal iş ortağı olarak Müşteri Fiyat Listesi (CPS) içinde gösterilen veya Azure satın alma işleminizin yapıldığı tarihte geçerli olan fiyatlardan veya bunlardan daha düşük fiyatlardan yararlanmanız garanti edilir. Bu fiyat, taban fiyat olarak adlandırılır. Azure satın alma tarihinden sonra sunulan hizmetler için hizmet kullanıma ilk sunulduğunda uygun düzeyde indirim uygulanan fiyat geçerli olur. Fiyat koruması, taahhüt süreniz boyunca geçerlidir ve bu süre, Kurumsal Anlaşmanıza göre bir ile üç yıl arasında olabilir.

### <a name="price-changes"></a>Fiyat değişiklikleri

Microsoft, kayıt dönemi içinde belirli Azure hizmetlerinin geçerli Kurumsal Anlaşma fiyatını düşürebilir. Düşük fiyatlar, geçerli olduğu süre boyunca mevcut müşterilere de yansıtılır. Bu ücretlerin ilerleyen dönemlerde artırılması durumunda bir hizmetin kayıt fiyatı, yukarıda belirtildiği şekilde fiyat koruması üst sınırını aşmaz. Ancak daha önceki düşük fiyata kıyasla bir artış yapılabilir. Her iki durumda da kayıtla ilişkilendirilmiş olan Kuruluş ve İş ortağı yöneticilerine e-posta yoluyla yapılacak fiyat değişikliklerini ileterek müşterileri ve dolaylı kanal iş ortaklarını bilgilendiririz.

### <a name="current-effective-pricing"></a>Geçerli etkin fiyatlandırma

Müşteri ve kanal iş ortakları, [Azure Enterprise Portal](https://ea.azure.com/)'da oturum açıp kayıtla ilgili fiyat listesi sayfasına giderek geçerli fiyatlandırma ayrıntılarını görüntüleyebilir. Azure'ı kanal iş ortaklarımızın biri aracılığıyla satın aldıysanız ve kaydınızla ilgili fiyatlandırma ayrıntılarını görüntülenme seçeneği etkinleştirilmediyse fiyatlandırma güncelleştirmelerini kanal iş ortağınızdan almanız gerekir.

### <a name="introduction-of-new-azure-services"></a>Yeni Azure hizmetlerinin kullanıma sunulması

Azure'ı sürekli geliştiriyor ve belirli aralıklarla mevcut hizmetlerden farklı fiyatlara sahip olan yeni hizmetler ekliyoruz. Bazı önizleme hizmetleri otomatik olarak kullanıma sunulurken diğer hizmetler için [Azure Hesabı portalından](https://account.windowsazure.com/PreviewFeatures) işlem yapılması gerekir.

Bazı hizmetler promosyon fiyatlandırmasıyla kullanıma sunulduktan sonra ilerleyen dönemlerde fiyat artışı yaşanabilir.

Hizmetler önizleme aşamasından genel kullanım (GA) aşamasına geçerken tam performans ve güvenilirlik SLA'larının uygulanması nedeniyle fiyatlar artabilir. Bu tür artışlar, normal taban fiyat koruması kapsamında değildir. Bu hizmetlerin kullanımı, artan fiyat üzerinden ücretlendirilir. Bu yeni hizmetlerle ilgili ücretlerden kaçınmak için bunları kullanmamanız gerekir.

### <a name="introduction-of-regional-pricing"></a>Bölgesel fiyatlandırmanın kullanıma sunulması

Kullanıma sunulan yeni hizmetler olduğu gibi bölgesel destek arttıkça tek bir küresel yapıdan bölgesel modele doğru geçiş yapan hizmetler de mevcuttur.

Bir hizmet belirli bir bölgeye yayılmaya başladığında taban fiyat koruması, yeni bölgeler için de geçerli olur. Ancak ilerleyen zamanlarda aynı hizmet için eklenen diğer bölgeler, yeni hizmetler olarak kabul edilir ve taban fiyat korumasından bağımsız olarak ayrı fiyatlara sahip olur.

### <a name="enterprise-devtest"></a>Kurumsal Geliştirme ve Test

Kuruluş yöneticileri, hesap sahiplerinin Kurumsal Geliştirme ve Test teklifini temel alan abonelikler oluşturmasına olanak sağlayabilir. Hesap sahibinin, temel alınan aboneler için gerekli olan Kurumsal Geliştirme ve Test aboneliklerini ayarlaması gerekir. Bu yapılandırma, etkin Visual Studio abonelerinin geliştirme ve test iş yüklerini Azure'da özel Kurumsal Geliştirme ve Test fiyatlarıyla çalıştırmasını sağlar. Daha fazla bilgi için bkz. [Kurumsal Geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0148p/).

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

İndirilen kullanım verileri raporunda ham kullanım verileri için altı ondalık basamak bulunur. Ancak Azure Enterprise Portal'da gösterilen veriler, kullanımı taahhüt birimleri için dört ondalık basamağa yuvarlar, fazla kullanım birimlerinde de tüm ondalık basamakları siler. Ham kullanım verileri, Azure Enterprise Portal'da kullanılan birimlere dönüştürülmeden önce dört basamağa yuvarlanır. Daha sonra dönüştürülen Enterprise birimleri yeniden dört basamağa yuvarlanır. Dönüştürme öncesindeki tüketilen gerçek saat sayısı yalnızca indirilen kullanım raporunda gösterilir, Azure Enterprise Portal'da gösterilmez.

Örneğin: Kullanım raporunda 694,533404 gerçek SQL Server saatlerinin bildirildiğini düşünelim. Bu birimler önce 6,94533404 birim 100 işlem saatine dönüştürülür ve ardından 6,9453 şeklinde yuvarlanır ve Azure Enterprise Portal'da bu şekilde görüntülenir.

- Görüntülenen birimler Taahhüt Birimi Fiyatı ile çarpılır ve sonuç iki ondalık basamağa düşürülerek genişletilmiş faturalama tutarı belirlenir. Japon Yeni (JPY) ve Kore Wonu (KRW) için genişletilmiş tutardaki tüm ondalık basamaklar silinir.
- Fazla kullanım için faturalandırma birimleri altı basamak olacak şekilde kısaltıldıktan sonra Fazla Kullanım Fiyatı ile çarpılarak genişletilmiş fatura tutarı belirlenir.
- Yönetilen Hizmet Sağlayıcısı (MSP) faturalandırma sürecinde MSP olarak işaretlenmiş bir bölüm ile ilgili olan tüm kullanımlar, EA ölçü birimine dönüştürüldükten sonra sıfır ondalık basamak olacak şekilde kısaltılır. Sonuç olarak bu kullanımın sonucu, Azure Enterprise Portal'da bildirilen tüm kullanımların toplamından daha düşük olabilir. MSP'nin parasal taahhüt bakiyesinde veya fazla kullanımda olma durumuna göre belirlenir.

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

Aynı veri merkezinde barındırılan hizmetler arasındaki veri çıkışından ücret alınmaz. Örneğin, Office 365 ve Azure.

### <a name="monetary-commitment-and-unbilled-usage"></a>Parasal taahhüt ve faturalanmamış kullanım

Azure parasal taahhüdü Azure hizmetleri için önceden ödenmiş tutardır. Hizmetler kullanıldıkça parasal taahhüt tüketilir. Parasal taahhüt, birinci taraf Azure hizmetleri için faturalandırılır. Ancak bazı ücretler ayrıca faturalandırılır ve Azure Market hizmetleri parasal taahhüdü kullanmaz.

### <a name="charges-billed-separately"></a>Ayrı olarak faturalandırılan ücretler

Üçüncü taraf kaynaklar tarafından sunulan bazı ürün ve hizmetler, Azure parasal taahhüdünü kullanmaz. Bu öğeler bunun yerine standart faturalama döneminin fazla kullanım faturasında ayrı olarak faturalanır.

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

## <a name="azure-marketplace-for-ea-customers"></a>EA müşterileri için Azure Market

Doğrudan müşteriler, Azure Market ücretlerini Azure Enterprise Portal'da görüntüleyebilir. Azure Market'ten yapılan satın alma ve tüketim işlemleri, parasal taahhüdün dışında faturalanır ve üç aylık veya aylık borçlar olarak yazılır.

Dolaylı müşteriler, Azure Market aboneliklerini Azure Enterprise Portal'ın **Abonelikleri Yönet** sayfasında bulabilir ancak fiyatlar gizli olacaktır. Müşteriler, Azure Market fiyatları için Lisanslama Çözümü Sağlayıcısı (LSP) ile iletişime geçmelidir.

Yeni aylık veya yıllık yinelenen Azure Market satın alma işlemleriyle ilgili ücretlerin tamamı, Azure Market öğelerinin satın alındığı dönemde faturalanacaktır. Bu öğeler bir sonraki dönemde ilk satın alma işlemiyle aynı günde otomatik olarak yenilenecektir.

Aylık yinelenen ücretler her takvim ayının ilk gününde yenilenmeye devam edecektir. Yıllık ücretler de satın alma tarihinin yıl dönümünde yenilenecektir.

Azure Market’teki bazı üçüncü taraf satıcı hizmetleri artık Kurumsal Anlaşma (EA) parasal taahhüt bakiyenizi kullanacaktır. Önceden bu hizmetler, EA parasal taahhüdü dışında faturalandırılıyor ve faturası ayrı kesiliyordu. Azure Market’teki bu hizmetler için EA parasal taahhüdü, müşterinin satın alma ve ödeme yönetiminin kolaylaştırılmasına yardımcı olur. Parasal taahhüdü kullanmaya başlayan hizmetlerin tam listesi için lütfen [Azure web sitesindeki 6 Mart 2018 güncelleştirmesini](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment/) inceleyin.

### <a name="partners"></a>İş Ortakları

LSP'ler, Azure Enterprise Portal'ın fiyat listesi sayfasından Azure Market fiyat listesini indirebilir. Sağ üstten **Market Fiyat Listesi**'ni seçin. Azure Market fiyat listesinde kullanılabilir durumdaki tüm hizmetler ve fiyatları yer alır.

Fiyat listesini indirmek için:

1. Azure Enterprise Portal'da **Raporlar** > **Fiyat Listesi**'ni seçin.
1. Sağ üst köşede, adınızın altında yer alan Azure Market fiyat listesi bağlantısını bulun.
1. Bağlantıya sağ tıklayıp **Hedefi Farklı Kaydet**'i seçin.
1. **Kaydet** penceresinde belgenin adını `AzureMarketplacePricelist.zip` olarak değiştirin. Bunu yaptığınızda dosya .xlsx yerine .zip dosyasına dönüşür.
1. İndirme işlemi tamamlandıktan sonra ülkeye özgü fiyat listelerinin bulunduğu bir zip dosyası elde edersiniz.
1. LSP'ler ülkeye özgü fiyatlandırma için ilgili ülke dosyasına başvurmalıdır. LSP'ler yeni veya kullanımdan kaldırılan SKU'ları **Bildirimler** sekmesinden takip edebilir.
1. Fiyatlar çok sık değiştirilmez. Fiyat artışları ve döviz kuru (FX) değişiklikleri LSP'lere 30 gün öncesinden e-posta ile bildirilir.
1. LSP'lere kayıt, ISV ve üç aylık dönem başına bir fatura gönderilir.

### <a name="enabling-azure-marketplace-purchases"></a>Azure Market'ten satın alma işlemlerini etkinleştirme

Kuruluş yöneticileri, kendi kayıtları altındaki tüm Azure abonelikleri için Azure Market'ten satın alma işlemlerini devre dışı bırakabilir veya etkinleştirebilir. Kuruluş yöneticilerinin satın alma işlemlerini devre dışı bırakması ve önceden Azure Market aboneliği satın almış olan Azure aboneliklerinin mevcut olması durumunda ilgili Azure Market abonelikleri iptal edilmez ve bu ayardan etkilenmez.

Müşteriler, doğrudan Azure aboneliklerini Azure Enterprise Portal kayıtlarıyla ilişkilendirerek Azure EA aboneliğine dönüştürebilir ancak bu işlem sonucunda alt abonelikleri otomatik olarak dönüştürülmez.

Azure Market'ten satın alma işlemlerini etkinleştirmek için:

1. Azure Enterprise Portal'da kuruluş yöneticisi olarak oturum açın.
1. **Yönet**'e gidin.
1. **Kayıt Ayrıntıları**'nın altında **Azure Market** satırının yanındaki kalem simgesini seçin.
1. **Etkin/Devre Dışı** durumunu veya Ücretsiz **Yalnızca KLG SKU'ları** seçeneğini belirleyin.
1. **Kaydet**’i seçin.

> [!NOTE]
> KLG (kendi lisansını getir) ve Yalnızca Ücretsiz seçeneği, Azure Market SKU satın alma ve edinme işlemlerini KLG ve Ücretsiz SKU'lar ile sınırlar.

[Azure Enterprise Portal raporlarındaki Azure Market ücretleri](#azure-marketplace-charges-in-azure-enterprise-portal-reports) hakkında daha fazla bilgi edinin.

### <a name="services-billed-hourly-for-azure-ea"></a>Azure EA için saatlik olarak faturalanan hizmetler

Aşağıdaki hizmetler, MOSP ile aylık ücret yerine Kurumsal Anlaşma kapsamında saatlik olarak faturalandırılır:

- Uygulama Teslim Ağı
- Web Uygulaması Güvenlik Duvarı

### <a name="azure-remoteapp"></a>Azure RemoteApp

Kurumsal Anlaşmanız varsa Azure RemoteApp için Kurumsal Anlaşma fiyat düzeyinize göre ödeme yaparsınız. Ek ücret alınmaz. Standart fiyata ilk 40 saat dahildir. Sınırsız fiyata ilk 80 saat dahildir. RemoteApp, 80 saati aşan kullanımları iletmez.

## <a name="azure-marketplace-faq"></a>Azure Market Hakkında SSS

Bu bölümde Azure parasal taahhüdünüzün Azure Market'teki bazı üçüncü taraf satıcı hizmetlerine nasıl uygulanabileceği anlatılmaktadır.

### <a name="what-changed-with-azure-marketplace-services-and-ea-monetary-commitment"></a>Azure Market hizmetleri ve EA parasal taahhüt ile neler değişti?

1 Mart 2018 itibarıyla bazı üçüncü taraf hizmetler EA parasal taahhüdünü (MC) kullanmaya başladı. Azure ayrılmış sanal makine örnekleri (RI) dışındaki hizmetler önceden EA parasal taahhüdünden ayrı bir şekilde faturalandırılıyordu.

Parasal taahhüt kullanım kapsamını Azure Market'te yayımlanmış olan ve sıklıkla satın alınan bazı üçüncü taraf hizmetleri kapsayacak şekilde genişlettik. Azure Market’teki bu hizmetler için EA parasal taahhüdü, satın alma ve ödeme yönetiminin kolaylaştırılmasına yardımcı olur.

### <a name="why-did-we-make-this-change"></a>Bu değişikliği neden yaptık?

Müşteriler, peşin parasal taahhüt ödemesini kullanabilecekleri farklı yollar arıyordu. Bu, müşteriler tarafından sıklıkla istenen bir değişiklikti ve Azure Market müşterilerinin büyük bir bölümünü etkiledi.

### <a name="how-do-you-benefit"></a>Nasıl faydalanabilirsiniz?

Daha basit bir faturalandırma deneyimine sahip olabilir ve EA parasal taahhüdünüzü daha iyi harcayabilirsiniz. Bu hizmetler peşin parasal taahhüt ödemenize dahil olduğundan EA parasal taahhüdünüz daha değerli hale gelir.

### <a name="what-azure-marketplace-services-use-ea-monetary-commitment-and-how-do-i-know"></a>EA parasal taahhüdünü kullanan Azure Market hizmetlerini nasıl tespit edebilirim?

Parasal taahhüt kullanan bir hizmet satın aldığınızda Azure Market'te bir sorumluluk reddi görüntülenir. Red Hat, SUSE, Autodesk ve Oracle tarafından yayımlanmış bazı hizmetler desteklenmektedir. Şu anda başkaları tarafından yayımlanan benzer ada sahip hizmetler parasal taahhüt kullanmamaktadır. Hizmetlerin tam listesini bu SSS sayfasının sonunda bulabilirsiniz.

### <a name="what-if-my-ea-monetary-commitment-runs-out"></a>EA parasal taahhüdüm biterse ne olacak?

Parasal taahhüdünüzü bitirip fazla kullanıma geçerseniz bu hizmetlerle ilgili ücretler, diğer tüketilen hizmetlerle birlikte bir sonraki fazla kullanım faturanıza yansıtılacaktır. 1 Mart 2018 tarihinden önce bu ücretler diğer Azure Market hizmetleriyle birlikte faturalandırılıyordu.

### <a name="why-dont-all-azure-marketplaces-consume-ea-monetary-commitment"></a>Neden tüm Azure Market hizmetleri EA parasal taahhüdünü kullanmıyor?

EA parasal taahhüdü ile ilgili en iyi müşteri deneyimi sunmak için düzenli güncelleştirmeler yapıyoruz. Bu değişiklik, çok sayıda müşteriyi ve Azure Market harcamalarının önemli bir miktarını etkileyecek. İleride farklı hizmetler de eklenebilir.

### <a name="how-does-this-impact-indirect-enrollment-and-partners"></a>Bu durum dolaylı kayıtları ve iş ortaklarını nasıl etkiliyor?

Dolaylı kayıt müşterileri veya iş ortakları bu durumdan etkilenmeyecektir. Bu hizmetler, diğer tüketim hizmetleriyle aynı iş ortağı kar payı özelliklerine sahiptir. Buradaki tek değişiklik, ücretlerin farklı bir faturada görünmesi ve ücretlerin ödemesinin müşterinin EA parasal taahhüdü ile yapılmasıdır.

### <a name="is-there-a-list-of-azure-marketplace-services-that-consume-ea-monetary-commitment"></a>EA parasal taahhüdünü kullanan Azure Market hizmetlerinin olduğu bir liste var mı?

Belirli Azure Market teklifleri, parasal taahhüt fonlarını kullanabilir. Bu programa katılan ürünlerin tam listesi için [parasal taahhüt kullanan üçüncü taraf hizmetler](https://azure.microsoft.com/updates/azure-marketplace-third-party-reseller-services-now-use-azure-monetary-commitment) sayfasına bakın.

## <a name="power-bi-reporting"></a>Power BI raporları

Power BI raporlarını EA doğrudan, iş ortağı ve fatura bilgilerini görüntüleme erişimine sahip olan dolaylı müşteriler kullanabilir.

### <a name="power-bi-pro"></a>Power BI Pro

Power BI Pro EA müşterileri tarafından kullanılabilir. Power BI Pro ile maliyet verilerinizi verimli bir şekilde yönetmenizi sağlayacak raporlar oluşturabilir ve paylaşabilirsiniz. Ayrıca ek işbirliği ve veri yenileme özelliklerinden de faydalanabilirsiniz. Power BI Pro, daha yüksek veri kapasitesi ve veri akışı sınırları sunar.

<!--We plan to add new cost management features for Azure Enterprise customers.

Current Power BI (free) users who use the Microsoft Azure Consumption Insights content pack can get a 60-day free trial of Power BI Pro. After the trial is over, you can continue using Power BI Pro by adding a license.

To sign up for the free Power BI Pro trial:

1. From the gear icon in Power BI, select **Manage personal storage**.
1. Select **Try Pro for free** on the right.

See [Power BI self-service sign up](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/#power-bi-pro-60-day-trial) for more information on the Power BI Pro free trial.

### Azure EA Power BI Pro trial terms

- **General purpose**: The extended Power BI Pro for the "Microsoft Azure Enterprise" content pack trial offer (the "Offer") is available to existing qualified users during the term of the Offer, to allow them to access certain insights related to their Azure consumption through the use of a specific Power BI content pack.
- **Eligibility**: Users under an Enterprise Agreement (EA) can participate in the Offer if they have a function related to their organization's Azure billing, service, or cost management.
- **Exclusions**:
  - Users already participating in the Extended Power BI Pro trial will continue to qualify under the pre-existing offer and can't enter into the Azure EA Power BI Pro trial offer.
  - Users participating in the Offer can only use Power BI Pro with the Microsoft Azure Enterprise content pack. Any other use of Power BI Pro is prohibited.
  - Term: The Offer began on June 1, 2017 and ended on May 31, 2018.  Acceptance can occur at any time during the 12-month period, though the offer will terminate on May 31, 2018 for all users regardless of when they accepted the Offer.
  -->

### <a name="to-access-microsoft-azure-consumption-insights"></a>Microsoft Azure Consumption Insights'a erişmek için:

1. [Microsoft Azure Consumption Insights](https://app.powerbi.com/getdata/services/azureconsumption?cpcode=MicrosoftAzureConsumptionInsights&amp;getDataForceConnect=true&amp;WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26)'a gidin.
1. **Şimdi Edinin**'i seçin.
1. Bir kayıt numarası ve ay sayısı girdikten sonra **İleri**'yi seçin.
1. Bağlanmak için API erişim anahtarınızı girin. Kaydınıza ait anahtarı [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26)'da bulabilirsiniz.
1. İçeri aktarma işlemini otomatik olarak başlatmak için **Oturum Aç**'ı seçin.
1. Bu işlem tamamlandığında gezinti bölmesinde yeni bir pano, rapor ve model görünür. İçeri aktarılan verilerinizi görüntülemek için panoyu seçin.

> [!TIP]
>
> - Kaydınız için API anahtarı oluşturmayı öğrenmek istiyorsanız lütfen [Enterprise Portal](https://ea.azure.com/?WT.mc_id=azurebg_email_Trans_33675_1378_Service_Notice_EA_Customer_Power_BI_EA_Content_Pack_Apr26)'daki API Raporları yardım dosyasını inceleyin.
> - Power BI'ı Azure tüketiminize bağlama hakkında daha fazla bilgi için bkz. [Microsoft Azure Consumption Insights](/power-bi/desktop-connect-azure-cost-management).

### <a name="to-access-the-legacy-power-bi-ea-content-pack"></a>Eski Power BI EA içerik paketine erişmek için:

1. [Power BI web sitesine](https://app.powerbi.com/getdata/services/azure-enterprise) gidin.
1. Geçerli bir iş veya okul hesabıyla oturum açın.

   Bu iş veya okul hesabı, Azure Enterprise Portal üzerinden kayda erişmek için kullandığınız hesapla aynı veya ondan farklı olabilir.
1. Hizmet panosunda **Microsoft Azure Kurumsal**'ı seçin ve **Bağlan**'ı seçin.
1. **Azure Enterprise'a Bağlan** ekranındaki alanları doldurun:
    - Azure Ortamı URL'si: [https://ea.azure.com](https://ea.azure.com/)
    - Ay Sayısı: 1 ile 36 arasında
    - Kayıt Numarası: kayıt numaranızı girin
1. **İleri**’yi seçin.
1. **Kimlik Doğrulama Anahtarı** kutusuna API anahtarını girin.

    Buraya girmeniz gereken API anahtarını Azure Enterprise Portal'ın **Kullanımı İndir** sekmesinden alabilirsiniz. **API Erişim Anahtarı**'nı seçin ve anahtarı **Hesap Anahtarı** kutusuna yapıştırın.
1. Veri kümelerinin boyutuna bağlı olarak verilerin Power BI'a yüklenmesi 5-30 dakika arasında sürebilir.

## <a name="reports-faq"></a>Raporlar Hakkında SSS

Bu bölümde raporlarla ilgili sık sorulan soruların yanıtları verilmiştir.

### <a name="why-is-my-cost-showing-as-0"></a>Maliyetlerim neden $0 görünüyor?

**Doğrudan kayıt anlaşması** müşterileri için kuruluş yöneticileri, hesap sahiplerine ve bölüm yöneticilerine kullanım raporlarındaki maliyet/fiyatlandırma bilgilerine erişim sağlayabilir. Şu adımları uygulayın:

1. Azure Enterprise Portal'ın sol tarafındaki gezinti menüsünden **Yönet**'i seçin.
1. DA (bölüm yöneticisi) ücretleri görüntüle öğesinin yanındaki mavi kalem simgesini seçin.
1. **Etkin**'i seçin ve kaydedin.
1. AO (hesap sahibi) ücretleri görüntüle öğesinin yanındaki mavi kalem simgesini seçin.
1. **Etkin**'i seçin ve kaydedin.

> [!NOTE]
> Hesap sahibi veya bölümü yöneticisiyseniz lütfen kuruluş yöneticinizle iletişime geçerek fiyatlandırma özelliğini etkinleştirmesini isteyin.

**Dolaylı kayıt** müşterileri, fiyatlandırma özelliğinin etkin olup olmadığını kontrol etmek için iş ortaklarıyla iletişime geçebilir. Bu işlem yalnızca iş ortağı tarafından gerçekleştirilebilir. Bu özellik etkinleştirildikten sonra kuruluş yöneticisi olarak kaydınızdaki ücretleri ve fiyatları görüntüleyebilirsiniz.

Bir hesap sahibi veya bölüm yöneticisi için ücretleri görüntüleme özelliğini etkinleştirmek isteyen iş ortaklarının **doğrudan kayıt anlaşması** bölümündeki adımları izlemesi gerekir.

### <a name="why-is-there-no-sku-information-on-my-usage-detail-report"></a>Kullanım ayrıntıları raporunda neden SKU bilgileri yok?

Kullanım ayrıntıları raporunda SKU bilgileri bulunmaz. Ancak rapordaki kullanım bilgileri sayesinde fiyat sayfası raporunu indirip SKU bilgilerine ulaşabilirsiniz.

### <a name="why-doesnt-the-total-amount-on-azure-marketplace-match-the-reports-for-usage-summary-and-detail"></a>Kullanımla ilgili özet ve ayrıntılı raporlarda Azure Market toplam tutarı neden farklı?

Azure Market ücretleri raporu, yalnızca kullanıma dayalı ücretleri gösterir. Tek seferlik ücretler gösterilmez. Kullanıma dayalı ve tek seferlik ücretlerle ilgili en güncel kullanımlar için kullanım özeti sayfasını inceleyin.

### <a name="why-is-there-no-information-on-my-api-report"></a>API raporumda neden hiç bilgi yok?

API anahtarları altı ay boyunca geçerlidir. Sorun yaşıyorsanız kuruluş yöneticisinin yeni bir API anahtarı oluşturması gerekir. API Raporları Hakkında SSS sayfasındaki adımları izlemeyi unutmayın.

### <a name="why-isnt-my-power-bi-report-working"></a>Power BI raporum neden çalışmıyor?

Power BI ile ilgili sorunlar için [Power BI destek ekibine](https://support.powerbi.com) bilet gönderin.

### <a name="why-dont-my-resource-tags-show-on-my-reports"></a>Kaynak etiketlerim neden raporlarımda görünmüyor?

Kaynak etiketleri Azure portalında yönetilir. [Azure portalından](https://portal.azure.com) Azure abonelik ekibiyle iletişime geçebilirsiniz. [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) makalesindeki adımları izleyin.

### <a name="why-does-my-resource-rate-change-every-day"></a>Kaynak ücretim neden her gün değişiyor?

Ayrıntılı kullanım raporunda gösterilen kaynak ücreti, hesaplanmış bir değerdir. Hizmet için ücretlendirilen ortalama aylık ücreti temsil eder. Kaynak ücreti, bir hizmet birimine ait aylık ortalama taahhüdünüz ve aylık fazla kullanım ücretleriniz kullanılarak hesaplanır. Taahüdünüzden tahsil edilen kullanım miktarı ve fazla kullanım ücretleri, ay sonuna kadar değişecektir. Bu nedenle listelenen ücret de ay içinde değişir. Kaynak ücreti, ay sonundan itibaren beşinci günde sabitlenir.

### <a name="glossary-of-processes-for-calculating-the-resource-rate"></a>Kaynak ücretini hesaplamak için kullanılan işlemlerle ilgili sözlük

- **Toplam Ham Birim Sayısı:** Ayrıntılı kullanım raporundaki tüketilen miktar değeridir.
- **Birim Başına MOCP Kaynağı:** Yukarı akış kullanım sistemi, her hizmet için kullanımları farklı birimlerde yayar. (Önceden ayarlanmış)
- **Birim Başına Tüketim:** Azure Enterprise ölçü birimidir. (Önceden ayarlanmış)
- **Fiyat:** Azure Enterprise Portal'daki birim fiyattır.
- **Toplam Maliyet:** Azure Enterprise Portal'dan alınan ayrıntılı kullanım raporu veya taahhüt kullanımına fazla kullanımın eklenmesi ile bulunan genişletilmiş maliyettir.

### <a name="charges-calculations"></a>Ücretlerin hesaplanması

- **Tüketilen MOCP kaynağına dönüştürme** = `ROUND(Total RAW Units * MOCP Resource Per Unit,4)`
- **Tüketilen birim sayısına dönüştürme** = `Consumed MOCP Resources / Consumption per Unit`
- **Toplam maliyeti hesaplama** = `Consumed Units * Price`

### <a name="logic-in-the-usage-calculation-logic"></a>Kullanım Hesaplama Mantığı

**Kaynak Ücreti** = `Total Cost /(Total RAW Units / MOCP Resource Per Unit)`

Kaynak ücreti, tahsil edilen ücretlere göre hesaplanır. Fiyat listesindeki gerçek birim fiyat ile aynı olmayabilir.

İndirilen kullanım verileri raporunda ham kullanım verileri için altı ondalık basamak bulunur. Bu veriler, fazla kullanım ücretlerinin hesaplanması için kullanılır. Ancak Azure Enterprise Portal'da gösterilen veriler, kullanımı taahhüt birimleri için dört ondalık basamağa yuvarlar, fazla kullanım birimlerinde de tüm ondalık basamakları siler. Azure Enterprise Portal'da tüm fazla kullanım yalnızca tam birimler üzerinden ücretlendirilir. Birim fiyat ile fazla kullanım veya karışık aylar içinde ücretlendirilen kullanıma ait kaynak ücreti arasında büyük bir fark görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Aşağıdaki Excel dosyaları, Azure hizmetleri hakkında ayrıntılı bilgiler içerir ve her ayın 6. ve 20. gününde güncelleştirilir:

   | Başlık | Açıklama | Dosya adı |
   | --- | --- | --- |
   | [Hizmet Kolay Adları](https://azurepricing.blob.core.windows.net/supplemental/Friendly_Service_Names.xlsx) | Tüm etkin hizmetlerin listesidir ve şunları içerir: <br>  <ul><li>hizmet kategorisi</li>   <li>hizmet kolay adı</li>   <li>taahhüt adı ve parça numarası</li> <li>tüketim adı ve parça numarası</li>   <li>ölçü birimi</li>   <li>bildirilen kullanım ile görüntülenen Enterprise Portal kullanımı arasındaki dönüştürme faktörleri</li></ul> | Hizmet\_Kolay\_Adları.xlsx |
   | [Hizmet İndirme Alanları](https://azurepricing.blob.core.windows.net/supplemental/Service_Download_Fields.xlsx) | Bu elektronik tabloda Kullanım İndirme Raporundaki hizmetle ilgili alanların tüm olası birleşimlerinin bir listesi bulunur. | Hizmet\_İndirme\_Alanları.xlsx |

- Faturanızı ve ücretlerinizi anlama konusunda bilgi için bkz. [Azure Kurumsal Anlaşma faturanızı anlama](../understand/review-enterprise-agreement-bill.md).
- Azure Enterprise Portal'ı kullanmaya başlamak için bkz. [Azure EA portalını kullanmaya başlama](ea-portal-get-started.md).
