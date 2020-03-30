---
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/11/2018
ms.author: rogarana
ms.openlocfilehash: aeb15fbb8da44a203789e06a359cb664998602ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77123170"
---
Azure Dosya Eşitleme aracısı, yeni işlevler eklemek ve sorunları gidermek için düzenli olarak güncelleştirilir. Azure Dosya Eşitleme aracısı için güncelleştirmeleri kullanılabilir hale getirmek için Microsoft Update'i yapılandırmanızı öneririz.

#### <a name="major-vs-minor-agent-versions"></a>Majör ve küçük ajan sürümleri
* Ana aracı sürümleri genellikle yeni özellikler içerir ve sürüm numarasının ilk bölümü olarak artan bir sayıya sahiptir. Örneğin: \*2.\*.\*\*
* Küçük aracı sürümleri de "yamalar" olarak adlandırılır ve ana sürümlere göre daha sık yayımlanır. Genellikle hata düzeltmeleri ve daha küçük iyileştirmeler içerir, ancak yeni özellikler içermez. Örneğin: \* \*.3.\*\*

#### <a name="upgrade-paths"></a>Yükseltme yolları
Azure Dosya Eşitleme aracısı güncelleştirmelerini yüklemenin dört onaylı ve sınanmış yolu vardır. 
1. **(Tercih edilen) Temsilci güncelleştirmelerini otomatik olarak karşıdan yüklemek ve yüklemek için Microsoft Update'i yapılandırın.**  
    Sunucu aracısının en son düzeltmelerine erişebildiğinizden emin olmak için her zaman her Azure Dosya Eşitleme güncelleştirmesini almanızı öneririz. Microsoft Update, güncelleştirmeleri sizin için otomatik olarak indirip yükleyerek bu işlemi sorunsuz hale getirir.
2. **Aracı güncelleştirmelerini indirmek ve yüklemek için AfsUpdater.exe'yi kullanın.**  
    AfsUpdater.exe ajan yükleme dizininde yer almaktadır. Aracı güncelleştirmelerini indirmek ve yüklemek için çalıştırılabilir'e çift tıklayın. 
