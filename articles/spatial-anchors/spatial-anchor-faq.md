---
title: Sık sorulan sorular
description: Azure uzamsal bağlayıcı hizmeti hakkında SSS.
author: ramonarguelles
manager: vriveras
services: azure-spatial-anchors
ms.author: rgarcia
ms.date: 02/24/2019
ms.topic: overview
ms.service: azure-spatial-anchors
ms.openlocfilehash: 563e2da8eba228636b05db2112739fdead4a4aa3
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76844896"
---
# <a name="frequently-asked-questions-about-azure-spatial-anchors"></a>Azure uzamsal bağlantıları hakkında sık sorulan sorular

Azure uzamsal bağlantıları, HoloLens, iOS ve Android cihazlarda çok kullanıcılı, istenmeyen, dağınık karma gerçeklik deneyimlerini sağlayan, yönetilen bir bulut hizmeti ve geliştirici platformudur.

Daha fazla bilgi için bkz. [Azure uzamsal Tutturucuların genel bakış](overview.md).

## <a name="azure-spatial-anchors-product-faqs"></a>Azure uzamsal çıpası ürün SSS

**S: Azure uzamsal bağlantıları hangi cihazları destekler?**

Y **:** Azure uzamsal bağlantıları, geliştiricilerin ARKit desteği olan iOS cihazlarında ve ARCore desteğiyle Android cihazlarda uygulama oluşturmalarına olanak sağlar; iOS ve Android için bu, hem telefonları hem de tabletleri içerir.

**S: Azure uzamsal bağlayıcılarını kullanmak için buluta bağlı mıyım?**

Y **:** Azure uzamsal Tutturucuların Şu anda internet 'e bir ağ bağlantısı olması gerekir. [Geri bildirim sitemiz](https://feedback.azure.com/forums/919252-azure-spatial-anchors)hakkındaki yorumlarınızı inceledik.

**S: Azure uzamsal Tutturucuların bağlantı gereksinimleri nelerdir?**

Y **:** Azure uzamsal bağlantıları Wi-Fi ve mobil geniş bant bağlantılarıyla birlikte kullanılabilir.

**S: Azure uzamsal bağlantıları bağlantıları ne kadar doğru bulabilir?**

Y **:** Birçok etken, bağlama koşullarını, ortamdaki nesneleri, hatta bağlayıcının yerleştirildiği yüzeyi bulmaya yönelik doğruluğu etkiler. Doğruluğun gereksinimlerinizi karşılayıp karşılamadığını öğrenmek için, bu bağlantıları kullanmayı planladığınız ortamlar temsilcisinden deneyin. Doğruluğunun gereksinimlerinizi karşılamadığı ortamlarda karşılaşırsanız bkz. [Azure uzamsal bağlayıcılarda günlüğe kaydetme ve tanılama](./concepts/logging-diagnostics.md).

**S: bağlantıları oluşturma ve bulma ne kadar sürer?**

Y **:** Bağlantı oluşturma ve bulma için gereken süre birçok etkene bağlıdır--ağ bağlantısı, cihazın işleme ve yükleme ve belirli ortam. BT, perakende ve oyun gibi birçok sektörde, hizmetin senaryoları için harika bir kullanıcı deneyimine izin vermediğini belirten uygulamalar oluşturuyor.

## <a name="privacy-faq"></a>Gizlilik SSS

**S: Uygulamam bir uzamsal bağlayıcı yerleştirdiğinden tüm uygulamaların bu konuma erişimi vardır mi?**

Y **:** Çıpaları Azure hesabı tarafından yalıtılmıştır. Yalnızca hesabınıza erişim izni verdiğiniz uygulamalar, hesap içindeki Tutturuculara erişebilecektir.

**S: Azure uzamsal bağlayıcıları kullanılırken bir ortamla ilgili hangi bilgiler iletilir ve hizmete depolanır? Aktarılan ve depolanan ortamın resimleri mı var?**

Y **: bağlantıları**oluştururken veya bulmada, ortamın resimleri, cihazda türetilmiş bir biçimde işlenir. Bu türetilmiş biçim öğesine iletilir ve hizmete depolanır.

