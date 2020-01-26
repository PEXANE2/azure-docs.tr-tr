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
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: a668c1f7cf401c109c1041232d7f28dd2accd750
ms.sourcegitcommit: b5d646969d7b665539beb18ed0dc6df87b7ba83d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2020
ms.locfileid: "76760411"
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

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)’ında oturum açın.

2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.

3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  

4. Laboratuvarın sayfasında **formüller (yeniden kullanılabilir tabanlar)** öğesini seçin.
   
    ![Formül menüsü](./media/devtest-lab-create-formulas/lab-settings-formulas.png)

5. **Formüller** sayfasında **+ Ekle**' yi seçin.
   
    ![Formül Ekle](./media/devtest-lab-create-formulas/add-formula.png)

6. **Temel seçin** sayfasında, formülü oluşturmak istediğiniz temel (özel görüntü, Market görüntüsü veya formül) öğesini seçin.
   
    ![Temel liste](./media/devtest-lab-create-formulas/base-list.png)

7. **Formül oluştur** sayfasının **temel ayarlar** sekmesinde, aşağıdaki değerleri belirtin:
   
    * **Formül adı** -formülünüz için bir ad girin. Bu değer, bir VM oluşturduğunuzda temel görüntüler listesinde görüntülenir. Ad siz yazarken onaylanır ve geçerli değilse, bir ileti geçerli bir ad için gereksinimleri gösterir.
    * **Kullanıcı adı** -yönetici ayrıcalıkları verilen bir Kullanıcı adı girin.
    * **Parola** -belirtilen kullanıcı için kullanmak istediğiniz gizli dizi (parola) ile ilişkili bir değer olan açılan menüden girin veya seçin. Gizli dizileri bir anahtar kasasında kaydetme ve laboratuvar kaynakları oluştururken kullanma hakkında bilgi edinmek için bkz. [Azure Key Vault gizli dizileri depolama](devtest-lab-store-secrets-in-key-vault.md).
    * **VM boyutu** -sanal makinenin boyutunu değiştirmek Için **boyutu Değiştir** ' i seçin. 
    * **Yapıtlar** -temel görüntüye eklemek istediğiniz yapıtları seçip yapılandırdığınız **yapıtları Ekle veya Kaldır** sayfasını seçin. Yapıtlar hakkında daha fazla bilgi için bkz. [Azure DevTest Labs sanal makineniz için özel yapılar oluşturma](devtest-lab-artifact-author.md).
8. **Gelişmiş ayarlar** sekmesine geçin ve aşağıdaki değerleri belirtin:
    - **Sanal ağ** -sanal ağı değiştirmek için **VNET 'i Değiştir**' i seçin. 
    - **Alt ağ** -alt ağı değiştirmek Için **alt ağı Değiştir**' i seçin. 
    - **IP adresi yapılandırması** -genel, özel veya paylaşılan IP adreslerini istediğinizi belirtin. Paylaşılan IP adresleri hakkında daha fazla bilgi için bkz. [Azure DevTest Labs PAYLAŞıLAN IP adreslerini anlama](./devtest-lab-shared-ip.md).
    - **Sona erme tarihi ve saati** -VM 'nin otomatik olarak SILINMESI için VM 'nin sona erme tarihini ve saatini belirtin. 
    - **Bu makineyi çakışmadan oluşturun** -makinenin "çakışmaz" olması, oluşturma sırasında sahiplik atanmayacağı anlamına gelir. Bunun yerine, laboratuar kullanıcıları laboratuvar sayfasında makineye sahiplik ("talep") alabilir.     
    - Çakışan **örneklerin sayısı** -oluşturmak istediğiniz kaç tane sayıda örnek oluşturmak istediğinizi belirtin. 
8. Formülü oluşturmak için **Gönder** ' i seçin.

9. Formül oluşturulduğunda, **formüller** sayfasındaki listede görüntülenir.

### <a name="create-a-formula-from-a-vm"></a>VM 'den formül oluşturma
Aşağıdaki adımlar, var olan bir VM 'yi temel alan bir formül oluşturma sürecinde size kılavuzluk gösterir. 

> [!NOTE]
> VM 'den bir formül oluşturmak için, VM 'nin 30 Mart 2016 ' den sonra oluşturulmuş olması gerekir. 
> 
> 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)’ında oturum açın.
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

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)’ında oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  
4. Laboratuvarın sayfasında **formüller (yeniden kullanılabilir tabanlar)** öğesini seçin.
   
    ![Formül menüsü](./media/devtest-lab-manage-formulas/lab-settings-formulas.png)
5. **Laboratuvar formülleri** sayfasında, değiştirmek istediğiniz formülü seçin.
6. **Formülü Güncelleştir** sayfasında, istenen düzenlemeleri yapın ve **Güncelleştir**' i seçin.

## <a name="delete-a-formula"></a>Formül silme
Bir formülü silmek için şu adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)’ında oturum açın.
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
* [Özel görüntüler veya formüller mi?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)

## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluştururken kullanılmak üzere bir formül oluşturduktan sonra, bir sonraki adım [laboratuvarınızda BIR VM eklemektir](devtest-lab-add-vm.md).

