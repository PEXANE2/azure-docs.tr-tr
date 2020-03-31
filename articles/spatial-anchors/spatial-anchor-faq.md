---
title: Sık sorulan sorular
description: Azure Uzamsal Çapalar hizmeti hakkında SSS'ler.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 563e2da8eba228636b05db2112739fdead4a4aa3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "76844896"
---
# <a name="frequently-asked-questions-about-azure-spatial-anchors"></a>Azure Uzamsal Çapalar hakkında sık sorulan sorular

Azure Uzamsal Çapalar, HoloLens, iOS ve Android cihazlarda çok kullanıcılı, mekansal olarak farkında karma gerçeklik deneyimleri sağlayan yönetilen bir bulut hizmeti ve geliştirici platformudur.

Daha fazla bilgi için Azure [Uzamsal Bağlantılarına genel bakış](overview.md)bilgisine bakın.

## <a name="azure-spatial-anchors-product-faqs"></a>Azure Uzamsal Çapalar Ürün SSS'leri

**S: Azure Uzamsal Çapalar hangi aygıtları destekler?**

**A:** Azure Uzamsal Çapalar, geliştiricilerin HoloLens'te, ARKit destekli iOS cihazlarda ve ARCore destekli Android cihazlarda uygulama oluşturmasına olanak tanır; iOS ve Android için bu hem telefonları hem de tabletleri içerir.

**S: Azure Uzamsal Çapaları kullanmak için buluta bağlanmam gerekiyor mu?**

