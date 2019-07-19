---
title: Azure Izleyici 'de Log Analytics çalışma alanlarını yönetme | Microsoft Docs
description: Kullanıcılar, hesaplar, çalışma alanları ve Azure hesapları ile ilgili çeşitli yönetim görevlerini kullanarak Azure Izleyici 'de Log Analytics çalışma alanlarını yönetebilirsiniz.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: d0e5162d-584b-428c-8e8b-4dcaa746e783
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 07/16/2019
ms.author: magoedte
ms.openlocfilehash: fbfbd8e26ab3e92f06194322be7ec2be2fb180fd
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68254460"
---
# <a name="manage-log-data-and-workspaces-in-azure-monitor"></a>Azure Izleyici 'de günlük verilerini ve çalışma alanlarını yönetme
Azure Izleyici, günlük verilerini, aslında veri ve yapılandırma bilgilerini içeren bir kapsayıcı olan Log Analytics çalışma alanında depolar. Günlük verilerine erişimi yönetmek için, çalışma alanlarıyla ilgili çeşitli yönetim görevleri gerçekleştirirsiniz. Siz veya kuruluşunuzun diğer üyeleri, IT altyapınızın tümünden veya bir bölümünden toplanan farklı veri kümelerini yönetmek için birden çok çalışma alanı kullanabilirsiniz.

Bu makalede, günlüklere erişimin nasıl yönetileceği ve bunları içeren çalışma alanlarının yönetimi açıklanmaktadır. 

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma
Bir Log Analytics çalışma alanı oluşturmak için şunları yapmanız gerekir:

1. Bir Azure aboneliğine sahip olmanız.
2. Bir çalışma alanı adı seçmeniz.
3. Çalışma alanı biri Abonelikleriniz ve kaynak grubu ile ilişkilendirin.
4. Coğrafi bir konum seçmeniz.

Çalışma alanı oluşturma hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın:

- [Azure portal Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace.md)
- [Azure CLı 2,0 ile Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace-cli.md)
- [Azure PowerShell Log Analytics çalışma alanı oluşturma](../learn/quick-create-workspace-posh.md)

## <a name="determine-the-number-of-workspaces-you-need"></a>İhtiyacınız olan çalışma alanı sayısını belirleme
Log Analytics çalışma alanı bir Azure kaynağıdır ve verilerin toplandığı, toplanan, çözümlenen ve Azure Izleyici 'de sunulduğu bir kapsayıcıdır. Azure aboneliği başına birden fazla çalışma alanınız olabilir ve bunlar genelinde kolayca sorgu yapabilme özelliği sayesinde birden fazla çalışma alanına erişiminiz olabilir. Bu bölümde birden çok çalışma alanı oluşturmanın yararlı olabileceği durumlar açıklanır.

Bir Log Analytics çalışma alanı şunları sağlar:

* Veri depolama için coğrafi bir konum.
* Çalışma alanı merkezli modda farklı Kullanıcı erişim hakları tanımlamak için veri yalıtımı. Kaynak merkezli modda çalışırken ilgili değildir.
* [Fiyatlandırma katmanı](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [bekletme](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period) ve [veri dönüşü](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#daily-cap)gibi ayarların yapılandırılması için kapsam.
* Veri alımı ve bekletme ile ilgili ücretler çalışma alanı kaynağında yapılır.

Tüketimi açısından, mümkün olduğunca az çalışma alanları oluşturma öneririz. Yönetim ve sorgu deneyimi daha kolay ve hızlı kolaylaştırır. Ancak, önceki özelliklere bağlı olarak, durumlarda birden çok çalışma alanı oluşturmak isteyebilirsiniz:

* Küresel bir şirkettir ve veri egemenliği veya uyumluluk nedenleriyle belirli bölgelerde depolanan günlük verilerine ihtiyacınız vardır.
* Azure kullanıyorsanız ve çalışma alanını, yönettiği Azure kaynaklarıyla aynı bölgede bulundurarak giden veri aktarımı ücretlerini ortadan kaldırmak istiyorsanız.
* Yönetilen bir hizmet sağlayıcısıysanız ve yönettiğiniz her bir müşteriye ilişkin Log Analytics verilerini diğer müşterilerin verilerinden yalıtmak istiyorsanız.
* Birden çok müşteriyi yönetebilir ve her bir müşterinin/departmanın/iş grubunun kendi verilerini görmesini, ancak başkalarından verileri görüntülemesini ve birleştirilmiş bir çapraz müşteri/departman/iş grubu görünümü için iş gereksinimi yoktur. ".

Verileri toplamak için Windows aracılarını kullanıyorsanız [her bir aracıyı, bir veya daha fazla çalışma alanına raporlama yapacak şekilde yapılandırabilirsiniz](../../azure-monitor/platform/agent-windows.md).

System Center Operations Manager'ı kullanıyorsanız her bir Operations Manager yönetim grubu yalnızca bir çalışma alanıyla bağlantılı olabilir. Operations Manager tarafından yönetilen bilgisayarlara Microsoft İzleme Aracısını yükleyebilir ve hem Operations Manager hem de farklı bir Log Analytics çalışma alanı için aracı raporu alabilirsiniz.

Çalışma alanı mimarisi tanımlandıktan sonra, bu ilkeyi Azure [ilkesi](../../governance/policy/overview.md)ile Azure kaynakları üzerinde zorlayabilmelisiniz. Bu, tüm Azure kaynaklarına otomatik olarak uygulanacak yerleşik bir tanım sağlayabilir. Örneğin, belirli bir bölgedeki tüm Azure kaynaklarınızın tüm tanılama günlüklerini belirli bir çalışma alanına gönderdiğinden emin olmak için bir ilke ayarlayabilirsiniz.

## <a name="view-workspace-details"></a>Çalışma alanı ayrıntılarını görüntüle
Azure portal **Azure izleyici** menüsünden Log Analytics çalışma alanınızdaki verileri analiz ederken, **Log Analytics çalışma alanları** menüsündeki çalışma alanlarını oluşturup yönetirsiniz.
 

1. [Azure Portal](https://portal.azure.com) oturum açın ve **tüm hizmetler**' e tıklayın. Kaynak listesinde **Log Analytics** yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Log Analytics** çalışma alanlarını seçin.  

    ![Azure portal](media/manage-access/azure-portal-01.png)  

3. Listeden çalışma alanınızı seçin.

4. Çalışma alanı sayfası, ek bilgiler için çalışma alanı, başlangıç, yapılandırma ve bağlantılarla ilgili ayrıntıları görüntüler.  

    ![Çalışma alanı ayrıntıları](./media/manage-access/workspace-overview-page.png)  


## <a name="workspace-permissions-and-scope"></a>Çalışma alanı izinleri ve kapsamı
Bir kullanıcının erişimi olan veriler, aşağıdaki tabloda listelenen birden çok faktöre göre belirlenir. Her biri aşağıdaki bölümlerde açıklanmıştır.

| faktörü | Açıklama |
|:---|:---|
| [Erişim modu](#access-modes) | Kullanıcının çalışma alanına erişmesi için kullandığı yöntem.  Kullanılabilir verilerin kapsamını ve uygulanan erişim denetimi modunu tanımlar. |
| [Erişim denetimi modu](#access-control-mode) | Çalışma alanında izinlerin, çalışma alanında veya kaynak düzeyinde uygulanıp uygulanmadığını tanımlayan ayar. |
| [İzinler](#manage-accounts-and-users) | Çalışma alanı veya kaynak için bir kişiye veya kullanıcı grubuna uygulanan izinler. Kullanıcının erişimi olacak verileri tanımlar. |
| [Tablo düzeyi RBAC](#table-level-rbac) | Erişim modundan veya erişim denetimi modundan bağımsız olarak tüm kullanıcılar için geçerli olan isteğe bağlı ayrıntılı izinler. Bir kullanıcının erişebileceği veri türlerini tanımlar. |



## <a name="access-modes"></a>Erişim modları
_Erişim modu_ , bir kullanıcının Log Analytics çalışma alanına nasıl eriştiğini ve erişebileceği verilerin kapsamını nasıl tanımladığını gösterir. 

**Çalışma alanı merkezli**: Bu modda, bir Kullanıcı, izinleri olan çalışma alanındaki tüm günlükleri görüntüleyebilir. Bu moddaki sorgular, çalışma alanındaki tüm tablolardaki tüm verilerin kapsamına alınır. Bu, Azure portal **Azure izleyici** menüsünden **Günlükler** ' i seçerken olduğu gibi, kapsam olarak çalışma alanıyla erişildiğinde kullanılan erişim modudur.

**Kaynak merkezli**: Belirli bir kaynak için çalışma alanına eriştiğinizde (örneğin, Azure portal bir kaynak menüsünden **Günlükler** ' i seçtiğinizde, yalnızca bu kaynak için, erişiminiz olan tüm tablolardaki günlükleri görüntüleyebilirsiniz. Bu moddaki sorgular yalnızca söz konusu kaynakla ilişkili verilere göre kapsamlandırılır. Bu mod ayrıca ayrıntılı rol tabanlı erişim denetimi 'ni (RBAC) da sunar. 

> [!NOTE]
> Günlükler yalnızca ilgili kaynakla düzgün şekilde ilişkilendirildiklerinde kaynak merkezli sorgular için kullanılabilir. Şu anda aşağıdaki kaynaklarda sınırlamalar var: 
> - Azure dışındaki bilgisayarlar
> - Service Fabric
> - Application Insights
> - Kapsayıcılar
>
> Bir sorgu çalıştırıp ilgilendiğiniz kayıtları inceleyerek günlüklerin kaynakları ile düzgün bir şekilde ilişkilendiriliyorsa test edebilirsiniz. Doğru kaynak KIMLIĞI [_Resourceıd](log-standard-properties.md#_resourceid) özelliğinde ise, veriler kaynak merkezli sorgular tarafından kullanılabilir.

### <a name="comparing-access-modes"></a>Erişim modlarını karşılaştırma

Aşağıdaki tabloda erişim modları özetlenmektedir:

| | Çalışma alanı merkezli | Kaynak merkezli |
|:---|:---|:---|
| Her bir model kim içindir? | Yönetim Merkezi. Veri toplamayı ve çok çeşitli kaynaklara erişmesi gereken kullanıcıları yapılandırması gereken yöneticiler. Şu anda Azure dışındaki kaynaklar için günlüklere erişmesi gereken kullanıcılar için de gereklidir. | Uygulama takımları. İzlenmekte olan Azure kaynaklarının yöneticileri. |
| Kullanıcı günlükleri görüntülemek için ne gerekir? | Çalışma alanı izinleri. Bkz. [hesapları ve kullanıcıları yönetme](#manage-accounts-and-users)Içindeki **çalışma alanı izinleri** . | Kaynağa yönelik okuma erişimi. Bkz. [hesapları ve kullanıcıları yönetme](#manage-accounts-and-users)içindeki **kaynak izinleri** . İzinler devralınabilir (örneğin, kapsayan kaynak grubundan) veya doğrudan kaynağa atanabilir. Kaynak için günlüklere izin verilecek izinler otomatik olarak atanır. |
| İzinlerin kapsamı nedir? | Alanında. Çalışma alanına erişimi olan kullanıcılar, bu çalışma alanındaki tüm günlükleri izinleri olan tablolardan sorgulayabilir. Bkz. [tablo erişim denetimi](#table-level-rbac) | Azure kaynağı. Kullanıcı herhangi bir çalışma alanından erişimi olan kaynaklar için günlükleri sorgulayabilir, ancak diğer kaynakların günlüklerini sorgulayamaz. |
| Kullanıcı günlüklere nasıl erişebilir? | **Günlükleri** **Azure izleyici** menüsünden veya **Log Analytics çalışma alanlarından**başlatın. | Azure kaynağı menüsünden **günlükleri** başlatın. |


## <a name="access-control-mode"></a>Erişim denetimi modu
_Erişim denetimi modu_ , bu çalışma alanı için izinlerin nasıl belirlendiğini tanımlayan her çalışma alanındaki bir ayardır.

**Çalışma alanı Izinleri iste**:  Bu denetim modu parçalı RBAC 'e izin vermiyor. Bir kullanıcının çalışma alanına erişmesi için, çalışma alanına veya belirli tablolara izin verilmesi gerekir. 

Bir kullanıcı çalışma alanına, çalışma alanı merkezli modda eriştiğinde, erişim izni verilen tüm tablolar veri erişimine sahip olur. Bir kullanıcı çalışma alanına kaynak merkezli modda eriştiğinde, erişim izni verilen tablolardaki bu kaynağa ait verilere yalnızca erişim hakkı verilecektir.

Bu, 2019 Mart 'tan önce oluşturulan tüm çalışma alanları için varsayılan ayardır.

**Kaynak veya çalışma alanı Izinlerini kullanın**: Bu denetim modu parçalı RBAC sağlar. Kullanıcılara yalnızca Azure izinleri, izinleri olan `read` kaynaklar aracılığıyla görüntüleyebilecekleri kaynaklarla ilişkili verilere erişim izni verilir. 

Bir kullanıcı çalışma alanı merkezli modda çalışma alanına eriştiğinde, çalışma alanı izinleri uygulanır. Bir Kullanıcı, kaynak merkezli modda çalışma alanına eriştiğinde yalnızca kaynak izinleri doğrulanır ve çalışma alanı izinleri yok sayılır. Çalışma alanı izinlerinden kaldırarak ve kaynak izinlerinin tanınmasını sağlayarak bir kullanıcı için RBAC 'yi etkinleştirin.

Bu, Mart 2019 ' den sonra oluşturulan tüm çalışma alanları için varsayılan ayardır.

> [!NOTE]
> Bir kullanıcının çalışma alanında yalnızca kaynak izinleri varsa, yalnızca [kaynak merkezli modu](#access-modes)kullanarak çalışma alanına erişebilecektir.


### <a name="define-access-control-mode-in-azure-portal"></a>Azure portal erişim denetimi modunu tanımlama
**Log Analytics çalışma** alanı menüsündeki çalışma alanının **genel bakış** sayfasında geçerli çalışma alanı erişim denetimi modunu görüntüleyebilirsiniz.

![Çalışma alanı erişim denetimi modunu görüntüle](media/manage-access/view-access-control-mode.png)

Bu ayarı, çalışma alanının **Özellikler** sayfasında değiştirebilirsiniz. Çalışma alanını yapılandırma izniniz yoksa ayarı değiştirmek devre dışı bırakılır.

![Çalışma alanı erişim modunu değiştir](media/manage-access/change-access-control-mode.png)

### <a name="define-access-control-mode-in-powershell"></a>PowerShell 'de erişim denetimi modunu tanımlama

Abonelikteki tüm çalışma alanları için erişim denetimi modunu incelemek üzere aşağıdaki komutu kullanın:

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {$_.Name + ": " + $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions} 
```

Belirli bir çalışma alanı için erişim denetimi modunu ayarlamak üzere aşağıdaki betiği kullanın:

```powershell
$WSName = "my-workspace"
$Workspace = Get-AzResource -Name $WSName -ExpandProperties
if ($Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $Workspace.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $Workspace.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $Workspace.ResourceId -Properties $Workspace.Properties -Force
```

Abonelikteki tüm çalışma alanları için erişim denetimi modunu ayarlamak üzere aşağıdaki betiği kullanın

```powershell
Get-AzResource -ResourceType Microsoft.OperationalInsights/workspaces -ExpandProperties | foreach {
if ($_.Properties.features.enableLogAccessUsingOnlyResourcePermissions -eq $null) 
    { $_.Properties.features | Add-Member enableLogAccessUsingOnlyResourcePermissions $true -Force }
else 
    { $_.Properties.features.enableLogAccessUsingOnlyResourcePermissions = $true }
Set-AzResource -ResourceId $_.ResourceId -Properties $_.Properties -Force
```

### <a name="define-access-mode-in-resource-manager-template"></a>Kaynak Yöneticisi şablonunda erişim modunu tanımlama
Azure Resource Manager şablonunda erişim modunu yapılandırmak için, çalışma alanında **Enablelogaccessusingonlyresourcepermissions** Özellik bayrağını aşağıdaki değerlerden birine ayarlayın.

- **yanlış**: Çalışma alanını çalışma alanı merkezli izinlere ayarlayın. Bayrak ayarlanmamışsa bu varsayılan ayardır.
- **doğru**: Çalışma alanını Kaynak merkezli izinlere ayarlayın.


## <a name="manage-accounts-and-users"></a>Hesapları ve kullanıcıları yönetme
Belirli bir kullanıcıya uygulanan çalışma alanı izinleri, erişim moduyla ve çalışma alanının [erişim denetimi moduna](#access-control-mode) göre tanımlanır. Bir kullanıcı çalışma alanı [merkezli modda](#access-modes) **çalışma alanı merkezli** bir çalışma alanına eriştiğinde **çalışma alanı izinleri** uygulanır. Kaynak **izinleri** , bir Kullanıcı kaynak [merkezli modu](#access-modes)kullanarak **kaynak veya çalışma alanı izinleri** [erişim denetimi moduna](#access-control-mode) sahip bir çalışma alanına eriştiğinde uygulanır.

### <a name="workspace-permissions"></a>Çalışma alanı izinleri
Her çalışma alanı kendisiyle ilişkilendirilmiş birden çok hesap içerebilir ve her hesabı birden çok çalışma alanına erişim sahibi olabilir. Erişim aracılığıyla yönetilir [Azure rol tabanlı erişim](../../role-based-access-control/role-assignments-portal.md). 


Şu etkinlikler de Azure izinleri gerektirir:

||Eylem |Gereken Azure İzni |Notlar |
|-------|-------------------------|------|
| İzleme çözümlerini ekleme ve kaldırma | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/*` <br> `Microsoft.OperationsManagement/*` <br> `Microsoft.Automation/*` <br> `Microsoft.Resources/deployments/*/write` | Bu izinlerin kaynak grubu veya abonelik düzeyinde verilmiş olması gerekir. |
| Fiyatlandırma katmanını değiştirme | `Microsoft.OperationalInsights/workspaces/*/write` | |
| *Backup* ve *Site Recovery* çözüm kutucuklarındaki verileri görüntüleme | Yönetici / Ortak yönetici | Klasik dağıtım modeli kullanılarak dağıtılan kaynaklara erişir |
| Azure portalında bir çalışma alanı oluşturma | `Microsoft.Resources/deployments/*` <br> `Microsoft.OperationalInsights/workspaces/*` ||
| Çalışma alanı temel özelliklerini görüntüleyin ve portalda çalışma alanı Dikey penceresini girin | `Microsoft.OperationalInsights/workspaces/read` ||
| Tüm arabirimleri kullanarak günlükleri sorgula | `Microsoft.OperationalInsights/workspaces/query/read` ||
| Sorguları kullanarak tüm günlük türlerine erişin | `Microsoft.OperationalInsights/workspaces/query/*/read` ||
| Belirli bir günlük tablosuna erişme | `Microsoft.OperationalInsights/workspaces/query/<table_name>/read` ||
| Bu çalışma alanına Günlükler gönderilmesine izin vermek için çalışma alanı anahtarlarını okuyun | `Microsoft.OperationalInsights/workspaces/sharedKeys/action` ||



#### <a name="manage-access-to-log-analytics-workspace-using-azure-permissions"></a>Azure izinlerini kullanarak Log Analytics çalışma alanına erişimi yönetme 
Azure izinlerini kullanarak Log Analytics çalışma alanına izin vermek için, [Azure abonelik kaynaklarınıza erişimi yönetmek için rol atamalarını kullanma](../../role-based-access-control/role-assignments-portal.md) bölümündeki adımları izleyin.

Azure 'da Log Analytics çalışma alanları için iki yerleşik kullanıcı rolü vardır:
- Log Analytics Okuyucusu
- Log Analytics Katkıda Bulunan

*Log Analytics Okuyucusu* rolünün üyeleri aşağıdakileri yapabilir:
- Tüm izleme verilerini görüntüleme ve arama 
- Tüm Azure kaynakları üzerinde Azure tanılama yapılandırmasını görüntüleme dahil olmak üzere, izleme ayarlarını görüntüleyin.

Log Analytics okuyucusu rolü, aşağıdaki Azure eylemleri içerir:

| Tür    | İzin | Açıklama |
| ------- | ---------- | ----------- |
| Eylem | `*/read`   | Tüm Azure kaynaklarını ve kaynak yapılandırması görüntüleme olanağı. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, tam Kısıtlanmamış izinlerin çalışma alanı ayarlarını okumasını ve verilerin üzerinde sorgu gerçekleştirmesini sağlar. Daha ayrıntılı seçeneklere bakın. |
| Action | `Microsoft.OperationalInsights/workspaces/analytics/query/action` | Kullanım dışı, bunları kullanıcılara atamaya gerek yok. s |
| Action | `Microsoft.OperationalInsights/workspaces/search/action` | Kullanım dışı, kullanıcılara atanması gerekmez. |
| Action | `Microsoft.Support/*` | Destek olayları açma özelliği |
|Eylem Dışı | `Microsoft.OperationalInsights/workspaces/sharedKeys/read` | Çalışma alanının engeller veri koleksiyonu API'sini kullanmak ve aracıları yüklemek için gereken anahtarı. Bu kullanıcının yeni kaynaklar çalışma alanına eklemesini engeller |


*Log Analytics Katkıda Bulunan* rolünün üyeleri aşağıdakileri yapabilir:
- Log Analytics okuyucusu gibi tüm izleme verilerini okuma  
- Otomasyon hesaplarını oluşturma ve yapılandırma  
- Yönetim çözümlerini ekleme ve kaldırma    
    > [!NOTE] 
    > Başarıyla son iki eylemleri gerçekleştirmek için bu izin kaynak grubu veya abonelik düzeyinde verilmesi gerekir.  

- Depolama hesabı anahtarlarını okuma   
- Azure Depolama’dan günlüklerin toplanmasını yapılandırma  
- Azure kaynaklarına ilişkin izleme ayarlarını düzenleme:
  - VM'lere VM uzantısı ekleme
  - Tüm Azure kaynaklarında Azure tanılamayı yapılandırma

> [!NOTE] 
> Özelliği, bir sanal makine üzerinde tam denetim kazanmak için bir sanal makineye sanal makine uzantısı eklemek için kullanabilirsiniz.

Log Analytics katkıda bulunan rolü, aşağıdaki Azure eylemleri içerir:

| İzin | Açıklama |
| ---------- | ----------- |
| `*/read`     | Tüm Azure kaynaklarını ve kaynak yapılandırması görüntüleme olanağı. Aşağıdakileri görüntülemeyi içerir: <br> Sanal makine uzantısı durumu <br> Kaynaklarda Azure tanılamalarının yapılandırması <br> Tüm kaynakların tüm özellikleri ve ayarları. <br> Çalışma alanları için, tam Kısıtlanmamış izinlerin çalışma alanı ayarlarını okumasını ve verilerin üzerinde sorgu gerçekleştirmesini sağlar. Daha ayrıntılı seçeneklere bakın. |
| `Microsoft.Automation/automationAccounts/*` | Runbook'ları ekleme ve düzenleme dahil olmak üzere Azure Otomasyonu hesapları oluşturma ve yapılandırma olanağı |
| `Microsoft.ClassicCompute/virtualMachines/extensions/*` <br> `Microsoft.Compute/virtualMachines/extensions/*` | Linux uzantısı için OMS aracısı ve Microsoft Monitoring Agent gibi sanal makine uzantılarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.ClassicStorage/storageAccounts/listKeys/action` <br> `Microsoft.Storage/storageAccounts/listKeys/action` | Depolama hesabı anahtarını görüntüleyin. Log Analytics’i Azure depolama hesaplarındaki günlükleri okuyacak şekilde yapılandırmak için gereklidir |
| `Microsoft.Insights/alertRules/*` | Uyarı kurallarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.Insights/diagnosticSettings/*` | Azure kaynaklarında tanılama ayarlarını ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationalInsights/*` | Log Analytics çalışma alanları için yapılandırmaları ekleme, güncelleştirme ve kaldırma |
| `Microsoft.OperationsManagement/*` | Yönetim çözümlerini ekleme ve kaldırma |
| `Microsoft.Resources/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |
| `Microsoft.Resources/subscriptions/resourcegroups/deployments/*` | Dağıtımları oluşturma ve silme. Çözümleri, çalışma alanlarını ve otomasyon hesaplarını eklemek ve kaldırmak için gereklidir |

Kullanıcıları bir kullanıcı rolüne eklemek ve bu rolden kaldırmak için `Microsoft.Authorization/*/Delete` ve `Microsoft.Authorization/*/Write` izinleri gereklidir.

Bu rolleri, kullanıcılara farklı kapsamlarda erişim vermek için kullanın:
- Abonelik - Abonelikteki tüm çalışma alanlarına erişim
- Kaynak grubu - Kaynak grubundaki tüm çalışma alanına erişim
- Kaynak - Yalnızca belirtilen çalışma alanına erişim

Doğru erişim denetimini güvence altına almak için atamaları Kaynak düzeyinde (çalışma alanı) gerçekleştirmeniz gerekir.  Gereken özel izinlere sahip rolleri oluşturmak için [özel rolleri](../../role-based-access-control/custom-roles.md) kullanın.

### <a name="resource-permissions"></a>Kaynak izinleri 
Kullanıcılar, kaynak merkezli erişimi kullanarak bir çalışma alanından günlükleri sorgularsa, kaynak üzerinde aşağıdaki izinlere sahip olurlar:

| İzin | Açıklama |
| ---------- | ----------- |
| `Microsoft.Insights/logs/<tableName>/read`<br><br>Örnekler:<br>`Microsoft.Insights/logs/*/read`<br>`Microsoft.Insights/logs/Heartbeat/read` | Kaynak için tüm günlük verilerini görüntüleme olanağı.  |
| `Microsoft.Insights/diagnosticSettings/write ` | Tanılama ayarını bu kaynak için günlükleri ayarlamaya izin verecek şekilde yapılandırma özelliği. |

Bu izin genellikle, yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) ve [katkıda](../../role-based-access-control/built-in-roles.md#contributor) bulunan rolleri gibi  _\*/Read veya_ _\*_ Permissions içeren bir rolden verilir. Belirli eylemleri içeren özel rollerin veya adanmış yerleşik rollerin bu izni içeremediğini unutmayın.

Farklı tablolar için farklı erişim denetimi oluşturmak isterseniz, aşağıdaki [tablo başına erişim denetimi tanımlama](#table-level-rbac) bölümüne bakın.


## <a name="table-level-rbac"></a>Tablo düzeyi RBAC
**Tablo DÜZEYI RBAC** , diğer izinlerin yanı sıra bir Log Analytics çalışma alanındaki verilere daha ayrıntılı denetim sağlamanıza olanak tanır. Bu denetim, yalnızca belirli bir kullanıcı kümesi için erişilebilen belirli veri türlerini tanımlamanızı sağlar.

Çalışma alanındaki belirli [tablolara](../log-query/logs-structure.md) erişim izni vermek ya da erişimi reddetmek için [Azure özel rolleriyle](../../role-based-access-control/custom-roles.md) tablo erişim denetimi uygulayabilirsiniz. Bu roller, kullanıcının [erişim modundan](#access-modes)bağımsız olarak, çalışma alanı merkezli veya kaynak merkezli [erişim denetimi modundaki](#access-control-mode) çalışma alanlarına uygulanır.

Tablo erişim denetimine erişimi tanımlamak için aşağıdaki eylemlerle [özel bir rol](../../role-based-access-control/custom-roles.md) oluşturun.

- Bir tabloya erişim vermek için, rol tanımının **Eylemler** bölümüne ekleyin.
- Bir tabloya erişimi reddetmek için, rol tanımının **NotActions** bölümüne ekleyin.
- Tüm tabloları belirtmek için * kullanın.

Örneğin, _sinyal_ ve _AzureActivity_ tablolarına erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
"Actions":  [
              "Microsoft.OperationalInsights/workspaces/query/Heartbeat/read",
              "Microsoft.OperationalInsights/workspaces/query/AzureActivity/read"
  ],
```

Yalnızca _Securitybaseline_ 'e erişimi olan ve başka tablolar olmadan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read"
    ],
    "NotActions":  [
        "Microsoft.OperationalInsights/workspaces/query/*/read"
    ],
```

### <a name="custom-logs"></a>Özel günlükler
 Özel Günlükler, özel Günlükler ve HTTP Veri Toplayıcı API 'SI gibi veri kaynakları tarafından oluşturulur. Günlük türünü belirlemenin en kolay yolu, [günlük şemasında özel Günlükler](../log-query/get-started-portal.md#understand-the-schema)altında listelenen tabloları denetlemede olur.

 Şu anda tek tek özel günlüklere erişim izni veremez veya vermeyebilirsiniz, ancak tüm özel günlüklere erişim verebilir veya erişimi reddedebilirsiniz. Tüm özel günlüklere erişimi olan bir rol oluşturmak için aşağıdaki eylemleri kullanarak özel bir rol oluşturun:

```
    "Actions":  [
        "Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read"
    ],
```

### <a name="considerations"></a>Dikkat edilmesi gerekenler

- Bir kullanıcıya,  _\*/Read_ eylemini içeren standart okuyucu veya katkıda bulunan rollerle genel okuma izni verildiyse, tablo başına erişim denetimini geçersiz kılar ve tüm günlük verilerine erişim sağlar.
- Bir kullanıcıya tablo başına erişim verildiyse ancak başka izinler yoksa, API 'den günlük verilerine erişebilecek ancak Azure portal. Azure portal erişim sağlamak için, temel rolü olarak Log Analytics okuyucu kullanın.
- Aboneliğin yöneticileri, diğer izin ayarlarından bağımsız olarak tüm veri türlerine erişebilir.
- Çalışma alanı sahipleri, tablo başına erişim denetimi için diğer kullanıcılar gibi davranır.
- Atama sayısını azaltmak için bireysel kullanıcılar yerine güvenlik gruplarına roller atamanız gerekir. Bu Ayrıca, erişimi yapılandırmak ve doğrulamak için mevcut Grup Yönetimi araçlarını kullanmanıza yardımcı olur.




## <a name="next-steps"></a>Sonraki adımlar
* Bkz: [Log Analytics Aracısı genel bakış](../../azure-monitor/platform/log-analytics-agent.md) veri merkezinizde veya diğer bulut ortamında bulunan bilgisayarlardaki verileri toplamak için.
* Azure VM’lerden veri toplamayı yapılandırmak için bkz. [Azure Sanal Makineler hakkında veri toplama](../../azure-monitor/learn/quick-collect-azurevm.md).  

