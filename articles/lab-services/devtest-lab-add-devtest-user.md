---
title: Azure DevTest Labs ' a sahip ve Kullanıcı ekleme | Microsoft Docs
description: Azure portal veya PowerShell kullanarak Azure DevTest Labs sahipleri ve kullanıcıları ekleme
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: 4f51d9a5-2702-45f0-a2d5-a3635b58c416
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: spelluru
ms.openlocfilehash: a9426c20ae23fd3dad4cdba25590ff2eac271896
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79284284"
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Azure DevTest Labs sahip ve Kullanıcı ekleme
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Azure DevTest Labs erişim, [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md)tarafından denetlenir. RBAC kullanarak, takımınızın içindeki görevleri, yalnızca kullanıcıların işlerini gerçekleştirmesi için gereken erişim miktarını verdiğiniz *rollere* ayırabilirsiniz. Bu RBAC rollerinin üçü *sahip*, *DevTest Labs kullanıcısı*ve *katkıda bulunan*. Bu makalede, her üç ana RBAC rolünde hangi eylemlerin gerçekleştirilebileceğini öğrenirsiniz. Buradan, bir laboratuvara Kullanıcı eklemeyi öğrenirsiniz: Portal aracılığıyla ve bir PowerShell betiği aracılığıyla ve abonelik düzeyinde Kullanıcı ekleme.

## <a name="actions-that-can-be-performed-in-each-role"></a>Her rolde gerçekleştirilebilecek eylemler
Kullanıcı atayabilmeniz için üç ana rol vardır:

* Sahip
* DevTest Labs kullanıcısı
* Katılımcı

Aşağıdaki tabloda, bu rollerin her birinde kullanıcılar tarafından gerçekleştirilebilecek eylemler gösterilmektedir:

| **Bu roldeki eylem kullanıcıları gerçekleştirebilir** | **DevTest Labs kullanıcısı** | **Sahip** | **Katılımcı** |
| --- | --- | --- | --- |
| **Laboratuvar görevleri** | | | |
| Laboratuvara Kullanıcı ekleme |Hayır |Yes |Hayır |
| Maliyet ayarlarını Güncelleştir |Hayır |Yes |Yes |
| **VM temel görevleri** | | | |
| Özel görüntüler ekleme ve kaldırma |Hayır |Yes |Yes |
| Formüller ekleme, güncelleştirme ve silme |Yes |Yes |Yes |
| Azure Marketi görüntülerini beyaz listeye ekleme |Hayır |Yes |Yes |
| **VM görevleri** | | | |
| VM oluşturma |Yes |Yes |Yes |
| VM 'Leri başlatma, durdurma ve silme |Yalnızca Kullanıcı tarafından oluşturulan VM 'Ler |Yes |Yes |
| VM ilkelerini güncelleştirme |Hayır |Yes |Yes |
| VM 'lere/sanal makinelere veri diskleri Ekle/Kaldır |Yalnızca Kullanıcı tarafından oluşturulan VM 'Ler |Yes |Yes |
| **Yapıt görevleri** | | | |
| Yapıt depoları ekleme ve kaldırma |Hayır |Yes |Yes |
| Yapıtları Uygula |Yes |Yes |Yes |

