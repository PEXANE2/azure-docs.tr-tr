---
title: Yönetim Işlemleri
titleSuffix: Azure SQL Managed Instance
description: Azure SQL yönetilen örnek yönetimi işlemleri süresi ve en iyi uygulamalar hakkında bilgi edinin.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: urosmil
ms.author: urmilano
ms.reviewer: sstein, carlrab, MashaMSFT
ms.date: 07/10/2020
ms.openlocfilehash: 5cff54b1f71d30f7932c4ead722d1dda0a7aec57
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86531989"
---
# <a name="overview-of-azure-sql-managed-instance-management-operations"></a>Azure SQL yönetilen örnek yönetimi işlemlerine genel bakış
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

## <a name="what-are-management-operations"></a>Yönetim işlemleri nelerdir?
Azure SQL yönetilen örneği, yeni yönetilen örnekleri otomatik olarak dağıtmak, örnek özelliklerini güncelleştirmek ve artık gerekli olmadığında örnekleri silmek için kullanabileceğiniz yönetim işlemleri sağlar.

[Azure sanal ağları içindeki dağıtımları](../../virtual-network/virtual-network-for-azure-services.md) desteklemek ve müşterilere yalıtım ve güvenlik sağlamak IÇIN, SQL yönetilen örneği [sanal kümelere](connectivity-architecture-overview.md#high-level-connectivity-architecture)bağımlıdır. Sanal küme, müşterinin sanal ağ alt ağı içinde dağıtılan ayrılmış bir yalıtılmış sanal makine kümesini temsil eder. Temelde, boş bir alt ağda bulunan her yönetilen örnek dağıtımı yeni bir sanal küme oluşturma ile sonuçlanır.

Dağıtılmış yönetilen örneklerin sonraki işlemleri de temel alınan sanal kümede etkileri olabilir. Bu işlemler, ek sanal makinelerin dağıtımı, Yeni dağıtımlar veya mevcut yönetilen örneklere güncelleştirmeler planlarken dikkate alınması gereken bir ek yük ile birlikte, yönetim işlemlerinin süresini etkiler.

Tüm yönetim işlemleri aşağıdaki gibi kategorilere ayrılabilir:

- Örnek dağıtımı (yeni örnek oluşturma).
- Örnek Güncelleştirmesi (sanal çekirdekler veya ayrılmış depolama gibi örnek özellikleri değiştirme).
- Örnek silme.

## <a name="management-operations-duration"></a>Yönetim işlemleri süresi
Genellikle, sanal kümelerdeki işlemler en uzun sürer. Sanal kümelerdeki işlemlerin süresi farklılık gösterir: aşağıda, mevcut hizmet telemetri verilerine göre genellikle tahmin edebileceğiniz değerler verilmiştir:

- **Sanal küme oluşturma**: oluşturma, örnek yönetimi işlemlerinde zaman uyumlu bir adımdır. **işlemlerin %90 ' i 4 saat içinde tamamlanır**.
- **Sanal küme yeniden boyutlandırma (genişleme veya daraltma)**: genişletme zaman uyumsuz olarak (örnek yönetim işlemleri süresince etkilenmeden) zaman uyumsuz olarak gerçekleştirilir. **küme genişletmeleri %90, en az 2,5 saat içinde tamamlanır**.
- **Sanal küme silme**: silme işlemi zaman uyumsuz bir adımdır, ancak boş bir sanal kümede [el ile de başlatılabilir](virtual-cluster-delete.md) , bu durumda eşzamanlı olarak yürütülür. **%90, sanal küme silme işleminin 1,5 saat içinde sona ermesini**.

Ayrıca, örneklerin yönetimi barındırılan veritabanlarında gerçekleştirilen işlemlerden birini de içerebilir ve bu da uzun süreler oluşmasına neden olabilir:

- **Azure depolama 'dan veritabanı dosyaları iliştirme**: genel amaçlı hizmet katmanında Işlem (vçekirdekler) veya depolama alanı ölçekleme gibi zaman uyumlu bir adım. **Bu işlemlerin %90 ' i 5 dakika içinde tamamlanır**.
- **Always on kullanılabilirlik grubu dengeli dağıtım**: iş açısından kritik hizmet katmanındaki Işlem (vçekirdekler) veya depolama Ölçeklendirmesi gibi zaman uyumlu bir adım ve hizmet katmanını Genel Amaçlı iken iş açısından kritik (veya tam tersi) olarak değiştirme. Bu işlemin süresi toplam veritabanı boyutuyla ve geçerli veritabanı etkinliğiyle orantılıdır (etkin işlem sayısı). Bir örneği güncelleştirme sırasında veritabanı etkinliği toplam süreye önemli bir varyans getirebilir. **Bu işlemlerin %90 ' i 220 GB/saat veya daha yüksek bir süre içinde yürütülür**.

Aşağıdaki tabloda işlemler ve genel toplam süreler özetlenmektedir:

|Kategori  |İşlem  |Uzun süre çalışan segment  |Tahmini süre  |
|---------|---------|---------|---------|
|**Dağıtım** |Boş bir alt ağdaki ilk örnek|Sanal küme oluşturma|işlemlerin %90 ' i 4 saat içinde tamamlanır.|
|Dağıtım |Boş olmayan bir alt ağda (örneğin, gen 4 örneklerine sahip bir alt ağdaki ilk Gen 5 örneği) başka bir donanım oluşturma örneği|Sanal küme oluşturma *|işlemlerin %90 ' i 4 saat içinde tamamlanır.|
|Dağıtım |Boş veya boş olmayan bir alt ağda 4 sanal çekirdekler için ilk örnek oluşturma|Sanal küme oluşturma * *|işlemlerin %90 ' i 4 saat içinde tamamlanır.|
|Dağıtım |Boş olmayan alt ağ içinde sonraki örnek oluşturma (2., üçüncü, vb. örnek)|Sanal küme yeniden boyutlandırma|işlemin %90 ' i 2,5 saat içinde tamamlanır.|
|**Güncelleştirme** |Örnek özelliği değişikliği (yönetici parolası, Azure AD oturum açma, Azure Hibrit Avantajı bayrağı)|Yok|En fazla 1 dakika.|
|Güncelleştirme |Örnek depolama ölçeği artırma/azaltma (Genel Amaçlı hizmet katmanı)|Veritabanı dosyalarını iliştirme|İşlem %90, 5 dakika içinde tamamlanır.|
|Güncelleştirme |Örnek depolama ölçeği artırma/azaltma (İş Açısından Kritik hizmet katmanı)|-Sanal küme yeniden boyutlandırma<br>-Always on kullanılabilirlik grubu dengeli dağıtımı|işlemlerin %90 ' i, tüm veritabanlarının (220 GB/saat) temel aldığı 2,5 saat ile sona ermesini sağlar.|
|Güncelleştirme |Örnek işlem (Vçekirdekler) ölçeği artırma ve azaltma (Genel Amaçlı)|-Sanal küme yeniden boyutlandırma<br>-Veritabanı dosyaları iliştirme|işlemin %90 ' i 2,5 saat içinde tamamlanır.|
|Güncelleştirme |Örnek işlem (Vçekirdekler) ölçeği artırma ve azaltma (İş Açısından Kritik)|-Sanal küme yeniden boyutlandırma<br>-Always on kullanılabilirlik grubu dengeli dağıtımı|işlemlerin %90 ' i, tüm veritabanlarının (220 GB/saat) temel aldığı 2,5 saat ile sona ermesini sağlar.|
|Güncelleştirme |Örnek hizmeti katmanı değişikliği (İş Açısından Kritik Genel Amaçlı ve tam tersi)|-Sanal küme yeniden boyutlandırma<br>-Always on kullanılabilirlik grubu dengeli dağıtımı|işlemlerin %90 ' i, tüm veritabanlarının (220 GB/saat) temel aldığı 2,5 saat ile sona ermesini sağlar.|
|**Silme**|Örnek silme|Tüm veritabanları için günlük kuyruğu yedeklemesi|%90 işlem en fazla 1 dakika içinde tamamlanır.<br>Note: alt ağdaki son örnek silinirse, bu işlem 12 saat sonra sanal küme silmeyi zamanlar. * *|
|Silme|Sanal küme silme (Kullanıcı tarafından başlatılan işlem olarak)|Sanal küme silme|İşlem %90, en fazla 1,5 saat içinde tamamlanır.|

\*Sanal küme, donanım oluşturma başına oluşturulur.

\*\*12 saat geçerli yapılandırmadır, ancak gelecekte değişebilir, bu yüzden buna çok daha fazla bağımlılık yapmayın. Bir sanal kümeyi daha önce silmeniz gerekiyorsa (örneğin, alt ağı serbest bırakmak için), bkz. [yönetilen bir örneği sildikten sonra bir alt ağı silme](virtual-cluster-delete.md).

## <a name="instance-availability-during-management-operations"></a>Yönetim işlemleri sırasında örnek kullanılabilirliği

Güncelleştirme **işlemleri SıRASıNDA**SQL yönetilen örneği, güncelleştirmenin sonunda gerçekleşen yük devretmenin neden olduğu kısa bir kesinti dışında kullanılabilir. Genellikle, uzun süreli işlemlerin kesintiye uğramasından, [hızlandırılmış veritabanı kurtarması](../accelerated-database-recovery.md)sayesinde 10 saniyeye kadar sürer.

> [!IMPORTANT]
> Azure SQL yönetilen örneği 'nin işlem veya depolama alanını ölçeklendirmeniz veya hizmet katmanını uzun süre çalışan işlemler (veri alma, veri işleme işleri, dizin yeniden oluşturma vb.) ile aynı anda değiştirmeniz önerilmez. İşlemin sonunda gerçekleştirilecek veritabanı yük devretmesi, devam eden tüm işlemleri iptal eder.

SQL yönetilen örneği, dağıtım ve silme işlemleri sırasında istemci uygulamaları için kullanılamaz.

## <a name="management-operations-cross-impact"></a>Yönetim işlemleri çapraz etki

Yönetilen bir örnekteki yönetim işlemleri, aynı sanal kümenin içine yerleştirilmiş örneklerin diğer yönetim işlemlerini etkileyebilir:

- Bir sanal kümede **uzun süre çalışan geri yükleme işlemleri** , aynı alt ağdaki diğer örnek oluşturma veya ölçeklendirme işlemlerini beklemeye alır.<br/>**Örnek:** Uzun süre çalışan bir geri yükleme işlemi varsa ve aynı alt ağda bir oluşturma veya ölçeklendirme isteği varsa, devam etmeden önce geri yükleme işleminin tamamlanmasını bekleneceği için bu isteğin tamamlanması daha uzun sürer.
    
- **Sonraki bir örnek oluşturma veya ölçeklendirme** işlemi, daha önce başlatılmış bir örnek oluşturma veya sanal küme yeniden boyutlandırmayı başlatan örnek ölçeği tarafından beklemeye konur.<br/>**Örnek:** Aynı alt ağda aynı sanal küme altında birden fazla oluşturma ve/veya ölçek isteği varsa ve bunlardan biri bir sanal küme yeniden boyutlandırmasını başlatırsa, bu isteklerin devam etmeden önce yeniden boyutlandırmanın tamamlanmasını beklemek zorunda kaldıkları için, sanal küme yeniden boyutlandırmasını gerektiren 5 + dakika sonra gönderilen tüm istekler beklenenden daha uzun olur.

- **5 dakikalık bir pencerede gönderilen oluşturma/ölçeklendirme işlemleri** , toplu olarak oluşturulur ve paralel olarak yürütülür.<br/>**Örnek:** 5 dakikalık bir pencerede gönderilen tüm işlemler için yalnızca bir sanal küme yeniden boyutlandırma gerçekleştirilecek (ilk işlem isteğini yürütme saatinden ölçme). İlk gönderildikten sonra başka bir istek 5 dakikadan uzun bir süre gönderilirse, yürütme başlamadan önce sanal küme yeniden boyutlandırmanın tamamlanmasını bekler.

> [!IMPORTANT]
> Devam eden başka bir işlem nedeniyle bekletilen yönetim işlemleri, devam eden koşullar karşılandıktan sonra otomatik olarak sürdürülecek. Geçici olarak duraklatılan yönetim işlemlerini sürdürmeniz için Kullanıcı eylemi gerekli değildir.

## <a name="canceling-management-operations"></a>Yönetim işlemlerini iptal etme

Aşağıdaki tablo belirli yönetim işlemlerini iptal etme ve genel toplam süreleri özetler:

Kategori  |İşlem  |İptal edilebilir  |Tahmini iptal süresi  |
|---------|---------|---------|---------|
|Dağıtım |Örnek oluşturma |No |  |
|Güncelleştirme |Örnek depolama ölçeği artırma/azaltma (Genel Amaçlı) |No |  |
|Güncelleştirme |Örnek depolama ölçeği artırma/azaltma (İş Açısından Kritik) |Yes |İşlem %90, 5 dakika içinde tamamlanır. |
|Güncelleştirme |Örnek işlem (Vçekirdekler) ölçeği artırma ve azaltma (Genel Amaçlı) |Yes |İşlem %90, 5 dakika içinde tamamlanır. |
|Güncelleştirme |Örnek işlem (Vçekirdekler) ölçeği artırma ve azaltma (İş Açısından Kritik) |Yes |İşlem %90, 5 dakika içinde tamamlanır. |
|Güncelleştirme |Örnek hizmeti katmanı değişikliği (İş Açısından Kritik Genel Amaçlı ve tam tersi) |Yes |İşlem %90, 5 dakika içinde tamamlanır. |
|Sil |Örnek silme |No |  |
|Sil |Sanal küme silme (Kullanıcı tarafından başlatılan işlem olarak) |No |  |

# <a name="portal"></a>[Portal](#tab/azure-portal)

Yönetim işlemini iptal etmek için genel bakış dikey penceresine gidin ve devam eden işlemin bildirim kutusu ' na tıklayın. Sağ taraftan, devam eden işlem içeren bir ekran görünür ve işlemi iptal etmek için bir düğme olacaktır. İlk tıkladıktan sonra yeniden tıklamalısınız ve işlemi iptal etmek istediğinizi onaylamanız istenir.

[![İşlemi iptal et](./media/management-operations-overview/canceling-operation.png)](./media/management-operations-overview/canceling-operation.png#lightbox)

İptal isteği gönderildikten ve işlendikten sonra, gönderme işlemi başarılı olduysa veya yoksa bir bildirim alırsınız.

Gönderilen iptal isteği başarılı olursa, yönetim işlemi birkaç dakika içinde iptal edilir ve hata oluşur.

![İşlem sonucu iptal ediliyor](./media/management-operations-overview/canceling-operation-result.png)

İptal isteği başarısız olursa veya iptal düğmesi etkin değilse, yönetim işleminin iptal edilemez durumuna girdiği ve birkaç dakika içinde tamamlayacağı anlamına gelir. Yönetim işlemi tamamlanana kadar yürütmeye devam eder.

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Zaten yüklü Azure PowerShell yoksa, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps).

Yönetim işlemini iptal etmek için, yönetim işlemi adı belirtmeniz gerekir. Bu nedenle, önce işlem listesini almak için Al komutunu kullanın ve ardından belirli bir işlemi iptal edin.

```powershell-interactive
$managedInstance = "yourInstanceName"
$resourceGroup = "yourResourceGroupName"

$managementOperations = Get-AzSqlInstanceOperation -ManagedInstanceName $managedInstance  -ResourceGroupName $resourceGroup

foreach ($mo in $managementOperations ) {
    if($mo.State -eq "InProgress" -and $mo.IsCancellable){
        $cancelRequest = Stop-AzSqlInstanceOperation -ResourceGroupName $resourceGroup -ManagedInstanceName $managedInstance -Name $mo.Name
        Get-AzSqlInstanceOperation -ManagedInstanceName $managedInstance  -ResourceGroupName $resourceGroup -Name $mo.Name
    }
}
```

Ayrıntılı komutların açıklaması için bkz. [Get-Azsqlınstanceoperation](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlinstanceoperation) ve [stop-azsqlınstanceoperation](https://docs.microsoft.com/powershell/module/az.sql/stop-azsqlinstanceoperation).

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Azure CLı yüklü değilse, bkz. [Azure CLI 'Yı yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest).

Yönetim işlemini iptal etmek için, yönetim işlemi adı belirtmeniz gerekir. Bu nedenle, önce işlem listesini almak için Al komutunu kullanın ve ardından belirli bir işlemi iptal edin.

```azurecli-interactive
az sql mi op list -g yourResourceGroupName --mi yourInstanceName --query "[?state=='InProgress' && isCancellable].{Name: name}" -o tsv |
while read -r operationName; do
    az sql mi op cancel -g yourResourceGroupName --mi yourInstanceName -n $operationName
done
```

Ayrıntılı komutların açıklaması için bkz. [az SQL mı op](https://docs.microsoft.com/cli/azure/sql/mi/op?view=azure-cli-latest).

---

## <a name="next-steps"></a>Sonraki adımlar
- İlk yönetilen örneğinizi oluşturmayı öğrenmek için bkz. [Hızlı Başlangıç Kılavuzu](instance-create-quickstart.md).
- Özellikler ve karşılaştırma listesi için bkz. [SQL ortak özellikleri](../database/features-comparison.md).
- VNet yapılandırması hakkında daha fazla bilgi için bkz. [SQL yönetilen örnek VNET yapılandırması](connectivity-architecture-overview.md).
- Yönetilen bir örnek oluşturan ve bir veritabanını bir yedekleme dosyasından geri yükleyen hızlı başlangıç için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- Geçiş için Azure veritabanı geçiş hizmeti 'ni kullanma hakkında bir öğretici için bkz. [veritabanı geçiş hizmeti kullanılarak SQL yönetilen örnek geçişi](../../dms/tutorial-sql-server-to-managed-instance.md).
