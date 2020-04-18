---
title: VM'ler oluşturmak için Azure DevTest Labs'da formülleri yönetme | Microsoft Dokümanlar
description: Bu makalede, bir temel (özel görüntü, Market görüntüsü veya başka bir formül) veya varolan bir VM'den formül nasıl oluşturulacak gösteriş yapılır.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 841dd95a-657f-4d80-ba26-59a9b5104fe4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2020
ms.author: spelluru
ms.openlocfilehash: ce980167cd88adfada1aa294aafa885184565799
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641155"
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs formüllerini yönetme

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Bu makalede, bir temel (özel görüntü, Market görüntüsü veya başka bir formül) veya varolan bir VM'den formül nasıl oluşturulacak gösteriş yapılır. Bu makalede, varolan formülleri yönetme de size rehberlik eder.

## <a name="create-a-formula"></a>Formül oluşturma
DevTest Labs *Kullanıcıları* izinleri olan herkes bir temel olarak bir formül kullanarak VM'ler oluşturabilirsiniz. Formüloluşturmanın iki yolu vardır: 

* Bir tabandan - Formülün tüm özelliklerini tanımlamak istediğinizde kullanın.
* Varolan bir laboratuvardan VM - Varolan bir VM'nin ayarlarını temel alan bir formül oluşturmak istediğinizde kullanın.

Kullanıcı ve izin ekleme hakkında daha fazla bilgi için Azure [DevTest Labs'daki sahiplerini ve kullanıcılarını ekle'ye](./devtest-lab-add-devtest-user.md)bakın.

### <a name="create-a-formula-from-a-base"></a>Tabandan formül oluşturma
Aşağıdaki adımlar, özel bir görüntüden, Market görüntüsünden veya başka bir formülden formül oluşturma sürecinde size yol gösterin.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.

3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  

4. Laboratuvarın sayfasında, sol menüde **Formüller'i (yeniden kullanılabilir üsler)** seçin.
5. **Formüller** sayfasında + **Ekle'yi**seçin.
   
    ![Formül ekleme](./media/devtest-lab-create-formulas/add-formula.png)

