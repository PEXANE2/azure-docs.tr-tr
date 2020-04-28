---
title: VM 'Ler oluşturmak için Azure DevTest Labs formülleri yönetme | Microsoft Docs
description: Bu makalede, bir temel (özel görüntü, Market görüntüsü veya başka bir formülden) veya var olan bir VM 'den nasıl formül oluşturacağınız gösterilmektedir.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81641155"
---
# <a name="manage-azure-devtest-labs-formulas"></a>Azure DevTest Labs formülleri yönetme

[!INCLUDE [devtest-lab-formula-definition](../../includes/devtest-lab-formula-definition.md)]

Bu makalede, bir temel (özel görüntü, Market görüntüsü veya başka bir formülden) veya var olan bir VM 'den nasıl formül oluşturacağınız gösterilmektedir. Bu makale ayrıca mevcut formülleri yönetirken size rehberlik eder.

## <a name="create-a-formula"></a>Formül oluşturma
DevTest Labs *kullanıcıları* izinleri olan herkes, bir formülü temel olarak kullanarak VM 'ler oluşturabilir. Formül oluşturmanın iki yolu vardır: 

* Bir taban kullanımı, formülün tüm özelliklerini tanımlamak istediğinizde.
* Mevcut bir laboratuvar VM 'sinden-mevcut bir sanal makinenin ayarlarına bağlı olarak bir formül oluşturmak istediğinizde kullanın.

Kullanıcıları ve izinleri ekleme hakkında daha fazla bilgi için, bkz. [Azure DevTest Labs sahip ve Kullanıcı ekleme](./devtest-lab-add-devtest-user.md).

### <a name="create-a-formula-from-a-base"></a>Bir tabandan formül oluşturma
Aşağıdaki adımlar özel görüntüden, Market görüntüsünden veya başka bir formülden formül oluşturma sürecinde size kılavuzluk gösterir.

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.

3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  

4. Laboratuvarın sayfasında, sol menüde **formüller (yeniden kullanılabilir tabanlar)** öğesini seçin.
5. **Formüller** sayfasında **+ Ekle**' yi seçin.
   
    ![Formül Ekle](./media/devtest-lab-create-formulas/add-formula.png)

6. **Temel seçin** sayfasında, formülü oluşturmak istediğiniz temeli (özel görüntü veya Market görüntüsü) seçin.
7. **Formül oluştur** sayfasının **temel ayarlar** sekmesinde, aşağıdaki değerleri belirtin:
   
    * **Formül adı** -formülünüz için bir ad girin. Bu değer, bir VM oluşturduğunuzda temel görüntüler listesinde görüntülenir. Ad siz yazarken onaylanır ve geçerli değilse, bir ileti geçerli bir ad için gereksinimleri gösterir.
    - Formül için isteğe bağlı bir **Açıklama** girin. 
    * **Kullanıcı adı** -yönetici ayrıcalıkları verilen bir Kullanıcı adı girin.
    * **Parola** -belirtilen kullanıcı için kullanmak istediğiniz gizli dizi (parola) ile ilişkili bir değer olan açılan menüden girin veya seçin. Gizli dizileri bir anahtar kasasında kaydetme ve laboratuvar kaynakları oluştururken kullanma hakkında bilgi edinmek için bkz. [Azure Key Vault gizli dizileri depolama](devtest-lab-store-secrets-in-key-vault.md).

        Parola kullanmak yerine Azure Key Vault bir gizli dizi kullanmak istiyorsanız, **kaydedilmiş gizli dizi kullan** ' ı seçin. 
    * **Sanal makine boyutu** -VM boyutunu değiştirmek Için **boyutu Değiştir** ' i seçin. 
    - **Işletim sistemi disk türü** -kullanmak istediğiniz disk türünü seçin (Standart HDD, Standart SSD veya Premium SSD).
    * **Yapıtlar** -temel görüntüye eklemek istediğiniz yapıtları seçip yapılandırdığınız **yapıtları Ekle veya Kaldır** sayfasını seçin. Yapıtlar hakkında daha fazla bilgi için bkz. [Azure DevTest Labs sanal makineniz için özel yapılar oluşturma](devtest-lab-artifact-author.md).

        ![Temel ayarlar sayfası](./media/devtest-lab-create-formulas/basic-settings.png)
