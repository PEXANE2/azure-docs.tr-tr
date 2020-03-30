---
title: Azure Cosmos DB için Gelişmiş Tehdit Koruması
description: Azure Cosmos DB'nin verilerin şifrelemesini nasıl sağladığını ve nasıl uygulandığını öğrenin.
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/13/2019
ms.custom: seodec18
ms.author: memildin
author: memildin
manager: rkarlin
ms.openlocfilehash: bcc1c6ffe7cdec4aed325a67969235ae993a5109
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77614842"
---
# <a name="advanced-threat-protection-for-azure-cosmos-db-preview"></a>Azure Cosmos DB için Gelişmiş Tehdit Koruması (Önizleme)

Azure Cosmos DB için Gelişmiş Tehdit Koruması, Azure Cosmos DB hesaplarına erişmek veya bunlardan yararlanmak için olağandışı ve zararlı olabilecek girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu koruma katmanı, bir güvenlik uzmanı olmadan bile tehditleri ele almanızı ve bunları merkezi güvenlik izleme sistemleriyle tümleştirmenize olanak tanır.

Aktivitedeki anormallikler oluştuğunda güvenlik uyarıları tetiklenir. Bu güvenlik uyarıları [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/)ile tümleştirilir ve ayrıca abonelik yöneticilerine e-posta yoluyla gönderilir ve şüpheli etkinliğin ayrıntıları ve tehditlerin nasıl araştırılacak ve düzeltilenlerle ilgili öneriler bulunur.

> [!NOTE]
>
> * Azure Cosmos DB için Gelişmiş Tehdit Koruması şu anda yalnızca SQL API'sı için kullanılabilir.
> * Azure Cosmos DB için Gelişmiş Tehdit Koruması Şu anda Azure devlet ve egemen bulut bölgelerinde kullanılamıyor.

