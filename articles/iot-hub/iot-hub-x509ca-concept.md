---
title: Azure IoT Hub X.509 güvenlik kavramları | Microsoft Dokümanlar
description: Kavram - IoT cihaz üretiminde X.509 sertifika yetkilisi sertifikaları değerini anlamak ve kimlik doğrulama.
author: eustacea
manager: arjmands
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/18/2017
ms.author: eustacea
ms.openlocfilehash: 3c7e1167b3326620863d35cb2d4b07235cbd5517
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61320483"
---
# <a name="conceptual-understanding-of-x509-ca-certificates-in-the-iot-industry"></a>IoT endüstrisinde X.509 CA sertifikalarının kavramsal olarak anlaşılması

Bu makalede, IoT aygıt üretiminde ve IoT Hub'a kimlik doğrulamasında X.509 sertifika yetkilisi (CA) sertifikaları kullanmanın değeri açıklanmaktadır. Tedarik zinciri kurulumu ve vurgulama avantajları hakkında bilgiler içerir.

Bu makalede açıklanır:

* X.509 CA sertifikaları nelerdir ve nasıl alınır?

* X.509 CA sertifikanızı IoT Hub'a kaydetme

* X.509 CA tabanlı kimlik doğrulama için üretim tedarik zinciri nasıl kurulur?

* X.509 CA ile imzalanan aygıtlar IoT Hub'a nasıl bağlanır?

## <a name="overview"></a>Genel Bakış

X.509 Sertifika Yetkilisi (CA) kimlik doğrulaması, tedarik zincirinde aygıt kimlik oluşturmayı ve yaşam döngüsü yönetimini önemli ölçüde kolaylaştıran bir yöntem kullanarak aygıtları IoT Hub'a doğrulayan bir yaklaşımdır.

X.509 CA kimlik doğrulaması ayırt edici bir özniteliği, BIR CA sertifikasının akış aşağı aygıtlarıyla olan bire bir ilişkisidir. Bu ilişki, x.509 CA sertifikasını bir kez kaydederek herhangi bir sayıda cihazın IoT Hub'a kaydedilmesini sağlar, aksi takdirde aygıtın bağlanabilmesi için cihaza özgü sertifikaların her cihaz için önceden kaydedilmesi gerekir. Bu bir-çok ilişkisi, aygıt sertifikalarıyaşam döngüsü yönetimi işlemlerini de kolaylaştırır.

X.509 CA kimlik doğrulaması bir diğer önemli özelliği tedarik zinciri lojistik basitleştirilmesidir. Aygıtların güvenli kimlik doğrulaması, her aygıtın güven temeli olarak anahtar gibi benzersiz bir sır saklaması gerektirir. Sertifika tabanlı kimlik doğrulamasında bu gizli özel bir anahtardır. Tipik bir cihaz üretim akışı birden çok adım ve vasiiçerir. Aygıt özel anahtarlarını birden çok vasi arasında güvenli bir şekilde yönetmek ve güveni korumak zor ve pahalıdır. Sertifika yetkililerinin kullanılması, her veliyi aygıt özel anahtarlarını emanet etmek yerine bir şifreleme güven zincirine imzalayarak bu sorunu çözer. Sırayla her vasi üretim akışının kendi süreç adımında cihazları işaretler. Genel sonuç, kriptografik güven zincirinin kullanımı yoluyla yerleşik hesap verebilirliğe sahip optimum bir tedarik zinciridir. Aygıtlar benzersiz özel anahtarlarını koruduğunda bu işlemin en fazla güvenliği verdiğini belirtmekte yarar vardır. Bu amaçla, donanım güvenli modülleri (HSM) dahili gün ışığı görmek asla özel anahtarlar oluşturma yeteneğine sahip kullanılmasını çağırıyoruz.

Bu makalede, x.509 CA kimlik doğrulaması kullanarak uçtan uca bir görünüm sunar, tedarik zinciri kurulumundan aygıt bağlantısına kadar, anlayışı sağlamlaştırmak için gerçek bir dünya örneğinden yararlanırken.

## <a name="introduction"></a>Giriş

X.509 CA sertifikası, sahibi diğer sertifikaları imzalayabilen dijital bir sertifikadır. Bu dijital sertifika X.509'dur, çünkü IETF'nin RFC 5280 standardı tarafından öngörülen bir sertifika biçimlendirme standardına uygundur ve sahibi diğer sertifikaları imzalayabildiği için bir sertifika yetkilisidir (CA).

