---
title: Azure Blockchain Workbench mimarisi
description: Azure Blockchain Çalışma Tezgahı Önizleme mimarisine ve bileşenlerine genel bakış.
ms.date: 09/05/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: aa972e8ae486d181f0c48df72ec89c925c940451
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74324891"
---
# <a name="azure-blockchain-workbench-architecture"></a>Azure Blockchain Workbench mimarisi

Azure Blockchain Workbench Preview, birkaç Azure bileşeni kullanarak bir çözüm sağlayarak blockchain uygulama geliştirmeyi kolaylaştırır. Blockchain Workbench, Azure Marketi'nde bir çözüm şablonu kullanılarak dağıtılabilir. Şablon, blockchain yığını, istemci uygulama türü ve IoT tümleştirme desteği de dahil olmak üzere dağıtmak için modülleri ve bileşenleri seçmeniz için izin verir. Blockchain Workbench dağıtıldıktan sonra bir web uygulamasına, iOS uygulamasına ve Android uygulamasına erişim sağlar.

![Blockchain Çalışma Tezgahı mimarisi](./media/architecture/architecture.png)

## <a name="identity-and-authentication"></a>Kimlik ve kimlik doğrulama

Bir konsorsiyum Blockchain Workbench'i kullanarak kurumsal kimliklerini Azure Active Directory (Azure AD) kullanarak fesatlayabilir. Workbench, Azure AD'de depolanan kurumsal kimliklerle zincirleme kimlikler için yeni kullanıcı hesapları oluşturur. Kimlik eşlemesi, istemci API'lerine ve uygulamalarına kimlik doğrulama girişinin kolaylaştırılmasını kolaylaştırır ve kuruluşların kimlik doğrulama ilkelerini kullanır. Workbench ayrıca belirli bir akıllı sözleşme içinde belirli rollere kurumsal kimlikleri ilişkilendirmek için yeteneği sağlar. Buna ek olarak, Workbench ayrıca bu rollerin ne zaman ve hangi zamanda neler yapabileceğinizi belirlemek için bir mekanizma sağlar.

Blockchain Workbench dağıtıldıktan sonra, kullanıcılar blockchain Workbench ile istemci uygulamaları, REST tabanlı istemci API veya Mesajlaşma API'si aracılığıyla etkileşimde bulunmaktadır. Her durumda etkileşimlerin Azure Etkin Dizin (Azure AD) veya aygıta özgü kimlik bilgileri aracılığıyla doğrulanması gerekir.

Kullanıcılar, katılımcılara e-posta adreslerinde bir e-posta davetiyesi göndererek kimliklerini bir konsorsiyum azure AD'ye aktarır. Oturum açarken, bu kullanıcıların adı, parolası ve ilkeleri kullanılarak kimlik doğrulaması yapılır. Örneğin, kuruluşlarının iki faktörlü kimlik doğrulaması.

Azure AD, Blockchain Workbench erişimi olan tüm kullanıcıları yönetmek için kullanılır. Akıllı bir sözleşmeye bağlanan her aygıt, Azure AD ile de ilişkilidir.

Azure AD, kullanıcıları özel bir yönetici grubuna atamak için de kullanılır. Yönetici grubuyla ilişkili kullanıcılara, sözleşmeleri dağıtma ve bir kullanıcıya sözleşmeye erişim izni verme de dahil olmak üzere Blockchain Workbench'teki haklara ve eylemlere erişim hakkı verilir. Bu grubun dışındaki kullanıcıların yönetici eylemlerine erişimi yoktur.

## <a name="client-applications"></a>İstemci uygulamaları

Workbench, blockchain uygulamalarını doğrulamak, test etmek ve görüntülemek için kullanılabilecek web ve mobil cihazlar (iOS, Android) için otomatik olarak oluşturulan istemci uygulamaları sağlar. Uygulama arabirimi akıllı sözleşme meta verilerine dayalı olarak dinamik olarak oluşturulur ve herhangi bir kullanım örneğini barındırabilir. İstemci uygulamaları Blockchain Workbench tarafından oluşturulan tam blockchain uygulamalarına kullanıcıya bakan bir ön uç sunar. İstemci uygulamaları, Azure Etkin Dizini (Azure AD) aracılığıyla kullanıcıların kimliğini doğrular ve ardından akıllı sözleşmenin iş bağlamına uygun bir kullanıcı deneyimi sunar. Kullanıcı deneyimi yetkili kişiler tarafından yeni akıllı sözleşme örnekleri oluşturulmasını sağlar ve daha sonra akıllı sözleşme temsil iş sürecinde uygun noktalarda belirli türde işlemleri yürütmek için yeteneği sunar.

