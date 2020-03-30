---
title: Akıllı sözleşme tümleştirme desenleri - Azure Blockchain Workbench
description: Azure Blockchain Workbench Preview'da akıllı sözleşme tümleştirme modellerine genel bakış.
ms.date: 11/20/2019
ms.topic: conceptual
ms.reviewer: mmercuri
ms.openlocfilehash: f9626edd5bd655e3de5d0f9648041faf832e3b84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74325964"
---
# <a name="smart-contract-integration-patterns"></a>Akıllı anlaşma tümleştirme desenleri

Akıllı sözleşmeler genellikle dış sistemler ve aygıtlarla tümleştirmesi gereken bir iş akışını temsil eleştirir.

Bu iş akışlarının gereksinimleri, harici bir sistem, hizmet veya aygıttan gelen verileri içeren dağıtılmış bir genel muhasebede işlem başlatma gereksinimini içerir. Ayrıca, harici sistemlerin dağıtılmış bir genel muhasebedeki akıllı sözleşmelerden kaynaklanan olaylara tepki göstermesi gerekir.

REST API ve mesajlaşma tümleştirmesi, dış sistemlerden azure blockchain Workbench uygulamasında yer alan akıllı sözleşmelere işlem gönderir. Ayrıca, bir uygulama içinde gerçekleşen değişikliklere dayalı olarak dış sistemlere olay bildirimleri gönderir.

Veri tümleştirme senaryoları için Azure Blockchain Workbench, blockchain'den gelen işlem verilerini ve uygulamalar ve akıllı sözleşmeler hakkındaki meta verileri birleştiren bir veritabanı görünümü kümesi içerir.

Buna ek olarak, tedarik zinciri veya medya ile ilgili olanlar gibi bazı senaryolar da belgelerin entegrasyonunu gerektirebilir. Azure Blockchain Workbench belgeleri doğrudan işlemek için API çağrıları sağlamasa da, belgeler blockchain uygulamasına dahil edilebilir. Bu bölüm de bu deseni içerir.

Bu bölüm, uçtan uca çözümlerinizde bu tür tümleştirmelerin her birini uygulamak için tanımlanan desenleri içerir.

## <a name="rest-api-based-integration"></a>REST API tabanlı tümleştirme

Azure Blockchain Workbench tarafından oluşturulan web uygulamasındaki özellikler REST API aracılığıyla ortaya çıkar. Özellikler arasında Azure Blockchain Workbench yükleme, uygulamaların yapılandırması ve yönetimi, hareketleri dağıtılmış bir genel muhasebeye gönderme ve uygulama meta verileri ve genel muhasebe verilerinin sorgulanması yer almaktadır.

REST API öncelikle web, mobil ve bot uygulamaları gibi etkileşimli istemciler için kullanılır.

Bu bölümde, dağıtılmış bir genel muhasebeye işlem gönderen REST API'sinin yönlerine odaklanan desenler ve Azure Blockchain Workbench'in *zincir dışı* SQL veritabanındaki işlemlerle ilgili verileri sorgulayan desenler ele ait.

### <a name="sending-transactions-to-a-distributed-ledger-from-an-external-system"></a>Dış sistemden dağıtılmış genel muhasebeye hareket gönderme

Azure Blockchain Workbench REST API, işlemleri dağıtılmış bir genel muhasebede yürütmek için kimlik doğrulaması yapılan istekler gönderir.

![Hareketleri dağıtılmış genel muhasebeye gönderme](./media/integration-patterns/send-transactions-ledger.png)

İşlemleri yürütme, daha önce gösterilen işlemi kullanarak oluşur ve aşağıdakileri içerir:

-   Dış uygulama, Azure Blockchain Workbench dağıtımının bir parçası olarak sağlanan Azure Etkin Dizini'nde kimlik doğrulanır.
-   Yetkili kullanıcılar, API'ye isteklerle birlikte gönderilebilen bir taşıyıcı belirteci alır.
-   Dış uygulamalar, taşıyıcı belirteci kullanarak REST API'ye çağrı yapar.
-   REST API isteği bir ileti olarak paketler ve Servis Veri Servisi'ne gönderir. Buradan alınır, imzalanır ve uygun dağıtılmış deftere gönderilir.
-   REST API, talebi kaydetmek ve geçerli sağlama durumunu belirlemek için Azure Blockchain Workbench SQL DB'ye bir istekte bulundu.
-   SQL DB sağlama durumunu döndürür ve API çağrısı kimliği onu çağıran dış uygulamaya döndürür.

