---
title: include dosyası
description: include dosyası
services: iot-fundamentals
author: robinsh
ms.service: iot-fundamentals
ms.topic: include
ms.date: 08/07/2018
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 4fdb891d668d99644d8a9ed9c15d158e65d53ba5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72793120"
---
Nesnelerin İnterneti (IoT) altyapısının güvenliğinin sağlanması için kapsamlı bir güvenlik stratejisi gerekir. Bu strateji, buluttaki verileri korumanızı, genel internet üzerinden geçiş sırasında veri bütünlüğünü korumanızı ve cihazları güvenli bir şekilde sağlamanızı gerektirir. Her katman, genel altyapıda daha fazla güvenlik güvencesi oluşturur.

## <a name="secure-an-iot-infrastructure"></a>Bir IoT altyapıyı güvenli hale

Bu derinlemesine güvenlik stratejisi, IoT cihazlarının ve altyapısının üretimi, geliştirilmesi ve dağıtımıyla ilgili çeşitli oyuncuların aktif katılımıyla geliştirilebilir ve yürütülebilir. Aşağıdaki bu oyuncuların üst düzey bir açıklamadır.

* **IoT donanım üreticisi/entegratörü**: Tipik olarak, bu oyuncular dağıtılan IoT donanımÜreticileri, çeşitli üreticilerin donanımı montajı entegratörleri veya diğer tedarikçiler tarafından üretilen veya entegre edilen bir IoT dağıtımı için donanım sağlayan tedarikçilerdir.

* **IoT çözüm geliştiricisi**: Bir IoT çözümünün geliştirilmesi genellikle bir çözüm geliştiricisi tarafından yapılır. Bu geliştirici, şirket içi bir ekibin veya bu etkinlikte uzmanlaşmış bir sistem entegratörünün (SI) bir parçası olabilir. IoT çözüm geliştiricisi, IoT çözümünün çeşitli bileşenlerini sıfırdan geliştirebilir, çeşitli hazır veya açık kaynaklı bileşenleri entegre edebilir veya küçük adaptasyonla çözüm hızlandırıcıları benimseyebilir.

* **IoT çözüm konuşlandırıcısı**: Bir IoT çözümü geliştirildikten sonra, sahada konuşlandırılması gerekir. Bu işlem, donanım dağıtımı, aygıtların ara bağlantısı ve donanım aygıtlarında veya bulutta çözümlerin dağıtılmasını içerir.

* **IoT çözüm operatörü**: IoT çözümü dağıtıldıktan sonra uzun süreli operasyonlar, izleme, yükseltmeler ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemleri ve bakım ekipleri ve genel IoT altyapısının doğru davranışını izleyen etki alanı uzmanlarından oluşan şirket içi bir ekip tarafından yapılabilir.

İzleyen bölümler, güvenli bir IoT altyapısının geliştirilmesine, dağıtılmasına ve çalıştırılabına yardımcı olmak için bu oyuncuların her biri için en iyi uygulamaları sağlar.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT donanım üreticisi/entegratörü

Aşağıda IoT donanım üreticileri ve donanım entegratörleri için en iyi uygulamalar veremistir.

* **Kapsam donanımı minimum gereksinimlere kadar**: Donanım tasarımı, donanımın çalışması için gereken minimum özellikleri içermelidir ve başka bir şey içermemelidir. Bir örnek, yalnızca aygıtın çalışması için gerekli olduğunda USB bağlantı noktalarını eklemektir. Bu ek özellikler, kaçınılması gereken istenmeyen saldırı vektörleri için aygıtı açar.

* **Donanım kurcalamaya karşı dayanıklı olun**: Aygıt kapağının açılması veya aygıtın bir bölümünün kaldırılması gibi fiziksel kurcalamayı algılayacak mekanizmalar oluşturun. Bu kurcalama sinyalleri buluta yüklenen veri akışının bir parçası olabilir ve bu da operatörleri bu olaylar hakkında uyarabilir.

