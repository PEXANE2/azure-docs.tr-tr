---
title: İş Ortağı Merkezi'nde ticari pazar analitiği için sık sorulan sorular ve terminoloji
description: Ticari pazar yeri analitiği hakkında sık sorulan soruları nasıl ele alabildiğini öğrenin. Analitik terminolojisi için bir veri sözlüğü içerir.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: b7367e58de818c20723c02a6763b1bf1e3b18f24
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81251835"
---
# <a name="frequently-asked-questions-and-terminology-for-commercial-marketplace-analytics"></a>Ticari pazar analizi için sık sorulan sorular ve terminoloji

Bu makalede, İş Ortağı Merkezi'nde analitik iletilerle ilgili sık sorulan sorular ele aldaki ve ayrıca analitik terminoloji sözlüğü de verilmektedir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu bölümde, İş Ortağı Merkezi'nde **Bulunan Analitik Yok henüz** iletilerle ilgili sık sorulan soruların yanıtları verilmektedir.

**İş Ortağı Merkezi'nde analiz verilerimi görüntüleyemiyorum. Bu sayfalara erişirken aşağıdaki iletiyi görürsünüz. Bu neden?**

![Tekliflerinizle ilgili henüz veri yok](./media/analytics-faq-no-data.png)

Bu iletiyi neden alasınız:

- Şu anda pazarda yayınlanan teklifler için hiçbir satın alma bulunmamaktadır. Bu, tekliflerinizin pazarda canlı olduğu ve ürün görüntüleme sayfalarındamüşterilerden görünüm ler kazandığı anlamına gelebilir, ancak müşteriler bunları satın almak ve dağıtmak için henüz harekete geçmemiş olabilir.
- Teklifinizin yayınlanması sürecinde olabilir ve henüz yayınlanmadı. Sadece canlı teklifler müşteriler tarafından elde edilebilir. Tekliflerinizin durumunu kontrol etmek için [Analiz panosunda](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)genel bakışa bakın. Daha fazla bilgi için [ticari pazar analizinde Özet Panosu'na](./summary-dashboard.md)bakın.
- Teklifleriniz, yalnızca liste teklifleri olan ve pazardaki müşteriler tarafından satın alınamayan **Bana Ulaşın**olarak listelenebilir. Bu teklifler müşteri adayları oluştursa ve sizinle paylaşılsa da, satın alınamadıkları için bu teklifler için siparişler oluşturulmaz. Teklif giriş türünü kontrol etmek için kurulum sayfasına gidin.

**Analitik verilerim olduğunu biliyorum, ancak aşağıdaki ileti görünüyor:**

![Verilen tarih aralığı için veri yok](./media/analytics-faq-data-range.png)

Bu iletiyi alıyorsanız, bu analiz verilerine sahip olduğunuz, ancak seçtiğiniz tarih aralığı için veri olmadığı anlamına gelir. 2010 yılından bu yana herhangi bir veriyi görüntülemek için farklı bir tarih aralığı veya özel tarih aralığı seçin. Daha fazla bilgi için, [ticari pazar analizinde Özet Panosu'nun](./summary-dashboard.md)Tarih Aralığı bölümüne bakın.

## <a name="dictionary-of-data-terms"></a>Veri terimleri sözlüğü

| Öznitelik Adı | Raporlar | Tanım|
|---|---|---|
| Azure Lisans Türü | Müşteri, Sipariş | Müşteriler tarafından Azure satın almak için kullanılan lisans sözleşmesi türü. Kanal olarak da bilinir |
| Azure Lisans Türü: Bulut Çözüm Sağlayıcısı | Müşteri, Sipariş | Son müşteri, Azure ve Market teklifinizi, satıcınız olarak görev yapan Bulut Çözüm Sağlayıcısı aracılığıyla satın alıyor.|
| Azure Lisans Türü: Kurumsal | Müşteri, Sipariş | Son müşteri, Azure ve Market teklifinizi doğrudan Microsoft ile imzalanan bir Kurumsal Sözleşme aracılığıyla satın adatır.|
| Azure Lisans Türü: Satıcı ile Kurumsal  | Müşteri, Sipariş | Son müşteri, Azure ve Market teklifinizi Microsoft ile Kurumsal Anlaşmalarını kolaylaştıran bir satıcı aracılığıyla satın alıyor.|  |
| Azure Lisans Türü: Gittiğiniz Kadar Öde| Müşteri, Sipariş | Son müşteri, Azure ve Market teklifinizi doğrudan Microsoft ile imzalanan "You Go Olarak Öde" anlaşması yla satın alıyor.||
| Bulut Örneği Adı| Sipariş verme| VM dağıtımının gerçekleştiği Microsoft Bulutu.||
| Bulut Örneği Adı: Azure Global| Sipariş verme| Genel genel Microsoft bulutu.|| |
| Bulut Örneği Adı: Azure Kamu | Sipariş verme| Aşağıdaki hükümetlerden biri için hükümete özel Microsoft bulutları: Çin, Almanya veya Amerika Birleşik Devletleri.| |
| Müşteri Şehri| Müşteri| Müşteri tarafından sağlanan şehir adı. Bir müşterinin Azure Aboneliği'nde şehir şehirden farklı olabilir.||
| Müşteri İletişim Dili  | Müşteri| İletişim için müşteri tarafından tercih edilen dil.||
| Müşteri Şirket Adı | Müşteri, Sipariş | Müşteri tarafından sağlanan şirket adı. Bir müşterinin Azure Aboneliği'nde ad şehirden farklı olabilir.|  |
| Müşteri Ülkesi | Müşteri, Sipariş | Müşteri tarafından sağlanan ülke adı. Ülke, bir müşterinin Azure Aboneliği'nde ülkeden farklı olabilir.|  |
| Müşteri E-postası| Müşteri| Son müşteri tarafından sağlanan e-posta adresi. E-posta, müşterinin Azure Aboneliği'ndeki e-posta adresinden farklı olabilir.||
| Müşteri Adı| Müşteri| Müşteri tarafından sağlanan ad. Ad, müşterinin Azure Aboneliği'nde sağlanan addan farklı olabilir.| |
| Müşteri Kimliği | Müşteri, Sipariş | Bir müşteriye atanan benzersiz tanımlayıcı. Bir müşterinin sıfır veya daha fazla Azure Market Aboneliği olabilir.|  |
| Müşteri Posta Kodu  | Müşteri| Müşteri tarafından sağlanan posta kodu. Kod, müşterinin Azure Aboneliği'nde sağlanan posta kodundan farklı olabilir.| |
| Müşteri Durumu| Müşteri| Müşteri tarafından sağlanan durum (adres). Durum, müşterinin Azure Aboneliği'nde sağlanan durumdan farklı olabilir.| |
| Edinilen Tarih| Müşteri| Müşterinin sizin yayınladığınız herhangi bir teklifi satın aldığı ilk tarih.| |
| Kayıp Tarih| Müşteri| Müşterinin daha önce satın alınan tüm tekliflerin sonuncusunu iptal ettiği son tarih.||
| Yeni Müşteri  | Sipariş verme| Değer, tekliflerinizden birini veya birkaçını ilk kez (veya değil) satın alan yeni bir müşteriyi tanımlar. "Edinilen Tarih" için aynı takvim ayı içinde "Evet" değeri olacaktır. Müşteri, bildirilen takvim ayından önce tekliflerinizi satın almışsa, değer "Hayır" olacaktır. |
| Önizleme SKU mı| Sipariş verme| SKU'yu "önizleme" olarak etiketlediyseniz, değer size haber verecektir. SKU buna göre etiketlenmişse değeri "Evet" olur ve yalnızca sizin yetkiverilen Azure abonelikleri bu resmi dağıtabilir ve kullanabilir. SKU "önizleme" olarak tanımlanmamışsa değeri "Hayır" olacaktır.  |
| Promosyon İletişim Opt In mı| Müşteri| Müşteri proaktif olarak yayıncılardan promosyon iletişimini tercih ederse, bu değer size haber verecektir. Şu anda, biz müşterilere seçeneği sunmuyor, bu yüzden yönetim kurulu genelinde "Hayır" belirttik. Bu özellik dağıtıldıktan sonra, buna göre güncellemeye başlayacağız.|
| Pazar Yeri Lisans Türü| Sipariş verme| Market teklifinin faturalandırma yöntemi.||
| Market Lisans Türü: Azure Üzerinden Faturalandırıldı| Sipariş verme| Microsoft, bu Market teklifi için aracınızdır ve sizin adınıza müşterilere fatura eder. (Ya PAYG Kredi Kartı veya Kurumsal Fatura)||
| Pazar Yeri Lisans Türü: Kendi Lisansınızı Getir | Sipariş verme| VM dağıtmak için müşteri tarafından sağlanan bir lisans anahtarı gerektirir. Microsoft, müşterilerine tekliflerini pazarda bu şekilde listelemeleri için faturalandırmaz.||
| Pazar Yeri Lisans Türü: Ücretsiz| Sipariş verme| Teklif tüm kullanıcılara ücretsiz olarak yapılandırılmıştır. Microsoft, bu teklifi kullanmaları için müşterilere fatura landırmaz.||
| Market Lisans Türü: Microsoft Bayi olarak  | Sipariş verme| Microsoft, bu Market teklifi için satıcınızdır.|  |
| Pazar Yeri Abonelik Kimliği | Müşteri, Sipariş | Müşterinin Market teklifinizi satın almak için kullandığı Azure Aboneliği ile ilişkili benzersiz tanımlayıcı. Kimlik eskiden Azure Abonelik GUID'di.||
| Teklif Adı  | Sipariş verme| Pazar sunan adı.|| |
| Teklif Türü  | Sipariş verme| Microsoft Marketplace teklifinin türü.|||
| Teklif Türü: Yönetilen Uygulama  | Sipariş | Aşağıdaki koşullar gerektiğinde Azure uygulamasını kullanın: yönetilen uygulama teklifi türü: Müşteriniz için vm veya IaaS tabanlı bir çözümün tamamını kullanarak abonelik tabanlı bir çözüm dağıtırsınız. Siz veya müşteriniz çözümün bir iş ortağı tarafından yönetilmesini gerektirir. |
| Teklif Türü: Azure Uygulaması| Sipariş | Çözümünüz basit bir VM'nin ötesinde ek dağıtım ve yapılandırma otomasyonu gerektirdiğinde Azure Uygulama çözüm şablonu teklif türünü kullanın.||
| Teklif Türü: Danışmanlık Hizmeti| Sipariş verme| Azure Marketi'ndeki Danışmanlık Hizmetleri, müşterilerin Azure kullanımını desteklemek ve genişletmek için hizmetlerle bağlantı kurmaya yardımcı olur.| |
| Teklif Türü: Konteyner | Sipariş verme| Çözümünüz Kubernetes tabanlı Azure kapsayıcı hizmeti olarak sağlanan docker kapsayıcı görüntüsü olduğunda Konteyner teklif türünü kullanın.||
| Teklif Türü: Dynamics 365 Business Central| Sipariş verme| Çözümünüz Finans ve Operasyonlar için Dynamics 365 ile entegre edildiğinde bu teklif türünü kullanın| |
| Teklif Türü: Müşteri Katılımı için Dynamics 365 | Sipariş verme| Çözümünüz Müşteri Katılımı için Dynamics 365 ile entegre edildiğinde bu teklif türünü kullanın.||
| Teklif Türü: IoT Edge Modülü | Sipariş verme| Azure IoT Edge modülleri, IoT Edge tarafından yönetilen en küçük hesaplama birimleridir ve Microsoft hizmetlerini (Azure Akış Analizi, üçüncü taraf hizmetleri veya çözüme özel kodunuzu gibi) içerebilir. |
| Teklif Türü: Power BI Uygulaması | Sipariş verme| Power BI ile tümleşik bir uygulama dağıtırken Power BI Application teklif türünü kullanın.|  |
| Teklif Türü: SaaS Uygulaması| Sipariş verme| Müşterilerinizin SaaS tabanlı, teknik çözümünüzü abonelik olarak satın alabilmesi için SaaS uygulama teklif türünü kullanın.||
| Teklif Türü: Sanal Makine | Sipariş verme| Müşterinizle ilişkili aboneye sanal bir cihaz dağıttığınızda sanal makine teklif türünü kullanın.||
| Teklif Türü: Visual Studio Marketplace Uzantısı  | Sipariş verme| Azure DevOps uzantı geliştiricileri için daha önce kullanılabilen teklif türü. Azure DevOps uzantısı geliştiricileri uzantılarını doğrudan müşterilere satabilir. Uzatma teklifleri ücretli olarak veya deneme sürümü de dahil olmak üzere yapılandırılabilir. |
| Sipariş İptal Tarihi| Sipariş verme| Market siparişinin iptal edildiği tarih.||
| Sipariş kimliği| Sipariş verme| Market hizmetiniz için müşteri siparişinin benzersiz tanımlayıcısı. Sanal Makine kullanım tabanlı teklifler bir siparişle ilişkili değildir.| |
| Sipariş Satın Alma Tarihi| Sipariş verme| Market Siparişi'nin oluşturulduğu tarih.|||
| Sipariş Durumu| Sipariş verme| Verilerin en son yenilendiği tarihteki Bir Market siparişinin durumu.|     |
| Sipariş Durumu: Etkin  | Sipariş verme| Müşteri bir sipariş satın aldı ve siparişini iptal etmedi.|         |
| Sipariş Durumu: iptal edildi | Sipariş verme| Müşteri daha önce bir sipariş satın almış ve daha sonra siparişini iptal etmiş.||
| Sağlayıcı E-posta| Müşteri| Microsoft ve son müşteri arasındaki ilişkiye dahil olan sağlayıcının e-posta adresi. Müşteri Bayi aracılığıyla bir Kuruluşsa, bu satıcı olacaktır. Bir Bulut Çözüm Sağlayıcısı (CSP) söz konusuysa, bu CSP olacaktır.|
| Sağlayıcı Adı| Müşteri| Microsoft ve son müşteri arasındaki ilişkide yer alan sağlayıcının adı. Müşteri Bayi aracılığıyla bir Kuruluşsa, bu satıcı olacaktır. Bir Bulut Çözüm Sağlayıcısı (CSP) söz konusuysa, bu CSP olacaktır.|
| SKU| Sipariş verme| Yayımlama sırasında tanımlanan SKU adı. Bir teklifin birçok SKU'su olabilir, ancak SKU yalnızca tek bir teklifle ilişkilendirilebilir.||
| Deneme Bitiş Tarihi| Sipariş verme| Bu siparişin deneme süresinin sona ereceği veya sona ereceği tarih.||

## <a name="next-steps"></a>Sonraki adımlar

- İş Ortağı Merkezi ticari pazarda bulunan analiz raporlarına genel bir bakış için, [İş Ortağı Merkezi'ndeki ticari pazar için Analytics'e](./analytics.md)bakın.
- Teklifiniz için pazar daki etkinliği özetleyen toplam verilerin grafikleri, eğilimleri ve değerleri [için, ticari pazar analizinde Özet Pano'ya](./summary-dashboard.md)bakın.
- Siparişleriniz hakkında grafiksel ve indirilebilir bir biçimde bilgi için, [ticari pazar analizinde Siparişler Panosu'na](./orders-dashboard.md)bakın.
- Sanal Makine (VM) kullanım ve ölçülü fatura ölçümleri için, [ticari pazar analizinde Kullanım Panosu'na](./usage-dashboard.md)bakın.
- Büyüme eğilimleri de dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için [ticari pazar analizinde Müşteri Panosu'na](./customer-dashboard.md)bakın.
- Son 30 gün içinde indirme isteklerinizin bir listesi için, [ticari pazar analizinde İndirme Panosu'na](./downloads-dashboard.md)bakın.
- Azure Marketi ve AppSource'daki teklifler için müşteri geri bildirimlerinin konsolide görünümünü görmek için, [ticari pazar analizinde Derecelendirmeler ve incelemeler panosuna](./ratings-reviews.md)bakın.
