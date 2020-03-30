---
title: Azure DevTest Labs'daki bir laboratuvara VM ekleme | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvara sanal makine eklemeyi öğrenin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: 22060cc6dd5eb15e81a0c397a7b0255f16780d74
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284258"
---
# <a name="add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki laboratuvara VM ekleme
Zaten ilk [VM oluşturduysanız,](tutorial-create-custom-lab.md#add-a-vm-to-the-lab)büyük olasılıkla önceden yüklenmiş bir [pazar görüntüsü](devtest-lab-configure-marketplace-images.md)nden yaptı. Şimdi, sonraki VM'leri laboratuvarınıza eklemek istiyorsanız, özel bir [resim](devtest-lab-create-template.md) veya [formül](devtest-lab-manage-formulas.md)olan bir *taban* da seçebilirsiniz. Bu öğretici, DevTest Labs'daki bir laboratuvara VM eklemek için Azure portalını kullanmanız için size yol açar.

Bu makalede, laboratuarınızda bir VM için eserler nasıl yönetilen gösterir.

## <a name="steps-to-add-a-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara VM ekleme adımları
1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından **DEVOPS** **bölümündeki DevTest Labs'ı** seçin. **DEVOPS** bölümünde **DevTest Labs** yanında * (yıldız) seçerseniz. Bu eylem, bir sonraki sefere kolayca erişebilmeniz için **DevTest Labs'ı** sol navigasyon menüsüne ekler. Ardından, sol daki gezinme menüsünden **DevTest Labs'ı** seçebilirsiniz.

    ![Tüm hizmetler - DevTest Labs'ı seçin](./media/devtest-lab-create-lab/all-services-select.png)
1. Laboratuvarlar listesinden, VM'yi oluşturmak istediğiniz laboratuarı seçin.
2. Laboratuvarın Genel **Bakış** sayfasında **+ Ekle'yi**seçin.

    ![VM düğmesi ekle](./media/devtest-lab-add-vm/devtestlab-home-blade-add-vm.png)
1. Temel sayfa **seçin'de** VM için bir pazar resmi seçin.
1. **Sanal makine** sayfasının **Temel Ayarlar** sekmesinde aşağıdaki işlemleri yapın:
    1. **Sanal makine adı** metin kutusuna VM için bir ad girin. Metin kutusu sizin için benzersiz bir otomatik oluşturulan adile doldurulmuştur. Ad, e-posta adresinizdeki kullanıcı adına ve ardından benzersiz bir 3 basamaklı bir sayıya karşılık gelir. Bu özellik, bir makine adını düşünmek ve bir makine oluşturmak her zaman yazmak için zaman kazandırır. İsterseniz bu otomatik doldurulmuş alanı seçtiğiniz bir adla geçersiz kılabilirsiniz. VM'nin otomatik doldurulan adını geçersiz kılmak için **Sanal makine adı** metin kutusuna bir ad girin.
    2. Sanal makinede yönetici ayrıcalıkları verilen bir **Kullanıcı Adı** girin. Makinenin **kullanıcı adı,** otomatik olarak oluşturulmuş benzersiz bir adla önceden doldurulmuştur. Ad, e-posta adresinizdeki kullanıcı adına karşılık gelir. Bu özellik, yeni bir makine oluşturduğunuzher zaman bir kullanıcı adına karar vermeniz için size zaman kazandırır. Yine, isterseniz seçtiğiniz bir kullanıcı adı ile bu otomatik doldurulmuş alanı geçersiz kılabilirsiniz. Kullanıcı adı için otomatik doldurulan değeri geçersiz kılmak için **Kullanıcı Adı** metin kutusuna bir değer girin. Bu kullanıcıya sanal makinede **yönetici** ayrıcalıkları verilir.
    3. Laboratuvarda ilk VM oluşturuyorsanız, kullanıcı için bir **parola** girin. Bu parolayı laboratuvarla ilişkili Azure anahtar kasasında varsayılan parola olarak kaydetmek için **varsayılan parola olarak kaydet'i**seçin. Varsayılan parola, **vmPassword**adı ile anahtar kasasına kaydedilir. Laboratuvarda sonraki VM'leri oluşturmaya çalıştığınızda, **VmPassword** **parola**için otomatik olarak seçilir. Değeri geçersiz kılmak için **kaydedilmiş gizli** onay kutusunu kullan'ı temizleyin ve bir parola girin.

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-virtual-machine.png)

        Ayrıca önce anahtar kasasında sırları kaydedebilir ve daha sonra laboratuvarda bir VM oluştururken kullanabilirsiniz. Daha fazla bilgi için, [önemli bir kasadaki mağaza sırlarına](devtest-lab-store-secrets-in-key-vault.md)bakın. Anahtar kasasında depolanan parolayı kullanmak **için, kaydedilmiş bir sırrı kullan'ı**seçin ve sırrınıza (parolanıza) karşılık gelen bir anahtar değeri belirtin.
    4. Diğer **seçenekler** bölümünde, **boyutu değiştir'i**seçin. Oluşturmak için işlemci çekirdeklerini, RAM boyutunu ve VM'nin sabit disk boyutunu belirten önceden tanımlanmış öğelerden birini seçin.
    5. **Yapı Tonu Ekle veya Kaldır'ı**seçin. Temel görüntüye eklemek istediğiniz yapıları seçin ve yapılandırır.
    **Not:** DevTest Labs'da yeniyseniz veya yapılandırılan yapıları yeniyseniz, [varolan bir yapıyı VM bölümüne ekle'ye](./devtest-lab-add-vm.md#add-an-existing-artifact-to-a-vm) bakın ve bittiğinde buraya dönün.
2. En üstteki **Gelişmiş Ayarlar** sekmesine geçin ve aşağıdaki işlemleri yapın:
    1. VM'nin içinde olduğu sanal ağı değiştirmek için **VNet'i değiştir'i**seçin.
    2. Alt ağı değiştirmek için **alt ağı değiştir'i**seçin.
    3. VM'nin IP adresinin **herkese açık mı, özel mi yoksa paylaşılan**mı olduğunu belirtin.
    4. VM'yi otomatik olarak silmek için **son kullanma tarihini ve saatini**belirtin.
    5. VM'yi bir laboratuvar kullanıcısı tarafından talep edilebilir hale getirmek **için, bu makineyi talep edilebilir hale getirmek** için **Evet** seçeneğini belirleyin.
    6. Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek istediğiniz **VM örneklerinin** sayısını belirtin.

        ![Bir temel seçin](./media/tutorial-create-custom-lab/new-vm-advanced-settings.png)
1. Belirtilen VM'yi laboratuvara eklemek için **Oluştur'u** seçin.

   Laboratuvar sayfası VM oluşturma durumunu görüntüler - oluşturma **olarak**ilk , sonra VM başlatıldıktan sonra **Çalışan** olarak.

    ![Sanal makine oluşturma durumu](./media/tutorial-create-custom-lab/vm-creation-status.png)

## <a name="add-an-existing-artifact-to-a-vm"></a>VM'ye varolan bir yapı ekleme
VM oluştururken varolan yapıları ekleyebilirsiniz. Her laboratuvar, Public DevTest Labs Artifact Repository'nin yanı sıra kendi Artifon Deposu'na oluşturduğunuz ve eklediğiniz eserleri içerir.

* Azure DevTest Labs *yapıları,* Windows PowerShell komutları çalıştırma, Bash komutlarını çalıştırma ve yazılım yükleme gibi VM sağlandığında gerçekleştirilen *eylemleri* belirtmenize izin tanır.
* Yapı *parametreleri,* belirli senaryonuz için yapıyı özelleştirmenize izin

Eserler oluşturmak için nasıl, makaleye bakın, [DevTest Labs ile kullanılmak üzere kendi eserler yazar öğrenin.](devtest-lab-artifact-author.md)

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden, çalışmak istediğiniz VM'yi içeren laboratuarı seçin.
1. **Sanal makinelerimi**seçin.
1. İstediğiniz VM'yi seçin.
1. **Yapıtları Yönet'i**seçin.
1. **Yapıtları Uygula'yı**seçin.
1. **Yapıtları Uygula** bölmesine, VM'ye eklemek istediğiniz yapıyı seçin.
1. Yapı **ekle** bölmesine, gerekli parametre değerlerini ve ihtiyacınız olan isteğe bağlı parametreleri girin.
1. Yapıyı eklemek ve **yapıtları uygula** bölmesine dönmek için **Ekle'yi** seçin.
1. VM'iniz için gerektiği gibi yapı eklemeye devam edin.
1. Yapıtlarınızı ekledikten sonra, [yapıtların çalıştırılan sırasını değiştirebilirsiniz.](#change-the-order-in-which-artifacts-are-run) Ayrıca [bir yapıyı görüntülemek veya değiştirmek](#view-or-modify-an-artifact)için geri dönebilirsiniz.
1. Yapı eklemeyi bitirdiğinizde Uygula'yı **seçin**

## <a name="change-the-order-in-which-artifacts-are-run"></a>Yapıtların çalıştırılme sırasını değiştirme
Varsayılan olarak, yapıtların eylemleri VM'ye eklendikleri sırada yürütülür.
Aşağıdaki adımlar, yapıtların çalıştırılan sırayı nasıl değiştireceğini gösterir.

1. **Yapıtları Uygula** bölmesinin üst kısmında, VM'ye eklenen yapıların sayısını belirten bağlantıyı seçin.

    ![VM'ye eklenen yapı sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. **Seçili eserler** bölmesindeki, yapıları sürükleyin ve istenilen sıraya bırakın. **Not:** Yapıyı sürüklemede sorun yaşıyorsanız, yapının sol tarafından sürüklediğinizden emin olun.
1. Tamamladığınızda **Tamam**’ı seçin.

## <a name="view-or-modify-an-artifact"></a>Bir yapıyı görüntüleme veya değiştirme
Aşağıdaki adımlar, bir yapının parametrelerini nasıl görüntülenebildiğini veya değiştirilebildiğini gösterir:

1. **Yapıtları Uygula** bölmesinin üst kısmında, VM'ye eklenen yapıların sayısını belirten bağlantıyı seçin.

    ![VM'ye eklenen yapı sayısı](./media/devtest-lab-add-vm-with-artifacts/devtestlab-add-artifacts-blade-selected-artifacts.png)
1. **Seçili yapılar** bölmesinde, görüntülemek veya görüntülemek istediğiniz yapıyı seçin.
1. Yapı **ekle** bölmesine, gerekli değişiklikleri yapın ve yapı **ekle** bölmesini kapatmak için **Tamam'ı** seçin.
1. **Seçili yapılar** bölmesini kapatmak için **Tamam'ı** seçin.

## <a name="save-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kaydet
Azure Kaynak Yöneticisi şablonu, yinelenebilir bir dağıtımı tanımlamak için bildirimsel bir yol sağlar.
Aşağıdaki adımlar, oluşturulan VM için Azure Kaynak Yöneticisi şablonuna nasıl kaydedilen açıklar.
Kaydedildikten sonra, [Azure PowerShell ile yeni VM'ler dağıtmak](../azure-resource-manager/templates/overview.md)için Azure Kaynak Yöneticisi şablonunu kullanabilirsiniz.

1. Sanal **makine** bölmesine, **Azure Kaynak Yöneticisi Şablonu'nu Görüntüle'yi**seçin.
2. Azure **Kaynak Yöneticisi'ni Görüntüle şablon** bölmesine şablon metnini seçin.
3. Seçili metni panoya kopyalayın.
4. **Azure Kaynak Yöneticisi Şablonu'nu Görüntülemek bölmesini**kapatmak için **Tamam'ı** seçin.
5. Bir metin düzenleyicisi açın.
6. Panodaki şablon metnine yapıştırın.
7. Dosyayı daha sonra kullanmak üzere kaydedin.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* VM oluşturulduktan sonra, VM bölmesindeki **Bağlan'ı** seçerek VM'ye bağlanabilirsiniz.
* [DevTest Labs VM'iniz için özel yapılar oluşturmayı](devtest-lab-artifact-author.md)öğrenin.
* [DevTest Labs Azure Kaynak Yöneticisi QuickStart şablon galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)keşfedin.
