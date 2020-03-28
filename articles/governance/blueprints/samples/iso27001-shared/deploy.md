---
title: ISO 27001 Paylaşılan Hizmetler plan örneğini dağıtın
description: PLAN yapı parametresi ayrıntılarını içeren ISO 27001 Paylaşılan Hizmetler plan örneği için adımları dağıtın.
ms.date: 01/13/2020
ms.topic: sample
ms.openlocfilehash: a9dabc99469321445006e449757a10fbc51aba87
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75920689"
---
# <a name="deploy-the-iso-27001-shared-services-blueprint-sample"></a>ISO 27001 Paylaşılan Hizmetler plan örneğini dağıtın

Azure Planları ISO 27001 Paylaşılan Hizmetler planı örneğini dağıtmak için aşağıdaki adımların atılması gerekir:

> [!div class="checklist"]
> - Örnekten yeni bir plan oluşturma
> - Örnek kopyanızı **Yayımlanmış** olarak işaretleme
> - Planın kopyasını varolan bir aboneye atama

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free) bir hesap oluşturun.

## <a name="create-blueprint-from-sample"></a>Örnekten plan oluşturma

İlk olarak, bir başlangıç olarak örnek kullanarak ortamınızda yeni bir plan oluşturarak plan örneği uygulayın.

1. Sol bölmede **Tüm hizmetler**'i seçin. **Planları**arayın ve seçin.

1. Soldaki **Başlat'tan plan** _Oluştur'un_altındaki **Oluştur** düğmesini seçin.

1. **ISO 27001: Diğer** _Örnekler_ altında Paylaşılan Hizmetler plan örneğini bulun ve bu **örneği kullanın'ı**seçin.

