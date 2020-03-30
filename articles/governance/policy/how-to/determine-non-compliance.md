---
title: Uyumsuzluk nedenlerini belirleme
description: Bir kaynak uyumlu olmadığında, birçok olası nedeni vardır. Uyumsuzluğa neyin yol açtığını öğrenin.
ms.date: 04/26/2019
ms.topic: how-to
ms.openlocfilehash: c931831ddf3cc727b9861e75969eac3bf00c9e45
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264641"
---
# <a name="determine-causes-of-non-compliance"></a>Uyumsuzluk nedenlerini belirleme

Bir Azure kaynağı bir ilke kuralına uygun olmadığı belirlendiğinde, kaynağın kuralın hangi bölümüyle uyumlu olmadığını anlamak yararlı olur. Hangi değişikliğin daha önce uyumlu olmayan bir kaynağı değiştirdiğini anlamak da yararlıdır. Bu bilgileri bulmanın iki yolu vardır:

> [!div class="checklist"]
> - [Uyumluluk ayrıntıları](#compliance-details)
> - [Geçmişi değiştirme (Önizleme)](#change-history)

## <a name="compliance-details"></a>Uyumluluk ayrıntıları

Bir kaynak uyumlu olmadığında, söz le ilgili kaynağın uyumluluk ayrıntıları **İlke uyumluluk** sayfasından kullanılabilir. Uyumluluk ayrıntıları bölmesi aşağıdaki bilgileri içerir:

- Ad, tür, konum ve kaynak kimliği gibi kaynak ayrıntıları
- Geçerli ilke ataması için son değerlendirmenin uyumluluk durumu ve zaman damgası
- Kaynak uyumsuzluğunun _nedenleri_ listesi

> [!IMPORTANT]
> _Uyumlu olmayan_ bir kaynağın uyumluluk ayrıntıları, söz sahibi kaynaktaki özelliklerin geçerli değerini gösterdiğinden, kullanıcının kaynak **türüne** işlemi **okumuş** olması gerekir. Örneğin, uyumlu _olmayan_ kaynak **Microsoft.Compute/virtualMachines** ise, kullanıcının **Microsoft.Compute/virtualMachines/read** işlemine sahip olması gerekir. Kullanıcı gerekli işlem yoksa, bir erişim hatası görüntülenir.

Uyumluluk ayrıntılarını görüntülemek için aşağıdaki adımları izleyin:

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Genel **Bakış** veya **Uyumluluk** sayfasında, _uyumlu olmayan_bir **uyumluluk durumunda** bir ilke seçin.

1. **İlke uyumluluk** sayfasının **Kaynak uyumluluk** sekmesi altında, _uyumlu olmayan_bir uyumluluk **durumunda** bir kaynağın elipslerini sağ tıklatın veya seçin. Ardından **uyumluluk ayrıntılarını görüntüle'yi**seçin.

   ![Uyumluluk ayrıntılarını görüntüleme seçeneğini görüntüleme](../media/determine-non-compliance/view-compliance-details.png)

1. **Uyumluluk ayrıntıları** bölmesi, kaynağın en son değerlendirmesinden geçerli ilke atamasına kadar olan bilgileri görüntüler. Bu örnekte, **microsoft.sql/servers/version** alanı _12.0_ iken ilke tanımı _14.0_bekleniyor. Kaynak birden çok nedenden dolayı uyumlu değilse, her biri bu bölmede listelenir.

   ![Uyumluluk ayrıntıları bölmesi ve uyumsuzluk nedenleri](../media/determine-non-compliance/compliance-details-pane.png)

   **DenetimifNotExists** veya **deployIfNotExists** ilke tanımı için ayrıntılar **ayrıntılar.type** özelliğini ve isteğe bağlı özellikleri içerir. Bir liste için [auditIfNotExists özelliklerine](../concepts/effects.md#auditifnotexists-properties) bakın ve [dağıtımıIfNotExists özellikleri.](../concepts/effects.md#deployifnotexists-properties) **Son değerlendirilen kaynak,** tanımın **ayrıntılar** bölümünden ilgili bir kaynaktır.

   Örnek kısmi **dağıtımIfNotExists** tanımı:

   ```json
   {
       "if": {
           "field": "type",
           "equals": "[parameters('resourceType')]"
       },
       "then": {
           "effect": "DeployIfNotExists",
           "details": {
               "type": "Microsoft.Insights/metricAlerts",
               "existenceCondition": {
                   "field": "name",
                   "equals": "[concat(parameters('alertNamePrefix'), '-', resourcegroup().name, '-', field('name'))]"
               },
               "existenceScope": "subscription",
               "deployment": {
                   ...
               }
           }
       }
   }
   ```

   ![Uyumluluk ayrıntıları bölmesi - *ifNotExists](../media/determine-non-compliance/compliance-details-pane-existence.png)

> [!NOTE]
> Verileri korumak için, özellik değeri _gizli_ olduğunda geçerli değer yıldız yıldızlarını görüntüler.

Bu ayrıntılar, kaynağın neden şu anda uyumlu olmadığını açıklar, ancak kaynağın uyumlu olmamasına neden olan kaynakta ne zaman değişiklik yapıldığını göstermez. Bu bilgiler için aşağıdaki [Geçmiş Değiştir 'e (Önizleme)](#change-history) bakın.

### <a name="compliance-reasons"></a>Uyumluluk nedenleri

Aşağıdaki matris, ilke tanımındaki sorumlu [koşula](../concepts/definition-structure.md#conditions) her olası _nedeni_ eşler:

|Neden | Koşul |
|-|-|
|Geçerli değer, anahtar olarak hedef değeri içermelidir. |içerirKey veya **notContainsKey** |
|Geçerli değer hedef değeri içermelidir. |içerir veya **içermezİçeriği** |
|Geçerli değer hedef değere eşit olmalıdır. |eşittir veya **eşit değilEşittir** |
|Geçerli değer hedef değerden daha az olmalıdır. |daha az veya **daha** büyük değilOrEquals |
|Geçerli değer, hedef değerden büyük veya eşit olmalıdır. |greaterOrEquals veya daha az **değil** |
|Geçerli değer hedef değerden büyük olmalıdır. |büyük **veya** küçük OrEquals |
|Geçerli değer, hedef değerden daha az veya eşit olmalıdır. |lessOrEquals veya daha büyük **değil** |
|Geçerli değer olmalıdır. |Var |
|Geçerli değer hedef değerde olmalıdır. |içinde veya **değilIn** |
|Geçerli değer hedef değer gibi olmalıdır. |gibi veya **değilLike** |
|Geçerli değer, büyük/küçük harf duyarlı hedef değeri eşleştirmelidir. |maç veya **notMatch** |
|Geçerli değer, büyük/küçük harf duyarsızı hedef değerle eşleşmelidir. |matchInsensitively **not** veya değilMatchInsensitively |
|Geçerli değer anahtar olarak hedef değeri içermemelidir. |notContainsKey veya **içermezKey** içerir|
|Geçerli değer hedef değeri içermemelidir. |notİçer veya **içermez** |
|Geçerli değer hedef değere eşit olmamalıdır. |notEquals veya **eşittir** |
|Geçerli değer bulunmamalıdır. |**yok**  |
|Geçerli değer hedef değerde olmamalıdır. |notIn veya **değil** |
|Geçerli değer hedef değer gibi olmamalıdır. |notLike veya **gibi değil** |
|Geçerli değer, büyük/küçük harf duyarlı hedef değeriyle eşleşmemelidir. |notMatch veya **match değil** |
|Geçerli değer, büyük/küçük harf duyarsızlığı hedef değerle eşleşmemelidir. |notMatchDuyarsız ya da matchinsensitively **değil** |
|İlgili kaynaklar ilke tanımındaki efekt ayrıntılarıyla eşleşmiyor. |O zaman tanımlanan tür bir **kaynak.details.type** ve ilke kuralının **if** bölümünde tanımlanan kaynak ile ilgili. |

## <a name="compliance-details-for-guest-configuration"></a>Konuk Yapılandırması için uyumluluk ayrıntıları

_DenetimIfNotExists_ ilkeleri Konuk _Yapılandırma_ kategorisinde, VM içinde değerlendirilen birden çok ayar olabilir ve ayar başına ayrıntıları görüntülemeniz gerekir. Örneğin, parola ilkeleri listesi için denetim yapıyorsunuz ve bunlardan yalnızca birinin _durumu uyumlu değilse,_ hangi belirli parola ilkelerinin uyumsuz olduğunu ve neden uyumlu olmadığını bilmeniz gerekir.

Ayrıca doğrudan VM oturum açma erişiminiz olmayabilir, ancak VM'nin neden _uyumsuz_olduğunu bildirmeniz gerekir.

### <a name="azure-portal"></a>Azure portalında

İlke uyumluluk ayrıntılarını görüntülemek için yukarıdaki bölümde aynı adımları izleyerek başlayın.

Uyumluluk **ayrıntıları** bölmesi görünümünde son **değerlendirilen kaynak**bağlantısını tıklatın.

   ![Denetimi görüntüleIfNotExists tanım ayrıntılarını](../media/determine-non-compliance/guestconfig-auditifnotexists-compliance.png)

**Konuk Atama** sayfası, kullanılabilir tüm uyumluluk ayrıntılarını görüntüler. Görünümdeki her satır, makine içinde gerçekleştirilen bir değerlendirmeyi temsil eder. **Neden** sütununda, Konuk Atama'nın neden _uyumsuz_ olduğunu açıklayan bir ifade gösterilir. Örneğin, parola ilkelerini denetliyorsanız, **Neden** sütunu her ayar için geçerli değeri içeren metni görüntüler.

![Uyumluluk ayrıntılarını görüntüleme](../media/determine-non-compliance/guestconfig-compliance-details.png)

### <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell'den uyumluluk ayrıntılarını da görüntüleyebilirsiniz. İlk olarak, Konuk Yapılandırma modüllerini yüklediğinizden emin olun.

```azurepowershell-interactive
Install-Module Az.GuestConfiguration
```

Bir VM için tüm Konuk Atamaları'nın geçerli durumunu aşağıdaki komutu kullanarak görüntüleyebilirsiniz:

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname>
```

```output
PolicyDisplayName                                                         ComplianceReasons
-----------------                                                         -----------------
Audit that an application is installed inside Windows VMs                 {[InstalledApplication]bwhitelistedapp}
Audit that an application is not installed inside Windows VMs.            {[InstalledApplication]NotInstalledApplica...
```

Yalnızca VM'nin neden _uyumlu olmadığını_açıklayan _neden_ ifadesini görüntülemek için yalnızca Neden alt özelliğini döndürün.

```azurepowershell-interactive
Get-AzVMGuestPolicyReport -ResourceGroupName <resourcegroupname> -VMName <vmname> | % ComplianceReasons | % Reasons | % Reason
```

```output
The following applications are not installed: '<name>'.
```

Ayrıca, makine kapsamında konuk atamaları için bir uyumluluk geçmişi çıktısı verebilirsiniz. Bu komutun çıktısı VM için her raporun ayrıntılarını içerir.

> [!NOTE]
> Çıktı büyük miktarda veri döndürebilir. Çıktıyı bir değişkende depolamak önerilir.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname>
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
[Preview]: Audit that an application is installed inside Windows VMs      NonCompliant                       02/10/2019 12:00:38 PM 02/10/2019 12:00:41 PM VM01  ../17fg0...
<truncated>
```

Bu görünümü basitleştirmek **için, Değiştirilen Göster** parametresini kullanın. Bu komutun çıktısı yalnızca uyumluluk durumundaki değişikliği izleyen raporları içerir.

```azurepowershell-interactive
$guestHistory = Get-AzVMGuestPolicyStatusHistory -ResourceGroupName <resourcegroupname> -VMName <vmname> -ShowChanged
$guestHistory
```

```output
PolicyDisplayName                                                         ComplianceStatus ComplianceReasons StartTime              EndTime                VMName LatestRepor
                                                                                                                                                                  tId
-----------------                                                         ---------------- ----------------- ---------              -------                ------ -----------
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/10/2019 10:00:38 PM 02/10/2019 10:00:41 PM VM01  ../12ab0...
Audit that an application is installed inside Windows VMs.                Compliant                          02/09/2019 11:00:38 AM 02/09/2019 11:00:39 AM VM01  ../e3665...
Audit that an application is installed inside Windows VMs                 NonCompliant                       02/09/2019 09:00:20 AM 02/09/2019 09:00:23 AM VM01  ../15ze1...
```

## <a name="change-history-preview"></a><a name="change-history"/>Geçmişi değiştirme (Önizleme)

Yeni bir **genel önizlemenin**bir parçası olarak, değişiklik geçmişinin son 14 [günü, tam mod silmeişlemini](../../../azure-resource-manager/templates/complete-mode-deletion.md)destekleyen tüm Azure kaynakları için kullanılabilir. Değişiklik geçmişi, bir değişikliğin ne zaman algılandığını ve her değişiklik için görsel bir _diff_ hakkında ayrıntılı bilgi sağlar. Kaynak Yöneticisi özellikleri eklendiğinde, kaldırıldığında veya değiştirildiğinde bir değişiklik algılaması tetiklenir.

1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

1. Genel **Bakış** veya **Uyumluluk** sayfasında, herhangi bir **uyumluluk durumunda**bir ilke seçin.

1. **İlke uyumluluk** sayfasının **Kaynak uyumluluk** sekmesialtında bir kaynak seçin.

1. **Kaynak Uyumluluğu** sayfasında **Geçmişdeğiştir (önizleme)** sekmesini seçin. Varsa algılanan değişikliklerin listesi görüntülenir.

   ![Kaynak Uyumluluğu sayfasında Azure İlke Değişikliği Geçmişi sekmesi](../media/determine-non-compliance/change-history-tab.png)

1. Algılanan değişikliklerden birini seçin. Kaynağın _görsel_ **diff'i Geçmiş değiştir** sayfasında sunulur.

   ![Azure İlke Değişikliği Geçmişi Görsel Diff Değiştir geçmişi sayfasında](../media/determine-non-compliance/change-history-visual-diff.png)

Bir kaynak değişikliklerini tanımlamada _görsel diff_ yardımcıları. Algılanan değişiklikler kaynağın geçerli uyumluluk durumuyla ilgili olmayabilir.

Geçmiş verilerini [değiştir, Azure Kaynak Grafiği](../../resource-graph/overview.md)tarafından sağlanır. Bu bilgileri Azure portalının dışında sorgulamak için [kaynak değişikliklerini al'a](../../resource-graph/how-to/get-resource-changes.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- Uyumluluk verilerini nasıl [alacağınızı](get-compliance-data.md)öğrenin.
- [Uyumlu olmayan kaynakları](remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.