**A:** Azure Uzamsal Bağlantılar şu anda internete bir ağ bağlantısı gerektirir. [Biz geribildirim sitemizde](https://feedback.azure.com/forums/919252-azure-spatial-anchors)yorumlarınızı bekliyoruz.

**S: Azure Uzamsal Bağlantı noktaları için bağlantı gereksinimleri nelerdir?**

**A:** Azure Uzamsal Çapalar Wi-Fi ve mobil geniş bant bağlantılarıyla çalışır.

**S: Azure Uzamsal Çapalar çapaları ne kadar doğru bulabilir?**

**A:** Birçok faktör çapa bulma doğruluğunu etkiler - aydınlatma koşulları, ortamdaki nesneler, ve hatta çapa yerleştirilir yüzey. Doğruluğun ihtiyaçlarınızı karşılayıp karşılamadığını belirlemek için, bunları kullanmayı planladığınız yeri temsil eden ortamlardaki çapaları deneyin. Doğruluğun ihtiyaçlarınızı karşıladığı ortamlarla karşılaşırsanız, [Azure Uzamsal Bağlantılarında Günlüğe Kaydetme ve tanılama'ya](./concepts/logging-diagnostics.md)bakın.

**S: Çapa oluşturmak ve bulmak ne kadar sürer?**

**A:** Bağlantı noktası oluşturmak ve bulmak için gereken süre, ağ bağlantısı, aygıtın işlenmesi ve yükü ve belirli ortam gibi birçok etkene bağlıdır. Üretim, perakende ve oyun dahil olmak üzere birçok sektörde uygulama yapan müşterilerimiz, hizmetin senaryoları için harika bir kullanıcı deneyimi sağladığını belirterek sahibiz.

## <a name="privacy-faq"></a>Gizlilik SSS

**S: Uygulamam bir yere Uzamsal Çapa yerleştirdiğinde tüm uygulamaların bu bağlantıya erişimi var mıdır?**

**A:** Çapalar Azure hesabı tarafından yalıtılır. Yalnızca hesabınıza erişim izni verdiğiniz uygulamalar, hesap içindeki çapalara erişebilir.

**S: Azure Uzamsal Çapalar kullanırken bir ortam la ilgili hangi bilgiler aktarılır ve hizmette depolanır? Ortam resimleri iletilen ve depolanan mı?**

**C**: Çapa oluştururken veya konumlandırılırken, ortam resimleri aygıtta türemiş bir biçimde işlenir. Bu türetilmiş biçim hizmete aktarılır ve depolanır.

Saydamlık sağlamak için, aşağıda bir ortamın görüntüsü ve türetilmiş seyrek nokta bulutu yer almaktadır. Nokta bulutu, hizmete iletilen ve depolanan ortamın geometrik temsilini gösterir. Seyrek nokta bulutundaki her nokta için, o noktanın görsel özelliklerinin bir karmasını iletir ve saklarız. Karma, herhangi bir piksel verisinden türetilir, ancak içermemektedir.

Azure Uzamsal Çapalar, [Azure Hizmet Sözleşmesi Koşullarına](https://go.microsoft.com/fwLink/?LinkID=522330&amp;amp;clcid=0x9)ve Microsoft Gizlilik [Bildirimi'ne](https://go.microsoft.com/fwlink/?LinkId=521839&amp;clcid=0x409)bağlıdır.

![Bir ortam ve türetilmiş seyrek](./media/sparse-point-cloud.png)
nokta bulutu*Şekil 1: Bir ortam ve türetilmiş seyrek nokta bulutu*


**S: Microsoft'a tanılama bilgilerini göndermemin bir yolu var mı?**

**A**: Evet. Azure Uzamsal Bağlantılar, geliştiricilerin Azure Uzamsal ÇapaAPI üzerinden tercih etmeyi seçebileceği bir tanılama moduna sahiptir. Bu, örneğin, bağlantı larını tahmin edilebileceği gibi oluşturamadığınız ve bulamadığınız bir ortamla karşılaşırsanız yararlıdır. Hata ayıklamamıza yardımcı olan bilgileri içeren bir tanılama raporu gönderebilir misiniz diye sorabiliriz. Daha fazla bilgi için Azure [Uzamsal Bağlantılarında Günlüğe Kaydetme ve tanılama'](./concepts/logging-diagnostics.md)ya bakın.

## <a name="availability-and-pricing-faqs"></a>Kullanılabilirlik ve Fiyatlandırma SSS'leri

**S: Bir SLA sağlıyor musunuz?**

**A:** Azure hizmetleri için standart olduğu gibi, kullanılabilirlik %99,9'dan daha büyük bir kullanılabilirlik hedefliyoruz. Azure Uzamsal Bağlantı Çapaları'nın şu anda Önizleme'de olduğunu ve bu nedenle [Önizleme Ek Koşulları'nın](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) geçerli olduğunu unutmayın.

**S: Uygulama mağazalarında Azure Uzamsal Çapalar kullanarak uygulamalarımı yayımlayabilir miyim? Kritik görev üretim senaryoları için Azure Uzamsal Çapaları kullanabilir miyim?**

**A:** Azure Uzamsal Çapalar şu anda Önizleme'de ve bu süre zarfında sizi uygulamalar geliştirmeye, ürün hakkında [geri bildirim de meye](https://feedback.azure.com/forums/919252-azure-spatial-anchors) ve üretim dağıtımlarınız için plan yapmaya davet ediyoruz.

Genel Kullanılabilirlik (GA) tarihleri yakında duyurulacaktır.

**S: Herhangi bir azaltma limitinvar mı?**

**C**: Evet, azaltma limitimiz var.  Tipik uygulama geliştirme ve test için bunları vuracağınız beklemiyoruz. Üretim dağıtımları için, müşterilerimizin yüksek ölçekli gereksinimlerini desteklemeye hazırız. Görüşmek için [bize ulaşın.](mailto:azuremrs@microsoft.com) Önizleme'nin bu aşamasında, katmanlama ve fiyatlandırma yapımızı henüz yayınlamadık, ancak bunu yakında yapmayı bekliyoruz.

**S: Azure Uzamsal Çapalar hangi bölgelerde kullanılabilir?**

**A:** Bugün Azure Doğu ABD 2 bölgesinde bir Azure Uzamsal Çapa hesabı oluşturabilirsiniz. Bunun anlamı, bu hizmete güç sağlayan hem hesaplama hem de depolama nın bu bölgede olduğudur. Bununla ilgili olarak, müşterilerinizin nerede bulunduğuna ilişkin herhangi bir kısıtlama yoktur. Gelecekte, hizmetin bölgesel kullanılabilirliğini tüm birincil Azure bölgelerine genişleteceğiz.

**S: Azure Uzamsal Çapalar için ücret lendirin mi? Hiç suçlayacak mısın?**

**A:** [Fiyatlandırma sayfamızda](https://azure.microsoft.com/pricing/details/spatial-anchors/)Önizleme sırasında fiyatlandırma ile ilgili ayrıntıları bulabilirsiniz.

## <a name="technical-faqs"></a>Teknik SSS

**S: Azure Uzamsal Çapalar nasıl çalışır?**

**A:** Azure Uzamsal Çapalar karışık gerçeklik / artırılmış gerçeklik izleyicilerine bağlıdır. Bu izleyiciler çevreyi kameralarla algılar ve uzayda hareket ederken cihazı 6-0 00 000 serbestlik (6DoF) içinde izlerler.

Bir yapı taşı olarak 6DoF izci göz önüne alındığında, Azure Uzamsal Çapalar gerçek ortamınızda belirli ilgi noktalarını "çapa" noktaları olarak belirlemenize olanak tanır. Örneğin, içeriği gerçek dünyada belirli bir yerde işlemek için bir çapa kullanabilirsiniz.

Bir bağlantı oluşturduğunuzda, istemci SDK bu noktanın çevresindeki ortam bilgilerini yakalar ve hizmete iletir. Başka bir aygıt aynı alanda çapa ararsa, benzer veriler hizmete iletir. Bu veriler, daha önce depolanan ortam verileriyle eşleşir. Bağlantının aygıta göre konumu daha sonra uygulamada kullanılmak üzere geri gönderilir.

**S: Azure Uzamsal Çapalar iOS ve Android'de ARKit ve ARCore ile nasıl entegre olur?**

**A:** Azure Uzamsal Çapalar, ARKit ve ARCore'un yerel izleme özelliklerinden yararlanır. Buna ek olarak, iOS ve Android için SDK'larımız, yönetilen bir bulut hizmetinde bağlantı larını sürdürmek ve uygulamalarınızın yalnızca hizmete bağlanarak bu çapaları yeniden bulmasına olanak sağlamak gibi özellikler sunar.

**S: Azure Uzamsal Çapalar HoloLens ile nasıl entegre olur?**

**A:** Azure Uzamsal Çapalar, HoloLens'in yerel izleme özelliklerinden yararlanır. HoloLens'te uygulama oluşturmak için bir Azure Uzamsal Çapa SDK sda sda sediyoruz. SDK yerli HoloLens yetenekleri ile entegre ve ek yetenekleri sağlar. Bu özellikler arasında uygulama geliştiricilerin yönetilen bir bulut hizmetinde bağlantılarını devam etmesine izin vermek ve uygulamalarınızın hizmete bağlanarak bu çapaları yeniden bulmasına izin vermek yer alır.

**S: Azure Uzamsal Çapalar hangi platformları ve dilleri destekler?**

**A:** Geliştiriciler, aygıtları için tanıdık araçlar ve çerçeveler kullanarak Azure Uzamsal Bağlantı Larını kullanarak uygulamalar oluşturabilir:

- HoloLens, iOS ve Android genelinde birlik
- Xamarin üzerinde iOS ve Android
- iOS'ta Swift veya Objective-C
- Android'de Java veya Android NDK
- HoloLens'te C++/WinRT

[Burada geliştirme](index.yml)ile başlayın.

**S: Unreal ile çalışır mı?**

**A:** Unreal için destek gelecekte dikkate alınacaktır.

**S: Azure Uzamsal Çapalar hangi bağlantı noktalarını ve protokollerini kullanır?**

**A:** Azure Uzamsal Çapalar, tcp bağlantı noktası 443 üzerinden şifreli bir protokol kullanarak iletişim kurar. Kimlik doğrulaması için, 443 bağlantı noktası üzerinden HTTPS kullanarak iletişim sağlayan [Azure Etkin Dizini'ni](https://docs.microsoft.com/azure/active-directory/)kullanır.