X.509 CA kullanımı en iyi somut bir örnek ile ilgili olarak anlaşılmaktadır. Profesyonel kurulum için tasarlanmış Smart-X-Widget üreticisi Company-X'i düşünün. Company-X hem üretim hem de kurulum kaynaklarını dış kaynakolarak karşılar. Bu smart-x-widget'lar üretmek için üretici Factory-Y sözleşmeleri ve servis sağlayıcı Teknisyen-Z yüklemek için. Şirket-X, Smart-X-Widget'ın kurulum için doğrudan Factory-Y'den Technician-Z'ye bağlanmasını ve şirket-X'in başka bir müdahalesi olmaksızın kurulumdan sonra Company-X'in IoT Hub örneğine doğrudan bağlanmasını arzu eder. Bunun gerçekleşmesi için, Şirket-X'in otomatik bağlantı için Smart-X-Widget'ı prime'e getirmek için birkaç tek seferlik kurulum işlemlerini tamamlaması gerekir. Uçtan uca senaryo göz önünde bulundurularak, bu makalenin geri kalanı aşağıdaki gibi yapılandırılır:

* X.509 CA sertifikasını edinin

* X.509 CA sertifikasını IoT Hub'a kaydedin

* Aygıtları sertifika güven zincirinde imzalama

* Cihaz bağlantısı

## <a name="acquire-the-x509-ca-certificate"></a>X.509 CA sertifikasını edinin

Company-X, bir genel kök sertifika yetkilisinden X.509 CA sertifikası satın alma veya kendi imzalanmış bir işlem yoluyla sertifika oluşturma seçeneğine sahiptir. Bir seçenek, uygulama senaryosuna bağlı olarak diğerine göre en uygun seçenek tir. Seçenek ne olursa olsun, işlem iki temel adım gerektirir, bir ortak /özel anahtar çifti oluşturma ve ortak anahtarı bir sertifika ya da imzalayarak.

![X509CA sertifikaları oluşturmak için akış](./media/iot-hub-x509ca-concept/csr-flow.png)

Bu adımların nasıl gerçekleştirilene ilişkin ayrıntılar çeşitli hizmet sağlayıcılarla farklılık gösterir.

### <a name="purchasing-an-x509-ca-certificate"></a>X.509 CA sertifikası satın alma

CA sertifikası satın almak, aygıtlar bağlandığında IoT aygıtlarının meşruiyetine kefil olmak için güvenilir bir üçüncü taraf olarak iyi bilinen bir kök CA eylemine sahip olmanın avantajına sahiptir. Şirket-X, Smart-X-Widget'ın IoT Hub'a ilk bağlantıdan sonra üçüncü taraf ürün veya hizmetlerle etkileşimde olmasını istiyorlarsa bu seçeneği seçer.

X.509 CA sertifikası satın almak için, Company-X bir kök sertifika hizmetleri sağlayıcısı seçer. 'Root CA' deyimi için bir internet araması iyi müşteri adayları verecektir. CA kökü, Company-X'e ortak/özel anahtar çiftinin nasıl oluşturulacağı ve hizmetleri için sertifika imzalama isteğinin (CSR) nasıl oluşturulacağı konusunda rehberlik edecektir. CSR, sertifika yetkilisinden sertifika başvurusu için resmi bir işlemdir. Bu satın alma nın sonucu, yetki belgesi olarak kullanılmak üzere bir sertifikadır. X.509 sertifikalarının her yerde olması göz önüne alındığında, sertifikanın IETF'nin RFC 5280 standardına uygun şekilde biçimlendirilmiş olması muhtemeldir.

### <a name="creating-a-self-signed-x509-ca-certificate"></a>Kendi İmzalı X.509 CA sertifikası oluşturma

Kendi Kendine İmzalanmış X.509 CA sertifikası oluşturma işlemi, kök sertifika yetkilisi gibi üçüncü taraf imzalayan bir sertifikayı dahil etmek dışında satın almaya benzer. Örneğimizde, Şirket-X, kök sertifika yetkilisi yerine yetki sertifikasını imzalar. Şirket-X, bir yetki sertifikası satın almaya hazır olana kadar test etmek için bu seçeneği seçebilir. Şirket-X, Smart-X-Widget'ın IoT Hub dışındaki herhangi bir üçüncü taraf hizmetine bağlanması amaçlanmamışsa, üretimde kendi imzalanmış bir X.509 CA sertifikası da kullanabilir.

