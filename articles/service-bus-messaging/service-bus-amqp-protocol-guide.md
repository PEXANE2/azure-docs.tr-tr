---
title: Azure Hizmet Veri Ve Etkinlik Veri Hizmetleri ve Etkinlik Hub'ları protokol kılavuzunda AMQP 1.0 | Microsoft Dokümanlar
description: Azure Hizmet Veri Ve Hizmet Veri Hizmetleri ve Etkinlik Hub'larında AMQP 1.0 ifadeleri ve açıklamaları için protokol kılavuzu
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: d706e9b3351b0693a1f352e15b6b9b0cc5c7a65d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77086164"
---
# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>Azure Hizmet Veri Ve Etkinlik Veri Hizmetleri ve Etkinlik Hub'ları protokol kılavuzunda AMQP 1.0

Gelişmiş İleti Sıraya Protokolü 1.0, iki taraf arasında eşzamanlı, güvenli ve güvenilir bir şekilde ileti aktarımı için standart laştırılmış çerçeveleme ve aktarım protokolüdür. Azure Servis Veri Servisi Mesajlaşma ve Azure Etkinlik Hub'larının birincil protokolüdür. Her iki hizmet de HTTPS'yi destekler. Ayrıca desteklenen tescilli SBMP protokolü AMQP lehine aşamalı olarak yürütülüyor.

AMQP 1.0, Microsoft ve Red Hat gibi ara yazılım satıcılarını bir araya getiren ve finansal hizmetler sektörünü temsil eden JP Morgan Chase gibi birçok mesajlaşma aracı kullanıcısıyla birlikte gelen geniş endüstri işbirliğinin bir sonucudur. AMQP protokolü ve genişletme spesifikasyonları için teknik standardizasyon forumu OASIS'tir ve ISO/IEC 19494 olarak uluslararası bir standart olarak resmi onay almıştır.

## <a name="goals"></a>Hedefler

Bu makale, ŞU anda OASIS AMQP teknik komitesinde kesinleşen küçük bir taslak uzatma belirtimi kümesiyle birlikte AMQP 1.0 mesajlaşma belirtiminin temel kavramlarını kısaca özetler ve Azure Hizmet Veri Tos'un nasıl bu özellikleri uygular ve üzerine inşa eder.

Amaç, herhangi bir platformda mevcut AMQP 1.0 istemci yığınını kullanan herhangi bir geliştiricinin, AMQP 1.0 üzerinden Azure Hizmet Veri Servisi ile etkileşim kurabilmesidir.

Apache Proton veya AMQP.NET Lite gibi genel amaçlı AMQP 1.0 yığınları, zaten tüm çekirdek AMQP 1.0 protokollerini uygular. Bu temel jestler bazen daha üst düzey bir API ile sarılır; Apache Proton bile iki sunuyor, zorunlu Messenger API ve reaktif Reaktör API.

Aşağıdaki tartışmada, AMQP bağlantılarının, oturumların ve bağlantıların yönetimi ile çerçeve aktarımlarının ve akış kontrolünün işlenmesinin ilgili yığın (Apache Proton-C gibi) tarafından ele alındığını ve herhangi bir özel dikkat uygulama geliştiricilerden. Biz soyut bağlanma yeteneği gibi birkaç API ilkel varlığını varsayalım, ve *gönderen* ve *alıcı* soyutlama nesneleri, daha sonra `send()` sırasıyla bazı şekil ve `receive()` işlemler var bir form oluşturmak için.

Azure Hizmet Veri Servisi'nin ileti tarama veya oturumların yönetimi gibi gelişmiş yetenekleri tartışılırken, bu özellikler AMQP açısından açıklanır, ancak aynı zamanda bu varsayılan API soyutlamanın üzerine katmanlı bir sözde uygulama olarak da açıklanır.

## <a name="what-is-amqp"></a>AMQP nedir?

AMQP bir çerçeveleme ve aktarım protokolüdür. Çerçeveleme, ağ bağlantısının her iki yönünde de akan ikili veri akışları için yapı sağladığı anlamına gelir. Yapı, bağlı taraflar arasında değiş tokuş edilecek *çerçeveler*olarak adlandırılan farklı veri blokları için deliasyon sağlar. Aktarım yetenekleri, her iki iletişim tarafının da çerçevelerin ne zaman aktarılacak ve aktarımların ne zaman tamamlanabileceği konusunda ortak bir anlayış oluşturmasını sağlıyor.

Hala birkaç ileti aracıcısı tarafından kullanılmakta olan AMQP çalışma grubu tarafından üretilen daha önce süresi dolmuş taslak sürümlerin aksine, çalışma grubunun nihai ve standart LAŞTıRıLMıŞ AMQP 1.0 protokolü, bir mesaj komisyoncusunun veya belirli bir topolojinin varlığını önalmaz. bir ileti aracısı içinde varlıklar.

Protokol, azure servis veri tos'un yaptığı gibi, kuyrukları destekleyen ileti aracıları ve yayımlama/abone varlıklarıyla etkileşim için simetrik eşler arası iletişim için kullanılabilir. Azure Etkinlik Hub'larında olduğu gibi etkileşim desenlerinin normal kuyruklardan farklı olduğu ileti altyapısıyla etkileşim için de kullanılabilir. Olay Hub'ı, olaylar gönderildiğinde bir sıra gibi davranır, ancak olaylar okunduğunda daha çok seri depolama hizmeti gibi davranır; biraz bir teyp sürücü benzer. İstemci kullanılabilir veri akışına bir ofset seçer ve daha sonra bu ofset en son kullanılabilir tüm olaylar servis edilir.

AMQP 1.0 protokolü genişletilebilir olacak şekilde tasarlanmıştır ve daha fazla belirtimin yeteneklerini geliştirmesine olanak sağlar. Bu belgede tartışılan üç uzantı belirtimi bunu göstermektedir. Varolan HTTPS/WebSockets altyapısı üzerinden iletişim için, yerel AMQP TCP bağlantı noktalarını yapılandırmak zor olabilir. Bağlama belirtimi, WebSockets üzerinde AMQP katmanı nasıl tanımlar. İleti altyapısıyla yönetim amacıyla bir istek/yanıt şeklinde etkileşim de bulunmak veya gelişmiş işlevsellik sağlamak için, AMQP yönetim belirtimi gerekli temel etkileşim ilkellerini tanımlar. Federe yetkilendirme modeli tümleştirmesi için, AMQP talep tabanlı güvenlik belirtimi, bağlantılarla ilişkili yetkilendirme belirteçlerinin nasıl ilişkilendirilen ve yenilenini tanımlar.

## <a name="basic-amqp-scenarios"></a>Temel AMQP senaryoları

Bu bölümde, amqp 1.0'ın Azure Hizmet Veri Servisi ile temel kullanımı açıklanmaktadır ve bunlar arasında bağlantılar, oturumlar ve bağlantılar oluşturulması ve iletilerin sıralar, konular ve abonelikler gibi Hizmet Veri Hizmetleri varlıklarına ve bu kuruluşlardan aktarılması yer almaktadır.

