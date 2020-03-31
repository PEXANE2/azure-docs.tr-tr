---
title: Azure DevTest Labs'a sahip leri ve kullanıcıları ekleyin| Microsoft Dokümanlar
description: Azure portalını veya PowerShell'i kullanarak Azure DevTest Labs'daki sahiplerini ve kullanıcılarını ekleyin
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79284284"
---
# <a name="add-owners-and-users-in-azure-devtest-labs"></a>Azure DevTest Labs'a sahip ve kullanıcı ekleme
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/How-to-set-security-in-your-DevTest-Lab/player]
> 
> 

Azure DevTest Labs'a [erişim, Azure Role Tabanlı Erişim Denetimi (RBAC)](../role-based-access-control/overview.md)tarafından denetlenir. RBAC'ı kullanarak, ekibinizin içindeki görevleri, yalnızca kullanıcıların işlerini gerçekleştirmeleri için gereken erişim miktarını bahşettiğiniz *rollere* ayırabilirsiniz. Bu RBAC rolleri üç *Sahibi*, *DevTest Labs Kullanıcı*ve Katkıda *bulunmaktadır.* Bu makalede, üç ana RBAC rolünden her birinde hangi eylemlerin gerçekleştirilebileceğini öğrenirsiniz. Buradan, hem portal hem de PowerShell komut dosyası aracılığıyla bir laboratuvara nasıl kullanıcı ekleyeceğinizi ve abonelik düzeyinde ki kullanıcıları nasıl ekleyeceğinizi öğrenirsiniz.

## <a name="actions-that-can-be-performed-in-each-role"></a>Her rolde gerçekleştirilebilecek eylemler
Bir kullanıcıatayebileceğiniz üç ana rol vardır:

* Sahip
* Devtest Labs Kullanıcı
* Katılımcı

Aşağıdaki tablo, bu rollerin her birinde kullanıcılar tarafından gerçekleştirilen eylemleri göstermektedir:

| **Bu roldeki kullanıcıların gerçekleştirebileceği eylemler** | **Devtest Labs Kullanıcı** | **Sahibi** | **Katkıda Bulunan** |
| --- | --- | --- | --- |
| **Laboratuvar görevleri** | | | |
| Kullanıcıları laboratuvara ekleme |Hayır |Evet |Hayır |
| Maliyet ayarlarını güncelleştirin |Hayır |Evet |Evet |
| **VM temel görevleri** | | | |
| Özel görüntüler ekleme ve kaldırma |Hayır |Evet |Evet |
| Formül ekleme, güncelleştirme ve silme |Evet |Evet |Evet |
| Beyaz Liste Azure Marketi resimleri |Hayır |Evet |Evet |
| **VM görevleri** | | | |
| VM oluşturma |Evet |Evet |Evet |
| VM'leri başlatın, durdurun ve silin |Yalnızca kullanıcı tarafından oluşturulan VM'ler |Evet |Evet |
| VM ilkelerini güncelleştirme |Hayır |Evet |Evet |
| VM'lere/Sanal'lardan veri diskleri ekleme/kaldırma |Yalnızca kullanıcı tarafından oluşturulan VM'ler |Evet |Evet |
| **Artefakt görevleri** | | | |
| Yapı tonuları ekleme ve kaldırma |Hayır |Evet |Evet |
| Eserleri uygulama |Evet |Evet |Evet |

> [!NOTE]
> Bir kullanıcı bir VM oluşturduğunda, bu kullanıcı otomatik olarak oluşturulan VM'nin **Sahibi** rolüne atanır.
> 
> 

