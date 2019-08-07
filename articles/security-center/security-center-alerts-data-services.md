---
title: Azure Güvenlik Merkezi 'nde veri Hizmetleri için tehdit algılama | Microsoft Docs
description: Bu konuda, Azure Güvenlik Merkezi 'nde bulunan veri Hizmetleri uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/24/2019
ms.author: v-mohabe
ms.openlocfilehash: 8812ac325e6bfd5ee019f6ddd6bf86c846ed5c10
ms.sourcegitcommit: f7998db5e6ba35cbf2a133174027dc8ccf8ce957
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68782458"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde veri Hizmetleri için tehdit algılama

 Güvenlik Merkezi veri depolama hizmetleri günlüklerini analiz eder ve veri kaynaklarınız için bir tehdit algıladığında uyarıları tetikler. Bu konuda, güvenlik merkezi 'nin aşağıdaki hizmetler için oluşturduğu uyarılar listelenmektedir:

* [Azure SQL veritabanı ve SQL veri ambarı](#data-sql)
* [Azure Depolama](#azure-storage)
* [Cosmos DB](#cosmos-db)

## Azure SQL veritabanı ve SQL veri ambarı<a name="data-sql"></a>

SQL tehdit algılama, veritabanlarına erişmek veya veritabanına yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri algılar. Güvenlik Merkezi, SQL denetim günlüklerini analiz eder ve SQL altyapısında yerel olarak çalışır.

|Uyarı|Açıklama|
|---|---|
|**SQL ekleme güvenlik açığı**|Bir uygulama veritabanında hatalı bir SQL açıklaması oluşturdu. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı deyim oluşturulmasının iki olası nedeni vardır: Ya da, uygulama kodundaki bir hata, hatalı SQL ifadesini oluşturulmuş. Ya da, uygulama kodu veya saklı yordamlar, SQL ekleme için yararlanılabilecek hatalı SQL ifadesini oluştururken Kullanıcı girişini temizlemeyen.|
|**Olası SQL ekleme**|SQL ekleme ile güvenlik açığı bulunan tanımlı bir uygulamaya karşı etkin bir yararlanma gerçekleşti. Bu, bir saldırganın savunmasız uygulama kodunu veya saklı yordamları kullanarak kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.|
|**Olağan dışı konumdan erişim**|SQL Server 'a erişim modelinde, birinin SQL Server 'da olağan dışı bir coğrafi konumdan oturum açtığı bir değişiklik vardı. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.|
|**Bilmediğiniz sorumludan erişim**|SQL Server 'a erişim düzeninde bir değişiklik vardı. birisi SQL Server 'da olağan dışı bir sorumlu (SQL kullanıcısı) kullanarak oturum açtı. Bazı durumlarda uyarı güvenli işlemleri (yeni uygulama ve geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı işlemleri (önceki çalışan ve şirket dışı saldırgan gibi) algılar.|
|**Zararlı olabilecek bir uygulamadan erişim**|Veritabanına erişmek için zararlı olabilecek bir uygulama kullanıldı. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, yarı yaygın saldırı araçlarının kullandığı saldırıları algılar.|
|**Deneme yanılma SQL kimlik bilgilerini zorlama**|Farklı kimlik bilgileri ile olağan dışı yüksek sayıda başarısız oturum açma işlemi oluştu. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı deneme yanılma saldırılarını algılar.|

SQL tehdit algılama uyarıları hakkında daha fazla bilgi için bkz.[Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview)ve tehdit algılama uyarıları bölümünü gözden geçirme. Ayrıca, Azure Güvenlik Merkezi 'nin bir saldırıyı saptamak için kötü amaçlı SQL etkinliği algılama özelliğinin nasıl kullanıldığını gösteren bir örnek görüntülemek için bir [siber saldırıya nasıl yardımcı olduğunu](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) görün.

## Azure depolama<a name="azure-storage"></a>

>[!NOTE]
> Azure depolama için Gelişmiş tehdit koruması Şu anda yalnızca BLOB depolama için kullanılabilir.

Azure Depolama için Gelişmiş Tehdit Koruması, depolama hesaplarına erişmeye veya güvenlik açıklarından yararlanmaya yönelik sıra dışı, zararlı olabilecek girişimleri algılayan güvenlik zekasına sahip ek bir güvenlik katmanı sağlar. Bu koruma katmanı, güvenlik uzmanı olmanıza ve güvenlik izleme sistemlerini yönetmenize gerek kalmadan tehditleri ele almanıza olanak sağlar.

Güvenlik Merkezi, tehditleri algılamak için blob depolamaya okuma, yazma ve silme isteklerinin tanılama günlüklerini analiz eder ve etkinlikteki bozukluklar gerçekleştiğinde uyarıları tetikler. Daha fazla bilgi için bkz. daha fazla bilgi için [depolama Analizi günlüğe kaydetmeyi yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging) .

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Olağan dışı konum erişim anomali**|Örneklenmiş ağ trafiği analizi, dağıtımınızdaki bir kaynaktan kaynaklanan anormal giden Uzak Masaüstü Protokolü (RDP) iletişimi algıladı. Bu etkinlik, bu ortam için anormal olarak kabul edilir ve kaynağınızın tehlikede olduğunu belirtebilir ve artık dış RDP uç noktası zorla deneme için kullanılır. Bu etkinlik türünün, IP’nizin dış varlıklar tarafından kötü amaçlı olarak işaretlenmesine neden olabileceğini unutmayın.|
|**Uygulama erişimi anomali**|Olağan dışı bir uygulamanın bu depolama hesabına eriştiği anlamına gelir. Olası bir neden, bir saldırganın yeni bir uygulama kullanarak depolama hesabınıza eriştiği bir nedendir.|
|**Anonim erişim anomali**|Bir depolama hesabına erişim modelinde değişiklik olduğunu gösterir. Örneğin, hesaba bu hesapta son erişim düzeniyle karşılaştırıldığında beklenmeyen bir şekilde (herhangi bir kimlik doğrulaması olmadan) erişilebilir. Olası bir neden, bir saldırganın BLOB depolama alanı tutan bir kapsayıcıya genel okuma erişiminin yararlanmasıdır.|
|**Tor anomali**|Bu hesaba, Tor 'ın etkin çıkış düğümü olarak bilinen bir IP adresinden başarıyla erişildiğini belirtir (bir anonim proxy). Bu uyarının önem derecesi, kullanılan (varsa) kimlik doğrulaması türünü ve bu erişimin ilk olması durumunda olup olmadığını dikkate alır. Olası nedenler, bir saldırgan tarafından, depolama hesabınıza Tor kullanılarak erişmiş olabilir veya yasal Kullanıcı, Tor kullanarak depolama hesabınıza erişmiş olabilir.|
|**Veri Taşalımı anomali**|Bu depolama kapsayıcısındaki en son etkinliğe kıyasla alışılmadık büyük miktarda verilerin ayıklandığını gösterir. Olası bir neden, bir saldırganın BLOB depolama alanı tutan bir kapsayıcıdan büyük miktarda veri ayıklamasıdır.|
|**Beklenmeyen silme anomali**|Bir depolama hesabında bir veya daha fazla beklenmeyen silme işlemi gerçekleştiğini, bu hesaptaki en son etkinliğe kıyasla olduğunu gösterir. Olası bir neden, saldırganın depolama hesabınızdan veri sildiği bir nedendir.|
|**Azure bulut hizmeti paketini karşıya yükle**|Bir Azure bulut hizmeti paketinin (. cspkg dosyası), bu hesaptaki en son etkinlikle karşılaştırıldığında, alışılmadık bir şekilde bir depolama hesabına yüklendiğini belirtir. Olası bir neden, bir saldırganın depolama hesabınızdan bir Azure bulut hizmetine kötü amaçlı kod dağıtmaya hazırlanmasıyla ilgili bir nedendir.|
|**İzin erişimi anomali**|Bu depolama kapsayıcısının erişim izinlerinin olağan dışı bir şekilde değiştirildiğini belirtir. Olası nedeni, bir saldırganın güvenlik duruşunu yumuşatmak veya kalıcılık kazanmak için kapsayıcı izinlerini değiştirleridir.|
|**Denetleme erişimi anomali**|Bir depolama hesabının erişim izinlerinin, bu hesaptaki en son etkinlikle karşılaştırıldığında olağan dışı bir şekilde incelenebileceğini belirtir. Olası bir neden, saldırganın gelecekteki bir saldırıya karşı keşif gerçekleştirmesinden dolayı gerçekleşir.|
|**Veri araştırma anomali**|Bir depolama hesabındaki Blobların veya kapsayıcıların, bu hesaptaki en son etkinlikle karşılaştırıldığında anormal bir şekilde numaralandırıldığını gösterir. Olası bir neden, saldırganın gelecekteki bir saldırıya karşı keşif gerçekleştirmesinden dolayı gerçekleşir.|

>[!NOTE]
>Azure depolama için Gelişmiş tehdit koruması Şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Depolama uyarıları hakkında daha fazla bilgi için bkz. [Azure Storage Için Gelişmiş tehdit koruması](../storage/common/storage-advanced-threat-protection.md) makalesi ve koruma uyarıları bölümünü gözden geçirme.

## Cosmos DB<a name="cosmos-db"></a>

Aşağıdaki uyarılar Azure Cosmos DB hesaba erişmek veya bu hesaplara yararlanmak için olağan dışı ve potansiyel olarak zararlı denemelere göre oluşturulmuştur:

|Uyarı|Açıklama|
|---|---|
|**Olağan dışı konumdan erişim**|Cosmos DB hesaba erişim modelinde bir değişiklik olduğunu gösterir. Birisi bu hesaba, en son etkinlikle karşılaştırıldığında, bilinmeyen bir IP adresinden erişti. Bir saldırgan bir Cosmos DB hesabına erişmiş veya yasal bir Kullanıcı yeni ve olağan dışı bir coğrafi konumdan Cosmos DB hesabına erişmiş durumda. Örneğin: yeni bir uygulama veya uzaktan geliştirici bakımı.|
|**Olağan dışı veri sızdırma**|Cosmos DB hesabından veri ayıklama düzeninde bir değişiklik olduğunu gösterir. Bir kişi, son etkinlikle karşılaştırıldığında olağan dışı miktarda veri ayıklamıştır. Bir saldırgan Cosmos DB veritabanından büyük miktarda veri ayıklamıştır. Örneğin: veri ayıklanma/sızıntı, yetkisiz veri aktarımı. Ya da yasal bir Kullanıcı ya da uygulama bir kapsayıcıdan olağan dışı miktarda veri ayıklamıştır. Örneğin: bakım yedekleme etkinliği.|

Daha fazla bilgi için bkz. [Azure Cosmos DB Için Gelişmiş tehdit koruması](../cosmos-db/cosmos-db-advanced-threat-protection.md).