AMQP'nin nasıl çalıştığı hakkında bilgi edinmek için en yetkili kaynak AMQP 1.0 belirtimidir, ancak belirtim, protokolü öğretmek için değil, tam olarak uygulamaya rehberlik etmek için yazılmıştır. Bu bölümde, Servis Veri Tos'un AMQP 1.0'ı nasıl kullandığını açıklamak için gerektiği kadar terminoloji nin getirilmesi üzerinde duruluyor. AMQP daha kapsamlı bir giriş için, hem de AMQP 1.0 daha geniş bir tartışma için, [bu video ders][this video course]gözden geçirebilirsiniz.

### <a name="connections-and-sessions"></a>Bağlantılar ve oturumlar

AMQP iletişim programları *kapsayıcıları*çağırır; bu *düğümleri*içeren , bu kapların içinde iletişim varlıklarıdır. Kuyruk böyle bir düğüm olabilir. AMQP çoklama sağlar, böylece düğümler arasındaki birçok iletişim yolları için tek bir bağlantı kullanılabilir; örneğin, bir uygulama istemcisi aynı anda bir kuyruktan alabilir ve aynı ağ bağlantısı üzerinden başka bir kuyruğa gönderebilir.

![][1]

Ağ bağlantısı böylece kapsayıcıya sabitlenir. İstemci rolündeki kapsayıcı tarafından başlatılan ve gelen TCP bağlantılarını dinleyen ve kabul eden alıcı rolündeki bir kapsayıcıya giden TCP soketi bağlantısı yapılır. Bağlantı el sıkışması protokol sürümünün pazarlığını, Taşıma Düzeyi Güvenliği'nin (TLS/SSL) kullanımını bildirmeyi veya müzakere etmeyi ve SASL'yi temel alan bağlantı kapsamında kimlik doğrulama/yetkilendirme el sıkışmasını içerir.

Azure Servis Veri Yolunda her zaman TLS kullanımı gerektirir. TCP bağlantı noktası 5671 üzerindeki bağlantıları destekler, bu nedenle TCP bağlantısı amqp protokol el sıkışmasını girmeden önce tls ile kaplanır ve ayrıca Sunucunun zorunlu bir bağlantı yükseltmesi sunduğu TCP portu 5672 üzerindeki bağlantıları destekler AMQP-reçete edilen modeli kullanarak TLS'ye. AMQP WebSockets bağlama, TCP bağlantı noktası 443 üzerinde amqp 5671 bağlantılarına eşdeğer bir tünel oluşturur.

Bağlantıyı ve TLS'yi kurduktan sonra, Servis Veri Servisi iki SASL mekanizması seçeneği sunar:

* SASL PLAIN genellikle kullanıcı adı ve parola kimlik bilgilerini bir sunucuya geçirmek için kullanılır. Hizmet Veri Servisi'nin hesabı yoktur, ancak hakları veren ve bir anahtarla ilişkilendirilen [Paylaşılan Erişim Güvenliği kuralları](service-bus-sas.md)olarak adlandırılır. Bir kuralın adı kullanıcı adı olarak kullanılır ve anahtar (base64 kodlanmış metin olarak) parola olarak kullanılır. Seçilen kuralla ilişkili haklar, bağlantıda izin verilen işlemleri yönetir.
* SASL ANONYMOUS, istemci daha sonra açıklanan talep tabanlı güvenlik (CBS) modelini kullanmak istediğinde SASL yetkilendirmesini atlamak için kullanılır. Bu seçenekle, istemcinin yalnızca CBS bitiş noktasıyla etkileşimkurabileceği ve CBS el sıkışmasının tamamlanması gereken kısa bir süre için anonim olarak bir istemci bağlantısı kurulabilir.

Aktarım bağlantısı kurulduktan sonra, kapsayıcıların her biri işlemek istedikleri maksimum çerçeve boyutunu bildirir ve boşta zaman aladıktan sonra bağlantıda herhangi bir etkinlik yoksa tek taraflı olarak bağlantıyı keseceklerdir.

Ayrıca, kaç eşzamanlı kanalın desteklenedildiğini de bildirirler. Kanal, bağlantının üstünde ki tek yönlü, giden, sanal bir aktarım yoludur. Oturum, birbirine bağlı kapsayıcıların her birinden bir kanal alır ve çift yönlü bir iletişim yolu oluşturur.

Oturumlar pencere tabanlı akış kontrol modeline sahiptir; bir oturum oluşturulduğunda, her iki taraf da alma penceresine kaç kare kabul etmek istediğini bildirir. Taraflar çerçeveleri değiştirirken, aktarılan çerçeveler bu pencereyi doldurur ve pencere dolduğunda ve pencere *akış performatifi* kullanılarak sıfırlanana veya genişletilene kadar durur *(performans,* iki taraf arasında değiş tokuş edilen protokol düzeyindeki hareketler için AMQP terimidir).

Bu pencere tabanlı model kabaca tcp kavramına benzer pencere tabanlı akış denetimi, ancak soket içinde oturum düzeyinde. Protokolün birden fazla eşzamanlı oturuma izin verme kavramı, yüksek öncelikli trafiğin otoyol ekspres şeridinde olduğu gibi daraltılmış normal trafiği aşması için var.

Azure Hizmet Veri Servisi şu anda her bağlantı için tam olarak bir oturum kullanır. Servis Veri Kurumu maksimum çerçeve boyutu, Servis Veri Günü Standardı ve Etkinlik Hub'ları için 262.144 bayt (256-K bayt) dir. Service Bus Premium için 1.048.576 (1 MB) Service Bus belirli bir oturum düzeyinde azaltma penceresi dayatmaz, ancak bağlantı düzeyi akış denetiminin bir parçası olarak pencereyi düzenli olarak sıfırlar [(sonraki bölüme](#links)bakın).

Bağlantılar, kanallar ve oturumlar geçicidir. Temel bağlantı çökerse, bağlantılar, TLS tüneli, SASL yetkilendirme bağlamı ve oturumlar yeniden oluşturulmalıdır.

### <a name="amqp-outbound-port-requirements"></a>AMQP giden bağlantı noktası gereksinimleri

TCP üzerinden AMQP bağlantıları kullanan istemciler, yerel güvenlik duvarında 5671 ve 5672 bağlantı noktalarının açılmasını gerektirir. Bu bağlantı noktalarıyla [birlikte, EnableLinkRedirect](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.amqp.amqptransportsettings.enablelinkredirect?view=azure-dotnet) özelliği etkinse ek bağlantı noktaları açmak gerekebilir. `EnableLinkRedirect`iletileri alırken tek atlamalı atlamayı sağlayan ve böylece iş elde etmeyi artırmaya yardımcı olan yeni bir ileti özelliğidir. İstemci, aşağıdaki resimde gösterildiği gibi bağlantı noktası aralığı 104XX üzerinden arka uç hizmeti ile doğrudan iletişim kurmaya başlar. 

![Hedef bağlantı noktaları listesi][4]

Bir .NET istemcisi, bu bağlantı noktaları güvenlik duvarı tarafından engellenirse, bir SocketException ile başarısız olur ("Bir sokete erişim izinleri tarafından yasaklanmış bir şekilde erişme girişiminde bulunulmuşsa") Özellik, istemcileri bağlantı `EnableAmqpLinkRedirect=false` noktası 5671 üzerinden uzak hizmetle iletişim kurmaya zorlayan bağlantı dizesinde ayarlayarak devre dışı tutulabilir.


### <a name="links"></a>Bağlantılar