### <a name="querying-blockchain-workbench-metadata-and-distributed-ledger-transactions"></a>Blockchain Workbench meta verilerini ve dağıtılmış genel muhasebe hareketlerini sorgulama

Azure Blockchain Workbench REST API, dağıtılmış bir genel muhasebede akıllı sözleşme yürütmeyle ilgili sorgu ayrıntılarına kimlik doğrulaması istekleri gönderir.

![Meta verileri sorgulama](./media/integration-patterns/querying-metadata.png)

Sorgulama, daha önce gösterilen işlemi kullanarak oluşur:

1. Dış uygulama, Azure Blockchain Workbench dağıtımının bir parçası olarak sağlanan Azure Etkin Dizini'nde kimlik doğrulanır.
2. Yetkili kullanıcılar, API'ye isteklerle birlikte gönderilebilen bir taşıyıcı belirteci alır.
3. Dış uygulamalar, taşıyıcı belirteci kullanarak REST API'ye çağrı yapar.
4. REST API, SQL DB'den gelen istek için verileri sorgular ve istemciye döndürür.

## <a name="messaging-integration"></a>Mesajlaşma tümleştirmesi

İleti entegrasyonu, etkileşimli oturum açmanın mümkün olmadığı veya istenmediği sistemler, hizmetler ve aygıtlarla etkileşimi kolaylaştırır. İleti tümleştirmesi iki tür iletiye odaklanır: hareketlerin dağıtılmış bir genel muhasebede yürütülmesini isteyen iletiler ve hareketler gerçekleştiğinde bu genel muhasebetarafından ortaya çıkarılan olaylar.

İleti tümleştirmesi, kullanıcı oluşturma, sözleşme oluşturma ve sözleşmelerle ilgili işlemlerin yürütülmesi ve izlenmesine odaklanır ve öncelikle *başsız* arka uç sistemleri tarafından kullanılır.

Bu bölümde, ileti tabanlı API'nin dağıtılmış bir genel muhasebeye işlem gönderen yönlerine odaklanan desenler ve altta yatan dağıtılmış genel muhasebe tarafından açığa çıkarılan olay iletilerini temsil eden desenler ele alınmaktadır.

### <a name="one-way-event-delivery-from-a-smart-contract-to-an-event-consumer"></a>Akıllı sözleşmeden bir etkinlik tüketicisine tek yönlü olay teslimi 

Bu senaryoda, bir olay akıllı bir sözleşme içinde oluşur, örneğin, bir durum değişikliği veya işlem belirli bir tür yürütülmesi. Bu olay, bir Olay Izgara üzerinden aşağı tüketicilere yayınlanır ve bu tüketiciler daha sonra uygun eylemleri gerçekleştirin.

Bu senaryoya örnek olarak, bir işlem gerçekleştiğinde, bir tüketici uyarılacak ve bilgileri BIR SQL DB'de veya Ortak Veri Hizmeti'nde kaydetmek gibi bir eylemde bulunabilir. Bu senaryo, Workbench'in *kapalı zinciri* SQL DB'sini doldurmak için izlediği desenle aynıdır.

Başka bir akıllı sözleşme belirli bir duruma geçiş, örneğin bir sözleşme *OutOfCompliance*gider olacaktır. Bu durum değişikliği gerçekleştiğinde, bir yöneticinin cep telefonuna gönderilmek üzere bir uyarı tetikleyebilir.

![Tek yönlü etkinlik teslimi](./media/integration-patterns/one-way-event-delivery.png)

Bu senaryo, daha önce gösterilen işlemi kullanarak oluşur:

-   Akıllı sözleşme yeni bir duruma geçiş eder ve bir olayı genel muhasebeye gönderir.
-   Genel muhasebe, etkinliği alır ve Azure Blockchain Workbench'e teslim eder.
-   Azure Blockchain Workbench genel muhasebeden olaylara abone olur ve olayı alır.
-   Azure Blockchain Workbench etkinliği Etkinlik Grid'indeki abonelere yayınlar.
-   Dış sistemler Olay Izgara'sına abone olur, iletiyi tüketir ve uygun eylemleri gerçekleştirin.

