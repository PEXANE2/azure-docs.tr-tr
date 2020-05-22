---
title: DSC yapılandırmaları oluşturma
description: Bu makalede, Azure Otomasyonu durum yapılandırması 'nda bileşik kaynakları kullanarak yapılandırmaların nasıl oluşturulduğu açıklanmaktadır.
keywords: PowerShell DSC, istenen durum yapılandırması, PowerShell DSC Azure, bileşik kaynaklar
services: automation
ms.subservice: dsc
ms.date: 08/21/2018
ms.topic: conceptual
ms.openlocfilehash: 04e32a88dcfb9f069e51b2f18207155b95da37d6
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83744342"
---
# <a name="compose-dsc-configurations"></a>DSC yapılandırmaları oluşturma

Kaynağı tek bir istenen durum yapılandırması (DSC) ile yönetihtiyacınız olduğunda, en iyi yol [bileşik kaynakları](/powershell/scripting/dsc/resources/authoringresourcecomposite)kullanmaktır. Bileşik kaynak, başka bir yapılandırma içinde DSC kaynağı olarak kullanılan iç içe ve parametreli bir yapılandırmadır. Bileşik kaynakların kullanımı, temel alınan bileşik kaynakların tek tek yönetilmesine ve oluşturulmasına izin verirken karmaşık yapılandırma oluşturmanıza olanak sağlar.

Azure Otomasyonu [bileşik kaynakların içeri ve derlemesini](automation-dsc-compile.md)sunar. Otomasyon hesabınıza bileşik kaynakları içeri aktardıktan sonra, Azure portal **Durum Yapılandırması (DSC** özelliği aracılığıyla Azure Automation durum yapılandırması 'nı kullanabilirsiniz.

## <a name="compose-a-configuration"></a>Yapılandırma oluşturma

Azure portal bileşik kaynaklardan yapılan bir yapılandırmayı atamadan önce yapılandırmayı oluşturmanız gerekir. Birleşim, **yapılandırmalar** ya da **derlenmiş yapılandırmalar** SEKMESINDEN durum yapılandırması (DSC) sayfasında **oluşturma yapılandırması** ' nı kullanır.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Sol tarafta **tüm kaynaklar** ' a ve ardından Otomasyon hesabınızın adına tıklayın.
1. Otomasyon hesabı sayfasında, **yapılandırma yönetimi**altında **Durum Yapılandırması (DSC)** öğesini seçin.
1. Durum Yapılandırması (DSC) sayfasında, **yapılandırmalar** veya **derlenen yapılandırmalar** sekmesine tıklayın ve ardından sayfanın üst kısmındaki menüde **yapılandırma oluştur** ' a tıklayın.
1. **Temel bilgiler** adımında yeni yapılandırma adını (gerekli) belirtin ve yeni yapılandırmanıza eklemek istediğiniz her bileşik kaynağın satırındaki herhangi bir yere tıklayın ve ardından **İleri** ' ye tıklayın veya **kaynak kodu** adımına tıklayın. Aşağıdaki adımlar için `PSExecutionPolicy` ve bileşik kaynaklar seçtik `RenameAndDomainJoin` .
   ![Yapılandırma oluşturma sayfasının temel kavramlar adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-basics.png)
1. **Kaynak kodu** adımı, seçili bileşik kaynakların oluşturulan yapılandırmasının nasıl göründüğünü gösterir. Tüm parametrelerin birleştirilmesini ve bunların bileşik kaynağa nasıl geçtiğini görebilirsiniz. Yeni kaynak kodu gözden geçirmeyi tamamladığınızda, **İleri** ' ye tıklayın veya **Parametreler** adımına tıklayın.
   ![Yapılandırma oluşturma sayfasının kaynak kodu adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-sourcecode.png)
1. **Parametreler** adımında, her bileşik kaynağın parametresi, değerlerin sağlanabilmesi için sunulur. Bir parametrenin açıklaması varsa, parametre alanının yanında görüntülenir. Bir parametre `PSCredential` türü ise, açılan liste geçerli Otomasyon hesabındaki **kimlik bilgisi** nesnelerinin bir listesini sağlar. Bir **+ kimlik bilgisi ekle** seçeneği de mevcuttur. Tüm gerekli parametreler sağlandıktan sonra **Kaydet ve derle**' ye tıklayın.
   ![Yapılandırma oluşturma sayfasının parametreler adımının ekran görüntüsü](./media/compose-configurationwithcompositeresources/compose-configuration-parameters.png)

## <a name="submit-the-configuration-for-compilation"></a>Derleme için yapılandırmayı gönder

Yeni yapılandırma kaydedildikten sonra, derleme için gönderilir. Derleme işinin durumunu, içeri aktarılan tüm yapılandırmayla yaptığınız gibi görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [derleme Işini görüntüleme](automation-dsc-getting-started.md#view-a-compilation-job).

Derleme başarıyla tamamlandığında, yeni yapılandırma **derlenen yapılandırmalar** sekmesinde görünür. Daha sonra, [bir düğümü farklı bir düğüm yapılandırmasına yeniden atama](automation-dsc-getting-started.md#reassign-a-node-to-a-different-node-configuration)bölümündeki adımları kullanarak bir yönetilen düğüme yapılandırmayı atayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Otomasyonu durum yapılandırması 'nı kullanmaya başlama](automation-dsc-getting-started.md)
- [Azure Otomasyonu durum yapılandırmasını etkinleştirme](automation-dsc-onboarding.md)
- [Azure Otomasyonu durum yapılandırmasında DSC yapılandırmalarını derleme](automation-dsc-compile.md)
- [Chocolatey ile sürekli dağıtım ayarlama](automation-dsc-cd-chocolatey.md)
- [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