AMQP iletileri bağlantılar üzerinden aktarıyor. Bağlantı, iletilerin tek bir yönde aktarılmasını sağlayan bir oturum üzerinde oluşturulan bir iletişim yoludur; transfer durumu müzakere sağlık bağlantısı üzerinde, bağlantılı taraflar arasındaki bağlantı ve iki yönlü

![][2]

Bağlantılar, aktarım ve aktarım yolunun başlatılmasının, transfer ve transfer yolunun başlatılmasının, aktarım ve aktarım yolunun özel bir ayrıcalığı olduğu HTTP ve MQTT de dahil olmak üzere, AMQP'yi diğer birçok protokolden farklı kılan, mevcut bir oturum da veya mevcut bir oturum üzerinde her iki kapsayıcı tarafından oluşturulabilir. soket bağlantısı.

Bağlantıyı başlatan kapsayıcı, karşı kapsayıcıdan bir bağlantıyı kabul etmesini ister ve gönderen veya alıcı nın rolünü seçer. Bu nedenle, kapsayıcı tek yönlü veya çift yönlü iletişim yolları oluşturma başlatabilir, ikinci bağlantı çiftleri olarak modellenmiştir.

Bağlantılar adlandırılmış ve düğümlerle ilişkilidir. Başlangıçta belirtildiği gibi, düğümler bir konteyner içinde iletişim varlıklarıdır.

Hizmet Veri Çörneği'nde düğüm, bir sıraya, bir konuya, aboneye veya bir kuyruğa veya aboneliğin deadletter alt kuyruğuna doğrudan eşdeğerdir. BU nedenle AMQP'da kullanılan düğüm adı, Hizmet Veri Birimi ad alanının içindeki varlığın göreli adıdır. Bir sıra adlandırılmışsa, `myqueue`bu da onun AMQP düğüm adıdır. Bir konu aboneliği bir "abonelikler" kaynak koleksiyonu içine sıralanmış ve böylece, bir konu **mytopic** bir abonelik **alt** AMQP düğüm adı **mytopic / abonelikleri / alt**sıralanmış olarak HTTP API kuralı izler.

Bağlanan istemcinin bağlantı oluşturmak için yerel bir düğüm adı kullanması da gerekir; Servis Otobüsü bu düğüm adları hakkında açıklayıcı değildir ve bunları yorumlamaz. AMQP 1.0 istemci yığınları genellikle bu geçici düğüm adlarının istemci kapsamında benzersiz olduğundan emin olmak için bir düzen kullanır.

### <a name="transfers"></a>Transfer

Bir bağlantı oluşturulduktan sonra, iletiler bu bağlantı üzerinden aktarılabilir. AMQP'de aktarım, bir bağlantı üzerinden gönderenden alıcıya ileti götüren açık bir protokol hareketi *(aktarım* performatifi) ile yürütülür. Bir transfer "kararlaştırıldığında" tamamlanır, yani her iki taraf da bu transferin sonucu hakkında ortak bir anlayış oluşturmuştur.

![][3]

En basit durumda, gönderen iletileri "önceden kapatılabilir" göndermeyi seçebilir, yani istemci sonuçla ilgilenmez ve alıcı işlemin sonucu hakkında herhangi bir geri bildirim sağlamaz. Bu mod, AMQP iletişim kuralı düzeyinde Servis Veri Servisi tarafından desteklenir, ancak istemci API'lerinin hiçbirinde açıklanmaz.

Normal durum, iletilerin tedirgin gönderilmesi ve alıcının daha sonra *düzenleme* performatifkullanarak kabul veya reddi gösterir olmasıdır. Ret, alıcı iletiyi herhangi bir nedenle kabul edemediğinde oluşur ve ret iletisi AMQP tarafından tanımlanan bir hata yapısı olan neden hakkında bilgi içerir. İletiler Servis Veri Kurumu'nun içindeki iç hatalar nedeniyle reddedilirse, destek isteğinde bulunarak destek personeliiçin tanılama ipuçları sağlamak için kullanılabilecek bu yapının içinde ek bilgiler verir. Hatalar hakkında daha fazla ayrıntı daha sonra öğrenirsiniz.

Özel bir ret şekli, alıcının transfere teknik bir itirazı olmadığını, aynı zamanda transferin yerleşmesini de gerektirmediğini gösteren *serbest bırakılan* durumdur. Bu durum, örneğin, bir Hizmet Veri Servisi istemcisine ileti teslim edildiğinde ve istemci iletiyi işlemeden kaynaklanan işi gerçekleştiremediği için iletiyi "terk etmeyi" seçtiğinde vardır; ileti tesliminin kendisi hatalı değildir. Bu durum bir varyasyonu yayımlanır gibi ileti değişiklikleri sağlayan *değiştirilmiş* durumdur. Bu durum şu anda Servis Otobüsü tarafından kullanılmaz.

AMQP 1.0 belirtimi *alınan*denilen bir daha disposition durumu tanımlar , özellikle bağlantı kurtarma işlemek için yardımcı olur. Bağlantı kurtarma, önceki bağlantı ve oturum kaybolduğunda, yeni bir bağlantı ve oturumun üzerine bağlantının durumunu ve bekleyen teslimatları yeniden oluşturmanızı sağlar.

Servis Veri Yolu bağlantı kurtarmayı desteklemez; Müşteri bekleyen bir ileti aktarımı ile Servis Veri Servisi bağlantısını kaybederse, bu ileti aktarımı kaybolur ve istemci yeniden bağlanmalı, bağlantıyı yeniden kurmalı ve aktarımı yeniden denemelidir.

Bu nedenle, Hizmet Veri Servisi ve Etkinlik Hub'ları, gönderenin iletinin depolandığı ve kabul edildiği için güvence alınabileceği "en az bir kez" aktarımını destekler, ancak sistemin bağlantıyı kurtarmaya çalışacağı AMQP düzeyinde "tam olarak bir kez" aktarımını desteklemez ve ileti aktarımının çoğaltılmasını önlemek için teslim durumu üzerinde görüşmeye devam edin.

Olası yinelenen göndermeleri telafi etmek için, Service Bus kuyruklarda ve konularüzerinde isteğe bağlı bir özellik olarak yinelenen algılamayı destekler. Yinelenen algılama, kullanıcı tanımlı bir zaman penceresi boyunca gelen tüm iletilerin ileti bilgilerini kaydeder ve aynı pencerede aynı ileti-disleriyle gönderilen tüm iletileri sessizce düşürür.

### <a name="flow-control"></a>Akış kontrolü

Daha önce tartışılan oturum düzeyi akış denetimi modeline ek olarak, her bağlantının kendi akış kontrol modeli vardır. Oturum düzeyinde akış denetimi, kapsayıcıyı aynı anda çok fazla kare işlemek zorunda kalmaktan korur, bağlantı düzeyi akış denetimi uygulamayı bir bağlantıdan işlemek istediği iletilerin kaç iletisinden ve ne zaman işlemek istediğinden sorumlu hale getirir.

![][4]

Bir bağlantıda, aktarımlar yalnızca gönderenin yeterli *bağlantı kredisine*sahip olması yla gerçekleşebilir. Bağlantı kredisi, bir bağlantıya kapsamlı olan *akış* performatifini kullanarak alıcı tarafından ayarlanan bir sayaçtır. Gönderene bağlantı kredisi atandığında, iletiler sunarak bu krediyi kullanmaya çalışır. Her ileti teslimi, kalan bağlantı kredisini 1 olarak erteler. Bağlantı kredisi kullanıldığında, teslimatlar durur.