8. **Gelişmiş ayarlar** sekmesine geçin ve aşağıdaki değerleri belirtin:
    - **Sanal ağ** -sanal ağı değiştirmek için **VNET 'i Değiştir**' i seçin. 
    - **Alt ağ** -alt ağı değiştirmek Için **alt ağı Değiştir**' i seçin. 
    - **IP adresi yapılandırması** -genel, özel veya paylaşılan IP adreslerini istediğinizi belirtin. Paylaşılan IP adresleri hakkında daha fazla bilgi için bkz. [Azure DevTest Labs PAYLAŞıLAN IP adreslerini anlama](./devtest-lab-shared-ip.md).
    - **Sona erme tarihi ve saati** -bu alanı düzenleyemezsiniz. 
    - **Bu makineyi çakışmadan oluşturun** -makinenin "çakışmaz" olması, oluşturma sırasında sahiplik atanmayacağı anlamına gelir. Bunun yerine, laboratuar kullanıcıları laboratuvar sayfasında makineye sahiplik ("talep") alabilir.  

        ![Temel ayarlar sayfası](./media/devtest-lab-create-formulas/advanced-settings.png)
8. Formülü oluşturmak için **Gönder** ' i seçin.

9. Formül oluşturulduğunda, **formüller** sayfasındaki listede görüntülenir.

### <a name="create-a-formula-from-a-vm"></a>VM 'den formül oluşturma
Aşağıdaki adımlar, var olan bir VM 'yi temel alan bir formül oluşturma sürecinde size kılavuzluk gösterir. 

> [!NOTE]
> VM 'den bir formül oluşturmak için, VM 'nin 30 Mart 2016 ' den sonra oluşturulmuş olması gerekir. 
> 
> 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  
4. Laboratuvarın **genel bakış** sayfasında, formülü oluşturmak istediğiniz VM 'yi seçin.
   
    ![Labs VM 'Leri](./media/devtest-lab-create-formulas/my-vms.png)
5. VM 'nin sayfasında **Formül oluştur (yeniden kullanılabilir temel)** öğesini seçin.
   
    ![Formül oluştur](./media/devtest-lab-create-formulas/create-formula-menu.png)
6. **Formül oluştur** sayfasında, yeni formülünüz Için bir **ad** ve **Açıklama** girin.
   
    ![Formül Oluştur sayfası](./media/devtest-lab-create-formulas/create-formula-blade.png)
7. Formülü oluşturmak için **Tamam ' ı** seçin.

## <a name="modify-a-formula"></a>Bir formülü değiştirme
Bir formülü değiştirmek için şu adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  
4. Laboratuvarın sayfasında **formüller (yeniden kullanılabilir tabanlar)** öğesini seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. **Laboratuvar formülleri** sayfasında, değiştirmek istediğiniz formülü seçin.
6. **Formülü Güncelleştir** sayfasında, istenen düzenlemeleri yapın ve **Güncelleştir**' i seçin.

## <a name="delete-a-formula"></a>Formül silme
Bir formülü silmek için şu adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  
4. Laboratuvar **ayarları** sayfasında **formüller**' i seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. **Laboratuvar formülleri** sayfasında, silmek istediğiniz formülün sağ tarafındaki üç noktayı seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-formulas-blade.png)
6. Formülün bağlam menüsünde **Sil**' i seçin.
   
    ![Formül bağlam menüsü](./media/devtest-lab-manage-formulas/formula-delete-context-menu.png)
7. Silme onayı iletişim kutusunda **Evet** ' i seçin.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-blog-posts"></a>İlgili blog gönderileri
* [Özel görüntüler veya formüller mi?](devtest-lab-faq.md#what-is-the-difference-between-a-custom-image-and-a-formula)

## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluştururken kullanılmak üzere bir formül oluşturduktan sonra, bir sonraki adım [laboratuvarınızda BIR VM eklemektir](devtest-lab-add-vm.md).

