---
title: Ağları Azure Otomasyonu 'na güvenli bir şekilde bağlamak için Azure özel bağlantısı 'nı kullanın
description: Ağları Azure Otomasyonu 'na güvenli bir şekilde bağlamak için Azure özel bağlantısı 'nı kullanın
author: mgoedtel
ms.author: magoedte
ms.topic: conceptual
ms.date: 06/22/2020
ms.subservice: ''
ms.openlocfilehash: fa473591355ef9e1ee582dd9c9b820dfa2f93f36
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85269103"
---
# <a name="use-azure-private-link-to-securely-connect-networks-to-azure-automation"></a>Ağları Azure Otomasyonu 'na güvenli bir şekilde bağlamak için Azure özel bağlantısı 'nı kullanın

Azure Özel Uç Noktası sizi Azure Özel Bağlantı ile desteklenen bir hizmete özel olarak ve güvenle bağlayan bir ağ arabirimidir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak Otomasyon hizmetini sanal ağınıza etkin bir şekilde getiriyor. VNet ve Otomasyon hesabı arasındaki ağ trafiği, VNet üzerinden ve Microsoft omurga ağındaki özel bir bağlantı üzerinden, genel İnternet 'ten etkilenme olasılığını ortadan kaldırır.

Örneğin, giden internet erişimini devre dışı bıraktığınız bir sanal ağınız var. Ancak, Otomasyon hesabınıza özel olarak erişmek ve karma runbook çalışanları üzerinde Web kancaları, durum yapılandırması ve Runbook işleri gibi Otomasyon özelliklerini kullanmak istersiniz. Üstelik, kullanıcıların yalnızca VNET aracılığıyla Otomasyon hesabına erişimi olmasını istiyorsunuz. Bu, Özel uç noktalar dağıtarak elde edilebilir.

Bu makalede, ' nin ne zaman kullanılacağı ve otomasyon hesabınızla özel bir uç noktanın nasıl ayarlanacağı ele alınmaktadır.

![Azure Otomasyonu için özel bağlantıya kavramsal genel bakış](./media/private-link-security/private-endpoints-automation.png)

## <a name="advantages"></a>Yararları

Özel bağlantıyla birlikte şunları yapabilirsiniz:

- Herkese açık bir ağ erişimi açmadan özel olarak Azure Otomasyonu 'na bağlanın.
- Herhangi bir genel ağ erişimi açmadan Azure Izleyici Log Analytics çalışma alanına özel olarak bağlanın.

    >[!NOTE]
    >Otomasyon hesabınız iş verilerini iletmek için bir Log Analytics çalışma alanına bağlanmışsa ve Güncelleştirme Yönetimi, Değişiklik İzleme ve envanter, durum yapılandırması veya VM'leri çalışma saatleri dışında başlat/durdur gibi özellikleri etkinleştirdiğinizde bu gereklidir. Azure Izleyici için özel bağlantı hakkında daha fazla bilgi için bkz. Azure [özel bağlantı kullanarak ağları güvenli bir şekilde Azure izleyici 'ye bağlama](../../azure-monitor/platform/private-link-security.md).

- Otomasyon verilerinize yalnızca yetkili özel ağlar aracılığıyla erişildiğinden emin olun.
- Özel uç noktanızla bağlanan Azure Otomasyonu kaynağınızı tanımlayarak özel ağlarınızdan veri alımını önleyin.
- Özel şirket içi ağınızı ExpressRoute ve özel bağlantı kullanarak Azure Otomasyonu 'na güvenle bağlayın.
- Tüm trafiği Microsoft Azure omurga ağı içinde saklayın.

