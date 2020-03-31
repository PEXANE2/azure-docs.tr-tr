---
title: Azure DevTest Labs'da talep edilebilir VM'ler oluşturma ve yönetme | Microsoft Dokümanlar
description: Azure DevTest Labs'daki bir laboratuvara nasıl talep edilebilir bir sanal makine ekleyeceğinizi öğrenin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: f671e66e-9630-4e30-a131-a6bad9ed9c11
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: 13d642597fdf5d0eae6c6fd4f0cab16181f033c2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270803"
---
# <a name="create-and-manage-claimable-vms-in-azure-devtest-labs"></a>Azure DevTest Laboratuvarlarında talep edilebilir VM'ler oluşturma ve yönetme
Bir laboratuvara, [standart vm'yi](devtest-lab-add-vm.md) nasıl eklediğinize benzer bir şekilde , özel bir [görüntü,](devtest-lab-create-template.md) [formül](devtest-lab-manage-formulas.md)veya Market [görüntüsü](devtest-lab-configure-marketplace-images.md)olan bir *tabandan* talep edilebilir bir VM eklersiniz. Bu öğretici, DevTest Labs'daki bir laboratuvara iddia edilebilir bir VM eklemek için Azure portalını kullanmanız için size yol gösterir ve kullanıcının VM'yi talep etmek ve geri almak için takip ettiği işlemleri gösterir.

## <a name="steps-to-add-a-claimable-vm-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'daki bir laboratuvara iddia edilebilir bir VM ekleme adımları
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
3. Belirtilen VM'yi laboratuvara eklemek için **Oluştur'u** seçin.

   Laboratuvar sayfası VM oluşturma durumunu görüntüler - oluşturma **olarak**ilk , sonra VM başlatıldıktan sonra **Çalışan** olarak.

> [!NOTE]
> Laboratuvar VM'lerini [Azure Kaynak Yöneticisi şablonları](devtest-lab-create-environment-from-arm.md)aracılığıyla dağıtırsanız, **allowClaim** özelliğini özellikler bölümünde doğru olarak ayarlayarak talep edilebilir VM'ler oluşturabilirsiniz.


## <a name="using-a-claimable-vm"></a>Talep edilebilir bir VM kullanma

Bir kullanıcı aşağıdaki adımlardan birini yaparak "Talep Edilebilir sanal makineler" listesinden herhangi bir VM talep edebilir:

* Laboratuvarın "Genel Bakış" bölmesinin altındaki "Talep Edilebilir sanal makineler" listesinden, listedeki VM'lerden birine sağ tıklayın ve **Talep makinesini**seçin.

  ![Belirli bir talep edilebilir VM isteyin.](./media/devtest-lab-add-vm/devtestlab-claim-VM.png)


* "Genel Bakış" bölmesinin üst kısmında, **Herhangi bir talep seçin.** İstenen Sanal Makineler listesinden rasgele bir sanal makine atanır.

  ![Talep edilebilir herhangi bir VM isteyin.](./media/devtest-lab-add-vm/devtestlab-claim-any.png)


Bir kullanıcı VM talep ettikten sonra, DevTest Labs makineyi başlatacak ve laboratuvar kullanıcısının "Sanal makinelerim" listesine taşıyacaktır. Bu, laboratuvar kullanıcısının artık bu makinede sahip ayrıcalıklarına sahip olacağı anlamına gelir. Bu adım için gereken süre, başlangıç süreleri ve talep olayı sırasında gerçekleştirilen diğer özel eylemlere bağlı olarak değişebilir. Talep edince, makine artık talep edilebilir havuzda kullanılamaz.  

## <a name="unclaim-a-vm"></a>VM'yi geri alma

Bir kullanıcı iddia edilen bir VM'yi kullanmayı bitirdiğinde ve bunu başka birinin kullanımına açmak istediğinde, aşağıdaki adımlardan birini yaparak talep edilen VM'yi talep edilebilir sanal makineler listesine geri döndürebilir:

- "Sanal makinelerim" listesinden, listedeki VM'lerden birine sağ tıklayın veya elipsini (...) seçin ve **Beğeni Yok'u**seçin.

  ![VM listesinde bir VM'yi reddedin.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM2.png)

- "Sanal makinelerim" listesinde, yönetim bölmesini açmak için bir VM seçin ve ardından üst menü çubuğundan **Reddi'ni** seçin.

  ![VM'nin yönetim bölmesindeki bir VM'yi geri alma.](./media/devtest-lab-add-vm/devtestlab-unclaim-VM.png)

Bir kullanıcı vm'yi geri aldığında, artık belirli bir laboratuvar VM'si için sahip izni yoktur ve bu izinler, havuza geri retured olduğunu eyaletteki herhangi bir laboratuvar kullanıcısı tarafından talep edilebilir. 

### <a name="transferring-the-data-disk"></a>Veri diskinin aktarılması
Talep edilebilir bir VM'de bir veri diski varsa ve kullanıcı bunu reddederse, veri diski VM'de kalır. Başka bir kullanıcı vm olduğunu iddia ettiğinde, yeni kullanıcı vm yanı sıra veri diski iddia ediyor.

Bu, "veri diskini aktarma" olarak bilinir. Veri diski daha sonra yeni kullanıcının yönetilebilmeleri için **benim veri diskleri** listesinde kullanılabilir hale gelir.

![Veri disklerini reddet.](./media/devtest-lab-add-vm/devtestlab-unclaim-datadisks.png)



## <a name="next-steps"></a>Sonraki adımlar
* Oluşturulduktan sonra, yönetim bölmesine **Bağlan'ı** seçerek VM'ye bağlanabilirsiniz.
* [DevTest Labs Azure Kaynak Yöneticisi hızlıBaşlat şablon galerisini](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates)keşfedin.
