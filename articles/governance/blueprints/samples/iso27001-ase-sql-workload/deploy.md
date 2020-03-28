---
title: ISO 27001 ASE/SQL iş yükü planı örneğini dağıtın
description: PLAN yapı parametresi ayrıntılarını içeren ISO 27001 App Service Environment/SQL Database iş yükü planı örneğinin adımlarını dağıtın.
ms.date: 01/13/2020
ms.topic: sample
ms.openlocfilehash: 6b8f3b753f1dd8cfbc247a77f2004e3c4d3423bb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75922572"
---
# <a name="deploy-the-iso-27001-app-service-environmentsql-database-workload-blueprint-sample"></a>ISO 27001 App Service Environment/SQL Database iş yükü planı örneğini dağıtma

Azure Planları ISO 27001 App Service Environment/SQL Database iş yükü planı örneğini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) plan örneğini dağıtın
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 Paylaşılan Hizmetler plan örneğini dağıtın

Bu plan örneğinin dağıtılaabilmesi için [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) plan örneğinin hedef aboneye dağıtılması gerekir. ISO 27001 Paylaşılan Hizmetler plan örneğinin başarılı bir şekilde dağıtılması olmadan, bu plan örneği altyapı bağımlılıkları eksik olur ve dağıtım sırasında başarısız olur.

> [!IMPORTANT]
> Bu plan [örneği, ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) planı örneğiyle aynı abonelikte atanmalıdır.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. _Diğer Örnekler_ altında **ISO 27001: ASE/SQL İş Yükü** planı örneğini bulun ve bu örneği **kullanın'ı**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: ISO 27001 ASE/SQL iş yükü planı örneğinin kopyasınız için bir ad verin.
   - **Tanım yeri**: Elipsleri kullanın ve numunenin kopyasını kaydetmek için yönetim grubunu seçin.

1. Sayfanın üst kısmındaki _Eserler_ sekmesini veya **Sonraki: Sayfanın** altındaki Eserler sekmesini seçin.

1. Plan örneğini oluşturan yapıtların listesini gözden geçirin. Yapıtların çoğunun daha sonra tanımlayabildiğimiz parametreleri var. Plan örneğini gözden geçirmeyi bitirdiğinizde **Taslağı Kaydet'i** seçin.

## <a name="publish-the-sample-copy"></a>Örnek kopyayı yayımla

Plan örneğinin kopyası artık ortamınızda oluşturuldu. **Taslak** modunda oluşturulur ve atanıp dağıtılmadan önce **yayımlanmalıdır.** Plan örneğinin kopyası ortamınıza ve gereksinimlerinize göre özelleştirilebilir, ancak bu değişiklik onu ISO 27001 standardından uzaklaştırabilir.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Sayfanın üst kısmında **ki planı Yayımla'yı** seçin. Sağdaki yeni sayfada, plan örneğinin kopyası için bir **Sürüm** sağlayın. Bu özellik, daha sonra bir değişiklik yaparsanız için yararlıdır. "ISO 27001 plan örneğinden yayınlanan ilk sürüm" gibi **Değişiklik notları** sağlayın. Ardından sayfanın altındaki **Yayımla'yı** seçin.

## <a name="assign-the-sample-copy"></a>Örnek kopyayı atama

Plan örneğinin kopyası başarıyla **Yayımlandıktan**sonra, kaydedildiği yönetim grubu içindeki bir aboneye atanabilir. Bu adım, plan örneğinin her kopyasını benzersiz hale getirmek için parametrelerin sağlandığı adımdır.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Blueprint tanımları** sayfasını seçin. Plan örneğinin kopyasını bulmak ve sonra seçmek için filtreleri kullanın.

1. Plan tanımı sayfasının üst **kısmındaki plan atay'ı** seçin.