## <a name="register-the-x509-certificate-to-iot-hub"></a>X.509 sertifikasını IoT Hub'a kaydedin

Şirket-X'in X.509 CA'yı IoT Hub'a kaydetmesi gerekir ve burada akıllı x widget'ları bağlanırken kimlik doğrulaması yapacaktır. Bu, herhangi bir sayıda Smart-X-Widget aygıtının kimliğini doğrulamave yönetme olanağı sağlayan tek seferlik bir işlemdir. Bu işlem, yetki sertifikası ve aygıtlar arasındaki bir-çok ilişkisi nedeniyle bir kereliktir ve x.509 CA kimlik doğrulama yöntemini kullanmanın başlıca avantajlarından birini oluşturur. Alternatif, her Smart-X-Widget cihazı için ayrı ayrı sertifika parmak izleri yüklemek ve böylece operasyonel maliyetlere katkıda bulunmaktadır.

X.509 CA sertifikasının kaydedilmesi iki aşamalı bir işlemdir, sertifika yüklemesi ve sertifika kanıtıdır.

![X509CA sertifikası kaydetme](./media/iot-hub-x509ca-concept/pop-flow.png)

### <a name="x509-ca-certificate-upload"></a>X.509 CA Sertifikası Yükleme

X.509 CA sertifika yükleme işlemi, CA sertifikasını IoT Hub'a yükleyin. IoT Hub sertifikayı bir dosyada bekler. Company-X sadece sertifika dosyasını yükler. Sertifika dosyası hiçbir koşulda herhangi bir özel anahtar içermemelidir. Kamu Anahtar Altyapısı (PKI) ile ilgili standartlardaki en iyi uygulamalar, bu durumda Şirket-X'in özel bilgisinin yalnızca Company-X'te yer almalarını zorunlu kılmalıdır.

### <a name="proof-of-possession-of-the-certificate"></a>Sertifikanın Sahip Olduğu Kanıt

X.509 CA sertifikası, herhangi bir dijital sertifika gibi, gizlice dinlemeye açık olan genel bilgilerdir. Bu nedenle, bir kulak misafiri bir sertifikanın yolunu kesebilir ve sertifikayı kendi sertifikaları olarak yüklemeyi deneyebilir. Örneğimizde, IoT Hub CA sertifikası Company-X'in gerçekten Company-X'e ait olduğundan emin olmak ister. Bunu şirket-X'e meydan okuyarak, aslında sertifikaya [sahip](https://tools.ietf.org/html/rfc5280#section-3.1)olduklarını kanıtlamaya çalışır. Sahip olma kanıtı akışı, IoT Hub'ın özel anahtarını kullanarak Şirket-X tarafından imzalanacak rastgele bir sayı oluşturmasını gerektirir. Eğer Şirket-X, PKI'nın en iyi uygulamalarını takip ettiyse ve özel anahtarlarını koruduysa, o zaman yalnızca onlar sahip olma kanıtı sorununa doğru şekilde yanıt verecek konumda olurlar. IoT Hub, sahip olma kanıtı mücadelesinin başarılı bir yanıtı üzerine X.509 CA sertifikasını kaydetmeye devam eder.

IoT Hub'ın sahip olma kanıtı sorununa başarılı bir yanıt X.509 CA kaydını tamamlar.

## <a name="sign-devices-into-a-certificate-chain-of-trust"></a>Cihazları Sertifika Güven Zincirinde Oturum Aç

IoT, her aygıtın benzersiz bir kimliğe sahip olmasını gerektirir. Bu kimlikler, sertifika tabanlı kimlik doğrulama düzenleri için form sertifikalarında yer alıyor. Örneğimizde, bu her Smart-X-Widget benzersiz bir cihaz sertifikasına sahip olması gerektiği anlamına gelir. Şirket-X, tedarik zincirinde bunun için nasıl kurulum yapar?

Bu konuda gitmek için bir yolu Smart-X-Widgets için önceden sertifikalar oluşturmak ve tedarik zinciri ortakları ile ilgili benzersiz cihaz özel anahtarları bilgi emanet etmektir. Şirket-X için bu, Fabrika-Y ve Teknisyen-Z'ye emanet anlamına gelir. Bu geçerli bir yöntem olmakla birlikte, aşağıdaki gibi güven sağlamak için aşılması gereken zorluklarla birlikte gelir:

1. Özel anahtarları asla paylaşmamak için PKI'nın en iyi uygulamalarını göz ardı etmenin yanı sıra, cihaz özel anahtarlarını tedarik zinciri ortaklarıyla paylaşmak zorunda kalmak, tedarik zincirine güven inşa etmeyi pahalıhale getirir. Bu, güvenli odalar gibi güvenli sistemler den cihaz özel anahtarları nın bulunduğu anlamına gelir ve periyodik güvenlik denetimleri gibi işlemlerin yüklenmesi gerekir. Her ikisi de tedarik zincirine maliyet ekler.

2. Tedarik zincirindeki aygıtlar için güvenli bir şekilde muhasebeve daha sonra dağıtımda bunları yönetmek, aygıta özgü sertifika (dolayısıyla özel anahtar) oluşturma noktasından aygıt emekliliğine kadar her anahtardan cihaza çift için bire bir görev haline gelir. Bu, grup kavramı bir şekilde sürece açıkça yerleşik olmadıkça cihazların grup yönetimini engellemez. Güvenli muhasebe ve cihaz yaşam döngüsü yönetimi, bu nedenle, ağır bir işlem yükü haline gelir. Örneğimizde, Şirket-X bu yükü taşıyacak.

X.509 CA sertifikası kimlik doğrulaması, sertifika zincirleri nin kullanımı yoluyla listelenen sorunlara zarif çözümler sunar. Bir ca'nın bir ara CA imzalamasından elde edilen sertifika zinciri, başka bir ara CA imzalar ve böylece son bir ara CA aygıtı imzalayana kadar devam eder. Örneğimizde, Company-X fabrika-y işaretleri, hangi sırayla sonunda Smart-X-Widget işaretleri Technician-Z işaretleri.

![Sertifika zinciri hiyerarşisi](./media/iot-hub-x509ca-concept/cert-chain-hierarchy.png)

Zincirdeki sertifikaların yukarıda mantıksal teslimi yetki sunar. Birçok tedarik zinciri, her ara CA'nın tüm upstream CA sertifikalarını alırken zincire imza attığı ve son ara CA'nın sonunda her aygıtı imzaladığı ve zincirdeki tüm yetki sertifikalarını enjekte ettiği bu mantıksal teslimi takip eder cihaza. Bu, fabrikalar hiyerarşisi ile sözleşme üretim şirketi üretim yapmak için belirli bir fabrika komisyonları zaman yaygındır. Hiyerarşi birkaç düzeyde derin olabilir (örneğin, coğrafya / ürün türü / üretim hattı), sadece fabrika sonunda cihaz ile etkileşim alır ama zincir hiyerarşinin üstünden korunur.

Alternatif zincirlerin cihazla etkileşimi farklı ara CA'ya sahip olabilir ve bu durumda CA'nın cihazla etkileşimi bu noktada sertifika zinciri içeriği enjekte eder. Hibrit modeller, CA'nın yalnızca bir kısmının cihazla fiziksel etkileşime sahip olduğu durumlarda da mümkündür.

Örneğimizde, hem Factory-Y hem de Technician-Z Smart-X-Widget ile etkileşime girebedilmektedir. Company-X Smart-X-Widget sahibi iken, aslında fiziksel olarak tüm tedarik zinciri ile etkileşim değildir. Bu nedenle Smart-X-Widget için güven sertifikası zinciri, Company-X imzası olan Factory-Y'yi kapsamaktadır ve bu da Daha sonra Smart-X-Widget'a son imzayı verecek olan Technician-Z'yi imzalar. Smart-X-Widget'ın üretimi ve kurulumu, her Smart-X-Widget'ı imzalamak için kendi ara CA sertifikalarını kullanarak Factory-Y ve Technician-Z'yi kapsar. Tüm bu sürecin sonucu, benzersiz cihaz sertifikalarına ve Şirket-X CA sertifikasına kadar giden sertifika güven zincirine sahip Smart-X-Widget'larıdır.

![Bir şirketin sertifikalarından başka bir şirketin sertifikalarına güven zinciri](./media/iot-hub-x509ca-concept/cert-mfr-chain.png)

