---
title: Azure IoT Hub için aygıt yapılandırması en iyi uygulamalar | Microsoft Dokümanlar
description: IoT aygıtlarının ölçekte yönetilmesiyle ilgili yinelenen ve karmaşık görevleri en aza indirmek için otomatik aygıt yönetimini kullanmak için en iyi uygulamalar hakkında bilgi edinin.
author: chrisgre
ms.author: chrisgre
ms.date: 06/28/2019
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: a3b70af71c2ce19835ac2ef8fc8ceed79ca5fe1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73889524"
---
# <a name="best-practices-for-device-configuration-within-an-iot-solution"></a>Bir IoT çözümü içinde cihaz yapılandırması için en iyi uygulamalar

Azure IoT Hub'daki otomatik aygıt yönetimi, büyük aygıt filolarını tüm yaşam döngüleri boyunca yönetmek le ilgili birçok yinelenen ve karmaşık görevi otomatikleştirir. Bu makalede, bir IoT çözümünün geliştirilmesi ve işletilmesinde yer alan çeşitli roller için en iyi uygulamaların çoğu tanımlanamaz.

* **IoT donanım üreticisi/entegratörü:** IoT donanım üreticileri, çeşitli üreticilerin donanımını biraraya getiren entegratörler veya diğer tedarikçiler tarafından üretilen veya entegre edilen bir IoT dağıtımı için donanım sağlayan tedarikçiler. Firmware, gömülü işletim sistemleri ve gömülü yazılım geliştirme ve entegrasyon dahil.

* **IoT çözüm geliştiricisi:** Bir IoT çözümünün geliştirilmesi genellikle bir çözüm geliştiricisi tarafından yapılır. Bu geliştirici, şirket içi ekibin veya bu etkinlikte uzmanlaşmış bir sistem entegratörünün bir parçası olabilir. IoT çözüm geliştiricisi, IoT çözümünün çeşitli bileşenlerini sıfırdan geliştirebilir, çeşitli standart veya açık kaynak bileşenlerini entegre edebilir veya bir [IoT çözüm hızlandırıcısını](/azure/iot-accelerators/)özelleştirebilir.

* **IoT çözüm operatörü:** IoT çözümü dağıtıldıktan sonra, uzun vadeli işlemler, izleme, yükseltmeler ve bakım gerektirir. Bu görevler, bilgi teknolojisi uzmanları, donanım işlemleri ve bakım ekipleri ve genel IoT altyapısının doğru davranışını izleyen etki alanı uzmanlarından oluşan şirket içi bir ekip tarafından yapılabilir.

## <a name="understand-automatic-device-management-for-configuring-iot-devices-at-scale"></a>IoT aygıtlarını ölçekte yapılandırmak için otomatik aygıt yönetimini anlama

Otomatik cihaz yönetimi, bulut ve aygıtlar arasında istenilen ve bildirilen durumları senkronize etmek için [aygıt ikizleri](iot-hub-devguide-device-twins.md) ve [modül ikizlerinin](iot-hub-devguide-module-twins.md) birçok avantajını içerir. [Otomatik aygıt yapılandırmaları](iot-hub-auto-device-config.md) büyük ikiz kümelerini otomatik olarak günceller ve ilerleme ve uyumluluğu özetler. Aşağıdaki üst düzey adımlar, otomatik aygıt yönetiminin nasıl geliştirildiğini ve kullanıldığını açıklar:

* **IoT donanım üreticisi/entegratörü,** [aygıt ikizlerini](iot-hub-devguide-device-twins.md)kullanarak gömülü bir uygulama içinde aygıt yönetimi özelliklerini uygular. Bu özellikler firmware güncelleştirmeleri, yazılım yükleme ve güncelleştirme ve ayar yönetimi içerebilir.

* **IoT çözüm** geliştiricisi, [aygıt ikizleri](iot-hub-devguide-device-twins.md) ve [otomatik aygıt yapılandırmalarını](iot-hub-auto-device-config.md)kullanarak aygıt yönetimi işlemlerinin yönetim katmanını uygular. Çözüm, aygıt yönetimi görevlerini gerçekleştirmek için bir operatör arabirimi tanımlamayı içermelidir.

* **IoT çözüm operatörü,** aygıt yönetimi görevlerini gerçekleştirmek için IoT çözümünü kullanır, özellikle aygıtları bir araya getirmek, firmware güncelleştirmeleri gibi yapılandırma değişiklikleri başlatmak, ilerlemeyi izlemek ve ortaya çıkan sorunları gidermek için.

## <a name="iot-hardware-manufacturerintegrator"></a>IoT donanım üreticisi/entegratörü