6. Bir temel sayfa **seçin,** formülü oluşturmak istediğiniz tabanı (özel resim veya Market görüntüsü) seçin.
7. **Formül Oluştur** sayfasının **Temel Ayarlar** sekmesinde aşağıdaki değerleri belirtin:
   
    * **Formül adı** - Formülünüz için bir ad girin. Bu değer, bir VM oluşturduğunuzda temel görüntüler listesinde görüntülenir. Siz yazarken ad doğrulanır ve geçerli değilse, ileti geçerli bir ada gereksinimini gösterir.
    - Formül için isteğe bağlı bir **açıklama** girin. 
    * **Kullanıcı adı** - Yönetici ayrıcalıkları verilen bir kullanıcı adı girin.
    * **Parola** - Belirtilen kullanıcı için kullanmak istediğiniz gizli (parola) ile ilişkili bir değer açılır bırakma -veya açılır seçim seçin. Önemli bir kasada sırları kaydetme ve laboratuvar kaynakları oluştururken bunları kullanma hakkında bilgi edinmek için [Azure Anahtar Kasası'nda sırlar saklayın.](devtest-lab-store-secrets-in-key-vault.md)

        Parola kullanmak yerine Azure Key Vault'tan bir sır kullanmak istiyorsanız **kaydedilmiş bir sırrı** kullan'ı seçin. 
    * **Sanal makine boyutu** - VM boyutunu değiştirmek için **Boyutu Değiştir'i** seçin. 
    - **İşletim sistemi disk türü** - kullanmak istediğiniz disk türünü (Standart HDD, Standart SSD veya Premium SSD) seçin.
    * **Yapılar** - Temel resme eklemek istediğiniz yapıları seçip yapılandırdığınız **yapıyı ekle veya kaldır** sayfasını seçin. Yapılar hakkında daha fazla bilgi için azure [DevTest Labs sanal makineniz için özel yapılar oluşturun'a](devtest-lab-artifact-author.md)bakın.

        ![Temel ayarlar sayfası](./media/devtest-lab-create-formulas/basic-settings.png)
8. **Gelişmiş ayarlar** sekmesine geçin ve aşağıdaki değerleri belirtin:
    - **Sanal ağ** - Sanal ağı değiştirmek için **Vnet'i Değiştir'i**seçin. 
    - **Subnet** - Alt ağı değiştirmek için **Alt Ağı Değiştir'i**seçin. 
    - **IP adresi yapılandırması** - Genel, Özel veya Paylaşılan IP adreslerini isteyip istemediğinizi belirtin. Paylaşılan IP adresleri hakkında daha fazla bilgi için Azure [DevTest Labs'da paylaşılan IP adreslerini anlayın'](./devtest-lab-shared-ip.md)a bakın.
    - **Son kullanma tarihi ve saati** - Bu alanı kaldıramaz. 
    - **Bu makineyi talep edilebilir hale getirin** - Bir makineyi "talep edilebilir" hale getirmek, makinenin oluşturuldukları anda sahiplenilemeyeceğini anlamına gelir. Bunun yerine laboratuvar kullanıcıları, laboratuvarın sayfasındaki makinenin sahipliğini ("talep") alabilir.  

        ![Temel ayarlar sayfası](./media/devtest-lab-create-formulas/advanced-settings.png)
8. Formülü oluşturmak için **Gönder'i** seçin.

9. Formül oluşturulduğunda, **Formüller** sayfasındaki listede görüntülenir.

### <a name="create-a-formula-from-a-vm"></a>VM'den formül oluşturma
Aşağıdaki adımlar, varolan bir VM'yi temel alan bir formül oluşturma sürecinde size yol gösterin. 

> [!NOTE]
> VM'den bir formül oluşturmak için VM'nin 30 Mart 2016'dan sonra oluşturulmuş olması gerekir. 
> 
> 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  
4. Laboratuvarın Genel **Bakış** sayfasında, formülü oluşturmak istediğiniz VM'yi seçin.
   
    ![Laboratuvarlar VMs](./media/devtest-lab-create-formulas/my-vms.png)
5. VM'nin sayfasında formül **oluştur'u (yeniden kullanılabilir taban)** seçin.
   
    ![Formül oluşturma](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. Formül **Oluştur** sayfasında, yeni formülünüz için bir **Ad** ve **Açıklama** girin.
   
    ![Formül sayfası oluşturma](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Formülü oluşturmak için **Tamam'ı** seçin.

## <a name="modify-a-formula"></a>Formülü değiştirme
Formülü değiştirmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  
4. Laboratuvarın sayfasında **Formüller 'i (yeniden kullanılabilir üsler)** seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Laboratuvar **formülleri** sayfasında, değiştirmek istediğiniz formülü seçin.
6. **Formülgüncelleme** sayfasında, istediğiniz değişen dünyaları yapve Update' i seçin. **Update**

## <a name="delete-a-formula"></a>Formülü silme
Formülü silmek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  
4. Laboratuvar **Ayarları** sayfasında **Formüller'i**seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. Laboratuvar **formülleri** sayfasında, silmek istediğiniz formülün sağındaki elipsleri seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Formülün bağlam menüsünde **Sil'i**seçin.
   
    ![Formül bağlam menüsü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Silme onay iletişim kutusuna **Evet'i** seçin.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel görüntüler veya formüller?](devtest-lab-faq.md#what-is-the-difference-between-a-custom-image-and-a-formula)

## <a name="next-steps"></a>Sonraki adımlar
VM oluştururken kullanılacak bir formül oluşturduktan sonra, bir sonraki adım [laboratuvarınıza bir VM eklemektir.](devtest-lab-add-vm.md)