Servis Veri Servisi alıcı rolündeyken, iletilerin hemen gönderilebilmeleri için gönderene anında yeterli bağlantı kredisi sağlar. Bağlantı kredisi kullanıldığında, Servis Veri Servisi bazen bağlantı kredi bakiyesini güncelleştirmek için gönderene bir *akış* performansı gönderir.

Gönderen rolünde, Servis Veri Servisi, bekleyen bağlantı kredilerini kullanmak için iletigönderir.

API düzeyindeki bir "alma" çağrısı, istemci tarafından Servis Veri Servisi'ne gönderilen bir *akış* gerçekleştirmesine dönüşür ve Servis Veri Servisi, kullanılabilir ilk, kilidi açan iletiyi sıradan alarak, kilitleyerek ve aktararak bu krediyi tüketir. Teslimat için hazır bir ileti yoksa, söz konusu varlıkla kurulan herhangi bir bağlantı tarafından yapılan herhangi bir ödenmemiş kredi varış sırasına göre kaydedilir ve iletiler kullanılabilir hale geldikçe, herhangi bir ödenmemiş kredi kullanmak üzere kilitlenir ve aktarılır.

İletinin kilidi, aktarım *kabul edilen,* *reddedilen*veya serbest bırakılan terminal durumlarından birine kapatıldığında serbest *bırakılır.* Terminal durumu *kabul*edildiğinde ileti Servis Veri Servisi'nden kaldırılır. Servis Veri Servisi'nde kalır ve aktarım diğer eyaletlerden herhangi biri geldiğinde bir sonraki alıcıya teslim edilir. Hizmet Veri Mes'i, yinelenen reddetmeler veya sürümler nedeniyle varlık için izin verilen maksimum teslimat sayısına ulaştığında iletiyi otomatik olarak varlığın deadletter kuyruğuna taşır.

Hizmet Veri Mesi API'leri bugün böyle bir seçeneği doğrudan ortaya çıkarmasa da, alt düzey bir AMQP protokolü istemcisi, her bir alma isteği için bir birim kredi verme etkileşimini "push-style" modeline dönüştürmek için bağlantı kredisi modelini kullanabilir çok sayıda bağlantı kredisi veren ve daha sonra daha fazla etkileşim olmadan kullanılabilir hale olarak iletileri almak. Push [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) veya [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver) özellik ayarları üzerinden desteklenir. Sıfır olmayan olduklarında, AMQP istemcisi bunu bağlantı kredisi olarak kullanır.

Bu bağlamda, varlık içindeki kilit süresinin sona erdirilme saatinin ileti kabloya konulduğunda değil, varlıktan alındığında başladığını anlamak önemlidir. İstemci bağlantı kredisi vererek ileti almaya hazır olduğunu gösterdiğinde, iletileri ağ üzerinden etkin bir şekilde çekmesi ve bunları işlemeye hazır olması beklenir. Aksi takdirde ileti kilidi ileti teslim edilmeden önce dolmuş olabilir. Bağlantı-kredi akışı denetiminin kullanımı, alıcıya gönderilen kullanılabilir iletilerle başa çıkılan acil durumu doğrudan yansıtmalıdır.

Özetle, aşağıdaki bölümler, farklı API etkileşimleri sırasında performans akışının şematik bir özetini sağlar. Her bölüm farklı bir mantıksal işlem açıklar. Bu etkileşimlerin bazıları "tembel" olabilir, yani yalnızca gerektiğinde gerçekleştirilebilirler. İleti gönderen inanın oluşturulması, ilk ileti gönderilene veya istenene kadar ağ etkileşimine neden olmayabilir.

Aşağıdaki tablodaki oklar performatif akış yönünü gösterir.

#### <a name="create-message-receiver"></a>İleti alıcısı oluşturma

| İstemci | Service Bus |
| --- | --- |
| --> takın(<br/>isim={bağlantı adı},<br/>tutamak={sayısal sap},<br/>role=**alıcı**,<br/>kaynak={varlık adı},<br/>target={istemci bağlantı kimliği}<br/>) |İstemci alıcı olarak varlığa bağlanır |
| Bağlantının sonuna iliştiren Servis Veri Servisi yanıtları |<- ekle(<br/>isim={bağlantı adı},<br/>tutamak={sayısal sap},<br/>role=**gönderen**,<br/>kaynak={varlık adı},<br/>target={istemci bağlantı kimliği}<br/>) |

#### <a name="create-message-sender"></a>İleti gönderen oluşturma

| İstemci | Service Bus |
| --- | --- |
| --> takın(<br/>isim={bağlantı adı},<br/>tutamak={sayısal sap},<br/>role=**gönderen**,<br/>kaynak={istemci bağlantı kimliği},<br/>hedef={varlık adı}<br/>) |Eylem yok |
| Eylem yok |<- ekle(<br/>isim={bağlantı adı},<br/>tutamak={sayısal sap},<br/>role=**alıcı**,<br/>kaynak={istemci bağlantı kimliği},<br/>hedef={varlık adı}<br/>) |

#### <a name="create-message-sender-error"></a>İleti gönderen oluşturma (hata)

| İstemci | Service Bus |
| --- | --- |
| --> takın(<br/>isim={bağlantı adı},<br/>tutamak={sayısal sap},<br/>role=**gönderen**,<br/>kaynak={istemci bağlantı kimliği},<br/>hedef={varlık adı}<br/>) |Eylem yok |
| Eylem yok |<- ekle(<br/>isim={bağlantı adı},<br/>tutamak={sayısal sap},<br/>role=**alıcı**,<br/>kaynak=null,<br/>hedef=null<br/>)<br/><br/><— müfreze<br/>tutamak={sayısal sap},<br/>kapalı=**doğru**,<br/>hata={hata bilgisi}<br/>) |

#### <a name="close-message-receiversender"></a>İleti alıcısı/göndereni kapatma

| İstemci | Service Bus |
| --- | --- |
| --> ayrılması<br/>tutamak={sayısal sap},<br/>kapalı=**true**<br/>) |Eylem yok |
| Eylem yok |<— müfreze<br/>tutamak={sayısal sap},<br/>kapalı=**true**<br/>) |

#### <a name="send-success"></a>Gönder (başarı)

| İstemci | Service Bus |
| --- | --- |
| --> transferi<br/>teslimat-id={sayısal kulp},<br/>teslim-tag={ikili tutamaç},<br/>yerleşmiş=**yanlış**,,more=**false**,<br/>durum=**null**,<br/>resume=**false**<br/>) |Eylem yok |
| Eylem yok |<- disposition(<br/>role=alıcı,<br/>ilk={teslimat kimliği},<br/>son={teslimat kimliği},<br/>yerleşmiş=**doğru**,<br/>durum=**kabul edildi**<br/>) |

#### <a name="send-error"></a>Gönder (hata)