Gömülü yazılım geliştirme ile ilgili donanım üreticileri ve entegratörleri için en iyi uygulamalar şunlardır:

* ** [Cihaz ikizleri uygulayın:](iot-hub-devguide-device-twins.md)** Aygıt ikizleri buluttan istenilen yapılandırmayı eşitlemeve geçerli yapılandırma ve aygıt özelliklerini raporlamayı sağlar. Gömülü uygulamalar da cihaz ikizleri uygulamak için en iyi yolu [Azure IoT SDKs](https://github.com/Azure/azure-iot-sdks)geçer. Aygıt ikizleri yapılandırma için en uygun olanlardır, çünkü aşağıdakiler:

    * Çift yönlü iletişimi destekleyin.
    * Hem bağlı hem de bağlantısı kesilen aygıt durumları için izin verin.
    * Nihai tutarlılık ilkesine uyun.
    * Bulutta tamamen sorgulanabilir.

* **Aygıt yönetimi için aygıtı ikiz yap::** Aygıt ikizi, aygıt yönetim özelliklerinin mantıksal olarak bölümler halinde gruplandırılması gibi yapılandırılmalıdır. Bunu yapmak, yapılandırma değişikliklerinin ikizin diğer bölümlerini etkilemeden yalıtılmasını sağlar. Örneğin, firmware için istenilen özellikler içinde bir bölüm, yazılım için başka bir bölüm ve ağ ayarları için üçüncü bir bölüm oluşturun. 

* **Aygıt yönetimi için yararlı olan aygıt özniteliklerini bildirin:** Fiziksel aygıt yapma ve modelleme, firmware, işletim sistemi, seri numarası ve diğer tanımlayıcılar gibi öznitelikler raporlama ve yapılandırma değişiklikleri hedefleme parametreleri için yararlıdır.

* **Raporlama durumu ve ilerleme için ana durumları tanımlayın:** Üst düzey durumları operatöre bildirilebilmek için numaralandırılmalıdır. Örneğin, bir firmware güncelleştirmesi geçerli, karşıdan yükleme, uygulama, devam etme ve hata durumunu bildirir. Her durum hakkında daha fazla bilgi için ek alanlar tanımlayın.

## <a name="iot-solution-developer"></a>IoT çözüm geliştiricisi

Azure tabanlı sistemler oluşturan IoT çözüm geliştiricileri için en iyi uygulamalar şunlardır:

* ** [Cihaz ikizleri uygulayın:](iot-hub-devguide-device-twins.md)** Aygıt ikizleri buluttan istenilen yapılandırmayı eşitlemeve geçerli yapılandırma ve aygıt özelliklerini raporlamayı sağlar. Bulut çözümleri uygulamalarında aygıt ikizlerini uygulamanın en iyi yolu [Azure IoT SDK'larıdır.](https://github.com/Azure/azure-iot-sdks) Aygıt ikizleri yapılandırma için en uygun olanlardır, çünkü aşağıdakiler:

    * Çift yönlü iletişimi destekleyin.
    * Hem bağlı hem de bağlantısı kesilen aygıt durumları için izin verin.
    * Nihai tutarlılık ilkesine uyun.
    * Bulutta tamamen sorgulanabilir.

* **Aygıt ikiz etiketlerini kullanarak cihazları düzenleme:** Çözüm, operatörün kanarya gibi çeşitli dağıtım stratejilerine dayalı olarak kaliteli halkaları veya diğer aygıt kümelerini tanımlamasına olanak tanır. Aygıt organizasyonu, çözümünüzün içinde aygıt ikiz etiketleri ve [sorguları](iot-hub-devguide-query-language.md)kullanılarak uygulanabilir. Aygıt organizasyonu, yapılandırma nın güvenli ve doğru bir şekilde kullanıma sunulmasıiçin gereklidir.

* ** [Otomatik cihaz yapılandırmalarını uygulayın:](iot-hub-auto-device-config.md)** Otomatik aygıt yapılandırmaları, aygıt ikizleri aracılığıyla büyük IoT aygıtları kümelerinde yapılandırma değişikliklerini dağıtır ve izler.

   Otomatik aygıt yapılandırmaları, aygıt ikizi etiketleri veya bildirilen özelliklerdeki bir sorgu olan **hedef koşulu** yla aygıt ikizkümelerini hedefler. **Hedef içerik,** hedeflenen aygıt ikizleri içinde ayarlanacak istenilen özellikler kümesidir. Hedef içerik, IoT donanım üreticisi/entegratörü tarafından tanımlanan aygıt ikiz yapısıyla uyumlu olmalıdır. **Ölçümler,** aygıt ikizi bildirilen özelliklerindeki sorgulardır ve Ayrıca IoT donanım üreticisi/entegratör tarafından tanımlanan aygıt ikiz yapısıyla uyumlu olmalıdır.

   Otomatik aygıt yapılandırmaları, yapılandırma oluşturulduktan kısa bir süre sonra ilk kez ve beş dakikalık aralıklarla çalışır. Ayrıca, IoT Hub'ın aygıt ikizi işlemleri, aygıt ikizokumaları ve güncellemeleri için [azaltma sınırlarını](iot-hub-devguide-quotas-throttling.md) asla aşmayacak bir oranda gerçekleştirmesinden de yararlanır.

* **Cihaz [Sağlama Hizmetini](../iot-dps/how-to-manage-enrollments.md)Kullanın :** Çözüm geliştiricileri, cihaz ikiz etiketlerini yeni aygıtlara atamak için Cihaz Sağlama Hizmetini kullanmalıdır, ancak bu etikete sahip ikizleri hedefleyen otomatik aygıt **yapılandırmaları** tarafından otomatik olarak yapılandırılır. 

## <a name="iot-solution-operator"></a>IoT çözüm operatörü

Azure'da oluşturulmuş bir IoT çözümüsyonu kullanan IoT çözüm operatörleri için en iyi uygulamalar şunlardır:

* **Yönetim için cihazları düzenleme:** IoT çözümü, kanarya gibi çeşitli dağıtım stratejilerine dayalı kalite halkaları veya diğer aygıt kümelerinin oluşturulmasını tanımlamalı veya buna izin vermelidir. Aygıt kümeleri, yapılandırma değişikliklerini kullanıma çıkarmak ve diğer ölçekdüzeyinde aygıt yönetimi işlemlerini gerçekleştirmek için kullanılır.

* **Aşamalı bir kullanıma sunulmasını kullanarak yapılandırma değişikliklerini gerçekleştirin:**  Aşamalı kullanıma alma, bir işleç tarafından ioT aygıtlarının genişletilmesi kümesine değişiklikler dağıttığı genel bir işlemdir. Amaç, geniş ölçekli kırma değişiklikleri yapma riskini azaltmak için kademeli olarak değişiklik yapmaktır.İşletici, otomatik bir [aygıt yapılandırması](iot-hub-auto-device-config.md) oluşturmak için çözümün arabirimini kullanmalı ve hedefleme koşulu ilk aygıt kümesini (kanarya grubu gibi) hedeflemelidir. İşletici daha sonra ilk aygıt kümesindeki yapılandırma değişikliğini doğrulamalıdır.

   Doğrulama tamamlandıktan sonra, operatör otomatik aygıt yapılandırmasını daha büyük bir aygıt kümesi içerecek şekilde günceller. İşletici ayrıca yapılandırmanın şu anda bu aygıtlara hedeflenen diğer yapılandırmalardan daha yüksek olması için önceliği belirlemelidir. Kullanıma alma, otomatik aygıt yapılandırması tarafından bildirilen ölçümler kullanılarak izlenebilir.

* **Hata veya yanlış yapılandırma durumunda geri alma işlemleri gerçekleştirin:**  Aygıtların artık hedefleme koşulunu karşılayaması için hedefleme **koşulunu** değiştirerek hatalara veya yanlış yapılandırmalara neden olan otomatik aygıt yapılandırması geri alınabilir. Bu aygıtlar için daha düşük öncelikli başka bir otomatik aygıt yapılandırmasının hala hedefli olduğundan emin olun. Geri alma nın ölçümleri görüntüleyerek başarılı olduğunu doğrulayın: Geri alma yapılandırması artık hedeflenmemiş aygıtlar için durum göstermemeli ve ikinci yapılandırmanın ölçümleri artık hedeflenen aygıtların sayımlarını içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

* [IoT Hub'da aygıt ikizleri anlama ve kullanma](iot-hub-devguide-device-twins.md)da cihaz ikizleri uygulama hakkında bilgi edinin.

* [IoT aygıtlarını yapılandırVe ölçekte izleyerek](iot-hub-auto-device-config.md)otomatik bir aygıt yapılandırması oluşturmak, güncelleştirmek veya silmek için adımları gözden geçirin.

* [Öğretici: Bir aygıt firmware güncelleştirme işlemi uygulayın, aygıt](tutorial-firmware-update.md)ikizleri ve otomatik aygıt yapılandırmaları kullanarak bir firmware güncelleştirme deseni uygulayın.
