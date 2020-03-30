---
title: Kompozit Kaynakları Kullanarak Azure Otomasyon Durumu Yapılandırmasında (DSC) DSC Yapılandırmaları Oluşturma
description: Azure Otomasyon Durumu Yapılandırması'nda (DSC) bileşik kaynakları kullanarak yapılandırmaları nasıl oluşturabilirsiniz öğrenin
keywords: powershell dsc, istenilen durum yapılandırması, powershell dsc azure, kompozit kaynaklar
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: e5083ec55ee0a57cd7defd466f5baf1704336320
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77370658"
---
# <a name="composing-dsc-configurations-in-azure-automation-state-configuration-dsc-using-composite-resources"></a>Kompozit Kaynakları Kullanarak Azure Otomasyon Durumu Yapılandırmasında (DSC) DSC Yapılandırmaları Oluşturma

Bir kaynağın istenilen tek bir durum yapılandırması (DSC) yapılandırmasıyla yönetilmesi gerektiğinde, en iyi yol [bileşik kaynakları](/powershell/scripting/dsc/resources/authoringresourcecomposite)kullanmaktır. Bileşik kaynak, başka bir yapılandırma içinde DSC kaynağı olarak kullanılan iç içe ve parametreli yapılandırmadır. Bu, temel bileşik kaynakların (parametrelendirilmiş yapılandırmalar) tek tek yönetilmesine ve oluşturulmasına olanak sağlarken karmaşık yapılandırmaların oluşturulmasına olanak tanır.

Azure [Otomasyonu, bileşik kaynakların içe aktarılmasını ve derlenmesine](automation-dsc-compile.md)olanak tanır.
Bileşik kaynaklar Otomasyon hesabınıza aktarıldıktan sonra, **Durum Yapılandırması (DSC)** **sayfasındaki Yapılandırma Yıkıntış** deneyiminden kullanabilirsiniz.

## <a name="composing-a-configuration-from-composite-resources"></a>Bileşik kaynaklardan yapılandırma oluşturma

Azure portalındaki bileşik kaynaklardan yapılmış bir yapılandırma atamadan önce yapılandırmayı oluşturmanız gerekir. Bu, **Yapılandırmalar** veya **Derlenmiş yapılandırmalar** sekmelerinde yken **Durum Yapılandırması (DSC)** **sayfasındaki Yapılandırmayı Oluştur** kullanılarak yapılabilir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Solda, Tüm **kaynakları** ve ardından Otomasyon hesabınızın adını tıklatın.
1. Otomasyon **hesap** sayfasında, **Yapılandırma Yönetimi**altında **Durum yapılandırmasını (DSC)** seçin.
1. Durum **yapılandırması (DSC)** sayfasında, **Yapılandırmalar** veya **Derlenmiş yapılandırmalar** sekmesini tıklatın ve ardından sayfanın üst kısmındaki menüde **yapılandırmayı oluştur'u** tıklatın.
1. Temel **Bilgiler** adımında, yeni yapılandırma adını (gerekli) girin ve yeni yapılandırmanıza eklemek istediğiniz her bileşik kaynağın satırında herhangi bir yere tıklayın, ardından **İleri'yi** tıklatın veya **Kaynak kodu** adımını tıklatın. Aşağıdaki adımlar için **PSExecutionPolicy** ve **RenameAndDomainJoin** bileşik kaynaklarını seçtik.
   ![Yapılandırma oluşturma sayfasının temel adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. **Kaynak kodu** adımı, seçili bileşik kaynakların oluşturulan yapılandırmasının nasıl göründüğünü gösterir. Tüm parametrelerin birleştirilmesini ve bunların bileşik kaynağa nasıl aktarıldıklarını görebilirsiniz. Yeni kaynak kodu gözden geçirmeyi bitirdiğinizde, **İleri'yi** tıklatın veya **Parametreler** adımını tıklatın.
   ![Yapılandırma sayfasının kaynak kodu adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. **Parametreler** adımında, her bileşik kaynağın sahip olduğu parametre, sağlanabilmesi için açığa çıkarır. Bir parametrenin açıklaması varsa, parametre alanının yanında görüntülenir. Bir alan **PSCredential** türü parametresi ise, yapılandırmak için açılan açılır, geçerli Otomasyon hesabında **ki Kimlik Bilgileri** nesnelerinin bir listesini sağlar. A **+ Kimlik bilgisi** seçeneği ekle seçeneği de mevcuttur. Gerekli tüm parametreler sağlandıktan sonra **Kaydet'i tıklatın ve derle'i tıklatın.**
   ![Yapılandırma sayfasının parametre adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

Yeni yapılandırma kaydedildikten sonra derleme için gönderilir. Derleme işinin durumu, içe aktarılan yapılandırmalar gibi görüntülenebilir. Daha fazla bilgi için [bkz.](automation-dsc-getting-started.md#viewing-a-compilation-job)

Derleme başarıyla tamamlandığında, yeni yapılandırma **Derlenen yapılandırmalar** sekmesinde görünür. Bu sekmede görünür olduktan sonra, farklı bir [düğüm yapılandırmasına düğüm atama](automation-dsc-getting-started.md#reassigning-a-node-to-a-different-node-configuration)adımlarını kullanarak yönetilen bir düğüme atanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)
- Düğümlere nasıl bindirilir öğrenince Azure [Otomasyon Durumu Yapılandırması ile yönetim için Onboarding makineleri](automation-dsc-onboarding.md) bölümüne bakın
- DSC yapılandırmalarını hedef düğümlerine atamak için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlet'ine](/powershell/module/azurerm.automation/#automation) bakın
- Fiyatlandırma bilgileri için Bkz. [Azure Otomasyon Durumu Yapılandırma fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