1. Plan örneğinin _Temellerini_ girin:

   - **Plan adı**: ISO 27001 Paylaşılan Hizmetler plan örneğinin kopyası için bir ad verin.
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
     - **Paylaşılan hizmetler alt net adresi öneki**: Dağıtılan kaynakları birbirine bağlatmak için CIDR gösterim değerini sağlayın.
     - **Paylaşılan hizmet konumu**: Yapıtların hangi konuma dağıtılan yeri belirler. Tüm hizmetler tüm konumlarda kullanılamaz. Bu tür hizmetleri dağıtan yapılar, söz konusu yapıyı dağıtmak için konumun bir parametre seçeneği sağlar.
     - **İzin verilen konum (İlke: ISO 27001 için plan girişimi)**: Kaynak grupları ve kaynakları için izin verilen konumları gösteren değer.
     - **VM aracıları için Günlük Analitik çalışma alanı (İlke: ISO 27001 için plan girişimi)**: Bir çalışma alanının Kaynak Kimliğini belirtir. Bu parametre `concat` Kaynak Kimliği oluşturmak için bir işlev kullanır.

   - Yapı parametreleri

     Bu bölümde tanımlanan parametreler, tanımlandığı yapı için geçerlidir. Bu parametreler, planın atanması sırasında tanımlandıkları için [dinamik parametrelerdir.](../../concepts/parameters.md#dynamic-parameters) Tam liste veya yapı parametreleri ve açıklamaları için [Yapı parametreleri tablosuna](#artifact-parameters-table)bakın.

1. Tüm parametreler girildikten sonra, sayfanın alt kısmında **Ata'yı** seçin. Plan ataması oluşturulur ve artefakt dağıtımı başlar. Dağıtım yaklaşık bir saat sürer. Dağıtım durumunu denetlemek için plan atamasını açın.

> [!WARNING]
> Azure Planları hizmeti ve yerleşik plan örnekleri **ücretsizdir.** Azure kaynakları [ürüne göre fiyatlandırılır.](https://azure.microsoft.com/pricing/) Bu plan örneği tarafından dağıtılan kaynakların çalışma maliyetini tahmin etmek için [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="artifact-parameters-table"></a>Yapı parametreleri tablosu

Aşağıdaki tablo, plan yapı parametrelerinin bir listesini sağlar:

|Artefakt adı|Artefakt türü|Parametre adı|Açıklama|
|-|-|-|-|
|\[Önizleme\]: Linux VM Ölçek Setleri (VMSS) için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|(İsteğe bağlı) Varsayılan değer _["yok"]_ olur.|
|\[Önizleme\]: Linux VM'leri için Log Analytics Agent'ı dağıtın|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Linux işletim sistemi desteklenen VM görüntülerinin listesi|(İsteğe bağlı) Varsayılan değer _["yok"]_ olur.|
|\[Önizleme\]: Windows VM Ölçek Setleri (VMSS) için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|(İsteğe bağlı) Varsayılan değer _["yok"]_ olur.|
|\[Önizleme\]: Windows VM'ler için Günlük Analizi Aracısı Dağıt|İlke ataması|İsteğe bağlı: Kapsamına eklemek için Windows işletim sistemi desteklenen VM görüntülerinin listesi|(İsteğe bağlı) Varsayılan değer _["yok"]_ olur.|
|İzin verilen kaynak türleri|İlke ataması|İzin verilen kaynak türleri|Dağıtılmasına izin verilen kaynak türlerinin listesi. Bu liste Paylaşılan Hizmetler'de dağıtılan tüm kaynak türlerinden oluşur.|
|İzin verilen depolama hesabı SKU'ları|İlke ataması|İzin verilen depolama SUS'ları|Tanıgünlükdepolama hesabı SK'lerin listesine izin verilir. Varsayılan değer _["Standard_LRS"]_ olur.|
|İzin verilen sanal makine SUS'lar|İlke ataması|Konuşlandırılmasına izin verilen sanal makine SUS'ların listesi. Varsayılan değer _["Standard_DS1_v2", "Standard_DS2_v2"]_ değeridir.|
|ISO 27001 için plan girişimi|İlke ataması|Tanılama günlüklerini denetlemek için kaynak türleri|Tanılama günlüğü ayarı etkin değilse denetlenenekaynak türlerinin listesi. Kabul edilebilir değerler [Azure Monitor tanı günlükleri şemalarında](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type)bulunabilir.|
|Log Analytics kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-sharedsvsc-log-rg` kılmak için Kuruluş adını birleştirir.|
|Log Analytics kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Günlük Analizi şablonu|Resource Manager şablonu|Hizmet katmanı|Log Analytics çalışma alanının katmanını ayarlar. Varsayılan değer _PerNode'dur._|
|Günlük Analizi şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Günlük Analizi şablonu|Resource Manager şablonu|Konum|Günlük Analizi çalışma alanını oluşturmak için kullanılan bölge. Varsayılan değer _Batı ABD 2'dir._|
|Ağ kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-sharedsvcs-net-rg` kılmak için Kuruluş adını birleştirir.|
|Ağ kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Azure Güvenlik Duvarı şablonu|Resource Manager şablonu|Azure güvenlik duvarı özel IP|[Azure güvenlik duvarının](../../../../firewall/overview.md)özel IP'sini yapılandırır. Bu değer, paylaşılan hizmetler alt netinde varsayılan rota tablosu olarak da kullanılır. **Azure Güvenlik Duvarı alt net adresi önekinde**tanımlanan CIDR gösteriminin bir parçası olmalıdır. Varsayılan değer _10.0.4.4'dür._|
|Azure Güvenlik Duvarı şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Ağ Güvenlik Grubu şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Sanal Ağ adresi öneki|Sanal ağ için CIDR gösterimi. Varsayılan değer _10.0.0.0/16'dır._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Sanal Ağ DDoS korumasını etkinleştirin|Sanal ağ için DDoS korumasını yapılandırır. Varsayılan değer _doğrudur._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Paylaşılan Hizmetler alt net adresi öneki|Paylaşılan Hizmetler alt ağı için CIDR gösterimi. Varsayılan değer _10.0.0.0/24'dür._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|DMZ subnet adresi öneki|DMZ alt ağı için CIDR gösterimi. Varsayılan değer _10.0.1.0/24'dür._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Uygulama Ağ Geçidi alt net adresi öneki|Uygulama ağ geçidi alt ağı için CIDR gösterimi. Varsayılan değer _10.0.2.0/24'dür._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Sanal Ağ Ağ Geçidi alt ağ adresi öneki|Sanal ağ ağ geçidi alt ağı için CIDR gösterimi. Varsayılan değer _10.0.3.0/24'dür._|
|Sanal Ağ ve Rota Tablosu şablonu|Resource Manager şablonu|Azure Güvenlik Duvarı alt ağ adresi öneki|[Azure güvenlik duvarı](../../../../firewall/overview.md) alt ağı için CIDR gösterimi. **Azure güvenlik duvarı özel IP** parametresini içermelidir.|
|Key Vault kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-sharedsvcs-kv-rg` kılmak için Kuruluş adını birleştirir.|
|Key Vault kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Anahtar Vault şablonu|Resource Manager şablonu|Jumpbox admin kullanıcı adı|Atlama kutusu için kullanıcı adı. Jumpbox şablonundaki aynı özellik **değeriyle eşleşmesi**gerekir. Varsayılan değer _jb-admin-kullanıcıdır._|
|Anahtar Vault şablonu|Resource Manager şablonu|Jumpbox admin ssh anahtar veya şifre|Atlama kutusundaki hesabın anahtarı veya parolası. Jumpbox şablonundaki aynı özellik **değeriyle eşleşmesi**gerekir. Varsayılan değer yok ve boş bırakılamaz.|
|Anahtar Vault şablonu|Resource Manager şablonu|Etki alanı yöneticisi kullanıcı adı|Active Directory VM'ye erişmek ve diğer VM'lere bir etki alanına katılmak için kullanılan kullanıcı adı. Etkin Dizin Etki Alanı Hizmetleri şablonundaki **Etki Alanı yöneticisi kullanıcı** özelliği **değeriyle eşleşmelidir.** Varsayılan değer _etki alanı yöneticisi-kullanıcısıdır._|
|Anahtar Vault şablonu|Resource Manager şablonu|Etki alanı yöneticisi şifresi|Etki alanı yöneticisi kullanıcının şifresi. Varsayılan değer yok ve boş bırakılamaz.|
|Anahtar Vault şablonu|Resource Manager şablonu|AAD nesne kimliği|Anahtar Kasa örneğine erişim gerektiren hesabın AAD nesne tanımlayıcısı. Varsayılan değer yok ve boş bırakılamaz. Azure portalından bu değeri bulmak için _Hizmetler_altında "Kullanıcılar"ı arayın ve seçin. Hesap adına filtre uygulayıp bu hesabı seçmek için _Ad_ kutusunu kullanın. Kullanıcı _profil_ sayfasında, _Nesne Kimliği'nin_yanındaki "Kopyalamak için tıkla" simgesini seçin.  |
|Anahtar Vault şablonu|Resource Manager şablonu|Gün içinde günlük tutma|Gün içinde veri saklama. Varsayılan değer _365'tir._|
|Anahtar Vault şablonu|Resource Manager şablonu|Anahtar Kasa SKU|Oluşturulan Anahtar Kasasının SKU'su belirtilir. Varsayılan değer _Premium_Premium'dur.|
|Jumpbox kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-sharedsvcs-jb-rg` kılmak için Kuruluş adını birleştirir.|
|Jumpbox kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Jumpbox şablonu|Resource Manager şablonu|Jumpbox admin kullanıcı adı|Jumpbox VMs'e erişmek için kullanılan kullanıcı adı. Key Vault şablonundaki aynı özellik **değeriyle**eşleşmelidir. Varsayılan değer _jb-admin-kullanıcıdır._|
|Jumpbox şablonu|Resource Manager şablonu|Jumpbox admin şifresi (Key Vault Kaynak Kimliği)|Anahtar Kasası'nın Kaynak Kimliği. "/abonelikler/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv" kullanın `{subscriptionId}` ve Abonelik Kimliğinizi ve `{orgName}` **Kuruluş adı** plan parametresi ile değiştirin.|
|Jumpbox şablonu|Resource Manager şablonu|Jumpbox admin şifre (Key Vault Gizli Adı)|Jumpbox admin kullanıcı adı. Key Vault **şablon özelliği** **Jumpbox yönetici kullanıcı adında**değeri eşleştirmek gerekir.|
|Jumpbox şablonu|Resource Manager şablonu|Jumpbox İşletim Sistemi|Atlama kutusu VM işletim sistemini belirler. Varsayılan değer _Windows_Windows'dur.|
|Etkin Dizin Etki Alanı Hizmetleri kaynak grubu|Kaynak grubu|Adı|**Kilitli** - Kaynak grubunu **Organization name** benzersiz `-sharedsvcs-adds-rg` kılmak için Kuruluş adını birleştirir.|
|Etkin Dizin Etki Alanı Hizmetleri kaynak grubu|Kaynak grubu|Konum|**Kilitli** - Plan parametresini kullanır.|
|Active Directory Etki Alanı Hizmetleri şablonu|Resource Manager şablonu|Etki alanı yöneticisi kullanıcı adı|ADDS jumpbox kullanıcı adı. Key Vault şablonundaki aynı özellik **değeriyle**eşleşmelidir. Varsayılan değer _ekler-admin-kullanıcı._|
|Active Directory Etki Alanı Hizmetleri şablonu|Resource Manager şablonu|Etki alanı yöneticisi şifresi (Key Vault Resource ID)|Anahtar Kasası'nın Kaynak Kimliği. "/abonelikler/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv" kullanın `{subscriptionId}` ve Abonelik Kimliğinizi ve `{orgName}` **Kuruluş adı** plan parametresi ile değiştirin.|
|Active Directory Etki Alanı Hizmetleri şablonu|Resource Manager şablonu|Domain admin şifresi (Key Vault Gizli Adı)|Etki alanı yöneticisinin kullanıcı adı. Key Vault **şablon özelliği** **Etki Alanı yönetici kullanıcı adında**değeri eşleştirmek gerekir.|
|Active Directory Etki Alanı Hizmetleri şablonu|Resource Manager şablonu|Etki alanı adı|Örnek tarafından oluşturulan Active Directory adı. Varsayılan değer _contoso.com._|
|Active Directory Etki Alanı Hizmetleri şablonu|Resource Manager şablonu|Etki alanı yöneticisi kullanıcı|Yönetici AD hesabı ve aygıtları AD etki alanına katılmak için kullanıcı adı. Anahtar Vault şablonundaki **AD yöneticisi kullanıcı adı** özellik **değeriyle**eşleşmelidir. Varsayılan değer _etki alanı yöneticisi-kullanıcısıdır._|
|Active Directory Etki Alanı Hizmetleri şablonu|Resource Manager şablonu|Etki alanı yöneticisi şifresi|Parolayı depolamak için Anahtar Kasa sı ayrıntılarını ayarlayın. Varsayılan değer yok ve boş bırakılamaz.|

## <a name="next-steps"></a>Sonraki adımlar

ISO 27001 Paylaşılan Hizmetler plan örneğini dağıtma adımlarını gözden geçirdiğiniz için, mimari ve denetim eşleme hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="nextstepaction"]
> [ISO 27001 Paylaşılan Hizmetler planı - Genel Bakış](./index.md)
> [ISO 27001 Paylaşılan Hizmetler planı - Kontrol haritalama](./control-mapping.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
