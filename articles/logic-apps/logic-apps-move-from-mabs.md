---
title: Uygulamaları BizTalk Hizmetlerinden Azure Mantık Uygulamalarına geçirin
description: Uygulamalarınızı ve çözümlerinizi Microsoft Azure BizTalk Hizmetleri'nden (MABS) Azure Mantık Uygulamalarına taşıma
services: logic-apps
ms.suite: integration
author: jonfancey
ms.author: jonfan
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 05/30/2017
ms.openlocfilehash: 97399635399c12022006ac95e60c5828bf2a9dc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76905443"
---
# <a name="migrate-your-apps-and-solutions-from-biztalk-services-to-azure-logic-apps"></a>Uygulamalarınızı ve çözümlerinizi BizTalk Hizmetlerinden Azure Mantık Uygulamalarına geçirin

Microsoft Azure BizTalk Hizmetleri (MABS) emekli oluyor. MABS tümleştirme çözümlerinizi [Azure Logic Apps'a](../logic-apps/logic-apps-overview.md)taşımak için bu makaledeki kılavuzu izleyin. 

## <a name="introduction"></a>Giriş

BizTalk Hizmetleri iki alt hizmetlerden oluşur:

* Microsoft BizTalk Hizmetleri Karma Bağlantılar
* EAI ve EDI köprü tabanlı entegrasyon

[Azure App Service Karma Bağlantıları,](../app-service/app-service-hybrid-connections.md) BizTalk Hizmetleri Karma Bağlantılarının yerini alır. Azure Karma Bağlantıları, Azure Uygulama Hizmeti ile Azure portalı üzerinden kullanılabilir. Bu hizmet, varolan BizTalk Hizmetleri karma bağlantılarını ve portalda oluşturduğunuz yeni karma bağlantıları yönetebilmeniz için bir Karma Bağlantı Yöneticisi sağlar. 

[Logic Apps,](../logic-apps/logic-apps-overview.md) EAI ve EDI köprü tabanlı tümleştirmenin yerine BizTalk Hizmetleri ve daha fazlası için aynı yeteneklerin yerini alır. Bu hizmet, bir tarayıcı veya Visual Studio ile karmaşık tümleştirme çözümleri oluşturmanız için bulut ölçekli tüketim tabanlı iş akışı ve orkestrasyon özellikleri sağlar.

Bu tablo, BizTalk Hizmetleri yeteneklerini Logic Apps ile eşler.

| BizTalk Hizmetleri   | Logic Apps            | Amaç                      |
| ------------------ | --------------------- | ---------------------------- |
| Bağlayıcı          | Bağlayıcı             | Veri gönderme ve alma   |
| Bridge             | Logic App             | Boru hattı işlemcisi           |
| Aşamayı doğrula     | XML Doğrulama eylemi | Bir XML belgesini şemaya karşı doğrulama | 
| Sahneyi zenginleştirin       | Veri Belirteçleri           | Özellikleri iletilere veya yönlendirme kararlarına tanıtmak için |
| Aşamayı dönüştür    | Eylemi dönüştürme      | XML iletilerini bir biçimden diğerine dönüştürme |
| Decode sahne       | Düz Dosya Decode eylem | Düz dosyadan XML'ye dönüştürme |
| Sahneyi kodlama       | Düz Dosya Kodlama eylemi | XML'den düz dosyaya dönüştürme |
| İleti Denetçisi  | Azure Fonksiyonları veya API Uygulamaları | Tümleştirmelerinizde özel kod çalıştırma |
| Rota Eylemi       | Koşul veya Anahtar | İletileri belirtilen bağlayıcılardan birine yönlendirme |
|||| 

## <a name="biztalk-services-artifacts"></a>BizTalk Hizmetleri eserler

BizTalk Hizmetleri eserler çeşitli vardır. 

## <a name="connectors"></a>Bağlayıcılar

BizTalk Hizmetleri bağlayıcıları, http tabanlı istek/yanıt etkileşimlerini etkinleştiren iki yönlü köprüler de dahil olmak üzere köprülerin veri göndermesine ve almasına yardımcı olur. Logic Apps aynı terminolojiyi kullanır ve çok çeşitli teknoloji ve hizmetlere bağlanarak aynı amaca hizmet eden yüzlerce bağlayıcıya sahiptir. Örneğin, BizTalk Hizmetleri için BizTalk Bağdaştırıcı sıcağı nın yerini alan Şirket İçi Veri Ağ Geçidi aracılığıyla OneDrive, Office365, Dynamics CRM ve daha fazlası gibi bulut SaaS ve PaaS hizmetleri için konektörler kullanılabilir. BizTalk Hizmetleri'ndeki kaynaklar FTP, SFTP ve Servis Veri Günü Sırası veya Konu aboneliği ile sınırlıdır.

![](media/logic-apps-move-from-mabs/sources.png)

Varsayılan olarak, her köprünün Çalışma Zamanı Adresi ve köprü için Göreli Adres özellikleriyle yapılandırılan bir HTTP bitiş noktası vardır. Logic Apps ile aynı sonuçları elde etmek için [İstek ve Yanıt](../connectors/connectors-native-reqres.md) eylemlerini kullanın.

## <a name="xml-processing-and-bridges"></a>XML işleme ve köprüler

BizTalk Hizmetleri'nde, bir köprü işleme boru hattına benzer. Köprü, bağlayıcıdan alınan verileri alabilir, verilerle bazı işler yapabilir ve sonuçları başka bir sisteme gönderebilir. Logic Apps, BizTalk Hizmetleri ile aynı boru hattı tabanlı etkileşim kalıplarını destekleyerek ve diğer tümleştirme kalıplarını da sağlayarak aynı şeyi yapar. BizTalk Hizmetleri'ndeki [XML İstek-Yanıt Köprüsü,](https://msdn.microsoft.com/library/azure/hh689781.aspx) bu görevleri gerçekleştiren aşamalardan oluşan veter ardışık hattı olarak bilinir:

* (V) Doğrulama
* (E) Zenginleştirme
* (T) Dönüştürme
* (E) Zenginleştirme
* (R) Rotası

Bu resim, işlemenin istek ve yanıt arasında nasıl bölündüğünü gösterir, bu da istek üzerinde denetim sağlar ve yanıt yolları ayrı ayrı,örneğin, her yol için farklı haritalar kullanarak:

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Ayrıca, bir XML Tek Yönlü köprü, işlemenin başlangıcında ve sonunda Decode ve Encode aşamalarını ekler. Pass-Through köprüsü tek bir Zenginleştirme aşaması içerir.

### <a name="message-processing-decoding-and-encoding"></a>İleti işleme, kod çözme ve kodlama

BizTalk Hizmetleri'nde, farklı türde XML iletileri alabilir ve alınan iletinin eşleşen şemasını belirleyebilirsiniz. Bu çalışma, alma işleme ardışık ardışık ardışık alın *Ileti Türleri* aşamasında gerçekleştirilir. Decode aşaması daha sonra sağlanan şemayı kullanarak iletiyi çözmek için algılanan ileti türünü kullanır. Şema düz bir dosya şeması ysa, bu aşama gelen düz dosyayı XML'e dönüştürür. 

Logic Apps benzer özellikler sağlar. Farklı bağlayıcı tetikleyicileri (Dosya Sistemi, FTP, HTTP vb.) kullanarak farklı protokoller üzerinden düz bir dosya alır sınız ve gelen verileri XML'e dönüştürmek için [Düz Dosya Çözme](../logic-apps/logic-apps-enterprise-integration-flatfile.md) eylemini kullanırsınız. Varolan düz dosya şemalarınızı herhangi bir değişiklik yapmadan doğrudan Logic Apps'a taşıyabilir ve ardından Şema'ları Entegrasyon Hesabınıza yükleyebilirsiniz.

### <a name="validation"></a>Doğrulama

Gelen veriler XML'e dönüştürüldükten sonra (veya alınan ileti biçimi XML ise), iletinin XSD şemanıza uyup uymadığını belirlemek için doğrulama çalışır. Bu görevi Logic Apps'ta gerçekleştirmek için [XML Doğrulama eylemini](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) kullanın. BizTalk Hizmetleri'ndeki aynı şemaları herhangi bir değişiklik yapmadan kullanabilirsiniz.

### <a name="transform-messages"></a>İletileri dönüştürme

BizTalk Hizmetlerinde Dönüşüm aşaması, bir XML tabanlı ileti biçimini diğerine dönüştürür. Bu çalışma TRFM tabanlı mapper kullanılarak bir harita uygulanarak yapılır. Logic Apps'ta işlem benzerdir. Dönüşüm eylemi, Tümleştirme Hesabınızdan bir harita yürütür. Temel fark, Mantık Uygulamaları'ndaki haritaların XSLT formatında olmasıdır. XSLT, biztalk server için oluşturulan ve funktoidler içeren haritalar da dahil olmak üzere, zaten sahip olduğunuz mevcut XSLT'yi yeniden kullanma olanağını içerir. 

### <a name="routing-rules"></a>Yönlendirme kuralları

BizTalk Hizmetleri, gelen iletileri veya verileri göndermek için hangi uç nokta veya bağlayıcı ya da yönlendirme kararı verir. Önceden yapılandırılmış uç noktalardan seçim yapma özelliği yönlendirme filtresi seçeneğini kullanarak mümkündür:

![](media/logic-apps-move-from-mabs/route-filter.png)

BizTalk Hizmetleri'nde, yalnızca iki seçenek varsa, *bir koşul* kullanmak, BizTalk Hizmetleri'nde yönlendirme filtrelerini dönüştürmenin en iyi yoludur. İkiden fazla varsa, bir **anahtar**kullanın.

Logic Apps gelişmiş mantık özellikleri artı gelişmiş kontrol akışı ve [koşullu ifadeler](../logic-apps/logic-apps-control-flow-conditional-statement.md) ve [geçiş ifadeleri](../logic-apps/logic-apps-control-flow-switch-statement.md)ile yönlendirme sağlar.

### <a name="enrich"></a>Zenginleştirmek

BizTalk Hizmetleri işleminde, Zenginleştirme aşaması alınan verilerle ilişkili ileti bağlamına özellikler ekler. Örneğin, bir özelliği veritabanı aramasından yönlendirme için kullanmak üzere veya XPath ifadesini kullanarak bir değer ayıklayarak tanıtMak. Logic Apps, önceki eylemlerden tüm bağlamsal veri çıktılarına erişim sağlayarak aynı davranışı çoğaltmayı kolaylaştırır. Örneğin, SQL `Get Row` bağlantı eylemini kullanarak, bir SQL Server veritabanından veri döndürün ve yönlendirme için karar eyleminde verileri kullanırsınız. Aynı şekilde, gelen Hizmet Veri Yolu'ndaki özellikler bir tetikleyici tarafından sıralanmış iletilerin yanı sıra xpath iş akışı tanımı dil ifadesini kullanarak XPath adreslenebilir.

### <a name="run-custom-code"></a>Özel kodu çalıştırma

BizTalk Hizmetleri, kendi derlemelerinizde yüklenen [özel kodu çalıştırmanızı](https://msdn.microsoft.com/library/azure/dn232389.aspx) sağlar. Bu işlev [iMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector) arabirimi tarafından uygulanır. Köprüdeki her aşama, oluşturduğunuz bu arabirimi uygulayan .NET türünü sağlayan iki özellik (Enter Inspector ve On Exit Inspector) içerir. Özel kod, veriler üzerinde daha karmaşık işleme ler gerçekleştirmenize ve ortak iş mantığı gerçekleştiren derlemelerde varolan kodu yeniden kullanmanıza olanak tanır. 

Logic Apps özel kodu yürütmenin iki birincil yolunu sağlar: Azure İşlevleri ve API Uygulamaları. Azure Fonksiyonları oluşturulabilir ve mantık uygulamalarından çağrılabilir. Bkz. [Azure İşlevleri aracılığıyla mantık uygulamaları için özel kod ekleyin ve çalıştırın.](../logic-apps/logic-apps-azure-functions.md) Kendi tetikleyicilerinizi ve eylemlerinizi oluşturmak için Azure Uygulama Hizmeti'nin bir parçası olan API Uygulamalarını kullanın. [Logic Apps ile kullanmak üzere özel bir API oluşturma](../logic-apps/logic-apps-create-api-app.md)hakkında daha fazla bilgi edinin. 

BizTalk Hizmetleri'nden aradığınız derlemelerde özel kodunuz varsa, bu kodu Azure İşlevlerine taşıyabilir veya uyguladığınız şeye bağlı olarak API Uygulamaları yla özel API'ler oluşturabilirsiniz. Örneğin, Logic Apps'ın bağlayıcısı olmayan başka bir hizmeti saran bir kodunuz varsa, bir API Uygulaması oluşturun ve API uygulamanızın mantık uygulamanızda sağladığı eylemleri kullanın. Yardımcı işlevleriniz veya kitaplıklarınız varsa, Azure Fonksiyonları büyük olasılıkla en uygun olandır.

### <a name="edi-processing-and-trading-partner-management"></a>EDI işleme ve ticaret ortağı yönetimi

BizTalk Hizmetleri ve Mantık Uygulamaları AS2 (Applicability Bildirimi 2), X12 ve EDIFACT desteği ile EDI ve B2B işleme içerir. BizTalk Services'da, EDI köprüleri oluşturun ve özel İzleme ve Yönetim portalında ticaret ortakları ve anlaşmalar oluşturun veya yönetin.
Logic Apps'ta bu işlevselliği [Kurumsal Tümleştirme Paketi (EIP)](../logic-apps/logic-apps-enterprise-integration-overview.md)aracılığıyla elde elabilirsiniz. EIP, EDI ve B2B işlemleri için [Entegrasyon Hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve B2B eylemleri sağlar. Ayrıca, [ticaret ortakları](../logic-apps/logic-apps-enterprise-integration-partners.md) ve [anlaşmaları](../logic-apps/logic-apps-enterprise-integration-agreements.md)oluşturmak ve yönetmek için bir Entegrasyon Hesabı da kullanırsınız. Bir Tümleştirme Hesabı oluşturduktan sonra, bir veya daha fazla mantık uygulamalarını hesaba bağlayabilirsiniz. Daha sonra mantık uygulamanızdan ticari iş ortağı bilgilerine erişmek için B2B eylemlerini kullanabilirsiniz. Aşağıdaki eylemler sağlanır:

* AS2 Kodlama
* AS2 Şifresini Çöz
* X12 Kodla
* X12 Şifreçözme
* EDIFACT Kodlama
* EDIFACT Decode

BizTalk Hizmetlerinin aksine, bu eylemler aktarım protokollerinden ayrışılır. Bu nedenle, mantık uygulamalarınızı oluşturduğunuzda, veri göndermek ve almak için kullandığınız bağlayıcılar için daha fazla esnekliğe sahip olabilirsiniz. Örneğin, X12 dosyalarını e-postadan ek olarak alabilir ve bu dosyaları bir mantık uygulamasında işleyebilirsiniz. 

## <a name="manage-and-monitor"></a>Yönetme ve izleme

BizTalk Services'da, özel bir portal sorunları izlemek ve sorun giderme için izleme özellikleri sağladı. Logic Apps, [Azure portalındaki mantıksal uygulamaları izlemek](../logic-apps/monitor-logic-apps.md)için daha zengin izleme ve izleme özellikleri sağlar ve hareket halindeyken her şeyi izlemek için bir mobil uygulama içerir.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

BizTalk Hizmetleri'nde yüksek kullanılabilirlik (HA) için, belirli bir bölgede birden fazla örnek kullanarak işlem yükünü paylaşabilirsiniz. Logic Apps ek ücret ödemeden bölge içi HA sağlar. 

BizTalk Hizmetleri'nde, B2B işleme için bölge dışı olağanüstü durum kurtarma yedekleme ve geri yükleme işlemi gerektirir. Logic Apps, iş sürekliliği için b2B verilerini farklı bölgelerdeki tümleştirme hesapları arasında senkronize etmenizi sağlayan bölgeler arası etkin/pasif [DR özelliği](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md)sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* [Logic Apps nedir?](../logic-apps/logic-apps-overview.md)
* [İlk mantıksal uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md) veya [önceden oluşturulmuş bir şablon](../logic-apps/logic-apps-create-logic-apps-from-templates.md) kullanarak hızlı şekilde çalışmaya başlayın  
* Mantık uygulamalarında kullanabileceğiniz [tüm kullanılabilir bağlayıcıları görüntüleyin](../connectors/apis-list.md)