| İstemci | Service Bus |
| --- | --- |
| --> transferi<br/>teslimat-id={sayısal kulp},<br/>teslim-tag={ikili tutamaç},<br/>yerleşmiş=**yanlış**,,more=**false**,<br/>durum=**null**,<br/>resume=**false**<br/>) |Eylem yok |
| Eylem yok |<- disposition(<br/>role=alıcı,<br/>ilk={teslimat kimliği},<br/>son={teslimat kimliği},<br/>yerleşmiş=**doğru**,<br/>durum=**reddedildi**(<br/>hata={hata bilgisi}<br/>)<br/>) |

#### <a name="receive"></a>Al

| İstemci | Service Bus |
| --- | --- |
| --> akış<br/>bağlantı-kredi=1<br/>) |Eylem yok |
| Eylem yok |< transferi<br/>teslimat-id={sayısal kulp},<br/>teslim-tag={ikili tutamaç},<br/>yerleşmiş=**yanlış**,<br/>more=**false**,<br/>durum=**null**,<br/>resume=**false**<br/>) |
| --><br/>role=**alıcı**,<br/>ilk={teslimat kimliği},<br/>son={teslimat kimliği},<br/>yerleşmiş=**doğru**,<br/>durum=**kabul edildi**<br/>) |Eylem yok |

#### <a name="multi-message-receive"></a>Çoklu ileti alma

| İstemci | Service Bus |
| --- | --- |
| --> akış<br/>bağlantı-kredi=3<br/>) |Eylem yok |
| Eylem yok |< transferi<br/>teslimat-id={sayısal kulp},<br/>teslim-tag={ikili tutamaç},<br/>yerleşmiş=**yanlış**,<br/>more=**false**,<br/>durum=**null**,<br/>resume=**false**<br/>) |
| Eylem yok |< transferi<br/>teslimat-id={sayısal tutamaç+1},<br/>teslim-tag={ikili tutamaç},<br/>yerleşmiş=**yanlış**,<br/>more=**false**,<br/>durum=**null**,<br/>resume=**false**<br/>) |
| Eylem yok |< transferi<br/>teslimat-id={sayısal tutamaç+2},<br/>teslim-tag={ikili tutamaç},<br/>yerleşmiş=**yanlış**,<br/>more=**false**,<br/>durum=**null**,<br/>resume=**false**<br/>) |
| --><br/>role=alıcı,<br/>ilk={teslimat kimliği},<br/>son={teslim kimliği+2},<br/>yerleşmiş=**doğru**,<br/>durum=**kabul edildi**<br/>) |Eylem yok |

### <a name="messages"></a>İletiler

Aşağıdaki bölümlerde, standart AMQP ileti bölümlerindeki özelliklerin Servis Veri Servisi tarafından kullanıldığı ve Servis Veri Servisi API kümesiyle nasıl eşlenebildikleri açıklanmaktadır.

Uygulamanın tanımlaması gereken tüm özellik AMQP'nin `application-properties` haritasına eşlenmelidir.

#### <a name="header"></a>üst bilgi

