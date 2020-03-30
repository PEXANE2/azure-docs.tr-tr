---
title: Pazar ölçüm hizmeti kullanarak tarifeli fatura | Azure Marketi
description: Bu dokümantasyon, esnek faturalandırma modelleri ile SaaS teklifleri yayınlayan ISV'ler için bir kılavuzdur.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 8e5a4813301cbab16d1cffabaaa60688f6e826ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80281332"
---
# <a name="metered-billing-using-the-marketplace-metering-service"></a>Pazar ölçüm hizmetini kullanarak ölçülü faturalandırma

Market ölçüm hizmeti ile, standart olmayan birimlere göre ücretlendirilen ticari pazar programında hizmet olarak yazılım (SaaS) teklifleri oluşturabilirsiniz.  Bu teklifi yayımlamadan önce, bant genişliği, biletler veya işlenen e-postalar gibi faturalandırma boyutlarını tanımlarsınız.  Müşteriler daha sonra bu boyutların tüketimine göre ödeme yaparlar ve sisteminiz, pazar yeri ölçüm hizmeti API'si aracılığıyla Microsoft'u faturalandırılabilir olaylar dan durumları bildirerek ödeme yaparlar.  

## <a name="prerequisites-for-metered-billing"></a>Ölçülü faturalandırma için ön koşullar

Bir SaaS teklifinin tarifeli faturalandırmayı kullanabilmesi için şunları yapması gerekir:

* [SaaS teklifi oluştur'da](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer)belirtildiği [gibi, Microsoft teklifi aracılığıyla satış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#sell-through-microsoft) için tüm teklif gereksinimlerini karşılayın.
* Müşterilerin hizmetine sunması ve teklifinize bağlanması için [SaaS Karşılama API'leri](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-fulfillment-api-v2) ile tümleştirin.  
* Müşterilerinizden hizmetiniz için ücretlendirmek için **sabit fiyat** fiyatlandırma modeli için yapılandırılabilir.  Boyutlar, sabit fiyat fiyatlandırma modeline isteğe bağlı bir uzantısıdır. 
* Microsoft'u faturalandırılabilir olaylar hakkında bilgilendirmek için [Market ölçüm hizmeti API'leriyle](./marketplace-metering-service-apis.md) tümleştirin.

>[!Note]
>Pazar yeri ölçüm hizmeti yalnızca sabit oranlı faturalandırma modeli için kullanılabilir ve kullanıcı başına faturalandırma modeli için geçerli değildir.

## <a name="how-metered-billing-fits-in-with-pricing"></a>Tarifeli faturalandırma nın fiyatlandırmaya nasıl uyduğu

Teklifin fiyatlandırma modelleri ile birlikte tanımlanması söz konusu olduğunda, teklif hiyerarşisini anlamak önemlidir.

* Her SaaS teklifi, Microsoft üzerinden satacak şekilde yapılandırılmıştır.  Bu ayar, teklif yayımlandıktan sonra değiştirilemez.
* Microsoft üzerinden satacak şekilde yapılandırılan her SaaS teklifi, bir veya daha fazla planı olabilir. Bir kullanıcı SaaS teklifine abone olur, ancak bir plan kapsamında Microsoft aracılığıyla satın alınır.
* Her planın onunla ilişkili bir fiyatlandırma modeli vardır: **sabit oran** veya **kullanıcı başına.** Bir teklifteki tüm planlar aynı fiyatlandırma modeliyle ilişkilendirilmelidir. Örneğin, planlarından birinin sabit oranlı fiyatlandırma modeli, diğerinin ise kullanıcı başına fiyatlandırma modeli olduğu bir teklif olamaz.
* Sabit oranlı faturalandırma modeli için yapılandırılan her planda, en az bir yinelenen ücret (0 TL olabilir) dahildir:
    * Yinelenen **aylık** ücret: Kullanıcı planı satın aldığında aylık yinelemede ön ödenen sabit aylık ücret.
    * Yinelenen **yıllık** ücret: Kullanıcı planı satın aldığında yıllık yineleme de ön ödenen sabit yıllık ücret.
* Yinelenen ücretlere ek olarak, plan, sabit fiyata dahil olmayan kullanımiçin müşterileri şarj etmek için kullanılan isteğe bağlı boyutları da içerebilir.   Her boyut, hizmetinizin [Pazar Yeri ölçüm hizmeti API'sini](./marketplace-metering-service-apis.md)kullanarak Microsoft'a ileteceği faturalandırılabilir bir birimi temsil eder.

## <a name="sample-offer"></a>Örnek teklif

Örnek olarak, Contoso Contoso Bildirim Hizmetleri (CNS) adlı bir SaaS hizmeti olan bir yayımcıdır. CNS, müşterilerin e-posta veya metin yoluyla bildirim göndermesine olanak tanır. Contoso, Azure müşterilerine teklif yayınlamak üzere ticari pazar programı için Ortak Merkezi'nde yayımcı olarak kayıtlıdır.  Aşağıda özetlenen CNS ile ilgili iki plan vardır:

* Temel plan
    * $0/month için 10000 e-posta ve 1000 mesaj gönderin
    * 10000 e-postanın ötesinde, her 100 e-posta için 1 $ ödeyin
    * 1000 metin dışında, her metin için 0,02 $ ödeyin
* Premium planı
    * $350/month için 50000 e-posta ve 10000 mesaj gönderin
    * 50000 e-postalar ötesinde, her 100 e-postalar için 0,5 $ ödemek
    * 10000 metin ötesinde, her metin için 0,01 $ ödeyin

CNS hizmetine abone olan bir Azure müşterisi, seçilen plana göre dahil edilen metin ve e-posta miktarını ayda gönderebilir.  Contoso, Microsoft'a herhangi bir kullanım olayı göndermeden kullanımı dahil edilen miktara kadar ölçer.  Müşteriler dahil edilen miktardan daha fazla tüketdiğinde, planları değiştirmek veya farklı bir şey yapmak zorunda değildir.  Contoso, dahil edilen miktarın üzerindeki fazlalıkları ölçecek ve [Market ölçüm hizmeti API'sini](./marketplace-metering-service-apis.md)kullanarak ek kullanım için kullanım olaylarını Microsoft'a yaymaya başlayacaktır.  Microsoft da yayımcı tarafından belirtildiği gibi ek kullanım için müşteriden ücret alacaktır.

## <a name="billing-dimensions"></a>Faturalandırma boyutları

Faturalandırma boyutları, yazılımı kullandıkları için nasıl faturalandırılacakları konusunda müşteriye iletişim kurmak ve ayrıca kullanım olaylarını Microsoft'a iletmek için kullanılır. Bunlar aşağıdaki gibi tanımlanır:

* **Boyut tanımlayıcısı**: kullanım olayları yayırken başvurulan değişmez tanımlayıcı.
* **Boyut adı**: boyutla ilişkili görüntü adı, örneğin "gönderilen metin iletileri".
* **Ölçü birimi:** faturabiriminin açıklaması, örneğin "kısa mesaj başına" veya "100 e-posta başına".
* **Birim fiyatı**: boyutun bir birim fiyatı.  
* **Aylık dönem için dahil edilen miktar**: yinelenen aylık ücreti ödeyen müşteriler için aylık dahil boyut miktarı, bir karşıcı olmalıdır.
* **Yıllık dönem için dahil edilen miktar**: yinelenen yıllık ücreti ödeyen müşteriler için aylık dahil boyut miktarı, bir karşıcı olmalıdır.

Faturalandırma boyutları, bir teklifiçin tüm planlar arasında paylaşılır.  Bazı öznitelikler tüm planlar arasında boyuta uygulanır ve diğer öznitelikler plana özgütür.

Boyutu tanımlayan öznitelikler, bir teklifiçin tüm planlar arasında paylaşılır.  Teklifi yayımlamadan önce, herhangi bir planın bağlamından bu özniteliklere yapılan bir değişiklik tüm planlardaki boyut tanımını etkiler.  Teklifi yayımladıktan sonra, bu öznitelikler artık edinilemez.  Bu öznitelikler şunlardır:

* Tanımlayıcı
* Adı
* Ölçü birimi

Bir boyutun diğer öznitelikleri her plana özgütür ve plandan plana farklı değerlere sahip olabilir.  Planı yayımlamadan önce bu değerleri edebilirsiniz ve yalnızca bu plan etkilenir.  Planı yayımladıktan sonra, bu öznitelikler artık edinilemez.  Bu öznitelikler şunlardır:

* Birim fiyat
* Aylık müşteriler için miktar dahil 
* Yıllık müşteriler için miktar dahil 

Boyutlar da iki özel kavram var, "etkin" ve "sonsuz":

* **Etkin,** bu planın bu boyuta katıldığını gösterir.  Bu boyuta dayalı kullanım olayları göndermeyen yeni bir plan oluşturuyorsanız, bu denetimsiz bırakmak isteyebilirsiniz.  Ayrıca, bir plan ilk yayımlandıktan sonra eklenen yeni boyutlar, zaten yayınlanmış olan planda "etkin değil" olarak gösterecektir.  Devre dışı bırakılmış bir boyut, müşteriler tarafından görülen bir plan için boyut listelerinde gösterilmez.
* **Sonsuzluk**sembolü "』" ile temsil edilen sonsuz, bu planın bu boyuta katıldığını, ancak bu boyuta karşı kullanımı ölçmediğini gösterir.  Müşterilerinize bu boyutun gösterdiği işlevselliğin plana dahil edildiğini, ancak kullanım sınırı olmadığını belirtmek isterseniz.  Sonsuz kullanıma sahip bir boyut, müşteriler tarafından görülen bir plan için boyut listelerinde gösterilecek ve bu plan için hiçbir zaman ücretalamayacağı na işaret eder.

>[!Note] 
>Aşağıdaki senaryolar açıkça desteklenir: <br> - Yeni bir plana yeni bir boyut ekleyebilirsiniz.  Yeni boyut, zaten yayımlanmış olan planlar için etkinleştirilmeyecektir. <br> - Herhangi bir boyut olmadan **sabit oranlı** bir plan yayımlayabilir, sonra yeni bir plan ekleyebilir ve bu plan için yeni bir boyut yapılandırabilirsiniz. Zaten yayımlanmış planlar için yeni boyut etkinleştirilmeyecektir.

## <a name="constraints"></a>Kısıtlamalar

### <a name="trial-behavior"></a>Deneme davranışı

Pazar ölçüm hizmetini kullanarak tarifeli faturalandırma, ücretsiz deneme sürümü sunmakla uyumlu değildir.  Hem ölçülü faturalandırma hem de ücretsiz deneme kullanmak için bir planı yapılandırmak mümkün değildir.

### <a name="locking-behavior"></a>Kilitleme davranışı

Market ölçüm hizmetinde kullanılan bir boyut, müşterinin hizmet için nasıl ödeme yapacağına dair bir anlayışı temsil ettiği için, bir boyutun tüm ayrıntıları, yayımladıktan sonra artık edilemeyecek.  Yayınlamadan önce bir plan için boyutlarınızın tam olarak tanımlanması önemlidir.
  
Bir teklif bir boyutla yayımlandıktan sonra, bu boyuta ait teklif düzeyi ayrıntıları artık değiştirilemez:

* Tanımlayıcı
* Adı
* Ölçü birimi

Bir plan yayımlandıktan sonra, plan düzeyindeki ayrıntılar artık değiştirilemez:

* Birim fiyat
* Aylık dönem için miktar dahil
* Yıllık dönem için miktar dahil
* Plan için boyutun etkin olup olmadığı

### <a name="upper-limits"></a>Üst sınırlar

Tek bir teklif için yapılandırılabilen maksimum boyut sayısı 18 benzersiz boyutdur.

## <a name="get-support"></a>Destek alın

Aşağıdakilerden birine sahipseniz, destek bileti açabilirsiniz.

* Pazar ölçüm hizmeti API ile teknik sorunlar.
* Tarafınızdaki bir hata veya hata nedeniyle çözülmesi gereken bir sorun (ör. yanlış kullanım olayı).
* Ölçülü faturalandırma ile ilgili diğer sorunlar. 

Destek biletinizi göndermek için aşağıdaki adımları izleyin:

1. [Destek sayfasına](https://support.microsoft.com/supportforbusiness/productselection?sapId=48734891-ee9a-5d77-bf29-82bf8d8111ff)gidin. İlk birkaç açılır menü ler sizin için otomatik olarak doldurulur. Market desteği için, ürün ailesini **Bulut ve Çevrimiçi Hizmetler,** ürünü **Market Yayıncısı**olarak tanımlayın.  Önceden doldurulmuş açılır menü seçimlerini değiştirmeyin.
2. "Ürün sürümünü seçin" **altında, Canlı teklif yönetimini**seçin.
3. "Sorunu en iyi açıklayan kategori seçin" altında **SaaS uygulamalarını**seçin.
4. "Sorunu en iyi açıklayan bir sorun seçin" **altında, ölçülü faturalandırmayı**seçin.
5. **Sonraki** düğmesini seçerek, sorununuzun daha fazla ayrıntısını girebileceğiniz **Sorun ayrıntıları** sayfasına yönlendirilirsiniz.

Daha fazla yayıncı destek seçeneği için [İş Merkezi'ndeki ticari pazar programı](https://docs.microsoft.com/azure/marketplace/partner-center-portal/support) için Destek'e bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Market ölçüm hizmeti API'leri'ne](./marketplace-metering-service-apis.md) bakın.