Bu, X.509 CA yönteminin değerini gözden geçirmek için iyi bir noktadır. Her Smart-X-Widget için ön üretim ve sertifikaları tedarik zincirine teslim etmek yerine, Company-X'in Factory-Y'yi yalnızca bir kez imzalaması gerekiyordu. Şirket-X, cihazın kullanım ömrü boyunca her cihazı izlemek yerine, bir yılın Temmuz ayından sonra Teknisyen-Z tarafından yüklenen cihazlar gibi, tedarik zinciri sürecinden doğal olarak çıkan gruplar aracılığıyla cihazları izleyebilir ve yönetebilir.

Son olarak, CA kimlik doğrulama yöntemi, cihaz üretim tedarik zincirine güvenli hesap verebilirliği aşılar. Sertifika zinciri işlemi nedeniyle, zincirdeki her üyenin eylemleri şifreleme olarak kaydedilir ve doğrulanabilir.

Bu işlem, bütünlük için su yüzüne çıkması gereken bazı varsayımlara dayanır. Aygıtbenzersiz ortak/özel anahtar çiftinin bağımsız oluşturulmasını ve özel anahtarın aygıt içinde korunmasını gerektirir. Neyse ki, donanım güvenli modülleri şeklinde güvenli silikon yongaları (HSM) dahili anahtarlar üreten ve özel tuşları koruma yeteneğine sahip var. Şirket-X'in bu tür fişlerden yalnızca smart-x-widget'ın bileşen malzeme faturasına eklemesi gerekir.

## <a name="device-connection"></a>Cihaz Bağlantısı

Yukarıdaki önceki bölümlerde aygıt bağlantısı kadar bina olmuştur. X.509 CA sertifikasını Bir kez IoT Hub'a kaydederek, potansiyel olarak milyonlarca cihaz ilk andan itibaren nasıl bağlanır ve kimlik doğrulaması olur?  Basit; x.509 CA sertifikasını kaydederken daha önce karşılaştığımız aynı sertifika yükleme ve sahip kanıtı akışı sayesinde.

X.509 CA kimlik doğrulaması için üretilen cihazlar, cihaza özgü sertifikalar ve kendi üretim tedarik zincirinden bir sertifika zinciri ile donatılmıştır. Cihaz bağlantısı, ilk kez bile iki aşamalı bir işlemle gerçekleşir: sertifika zinciri yüklemeve bulundurma kanıtı.

Sertifika zinciri yüklemesi sırasında cihaz, içinde bulunan sertifika zinciriyle birlikte cihaza özgü sertifikasını IoT Hub'a yükler. Önceden kaydedilmiş X.509 CA sertifikasını kullanan IoT Hub, yüklenen sertifika zincirinin dahili olarak tutarlı olduğu ve zincirin X.509 CA sertifikasının geçerli sahibi tarafından kaynaklandığı birkaç şeyi şifreleme olarak doğrulayabilir. Sadece X.509 CA kayıt işlemi ile oldu, IoT Hub zincir ve dolayısıyla cihaz sertifikası aslında yüklenen cihaza ait olduğunu tespit etmek için bir kanıt-sahip sorun-yanıt süreci başlatacak. Bunu, IoT Hub tarafından doğrulama için özel anahtarını kullanarak aygıt tarafından imzalanacak rasgele bir meydan okuma oluşturarak yapar. Başarılı bir yanıt, IoT Hub'ın aygıtı özgün olarak kabul etmesini ve bağlantı vermesini sağlar.

Örneğimizde, her Smart-X-Widget cihaza özgü sertifikasını Factory-Y ve Technician-Z X.509 CA sertifikalarıile birlikte yükler ve ardından IoT Hub'ın sahip kanıtı mücadelesine yanıt verir.

![Bir sertifikadan diğerine akış, hub'dan pop meydan okuma](./media/iot-hub-x509ca-concept/device-pop-flow.png)

Güvenin temelinin cihaz özel anahtarları da dahil olmak üzere özel anahtarları korumaya dayandığına dikkat edin. Bu nedenle, donanım özel anahtarları korumak için Donanım Güvenli Modülleri (HSM) şeklinde güvenli silikon yongaları önemini yeterince vurgulamak değil, ve herhangi bir özel tuşları paylaşan asla genel en iyi uygulama, bir fabrika ile başka emanet gibi özel anahtar.