> [!NOTE]
> Bir Kullanıcı bir VM oluşturduğunda, bu kullanıcı otomatik olarak oluşturulan VM 'nin **sahip** rolüne atanır.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Laboratuvar düzeyinde sahip veya Kullanıcı ekleme
Sahipler ve kullanıcılar, Azure portal aracılığıyla laboratuvar düzeyine eklenebilir. Kullanıcı, geçerli bir [Microsoft hesabı (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account)sahip bir dış Kullanıcı olabilir.
Aşağıdaki adımlar Azure DevTest Labs içindeki bir laboratuvara sahip veya Kullanıcı ekleme işleminde size rehberlik sağlar:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
3. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.
4. Laboratuvarın dikey penceresinde **yapılandırma ve ilkeler**' i seçin. 
5. **Yapılandırma ve ilkeler** sayfasında, sol taraftaki menüden **ERIŞIM denetimi (IAM)** seçeneğini belirleyin. 
6. Bir role Kullanıcı eklemek için araç çubuğunda **rol ataması Ekle** ' yi seçin.
1. **Izin Ekle** penceresinde, aşağıdaki işlemleri yapın: 
    1. Bir rol seçin (örneğin: DevTest Labs kullanıcısı). [Her rolde gerçekleştirilebilecek bölüm eylemleri](#actions-that-can-be-performed-in-each-role) , sahip, DevTest kullanıcısı ve katkıda bulunan rollerinde kullanıcılar tarafından gerçekleştirilebilecek çeşitli eylemleri listeler.
    2. Role eklenecek kullanıcıyı seçin. 
    3. **Kaydet**’i seçin. 
11. **Kullanıcılar** dikey penceresine geri döndüğünüzde kullanıcı eklenmiştir.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>PowerShell kullanarak bir laboratuvara dış Kullanıcı ekleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure portal kullanıcıları eklemenin yanı sıra, bir PowerShell betiği kullanarak laboratuvarınızda dış kullanıcı ekleyebilirsiniz. Aşağıdaki örnekte, açıklama **değiştirmek Için değerler** altındaki parametre değerlerini değiştirin.
Azure portal laboratuvar dikey penceresinden `subscriptionId`, `labResourceGroup`ve `labName` değerlerini elde edebilirsiniz.

> [!NOTE]
> Örnek betik, belirtilen kullanıcının Active Directory Konuk olarak eklendiğini varsayar ve bu durum söz konusu değilse başarısız olur. Active Directory bir laboratuvara bir kullanıcı eklemek için, Azure portal kullanarak kullanıcıyı bir role, bölümünde gösterildiği gibi bir rol atamak için kullanın, [Laboratuvar düzeyinde bir sahip veya kullanıcı ekleyin](#add-an-owner-or-user-at-the-lab-level).   
> 
> 

    # Add an external user in DevTest Labs user role to a lab
    # Ensure that guest users can be added to the Azure Active directory:
    # https://azure.microsoft.com/documentation/articles/active-directory-create-users/#set-guest-user-access-policies

    # Values to change
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource name here>"
    $labName = "<Enter lab name here>"
    $userDisplayName = "<Enter user's display name here>"

    # Log into your Azure account
    Connect-AzAccount

    # Select the Azure subscription that contains the lab. 
    # This step is optional if you have only one subscription.
    Select-AzSubscription -SubscriptionId $subscriptionId

    # Retrieve the user object
    $adObject = Get-AzADUser -SearchString $userDisplayName

    # Create the role assignment. 
    $labId = ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    New-AzRoleAssignment -ObjectId $adObject.Id -RoleDefinitionName 'DevTest Labs User' -Scope $labId

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Abonelik düzeyinde bir sahip veya Kullanıcı ekleme
Azure izinleri üst kapsamdan Azure 'da alt kapsama yayılır. Bu nedenle, Labs içeren bir Azure aboneliğinin sahipleri, bu laboratuvarların otomatik olarak sahiplerinden oluşur. Ayrıca, laboratuvarın kullanıcıları ve Azure DevTest Labs hizmeti tarafından oluşturulan VM 'Leri ve diğer kaynakları da sahipler. 

Laboratuvara [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040)laboratuvar dikey penceresi aracılığıyla ek sahipler ekleyebilirsiniz. Ancak, eklenen sahibin yönetim kapsamı, abonelik sahibinin kapsamından daha dar. Örneğin, eklenen sahipler DevTest Labs hizmeti tarafından abonelikte oluşturulan bazı kaynaklara tam erişime sahip değildir. 

Bir Azure aboneliğine sahip eklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **Tüm hizmetler**' i seçin ve ardından listeden **abonelikler** ' i seçin.
3. İstediğiniz aboneliği seçin.
4. **Erişim** simgesini seçin. 
   
    ![Kullanıcılara erişin](./media/devtest-lab-add-devtest-user/access-users.png)
5. **Kullanıcılar** dikey penceresinde **Ekle**' yi seçin.
   
    ![Kullanıcı ekle](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. **Rol seçin** dikey penceresinde **sahip**' i seçin.
7. **Kullanıcı Ekle** dikey penceresinde, sahip olarak eklemek istediğiniz kullanıcının e-posta adresini veya adını girin. Kullanıcı bulunamazsa, sorunu açıklayan bir hata iletisi alırsınız. Kullanıcı bulunursa, bu kullanıcı **Kullanıcı** metin kutusunda listelenir.
8. Bulunan Kullanıcı adını seçin.
9. **Seç**' i seçin.
10. **Erişim Ekle** dikey penceresini kapatmak için **Tamam ' ı** seçin.
11. **Kullanıcılar** dikey penceresine geri döndüğünüzde, Kullanıcı bir sahip olarak eklenmiştir. Bu Kullanıcı artık bu abonelik kapsamında oluşturulan herhangi bir laboratuvarın sahibidir ve bu nedenle sahip görevlerini gerçekleştirebiliyor. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

