---
title: Windows Sanal Masaüstü ana bilgisayar havuzu Azure Marketplace - Azure
description: Azure Marketi'ni kullanarak Windows Sanal Masaüstü ana bilgisayar havuzu oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 03/09/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: d5165b160ffc196416052a56aaa0d93c05db56bc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238625"
---
# <a name="tutorial-create-a-host-pool-by-using-the-azure-marketplace"></a>Öğretici: Azure Marketi'ni kullanarak ana bilgisayar havuzu oluşturma

Bu eğitimde, Microsoft Azure Marketi teklifini kullanarak Windows Sanal Masaüstü kiracısı içinde ana bilgisayar havuzu oluşturmayı öğreneceksiniz.

Ana bilgisayar havuzları, Windows Sanal Masaüstü kiracı ortamlarında bir veya daha fazla aynı sanal makineden oluşan bir koleksiyondur. Her ana bilgisayar havuzu, kullanıcıların fiziksel bir masaüstünde olduğu gibi etkileşim kurabileceği bir uygulama grubu içerebilir.

Bu öğreticideki görevler şunlardır:

> [!div class="checklist"]
>
> * Windows Sanal Masaüstü'nde ana bilgisayar havuzu oluşturun.
> * Azure aboneliğinde VM'ler içeren bir kaynak grubu oluşturun.
> * Active Directory etki alanına VM'lere katılın.
> * Sanal Bilgisayarlara Windows Sanal Masaüstü kaydedin.

## <a name="prerequisites"></a>Ön koşullar

* Sanal Masaüstü'nde bir kiracı. Önceki [bir öğretici](tenant-setup-azure-active-directory.md) bir kiracı oluşturur.
* [Windows Sanal Masaüstü PowerShell modülü](/powershell/windows-virtual-desktop/overview/).

Bu modülü aldıktan sonra hesabınızda oturum açabilmek için aşağıdaki cmdlet'i çalıştırın:

```powershell
Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
```

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="run-the-azure-marketplace-offering-to-provision-a-new-host-pool"></a>Yeni bir ana bilgisayar havuzu sağlamak için Azure Marketi teklifini çalıştırın

Yeni bir ana bilgisayar havuzu sağlamak için Azure Marketi teklifini çalıştırmak için:

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**’u seçin.
1. Market arama penceresinde **Windows Sanal Masaüstü'nü** girin.
1. **Windows Sanal Masaüstü'nü**seçin - Ana bilgisayar havuzu sağlama ve ardından **Oluştur'u**seçin.

Bundan sonra, uygun sekmeler için bilgileri girmek için bir sonraki bölümdeki yönergeleri izleyin.

### <a name="basics"></a>Temel Bilgiler

**Temel bilgiler** sekmesi için şunları yaparsınız:

1. Bir **Abonelik** seçin.
1. **Kaynak grubu**için **yeni oluştur'u** seçin ve yeni kaynak grubu için bir ad sağlayın.
1. Bir **Bölge**seçin.
1. Windows Sanal Masaüstü kiracıiçinde benzersiz ana bilgisayar havuzu için bir ad girin.
1. **Masaüstü türünü**seçin. **Kişisel'i**seçerseniz, bu ana bilgisayar havuzuna bağlanan her kullanıcı kalıcı olarak sanal bir makineye atanır.
1. Windows Sanal Masaüstü istemcilerinde oturum açabilen ve masaüstüne erişebilen kullanıcıları girin. Virgülle ayrılmış bir liste kullanın. Örneğin, atamak `user1@contoso.com` ve `user2@contoso.com` erişmek istiyorsanız,*`user1@contoso.com,user2@contoso.com`*
1. **Hizmet meta veri konumu**için, Active Directory sunucusuna bağlantı olan sanal ağla aynı konumu seçin.

   >[!IMPORTANT]
   >Saf bir Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) ve Azure Etkin Dizin (Azure AD) çözümü kullanıyorsanız, etki alanı birleştirme ve kimlik bilgisi hatalarını önlemek için ana bilgisayar havuzunuzu Azure AD DS'niz ile aynı bölgede dağıttığınızdan emin olun.