3. **Microsoft Update yama dosyasI veya .msp çalıştırılabilir kullanarak varolan bir Azure Dosya Eşitleme aracısı yama. En son Azure Dosya Eşitleme güncelleştirme paketi [Microsoft Güncelleştirme Kataloğu'ndan](https://www.catalog.update.microsoft.com/Search.aspx?q=Azure%20File%20Sync)indirilebilir.**  
    .msp çalıştırılabilir çalıştırmak, Azure Dosya Eşitleme yüklemenizi önceki yükseltme yolunda Microsoft Update tarafından otomatik olarak kullanılan yöntemle yükseltir. Microsoft Update düzeltme eki uygulamak, Azure Dosya Eşitleme yüklemesinin yerinde yükseltmesini gerçekleştirir.
4. **[Microsoft İndirme Merkezi'nden](https://go.microsoft.com/fwlink/?linkid=858257)en yeni Azure Dosya Eşitleme aracısını indirin.**  
    Varolan bir Azure Dosya Eşitleme aracısı yüklemesini yükseltmek için eski sürümü kaldırın ve indirilen yükleyiciden en son sürümü yükleyin. Sunucu kaydı, eşitleme grupları ve diğer ayarlar Azure Dosya Eşitleyicisi tarafından korunur.

#### <a name="automatic-agent-lifecycle-management"></a>Otomatik ajan yaşam döngüsü yönetimi
Aracı sürüm 6 ile, dosya eşitleme ekibi bir aracı otomatik yükseltme özelliği tanıttı. İki moddan birini seçebilir ve yükseltmenin sunucuda denenecek bir bakım penceresi belirtebilirsiniz. Bu özellik, aracınızın süresinin dolmasını engelleyen bir korkuluk sağlayarak veya sorun yaşamadan, geçerli ayarı kalmaya izin vererek aracı yaşam döngüsü yönetimikonusunda size yardımcı olmak üzere tasarlanmıştır.
1. **Varsayılan ayar,** aracının son kullanma tarihini engellemeye çalışır. Bir aracının son kullanma tarihinden itibaren 21 gün içinde, aracı kendi kendini yükseltmeye çalışır. Bu, son kullanma tarihinden önceki 21 gün içinde ve seçili bakım penceresinde haftada bir kez yükseltme denemesi başlatacaktır. **Bu seçenek, normal Microsoft Güncelleştirme düzeltme eki alma gereksinimini ortadan kaldırmaz.**
1. İsteğe bağlı olarak, aracının yeni bir aracı sürümü kullanıma sunulduğu anda kendisini otomatik olarak yükselteceğini seçebilirsiniz (şu anda kümelenmiş sunucular için geçerli değildir). Bu güncelleştirme, seçili bakım penceresi sırasında gerçekleşir ve sunucunuzun genel kullanıma sunulduğu anda yeni özelliklerden ve geliştirmelerden yararlanmasını sağlar. Bu, sunucunuza büyük aracı sürümlerinin yanı sıra düzenli güncelleştirme düzeltme eki olacak önerilir, sorunsuz ayardır. Serbest bırakılan her ajan GA kalitesindedir. Bu seçeneği belirlerseniz, Microsoft en yeni aracı sürümünü size karşı kullanacaktır. Kümelenmiş sunucular hariç tutulur. Uçuş tamamlandıktan sonra, aracı microsoft [Download Center](https://go.microsoft.com/fwlink/?linkid=858257) aka.ms/AFS/agent'da da kullanılabilir hale gelecektir.

 ##### <a name="changing-the-auto-upgrade-setting"></a>Otomatik yükseltme ayarını değiştirme

Aşağıdaki yönergeler, yüklemeyi tamamladıktan sonra, değişiklik yapmanız gerekiyorsa ayarları nasıl değiştireceğiniz açıklanır.

PowerShell konsolu açın ve eşitleme aracısını yüklediğiniz dizine gidin ve sunucu cmdlets'i içe aktarın. Varsayılan olarak bu şuna benzer:
```powershell
cd 'C:\Program Files\Azure\StorageSyncAgent'
Import-Module -Name .\StorageSync.Management.ServerCmdlets.dll
```

`Get-StorageSyncAgentAutoUpdatePolicy` Geçerli ilke ayarını denetlemek ve değiştirmek isteyip istemediğinizbelirlenebilir.

Geçerli ilke ayarını gecikmiş güncelleştirme parçasıyla değiştirmek için şunları kullanabilirsiniz:
```powershell
Set-StorageSyncAgentAutoUpdatePolicy -PolicyMode UpdateBeforeExpiration
```

Geçerli ilke ayarını hemen güncelleştirme parçasına değiştirmek için şunları kullanabilirsiniz:
```powershell
Set-StorageSyncAgentAutoUpdatePolicy -PolicyMode InstallLatest
```

#### <a name="agent-lifecycle-and-change-management-guarantees"></a>Ajan yaşam döngüsü ve değişim yönetimi garantileri
Azure Dosya Eşitlemi, sürekli olarak yeni özellikler ve geliştirmeler sunan bir bulut hizmetidir. Bu, belirli bir Azure Dosya Eşitleme aracısı sürümünün yalnızca sınırlı bir süre için desteklenebileceği anlamına gelir. Dağıtımınızı kolaylaştırmak için, aşağıdaki kurallar, değişiklik yönetimi sürecinizde aracı güncelleştirmelerini/yükseltmelerini karşılamak için yeterli zamana ve bildirime sahip olduğunuzu garanti eder:

- Ana aracı sürümleri, ilk sürüm tarihinden itibaren en az altı ay boyunca desteklenir.
- Biz büyük ajan sürümleri desteği arasında en az üç aylık bir örtüşme olduğunu garanti. 
- Son kullanma tarihinden en az üç ay önce süresi dolacak bir aracı kullanan kayıtlı sunucular için uyarılar verilir. Kayıtlı bir sunucunun Depolama Eşitleme Hizmeti'nin kayıtlı sunucular bölümü altında aracının eski bir sürümünü kullanıp kullanmadığınızı kontrol edebilirsiniz.
- Küçük bir aracı sürümün ömrü ilişkili ana sürüme bağlıdır. Örneğin, aracı sürüm 3.0 yayımlandığında, aracı sürümleri 2. \* birlikte sona erecek şekilde ayarlanır.

> [!Note]
> Bir son kullanma uyarısı ile aracı sürümü yüklemek bir uyarı görüntüler, ancak başarılı olur. Süresi dolmuş bir aracı sürümüyüklemeye veya bağlantı kurmaya çalışmak desteklenmez ve engellenir.