* **Güvenli donanım oluşturma**: COGS izin verirse, güvenli ve şifreli depolama gibi güvenlik özellikleri oluşturun veya Güvenilir Platform Modülüne (TPM) dayalı önyükleme işlevselliği oluşturun. Bu özellikler aygıtları daha güvenli hale getirmekte ve genel IoT altyapısının korunmasına yardımcı olur.

* **Yükseltmeleri güvenli hale getirin:** Cihazın ömrü boyunca firmware yükseltmeleri kaçınılmazdır. Yükseltmeler için güvenli yollara ve firmware sürümlerinin şifreleme güvencesine sahip aygıtlar oluşturmak, aygıtın yükseltmeler sırasında ve sonrasında güvenli olmasını sağlar.

## <a name="iot-solution-developer"></a>IoT çözüm geliştiricisi

IoT çözüm geliştiricileri için en iyi uygulamalar şunlardır:

* **Güvenli yazılım geliştirme metodolojisini takip edin**: Güvenli yazılımın geliştirilmesi, projenin başlangıcından uygulanmasına, sınamasına ve dağıtımına kadar güvenlik hakkında temel düşünmeyi gerektirir. Platformların, dillerin ve araçların seçeneklerinin tümü bu metodolojiden etkilenir. Microsoft Güvenlik Geliştirme Yaşam Döngüsü, güvenli yazılım oluşturmak için adım adım bir yaklaşım sağlar.

* **Özenle açık kaynak yazılım seçin**: Açık kaynak yazılım hızlı çözümler geliştirmek için bir fırsat sağlar. Açık kaynak kodlu yazılım seçerken, her açık kaynak bileşeni için topluluğun etkinlik düzeyini göz önünde bulundurun. Etkin bir topluluk, yazılımın desteklenmesini ve sorunların keşfedilmesini ve ele alınmasını sağlar. Alternatif olarak, belirsiz ve etkin olmayan bir açık kaynak yazılım projesi desteklenmeyebilir ve sorunlar büyük olasılıkla keşfedilmeyebilir.

* **Bakım ile tümleştirme**: Kütüphaneler ve API'ler sınırında birçok yazılım güvenlik açıklığı vardır. Geçerli dağıtım için gerekli olmayabilir işlevsellik hala bir API katmanı üzerinden kullanılabilir olabilir. Genel güvenliği sağlamak için, güvenlik açıkları için tümleşik bileşenlerin tüm arabirimlerini kontrol ettiğinizden emin olun.

## <a name="iot-solution-deployer"></a>IoT çözüm dağıtımcısı

IoT çözüm dağıtıcıları için en iyi uygulamalar şunlardır:

* **Donanımı güvenli bir şekilde dağıtma**: IoT dağıtımları, donanımın ortak alanlarda veya denetimsiz yerel alanlar gibi güvenli olmayan konumlarda dağıtılmasını gerektirebilir. Bu gibi durumlarda, donanım dağıtımının en yüksek ölçüde kurcalanmaya karşı dayanıklı olduğundan emin olun. Donanımda USB veya başka bağlantı noktaları varsa, bunların güvenli bir şekilde kapatıldığından emin olun. Birçok saldırı vektörü bunları giriş noktası olarak kullanabilir.

* **Kimlik doğrulama anahtarlarını güvende tutun**: Dağıtım sırasında her cihaz, bulut hizmeti tarafından oluşturulan aygıt kimliklerini ve ilişkili kimlik doğrulama anahtarlarını gerektirir. Dağıtımdan sonra bile bu anahtarları fiziksel olarak güvende tutun. Herhangi bir tehlikeye anahtar varolan bir aygıt olarak görünmesi için kötü amaçlı bir aygıt tarafından kullanılabilir.

## <a name="iot-solution-operator"></a>IoT çözüm operatörü

IoT çözüm operatörleri için en iyi uygulamalar şunlardır:

* **Sistemi güncel tutun**: Cihaz işletim sistemlerinin ve tüm aygıt sürücülerinin en son sürümlere yükseltilmesini sağlayın. Windows 10'da (IoT veya diğer SUS'larda) otomatik güncelleştirmeleri açarsanız, Microsoft bunu güncel tutar ve IoT aygıtları için güvenli bir işletim sistemi sağlar. Diğer işletim sistemlerini (Linux gibi) güncel tutmak, sistemlerin kötü amaçlı saldırılara karşı da korunmasına yardımcı olur.

* **Kötü amaçlı faaliyetlere karşı koruyun**: İşletim sistemi izin verirse, her cihaz işletim sistemine en son virüsten koruma ve kötü amaçlı yazılımdan koruma özelliklerini yükleyin. Bu uygulama, çoğu dış tehditleri azaltmaya yardımcı olabilir. Uygun adımları atarak en modern işletim sistemlerini tehditlere karşı koruyabilirsiniz.

* **Sık sık denetim**: Güvenlikle ilgili sorunlar için IoT altyapısının denetilmesi, güvenlik olaylarına yanıt verirken önemlidir. Çoğu işletim sistemi, güvenlik ihlali oluştuğundan emin olmak için sık sık gözden geçirilmesi gereken yerleşik olay günlüğe kaydetme sağlar. Denetim bilgileri, analiz edilebildiği bulut hizmetine ayrı bir telemetri akışı olarak gönderilebilir.

* **IoT altyapısını fiziksel olarak koruyun**: IoT altyapısına yönelik en kötü güvenlik saldırıları, cihazlara fiziksel erişim kullanılarak başlatılır. Önemli bir güvenlik uygulaması USB bağlantı noktaları ve diğer fiziksel erişim kötü niyetli kullanımına karşı korumaktır. Meydana gelmiş olabilecek ihlalleri ortaya çıkarmanın anahtarlarından biri, USB bağlantı noktası kullanımı gibi fiziksel erişimin günlüğe kaydedilmesidir. Yine, Windows 10 (IoT ve diğer SUS' lar) bu olayların ayrıntılı günlüğe kaydedilmesini sağlar.

* **Bulut kimlik bilgilerini koruyun**: Bir IoT dağıtımını yapılandırmak ve işletmek için kullanılan bulut kimlik doğrulama kimlik bilgileri, bir IoT sistemine erişmek ve tehlikeye atmak için en kolay yoldur. Parolayı sık sık değiştirerek kimlik bilgilerini koruyun ve bu kimlik bilgilerini ortak makinelerde kullanmaktan kaçının.

Farklı IoT aygıtlarının yetenekleri farklılık gösterir. Bazı aygıtlar ortak masaüstü işletim sistemleri çalıştıran bilgisayarlar olabilir ve bazı aygıtlar çok hafif işletim sistemleri çalıştırıyor olabilir. Daha önce açıklanan en iyi güvenlik uygulamaları, bu aygıtlar için farklı derecelerde uygulanabilir olabilir. Sağlandığı takdirde, bu aygıtların üreticilerinden gelen ek güvenlik ve dağıtım en iyi uygulamaları izlenmelidir.

Bazı eski ve kısıtlı aygıtlar, IoT dağıtımı için özel olarak tasarlanmamış olabilir. Bu aygıtlar verileri şifreleme, Internet'e bağlanma veya gelişmiş denetim sağlama özelliğine sahip olmayabilir. Bu gibi durumlarda, modern ve güvenli bir alan ağ geçidi eski aygıtlardan veri toplayabilir ve bu aygıtları Internet üzerinden bağlamak için gereken güvenliği sağlayabilir. Alan ağ geçitleri güvenli kimlik doğrulaması, şifreli oturumların pazarlanması, buluttan komutların alınmasını ve diğer birçok güvenlik özelliği sağlayabilir.