1. Plan ataması için parametre değerlerini sağlayın:

   - Temel Bilgiler

     - **Abonelikler**: Plan örneğinin kopyasını kaydettiğiniz yönetim grubundaki aboneliklerden birini veya birkaçını seçin. Birden fazla abonelik seçerseniz, girilen parametreleri kullanarak her biri için bir atama oluşturulur.
     - **Atama adı**: Adı, planın adına göre sizin için önceden doldurulur.
       Gerektiği gibi değiştirin veya olduğu gibi ayrılın.
     - **Konum**: Oluşturulan yönetilen kimlik için bir bölge seçin. Azure Blueprint bu yönetilen kimliği kullanarak tüm yapıtları atanmış şemaya dağıtır. Daha fazla bilgi için bkz. [Azure kaynakları için yönetilen kimlikler](../../../../active-directory/managed-identities-azure-resources/overview.md).
     - **Blueprint tanımı sürümü**: Plan örneğinin kopyanızın **Yayınlanmış** sürümünü seçin.

   - Kilit Ataması

     Ortamınız için plan kilidi ayarını seçin. Daha fazla bilgi için bkz. [şema kaynağı kilitleme](../../concepts/resource-locking.md).

   - Yönetilen Kimlik

     Varsayılan sistemi yönetilen kimlik _seçeneğiatanmış_ bırakın.

   - Şema parametreleri

     Bu bölümde tanımlanan parametreler tutarlılık sağlamak için plan tanımındaki birçok yapıt tarafından kullanılır.

     - **Kuruluş adı**: Kuruluşunuz için bir kısa ad girin. Bu özellik öncelikle kaynakları adlandırmak için kullanılır.
     - **Paylaşılan Hizmet Abonelik Kimliği**: [ISO 27001 Paylaşılan Hizmetler](../iso27001-shared/index.md) plan örneğinin atandığı abonelik kimliği.
     - **Varsayılan alt ağ adresi öneki**: Sanal ağ varsayılan alt ağı için CIDR gösterimi.
       Varsayılan değer _10.1.0.0/16'dır._
     - **İş yükü konumu**: Yapıtların hangi konuma dağıtılan yeri belirler. Tüm hizmetler tüm konumlarda kullanılamaz. Bu tür hizmetleri dağıtan yapılar, söz konusu yapıyı dağıtmak için konumun bir parametre seçeneği sağlar.

   - Yapı parametreleri

     Bu bölümde tanımlanan parametreler, tanımlandığı yapı için geçerlidir. Bu parametreler, planın atanması sırasında tanımlandıkları için [dinamik parametrelerdir.](../../concepts/parameters.md#dynamic-parameters) Tam liste veya yapı parametreleri ve açıklamaları için [Yapı parametreleri tablosuna](#artifact-parameters-table)bakın.

1. Tüm parametreler girildikten sonra, sayfanın alt kısmında **Ata'yı** seçin. Plan ataması oluşturulur ve artefakt dağıtımı başlar. Dağıtım yaklaşık bir saat sürer. Dağıtım durumunu denetlemek için plan atamasını açın.

> [!WARNING]
> Azure Planları hizmeti ve yerleşik plan örnekleri **ücretsizdir.** Azure kaynakları [ürüne göre fiyatlandırılır.](https://azure.microsoft.com/pricing/) Bu plan örneği tarafından dağıtılan kaynakların çalışma maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapı parametreleri tablosu

Aşağıdaki tablo, plan yapı parametrelerinin bir listesini sağlar:

|Artefakt adı|Artefakt türü|Parametre adı|Açıklama|
|-|-|-|-|
|Log Analytics kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-workload-log-rg` kılmak için Kuruluş adını birleştirir.|
|Log Analytics kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Günlük Analizi şablonu|Resource Manager şablonu|Hizmet katmanı|Log Analytics çalışma alanının katmanını ayarlar. Varsayılan değer _PerNode'dur._|
|Günlük Analizi şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Günlük Analizi şablonu|Resource Manager şablonu|Konum|Günlük Analizi çalışma alanını oluşturmak için kullanılan bölge. Varsayılan değer _Batı ABD 2'dir._|
|Ağ kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-workload-net-rg` kılmak için Kuruluş adını birleştirir.|
|Ağ kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Ağ Güvenlik Grubu şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Azure güvenlik duvarı özel IP|[Azure güvenlik duvarının](../../../../firewall/overview.md)özel IP'sini yapılandırır. _ISO 27001'de_ tanımlanan CIDR gösteriminin bir parçası olmalıdır: Paylaşılan Hizmetler yapı parametresi **Azure Güvenlik Duvarı alt ağ adresi öneki.** Varsayılan değer _10.0.4.4'dür._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Paylaşılan hizmetler Abonelik Kimliği|VNET'in İş Yükü ve Paylaşılan Hizmetler arasında eşlemesini etkinleştirmek için kullanılan değer.|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Sanal Ağ adresi öneki|Sanal ağ için CIDR gösterimi. Varsayılan değer _10.1.0.0/16'dır._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Varsayılan alt net adresi öneki|Sanal ağ varsayılan alt ağı için CIDR gösterimi. Varsayılan değer _10.1.0.0/16'dır._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|IP adresi EKLER|İlk ADDS VM IP adresi. Bu değer özel VNET DNS olarak kullanılır.|
|Key Vault kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-workload-kv-rg` kılmak için Kuruluş adını birleştirir.|
|Key Vault kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Anahtar Vault şablonu|Resource Manager şablonu|AAD nesne kimliği|Anahtar Kasa örneğine erişim gerektiren hesabın AAD nesne tanımlayıcısı. Varsayılan değer yok ve boş bırakılamaz. Azure portalından bu değeri bulmak için _Hizmetler_altında "Kullanıcılar"ı arayın ve seçin. Hesap adına filtre uygulayıp bu hesabı seçmek için _Ad_ kutusunu kullanın. Kullanıcı _profil_ sayfasında, _Nesne Kimliği'nin_yanındaki "Kopyalamak için tıkla" simgesini seçin.|
|Anahtar Vault şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Anahtar Vault şablonu|Resource Manager şablonu|Anahtar Kasa SKU|Oluşturulan Anahtar Kasasının SKU'su belirtilir. Varsayılan değer _Premium_Premium'dur.|
|Anahtar Vault şablonu|Resource Manager şablonu|Azure SQL Server admin kullanıcı adı|Azure SQL Server'a erişmek için kullanılan kullanıcı adı. Azure SQL Veritabanı şablonundaki aynı özellik **değeriyle**eşleşmelidir. Varsayılan değer _sql-admin-user'dır._|
|Azure SQL Veritabanı kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-workload-azsql-rg` kılmak için Kuruluş adını birleştirir.|
|Azure SQL Veritabanı kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server admin kullanıcı adı|Azure SQL Server için kullanıcı adı. Key Vault şablonundaki aynı özellik **değeriyle**eşleşmelidir. Varsayılan değer _sql-admin-user'dır._|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server yönetici şifresi (Key Vault Resource ID)|Anahtar Kasası'nın Kaynak Kimliği. "/abonelik/{subscriptionId}/resourceGroups/{orgName}-workload-kv/providers/Microsoft.KeyVault/vaults/{orgName}-workload-kv" kullanın `{subscriptionId}` ve Abonelik Kimliğiniz ve `{orgName}` **Kuruluş adı** plan parametresi ile değiştirin.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Azure SQL Server admin şifresi (Key Vault Secret Name)|SQL Server yöneticisinin kullanıcı adı. Key Vault **şablon özelliği** **Azure SQL Server yönetici kullanıcı adında**değer eşleştirmek gerekir.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|AAD admin nesne kimliği|Active Directory yöneticisi olarak atanacak kullanıcının AAD nesne kimliği. Varsayılan değer yok ve boş bırakılamaz. Azure portalından bu değeri bulmak için _Hizmetler_altında "Kullanıcılar"ı arayın ve seçin. Hesap adına filtre uygulayıp bu hesabı seçmek için _Ad_ kutusunu kullanın. Kullanıcı _profil_ sayfasında, _Nesne Kimliği'nin_yanındaki "Kopyalamak için tıkla" simgesini seçin.|
|Azure SQL Veritabanı şablonu|Resource Manager şablonu|AAD admin giriş|Şu anda, Microsoft hesapları (live.com veya outlook.com gibi) yönetici olarak ayarlanamaz. Yalnızca kuruluşunuzdaki kullanıcılar ve güvenlik grupları yönetici olarak ayarlanabilir. Varsayılan değer yok ve boş bırakılamaz. Azure portalından bu değeri bulmak için _Hizmetler_altında "Kullanıcılar"ı arayın ve seçin. Hesap adına filtre uygulayıp bu hesabı seçmek için _Ad_ kutusunu kullanın. Kullanıcı _profil_ sayfasında, _Kullanıcı adını_kopyalayın.|
|Uygulama Hizmeti Ortamı kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-workload-ase-rg` kılmak için Kuruluş adını birleştirir.|
|Uygulama Hizmeti Ortamı kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Uygulama Hizmet Ortamı şablonu|Resource Manager şablonu|Etki alanı adı|Örnek tarafından oluşturulan Active Directory adı. Varsayılan değer _contoso.com._|
|Uygulama Hizmet Ortamı şablonu|Resource Manager şablonu|ASE konumu|Uygulama Hizmet Ortamı konumu. Varsayılan değer _Batı ABD 2'dir._|
|Uygulama Hizmet Ortamı şablonu|Resource Manager şablonu|Uygulama Ağ Geçidi gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 App Service Environment/SQL Database iş yükü planı örneğini dağıtma adımlarını gözden geçirdiğiniz için, mimari ve denetim eşleme hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 App Service Environment/SQL Database iş yükü planı - Genel Bakış](./index.md)
> [ISO 27001 App Service Environment/SQL Veritabanı iş yükü planı - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.