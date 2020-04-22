---
title: Bileşik kaynakları kullanarak Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmaları oluşturma
description: Azure Otomasyon Durumu Yapılandırmasında bileşik kaynakları kullanarak yapılandırmaları nasıl oluşturabilirsiniz öğrenin.
keywords: powershell dsc, istenilen durum yapılandırması, powershell dsc azure, kompozit kaynaklar
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: 7e7625ae95d5355ae1122a16ea4828eed5f78c73
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682921"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-using-composite-resources"></a>Bileşik kaynakları kullanarak Azure Otomasyon Durumu Yapılandırmasında DSC yapılandırmaları oluşturma

Kaynağı tek bir durumdan daha fazla durum yapılandırmasıyla (DSC) yönetmeniz gerektiğinde, en iyi yol [bileşik kaynakları](/powershell/scripting/dsc/resources/authoringresourcecomposite)kullanmaktır. Bileşik kaynak, başka bir yapılandırma içinde DSC kaynağı olarak kullanılan iç içe ve parametreli yapılandırmadır. Bileşik kaynakların kullanımı, temel bileşik kaynakların tek tek yönetilmesine ve oluşturulmasına olanak sağlarken karmaşık yapılandırmalar oluşturmanıza olanak tanır.

Azure [Otomasyonu, bileşik kaynakların içe aktarılmasını ve derlenmesine](automation-dsc-compile.md)olanak tanır. Otomasyon hesabınıza bileşik kaynaklar aktardıktan sonra, Devlet Yapılandırması (Azure portalındaki **DSC özelliği)** aracılığıyla Azure Otomasyon Durumu Yapılandırmasını kullanabilirsiniz.

## <a name="composing-a-configuration-from-composite-resources"></a>Bileşik kaynaklardan yapılandırma oluşturma

Azure portalındaki bileşik kaynaklardan yapılmış bir yapılandırma atamadan önce yapılandırmayı oluşturmanız gerekir. Kompozisyon, **Yapılandırmalar** veya **Derlenmiş yapılandırmalar** sekmesinde yken Durum Yapılandırması (DSC) sayfasında **Yapılandırma** yapılandırmasını kullanır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon hesap sayfasında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** seçin.
1. Durum yapılandırması (DSC) sayfasında, **Yapılandırmalar** veya **Derlenmiş yapılandırmalar** sekmesini tıklatın ve ardından sayfanın üst kısmındaki menüde **yapılandırmayı oluştur'u** tıklatın.
1. Temel **Bilgiler** adımında, yeni yapılandırma adını (gerekli) girin ve yeni yapılandırmanıza eklemek istediğiniz her bileşik kaynağın satırında herhangi bir yere tıklayın, ardından **İleri'yi** tıklatın veya **Kaynak kodu** adımını tıklatın. Aşağıdaki adımlar için kaynakları `PSExecutionPolicy` `RenameAndDomainJoin` seçtik ve birleştirdik.
   ![Yapılandırma oluşturma sayfasının temel adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. **Kaynak kodu** adımı, seçili bileşik kaynakların oluşturulan yapılandırmasının nasıl göründüğünü gösterir. Tüm parametrelerin birleştirilmesini ve bunların bileşik kaynağa nasıl aktarıldıklarını görebilirsiniz. Yeni kaynak kodu gözden geçirmeyi bitirdiğinizde, **İleri'yi** tıklatın veya **Parametreler** adımını tıklatın.
   ![Yapılandırma sayfasının kaynak kodu adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. **Parametreler** adımında, değerler sağlanabilmesi için her bileşik kaynağın parametresi açığa çekilir. Bir parametrenin açıklaması varsa, parametre alanının yanında görüntülenir. Bir parametre türdeyse, `PSCredential` açılır bırakma, geçerli Otomasyon hesabındaki Kimlik **Bilgileri** nesnelerinin bir listesini sağlar. A **+ Kimlik bilgisi** seçeneği ekle seçeneği de mevcuttur. Gerekli tüm parametreler sağlandıktan sonra **Kaydet'i tıklatın ve derle'i tıklatın.**
   ![Yapılandırma sayfasının parametre adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

Yeni yapılandırma kaydedildikten sonra derleme için gönderilir. Derleme işinin durumu, içe aktarılan yapılandırmalar gibi görüntülenebilir. Daha fazla bilgi için [bkz.](automation-dsc-getting-started.md#viewing-a-compilation-job)

Derleme başarıyla tamamlandığında, yeni yapılandırma **Derlenen yapılandırmalar** sekmesinde görünür. Daha sonra farklı bir [düğüm yapılandırmasına düğüm atama](automation-dsc-getting-started.md#reassigning-a-node-to-a-different-node-configuration)adımlarını kullanarak yapılandırmayı yönetilen bir düğüme atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)bkz.
- Düğümlere nasıl bindirilir öğrenince Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen Yerleşik makinelere](automation-dsc-onboarding.md)bakın.
- DSC yapılandırmalarını hedef düğümlerine atayabilmeniz için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlets'e](/powershell/module/azurerm.automation/#automation)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