Saydamlığın sağlanması için bir ortamın görüntüsü ve türetilmiş seyrek nokta bulutu aşağıda verilmiştir. Nokta bulutu, hizmet üzerinde aktarılan ve depolanan ortamın geometrik temsilini gösterir. Seyrek nokta bulutundaki her bir nokta için, bu noktanın görsel özelliklerinin karmasını ilettik ve depolarız. Karma, herhangi bir piksel verisini içermez, ancak içermez.

Azure uzamsal bağlantıları, [Azure hizmet sözleşmesi koşullarına](https://go.microsoft.com/fwLink/?LinkID=522330&amp;amp;clcid=0x9)ve [Microsoft gizlilik bildirimine](https://go.microsoft.com/fwlink/?LinkId=521839&amp;clcid=0x409)uyar.

![Bir ortam ve onun türetilmiş seyrek nokta bulutu](./media/sparse-point-cloud.png)
*Şekil 1: bir ortam ve onun türetilmiş seyrek nokta bulutu*


**S: Microsoft 'a tanılama bilgilerini gönderemem için bir yol var mı?**

Y **: Evet**. Azure uzamsal bağlayıcılarının, geliştiricilerin Azure uzamsal bağlayıcı API 'SI aracılığıyla kabul etmek için seçim yapabileceğiniz bir tanılama modu vardır. Bu, örneğin, oluşturma ve bağlayıcıları tahmin edilemeyecek şekilde tespit edilebileceği bir ortamla karşılaşırsanız yararlı olur. Hata ayıklamamıza yardımcı olan bilgiler içeren bir tanılama raporu gönderebildiğinize sorabilirsiniz. Daha fazla bilgi için bkz. [Azure uzamsal bağlayıcılarda günlüğe kaydetme ve tanılama](./concepts/logging-diagnostics.md).

## <a name="availability-and-pricing-faqs"></a>Kullanılabilirlik ve fiyatlandırma SSS

**S: bir SLA sağlayın mi?**

Y **:** Azure hizmetleri için standart olduğu gibi,% 99,9 ' den büyük bir kullanılabilirliği hedefliyoruz. Azure uzamsal Çıpaların Şu anda önizleme aşamasında olduğunu ve [Önizleme ek koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 'nın geçerli olduğunu unutmayın.

**S: uygulamalarımı Azure uzamsal bağlayıcılarını uygulama mağazalarına kullanarak yayımlayabilirim miyim? Görev açısından kritik üretim senaryolarında Azure uzamsal bağlayıcılarını kullanabilir miyim?**

Y **:** Azure uzamsal çıpası Şu anda önizleme aşamasındadır ve bu süre boyunca uygulama geliştirmeye, ürünle ilgili [geri bildirim sağlamanıza](https://feedback.azure.com/forums/919252-azure-spatial-anchors) ve üretim dağıtımlarınızı planlamaya davet ediyoruz.

Genel kullanılabilirlik (GA) tarihleri yakında duyurulacak.

**S: herhangi bir azaltma sınırlarınız var mı?**

Y **: Evet**, azaltma sınırımız var.  Bunlara, tipik uygulama geliştirme ve test için ulaşacağız. Üretim dağıtımları için, müşterilerimizin yüksek ölçekli gereksinimlerini desteklemeye hazırız. Tartışmak için [bizimle Iletişim kurun](mailto:azuremrs@microsoft.com) . Bu önizleme aşamasında, katmanlama ve fiyatlandırma yapınızı henüz yayımladık, ancak bu işlemi yakında yapacağız.

**S: Azure uzamsal çıpası hangi bölgelerde kullanılabilir?**

Y **:** Azure Doğu ABD 2 bölgesinde bugün bir Azure uzamsal bağlayıcı hesabı oluşturabilirsiniz. Bunun anlamı, bu hizmeti destekleyen işlem ve depolama alanının bu bölgede olduğunu gösterir. Bu şekilde, istemcilerinizin bulunduğu yerde bir kısıtlama yoktur. Gelecekte, hizmetin bölgesel kullanılabilirliğini tüm birincil Azure bölgelerine genişleteceğiz.

**S: Azure uzamsal Tutturucuların ücretlendirmi? Ücret ödemeniz gerekecek mi?**

Y **:** Fiyatlandırma [sayfamız](https://azure.microsoft.com/pricing/details/spatial-anchors/)sırasında, önizleme sırasında fiyatlandırma hakkındaki ayrıntıları bulabilirsiniz.

## <a name="technical-faqs"></a>Teknik SSS

**S: Azure uzamsal çıpası nasıl çalışır?**

Y **:** Azure uzamsal bağlantıları, karma gerçeklik/genişletilmiş gerçeklik izleyicileri 'ne bağlıdır. Bu izleyiciler, ortamı kameralardan çevrelenir ve alana hareket ederken cihazı 6-serbestlik (6DoF) olarak izler.

Yapı bloğu olarak bir 6DoF izleyici verildiğinde, Azure uzamsal bağlantıları gerçek ortamınızda "bağlayıcı" noktaları olarak belirli noktaları tanımlamanızı sağlar. Örneğin, gerçek dünyada belirli bir yerde içerik işlemek için bir tutturucu kullanabilirsiniz.

Bir tutturucu oluşturduğunuzda, istemci SDK 'Sı bu noktanın etrafında ortam bilgilerini yakalar ve hizmete iletir. Diğer bir cihaz aynı alanda yer alan bir bağlantı için görünüyorsa, benzer veriler hizmete iletir. Bu veriler, daha önce depolanan ortam verileriyle eşleştirilir. Cihaza göre bağlayıcının konumu daha sonra uygulamada kullanılmak üzere geri gönderilir.

**S: Azure uzamsal bağlantıları iOS ve Android 'de ARKit ve ARCore ile nasıl tümleştirilir?**

Y **:** Azure uzamsal bağlantıları, ARKit ve ARCore 'un yerel izleme yeteneklerini kullanır. Ayrıca, iOS ve Android için SDK 'larımız, yönetilen bir bulut hizmetindeki kalıcı Çıpaları gibi yetenekler sunar ve uygulamalarınızın yalnızca hizmete bağlanarak bu bağlantıları tekrar bulmasını sağlar.

**S: Azure uzamsal bağlantıları HoloLens ile nasıl tümleştirilir?**

Y **:** Azure uzamsal bağlantıları, HoloLens 'in yerel izleme yeteneklerini kullanır. HoloLens 'te uygulama oluşturmaya yönelik bir Azure uzamsal bağlayıcı SDK 'Sı sunuyoruz. SDK, yerel HoloLens özellikleri ile tümleşir ve ek yetenekler sağlar. Bu yetenekler, uygulama geliştiricilerinin bağlantıları yönetilen bir bulut hizmetinde kalıcı hale getirebilmesine ve uygulamalarınızın hizmete bağlanarak bu bağlantıları yeniden bulmasını sağlar.

**S: Azure uzamsal bağlantıları hangi platformları ve dilleri destekler?**

Y **:** Geliştiriciler, cihazları için tanıdık araçları ve çerçeveleri kullanarak Azure uzamsal bağlayıcılarıyla uygulama oluşturabilir:

- HoloLens, iOS ve Android genelinde Unity
- İOS ve Android 'de Xamarin
- İOS üzerinde Swift veya amaç-C
- Android üzerinde Java veya Android NDK
- HoloLens üzerinde C++/Wınrt

[Geliştirmeye buradan](index.yml)başlayın.

**S: gerçek zamanlı değil mi çalışıyor?**

Y **:** Unreal desteği gelecekte kabul edilecek.

**S: Azure uzamsal bağlantıları hangi bağlantı noktaları ve protokoller kullanır?**

Y **:** Azure uzamsal bağlantıları, TCP bağlantı noktası 443 üzerinden şifrelenmiş bir protokol kullanarak iletişim kurar. Kimlik doğrulaması için, 443 bağlantı noktası üzerinden HTTPS kullanarak iletişim kuran [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)kullanır.
