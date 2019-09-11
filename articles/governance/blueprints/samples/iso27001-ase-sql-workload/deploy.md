---
title: Örnek-ISO 27001 Ao/SQL iş yükü şema-Deploy adımları
description: ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması örneği için adımları dağıtın.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: fb68b2f78e48dabf5b1377b7c811840a85604f53
ms.sourcegitcommit: b7b0d9f25418b78e1ae562c525e7d7412fcc7ba0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70802436"
---
# <a name="deploy-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması örneğini dağıtma

Azure şemaları ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması ' nı dağıtmak için aşağıdaki adımlar gerçekleştirilmelidir:

> [!div class="checklist"]
> - [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneğini dağıtma
> - Örnekten yeni bir şema oluştur
> - Örnek kopyanızı **yayımlandı** olarak işaretleyin
> - Şema kopyanızı mevcut bir aboneliğe atama

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturun.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 paylaşılan hizmetleri şema örneğini dağıtma

Bu şema örneği dağıtılmadan önce [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneğinin hedef aboneliğe dağıtılması gerekir. ISO 27001 paylaşılan hizmetleri şema örneğinin başarılı bir şekilde dağıtılması olmadan, bu şema örneğinde altyapı bağımlılıkları eksik olur ve dağıtım sırasında başarısız olur.

> [!IMPORTANT]
> Bu şema örneği [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneği ile aynı abonelikte atanmalıdır.

## <a name="create-blueprint-from-sample"></a>Örnekten şema oluştur

İlk olarak, bir başlangıç olarak örneği kullanarak ortamınızda yeni bir şema oluşturarak şema örneğini uygulayın.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Soldaki **Başlarken** sayfasında, şema _Oluştur_altında **Oluştur** düğmesini seçin.

1. **ISO 27001 'yi bulun: Ao/SQL iş** yükü şeması _diğer örnekler_ altında bir örnek ve **Bu örneği kullan**' ı seçin.

1. Şema örneği _hakkında temel bilgileri_ girin:

   - **Blueprint adı**: ISO 27001 Ao/SQL iş yükü şeması örneğinin kopyasına bir ad verin.
   - **Tanım konumu**: Üç noktayı kullanın ve örnek kopyanızı ' e kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _yapıtlar_ sekmesini veya **ileri ' yi seçin: Sayfanın** alt kısmındaki yapıtlar.

1. Şema örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunda, daha sonra tanımlayacağımız parametreler vardır. Şema örneğini gözden geçirmeyi bitirdiğinizde **Taslağı kaydet** ' i seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı Yayımla

Şema örneğinin kopyası artık ortamınızda oluşturulmuştur. **Taslak** modunda oluşturulur ve atanmadan ve dağıtılmadan önce **yayımlanmaları** gerekir. Şema örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik bu değişikliği ISO 27001 standbundan uzağa taşıyabilir.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Sayfanın üst kısmındaki şemayı **Yayımla** ' yı seçin. Sağ taraftaki yeni sayfada, şema örneğinin kopyası için bir **Sürüm** belirtin. Daha sonra bir değişiklik yaparsanız, bu özellik için faydalıdır. "ISO 27001 şema örneğinden yayımlanan ilk sürüm" gibi **değişiklik notları** sağlayın. Ardından sayfanın alt kısmında **Yayımla** ' yı seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Şema örneğinin kopyası başarıyla **yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneliğe atanabilir. Bu adım, her bir şema örneğinin kopyasının her dağıtımını yapmak için parametrelerin sağlandığı yerdir.

1. Sol bölmedeki **tüm hizmetler** ' i seçin. **Şemaları**arayın ve seçin.

1. Sol taraftaki **Blueprint tanımları** sayfasını seçin. Şemayı kullanarak şema örneğinin kopyasını bulun ve ardından seçin.

1. Şema tanım sayfasının en üstünde şema **ata** ' yı seçin.

1. Şema atamasının parametre değerlerini sağlayın:

   - Temel

     - **Abonelikler**: Şema örneğinin kopyasına kaydettiğiniz yönetim grubundaki bir veya daha fazla abonelik seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: Ad, BLUEPRINT adına göre önceden doldurulur.
       Gerektiğinde değiştirin veya olduğu gibi bırakın.
     - **Konum**: Yönetilen kimliğin oluşturulacağı bölgeyi seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Blueprint tanım sürümü**: Şema örneğinin kopyasının **yayınlanmış** bir sürümünü seçin.

   - Atamayı Kilitle

     Ortamınız için BLUEPRINT Lock ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistem tarafından _atanmış_ yönetilen kimlik seçeneğini bırakın.

   - Şema parametreleri

     Bu bölümde tanımlanan parametreler, tutarlılığı sağlamak için şema tanımındaki birçok yapıtlar tarafından kullanılır.

     - **Kuruluş adı**: Kuruluşunuz için kısa bir ad girin. Bu özellik birincil olarak adlandırma kaynakları için kullanılır.
     - **Paylaşılan hizmet ABONELIĞI kimliği**: [ISO 27001 paylaşılan hizmetleri](../iso27001-shared/index.md) şema örneğinin atandığı abonelik kimliği.
     - **Varsayılan alt ağ adresi ön eki**: Sanal ağ varsayılan alt ağının CıDR gösterimi.
       Varsayılan değer _10.1.0.0/16_' dır.
     - **Iş yükü konumu**: Yapıtların hangi konuma dağıtıldığını belirler. Tüm hizmetler tüm konumlarda kullanılabilir değildir. Bu tür hizmetleri dağıtan yapıtlar, bu yapıtın dağıtılacağı konum için bir parametre seçeneği sağlar.

   - Yapıt parametreleri

     Bu bölümde tanımlanan parametreler, tanımlanan yapıt için geçerlidir. Bu parametreler, Blueprint atama sırasında tanımlandıklarından [dinamik parametrelerdir](../../concepts/parameters.md#dynamic-parameters) . Tam liste veya yapıt parametreleri ve açıklamaları için bkz. [yapıt parametreleri tablosu](#artifact-parameters-table).

1. Tüm parametreler girildikten sonra sayfanın alt kısmındaki **ata** ' yı seçin. Şema ataması oluşturulur ve yapıt dağıtımı başlar. Dağıtım kabaca bir saat sürer. Dağıtımın durumunu denetlemek için, BLUEPRINT atamasını açın.

> [!WARNING]
> Azure şemaları hizmeti ve yerleşik şema örnekleri **ücretsiz olarak ücretsizdir**. Azure kaynakları [ürüne göre fiyatlandırılır](https://azure.microsoft.com/pricing/). Bu şema örneği tarafından dağıtılan çalışan kaynakların maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapıt parametreleri tablosu

Aşağıdaki tabloda, şema yapıt parametrelerinin bir listesi verilmiştir:

|Yapıt adı|Yapıt türü|Parametre adı|Açıklama|
|-|-|-|-|
|Log Analytics kaynak grubu|Resource group|Name|**Kilitli** -kaynak grubunu benzersiz hale getirmek `-workload-log-rg` için **kuruluş adını** ile birleştirir.|
|Log Analytics kaynak grubu|Resource group|Location|**Kilitli** -şema parametresini kullanır.|
|Log Analytics şablonu|Resource Manager şablonu|Hizmet katmanı|Log Analytics çalışma alanının katmanını ayarlar. Varsayılan değer _PerNode_değeridir.|
|Log Analytics şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Log Analytics şablonu|Resource Manager şablonu|Location|Log Analytics çalışma alanını oluşturmak için kullanılan bölge. Varsayılan değer _Batı ABD 2_.|
|Ağ kaynak grubu|Resource group|Name|**Kilitli** -kaynak grubunu benzersiz hale getirmek `-workload-net-rg` için **kuruluş adını** ile birleştirir.|
|Ağ kaynak grubu|Resource group|Location|**Kilitli** -şema parametresini kullanır.|
|Ağ Güvenlik Grubu şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Azure güvenlik duvarı özel IP'si|[Azure Güvenlik duvarının](../../../../firewall/overview.md)özel IP 'sini yapılandırır. ISO 27001 ' de _tanımlanan CIDR gösteriminin bir parçası olmalıdır: Paylaşılan Hizmetler_ yapıt parametresi **Azure Güvenlik Duvarı alt ağ adresi ön eki**. Varsayılan değer _10.0.4.4_' dir.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Paylaşılan hizmetler Abonelik kimliği|Bir Iş yükü ve paylaşılan hizmetler arasında VNET eşlemesini etkinleştirmek için kullanılan değer.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Sanal Ağ adresi ön eki|Sanal ağın CıDR gösterimi. Varsayılan değer _10.1.0.0/16_' dır.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|Varsayılan alt ağ adresi ön eki|Sanal ağ varsayılan alt ağının CıDR gösterimi. Varsayılan değer _10.1.0.0/16_' dır.|
|Sanal ağ ve yol tablosu şablonu|Resource Manager şablonu|IP adresi ekler|İlk VM 'yi ekleyen IP adresi. Bu değer özel VNET DNS olarak kullanılır.|
|Key Vault kaynak grubu|Resource group|Name|**Kilitli** -kaynak grubunu benzersiz hale getirmek `-workload-kv-rg` için **kuruluş adını** ile birleştirir.|
|Key Vault kaynak grubu|Resource group|Location|**Kilitli** -şema parametresini kullanır.|
|Key Vault şablonu|Resource Manager şablonu|AAD nesnesi kimliği|Key Vault örneğine erişim gerektiren hesabın AAD nesne tanımlayıcısı. Varsayılan değer olmadığından boş bırakılamaz. Azure portal bu değeri bulmak için, _Hizmetler_altında "kullanıcılar" ı arayın ve seçin. Hesap adını filtrelemek için _ad_ kutusunu kullanın ve bu hesabı seçin. _Kullanıcı profili_ sayfasında, _nesne kimliğinin_yanındaki "kopyalamak için tıklayın" simgesini seçin.|
|Key Vault şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Key Vault şablonu|Resource Manager şablonu|Key Vault SKU'su|Oluşturulan Key Vault SKU 'sunu belirtir. Varsayılan değer _Premium_' dur.|
|Key Vault şablonu|Resource Manager şablonu|Azure SQL Server yönetici kullanıcı adı|Azure SQL Server erişmek için kullanılan Kullanıcı adı. **Azure SQL veritabanı şablonundaki**aynı özellik değeriyle eşleşmelidir. Varsayılan değer _SQL-admin-User_değeridir.|
|Azure SQL veritabanı kaynak grubu|Resource group|Name|**Kilitli** -kaynak grubunu benzersiz hale getirmek `-workload-azsql-rg` için **kuruluş adını** ile birleştirir.|
|Azure SQL veritabanı kaynak grubu|Resource group|Location|**Kilitli** -şema parametresini kullanır.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici kullanıcı adı|Azure SQL Server için Kullanıcı adı. **Key Vault şablonunda**aynı özellik değeriyle eşleşmelidir. Varsayılan değer _SQL-admin-User_değeridir.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici parolası (Key Vault kaynak KIMLIĞI)|Key Vault kaynak KIMLIĞI. "/Subscription/{SubscriptionID}/resourceGroups/{orgName}-Workload-kV/Providers/Microsoft.KeyVault/Vaults/{orgName}-Workload-kV" kullanın ve abonelik `{subscriptionId}` kimliğinizle ve `{orgName}` **kuruluş adıyla** değiştirin Şema parametresi.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici parolası (Key Vault gizli dizi adı)|SQL Server yöneticisinin Kullanıcı adı. **Azure SQL Server Yönetici Kullanıcı adı** **Key Vault şablon** özelliğindeki değerle eşleşmesi gerekir.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Gün cinsinden günlük tutma süresi|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|AAD yönetim nesnesi KIMLIĞI|Active Directory yöneticisi olarak atanacak kullanıcının AAD nesne KIMLIĞI. Varsayılan değer olmadığından boş bırakılamaz. Azure portal bu değeri bulmak için, _Hizmetler_altında "kullanıcılar" ı arayın ve seçin. Hesap adını filtrelemek için _ad_ kutusunu kullanın ve bu hesabı seçin. _Kullanıcı profili_ sayfasında, _nesne kimliğinin_yanındaki "kopyalamak için tıklayın" simgesini seçin.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|AAD yönetici oturumu açma|Şu anda Microsoft hesapları (örneğin live.com veya outlook.com) yönetici olarak ayarlanamaz. Yalnızca kuruluşunuzdaki kullanıcılar ve güvenlik grupları yönetici olarak ayarlanabilir. Varsayılan değer olmadığından boş bırakılamaz. Azure portal bu değeri bulmak için, _Hizmetler_altında "kullanıcılar" ı arayın ve seçin. Hesap adını filtrelemek için _ad_ kutusunu kullanın ve bu hesabı seçin. _Kullanıcı profili_ sayfasında, _Kullanıcı adını_kopyalayın.|
|App Service Ortamı kaynak grubu|Resource group|Name|**Kilitli** -kaynak grubunu benzersiz hale getirmek `-workload-ase-rg` için **kuruluş adını** ile birleştirir.|
|App Service Ortamı kaynak grubu|Resource group|Location|**Kilitli** -şema parametresini kullanır.|
|App Service Ortamı şablonu|Resource Manager şablonu|Etki alanı adı|Örnek tarafından oluşturulan Active Directory adı. Varsayılan değer _contoso.com_' dir.|
|App Service Ortamı şablonu|Resource Manager şablonu|Ao konumu|App Service Ortamı konumu. Varsayılan değer _Batı ABD 2_.|
|App Service Ortamı şablonu|Resource Manager şablonu|Günlük tutma Application Gateway gün cinsinden|Gün cinsinden veri saklama. Varsayılan değer _365_' dir.|

## <a name="next-steps"></a>Sonraki adımlar

Artık ISO 27001 App Service Ortamı/SQL veritabanı iş yükü şeması örneğini dağıtma adımlarını gözden geçirdiğinize göre, mimari ve denetim eşlemesi hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 App Service ortamı/SQL veritabanı iş yükü şeması-genel bakış](./index.md)
> [ISO 27001 App Service ortamı/SQL veritabanı iş yükü şeması-denetim eşleme](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.