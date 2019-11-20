---
title: Azure Güvenlik Merkezi 'nde veri Hizmetleri için tehdit algılama | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi 'nde bulunan veri Hizmetleri uyarıları sunulmaktadır.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: da960861-0b6c-4d80-932d-898cdebb4f83
ms.service: security-center
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: memildin
ms.openlocfilehash: 8b99d89e8895cd1c8d8e9fe3961f0db9855fb7ee
ms.sourcegitcommit: 8e31a82c6da2ee8dafa58ea58ca4a7dd3ceb6132
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74196116"
---
# <a name="threat-detection-for-data-services-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde veri Hizmetleri için tehdit algılama

 Azure Güvenlik Merkezi, veri depolama hizmetlerinin günlüklerini analiz eder ve veri kaynaklarınız için bir tehdit algıladığında uyarıları tetikler. Bu makalede, güvenlik merkezi 'nin aşağıdaki hizmetler için oluşturduğu uyarılar listelenmektedir:

* [Azure SQL veritabanı ve Azure SQL veri ambarı](#data-sql)
* [Azure depolama alanı](#azure-storage)
* [Azure Cosmos DB](#cosmos-db)

## SQL veritabanı ve SQL veri ambarı<a name="data-sql"></a>

SQL tehdit algılama, veritabanlarına erişmek veya veritabanına yararlanmak için olağandışı ve potansiyel olarak zararlı girişimleri gösteren anormal etkinlikleri tanımlar. 

|Uyarı|Açıklama|
|---|---|
|**SQL ekleme için olası bir güvenlik açığı**|Bir uygulama veritabanında hatalı bir SQL açıklaması oluşturdu. Bu, SQL ekleme saldırılarına karşı olası bir güvenlik açığını gösterebilir. Hatalı bir bildirimin iki olası nedeni vardır. Uygulama kodundaki bir hata, hatalı SQL ifadesini oluşturulmuş olabilir. Ya da, uygulama kodu veya saklı yordamlar, SQL ekleme için yararlanılabilen hatalı SQL ifadesini oluştururken Kullanıcı girişini temizlemeyen.|
|**Olası SQL ekleme**|SQL ekleme ile güvenlik açığı bulunan tanımlı bir uygulamaya karşı etkin bir yararlanma gerçekleşti. Bu, bir saldırganın savunmasız uygulama kodunu veya saklı yordamları kullanarak kötü amaçlı SQL deyimleri eklemeye çalıştığı anlamına gelir.|
|**Olağan dışı bir konumdan oturum açma**|SQL Server erişim modelinde, birinin sunucuda olağan dışı bir coğrafi konumdan oturum açmış olduğu bir değişiklik vardı. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı bir eylem (eski bir çalışan veya dış saldırgan) algılar.|
|**Tanıdık bir sorumlu tarafından oturum açma**|SQL Server erişim deseninin bir değişikliği vardı. Bir kişi, olağan dışı bir sorumlu (Kullanıcı) kullanarak sunucuda oturum açtı. Bazı durumlarda uyarı güvenli işlemleri (yeni bir uygulama veya geliştirici bakımı gibi) de algılar. Diğer durumlarda, uyarı kötü amaçlı bir eylem (eski bir çalışan veya dış saldırgan) algılar.|
|**Zararlı olabilecek bir uygulama tarafından oturum açılmaya çalışıldı**|Veritabanına erişmek için zararlı olabilecek bir uygulama kullanıldı. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı ortak araçları kullanan bir saldırı algılar.|
|**Olası SQL deneme yanılma denemesi**|Farklı kimlik bilgileri ile anormal yüksek sayıda başarısız oturum açma işlemi oluştu. Bazı durumlarda, uyarı güvenlik testlerini algılar. Diğer durumlarda, uyarı bir deneme yanılma saldırısı algılar.|

SQL tehdit algılama uyarıları hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview). Özellikle, tehdit algılama uyarıları bölümünü gözden geçirin. Ayrıca, Azure Güvenlik Merkezi 'nin bir saldırıyı saptamak için kötü amaçlı SQL etkinliği algılama özelliğinin nasıl kullanıldığını gösteren bir örnek görüntülemek için bir [siber saldırıya nasıl yardımcı olduğunu](https://azure.microsoft.com/blog/how-azure-security-center-helps-reveal-a-cyberattack/) görün.

## Azure depolama<a name="azure-storage"></a>

>[!NOTE]
> Depolama için Gelişmiş tehdit koruması Şu anda yalnızca BLOB depolama için kullanılabilir.

Depolama için Gelişmiş tehdit koruması, depolama hesaplarına yönelik olağan dışı ve potansiyel olarak zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, güvenlik uzmanı olmanıza ve güvenlik izleme sistemlerini yönetmenize gerek kalmadan tehditleri ele almanıza olanak sağlar.

Güvenlik Merkezi, tehditleri algılamak için blob depolamaya okuma, yazma ve silme isteklerinin tanılama günlüklerini analiz eder ve etkinlikteki bozukluklar gerçekleştiğinde uyarıları tetikler. Daha fazla bilgi için bkz. [depolama Analizi günlüğe kaydetmeyi yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account#configure-logging).

> [!div class="mx-tableFixed"]

|Uyarı|Açıklama|
|---|---|
|**Olağan dışı bir konumdan depolama hesabına erişim**|Azure depolama hesabına erişim modelinde bir değişiklik olduğunu gösterir. Bu hesaba, son etkinlikle karşılaştırıldığında, bilmediğiniz bir IP adresinden erişen bir IP adresi. Bir saldırgan hesaba erişim elde etti ya da yasal bir Kullanıcı yeni veya olağan dışı bir coğrafi konumdan bağlandı. İkinci bir örnek, yeni bir uygulama veya geliştiriciden uzaktan bakım örneğidir.|
|**Olağan dışı uygulama bir depolama hesabına erişti**|Olağan dışı bir uygulamanın bu depolama hesabına eriştiği anlamına gelir. Olası bir neden, bir saldırganın yeni bir uygulama kullanarak depolama hesabınıza eriştiği bir nedendir.|
|**Bir depolama hesabına anonim erişim**|Bir depolama hesabına erişim modelinde değişiklik olduğunu gösterir. Örneğin, hesaba bu hesapta son erişim düzeniyle karşılaştırıldığında beklenmeyen bir şekilde (herhangi bir kimlik doğrulaması olmadan) erişilebilir. Olası bir neden, saldırganın blob depolamayı tutan bir kapsayıcıya genel okuma erişiminin yararlanmasıdır.|
|**Bir Tor çıkış düğümünden depolama hesabına erişim**|Bu hesaba, Tor 'ın etkin çıkış düğümü olarak bilinen bir IP adresinden başarıyla erişildiğini belirtir (bir anonim proxy). Bu uyarının önem derecesi, kullanılan (varsa) kimlik doğrulaması türünü ve bu erişimin ilk olması durumunda olup olmadığını dikkate alır. Olası nedenler, Tor kullanarak depolama hesabınıza erişen bir saldırgan veya Tor kullanarak depolama hesabınıza erişen meşru bir kullanıcı olabilir.|
|**Depolama hesabından ayıklanan olağan dışı miktarda veri**|Bu depolama kapsayıcısındaki en son etkinliğe kıyasla alışılmadık büyük miktarda verilerin ayıklandığını gösterir. Olası bir neden, bir saldırganın blob depolamayı tutan bir kapsayıcıdan büyük miktarda veri ayıklamasıdır.|
|**Depolama hesabında olağan dışı silme**|Bir depolama hesabında bir veya daha fazla beklenmeyen silme işlemi gerçekleştiğini, bu hesaptaki en son etkinliğe kıyasla olduğunu gösterir. Olası bir neden, saldırganın depolama hesabınızdan veri sildiği bir nedendir.|
|**Bir depolama hesabına olağan dışı. cspkg yüklemesi**|Azure Cloud Services paketinin (. cspkg dosyası), bu hesaptaki en son etkinlikle karşılaştırıldığında, alışılmadık bir şekilde bir depolama hesabına yüklendiğini belirtir. Olası bir neden, bir saldırganın depolama hesabınızdan bir Azure bulut hizmetine kötü amaçlı kod dağıtmaya hazırlanmasıyla ilgili bir nedendir.|
|**Bir depolama hesabında olağan dışı erişim izinleri değişikliği**|Bu depolama kapsayıcısının erişim izinlerinin olağan dışı bir şekilde değiştirildiğini belirtir. Olası bir neden, saldırganın güvenlik duruşunu yumuşatmak veya kalıcılık kazanmak için kapsayıcı izinlerini değiştirleridir.|
|**Depolama hesabında olağan dışı erişim incelemesi**|Bir depolama hesabının erişim izinlerinin, bu hesaptaki en son etkinlikle karşılaştırıldığında olağan dışı bir şekilde incelenebileceğini belirtir. Olası bir neden, saldırganın gelecekteki bir saldırıya karşı keşif gerçekleştirmesinden dolayı gerçekleşir.|
|**Depolama hesabındaki olağan dışı veri araştırması**|Bir depolama hesabındaki Blobların veya kapsayıcıların, bu hesaptaki en son etkinlikle karşılaştırıldığında anormal bir şekilde numaralandırıldığını gösterir. Olası bir neden, saldırganın gelecekteki bir saldırıya karşı keşif gerçekleştirmesinden dolayı gerçekleşir.|
|**ÖNIZLEME-bir depolama hesabına yüklenen olası kötü amaçlı yazılımlar**|Potansiyel kötü amaçlı yazılım içeren bir Blobun bir depolama hesabına yüklendiğini belirtir. Olası nedenler, meşru bir kullanıcı tarafından bir saldırgan veya kötü amaçlı bir Blobun yanlışlıkla karşıya yüklenmesi olabilir.|

>[!NOTE]
>Depolama için Gelişmiş tehdit koruması Şu anda Azure Kamu ve bağımsız bulut bölgelerinde kullanılamaz.

Depolama uyarıları hakkında daha fazla bilgi için bkz. [Azure depolama Için Gelişmiş tehdit koruması](../storage/common/storage-advanced-threat-protection.md). Özellikle, "koruma uyarıları" bölümünü gözden geçirin.

## Azure Cosmos DB<a name="cosmos-db"></a>

Aşağıdaki uyarılar Azure Cosmos DB hesaba erişmek veya bu hesaplara yararlanmak için olağan dışı ve potansiyel olarak zararlı denemelere göre oluşturulmuştur:

|Uyarı|Açıklama|
|---|---|
|**Olağan dışı bir konumdan Cosmos DB hesaba erişim**|Azure Cosmos DB hesaba erişim modelinde bir değişiklik olduğunu gösterir. Birisi bu hesaba, en son etkinlikle karşılaştırıldığında, bilinmeyen bir IP adresinden erişti. Bir saldırgan hesaba erişti ya da yasal bir Kullanıcı yeni ve olağan dışı bir coğrafi konumdan erişti. İkinci bir örnek, yeni bir uygulama veya geliştiriciden uzaktan bakım örneğidir.|
|**Cosmos DB hesabından ayıklanan olağan dışı miktarda veri**|Azure Cosmos DB hesabından veri ayıklama düzeninde bir değişiklik olduğunu gösterir. Bir kişi, son etkinlikle karşılaştırıldığında olağan dışı miktarda veri ayıklamıştır. Bir saldırgan Azure Cosmos DB veritabanından büyük miktarda veri ayıklamıştır (örneğin, veri veya sızıntı ya da bir yetkisiz veri aktarımı). Ya da, yasal bir kullanıcı veya uygulama bir kapsayıcıdan olağan dışı miktarda veri ayıklamıştır (örneğin, bakım yedekleme etkinliği için).|

Daha fazla bilgi için bkz. [Azure Cosmos DB Için Gelişmiş tehdit koruması](../cosmos-db/cosmos-db-advanced-threat-protection.md).