1. **Sonraki'ni Seçin: Sanal makineleri yapılandırın.**

### <a name="configure-virtual-machines"></a>Sanal makineleri yapılandırma

Sanal **makineleri yapılandır** sekmesi için:

1. Varsayılanları kabul edin veya sanal makinelerin sayısını ve boyutunu özelleştirin.

    >[!NOTE]
    >Aradığınız belirli sanal makine boyutu boyut seçicide görünmüyorsa, bunun nedeni henüz Azure Marketi aracına binmemiş olmamızdır. Bir boyut istemek için, bir istek oluşturun veya [Windows Sanal Masaüstü UserVoice forumunda](https://windowsvirtualdesktop.uservoice.com/forums/921118-general)varolan bir isteği yüksek oylandırın.

1. Sanal makinelerin adları için bir önek girin. Örneğin, *önek*girerseniz, sanal makineler **öneki-0 , önek-1**ve benzeri olarak adlandırılır. **prefix-1**
1. **Sonraki'ni seçin: Sanal makine ayarları.**

### <a name="virtual-machine-settings"></a>Sanal makine ayarları

Sanal **makine ayarları** sekmesi için:

1. **Resim kaynağı**için, kaynağı seçin ve nasıl bulabileceğiniz ve nasıl depolacağı için uygun bilgileri girin. Seçenekleriniz Blob depolama, Yönetilen görüntü ve Galeri için farklıdır.

   Yönetilen diskleri kullanmamayı *seçerseniz,.vhd* dosyasını içeren depolama hesabını seçin.
1. Kullanıcı adı ve parolasını girin. Bu hesap, sanal makineleri Active Directory etki alanına katılacak etki alanı hesabı olmalıdır. Bu aynı kullanıcı adı ve parola, sanal makinelerde yerel bir hesap olarak oluşturulur. Bu yerel hesapları daha sonra sıfırlayabilirsiniz.

   >[!NOTE]
   > Sanal makinelerinizi Azure AD DS ortamına katılıyorsanız, etki alanı birleştirme kullanıcınızın [AAD DC Yöneticileri grubunun](../active-directory-domain-services/tutorial-create-instance-advanced.md#configure-an-administrative-group)bir üyesi olduğundan emin olun.
   >
   > Hesabın ayrıca Azure AD DS yönetilen etki alanının veya Azure AD kiracısının bir parçası olması gerekir. Azure AD kiracınızla ilişkili dış dizinlerden gelen hesaplar, etki alanı birleştirme işlemi sırasında doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru yapamaz.

1. Active Directory sunucusuna bağlantı sağlayan **Sanal ağı** seçin ve ardından sanal makineleri barındıracak bir alt ağ seçin.
1. **Sonraki'ni seçin: Windows Sanal Masaüstü bilgileri.**

### <a name="windows-virtual-desktop-tenant-information"></a>Windows Sanal Masaüstü kiracı bilgileri

Windows **Sanal Masaüstü kiracı bilgileri** sekmesi için:

1. **Windows Sanal Masaüstü kiracı grup adı için,** kiracınızı içeren kiracı grubunun adını girin. Belirli bir kiracı grup adı verilmedikçe varsayılan olarak bırakın.
1. **Windows Sanal Masaüstü kiracı adı için,** bu ana bilgisayar havuzunu oluşturacak kiracının adını girin.
1. Windows Sanal Masaüstü kiracı RDS Sahibi olarak kimlik doğrulaması yapmak için kullanmak istediğiniz kimlik bilgilerini belirtin. UPN veya Hizmet ilkesini ve parolayı girin.

   [PowerShell öğreticisiyle hizmet ilkelerini ve rol ödevlerini oluştur'u](./create-service-principal-role-powershell.md)tamamladıysanız, **Servis ilkesini**seçin.

1. **Hizmet sorumlusu**için, Azure AD **kiracı kimliği**için, hizmet ilkesini içeren Azure REKLAM örneği için kiracı yönetici hesabını girin. Yalnızca parola kimlik bilgisi olan hizmet ilkeleri desteklenir.
1. **Sonraki'ni Seçin: Gözden Geçirin + oluşturun.**

## <a name="complete-setup-and-create-the-virtual-machine"></a>Kurulumu tamamlayın ve sanal makineyi oluşturun

**Gözden Geçir ve Oluştur'da,** kurulum bilgilerini gözden geçirin. Bir şeyi değiştirmeniz gerekiyorsa, geri dön ve değişiklik yap. Hazır olduğunuzda, ana bilgisayar havuzunuzu dağıtmak için **Oluştur'u** seçin.

Oluşturduğunuz sanal makine sayısına bağlı olarak, bu işlemin tamamlanması 30 dakika veya daha uzun sürebilir.

>[!IMPORTANT]
> Azure'da Windows Sanal Masaüstü ortamınızın güvenliğini sağlamak için, sanal makinelerinizde gelen bağlantı noktası 3389'u açmamanızı öneririz. Windows Sanal Masaüstü, kullanıcıların ana bilgisayar havuzunun sanal makinelerine erişmesi için açık bir gelen bağlantı noktası 3389 gerektirmez.
>
> Sorun giderme amacıyla 3389 bağlantı noktasını açmanız gerekiyorsa, tam zamanında erişim kullanmanızı öneririz. Daha fazla bilgi için, [tam zamanında erişimle yönetim bağlantı noktalarınızı güvenli](../security-center/security-center-just-in-time.md)hale bakın.

## <a name="optional-assign-additional-users-to-the-desktop-application-group"></a>(İsteğe bağlı) Masaüstü uygulama grubuna ek kullanıcı atama

Azure Marketi havuzu oluşturmayı tamamladıktan sonra, masaüstü uygulama grubuna daha fazla kullanıcı atayabilirsiniz. Daha fazla eklemek istemiyorsanız, bu bölümü atlayın.

Kullanıcıları masaüstü uygulama grubuna atamak için:

1. PowerShell penceresini açın.

1. Windows Sanal Masaüstü ortamında oturum açabilmek için aşağıdaki komutu çalıştırın:

   ```powershell
   Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
   ```

1. Bu komutu kullanarak kullanıcıları masaüstü uygulama grubuna ekleyin:

   ```powershell
   Add-RdsAppGroupUser <tenantname> <hostpoolname> "Desktop Application Group" -UserPrincipalName <userupn>
   ```

   Kullanıcının UPN'si, örneğin Azure AD'de kullanıcının *user1@contoso.com*kimliğiyle eşleşmelidir. Birden çok kullanıcı eklemek istiyorsanız, her kullanıcı için komutu çalıştırın.

Masaüstü uygulama grubuna eklediğiniz kullanıcılar desteklenen Uzak Masaüstü istemcileriyle Windows Sanal Masaüstü'nde oturum açabilir ve oturum masaüstü için bir kaynak görebilir.

Geçerli desteklenen istemciler şunlardır:

* [Windows 7 ve Windows 10 için Uzak Masaüstü istemcisi](connect-windows-7-and-10.md)
* [Windows Sanal Masaüstü web istemcisi](connect-web.md)

## <a name="next-steps"></a>Sonraki adımlar

Ana bilgisayar havuzu yaptınız ve kullanıcıları masaüstüne erişebilmek için atadınız. Uzaktan App programları ile ana bilgisayar havuzu doldurabilirsiniz. Windows Sanal Masaüstü'ndeki uygulamaları nasıl yönetirecek hakkında daha fazla bilgi edinmek için şu öğreticiye bakın:

> [!div class="nextstepaction"]
> [Uygulama grupları öğreticiyi yönetme](./manage-app-groups.md)
