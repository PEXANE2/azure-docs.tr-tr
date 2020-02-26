---
title: Azure İlkesi’ni Kullanarak VM Oluştururken Yedeklemeyi Otomatik Olarak Etkinleştirme
description: Belirli bir kapsamda oluşturulan tüm VM 'Lerde yedeklemeyi otomatik olarak etkinleştirmek için Azure Ilkesi 'ni nasıl kullanacağınızı açıklayan bir makale
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 1d423371788f87155328e8f5c9334e47349a68e8
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77584277"
---
# <a name="auto-enable-backup-on-vm-creation-using-azure-policy"></a>Azure İlkesi’ni Kullanarak VM Oluştururken Yedeklemeyi Otomatik Olarak Etkinleştirme

Bir kuruluştaki bir yedeklemenin veya uyumluluk yöneticisinin temel sorumluluklarıyla biri, iş açısından kritik tüm makinelerin uygun saklama ile yedeklenmesini sağlamaktır.

Bugün Azure Backup, **bir abonelik veya kaynak grubu içindeki belirli bir konumdaki tüm Azure VM**'lerine atanabilecek yerleşik bir Ilke (Azure ilkesi kullanarak) sağlar. Bu ilke belirli bir kapsama atandığında, bu kapsamda oluşturulan tüm yeni VM 'Ler, **aynı konum ve abonelik içindeki mevcut bir kasaya**yedekleme için otomatik olarak yapılandırılır. Kullanıcı, yedeklenen sanal makinelerin ilişkilendirilmesi gereken kasayı ve bekletme ilkesini belirtebilir.

## <a name="supported-scenarios"></a>Desteklenen Senaryolar

* Yerleşik ilke şu anda yalnızca Azure VM 'Leri için desteklenmektedir. Kullanıcılar, atama sırasında belirtilen bekletme ilkesinin bir VM bekletme ilkesi olduğundan emin olunması gerekir. Bu ilke tarafından desteklenen tüm VM SKU 'Larını görmek için [Bu](https://docs.microsoft.com/azure/backup/backup-azure-policy-supported-skus) belgeye başvurun.

* İlke her seferinde tek bir konuma ve aboneliğe atanabilir. Konumlar ve abonelikler arasında VM 'Lere yönelik yedeklemeyi etkinleştirmek için, her bir konum ve abonelik birleşimi için bir tane olmak üzere, ilke atamasının birden çok örneğinin oluşturulması gerekir.

* Belirtilen kasa ve yedekleme için yapılandırılmış VM 'Ler farklı kaynak grupları altında olabilir.

* Yönetim grubu kapsamı şu anda desteklenmiyor.

* Yerleşik ilke şu anda ulusal bulutlarda kullanılamaz.

## <a name="using-the-built-in-policy"></a>Yerleşik ilkeyi kullanma

İlkeyi gerekli kapsama atamak için lütfen aşağıdaki adımları izleyin:

1. Azure portalında oturum açın ve **ilke** panosuna gidin.
2. Azure kaynakları genelinde tüm yerleşik ilkelerin listesini almak için sol menüdeki **tanımlar** ' ı seçin.
3. **Kategori = Yedekleme**listesini filtreleyin. Listenin, bir konumun VM 'lerde yedeklemeyi aynı konumda bulunan mevcut bir merkezi kasaya yapılandırın ' adlı tek bir ilkeye göre filtrelenmiş olduğunu görürsünüz.
![Ilkesi panosu](./media/backup-azure-auto-enable-backup/policy-dashboard.png)
4. İlkenin adına tıklayın. Bu ilkenin ayrıntılı tanımına yönlendirilirsiniz.
![Ilke tanımı dikey penceresi](./media/backup-azure-auto-enable-backup/policy-definition-blade.png)
5. Dikey pencerenin üst kısmındaki **ata** düğmesine tıklayın. Bu, sizi **Ilke ata** dikey penceresine yönlendirir.
6. **Temel bilgiler**altında, **kapsam** alanının yanındaki üç noktaya tıklayın. Bu, ilke için uygulanacak aboneliği seçebileceğiniz sağ bağlam dikey penceresini açar. Ayrıca, ilkenin yalnızca belirli bir kaynak grubundaki VM 'Ler için uygulanması için, isteğe bağlı olarak bir kaynak grubu seçebilirsiniz.
![Ilke atama temelleri](./media/backup-azure-auto-enable-backup/policy-assignment-basics.png)
7. **Parametreler** sekmesinde, açılan listeden bir konum seçin ve kapsamdaki VM 'lerin ilişkilendirilmesi gereken kasa ve yedekleme ilkesini seçin.
![Ilkesi atama parametreleri](./media/backup-azure-auto-enable-backup/policy-assignment-parameters.png)
8. **Efektin** deployIfNotExists olarak ayarlandığından emin olun.
9. **Gözden geçir + oluştur** ' a gidin ve **Oluştur**' a tıklayın.

> [!NOTE]
>
> Azure Ilkesi, [Düzeltme](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources)kullanılarak mevcut VM 'lerde de kullanılabilir.

> [!NOTE]
>
> Bu ilkenin aynı anda 200 ' den fazla VM 'ye atanması önerilir. İlke 200 ' den fazla VM 'ye atanırsa, bu, yedeklemenin zamanlama tarafından belirtilenden birkaç saat sonra tetiklendiğine neden olabilir.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure Ilkesi hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/policy/overview)
