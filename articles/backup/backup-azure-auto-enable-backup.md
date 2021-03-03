---
title: Azure İlkesi’ni Kullanarak VM Oluştururken Yedeklemeyi Otomatik Olarak Etkinleştirme
description: Belirli bir kapsamda oluşturulan tüm VM 'Lerde yedeklemeyi otomatik olarak etkinleştirmek için Azure Ilkesi 'ni nasıl kullanacağınızı açıklayan bir makale
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: dfa4364eeaa9f5b60af3f5d6a19aaeb188d4f65e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101707311"
---
# <a name="auto-enable-backup-on-vm-creation-using-azure-policy"></a>Azure İlkesi’ni Kullanarak VM Oluştururken Yedeklemeyi Otomatik Olarak Etkinleştirme

Bir kuruluştaki bir yedeklemenin veya uyumluluk yöneticisinin temel sorumluluklarıyla biri, iş açısından kritik tüm makinelerin uygun saklama ile yedeklenmesini sağlamaktır.

Günümüzde Azure Backup, Azure sanal makinelerinizin yedekleme için yapılandırılmasını otomatik olarak sağlamanıza yardımcı olmak üzere çeşitli yerleşik ilkeler ( [Azure ilkesi](../governance/policy/overview.md)kullanarak) sağlar. Yedekleme takımlarınızın ve kaynaklarınızın nasıl düzenlendiğini bağlı olarak, aşağıdaki ilkelerden birini kullanabilirsiniz:

## <a name="policy-1---configure-backup-on-vms-without-a-given-tag-to-an-existing-recovery-services-vault-in-the-same-location"></a>İlke 1-aynı konumdaki mevcut bir kurtarma hizmetleri kasasında verilen bir etiketi olmayan VM 'lerde yedeklemeyi yapılandırma

Kuruluşunuzda, uygulama ekiplerinde yedeklemeleri yöneten merkezi bir yedekleme takımı varsa, bu ilkeyi, yönetilen VM 'Lerle aynı abonelikte ve konumda bulunan mevcut bir merkezi kurtarma hizmetleri kasasında yedeklemeyi yapılandırmak için kullanabilirsiniz. Belirli bir etiketi içeren VM 'leri bu ilkenin kapsamından **hariç bırakmayı** seçebilirsiniz.

## <a name="policy-2---preview-configure-backup-on-vms-with-a-given-tag-to-an-existing-recovery-services-vault-in-the-same-location"></a>İlke 2-[Önizleme] belirli bir etikete sahip VM 'lerde yedeklemeyi aynı konumdaki mevcut bir kurtarma hizmetleri kasasına yapılandırma
Bu ilke, bu ilkenin kapsamında belirli bir etiketi içeren VM 'leri **dahil** etmek için bu ilkeyi kullanmanın tek farkı Ile yukarıdaki ilke 1 ile aynı şekilde çalışmaktadır. 

## <a name="policy-3---preview-configure-backup-on-vms-without-a-given-tag-to-a-new-recovery-services-vault-with-a-default-policy"></a>İlke 3-[Önizleme] belirtilen bir etiketi olmayan VM 'lerde yedeklemeyi varsayılan ilkeyle yeni bir kurtarma hizmetleri kasasına yapılandırın
Uygulamaları adanmış kaynak gruplarında organize ederseniz ve aynı kasa tarafından yedeklenmek istiyorsanız bu ilke, bu eylemi otomatik olarak yönetmenizi sağlar. Belirli bir etiketi içeren VM 'leri bu ilkenin kapsamından **hariç bırakmayı** seçebilirsiniz.

## <a name="policy-4---preview-configure-backup-on-vms-with-a-given-tag-to-a-new-recovery-services-vault-with-a-default-policy"></a>İlke 4-[Önizleme] belirli bir etikete sahip VM 'lerde yedeklemeyi varsayılan ilkeyle yeni bir kurtarma hizmetleri kasasına yapılandırma
Bu ilke, bu ilkenin kapsamında belirli bir etiketi içeren VM 'leri **dahil** etmek için bu ilkeyi kullanmanın tek farkı Ile yukarıdaki ilke 3 ile aynı şekilde çalışmaktadır. 