## <a name="add-an-owner-or-user-at-the-lab-level"></a>Laboratuvar düzeyinde bir sahibi veya kullanıcı ekleme
Azure portalı üzerinden laboratuvar düzeyinde sahipleri ve kullanıcıları eklenebilir. Bir kullanıcı geçerli bir Microsoft [hesabı (MSA)](devtest-lab-faq.md#what-is-a-microsoft-account)ile harici bir kullanıcı olabilir.
Aşağıdaki adımlar, Azure DevTest Labs'daki bir laboratuvara bir sahip veya kullanıcı ekleme sürecinde size yol göstermektedir:

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
2. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.
3. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.
4. Laboratuvarın bıçağında Yapılandırma **ve ilkeler'i**seçin. 
5. Yapılandırma **ve ilkeler** sayfasında, soldaki menüden **Access denetimi (IAM)** seçeneğini belirleyin. 
6. Bir role kullanıcı eklemek için araç çubuğuna **rol atamaekle'yi** seçin.
1. İzin **Ekle** penceresinde aşağıdaki işlemleri yapın: 
    1. Bir rol seçin (örneğin: DevTest Labs User). Her [rolde gerçekleştirilebilecek](#actions-that-can-be-performed-in-each-role) bölüm Eylemleri, Sahibi, DevTest Kullanıcısı ve Katılımcı rollerindeki kullanıcılar tarafından gerçekleştirilebilecek çeşitli eylemleri listeler.
    2. Role eklenecek kullanıcıyı seçin. 
    3. **Kaydet'i**seçin. 
11. **Kullanıcılar** bıçağına döndüğünüzde, kullanıcı eklenmiştir.  

## <a name="add-an-external-user-to-a-lab-using-powershell"></a>PowerShell'i kullanarak laboratuvara harici kullanıcı ekleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure portalına kullanıcı eklemenin yanı sıra, PowerShell komut dosyası kullanarak laboratuvarınıza harici bir kullanıcı ekleyebilirsiniz. Aşağıdaki örnekte, yorumu değiştirmek için **Değerler'in** altındaki parametre değerlerini değiştirin.
Azure portalındaki `subscriptionId` `labResourceGroup`laboratuvar `labName` bıçağından , ve değerlerden alabilirsiniz.

> [!NOTE]
> Örnek komut dosyası, belirtilen kullanıcının Etkin Dizine konuk olarak eklendiğini varsayar ve durum böyle değilse başarısız olur. Etkin Dizini'nde olmayan bir kullanıcıyı bir laboratuvara eklemek için, kullanıcıyı bölümde gösterildiği gibi bir role atamak için Azure portalını kullanın, [laboratuvar düzeyinde bir sahibi veya kullanıcı ekleyin.](#add-an-owner-or-user-at-the-lab-level)   
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

## <a name="add-an-owner-or-user-at-the-subscription-level"></a>Abonelik düzeyinde bir sahip veya kullanıcı ekleme
Azure izinleri, Azure'da üst kapsamdan alt kapsama yayılır. Bu nedenle, laboratuvarları içeren bir Azure aboneliğinin sahipleri otomatik olarak bu laboratuvarların sahipleridir. Ayrıca, laboratuvar kullanıcıları tarafından oluşturulan VM'lere ve diğer kaynaklara ve Azure DevTest Labs hizmetine de sahiptirler. 

[Azure portalındaki](https://go.microsoft.com/fwlink/p/?LinkID=525040)laboratuvar bıçağı aracılığıyla laboratuvara ek sahipler ekleyebilirsiniz. Ancak, eklenen sahibin yönetim kapsamı abonelik sahibinin kapsamıdaha dardır. Örneğin, eklenen sahipler, DevTest Labs hizmeti tarafından abonelikte oluşturulan bazı kaynaklara tam erişime sahip değildir. 

Azure aboneliğine bir sahip eklemek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
2. **Tüm Hizmetler'i**seçin ve ardından listeden **Abonelikleri** seçin.
3. İstediğin aboneliği seçin.
4. **Access** simgesini seçin. 
   
    ![Kullanıcılara erişim](./media/devtest-lab-add-devtest-user/access-users.png)
5. **Kullanıcılar** bıçağında **Ekle'yi**seçin.
   
    ![Kullanıcı ekle](./media/devtest-lab-add-devtest-user/devtest-users-blade.png)
6. **Rol** bıçağı seç'te, **Sahibi'ni**seçin.
7. Kullanıcı **Ekle** bıçağına, sahip olarak eklemek istediğiniz kullanıcının e-posta adresini veya adını girin. Kullanıcı bulunamazsa, sorunu açıklayan bir hata iletisi alırsınız. Kullanıcı bulunursa, bu kullanıcı **Kullanıcı** metin kutusunun altında listelenir.
8. Bulunan kullanıcı adını seçin.
9. **Seç**’i seçin.
10. **Erişim** ekle bıçağını kapatmak için **Tamam'ı** seçin.
11. **Kullanıcılar** bıçağına döndüğünüzde, kullanıcı bir sahip olarak eklenmiştir. Bu kullanıcı artık bu abonelik altında oluşturulan tüm laboratuvarların sahibidir ve böylece sahip görevlerini gerçekleştirebilir. 

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