Web uygulamasında, yetkili kullanıcılar Yönetici Konsolu'na erişebilir. Konsol, Azure AD'deki Yönetici grubundaki kullanıcılar tarafından kullanılabilir ve aşağıdaki işlevlere erişim sağlar:

* Microsoft'u dağıtma, popüler senaryolar için akıllı sözleşmeler sağladı. Örneğin, bir varlık transferi senaryosu.
* Kendi akıllı sözleşmelerini yükleyin ve dağıtın.
* Belirli bir rol bağlamında akıllı sözleşmeye kullanıcı erişimi atayın.

Daha fazla bilgi için [GitHub'daki Azure Blockchain Workbench örnek istemci uygulamalarına](https://github.com/Azure-Samples/blockchain-devkit/tree/master/connect/mobile)bakın.

## <a name="gateway-service-api"></a>Ağ geçidi hizmeti API'si

Blockchain Workbench, REST tabanlı ağ geçidi hizmeti API'sini içerir. Bir blockchain'e yazarken, API bir etkinlik aracısına ileti ler oluşturur ve iletler sunar. API tarafından veri istendiğinde, sorgular zincir dışı SQL veritabanına gönderilir. SQL veritabanı, desteklenen akıllı sözleşmeler için bağlam ve yapılandırma bilgileri sağlayan zincir içi verilerin ve meta verilerin bir kopyasını içerir. Sorgular, sözleşmeiçin meta veriler tarafından bilgilendirilen bir biçimde zincir dışı yinelemeden gerekli verileri döndürer.

Geliştiriciler Blockchain Workbench istemci uygulamalarına güvenmeden blockchain çözümleri oluşturmak veya tümleştirmek için ağ geçidi hizmeti API'sine erişebilir.

> [!NOTE]
> API'ye kimlik doğrulamaerişimini etkinleştirmek için Azure Active Directory'de iki istemci uygulaması kaydedilir. Azure Etkin Dizin, her uygulama türünde (yerel ve web) farklı uygulama kayıtları gerektirir. 

## <a name="message-broker-for-incoming-messages"></a>Gelen iletiler için ileti aracısı

Doğrudan Blockchain Workbench'e ileti göndermek isteyen geliştiriciler, iletileri doğrudan Servis Veri Yolu'na gönderebilir. Örneğin, iletiAPI sistemden sisteme tümleştirme veya IoT aygıtları için kullanılabilir.

## <a name="message-broker-for-downstream-consumers"></a>Downstream tüketiciler için mesaj komisyoncusu

Uygulamanın yaşam döngüsü sırasında olaylar oluşur. Olaylar Ağ Geçidi API'sı veya genel muhasebetarafından tetiklenebilir. Olay bildirimleri, olaya göre akış aşağı kodu başlatabilir.

Blockchain Workbench otomatik olarak iki tür etkinlik tüketicisi dağıtır. Bir tüketici, zincirdışı SQL mağazasını doldurmak için blockchain olayları tarafından tetiklenir. Diğer tüketici, belgelerin yüklenmesi ve saklanması ile ilgili OLARAK API tarafından oluşturulan olaylar için meta verileri yakalamaktır.

## <a name="message-consumers"></a>İleti tüketiciler

 İleti tüketiciler servis veri servisinden mesaj alır. İleti tüketicileri için temel olay modeli, ek hizmet ve sistemlerin genişletilmesine olanak tanır. Örneğin, CosmosDB'yi doldurmak veya Iletileri Azure Akış Analitiği'ni kullanarak değerlendirmek için destek ekleyebilirsiniz. Aşağıdaki bölümlerde blockchain Workbench'e dahil edilen mesaj tüketicilerin açıklayınız.

### <a name="distributed-ledger-consumer"></a>Dağıtılmış genel muhasebe tüketicisi

Dağıtılmış genel muhasebe teknolojisi (DLT) iletileri, blockchain'e yazılacak işlemler için meta verileri içerir. Tüketici iletileri alır ve verileri bir işlem oluşturucu, imzalayan ve yönlendiriciye iter.

### <a name="database-consumer"></a>Veritabanı tüketici

Veritabanı tüketicisi Service Bus'tan iletialır ve verileri SQL veritabanı gibi bağlı bir veritabanına iter.

### <a name="storage-consumer"></a>Depolama tüketicisi

Depolama tüketicisi Servis Veri Servisi'nden ileti alır ve verileri ekli bir depolama alanına iter. Örneğin, hashed belgeleri Azure Depolama'da depolama.

## <a name="transaction-builder-and-signer"></a>İşlem oluşturucu ve imzalayan

Gelen ileti aracısındaki bir iletinin blockchain'e yazılması gerekiyorsa, dlt tüketicisi tarafından işlenir. DLT tüketicisi, yürütülmesi istenen bir işlem için meta veri içeren iletiyi alan ve bilgileri *işlem oluşturucusu ve imzalayana*gönderen bir hizmettir. *İşlem oluşturucu ve imzalayan,* verilere ve istenilen blockchain hedefine dayalı bir blockchain işlemi bir araya getirmektedir. Bir kez monte edildikten sonra, hareket imzalanır. Özel anahtarlar Azure Key Vault'ta saklanır.

 Blockchain Workbench Key Vault'tan uygun özel anahtarı alır ve işlemi Key Vault'un dışında imzalar. İmzalandıktan sonra, hareket hareket yönlendiricilerine ve genel muhasebelerine gönderilir.

## <a name="transaction-routers-and-ledgers"></a>İşlem yönlendiricileri ve genel muhasebeleri

İşlem yönlendiricileri ve defterleri imzalı işlemleri alır ve bunları uygun blockchain'e yönlendirir. Şu anda Blockchain Workbench, Ethereum'u hedef blockchain olarak desteklemektedir.

## <a name="dlt-watcher"></a>DLT izleyicisi

Dağıtılmış genel muhasebe teknolojisi (DLT) izleyicisi, Blockchain Workbench'e bağlı blok zincirlerinde meydana gelen olayları izler.
Olaylar bireyler ve sistemlerle ilgili bilgileri yansıtır. Örneğin, yeni sözleşme örneklerinin oluşturulması, hareketlerin yürütülmesi ve durum değişiklikleri. Olaylar yakalanır ve giden mesaj aracısına gönderilir, böylece aşağı akım tüketiciler tarafından tüketilebilir.

Örneğin, SQL tüketicisi olayları izler, bunları tüketir ve SQL veritabanını dahil edilen değerlerle doldurur. Kopya, zincirleme bir mağazada zincir içi verilerin bir kopyasının yeniden bir ekidir.

## <a name="azure-sql-database"></a>Azure SQL veritabanı

Blockchain Workbench'e bağlı Azure SQL veritabanı sözleşme tanımlarını, yapılandırma meta verilerini ve blockchain'de depolanan verilerin SQL'e erişilebilen bir kopyasını depolar. Bu veriler, doğrudan veritabanına erişilerek kolayca sorgulanabilir, görselleştirilebilir veya çözümlenebilir. Geliştiriciler ve diğer kullanıcılar veritabanını raporlama, analitik veya diğer veri merkezli tümleştirmeler için kullanabilir. Örneğin, kullanıcılar Power BI kullanarak işlem verilerini görselleştirebilir.

Bu zincir dışı depolama, kurumsal kuruluşların blockchain genel muhasebesi yerine SQL'deki verileri sorgulama olanağı sağlar. Ayrıca, blockchain teknoloji yığınlarının agnostik standartlaştırarak, zincir dışı depolama, raporlar ve diğer yapılar projeler, senaryolar ve kuruluşlar arasında yeniden kullanılmasını sağlar.

## <a name="azure-storage"></a>Azure Storage

Azure Depolama, sözleşmelerle ilişkili sözleşmeleri ve meta verileri depolamak için kullanılır.

Satın alma siparişleri ve konşimentolar, haber ve tıbbi görüntülerde kullanılan görüntüler, polis vücut kameraları ve büyük sinema dahil olmak üzere bir süreklilik kaynaklanan video, belgeler birçok blockchain merkezli senaryolarda rol oynamaktadır. Belgeler doğrudan blockchain üzerine yerleştirmek için uygun değildir.

Blockchain Workbench, blockchain iş mantığına sahip belge veya diğer medya içeriği ekleme yeteneğini destekler. Belge nin veya medya içeriğinin bir bölümü blockchain'de depolanır ve gerçek belge veya medya içeriği Azure Depolama'da depolanır. İlişkili işlem bilgileri gelen ileti aracısına teslim edilir, paketlenir, kaydedilir ve blockchain'e yönlendirilir. Bu işlem, giden ileti aracısı aracılığıyla paylaşılan olayları tetikler. SQL DB bu bilgileri tüketir ve daha sonra sorgulama için DB'ye gönderir. Downstream sistemleri de uygun şekilde hareket etmek için bu olayları tüketebilir.

## <a name="monitoring"></a>İzleme

Workbench, Application Insights ve Azure Monitor'u kullanarak uygulama günlüğe kaydetme sağlar. Application Insights, Blockchain Workbench'ten günlüğe kaydedilmiş tüm bilgileri depolamak için kullanılır ve hatalar, uyarılar ve başarılı işlemleri içerir. Uygulama Öngörüleri, geliştiriciler tarafından Blockchain Workbench ile ilgili sorunları hata ayıklamak için kullanılabilir. 

Azure Monitor, blockchain ağının durumu hakkında bilgi sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench’i dağıtma](../../blockchain-workbench/blockchain-workbench-deploy.md)