| Alan Adı | Kullanım | API adı |
| --- | --- | --- |
| Dayanıklı |- |- |
| Öncelik |- |- |
| ttl |Bu ileti için yaşama zamanı |[TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| ilk satın alan |- |- |
| teslimat-sayısı |- |[Teslimat Sayısı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |

#### <a name="properties"></a>properties

| Alan Adı | Kullanım | API adı |
| --- | --- | --- |
| mesaj kimliği |Bu ileti için uygulama tanımlı, serbest biçim tanımlayıcısı. Yinelenen algılama için kullanılır. |[Messageıd](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| user-id |Uygulama tanımlı kullanıcı tanımlayıcısı, Servis Veri Servisi tarafından yorumlanmaz. |Servis Veri Servisi API'si aracılığıyla erişilemez. |
| - |Uygulama tanımlı hedef tanımlayıcı, Servis Veri Servisi tarafından yorumlanmaz. |[Hedef](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| Konu |Uygulama tanımlı ileti amacı tanımlayıcısı, Servis Veri Servisi tarafından yorumlanmaz. |[Etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| yanıt-to |Uygulama tanımlı yanıt yolu göstergesi, Servis Veri Yolu tarafından yorumlanmaz. |[Replyto](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| correlation-id |Uygulama tanımlı korelasyon tanımlayıcısı, Servis Veri Servisi tarafından yorumlanmaz. |[Correlationıd](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| içerik türü |Hizmet Veri Servisi tarafından yorumlanmayan gövde için uygulama tanımlı içerik türü göstergesi. |[Contenttype](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| içerik kodlama |Hizmet Veri Kurumu tarafından yorumlanmayan, gövde için uygulama tanımlı içerik kodlama göstergesi. |Servis Veri Servisi API'si aracılığıyla erişilemez. |
| mutlak vade sonu-zaman |İletinin süresinin mutlak anda dolduğunu bildirir. Girişte göz ardı edilir (üstbilgi TTL gözlenir), çıktıüzerinde yetkilidir. |[Süresi DoluyorAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| yaratılış zamanı |İletinin oluşturulduğu anda bildirir. Servis Otobüsü tarafından kullanılmaz |Servis Veri Servisi API'si aracılığıyla erişilemez. |
| grup kimliği |İlgili iletiler kümesi için uygulama tanımlı tanımlayıcı. Servis Otobüsü oturumları için kullanılır. |[Sessionıd](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| grup sırası |Bir oturum içindeki iletinin göreli sıra numarasını tanımlayan sayaç. Servis Otobüsü tarafından göz ardı edilir. |Servis Veri Servisi API'si aracılığıyla erişilemez. |
| yanıt-grup-id |- |[Yanıttosessionid](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |

#### <a name="message-annotations"></a>İleti ek açıklamaları

AMQP ileti özelliklerinin bir parçası olmayan ve iletideki gibi `MessageAnnotations` aktarılan birkaç başka servis veri günü iletisi özelliği vardır.

| Ek Açıklama Haritası Anahtarı | Kullanım | API adı |
| --- | --- | --- |
| x-opt-scheduled-enqueue-time | İletinin varlıkta hangi anda görünmesi gerektiğini bildirir |[ScheduledEnqueueTime](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.scheduledenqueuetimeutc?view=azure-dotnet) |
| x-opt-partition-tuşu | İletinin hangi bölüme inmesi gerektiğini belirleyen uygulama tanımlı anahtar. | [PartitionKey](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey?view=azure-dotnet) |
| x-opt-via-partition-key | Bir hareket aktarım sırası üzerinden ileti göndermek için kullanılacaksa uygulama tanımlı bölüm anahtarı değeri. | [ViaPartitionKey](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.viapartitionkey?view=azure-dotnet) |
| x-opt-enqueued-time | İletiyi sıralamanın gerçek zamanını temsil eden hizmet tanımlı UTC zamanı. Girişte göz ardı edilir. | [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc?view=azure-dotnet) |
| x-opt-sequence-number | İletiye atanan hizmet tanımlı benzersiz numara. | [Sequencenumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber?view=azure-dotnet) |
| x-opt-ofset | İletinin hizmet tanımlı sıra numarası. | [EnqueuedSequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedsequencenumber?view=azure-dotnet) |
| x-opt-kilitli-kadar | Hizmet tanımlı. İletinin sıraya/abonelikte kilitlendiği tarih ve saat. | [KilitliUntilUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.lockeduntilutc?view=azure-dotnet) |
| x-opt-deadletter-kaynak | hizmet tanımlı. İleti ölü harf kuyruğundan alınırsa, özgün iletinin kaynağı. | [DeadLetterSource](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deadlettersource?view=azure-dotnet) |

### <a name="transaction-capability"></a>İşlem yeteneği

Bir hareket, iki veya daha fazla işlemi tek bir yürütme kapsamında bir araya toplar. Doğası gereği, bu tür bir işlem, belirli bir operasyon grubuna ait tüm işlemlerin birlikte başarılı veya başarısız olmasını sağlamalıdır.
İşlemler bir tanımlayıcı tarafından `txn-id`gruplandırılır.

İşlemsel etkileşim için istemci, `transaction controller` birlikte gruplandırılması gereken işlemleri kontrol eden bir , olarak hareket eder. Servis Otobüs Servisi bir `transactional resource` olarak hareket eder ve `transaction controller`tarafından talep edildiği gibi iş gerçekleştirir.

İstemci ve hizmet, istemci tarafından kurulan bir `control link` üzerinden iletişim kurar. Ve `declare` `discharge` iletiler, sırasıyla işlemleri ayırmak ve tamamlamak için denetim bağlantısı üzerinden denetleyici tarafından gönderilir (bunlar işlem çalışmasının çizimini temsil etmez). Gerçek gönderme/alma bu bağlantıda gerçekleştirilmez. İstenen her işlem işlemi açıkça istenilen `txn-id` ile tanımlanır ve bu nedenle Bağlantı üzerindeki herhangi bir bağlantıda oluşabilir. Oluşturduğu boşaltılmamış hareketler varken denetim bağlantısı kapatılırsa, bu tür tüm işlemler hemen geri alınır ve bunlar üzerinde daha fazla işlem gerçekleştirme girişimleri hataya neden olur. Denetim bağlantısındaki iletiler önceden kapatılmamalıdır.

Hareketleri başlatıp sonlayabilmek için her bağlantının kendi denetim bağlantısını başlatması gerekir. Hizmet, `coordinator`bir . İstemci/denetleyici bu hedefe bir denetim bağlantısı kurar. Denetim bağlantısı bir varlığın sınırının dışındadır, yani aynı denetim bağlantısı birden çok varlık için hareketleri başlatmak ve boşaltmak için kullanılabilir.

#### <a name="starting-a-transaction"></a>İşlem başlatma

İşlem çalışmalarına başlamak için. denetleyici koordinatörden `txn-id` bir bilgi almalıdır. Bunu bir `declare` tür iletisi göndererek yapar. Bildirim başarılı olursa, koordinatör atanan `txn-id`.

| İstemci (Denetleyici) | | Servis Otobüsü (Koordinatör) |
| --- | --- | --- |
| ekle(<br/>isim={bağlantı adı},<br/>... ,<br/>role=**gönderen**,<br/>hedef=**Koordinatör**<br/>) | ------> |  |
|  | <------ | ekle(<br/>isim={bağlantı adı},<br/>... ,<br/>hedef=Koordinatör()<br/>) |
| transferi(<br/>teslimat-id=0, ...)<br/>{ AmqpValue (**Bildir()**)}| ------> |  |
|  | <------ | (disposition) <br/> ilk=0, son=0, <br/>durum=**Beyan edilen**(<br/>**txn-id**={işlem kimliği}<br/>))|

#### <a name="discharging-a-transaction"></a>İşlemi boşaltma

Denetleyici, koordinatöre bir `discharge` ileti göndererek işlem çalışmasını sonuçlandırır. Denetleyici, `fail` bayrağı boşaltma gövdesine ayarlayarak işlem işini işlemek veya geri almak istediğini belirtir. Koordinatör deşarjı tamamlayamıyorsa, ileti bu sonucu taşıyan `transaction-error`ile reddedilir.

> Not: fail=true bir işlemin Rollback'ini, fail=false ise Commit anlamına gelir.

| İstemci (Denetleyici) | | Servis Otobüsü (Koordinatör) |
| --- | --- | --- |
| transferi(<br/>teslimat-id=0, ...)<br/>{ AmqpValue (Bildir())}| ------> |  |
|  | <------ | (disposition) <br/> ilk=0, son=0, <br/>durum=Beyan edilen<br/>txn-id={işlem kimliği}<br/>))|
| | . . . <br/>İşlemsel çalışma<br/>diğer bağlantılarda<br/> . . . |
| transferi(<br/>teslimat-id=57, ...)<br/>{ AmqpValue (<br/>**Deşarj(txn-id=0,<br/>fail=false)**)}| ------> |  |
| | <------ | (disposition) <br/> ilk=57, son=57, <br/>durum=**Kabul edildi()**)|

#### <a name="sending-a-message-in-a-transaction"></a>Harekette ileti gönderme

Tüm işlem işleri txn-id `transactional-state` taşıyan işlem teslim durumu ile yapılır. İleti gönderme durumunda, işlem durumu iletinin aktarım çerçevesi tarafından gerçekleştirilir. 

| İstemci (Denetleyici) | | Servis Otobüsü (Koordinatör) |
| --- | --- | --- |
| transferi(<br/>teslimat-id=0, ...)<br/>{ AmqpValue (Bildir())}| ------> |  |
|  | <------ | (disposition) <br/> ilk=0, son=0, <br/>durum=Beyan edilen<br/>txn-id={işlem kimliği}<br/>))|
| transferi(<br/>tutamak=1,<br/>teslimat-id=1, <br/>**durum=<br/>TransactionalState(<br/>txn-id=0) )**<br/>{ yük }| ------> |  |
| | <------ | (disposition) <br/> ilk=1, son=1, <br/>durum=**<br/>TransactionalState( txn-id=0,<br/>sonuç=Kabul()**))|

#### <a name="disposing-a-message-in-a-transaction"></a>Hareketteki bir iletiyi çıkarma

İleti mizacı `Complete`  /  `Abandon`  /  `DeadLetter`  /  `Defer`gibi işlemleri içerir. Bir işlem içinde bu işlemleri `transactional-state` gerçekleştirmek için, disposition ile geçmek.

| İstemci (Denetleyici) | | Servis Otobüsü (Koordinatör) |
| --- | --- | --- |
| transferi(<br/>teslimat-id=0, ...)<br/>{ AmqpValue (Bildir())}| ------> |  |
|  | <------ | (disposition) <br/> ilk=0, son=0, <br/>durum=Beyan edilen<br/>txn-id={işlem kimliği}<br/>))|
| | <------ |transferi(<br/>tutamak=2,<br/>teslimat-id=11, <br/>durum=null)<br/>{ yük }|  
| (disposition) <br/> ilk=11, son=11, <br/>durum=**<br/>TransactionalState( txn-id=0,<br/>sonuç=Kabul()**))| ------> |


## <a name="advanced-service-bus-capabilities"></a>Gelişmiş Servis Veri Servisi özellikleri

Bu bölüm, şu anda AMQP için OASIS Teknik Komitesi'nde geliştirilmekte olan AMQP'ye taslak uzantılara dayanan Azure Hizmet Veri Tos'unun gelişmiş yeteneklerini kapsar. Service Bus, bu taslakların en son sürümlerini uygular ve bu taslaklar standart statüye ulaştıkça getirilen değişiklikleri benimser.

> [!NOTE]
> Service Bus Messaging gelişmiş işlemleri bir istek/yanıt deseni ile desteklenir. Bu işlemlerin ayrıntıları [Hizmet Veri Servisi'nde AMQP 1.0 makalesinde açıklanmıştır: istek-yanıt tabanlı işlemler.](service-bus-amqp-request-response.md)
> 
> 

### <a name="amqp-management"></a>AMQP yönetimi

AMQP yönetim belirtimi, bu makalede tartışılan taslak uzantıların ilkidir. Bu belirtim, AMQP üzerinden ileti altyapısıyla yönetim etkileşimlerine izin veren AMQP protokolünün üstüne katmanlı bir protokol kümesi tanımlar. Belirtim, ileti altyapısı ve sorgu işlemleri kümesi içindeki varlıkları yönetmek için *oluşturma,* *okuma,* *güncelleştirme*ve *silme* gibi genel işlemleri tanımlar.

Tüm bu hareketler istemci ve ileti altyapısı arasında bir istek/yanıt etkileşimi gerektirir ve bu nedenle belirtim, bu etkileşim deseninin AMQP'nin üstünde nasıl modellenebildiğini tanımlar: istemci ileti altyapısına bağlanır, bir oturum başlatır ve ardından bir çift bağlantı oluşturur. Bir bağlantıda, istemci gönderen gibi davranır ve diğer tarafta alıcı gibi davranır, böylece çift yönlü bir kanal olarak hareket edebilir bağlantılar bir çift oluşturma.

| Mantıksal İşlem | İstemci | Service Bus |
| --- | --- | --- |
| İstek Yanıt Yolu Oluştur |--> takın(<br/>isim={*bağlantı adı*},<br/>sapı={*sayısal sap*},<br/>role=**gönderen**,<br/>kaynak=**null**,<br/>hedef="myentity/$management"<br/>) |Eylem yok |
| İstek Yanıt Yolu Oluştur |Eylem yok |\<-- ekle(<br/>isim={*bağlantı adı*},<br/>sapı={*sayısal sap*},<br/>role=**alıcı**,<br/>kaynak=null,<br/>hedef="myentity"<br/>) |
| İstek Yanıt Yolu Oluştur |--> takın(<br/>isim={*bağlantı adı*},<br/>sapı={*sayısal sap*},<br/>role=**alıcı**,<br/>kaynak="myentity/$management",<br/>hedef="myclient$id"<br/>) | |
| İstek Yanıt Yolu Oluştur |Eylem yok |\<-- ekle(<br/>isim={*bağlantı adı*},<br/>sapı={*sayısal sap*},<br/>role=**gönderen**,<br/>kaynak="myentity",<br/>hedef="myclient$id"<br/>) |

Bu bağlantı çiftinin yerinde olması, istek/yanıt uygulaması basittir: istek, ileti altyapısı içindeki bu deseni anlayan bir varlığa gönderilen iletidir. Bu istek iletisinde, *özellikler* bölümündeki *yanıt alanı,* yanıtı teslim edilebilmek için bağlantı için *hedef* tanımlayıcıya ayarlanır. İşlem eki, isteği işler ve ardından *yanıtı, hedef* tanımlayıcısı belirtilen *yanıt tanımlayıcısı* ile eşleşen bağlantı üzerinden teslim eder.

Desen açıkça istemci kapsayıcı ve yanıt hedefi için istemci tarafından oluşturulan tanımlayıcı tüm istemciler arasında benzersiz ve güvenlik nedenleriyle, aynı zamanda tahmin etmek zor gerektirir.

Yönetim protokolü ve aynı deseni kullanan diğer tüm protokoller için kullanılan ileti alışverişleri uygulama düzeyinde gerçekleşir; yeni AMQP protokol düzeyinde hareketler tanımlamaz. Bu, böylece uygulamalar uyumlu AMQP 1.0 yığınları ile bu uzantıları hemen yararlanabilir, kasıtlı.

Service Bus şu anda yönetim belirtiminin temel özelliklerinden herhangi birini uygulamamaktadır, ancak yönetim belirtimi tarafından tanımlanan istek/yanıt deseni, talep tabanlı güvenlik özelliği nin ve aşağıdaki bölümlerde tartışılan gelişmiş yeteneklerin neredeyse tümü için temeldir:

### <a name="claims-based-authorization"></a>Talep tabanlı yetkilendirme

AMQP Alacaklarına Dayalı Yetkilendirme (CBS) belirtimi taslağı, yönetim belirtimi isteği/yanıt desenini temel alır ve AMQP ile federe güvenlik belirteçlerinin nasıl kullanılacağına ilişkin genelleştirilmiş bir modeli açıklar.

Girişte tartışılan AMQP'nin varsayılan güvenlik modeli SASL'yi temel alır ve AMQP bağlantı el sıkışması ile tümleşir. SASL'nin kullanılması, resmi olarak SASL'ye dayanan herhangi bir protokolün yararlanabileceği bir dizi mekanizmanın tanımlandığı genişletilebilir bir model sağlama avantajına sahiptir. Bu mekanizmalar arasında kullanıcı adları ve parolaların aktarılması için "DÜZ", TLS düzeyinde güvenliğe bağlayacak "EXTERNAL", açık kimlik doğrulama/yetkilendirme eksikliğini ifade etmek için "ANONYMOUS" ve kimlik doğrulama ve/veya yetki kimlik bilgilerinin veya belirteçlerin geçirilmesine olanak tanıyan çok çeşitli ek mekanizmalar yer almaktadır.

AMQP'nin SASL entegrasyonunun iki dezavantajı vardır:

* Tüm kimlik bilgileri ve belirteçleri bağlantı kapsamındadır. İleti altyapısı, her kuruluş için ayrı ayrı erişim denetimi sağlamak isteyebilir; örneğin, bir belirteci taşıyıcısının A sırasına göndermesine izin verir, ancak B sırasına göndermez. Bağlantıya sabitlenmiş yetkilendirme bağlamı yla, tek bir bağlantı kullanmak ve yine de A ve B kuyruğu için farklı erişim belirteçleri kullanmak mümkün değildir.
* Erişim belirteçleri genellikle yalnızca sınırlı bir süre için geçerlidir. Bu geçerlilik, kullanıcının belirteçleri düzenli aralıklarla yeniden almasını gerektirir ve kullanıcının erişim izinleri değiştiyse belirteç verene yeni bir belirteç vermeyi reddetmesi için bir fırsat sağlar. AMQP bağlantıları uzun süre devam edebilir. SASL modeli yalnızca bağlantı sırasında bir belirteç ayarlama şansı sağlar, bu da ileti altyapısının belirteç süresi dolduğunda istemcinin bağlantısını kesmesi gerektiği veya bağlantı nın istemciyle sürekli iletişime izin verme riskini kabul etmesi gerektiği anlamına gelir. erişim hakları geçici olarak iptal edilmiş olabilir.

Service Bus tarafından uygulanan AMQP CBS belirtimi, bu sorunların her ikisi için de zarif bir geçici çözüm sağlar: Bir istemcinin erişim belirteçlerini her düğümle ilişkilendirmesine ve bu belirteçleri ileti akışını kesintiye uğratmadan sona ermeden önce güncelleştirmesine olanak tanır.

CBS, ileti altyapısı tarafından sağlanacak *$cbs*adlı sanal bir yönetim düğümü tanımlar. Yönetim düğümü, ileti altyapısındaki diğer düğümler adına belirteçleri kabul eder.

Protokol hareketi, yönetim belirtimi tarafından tanımlandığı şekilde bir istek/yanıt değişimidir. Bu, istemcinin *$cbs* düğümüyle bir çift bağlantı oluşturduğu ve giden bağlantıda bir istek geçtiği ve ardından gelen bağlantıdaki yanıtı beklediği anlamına gelir.

İstek iletisi aşağıdaki uygulama özelliklerine sahiptir:

| Anahtar | İsteğe bağlı | Değer Türü | Değer İçeriği |
| --- | --- | --- | --- |
| Işlem |Hayır |string |**put-belirteç** |
| type |Hayır |string |Konulan belirteç türü. |
| ad |Hayır |string |Belirteç geçerli olduğu "hedef kitle". |
| Sona erme |Evet |timestamp |Belirteç son kullanma süresi. |

*Ad* özelliği, belirteçle ilişkilendirilecek varlığı tanımlar. Hizmet Veri Yolu'nda kuyruğa giden yol veya konu/abonelik. *Tür* özelliği belirteç türünü tanımlar:

| Belirteç Türü | Jeton Açıklaması | Vücut Tipi | Notlar |
| --- | --- | --- | --- |
| amqp:jwt |JSON Web Belirteci (JWT) |AMQP Değeri (dize) |Henüz mevcut değil. |
| amqp:swt |Basit Web Belirteci (SWT) |AMQP Değeri (dize) |Yalnızca AAD/ACS tarafından yayınlanan SWT belirteçleri için desteklenir |
| servicebus.windows.net:sastoken |Servis Otobüsü SAS Jeton |AMQP Değeri (dize) |- |

Belirteçler hakları verir. Service Bus üç temel hak hakkında bilgi verir: "Gönder" göndermeyi sağlar, "Dinle" alma sağlar ve "Yönet" varlıkları manipüle sağlar. AAD/ACS tarafından yayınlanan SWT belirteçleri, bu hakları talep olarak açıkça içerir. Service Bus SAS belirteçleri ad alanı veya varlık üzerinde yapılandırılan kuralları ifade eder ve bu kurallar haklar ile yapılandırılır. Belirteci bu kuralla ilişkili anahtarla imzalamak, belirteci ilgili hakları ifade eder. *Put-token* kullanan bir varlıkla ilişkili belirteç, bağlı istemcinin belirteç hakları başına varlıkla etkileşimkurmasını sağlar. İstemcinin *gönderen* rolünü aldığı bir bağlantı "Gönder" hakkını gerektirir; *alıcı* rolünü üstlenmek "Dinle" hakkını gerektirir.

Yanıt iletisi aşağıdaki *uygulama özellikleri* değerlerine sahiptir

| Anahtar | İsteğe bağlı | Değer Türü | Değer İçeriği |
| --- | --- | --- | --- |
| durum kodu |Hayır |int |HTTP yanıt kodu **[RFC2616]**. |
| durum açıklaması |Evet |string |Durumun açıklaması. |

İstemci, ileti altyapısındaki herhangi bir varlık için *put-token'i* tekrar tekrar arayabilir. Belirteçler geçerli istemciye kadar genişbir kapsamdadır ve geçerli bağlantıya sabitlenir, bu da bağlantı düştüğünde sunucunun tutulan belirteçleri düşürttettiği anlamına gelir.

Mevcut Servis Veri Servisi uygulaması yalnızca CBS'in SASL yöntemi "ANONYMOUS" ile birlikte çalışmasına izin verir. SASL el sıkışmadan önce her zaman bir SSL/TLS bağlantısı olmalıdır.

Bu nedenle ANONYMOUS mekanizması seçilen AMQP 1.0 istemcisi tarafından desteklenmelidir. Anonim erişim, ilk oturumun oluşturulması da dahil olmak üzere ilk bağlantı el sıkışmasının, Hizmet Veri Yolu'nun bağlantıyı kimin oluşturduğunu bilmeden gerçekleştiği anlamına gelir.

Bağlantı ve oturum kurulduktan sonra, *$cbs* düğümüne bağlantılar takılması ve *put-token* isteğinin gönderilmesi izin verilen tek işlemlerdir. Geçerli bir belirteç, bağlantı kurulduktan sonra 20 saniye içinde bazı varlık düğümü için *bir put-token* isteği kullanılarak başarıyla ayarlanmalıdır, aksi takdirde bağlantı Hizmet Veri Birimi tarafından tek taraflı olarak bırakılır.

İstemci daha sonra belirteç sona erme tarihini izlemekten sorumludur. Bir belirteç süresi dolduğunda, Servis Veri Servisi ilgili kuruluşa olan bağlantıdaki tüm bağlantıları derhal düşürür. Sorunun oluşmasını önlemek için, istemci aynı *put-token* hareketi ile sanal *$cbs* yönetim düğümü aracılığıyla ve farklı bağlantılarda akan yük trafiğinin önüne geçmeden düğüm belirteci yenisiyle değiştirebilir.

### <a name="send-via-functionality"></a>Gönderi-gönder işlevi

[Gönder / Transfer gönderen,](service-bus-transactions.md#transfers-and-send-via) servis veri tonunun belirli bir iletiyi başka bir varlık aracılığıyla hedef bir kuruluşa iletmesini sağlayan bir işlevdir. Bu özellik, tek bir işlemdeki varlıklar arasında işlemleri gerçekleştirmek için kullanılır.

Bu işlevsellikle, bir gönderen oluşturmak ve `via-entity`bağlantı kurmak . Bağlantıyı kurarken, bu bağlantıdaki iletilerin/aktarımların gerçek hedefini belirlemek için ek bilgiler aktarılır. Ekleme başarılı olduktan sonra, bu bağlantıda gönderilen tüm iletiler otomatik olarak varlık *üzerinden* *hedef kuruluşa* iletilir. 

> Not: Bu bağlantıyı kurmadan önce kimlik doğrulamanın hem *varlık* hem de *hedef varlık* için gerçekleştirilmesi gerekir.

| İstemci | | Service Bus |
| --- | --- | --- |
| ekle(<br/>isim={bağlantı adı},<br/>role=gönderen,<br/>kaynak={istemci bağlantı kimliği},<br/>hedef=**{via-entity}**,<br/>**properties=map [(<br/>com.microsoft:transfer-hedef-adres=<br/>{hedef-varlık} )]** ) | ------> | |
| | <------ | ekle(<br/>isim={bağlantı adı},<br/>role=alıcı,<br/>kaynak={istemci bağlantı kimliği},<br/>hedef={via-entity},<br/>özellikleri=harita [(<br/>com.microsoft:transfer-hedef-adres=<br/>{hedef-varlık} )] ) |

## <a name="next-steps"></a>Sonraki adımlar

AMQP hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları ziyaret edin:

* [Servis Veri Servisi AMQP'ye genel bakış]
* [Hizmet Veri Servisi bölümlü kuyruklar ve konular için AMQP 1.0 desteği]
* [Windows Server için Hizmet Veri Servisi'nde AMQP]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Servis Veri Servisi AMQP'ye genel bakış]: service-bus-amqp-overview.md
[Hizmet Veri Servisi bölümlü kuyruklar ve konular için AMQP 1.0 desteği]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[Windows Server için Hizmet Veri Servisi'nde AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