## <a name="one-way-event-delivery-of-a-message-from-an-external-system-to-a-smart-contract"></a>Harici sistemden akıllı bir sözleşmeye iletinin tek yönlü olay teslimi

Ters yönden akan bir senaryo da vardır. Bu durumda, bir olay bir sensör veya harici bir sistem tarafından oluşturulur ve bu olaydan elde edilen veriler akıllı bir sözleşmeye gönderilmelidir.

Yaygın bir örnek, finansal piyasalardan gelen verilerin ( örneğin, emtia, hisse senedi veya tahvil fiyatlarının akıllı bir sözleşmeye" ile iletilmesidir.

### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Azure Blockchain Workbench'in beklenen biçimde doğrudan teslimi

Bazı uygulamalar Azure Blockchain Workbench ile tümleştirmek için oluşturulur ve doğrudan oluşturur ve beklenen biçimlerde iletigönderir.

![Doğrudan teslimat](./media/integration-patterns/direct-delivery.png)

Bu teslim, daha önce gösterilen işlem kullanılarak oluşur:

-   Bir olay, Azure Blockchain Workbench için bir ileti oluşturulmasını tetikleyen harici bir sistemde oluşur.
-   Dış sistem, bu iletiyi bilinen bir biçimde oluşturmak için yazılmış bir koda sahiptir ve doğrudan Servis Veri Servisi'ne gönderir.
-   Azure Blockchain Workbench, Hizmet Veri Yolu'ndaki etkinliklere abone olur ve iletiyi alır.
-   Azure Blockchain Workbench, dış sistemden belirli bir sözleşmeye veri göndererek genel muhasebeye çağrı gönderir.
-   İleti alındıktan sonra, sözleşme yeni bir duruma geçiş eder.

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Azure Blockchain Workbench tarafından bilinmeyen bir biçimde ileti teslimi

Bazı sistemler, Azure Blockchain Workbench tarafından kullanılan standart biçimlerde ileti ler teslim etmek için değiştirilemez. Bu gibi durumlarda, bu sistemlerden gelen varolan mekanizmalar ve ileti biçimleri genellikle kullanılabilir. Özellikle, bu sistemlerin yerel ileti türleri, beklenen standart ileti biçimlerinden biriyle eşlenecek Şekilde Logic Apps, Azure İşlevleri veya diğer özel kod kullanılarak dönüştürülebilir.

![Bilinmeyen ileti biçimi](./media/integration-patterns/unknown-message-format.png)

Bu, daha önce gösterilen işlemi kullanarak oluşur:

-   Bir olay, ileti oluşturulmasını tetikleyen harici bir sistemde oluşur.
-   Bu iletiyi almak ve standart bir Azure Blockchain Workbench biçimlendirilmiş iletisine dönüştürmek için bir Mantık Uygulaması veya özel kod kullanılır.
-   Logic App, dönüştürülmüş iletiyi doğrudan Servis Veri Servisi'ne gönderir.
-   Azure Blockchain Workbench, Hizmet Veri Yolu'ndaki etkinliklere abone olur ve iletiyi alır.
-   Azure Blockchain Workbench, dış sistemden gelen verileri sözleşmedeki belirli bir işleve göndererek genel muhasebeye çağrı gönderir.
-   İşlev yürütür ve genellikle durumu değiştirir. Durum değişikliği, akıllı sözleşmeye yansıyan iş akışını ileriye taşıyarak diğer işlevlerin artık uygun şekilde yürütülmesini sağlar.

### <a name="transitioning-control-to-an-external-process-and-await-completion"></a>Denetimin harici bir sürece geçişi ve tamamlanmasıbekleniyor

Akıllı bir sözleşmenin dahili yürütmeyi durdurması ve dış işleme teslim etmesi gereken senaryolar vardır. Bu dış işlem daha sonra tamamlanır, akıllı sözleşmeye bir ileti gönderir ve yürütme daha sonra akıllı sözleşme içinde devam eder.

#### <a name="transition-to-the-external-process"></a>Dış işleme geçiş

Bu desen genellikle aşağıdaki yaklaşım kullanılarak uygulanır:

-   Akıllı sözleşme belirli bir duruma geçiş eder. Bu durumda, harici bir sistem istenen bir eylem icra edene kadar hiçbir veya sınırlı sayıda işlev yürütülebilir.
-   Durum değişikliği bir aşağı tüketici için bir olay olarak su yüzüne çıkar.
-   Akış aşağı tüketici olay alır ve dış kod yürütme tetikler.

![Dış işleme geçiş kontrolü](./media/integration-patterns/transition-external-process.png)

#### <a name="return-of-control-from-the-smart-contract"></a>Akıllı sözleşmeden kontrolün iadesi

Dış sistemi özelleştirme özelliğine bağlı olarak, iletileri Azure Blockchain Workbench'in beklediği standart biçimlerden birinde teslim edebilir veya teslim edemeyebilir. Dış sistemlere dayanarak bu iletilerden birini oluşturma yeteneği, aşağıdaki iki geri dönüş yolundan hangisinin alındığını belirler.

##### <a name="direct-delivery-of-an-azure-blockchain-workbench-in-the-expected-format"></a>Azure Blockchain Workbench'in beklenen biçimde doğrudan teslimi

![](./media/integration-patterns/direct-delivery.png)

Bu modelde, sözleşme ve sonraki durum değişikliği iletişim önceki süreci aşağıdaki oluşur nerede -

-   Tamamlanma veya dış kod yürütme belirli bir kilometre taşına ulaştıktan sonra, bir olay Azure Blockchain Workbench bağlı Servis Veri Yolu gönderilir.

-   API'nin beklentilerine uygun bir ileti yazmaya doğrudan adapte edilemeyen sistemler için dönüştürülür.

-   İletinin içeriği paketlenir ve akıllı sözleşmedeki belirli bir işleve gönderilir. Bu teslim, dış sistemle ilişkili kullanıcı adına yapılır.

-   İşlev yürütür ve genellikle durumu değiştirir. Durum değişikliği, akıllı sözleşmeye yansıyan iş akışını ileriye taşıyarak diğer işlevlerin artık uygun şekilde yürütülmesini sağlar.

### 

### <a name="delivery-of-a-message-in-a-format-unknown-to-azure-blockchain-workbench"></a>Azure Blockchain Workbench tarafından bilinmeyen bir biçimde ileti teslimi

![Bilinmeyen ileti biçimi](./media/integration-patterns/unknown-message-format.png)

Standart biçimdeki bir iletinin doğrudan gönderilemediği bu modelde, sözleşmeye iletişim ve sonraki durum değişikliği aşağıdaki işlemi takip eder:

1.  Tamamlanma veya dış kod yürütme belirli bir kilometre taşına ulaştıktan sonra, bir olay Azure Blockchain Workbench bağlı Servis Veri Yolu gönderilir.
2.  Bu iletiyi almak ve standart bir Azure Blockchain Workbench biçimlendirilmiş iletisine dönüştürmek için bir Mantık Uygulaması veya özel kod kullanılır.
3.  Logic App, dönüştürülmüş iletiyi doğrudan Servis Veri Servisi'ne gönderir.
4.  Azure Blockchain Workbench, Hizmet Veri Yolu'ndaki etkinliklere abone olur ve iletiyi alır.
5.  Azure Blockchain Workbench, dış sistemden belirli bir sözleşmeye veri göndererek genel muhasebeye çağrı gönderir.
6. İletinin içeriği paketlenir ve akıllı sözleşmedeki belirli bir işleve gönderilir. Bu teslim, dış sistemle ilişkili kullanıcı adına yapılır.
7.  İşlev yürütür ve genellikle durumu değiştirir. Durum değişikliği, akıllı sözleşmeye yansıyan iş akışını ileriye taşıyarak diğer işlevlerin artık uygun şekilde yürütülmesini sağlar.

## <a name="iot-integration"></a>IoT entegrasyonu

Yaygın bir tümleştirme senaryosu, sensörlerden alınan telemetri verilerinin akıllı bir sözleşmeye eklenmesidir. Sensörler tarafından sunulan verilere dayanarak, akıllı sözleşmeler bilinçli eylemlerde kalabilir ve sözleşmenin durumunu değiştirebilir.

Örneğin, ilaç teslim eden bir kamyonun sıcaklığı 110 dereceye kadar yükseliyorsa, ilacın etkinliğini etkileyebilir ve tespit edilip tedarik zincirinden çıkarıldığı takdirde bir kamu güvenliği sorununa neden olabilir. Eğer bir sürücü arabasını saatte 160 km hıza çıkarsa, ortaya çıkan sensör bilgileri sigorta sağlayıcısı tarafından sigorta iptaline neden olabilir. Araç kiralık bir araba ise, GPS verileri sürücünün kira sözleşmesi kapsamındaki bir coğrafyanın dışına ne zaman gittiğini ve ceza talep ettiğini gösterebilir.

Sorun, bu sensörlerin sürekli olarak veri sunması ve tüm bu verilerin akıllı bir sözleşmeye gönderilmesinin uygun olmamasıdır. Tipik bir yaklaşım, tüm iletileri ikincil bir mağazaya teslim ederken blockchain'e gönderilen ileti sayısını sınırlamaktır. Örneğin, yalnızca sabit aralıklarla alınan iletileri, örneğin, saat başına bir kez ve içerdiği bir değer akıllı bir sözleşme için üzerinde anlaşmaya varılan aralığın dışına düştüğünde teslim edin. Toleransların dışına çıkan değerlerin denetlenirse, sözleşmeiş mantığıyla ilgili verilerin alınmasını ve yürütülmesini sağlar. Aralıktaki değeri kontrol etmek sensörün hala rapor verdiğini doğrular. Tüm veriler, daha geniş raporlama, analitik ve makine öğrenimi sağlamak için ikincil bir raporlama deposuna gönderilir. Örneğin, akıllı bir sözleşme için GPS için sensör okumaları her dakika gerekli olmayabilir, ancak raporlarda veya haritalama rotalarında kullanılmak üzere ilginç veriler sağlayabilirler.

Azure platformunda, aygıtlarla tümleştirme genellikle IoT Hub ile yapılır. IoT Hub, iletilerin içeriğe dayalı olarak yönlendirmesini sağlar ve daha önce açıklanan işlevsellik türünü sağlar.

![IoT iletileri](./media/integration-patterns/iot.png)

İşlem bir desen tasvir eder:

-   Aygıt doğrudan veya IoT Hub'a bir alan ağ geçidi üzerinden iletişim kurar.
-   IoT Hub iletileri alır ve iletinin içeriğini kontrol eden, örneğin oluşturulan yollara karşı iletileri değerlendirir. *Sensör 50 dereceden daha yüksek bir sıcaklık bildiriyor mu?*
-   IoT Hub, ölçütleri karşılayan iletileri rota için tanımlı bir Hizmet Veri Yolu'na gönderir.
-   Bir Mantık Uygulaması veya başka bir kod, IoT Hub'ın rota için oluşturduğu Servis Veri Yolunu dinler.
-   Mantık Uygulaması veya diğer kod alır ve bilinen bir biçime ileti dönüştürmek.
-   Artık standart bir biçimde olan dönüştürülmüş ileti, Azure Blockchain Workbench için Servis Veri Yolu'na gönderilir.
-   Azure Blockchain Workbench, Hizmet Veri Yolu'ndaki etkinliklere abone olur ve iletiyi alır.
-   Azure Blockchain Workbench, dış sistemden belirli bir sözleşmeye veri göndererek genel muhasebeye çağrı gönderir.
-   İletialındıktan sonra, sözleşme verileri değerlendirir ve bu değerlendirmenin sonucuna göre durumu değiştirebilir, örneğin, yüksek sıcaklık için, durumu *Uyumsuz*olarak değiştirebilir.

## <a name="data-integration"></a>Veri tümleştirmesi

Azure Blockchain Workbench, REST ve ileti tabanlı API'ye ek olarak, uygulama ve sözleşme meta verilerinin yanı sıra dağıtılmış genel muhasebelerden gelen işlem verilerinin yanı sıra doldurulan bir SQL DB'ye de erişim sağlar.

![Veri tümleştirmesi](./media/integration-patterns/data-integration.png)

Veri tümleştirmesi iyi bilinmektedir:

-   Azure Blockchain Workbench, normal çalışma davranışının bir parçası olarak uygulamalar, iş akışları, sözleşmeler ve işlemler le ilgili meta verileri depolar.
-   Dış sistemler veya araçlar, veritabanı sunucusu adı, veritabanı adı, kimlik doğrulama türü, oturum açma kimlik bilgileri ve hangi veritabanı görünümlerini kullanmak gibi veritabanı hakkında bilgi toplamayı kolaylaştırmak için bir veya daha fazla iletişim kutusu sağlar.
-   Sorgular, dış sistemler, hizmetler, raporlama, geliştirici araçları ve kurumsal üretkenlik araçları tarafından akış aşağı tüketimini kolaylaştırmak için SQL veritabanı görünümlerine karşı yazılır.

## <a name="storage-integration"></a>Depolama entegrasyonu

Birçok senaryo, kanıtlanabilir dosyaları dahil etmek gerek gerektirebilir. Birden çok nedenden dolayı, bir blockchain dosyaları koymak için uygun değildir. Bunun yerine, yaygın bir yaklaşım, bir dosyaya karşı bir şifreleme karma (örneğin, SHA-256) gerçekleştirmek ve dağıtılmış bir genel muhasebeüzerinde karma paylaşmaktır. Karmanın ileride tekrar ifa etmesi aynı sonucu döndürmelidir. Dosya değiştirilirse, görüntüde sadece bir piksel değiştirilse bile karma farklı bir değer döndürür.

![Depolama entegrasyonu](./media/integration-patterns/storage-integration.png)

Desen nerede uygulanabilir:

-   Harici bir sistem, azure depolama gibi bir depolama mekanizmasında dosyayı devam ettir.
-   Dosya veya dosya ile birlikte karma oluşturulur ve sahibi için tanımlayıcı, dosyanın bulunduğu URL gibi ilişkili meta veriler vb.
-   Karma ve herhangi bir meta *veri, FileAdded* gibi akıllı bir sözleşmedeki bir işleve gönderilir
-   Gelecekte, dosya ve meta-veri yeniden işlenebilir ve defterde depolanan değerlerle karşılaştırılabilir.

## <a name="prerequisites-for-implementing-integration-patterns-using-the-rest-and-message-apis"></a>REST ve mesaj API'lerini kullanarak tümleştirme kalıplarını uygulamak için ön koşullar

Harici bir sistemin veya cihazın REST veya mesaj API'sini kullanarak akıllı sözleşmeyle etkileşimedebilmesini kolaylaştırmak için aşağıdakiler -

1. Konsorsiyumiçin Azure Etkin Dizini'nde, dış sistemi veya aygıtı temsil eden bir hesap oluşturulur.
2. Azure Blockchain Workbench uygulamanız için bir veya daha fazla uygun akıllı sözleşme, olayları harici sisteminizden veya cihazınızdan kabul etmek için tanımlanmış işlevlere sahiptir.
3. Akıllı sözleşmenizin uygulama yapılandırma dosyası, sistemin veya aygıtın atandığı rolü içerir.
4. Akıllı sözleşmenizin uygulama yapılandırma dosyası, bu işlevin tanımlanan rol tarafından hangi durumları adlandırıltır olduğunu tanımlar.
5. Uygulama yapılandırma dosyası ve akıllı sözleşmeleri Azure Blockchain Workbench'e yüklenir.

Uygulama yüklendikten sonra, dış sistemin Azure Etkin Dizin hesabı sözleşmeye ve ilişkili role atanır.

## <a name="testing-external-system-integration-flows-prior-to-writing-integration-code"></a>Entegrasyon Kodu Yazmadan Önce Dış Sistem Tümleştirme Akışlarını Test Etme 

Dış sistemlerle tümleştirme birçok senaryonun temel gereksinimidir. Akıllı sözleşme tasarımını, harici sistemlerle tümleştirmek için kod geliştirmeden önce veya buna paralel olarak doğrulayabilmek arzu edilir.

Azure Active Directory (Azure AD) kullanımı, geliştirici üretkenliğini ve değer verme süresini büyük ölçüde hızlandırabilir. Özellikle, harici bir sistemle kod tümleştirmesi önemsiz olmayan bir zaman alabilir. Azure AD'yi ve Azure Blockchain Workbench tarafından ux otomatik nesil kullanarak, geliştiricilerin blockchain Workbench'te dış sistem olarak oturum açmalarına ve UX üzerinden dış sistemdeki değerleri doldurmalarına izin verebilirsiniz. Dış sistemler için entegrasyon kodu yazılmadan önce fikirleri kavram ortamının kanıtı olarak hızla geliştirebilir ve doğrulayabilirsiniz.
