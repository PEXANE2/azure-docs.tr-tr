---
title: Visual Studio Code için Azure Ilke uzantısı
description: Visual Studio Code için Azure Ilke uzantısı 'nı kullanarak Azure Resource Manager diğer adları arama hakkında bilgi edinin.
ms.date: 06/16/2020
ms.topic: how-to
ms.openlocfilehash: 9b3f27492bee5301e22e83ebf9734afa3de43897
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89648340"
---
# <a name="use-azure-policy-extension-for-visual-studio-code"></a>Visual Studio Code için Azure Ilke uzantısı 'nı kullanın

> Azure Ilke uzantısı sürüm **0.0.21** ve üzeri için geçerlidir

[Diğer adları](../concepts/definition-structure.md#aliases) aramak ve kaynakları ve ilkeleri gözden geçirmek için Visual Studio Code Için Azure ilke uzantısı 'nı nasıl kullanacağınızı öğrenin. İlk olarak, Visual Studio Code Azure Ilke uzantısının nasıl yükleneceğini açıklayacağız. Daha sonra diğer adların nasıl arandığını adım adım inceleyeceğiz.

Visual Studio Code için Azure Ilke uzantısı, Visual Studio Code tarafından desteklenen tüm platformlara yüklenebilir. Bu destek, Windows, Linux ve macOS içerir.

> [!NOTE]
> Visual Studio Code için Azure Ilke uzantısında görüntülenen ilkelerde yerel olarak yapılan değişiklikler Azure ile eşitlenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki öğeler gereklidir:

- Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- [Visual Studio Code](https://code.visualstudio.com).

## <a name="install-azure-policy-extension"></a>Azure Ilke uzantısı 'nı yükler

Önkoşulları karşıladıktan sonra, aşağıdaki adımları izleyerek Visual Studio Code için Azure Ilke uzantısı 'nı yükleyebilirsiniz:

1. Visual Studio Code’u açın.

1. Menü çubuğunda, uzantıları **görüntüle**' ye gidin  >  **Extensions**.

1. Arama kutusuna **Azure ilkesi**' ni girin.

1. Arama sonuçlarından **Azure ilkesi** ' ni seçin ve ardından **Install**' ı seçin.

1. Gerektiğinde **yeniden yükle** ' yi seçin.

## <a name="set-the-azure-environment"></a>Azure ortamını ayarlama

Ulusal bir bulut kullanıcısı için, önce Azure ortamını ayarlamak üzere aşağıdaki adımları izleyin:

1. **File\preferences\settings**öğesini seçin.

1. Şu dizeyi arayın: _Azure: Cloud_

1. Listeden ulus bulutu ' nı seçin:

   :::image type="content" source="../media/extension-for-vscode/set-default-azure-cloud-sign-in.png" alt-text="Visual Studio Code için ulus Azure bulutu oturum açma seçeneğinin ekran görüntüsü." border="false":::

## <a name="connect-to-an-azure-account"></a>Azure hesabına bağlanma

Kaynakları ve arama diğer adlarını değerlendirmek için Azure hesabınıza bağlanmanız gerekir. Visual Studio Code Azure 'a bağlanmak için şu adımları izleyin:

1. Azure Ilke uzantısından veya komut paletinden Azure 'da oturum açın.

   - Azure Ilke uzantısı

     Azure Ilke uzantısı ' **nda Azure 'Da oturum aç**' ı seçin.

     :::image type="content" source="../media/extension-for-vscode/azure-cloud-sign-in-policy-extension.png" alt-text="Visual Studio Code ekran görüntüsü ve Azure Ilke uzantısı simgesi." border="false":::

   - Komut paleti

     Menü çubuğundan **Görünüm**  >  **komut paleti**' ne gidin ve **Azure: oturum aç**' ı girin.

     :::image type="content" source="../media/extension-for-vscode/azure-cloud-sign-in-command-palette.png" alt-text="Komut paletinden Visual Studio Code için Azure bulutu oturum açma seçeneklerinin ekran görüntüsü." border="false":::

1. Azure 'da oturum açmak için oturum açma yönergelerini izleyin. Bağlandıktan sonra, Azure hesabınızın adı Visual Studio Code penceresinin altındaki durum çubuğunda gösterilir.

## <a name="select-subscriptions"></a>Abonelikleri Seç

İlk kez oturum açtığınızda, Azure Ilke uzantısı tarafından yalnızca varsayılan abonelik kaynakları ve ilkeleri yüklenir. Kaynakları ve ilkeleri görüntülemeye yönelik abonelikler eklemek veya kaldırmak için şu adımları izleyin:

1. Komut paletinden veya pencere altbilgisinde abonelik komutunu başlatın.

   - Komut paleti: 

     Menü çubuğundan, **görüntüleme**  >  **komut paleti**' ne gidin ve **Azure: abonelikleri Seç**' i girin.

   - Pencere altbilgisi

     Ekranın alt kısmındaki pencere altbilgisinde, Azure ile eşleşen segmenti seçin **: \<your account\> **.

1. Abonelik adına göre hızlı bir şekilde bulmak için filtre kutusunu kullanın. Ardından, Azure Ilke uzantısı tarafından gösterilen abonelikleri ayarlamak için her bir aboneliğin denetimini denetleyin veya kaldırın. Görüntülenecek abonelikleri ekleme veya kaldırma bittiğinde **Tamam**' ı seçin.

## <a name="search-for-and-view-resources"></a>Kaynakları arama ve görüntüleme

Azure Ilke uzantısı kaynak sağlayıcısına göre seçilen aboneliklerdeki kaynakları ve **kaynaklar** bölmesindeki kaynak grubuna göre listeler. TreeView, seçilen abonelik içinde veya abonelik düzeyinde aşağıdaki kaynak gruplarını içerir:

- **Kaynak sağlayıcıları**
  - Kaynaklara ve ilke diğer adlarına sahip ilgili alt kaynaklara sahip kayıtlı her kaynak sağlayıcısı
- **Kaynak grupları**
  - Kaynak grubuna göre tüm kaynaklar

Varsayılan olarak, uzantı ' kaynak sağlayıcısı ' bölümünü, ilke diğer adlarına sahip mevcut kaynak ve kaynaklarla filtreler. **Settings**  >  **Extensions**  >  Tüm kaynak sağlayıcılarını filtrelemeden görmek için ayarlar uzantıları**Azure ilkesi** ' nde bu davranışı değiştirin.

Tek bir abonelikte yüzlerce veya binlerce kaynağa sahip müşteriler, kaynaklarını bulmaya yönelik aranabilir bir yol gerektirebilir. Azure Ilke uzantısı, aşağıdaki adımlarla belirli bir kaynağı aramanızı mümkün kılar:

1. Arama arabirimini Azure Ilke uzantısı veya komut paleti ' nden başlatın.

   - Azure Ilke uzantısı

     Azure Ilke uzantısı ' ndan **kaynaklar** panelinin üzerine gelin ve üç noktayı seçip **kaynakları ara**' yı seçin.

   - Komut paleti:

     Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve **Kaynaklar: ara kaynaklar**' a gidin.

1. Görüntülenmek üzere birden fazla abonelik seçildiyse, hangi aboneliğin aranacağını seçmek için filtreyi kullanın.

1. Daha önce seçilen aboneliğin parçası olan arama yapılacak kaynak grubunu seçmek için filtreyi kullanın.

1. Görüntülenecek kaynağı seçmek için filtreyi kullanın. Filtre, hem kaynak adı hem de kaynak türü için kullanılabilir.

## <a name="discover-aliases-for-resource-properties"></a>Kaynak özellikleri için diğer adları keşfet

Bir kaynak seçildiğinde, arama arabiriminden veya TreeView 'da seçilerek, Azure Ilke uzantısı bu kaynağı temsil eden JSON dosyasını ve tüm Azure Resource Manager özellik değerlerini açar.

Bir kaynak açıldıktan sonra, Kaynak Yöneticisi özellik adı veya değerinin üzerine gelindiğinde, varsa Azure Ilkesi diğer adı görüntülenir. Bu örnekte, kaynak bir `Microsoft.Compute/virtualMachines` kaynak türüdür ve **Properties. Storageprofile. ImageReference. teklifin** özelliği üzerine düştir. Vurgulama, eşleşen diğer adları gösterir.

:::image type="content" source="../media/extension-for-vscode/extension-hover-shows-property-alias.png" alt-text="Visual Studio Code için Azure Ilke uzantısının, diğer ad adlarını görüntüleyecek bir özelliği bir ekran görüntüsü." border="false":::

> [!NOTE]
> VS Code uzantısı yalnızca Kaynak Yöneticisi modu özelliklerini gösterir ve herhangi bir [kaynak sağlayıcısı modu](../concepts/definition-structure.md#mode) özelliği görüntülemez.

## <a name="search-for-and-view-policies-and-assignments"></a>İlke ve atamaları arama ve görüntüleme

Azure Ilke uzantısı ilke türlerini ve ilke atamalarını, **ilkeler** bölmesinde görüntülenmek üzere seçilen abonelikler için bir TreeView olarak listeler. Tek bir abonelikte yüzlerce veya binlerce ilke veya atama içeren müşteriler, ilkelerini veya atamalarını bulmak için aranabilir bir yol tercih edebilir. Azure Ilke uzantısı, aşağıdaki adımlarla belirli bir ilke veya atamaya yönelik arama yapmayı mümkün kılar:

1. Arama arabirimini Azure Ilke uzantısı veya komut paleti ' nden başlatın.

   - Azure Ilke uzantısı

     Azure Ilke uzantısı ' ndan, **ilkeler** panelinin üzerine gelin ve üç noktayı seçin ve ardından **ilkeleri ara**' yı seçin.

   - Komut paleti:

     Menü çubuğundan **Görünüm** > **komut paleti**' ne gidin ve **ilkeler: ara ilkeler**' i girin.

1. Görüntülenmek üzere birden fazla abonelik seçildiyse, hangi aboneliğin aranacağını seçmek için filtreyi kullanın.

1. Daha önce seçilen aboneliğin bir parçası olan arama yapılacak ilke türünü veya atamayı seçmek için filtreyi kullanın.

1. Hangi ilkenin veya görüntülenecek ilkeyi seçmek için filtreyi kullanın. Filtre, ilke tanımı veya ilke ataması için _DisplayName_ için geçerlidir.

Bir ilke veya atamayı seçerken, arama arabiriminden veya TreeView 'da seçerek Azure Ilke uzantısı, ilkeyi veya atamayı ve tüm Kaynak Yöneticisi özellik değerlerini temsil eden JSON 'ı açar. Uzantı, açılan Azure Ilkesi JSON şemasını doğrulayabilir.

## <a name="sign-out"></a>Oturumu kapat

Menü çubuğundan **Görünüm**  >  **komut paleti**' ne gidin ve **Azure: oturumu Kapat**' ı girin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure ilke örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [Program aracılığıyla ilkelerin nasıl oluşturulduğunu](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları nasıl düzelteceğinizi](remediate-resources.md)öğrenin.
- [Kaynakları Azure Yönetim gruplarıyla düzenleme](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu inceleyin.