Daha fazla bilgi için bkz. [özel bağlantının önemli avantajları](../../private-link/private-link-overview.md#key-benefits).

## <a name="how-it-works"></a>Nasıl çalışır?

Azure Otomasyonu özel bağlantısı, bir veya daha fazla özel uç noktayı (ve bu nedenle içerdikleri sanal ağları) Otomasyon hesabı kaynağına bağlar. Bu uç noktalar, bir runbook 'u başlatmak için Web kancaları kullanan makinelerdir, karma Runbook Worker rolünü barındıran makineler ve DSC düğümleri.

Otomasyon için özel uç noktalar oluşturduktan sonra, siz veya bir makine doğrudan iletişim kurabildiğiniz, genel kullanıma açık Otomasyon URL 'lerinin her biri, VNet 'iniz içindeki bir özel uç noktaya eşlenir.

### <a name="webhook-scenario"></a>Web kancası senaryosu

Web kancası URL 'sinde bir GÖNDERI yaparak runbook 'ları başlatabilirsiniz. Örneğin, URL şöyle görünür:`https://<automationAccountId>.webhooks. <region>.azure-automation.net/webhooks?token=gzGMz4SMpqNo8gidqPxAJ3E%3d`

### <a name="state-configuration-agentsvc-scenario"></a>Durum Yapılandırması (Agentsvc) senaryosu

Durum Yapılandırması size herhangi bir bulutta veya şirket içi veri merkezinde bulunan düğümler için PowerShell Istenen durum yapılandırması (DSC) yapılandırmalarını yazmanıza, yönetmenize ve derlemenize olanak tanıyan Azure yapılandırma yönetimi hizmeti sağlar.

Makinedeki aracı DSC hizmetine kaydolur ve sonra DSC yapılandırması çekmek için hizmet uç noktasını kullanır. Aracı hizmeti uç noktası şuna benzer: `https://<automationAccountId>.agentsvc.<region>.azure-automation.net` .

Ortak & özel uç nokta URL 'SI aynı olur, ancak özel bağlantı etkinleştirildiğinde özel bir IP adresine eşlenir.

## <a name="planning-based-on-your-network"></a>Ağınızı temel alan planlama

Otomasyon hesabı kaynağını ayarlamadan önce ağ yalıtımı gereksinimlerinizi göz önünde bulundurun. Sanal ağlarınızın genel İnternet 'e erişimini ve otomasyon hesabınıza yönelik erişim kısıtlamalarını (Otomasyon hesabınızla tümleşikse Azure Izleyici günlüklerine özel bir bağlantı grubu kapsamı ayarlama dahil) değerlendirin.

### <a name="connect-to-a-private-endpoint"></a>Özel bir uç noktaya Bağlan

Ağımızı bağlamak için özel bir uç nokta oluşturun. Bu görevi [Azure Portal özel bağlantı merkezinde](https://portal.azure.com/#blade/Microsoft_Azure_Network/PrivateLinkCenterBlade/privateendpoints)yapabilirsiniz. PublicNetworkAccess ve Private bağlantısına yaptığınız değişiklikler uygulandıktan sonra, bu değişikliklerin etkili olması 35 dakika sürebilir.

Bu bölümde, Otomasyon hesabınız için özel bir uç nokta oluşturacaksınız.

1. Ekranın sol üst kısmında, **ağ > özel bağlantı merkezi (Önizleme) > kaynak oluştur**' u seçin.

2. **Özel bağlantı merkezi 'Ne genel bakış**' da, **bir hizmete özel bağlantı oluşturma**seçeneğinde, **Başlat**' ı seçin.

3. **Sanal makine oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Aboneliğinizi seçin. |
    | Kaynak grubu | **Myresourcegroup**öğesini seçin. Bu, önceki bölümde oluşturdunuz.  |
    | **ÖRNEK AYRıNTıLARı** |  |
    | Name | *Privateendpoint*girin. |
    | Bölge | **Yourregion**' ı seçin. |
    |||

4. **Sonraki: kaynak**' ı seçin.

5. **Özel uç nokta oluştur-kaynak**bölümünde bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |Bağlantı yöntemi  | Dizinimde bir Azure kaynağına bağlan ' ı seçin.|
    | Abonelik| Aboneliğinizi seçin. |
    | Kaynak türü | **Microsoft. Automation/automationAccounts**' ı seçin. |
    | Kaynak |*Myautomationaccount* seçin|
    |Hedef alt kaynak |Senaryonuza bağlı olarak *Web kancası* veya *Dscandhyıbridworker* ' ı seçin.|
    |||

6. Ileri 'yi seçin **: yapılandırma**.

7. **Özel uç nokta oluşturma-yapılandırma**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    |**IŞLEMLERI**| |
    | Sanal ağ| *MyVirtualNetwork*öğesini seçin. |
    | Alt ağ | *Mysubnet*öğesini seçin. |
    |**ÖZEL DNS TÜMLEŞTIRMESI**||
    |Özel DNS bölgesiyle tümleştirin |**Evet**' i seçin. |
    |Özel DNS bölgesi |Seç *(yeni) Privatelink. Azure-Automation.net* |
    |||

8. **İncele ve oluştur**’u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur** sayfasına götürülürsünüz.

9. **Doğrulama başarılı** Iletisini gördüğünüzde **Oluştur**' u seçin.

Özel **bağlantı merkezi 'nde (Önizleme)** özel bağlantı kaynağınızı görüntülemek için özel **uç noktalar** ' ı seçin.

![Otomasyon kaynağı özel bağlantısı](./media/private-link-security/private-link-automation-resource.png)

Tüm ayrıntıları görmek için kaynağı seçin. Bu, Otomasyon hesabınız için yeni bir özel uç nokta oluşturur ve sanal ağınızdan özel bir IP atar. **Bağlantı durumu** **Onaylandı**olarak gösterilir.

Benzer şekilde, durum yapılandırması (Agentsvc) ve karma Runbook Worker iş çalışma zamanı (jrds) için benzersiz bir tam etki alanı adı (FQDN) oluşturulur. Her birine sanal ağınızdan ayrı bir IP atanır ve **bağlantı durumu** **Onaylandı**olarak gösterilir.

Hizmet tüketicisinin Otomasyon kaynağında RBAC izinleri varsa, otomatik onay yöntemini seçebilirler. Bu durumda, istek Otomasyon sağlayıcı kaynağına ulaştığında, hizmet sağlayıcısından herhangi bir işlem yapmanız gerekmez ve bağlantı otomatik olarak onaylanır.

## <a name="set-public-network-access-flags"></a>Genel ağ erişim bayraklarını ayarla

Bir Otomasyon hesabını tüm genel konfigürasyonları reddedecek şekilde yapılandırabilir ve yalnızca özel uç noktalar aracılığıyla ağ güvenliğini artırmak için bağlantılara izin verebilirsiniz. Otomasyon hesabına erişimi yalnızca VNet içinden kısıtlamak ve genel İnternet 'ten erişime izin vermek istiyorsanız `publicNetworkAccess` özelliği olarak ayarlayabilirsiniz `$true` .

**Ortak ağ erişimini reddet** ayarı olarak ayarlandığında `true` , yalnızca özel uç noktalar aracılığıyla bağlantılara izin verilir ve genel uç noktalar aracılığıyla tüm bağlantılar bir hata iletisiyle reddedilir.

Aşağıdaki PowerShell betiği, `Get` `Set` Otomasyon hesabı düzeyinde nasıl ve **genel ağ erişim** özelliğinin nasıl yapılacağını gösterir:

```powershell
$account = Get-AzResource -ResourceType Microsoft.Automation/automationAccounts -ResourceGroupName "<resourceGroupName>" -Name "<automationAccountName>" -ApiVersion "2020-01-13-preview"
$account.Properties | Add-Member -Name 'publicNetworkAccess' -Type NoteProperty -Value $true
$account | Set-AzResource -Force -ApiVersion "2020-01-13-preview"
```

## <a name="dns-configuration"></a>DNS yapılandırması

Bağlantı dizesinin parçası olarak bir FQDN kullanarak özel bir bağlantı kaynağına bağlanırken, DNS ayarlarınızı ayrılmış özel IP adresine çözümlemek üzere doğru şekilde yapılandırmak önemlidir. Mevcut Azure hizmetlerinde ortak bir uç nokta üzerinden bağlanılırken kullanılacak bir DNS yapılandırması zaten olabilir. Özel uç noktanız kullanılarak bağlanmak için bunun geçersiz kılınması gerekir.

Özel uç nokta ile ilişkili ağ arabirimi, DNS 'nizi yapılandırmak için gerekli olan ve belirli bir özel bağlantı kaynağı için ayrılan FQDN ve özel IP adresleri dahil olmak üzere tam bilgi kümesini içerir.

Özel uç noktalar için DNS ayarlarınızı yapılandırmak üzere aşağıdaki seçenekleri kullanabilirsiniz:

* Ana bilgisayar dosyasını kullanın (yalnızca test için önerilir). Önce ad çözümlemesi için DNS kullanarak geçersiz kılmak üzere bir sanal makinede ana bilgisayar dosyasını kullanabilirsiniz.

* Özel bir [DNS bölgesi](../../dns/private-dns-privatednszone.md)kullanın. Belirli bir özel uç nokta için DNS çözümlemesini geçersiz kılmak üzere özel DNS bölgelerini kullanabilirsiniz. Özel bir DNS bölgesi, belirli etki alanlarını çözümlemek için sanal ağınıza bağlanabilir. Sanal makinenizde aracının özel uç nokta üzerinden iletişim kurmasını sağlamak için, olarak Özel DNS bir kayıt oluşturun `privatelink.azure-automation.net` . Özel uç noktanın IP 'si için yeni *BIR DNS a* kaydı eşlemesi ekleyin.

* DNS ileticinizi kullanın (isteğe bağlı). Belirli bir özel bağlantı kaynağı için DNS çözümlemesini geçersiz kılmak üzere DNS ileticinizi kullanabilirsiniz. [DNS sunucunuz](../../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) bir sanal ağ üzerinde barındırılıyorsa, tüm özel bağlantı kaynakları için yapılandırmayı basitleştirmek üzere özel bir DNS bölgesi kullanmak üzere bir DNS iletme kuralı oluşturabilirsiniz.

Daha fazla bilgi için bkz. [Azure özel uç nokta DNS yapılandırması](../../private-link/private-endpoint-dns.md).

## <a name="next-steps"></a>Sonraki adımlar

Özel uç nokta hakkında daha fazla bilgi edinmek için bkz. [Azure özel uç noktası nedir?](../../private-link/private-endpoint-overview.md).