---
title: Azure İlkesi’ni Kullanarak VM Oluştururken Yedeklemeyi Otomatik Olarak Etkinleştirme
description: Belirli bir kapsamda oluşturulan tüm VM'ler için yedeklemeyi otomatik olarak etkinleştirmek için Azure İlkesi'nin nasıl kullanılacağını açıklayan bir makale
ms.topic: conceptual
ms.date: 11/08/2019
ms.openlocfilehash: 1d423371788f87155328e8f5c9334e47349a68e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77584277"
---
# <a name="auto-enable-backup-on-vm-creation-using-azure-policy"></a>Azure İlkesi’ni Kullanarak VM Oluştururken Yedeklemeyi Otomatik Olarak Etkinleştirme

Bir kuruluştaki Yedekleme veya Uyumluluk Yöneticisi'nin temel sorumluluklarından biri, iş açısından kritik olan tüm makinelerin uygun bekletmeyle yedeklenmelerini sağlamaktır.

Bugün, Azure Yedekleme, bir abonelik veya kaynak grubu içinde **belirli bir konumda ki tüm Azure VM'lerine**atanabilen yerleşik bir ilke (Azure İlkesi kullanarak) sağlar. Bu ilke belirli bir kapsama atandığında, bu kapsamda oluşturulan tüm yeni VM'ler otomatik olarak **aynı konum ve abonelikteki varolan**bir kasaya yedekleme için yapılandırılır. Kullanıcı, yedeklenen VM'lerin ilişkilendirilmesi gereken kasa ve bekletme ilkesini belirtebilir.

## <a name="supported-scenarios"></a>Desteklenen Senaryolar

* Yerleşik ilke şu anda yalnızca Azure VM'leri için desteklenir. Kullanıcılar, atama sırasında belirtilen bekletme ilkesinin bir VM saklama ilkesi olduğundan emin olmak için dikkatli olmalıdır. Bu politika tarafından desteklenen tüm VM SK'leri görmek için [bu](https://docs.microsoft.com/azure/backup/backup-azure-policy-supported-skus) belgeye bakın.

* İlke, aynı anda tek bir konuma ve aboneye atanabilir. Konumlar ve abonelikler arasında VM'ler için yedeklemeyi etkinleştirmek için, ilke atamasının birden çok örneğinin, konum ve aboneliğin her birleşimi için bir tane oluşturulması gerekir.

* Belirtilen kasa ve yedekleme için yapılandırılan VM'ler farklı kaynak grupları altında olabilir.

* Yönetim Grubu kapsamı şu anda desteklenmiş.

* Yerleşik ilke şu anda ulusal bulutlarda kullanılamıyor.

## <a name="using-the-built-in-policy"></a>Yerleşik ilkeyi kullanma

İlkeyi gerekli kapsama atamak için lütfen aşağıdaki adımları izleyin:

1. Azure Portalı'nda oturum açın ve **İlke** Panosu'na gidin.
2. Azure Kaynakları'ndaki tüm yerleşik ilkelerin listesini almak için sol menüdeki **Tanımlar'ı** seçin.
3. **Kategori=Yedekleme**için listeye filtre uygulayın. Listenin 'Bir yerin VM'lerinde yedeklemeyi aynı konumdaki mevcut bir merkezi Vault'a yapılandır' adlı tek bir ilke yle filtrelenmiş olarak görürsünüz.
![İlke Panosu](./media/backup-azure-auto-enable-backup/policy-dashboard.png)
4. İlkenin adını tıklatın. Bu ilke için ayrıntılı tanıma yönlendirilirsiniz.
![İlke Tanımı Blade](./media/backup-azure-auto-enable-backup/policy-definition-blade.png)
5. Bıçağın üst kısmındaki **Atla** düğmesine tıklayın. Bu, sizi Atama **İlkesi** bıçağına yönlendirir.
6. **Temel bilgiler**altında, **Kapsam** alanının yanındaki üç noktanın üzerine tıklayın. Bu, uygulanacak ilke için aboneliği seçebileceğiniz doğru bir bağlam bıçağı açar. Ayrıca isteğe bağlı olarak bir kaynak grubu seçebilirsiniz, böylece ilke yalnızca belirli bir kaynak grubundaki VM'ler için uygulanır.
![İlke Atama Temelleri](./media/backup-azure-auto-enable-backup/policy-assignment-basics.png)
7. **Parametreler** sekmesinde, açılır noktadan bir konum seçin ve kapsamdaki VM'lerin ilişkilendirilmesi gereken kasa ve yedekleme ilkesini seçin.
![İlke Atama Parametreleri](./media/backup-azure-auto-enable-backup/policy-assignment-parameters.png)
8. **Efektin** dağıtımifNotExists olarak ayarlandığından emin olun.
9. Gözden **Geçir+oluştur'a** gidin ve **Oluştur'u**tıklatın.

> [!NOTE]
>
> Azure İlkesi, düzeltme [yi](https://docs.microsoft.com/azure/governance/policy/how-to/remediate-resources)kullanarak varolan VM'lerde de kullanılabilir.

> [!NOTE]
>
> Bu ilkenin aynı anda 200'den fazla VM'ye atanmamış olması önerilir. İlke 200'den fazla VM'ye atanmışsa, yedeklemenin zamanlamada belirtilenden birkaç saat sonra tetiklenebilmiştir.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure İlkesi hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/governance/policy/overview)
