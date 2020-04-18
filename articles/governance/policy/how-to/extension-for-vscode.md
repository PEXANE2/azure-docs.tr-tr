---
title: Visual Studio Code için Azure İlkesi uzantısı
description: Kaynak Yöneticisi takma adlarını aramak için Visual Studio Code için Azure İlkesi uzantısını nasıl kullanacağınızı öğrenin.
ms.date: 03/07/2020
ms.topic: how-to
ms.openlocfilehash: 5e31af652f2746adbf0eda386bdb178c752f5f84
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641016"
---
# <a name="use-azure-policy-extension-for-visual-studio-code"></a>Visual Studio Code için Azure İlkesi uzantısını kullanma

> Azure İlkesi uzantısı **0.0.21** sürümü ve daha yeni sürümü için geçerlidir

[Diğer adları](../concepts/definition-structure.md#aliases) aramak ve kaynakları ve ilkeleri gözden geçirmek için Visual Studio Code için Azure İlkesi uzantısını nasıl kullanacağınızı öğrenin. İlk olarak, Azure İlkesi uzantısını Visual Studio Code'a nasıl yükleyeceğiniz açıklanır. Sonra takma adlara nasıl bakacağız.

Visual Studio Code için Azure İlkesi uzantısı Visual Studio Code tarafından desteklenen tüm platformlara yüklenebilir. Bu destek Windows, Linux ve macOS'u içerir.

> [!NOTE]
> Visual Studio Code için Azure İlkesi uzantısında görüntülenen ilkelerde yerel olarak yapılan değişiklikler Azure ile eşitlenmez.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için aşağıdaki öğeler gereklidir:

- Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.
- [Görsel Stüdyo Kodu](https://code.visualstudio.com).

## <a name="install-azure-policy-extension"></a>Azure İlkesi uzantısını yükleme

Ön koşulları tutturdıktan sonra, Visual Studio Code için Azure İlkesi uzantısını aşağıdaki adımları izleyerek yükleyebilirsiniz:

1. Visual Studio Code'u açın.

1. Menü çubuğundan**Uzantıları** **Görüntüle'ye** > gidin.

1. Arama kutusuna **Azure İlkesi'ni**girin.

1. Arama sonuçlarından **Azure İlkesi'ni** seçin ve ardından **Yükle'yi**seçin.

1. Gerektiğinde **Yeniden Yükle'yi** seçin.

## <a name="set-the-azure-environment"></a>Azure ortamını ayarlama

Ulusal bir bulut kullanıcısı için Azure ortamını önce ayarlamak için aşağıdaki adımları izleyin:

1. **Dosya\Tercihler\Ayarlar'ı**seçin.

1. Aşağıdaki dizeüzerinde arama: _Azure: Bulut_

1. Listeden ulus bulutu seçin:

   ![Visual Studio Code için varsayılan Azure bulut oturum açma](../media/extension-for-vscode/set-default-azure-cloud-sign-in.png)

## <a name="connect-to-an-azure-account"></a>Azure hesabına bağlanma

Kaynakları değerlendirmek ve arama takma adları için Azure hesabınıza bağlanmanız gerekir. Visual Studio Code'tan Azure'a bağlanmak için aşağıdaki adımları izleyin:

1. Azure İlkesi uzantısı veya Komut Paleti'nden Azure'da oturum açın.

   - Azure İlkesi uzantısı

     Azure İlkesi uzantısından **Azure'da Oturum Aç'ı'nı**seçin.

     ![Azure İlkesi uzantısından Visual Studio Kodu için Azure bulut oturum açma](../media/extension-for-vscode/azure-cloud-sign-in-policy-extension.png)

   - Komut Paleti

     Menü çubuğundan**Komut Paletini** **Görüntüle'ye** > gidin ve Azure' u **girin: Oturum Aç**.

     ![Command Palette'den Visual Studio Kodu için Azure bulut oturum açma](../media/extension-for-vscode/azure-cloud-sign-in-command-palette.png)

1. Azure'da oturum açma yönergelerini izleyin. Bağlandıktan sonra Azure hesap adınız Visual Studio Code penceresinin altındaki durum çubuğunda gösterilir.

## <a name="select-subscriptions"></a>Abonelikleri seçin

İlk oturum açtığınızda, Azure İlkesi uzantısı tarafından yalnızca varsayılan abonelik kaynakları ve ilkeleri yüklenir. Abonelikleri görüntüleyen kaynaklar ve ilkeler eklemek veya kaldırmak için aşağıdaki adımları izleyin:

1. Abonelik komutunu Komut Paleti'nden veya pencere altbilgisinden başlatın.

   - Komut Paleti: 

     Menü çubuğundan**Komut Paletini** **Görüntüle'ye** > gidin ve **Azure'u girin: Abonelikleri seçin.**

   - Pencere altbilgi

     Ekranın altındaki pencere altbilgisinde, Azure ile eşleşen kesimi ** \<seçin:\>hesabınız.**

1. Abonelikleri ada göre hızla bulmak için filtre kutusunu kullanın. Ardından, Azure İlkesi uzantısı tarafından gösterilen abonelikleri ayarlamak için her abonelikten çeki denetleyin veya kaldırın. Görüntülemek için abonelik ekleme veya kaldırma bittiğinde, **Tamam'ı**seçin.

## <a name="search-for-and-view-resources"></a>Kaynakları arama ve görüntüleme

Azure İlkesi uzantısı, Kaynak Sağlayıcı tarafından seçili aboneliklerde ve **Kaynaklar** bölmesinde kaynak grubuna göre kaynakları listeler. Ağaç görünümü, seçili abonelik içinde veya abonelik düzeyinde ki kaynakların aşağıdaki gruplandırmalarını içerir:

- **Kaynak Sağlayıcıları**
  - İlke takma adları olan kaynaklara ve ilgili alt kaynaklara sahip her kayıtlı Kaynak Sağlayıcısı
- **Kaynak Grupları**
  - Içinde oldukları kaynak grubuna göre tüm kaynaklar

Varsayılan olarak, uzantı 'Kaynak Sağlayıcı' bölümünü ilke takma adları olan varolan kaynaklara ve kaynaklara göre filtreler. Tüm Kaynak **Settings** > **Sağlayıcılarını** > filtreleme den görmek için Ayarlar Uzantıları**Azure İlkesi'nde** bu davranışı değiştirin.

Tek bir abonelikte yüzlerce veya binlerce kaynağı olan müşteriler, kaynaklarını bulmak için aranabilir bir yolu tercih edebilir. Azure İlkesi uzantısı, aşağıdaki adımlarla belirli bir kaynağı aramayı mümkün kılar:

1. Arama arabirimini Azure İlkesi uzantısından veya Komut Paleti'nden başlatın.

   - Azure İlkesi uzantısı

     Azure İlkesi uzantısından, **Kaynaklar** panelinin üzerine gidip elipsleri seçin, ardından **Arama Kaynakları'nı**seçin.

   - Komut Paleti:

     Menü çubuğundan **Komut Paletini** **Görüntüle'ye** > gidin ve **Kaynakları girin: Kaynakları Arayın.**

1. Görüntülemek için birden fazla abonelik seçilirse, hangi aboneliğin aranmasını seçmek için filtreyi kullanın.

1. Daha önce seçilen aboneliğin bir parçası olan hangi kaynak grubunu aramak için filtreyi kullanın.

1. Hangi kaynağın görüntüleneciyi seçmek için filtreyi kullanın. Filtre hem kaynak adı hem de kaynak türü için çalışır.

## <a name="discover-aliases-for-resource-properties"></a>Kaynak özellikleri için diğer adları bulma

Bir kaynak seçildiğinde, ister arama arabirimi üzerinden ister ağaç görünümünde seçilerek, Azure İlkesi uzantısı, bu kaynağı ve tüm Kaynak Yöneticisi özellik değerlerini temsil eden JSON dosyasını açar.

Bir kaynak açıldıktan sonra, Kaynak Yöneticisi özellik adının veya değerinin üzerinde gezinirken varsa Azure İlkesi takma adını görüntüler. Bu örnekte, kaynak `Microsoft.Compute/virtualMachines` bir kaynak türüdür ve **properties.storageProfile.imageReference.offer** özelliği üzerinde gezinilir. Gezinme eşleşen diğer adları gösterir.

![Azure İlkesi uzantısı kaynak yöneticisi özellik takma adını gösterir](../media/extension-for-vscode/extension-hover-shows-property-alias.png)

## <a name="search-for-and-view-policies-and-assignments"></a>İlkeleri ve atamaları arama ve görüntüleme

Azure İlkesi uzantısı, **İlkeler** bölmesinde görüntülenmek üzere seçilen abonelikler için ilke türlerini ve ilke atamalarını ağaç görünümü olarak listeler. Tek bir abonelikte yüzlerce veya binlerce ilke veya atamaya sahip müşteriler, ilkelerini veya görevlerini bulmak için aranabilir bir yol tercih edebilir. Azure İlkesi uzantısı, aşağıdaki adımlarla belirli bir ilke veya atamada arama yapmayı mümkün kılar:

1. Arama arabirimini Azure İlkesi uzantısından veya Komut Paleti'nden başlatın.

   - Azure İlkesi uzantısı

     Azure İlkesi uzantısından, **İlkeler** panelinin üzerine gidip elipsleri seçin, ardından **Arama İlkeleri'ni**seçin.

   - Komut Paleti:

     Menü çubuğundan **Komut Paleti'ni** **görüntüle'ye** > gidin ve **İlkeler: Arama İlkeleri'ni**girin.

1. Görüntülemek için birden fazla abonelik seçilirse, hangi aboneliğin aranmasını seçmek için filtreyi kullanın.

1. Daha önce seçilen aboneliğin bir parçası olan aramayı yapmak için hangi ilke türünü veya atamayı seçmek için filtreyi kullanın.

1. Hangi ilkeyi veya görüntülenin i Filtre, ilke tanımı veya ilke ataması için _displayName_ için çalışır.

Bir ilke veya atama seçerken, ister arama arabirimi üzerinden ister ağaç görünümünde seçerek, Azure İlkesi uzantısı ilkeyi veya atamayı ve tüm Kaynak Yöneticisi özellik değerlerini temsil eden JSON'u açar. Uzantı, açılan Azure İlkesi JSON şeasını doğrulayabilir.

## <a name="sign-out"></a>Oturumu kapat

Menü çubuğundan**Komut Paletini** **Görüntüle'ye** > gidin ve ardından **Azure'u girin: Çıkış Yapın.**

## <a name="next-steps"></a>Sonraki adımlar

- [Azure İlkesi örneklerindeki](../samples/index.md)örnekleri gözden geçirin.
- [Azure İlkesi tanımı yapısını](../concepts/definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](../concepts/effects.md) konusunu gözden geçirin.
- [İlkeleri programlı bir şekilde nasıl oluşturlayacağımı](programmatically-create.md)anlayın.
- [Uyumlu olmayan kaynakları](remediate-resources.md)nasıl düzelteriz öğrenin.
- [Azure yönetim gruplarıyla kaynaklarınızı düzenleyin](../../management-groups/overview.md)ile yönetim grubunun ne olduğunu gözden geçirin.