---
title: Azure Uygulama Öngörüleri'nde kaynaklar, roller ve erişim denetimi | Microsoft Dokümanlar
description: Kuruluşunuzun istatistiklerinin sahipleri, katılımcıları ve okuyucuları.
ms.topic: conceptual
ms.date: 02/14/2019
ms.openlocfilehash: 1e57af269c4052d0dcd4a8f7970ca23017024299
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79473141"
---
# <a name="resources-roles-and-access-control-in-application-insights"></a>Uygulama Öngörüleri'nde kaynaklar, roller ve erişim denetimi

[Microsoft Azure'da Rol tabanlı erişim denetimini](../../role-based-access-control/role-assignments-portal.md)kullanarak Azure Uygulama [Öngörüleri'nde][start]verilerinize erişimi kimlerin okuduğunu ve güncellediğini kontrol edebilirsiniz.

> [!IMPORTANT]
> Kaynak **grubundaki veya** uygulama kaynağınızın ait olduğu abonelikteki kullanıcılara erişim atamayın - kaynağın kendisine değil. **Application Insights bileşeni katılımcı** sıfatını atayın. Bu, uygulama kaynağınızla birlikte web testlerine ve uyarılarına erişimin tek düze denetimini sağlar. [Daha fazla bilgi edinin](#access).


[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="resources-groups-and-subscriptions"></a>Kaynaklar, gruplar ve abonelikler

İlk olarak, bazı tanımlar:

* **Kaynak** - Microsoft Azure hizmetiörneği. Uygulama Öngörüleri kaynağınız, uygulamanızdan gönderilen telemetri verilerini toplar, analiz eder ve görüntüler.  Azure kaynaklarının diğer türleri arasında web uygulamaları, veritabanları ve VM'ler yer almaktadır.
  
    Kaynaklarınızı görmek için [Azure portalını][portal]açın, oturum açın ve Tüm Kaynaklar'ı tıklatın. Kaynak bulmak için filtre alanına adının bir bölümünü yazın.
  
    ![Azure kaynakları listesi](./media/resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* [**Kaynak grubu**][group] - Her kaynak bir gruba aittir. Grup, özellikle erişim denetimi için ilgili kaynakları yönetmenin kullanışlı bir yoludur. Örneğin, bir kaynak grubuna bir Web Uygulaması, uygulamayı izlemek için bir Uygulama Öngörüleri kaynağı ve dışa aktarılan verileri tutmak için bir Depolama kaynağı koyabilirsiniz.

* [**Abonelik**](https://portal.azure.com) - Uygulama Öngörüleri'ni veya diğer Azure kaynaklarını kullanmak için bir Azure aboneliğinde oturum açabilirsiniz. Her kaynak grubu, fiyat paketinizi seçtiğiniz ve kuruluş aboneliğiyse üyeleri ve erişim izinlerini seçtiğiniz bir Azure aboneliğine aittir.
* [**Microsoft hesabı**][account] - Microsoft Azure aboneliklerinde, XBox Live'da, Outlook.com ve diğer Microsoft hizmetlerinde oturum açmak için kullandığınız kullanıcı adı ve parola.

## <a name="control-access-in-the-resource-group"></a><a name="access"></a>Kaynak grubunda erişimi denetleme

Uygulamanız için oluşturduğunuz kaynağa ek olarak, uyarılar ve web testleri için ayrı gizli kaynaklar da olduğunu anlamak önemlidir. Bunlar, Application Insights kaynağınızla aynı [kaynak grubuna](#resource-group) eklenir. Web siteleri veya depolama gibi diğer Azure hizmetlerini de oraya koymuş olabilirsiniz.

Bu kaynaklara erişimi denetlemek için şunları tavsiye edilir:

* Kaynak grubunda **veya abonelik** düzeyinde erişimi denetle.
* **Uygulama Öngörüleri Bileşeni katılımcı** sıfatını kullanıcılara atayın. Bu, gruptaki diğer hizmetlere erişim sağlamadan web testlerini, uyarıları ve Uygulama Öngörüleri kaynaklarını yeniden izlemelerine olanak tanır.

## <a name="to-provide-access-to-another-user"></a>Başka bir kullanıcıya erişim sağlamak için

Aboneliğin veya kaynak grubunun Sahibi haklarına sahip olmalısınız.

Kullanıcının bir [Microsoft Hesabı][account]veya [kuruluş Microsoft Hesabına](../../active-directory/fundamentals/sign-up-organization.md)erişimi olmalıdır. Azure Active Directory'de tanımlanan kişilere ve kullanıcı gruplarına erişim sağlayabilirsiniz.

#### <a name="navigate-to-resource-group-or-directly-to-the-resource-itself"></a>Kaynak grubuna veya doğrudan kaynağın kendisine gidin

Sol menüden **Access denetimi (IAM)** seçeneğini belirleyin.

![Azure portalında Access denetim düğmesinin ekran görüntüsü](./media/resources-roles-access-control/0001-access-control.png)

**Rol Atama ekle'yi** seçin

![Kırmızı ile vurgulanan ekle düğmesi ile Access denetim menüsünün ekran görüntüsü](./media/resources-roles-access-control/0002-add.png)

Aşağıda **izin ekle** görünümü öncelikle Uygulama Öngörüleri kaynaklarına özgüdür, erişim denetimi izinlerini kaynak grupları gibi daha yüksek bir düzeyden görüntülüyorsanız, Uygulama İçgörü olmayan ek roller görürsünüz.

Tüm Azure rol tabanlı erişim denetimi yerleşik rolleri hakkındaki bilgileri görüntülemek için [resmi başvuru içeriğini](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)kullanın.

![Access denetimi kullanıcı rol listesinin ekran görüntüsü](./media/resources-roles-access-control/0003-user-roles.png)

#### <a name="select-a-role"></a>Rol seçin

Geçerli olduğu durumlarda, ilgili resmi referans belgelerine bağlanırız.

| Rol | Kaynak grubunda |
| --- | --- |
| [Sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#owner) |Kullanıcı erişimi de dahil olmak üzere her şeyi değiştirebilir. |
| [Katkıda Bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) |Tüm kaynaklar da dahil olmak üzere her şeyi edebilirsiniz. |
| [Uygulama Öngörüleri Bileşeni katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-component-contributor) |Application Insights kaynaklarını edinebilir. |
| [Okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#reader) |Görüntüleyebilir ama hiçbir şeyi değiştiremez. |
| [Uygulama Öngörüleri Anlık Görüntü Hata Ayıkcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#application-insights-snapshot-debugger) | Kullanıcıya Application Insights Snapshot Hata Ayıklama özelliklerini kullanma izni verir. Bu rolün ne Sahibi ne de Katılımcı rollerinde yer olmadığını unutmayın. |
| Azure Hizmet Dağıtma Sürüm Yönetimi Katılımcısı | Azure Hizmet Dağıtımı aracılığıyla dağıtılatan hizmetler için katılımcı rolü. |
| [Veri Purger](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#data-purger) | Kişisel verilerin temizlenmesinde özel bir rol. Daha fazla bilgi [için kişisel veriler için kılavuzumuza](https://docs.microsoft.com/azure/application-insights/app-insights-customer-data) bakın.   |
| ExpressRoute Yöneticisi | Silme oluşturabilir ve ekspres yolları yönetebilir.|
| [Log Analytics Katkıda Bulunan](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-contributor) | Log Analytics Katılımcısı tüm izleme verilerini okuyabilir ve izleme ayarlarını düzeltebilir. İzleme ayarlarını düzenleme, VM uzantısını VM'lere eklemeyi içerir; Azure Depolama'dan günlük koleksiyonunu yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılamalarını tüm Azure kaynaklarında yapılandırma.  |
| [Log Analytics Okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#log-analytics-reader) | Log Analytics Reader, Azure tanılama yapılandırmasını tüm Azure kaynaklarında görüntülemek de dahil olmak üzere tüm izleme verilerinin yanı sıra izleme ayarlarını görüntüleyebilir ve görüntüleyebilir. |
| masterreader | Kullanıcının her şeyi görüntülemesine, ancak değişiklik yapmamasına izin verir. |
| [Katılımcıyı İzleme](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) | Tüm izleme verilerini okuyabilir ve izleme ayarlarını güncelleyebilir.|
| [İzleme Ölçümleri Yayıncı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-metrics-publisher) | Azure kaynaklarına karşı ölçüm yayımlamayı sağlar. |
| [İzleme Okuyucu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) | Tüm izleme verilerini okuyabilir. |
| Kaynak İlkesi Katılımcısı (Önizleme) | Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynak/hiyerarşi okuma hakları olan EA'dan gelen yedek doldurulan kullanıcılar.  |
| [Kullanıcı Erişimi Yöneticisi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#user-access-administrator) | Bir kullanıcının diğer kullanıcıların Azure kaynaklarına erişimini yönetmesine olanak tanır.|
| [Web Sitesi Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#website-contributor) | Web sitelerini (web planlarını değil) yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır...|

'Düzenleme' oluşturmayı, silmeyi ve güncelleştirmeyi içerir:

* Kaynaklar
* Web testleri
* Uyarılar
* Sürekli dışarı aktarma

#### <a name="select-the-user"></a>Kullanıcıyı seçin

İstediğiniz kullanıcı dizinde yoksa, Microsoft hesabı olan herkesi davet edebilirsiniz.
(Outlook.com, OneDrive, Windows Phone veya XBox Live gibi hizmetleri kullanıyorlarsa, microsoft hesapları vardır.)

## <a name="related-content"></a>İlgili içerik

* [Azure'da rol tabanlı erişim denetimi](../../role-based-access-control/role-assignments-portal.md)

## <a name="powershell-query-to-determine-role-membership"></a>Rol üyeliğini belirlemek için PowerShell sorgusu

Belirli roller bildirimlere ve e-posta uyarılarına bağlanabildiği için, belirli bir role ait kullanıcıların listesini oluşturabilmek yararlı olabilir. Bu tür listeler oluşturmaya yardımcı olmak için, özel gereksinimlerinize uyacak şekilde ayarlanabilen aşağıdaki örnek sorguları sunuyoruz:

### <a name="query-entire-subscription-for-admin-roles--contributor-roles"></a>Yönetici rolleri + Katılımcı rolleri için tüm aboneliği sorgula

```powershell
(Get-AzRoleAssignment -IncludeClassicAdministrators | Where-Object {$_.RoleDefinitionName -in @('ServiceAdministrator', 'CoAdministrator', 'Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

### <a name="query-within-the-context-of-a-specific-application-insights-resource-for-owners-and-contributors"></a>Sahipleri ve katkıda bulunanlar için belirli bir Uygulama Öngörüleri kaynağı bağlamında sorgula

```powershell
$resourceGroup = "RGNAME"
$resourceName = "AppInsightsName"
$resourceType = "microsoft.insights/components"
(Get-AzRoleAssignment -ResourceGroup $resourceGroup -ResourceType $resourceType -ResourceName $resourceName | Where-Object {$_.RoleDefinitionName -in @('Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

### <a name="query-within-the-context-of-a-specific-resource-group-for-owners-and-contributors"></a>Sahipleri ve katkıda bulunanlar için belirli bir kaynak grubu bağlamında sorgu

```powershell
$resourceGroup = "RGNAME"
(Get-AzRoleAssignment -ResourceGroup $resourceGroup | Where-Object {$_.RoleDefinitionName -in @('Owner', 'Contributor') } | Select -ExpandProperty SignInName | Sort-Object -Unique) -Join ", "
```

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../../azure-resource-manager/management/overview.md
[portal]: https://portal.azure.com/
[start]: ../../azure-monitor/app/app-insights-overview.md
