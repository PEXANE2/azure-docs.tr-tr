---
title: Iş Ortağı Merkezi 'nde ticari Market analizi için sık sorulan sorular ve terminoloji
description: Ticari Market Analytics hakkında sık sorulan soruların nasıl ele alınacağını öğrenin. Analiz terimleri için bir veri sözlüğü içerir.
author: ChJenk
manager: evansma
ms.author: v-chjen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 559c0e2a56bfb452f1faf10f68c19d4f406427e8
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75480560"
---
# <a name="frequently-asked-questions-and-terminology-for-commercial-marketplace-analytics"></a>Ticari Market Analytics hakkında sık sorulan sorular ve terminoloji

Bu makalede, Iş Ortağı Merkezi 'nde analiz iletileri hakkında sıkça sorulan sorular ele alınmaktadır ve ayrıca analiz terminolojisi sözlüğü de sağlanmaktadır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Bu bölümde, Iş Ortağı Merkezi 'nde **henüz analiz yok** iletisi bulunmayan sorular hakkında sık sorulan sorulara yanıtlar verilmektedir.

**Iş Ortağı Merkezi 'nde Analytics verilerimi görüntüleyemiyorum. Bu sayfalara erişirken aşağıdaki iletiyi görüyorum. Bunun nedeni nedir?**

![Henüz tekliflerinizi için veri yok](./media/analytics-faq-no-data.png)

Bu iletiyi neden alabilirsiniz:

- Market 'te listelenen tekliflerinizi SaaS teklifleri değildir. Şu anda Iş ortağı merkezi yalnızca SaaS teklifleri için analiz raporlar. Dynamics 365 teklifleri iş ortağı merkezi 'nde yayımlanabilir, ancak bu tekliflere ait analizler henüz iş ortağı merkezi 'nde etkin değildir.
- Market 'teki yayımlanmış tekliflerinizin Şu anda bir alım yok. Bu, tekliflerinizin Market 'te canlı olması ve ürün görüntüleme sayfalarındaki müşterilerden görünüm elde etmesinin yanı sıra müşterilerin satın alma ve dağıtma eylemi gerçekleştirmemiş olması anlamına gelir.
- Teklifinizin yayımlanması sürecinde olabilir ve henüz canlı değildir. Müşteriler tarafından yalnızca canlı tekliflerle elde edilebilir. Tekliflerinizin durumunu denetlemek için bkz. [analiz panosunda](https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary)genel bakış. Daha fazla bilgi için bkz. [ticari Market Analytics 'Teki Özet Panosu](./summary-dashboard.md).
- Tekliflerinizin yalnızca liste teklifleri olan ve Market 'teki müşteriler tarafından satın alınamamakta olan **benimle Iletişim kurmak**üzere listelenmiş olabilir. Bu teklif, müşteri adayları oluşturur ve sizinle paylaşılır, ancak satın alınamazlar, bu teklifler için Siparişler oluşturulmaz. Teklif listesi türünü denetlemek için kurulum sayfasına gidin.

**Analytics verilerine sahip olduğumu biliyorum, ancak aşağıdaki ileti görünür:**

![Belirtilen tarih aralığı için veri yok](./media/analytics-faq-data-range.png)

Bu iletiyi alıyorsanız, analiz verileriniz vardır ancak seçtiğiniz tarih aralığı için veri yok demektir. 2010 sonrasındaki verileri görüntülemek için farklı bir tarih aralığı veya özel tarih aralığı seçin. Daha fazla bilgi için, [ticari Market Analytics 'Teki Özet panosunun](./summary-dashboard.md)tarih aralığı bölümüne bakın.

## <a name="dictionary-of-data-terms"></a>Veri koşullarının sözlüğü

| Öznitelik adı | Raporlar | Tanım|
|---|---|---|
| Azure lisans türü | Müşteri, sipariş | Müşteriler tarafından Azure satın alma için kullanılan lisans sözleşmesinin türü. Kanal olarak da bilinen |
| Azure lisans türü: bulut çözümü sağlayıcısı | Müşteri, sipariş | Azure ve Market procures son müşterisi, bulut çözümü sağlayıcısı aracılığıyla satıcıya göre davranan bir tekliftir.|
| Azure lisans türü: Kurumsal | Müşteri, sipariş | Azure procures son müşterisi ve Market, doğrudan Microsoft ile imzalanmış bir Kurumsal Anlaşma aracılığıyla teklif.|
| Azure lisans türü: satıcı aracılığıyla kurumsal  | Müşteri, sipariş | Azure ve Market procures son müşterisi, Microsoft ile Kurumsal Anlaşma kolaylaştıran bir satıcı aracılığıyla teklif sunuyor.|  |
| Azure lisans türü: Kullandıkça öde| Müşteri, sipariş | Azure procures son müşterisi ve Market, doğrudan Microsoft ile imzalanmış bir "Kullandıkça öde" sözleşmesi aracılığıyla teklif sunuyor.||
| Bulut örneği adı| Sipariş| Bir VM dağıtımının gerçekleştiği Microsoft Bulut.||
| Bulut örneği adı: Azure genel| Sipariş| Genel genel Microsoft bulutu.|| |
| Bulut örneği adı: Azure Kamu | Sipariş| Aşağıdaki hükümetlerden biri için kamu 'ya özgü Microsoft bulutları: Çin, Almanya veya Amerika Birleşik Devletler.| |
| Müşteri şehri| Müşteri| Müşteri tarafından sunulan şehir adı. Şehir, müşterinin Azure aboneliğindeki şehirden farklı olabilir.||
| Müşteri Iletişim dili  | Müşteri| İletişim için müşterinin tercih ettiği dil.||
| Müşteri şirket adı | Müşteri, sipariş | Müşterinin sunduğu şirket adı. Ad, müşterinin Azure aboneliğindeki şehirden farklı olabilir.|  |
| Müşteri ülkesi | Müşteri, sipariş | Müşterinin sunduğu ülke adı. Ülke, müşterinin Azure aboneliğindeki ülkeden farklı olabilir.|  |
| Müşteri e-postası| Müşteri| Son müşteri tarafından sunulan e-posta adresi. E-posta, müşterinin Azure aboneliğindeki e-posta adresinden farklı olabilir.||
| Müşteri adı| Müşteri| Müşterinin verdiği ad. Ad, müşterinin Azure aboneliğinde belirtilen ad ile farklı olabilir.| |
| Müşteri Kimliği | Müşteri, sipariş | Müşteriye atanan benzersiz tanımlayıcı. Müşterinin sıfır veya daha fazla Azure Market aboneliği olabilir.|  |
| Müşteri posta kodu  | Müşteri| Müşteri tarafından sunulan posta kodu. Kod, müşterinin Azure aboneliğinde girilen posta kodundan farklı olabilir.| |
| Müşteri durumu| Müşteri| Müşteri tarafından sunulan eyalet (adres). Durum, müşterinin Azure aboneliğinde belirtilen durumdan farklı olabilir.| |
| Alınma tarihi| Müşteri| Müşterinin sizin tarafınızdan yayınlanan teklifi satın aldığı ilk tarih.| |
| Kayıp tarihi| Müşteri| Müşterinin daha önce satın alınan tüm tekliflerden son iptal ettiği son tarih.||
| Yeni müşteri  | Sipariş| Bu değer, yeni bir müşteriyi, bir veya daha fazla teklifinizden ilk kez (veya değil) edinirsiniz. "Tarihi alındı" için aynı takvim ayı içinde değer "Evet" olacaktır. Müşteri tekliflerinizin herhangi birini bildirilen takvim ayına göre satın alıyorsa değer "Hayır" olacaktır. |
| Önizleme SKU 'SU| Sipariş| Bu değer, SKU 'YU "Önizleme" olarak etiketlediyseniz size bilgi verir. SKU buna uygun şekilde etiketlenmişse değer "Evet" olur ve yalnızca sizin tarafından yetkilendirilen Azure abonelikleri bu görüntüyü dağıtabilir ve kullanabilir. SKU "Önizleme" olarak tanımlanmamışsa, değer "Hayır" olacaktır.  |
| Promosyon ekibine katılım kabul ediyor| Müşteri| Bu değer, müşterinin yayımcılardan promosyon kişisini önceden kabul eder olduğunu bilmenizi sağlar. Şu anda, bu seçeneği müşterilere sunuyoruz, bu nedenle Pano genelinde "Hayır" belirtiyoruz. Bu özellik dağıtıldıktan sonra, güncelleştirmeye uygun olarak başlayacağız.|
| Market lisans türü| Sipariş| Market teklifinin faturalandırma yöntemi.||
| Market lisans türü: Azure üzerinden faturalandırılır| Sipariş| Microsoft bu Market 'e yönelik aracısıdır ve sizin adınıza müşteri reçeteleri sunun. (PAYG kredi kartı ya da kurumsal fatura)||
| Market lisans türü: kendi lisansınızı getirin | Sipariş| VM, dağıtım için müşterinin sunduğu bir lisans anahtarı gerektirir. Microsoft, tekliflerini Market aracılığıyla bu şekilde listelemek için müşterileri faturalandırmaz.||
| Market lisans türü: ücretsiz| Sipariş| Teklif, tüm kullanıcılar için ücretsiz olacak şekilde yapılandırılmıştır. Microsoft, müşterileri bu teklifin kullanımıyla ilgili olarak faturalandırmaz.||
| Market lisans türü: satıcı olarak Microsoft  | Sipariş| Microsoft bu Market teklifinin satıcısına yöneliktir.|  |
| Market abonelik KIMLIĞI | Müşteri, sipariş | Market teklifinizi satın almak için kullanılan müşterinin Azure aboneliğiyle ilişkili benzersiz tanımlayıcı. KIMLIĞI daha önce Azure aboneliği GUID 'iydi.||
| Teklif Adı  | Sipariş| Market sunumunun adı.|| |
| Teklif Türü  | Sipariş| Microsoft Market teklif türü.|||
| Teklif türü: yönetilen uygulama  | Siparişi | Aşağıdaki koşullar gerektiğinde Azure uygulaması: yönetilen uygulama teklifi türünü kullanın: bir VM veya tüm IaaS tabanlı bir çözümü kullanarak müşteriniz için abonelik tabanlı bir çözüm dağıtırsınız. Siz veya müşteriniz, çözümün bir iş ortağı tarafından yönetilmesini gerektirir. |
| Teklif türü: Azure uygulaması| Siparişi | Çözümünüz basit bir sanal makinenin ötesinde ek dağıtım ve yapılandırma Otomasyonu gerektirdiğinde Azure Uygulama çözümü şablonu teklif türünü kullanın.||
| Teklif türü: danışmanlık hizmeti| Sipariş| Azure Marketi 'ndeki Danışmanlık Hizmetleri, Azure 'un kullanımını desteklemek ve genişletmek için müşterilerin hizmetleriyle bağlantı sağlanmasına yardımcı olur.| |
| Teklif türü: kapsayıcı | Sipariş| Çözümünüz, Kubernetes tabanlı bir Azure Container Service olarak sağlanan bir Docker kapsayıcı görüntüsü olduğunda kapsayıcı teklif türünü kullanın.||
| Teklif türü: Dynamics 365 Iş Merkezi| Sipariş| Çözümünüz finans ve Işlemler için Dynamics 365 ile tümleştirildiğinde bu teklif türünü kullanın| |
| Teklif türü: müşteri katılımı için Dynamics 365 | Sipariş| Çözümünüz müşteri katılımı için Dynamics 365 ile tümleştirildiğinde bu teklif türünü kullanın.||
| Teklif türü: IoT Edge modülü | Sipariş| Azure IoT Edge modüller, IoT Edge tarafından yönetilen en düşük hesaplama birimleridir ve Microsoft hizmetlerini (Azure Stream Analytics), 3. taraf hizmetleri veya kendi çözümüne özgü kodunuzu içerebilir. |
| Teklif türü: Power BI uygulama | Sipariş| Power BI ile tümleştirilmiş bir uygulama dağıtırken Power BI uygulama teklifi türünü kullanın.|  |
| Teklif türü: SaaS uygulaması| Sipariş| Müşterinizin SaaS tabanlı, teknik çözümünüzü bir abonelik olarak satın almasını sağlamak için SaaS uygulaması teklif türünü kullanın.||
| Teklif türü: sanal makine | Sipariş| Müşterinizin ilişkili aboneliğine bir Sanal Gereç dağıtırken sanal makine teklif türünü kullanın.||
| Teklif türü: Visual Studio Market uzantısı  | Sipariş| Daha önce Azure DevOps uzantı geliştiricileri için sunulan teklif türü. Azure DevOps uzantısı geliştiricilerinin ileri, doğrudan müşterilere dahili olarak dahili olarak iletilmesini sağlayabilir. Uzantı teklifleri ücretli olarak veya bir deneme dahil olmak üzere yapılandırılabilir. |
| Sipariş Iptali tarihi| Sipariş| Market siparişinin iptal edildiği tarih.||
| Sipariş Kimliği| Sipariş| Market hizmetiniz için müşteri siparişinin benzersiz tanımlayıcısı. Sanal makine kullanımı tabanlı teklifler bir siparişle ilişkili değildir.| |
| Sipariş satın alma tarihi| Sipariş| Market siparişinin oluşturulduğu tarih.|||
| Sipariş Durumu| Sipariş| Verilerin son yenilenme sırasında Market siparişi durumu.|     |
| Sipariş durumu: etkin  | Sipariş| Müşterinin siparişi satın almış ve siparişi iptal edilmemiş.|         |
| Sipariş durumu: iptal edildi | Sipariş| Müşteri daha önce bir siparişi satın almış ve ardından sıralarını iptal etti.||
| Sağlayıcı e-postası| Müşteri| Sağlayıcının Microsoft ile son müşteri arasındaki ilişkiye dahil olan e-posta adresi. Müşteri, satıcı üzerinden bir kurumsal ise, bu satıcı olacaktır. Bir bulut çözümü sağlayıcısı (CSP) dahil edilmişse, bu CSP olacaktır.|
| Sağlayıcı Adı| Müşteri| Microsoft ile son müşteri arasındaki ilişkiye katılan sağlayıcının adı. Müşteri, satıcı üzerinden bir kurumsal ise, bu satıcı olacaktır. Bir bulut çözümü sağlayıcısı (CSP) dahil edilmişse, bu CSP olacaktır.|
| SKU| Sipariş| Yayımlama sırasında tanımlanan SKU adı. Bir teklifin birçok SKU 'su olabilir, ancak bir SKU yalnızca tek bir teklifle ilişkilendirilebilir.||
| Deneme bitiş tarihi| Sipariş| Bu sipariş için deneme döneminin sona erdiği veya sonlandırmış olacağı tarih.||

## <a name="next-steps"></a>Sonraki adımlar

- Iş ortağı merkezi ticari marketi 'nde bulunan analiz raporlarına genel bakış için bkz. [Iş Ortağı Merkezi 'Nde ticari Market Için analiz](./analytics.md).
- Teklifinizin Market etkinliklerini özetleyen grafikler, eğilimler ve veri değerleri için bkz. [ticari Market Analytics 'Te Özet Panosu](./summary-dashboard.md).
- Grafik ve indirilebilir biçimdeki Siparişleriniz hakkında daha fazla bilgi için bkz. [Orders Dashboard in The Commercial Market Analytics](./orders-dashboard.md).
- Sanal makine (VM) için kullanım ve ölçümlenen faturalandırma ölçümleri, bkz. [ticari Market Analytics 'Te kullanım panosu](./usage-dashboard.md).
- Büyüme eğilimleri dahil olmak üzere müşterileriniz hakkında ayrıntılı bilgi için bkz. [ticari Market Analytics 'Te müşteri panosu](./customer-dashboard.md).
- Son 30 güne ait indirme isteklerinizin listesi için bkz. [Commercial Market Analytics 'Te panoyu indirme](./downloads-dashboard.md).
- Azure Market ve AppSource 'ta teklifler için müşteri geri bildirimlerinin birleştirilmiş bir görünümünü görmek için bkz. [ticari Market Analytics 'Te derecelendirmeler ve İncelemeler panosu](./ratings-reviews.md).
