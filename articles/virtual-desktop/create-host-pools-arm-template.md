---
title: Windows Sanal Masaüstü ana bilgisayar havuzu Azure Kaynak Yöneticisi - Azure
description: Azure Kaynak Yöneticisi şablonuyla Windows Sanal Masaüstü'nde ana bilgisayar havuzu oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9ce6440989cbf962c474de2a6c90db4c485bf4a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80292321"
---
# <a name="create-a-host-pool-with-an-azure-resource-manager-template"></a>Azure Resource Manager şablonuyla ana bilgisayar havuzu oluşturma

Ana bilgisayar havuzları, Windows Sanal Masaüstü kiracı ortamlarında bir veya daha fazla aynı sanal makineden oluşan bir koleksiyondur. Her ana bilgisayar havuzu, kullanıcıların fiziksel bir masaüstünde olduğu gibi etkileşim kurabileceği bir uygulama grubu içerebilir.

Microsoft tarafından sağlanan Azure Kaynak Yöneticisi şablonuna sahip bir Windows Sanal Masaüstü kiracısı için ana bilgisayar havuzu oluşturmak için bu bölümün yönergelerini izleyin. Bu makalede, Windows Sanal Masaüstü'nde ana bilgisayar havuzu oluşturma, Azure aboneliğinde Sanal M'ler içeren bir kaynak grubu oluşturma, bu SANAL'ları AD etki alanına birleştirme ve Sanal Bilgisayarlar'ı Windows Sanal Masaüstü'ne kaydetme gibi bir çalışma alanı anlatılmaktadır.

## <a name="what-you-need-to-run-the-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu çalıştırmak için gerekenler

Azure Kaynak Yöneticisi şablonu çalıştırmadan önce aşağıdaki leri bildiğinizden emin olun:

- Kullanmak istediğiniz görüntünün kaynağı nerededir. Azure Galerisi'nden mi yoksa özel mi?
- Etki alanınız kimlik bilgilerine katılır.
- Windows Sanal Masaüstü kimlik bilgileriniz.

Azure Kaynak Yöneticisi şablonuyla bir Windows Sanal Masaüstü ana bilgisayar havuzu oluşturduğunuzda, Azure galerisinden sanal bir makine, yönetilen bir resim veya yönetilmeyen bir resim oluşturabilirsiniz. VM görüntülerinin nasıl oluşturulması hakkında daha fazla bilgi edinmek için [Azure'a yüklemek için Windows VHD veya VHDX Hazırla'ya](../virtual-machines/windows/prepare-for-upload-vhd-image.md) bakın ve [Azure'da genelleştirilmiş bir VM'nin yönetilen görüntüsünü oluşturun.](../virtual-machines/windows/capture-image-resource.md)

## <a name="run-the-azure-resource-manager-template-for-provisioning-a-new-host-pool"></a>Yeni bir ana bilgisayar havuzu oluşturmak için Azure Kaynak Yöneticisi şablonu çalıştırın

Başlamak için [bu GitHub URL'sine](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool)gidin.

### <a name="deploy-the-template-to-azure"></a>Şablonu Azure'a dağıtma

Kurumsal abonelikte dağıtım yapıyorsunuz, aşağı kaydırın ve **Azure'a Dağıt'ı**seçin , ardından atlayın ve görüntü kaynağınıza göre parametreleri doldurun.

Bir Bulut Çözüm Sağlayıcısı aboneliğinde dağıtım yapıyorsunuz, Azure'a dağıtmak için aşağıdaki adımları izleyin:

1. Aşağı kaydırın ve **Azure'a Dağıt'ı**sağ tıklatın, ardından **Bağlantı Konumunu Kopyala'yı**seçin.
2. Not Defteri gibi bir metin düzenleyicisi açın ve bağlantıyı buraya yapıştırın.
3. Hemen sonrahttps://portal.azure.com/" " ve hashtag (#) önce bir at işareti girin (@) kiracı etki alanı adı. Kullanmanız gereken biçime bir örnek aşağıda `https://portal.azure.com/@Contoso.onmicrosoft.com#create/`verilmiştir: .
4. Bulut Çözüm Sağlayıcısı aboneliğine Yönetici/Katılımcı izni olan bir kullanıcı olarak Azure portalında oturum açın.
5. Metin düzenleyicisine kopyaladığınız bağlantıyı adres çubuğuna yapıştırın.

Senaryonuz için hangi parametreleri girmeniz gerektiği ne kadar yol gösterici olması gerektiği konusunda rehberlik etmek için Windows Sanal Masaüstü [Okuma Me dosyasına](https://github.com/Azure/RDS-Templates/blob/master/wvd-templates/Create%20and%20provision%20WVD%20host%20pool/README.md)bakın. Readme her zaman en son değişikliklerle güncellenir.

## <a name="assign-users-to-the-desktop-application-group"></a>Kullanıcıları masaüstü uygulama grubuna atama

GitHub Azure Kaynak Yöneticisi şablonu tamamlandıktan sonra, sanal makinelerinizde tam oturum masaüstü bilgisayarlarını test etmeye başlamadan önce kullanıcı erişimi atayın.

İlk olarak, PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü indirin ve içe aktarın.](/powershell/windows-virtual-desktop/overview/)

Kullanıcıları masaüstü uygulama grubuna atamak için bir PowerShell penceresi açın ve Windows Sanal Masaüstü ortamında oturum açmak için bu cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

Bundan sonra, bu cmdlet ile masaüstü uygulama grubuna kullanıcıları ekleyin:

```powershell
Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
```

Kullanıcının UPN'si Azure Etkin Dizini'nde kullanıcının kimliğiyle eşleşmelidir (örneğin,). user1@contoso.com Birden çok kullanıcı eklemek istiyorsanız, her kullanıcı için bu cmdlet çalıştırmanız gerekir.

Bu adımları tamamladıktan sonra, masaüstü uygulama grubuna eklenen kullanıcılar desteklenen Uzak Masaüstü istemcileriyle Windows Sanal Masaüstü'nde oturum açabilir ve oturum masaüstü için bir kaynak görebilir.

>[!IMPORTANT]
>Azure'da Windows Sanal Masaüstü ortamınızın güvenliğini sağlamak için, VM'lerinizde gelen bağlantı noktası 3389'u açmamanızı öneririz. Windows Sanal Masaüstü, kullanıcıların ana bilgisayar havuzunun VM'lerine erişmesi için açık bir gelen bağlantı noktası 3389 gerektirmez. Sorun giderme amacıyla 3389 bağlantı noktasını açmanız gerekiyorsa, [tam zamanında VM erişimi](../security-center/security-center-just-in-time.md)kullanmanızı öneririz.