Güvenlik uyarılarının tam bir araştırma deneyimi için, tüm belgeler, kapsayıcılar ve veritabanlarındaki CRUD işlemleri de dahil olmak üzere veritabanındaki işlemleri günlüğe kaydeden [Azure Cosmos DB'de tanılama günlüğünü](https://docs.microsoft.com/azure/cosmos-db/logging)etkinleştirmenizi tavsiye ettik.

## <a name="threat-types"></a>Tehdit türleri

Azure Cosmos DB için Gelişmiş Tehdit Koruması, veritabanlarına erişmek veya veritabanlarından yararlanmak için olağandışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Şu anda aşağıdaki uyarıları tetikleyebilir:

- **Olağandışı konumlara erişim**: Bu uyarı, birisinin Azure Cosmos DB bitiş noktasına alışılmadık bir coğrafi konumdan bağlandığı bir Azure Cosmos hesabına erişim deseninde bir değişiklik olduğunda tetiklenir. Bazı durumlarda, uyarı yasal bir eylem algılar, yani yeni bir uygulama veya geliştiricinin bakım işlemi. Diğer durumlarda, uyarı eski bir çalışan, harici saldırgan, vb kötü niyetli bir eylem algılar.

- **Olağandışı veri ayıklama**: Bu uyarı, istemci bir Azure Cosmos DB hesabından olağandışı miktarda veri ayıklarken tetiklenir. Bu, hesapta depolanan tüm verileri harici bir veri deposuna aktarmak için gerçekleştirilen bazı veri sızma belirtisi olabilir.

## <a name="set-up-advanced-threat-protection"></a>Gelişmiş Tehdit Koruması Ayarlama

### <a name="set-up-atp-using-the-portal"></a>Portalı kullanarak ATP'yi ayarlama

1. Azure portalını [https://portal.azure.com](https://portal.azure.com/)' dan başlatın.

2. Azure Cosmos DB hesabından, **Ayarlar** menüsünden **Gelişmiş güvenlik'i**seçin.

    ![ATP'yi ayarlama](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp.png)

3. Gelişmiş **güvenlik** yapılandırma bıçak:

    * **A.C.** olarak ayarlamak için **Gelişmiş Tehdit Koruması** seçeneğini tıklatın.
    * Yeni veya güncelleştirilmiş Gelişmiş Tehdit Koruması ilkesini kaydetmek için **Kaydet**’e tıklayın.   

### <a name="set-up-atp-using-rest-api"></a>REST API'yi kullanarak ATP'yi ayarlama

Belirli bir Azure Cosmos DB hesabı için Gelişmiş Tehdit Koruması ayarını oluşturmak, güncelleştirmek veya almak için Rest API komutlarını kullanın.

* [Gelişmiş Tehdit Koruması - Oluştur](https://go.microsoft.com/fwlink/?linkid=2099745)
* [Gelişmiş Tehdit Koruması - Get](https://go.microsoft.com/fwlink/?linkid=2099643)

### <a name="set-up-atp-using-azure-powershell"></a>Azure PowerShell'i kullanarak ATP'yi ayarlama

Aşağıdaki PowerShell cmdlets kullanın:

* [Gelişmiş Tehdit Koruması'nı etkinleştirme](https://go.microsoft.com/fwlink/?linkid=2099607&clcid=0x409)
* [Gelişmiş Tehdit Koruması Alın](https://go.microsoft.com/fwlink/?linkid=2099608&clcid=0x409)
* [Gelişmiş Tehdit Koruması devre dışı](https://go.microsoft.com/fwlink/?linkid=2099709&clcid=0x409)

### <a name="using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanma

Gelişmiş Tehdit Koruması etkinleştirilmiş Cosmos DB'yi ayarlamak için Azure Kaynak Yöneticisi şablonu kullanın.
Daha fazla bilgi için [bkz.](https://azure.microsoft.com/resources/templates/201-cosmosdb-advanced-threat-protection-create-account/)

### <a name="using-azure-policy"></a>Azure İlkesi'ni Kullanma

Cosmos DB için Gelişmiş Tehdit Koruması sağlamak için bir Azure İlkesi kullanın.

1. Azure **İlkesi - Tanımlar** sayfasını başlatın ve **Cosmos DB** ilkesi için Gelişmiş Tehdit Koruması Dağıt'ı arayın.

    ![Arama Politikası](./media/cosmos-db-advanced-threat-protection/cosmos-db.png) 

1. **CosmosDB** için Gelişmiş Tehdit Koruması Dağıt'ı tıklatın ve sonra **Atay'ı**tıklatın.

    ![Abonelik veya Grup Seçin](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-policy.png)


1. **Kapsam** alanından üç noktayı tıklatın, bir Azure aboneliği veya kaynak grubu seçin ve sonra **Seç'i**tıklatın.

    ![İlke Tanımları Sayfası](./media/cosmos-db-advanced-threat-protection/cosmos-db-atp-details.png)


1. Diğer parametreleri girin ve **Atla'yı**tıklatın.

## <a name="manage-atp-security-alerts"></a>ATP güvenlik uyarılarını yönetme

Azure Cosmos DB etkinlik anormallikleri oluştuğunda, şüpheli güvenlik olayı yla ilgili bilgilerle birlikte bir güvenlik uyarısı tetiklenir. 

 Azure Güvenlik Merkezi'nden, mevcut [güvenlik uyarılarınızı](../security-center/security-center-alerts-overview.md)gözden geçirebilir ve yönetebilirsiniz.  Olası nedenleri ve olası tehdidi araştırmak ve azaltmak için önerilen eylemleri görüntülemek için [Güvenlik Merkezi'ndeki](https://ms.portal.azure.com/#blade/Microsoft_Azure_Security/SecurityMenuBlade/0) belirli bir uyarıyı tıklatın. Aşağıdaki resim, Güvenlik Merkezi'nde sağlanan uyarı ayrıntılarının bir örneğini gösterir.

 ![Tehdit ayrıntıları](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert-details.png)

Uyarı ayrıntıları ve önerilen eylemlerle birlikte bir e-posta bildirimi de gönderilir. Aşağıdaki resimde bir uyarı e-postası örneği gösterilmektedir.

 ![Uyarı ayrıntıları](./media/cosmos-db-advanced-threat-protection/cosmos-db-alert.png)

## <a name="cosmos-db-atp-alerts"></a>Cosmos DB ATP uyarıları

 Azure Cosmos DB hesaplarını izlerken oluşturulan uyarıların listesini görmek için Azure Güvenlik Merkezi belgelerindeki [Cosmos DB uyarıları](https://docs.microsoft.com/azure/security-center/alerts-reference#alerts-azurecosmos) bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB'de Tanılama günlüğü](cosmosdb-monitor-resource-logs.md) hakkında daha fazla bilgi edinin
* [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) hakkında daha fazla bilgi edinin