Yukarıdaki Azure Backup Ayrıca, [yalnızca bir denetim](../governance/policy/concepts/effects.md#audit) ilkesi sağlar- **Azure Backup sanal makineler için etkinleştirilmelidir**. Bu ilke, hangi sanal makinelerin yedeklenmediğini belirtir, ancak bu VM 'Ler için otomatik olarak yedeklemeleri yapılandırmaz. Bu, yalnızca VM 'lerin genel uyumluluğunu değerlendirmek ve hemen işlem yapmak için arama yapmak istediğinizde yararlıdır.

## <a name="supported-scenarios"></a>Desteklenen Senaryolar

* Yerleşik ilke şu anda yalnızca Azure VM 'Leri için desteklenmektedir. Kullanıcılar, atama sırasında belirtilen bekletme ilkesinin bir VM bekletme ilkesi olduğundan emin olunması gerekir. Bu ilke tarafından desteklenen tüm VM SKU 'Larını görmek için [Bu](./backup-azure-policy-supported-skus.md) belgeye başvurun.

* İlkeler 1 ve 2 tek seferde tek bir konuma ve aboneliğe atanabilir. Konumlar ve abonelikler arasında VM 'Lere yönelik yedeklemeyi etkinleştirmek için, her bir konum ve abonelik birleşimi için bir tane olmak üzere, ilke atamasının birden çok örneğinin oluşturulması gerekir.

* Ilkeler 1 ve 2 için yönetim grubu kapsamı şu anda desteklenmiyor.

* Ilkeler 1 ve 2 için, belirtilen kasa ve yedekleme için yapılandırılmış VM 'Ler farklı kaynak grupları altında olabilir.

* 1, 2, 3 ve 4 ilkeleri Şu anda ulusal bulutlarda kullanılamaz.

* İlkeler 3 ve 4 tek seferde tek bir aboneliğe atanabilir (veya bir abonelik içindeki bir kaynak grubu).

[!INCLUDE [backup-center.md](../../includes/backup-center.md)]

## <a name="using-the-built-in-policies"></a>Yerleşik ilkeleri kullanma

Aşağıdaki adımlarda, Ilke 1 ' i atamaya yönelik uçtan uca işlem açıklanır: verilen bir **etiketi olmayan VM 'lerde yedeklemeyi, belirli bir kapsama aynı konumdaki mevcut bir kurtarma hizmetleri kasasına yapılandırma** . Diğer ilkeler için de benzer yönergeler geçerlidir. Atandıktan sonra kapsamda oluşturulan tüm yeni VM 'ler yedekleme için otomatik olarak yapılandırılır.

1. Azure portal oturum açın ve **ilke** panosuna gidin.
2. Azure kaynakları genelinde tüm yerleşik ilkelerin listesini almak için sol menüdeki **tanımlar** ' ı seçin.
3. **Category = Backup** listesini filtreleyin ve ' bir konumun VM 'lerde yedeklemeyi aynı konumda bulunan mevcut bir merkezi kasada yapılandırın ' adlı ilkeyi seçin.
![İlke panosu](./media/backup-azure-auto-enable-backup/policy-dashboard.png)
4. İlkenin adını seçin. Bu ilkenin ayrıntılı tanımına yönlendirilirsiniz.
![İlke tanımı bölmesi](./media/backup-azure-auto-enable-backup/policy-definition-blade.png)
5. Bölmenin üst kısmındaki **ata** düğmesini seçin. Bu, sizi **Ilke ata** bölmesine yönlendirir.
6. **Temel bilgiler** altında, **kapsam** alanının yanındaki üç noktayı seçin. Bu, ilke için uygulanacak aboneliği seçebileceğiniz bir sağ bağlam bölmesi açar. Ayrıca, ilkenin yalnızca belirli bir kaynak grubundaki VM 'Ler için uygulanması için, isteğe bağlı olarak bir kaynak grubu seçebilirsiniz.
![İlke atama temelleri](./media/backup-azure-auto-enable-backup/policy-assignment-basics.png)
7. **Parametreler** sekmesinde, açılan listeden bir konum seçin ve kapsamdaki VM 'lerin ilişkilendirilmesi gereken kasa ve yedekleme ilkesini seçin. Bir etiket adı ve bir etiket değerleri dizisi belirtmeyi de tercih edebilirsiniz. Verilen etiket için belirtilen değerlerden herhangi birini içeren bir VM, ilke atamasının kapsamından dışlanır.
![İlke atama parametreleri](./media/backup-azure-auto-enable-backup/policy-assignment-parameters.png)
8. **Efektin** deployIfNotExists olarak ayarlandığından emin olun.
9. **Gözden geçir + oluştur** ' a gidin ve **Oluştur**' u seçin.

> [!NOTE]
>
> Azure Ilkesi, [Düzeltme](../governance/policy/how-to/remediate-resources.md)kullanılarak mevcut VM 'lerde de kullanılabilir.

> [!NOTE]
>
> Bu ilkenin aynı anda 200 ' den fazla VM 'ye atanması önerilir. İlke 200 ' den fazla VM 'ye atanırsa bu, yedeklemenin zamanlama tarafından belirtilenden birkaç saat sonra tetiklenmesi ile sonuçlanabilir.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Ilkesi hakkında daha fazla bilgi edinin](../governance/policy/overview.md)