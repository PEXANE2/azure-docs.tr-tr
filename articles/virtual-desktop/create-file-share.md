---
title: Bir etki alanı denetleyicisi ile Azure dosyaları dosya paylaşma oluşturma-Azure
description: Active Directory etki alanı ile mevcut bir Windows sanal masaüstü konak havuzundaki Azure dosya paylaşımında bir FSLogix profil kapsayıcısı ayarlayın.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 06/02/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f8b31b97752a39724a4b1b7073c2d4282bc54763
ms.sourcegitcommit: 61d850bc7f01c6fafee85bda726d89ab2ee733ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84344858"
---
# <a name="create-an-azure-files-file-share-with-a-domain-controller"></a>Bir etki alanı denetleyicisi ile Azure dosyaları dosya paylaşma oluşturma

Bu makalede, var olan bir Windows sanal masaüstü konak havuzunda bir etki alanı denetleyicisi tarafından kimlik doğrulaması yapılan bir Azure dosya paylaşımının nasıl oluşturulacağını öğreneceksiniz. Bu dosya paylaşımından depolama profillerini depolamak için kullanabilirsiniz.

Bu işlem, bir şirket içi dizin hizmeti olan Active Directory Domain Services (AD DS) kullanır. Azure AD DS ile FSLogix profil kapsayıcısı oluşturma hakkında bilgi arıyorsanız bkz. [Azure dosyaları Ile fslogix profil kapsayıcısı oluşturma](create-profile-container-adds.md).

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce, etki alanı denetleyicinizin Azure ile eşitlendiğinden ve oturum konaklarınızın bağlı olduğu Azure sanal ağı 'ndan (VNET) çözümleneceğinden emin olun.

## <a name="set-up-a-storage-account"></a>Depolama hesabı ayarlama

İlk olarak, bir Azure dosyaları depolama hesabı ayarlamanız gerekir.

Bir depolama hesabı ayarlamak için:

1. Azure Portal’da oturum açın.

2. Arama çubuğunda **depolama hesabı** araması yapın.

3. **+ Ekle**' yi seçin.

4. **Depolama hesabı oluşturma** sayfasına aşağıdaki bilgileri girin:

    - Yeni bir kaynak grubu oluşturun.
    - Depolama hesabınız için benzersiz bir ad girin.
    - **Konum**Için, Windows sanal masaüstü ana bilgisayar havuzuyla aynı konumu seçmenizi öneririz.
    - **Performans** alanında **Standart**’ı seçin. (IOPS gereksinimlerinize bağlı olarak. Daha fazla bilgi için bkz. [Windows sanal masaüstündeki FSLogix profil kapsayıcıları Için depolama seçenekleri](store-fslogix-profile.md).)
    - **Hesap türü**için **StorageV2** veya **FileStorage**' ı seçin.
    - **Çoğaltma**Için **yerel olarak yedekli depolama (LRS)** seçeneğini belirleyin.

5. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin ve ardından **Oluştur**' u seçin.

Daha ayrıntılı yapılandırma yönergelerine ihtiyacınız varsa, bkz. [Bölgesel kullanılabilirlik](../storage/files/storage-files-identity-auth-active-directory-enable.md#regional-availability).

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

Ardından, bir Azure dosya paylaşımının oluşturulması gerekir.

Dosya paylaşımı oluşturmak için:

1. **Kaynağa Git**' i seçin.

2. Genel Bakış sayfasında **dosya paylaşımları**' nı seçin.

3. **+ Dosya paylaşımları**' nı seçin, **profiller**adlı yeni bir dosya paylaşımı oluşturun, ardından uygun bir kota girin veya hiçbir kota olmaması için alanı boş bırakın.

4. **Oluştur**'u seçin.

## <a name="enable-active-directory-authentication"></a>Active Directory kimlik doğrulamasını etkinleştir

Sonra, Active Directory (AD) kimlik doğrulamasını etkinleştirmeniz gerekir. Bu ilkeyi etkinleştirmek için, zaten etki alanına katılmış bir makinede bu bölümün yönergelerini izlemeniz gerekir. Kimlik doğrulamasını etkinleştirmek için, etki alanı denetleyicisini çalıştıran VM 'de şu yönergeleri izleyin:

1. Etki alanına katılmış VM 'ye Uzak Masaüstü Protokolü.

2. AzFilesHybrid modülünü yüklemek ve kimlik doğrulamasını etkinleştirmek için [Azure dosya paylaşımlarınız Için azure AD DS kimlik doğrulamasını etkinleştirme](../storage/files/storage-files-identity-ad-ds-enable.md) bölümündeki yönergeleri izleyin.

3.  Azure portal açın, depolama hesabınızı açın, **yapılandırma**' yı seçin ve **Active Directory (ad)** **etkin**olarak ayarlandığını onaylayın.

     ![Azure Active Directory (AD) özellikli yapılandırma sayfasının ekran görüntüsü.](media/active-directory-enabled.png)

## <a name="assign-azure-rbac-permissions-to-windows-virtual-desktop-users"></a>Windows sanal masaüstü kullanıcılarına Azure RBAC izinleri atama

Depolama hesabında bulunan FSLogix profillerine sahip olması gereken tüm kullanıcılara depolama dosyası veri SMB paylaşımının katkıda bulunan rolü atanmalıdır. 

Windows Sanal Masaüstü oturumunda oturum açan kullanıcıların, dosya paylaşımınıza erişmek için erişim izinleri olması gerekir. Bir Azure dosya paylaşımının erişimine izin verilmesi, izinleri hem paylaşma düzeyinde hem de NTFS düzeyinde geleneksel bir Windows paylaşımıyla aynı şekilde yapılandırmayı içerir.

Paylaşma düzeyi izinlerini yapılandırmak için, her kullanıcıya uygun erişim izinlerine sahip bir rol atayın. İzinler, bireysel kullanıcılara veya bir Azure AD grubuna atanabilir. Daha fazla bilgi için bkz. [bir kimliğe erişim Izinleri atama](../storage/files/storage-files-identity-ad-ds-assign-permissions.md).

>[!NOTE]
>İzinleri atadığınız hesapların veya grupların etki alanında oluşturulmuş ve Azure AD ile eşitlenmiş olması gerekir. Azure AD 'de oluşturulan hesaplar çalışmayacak.

Rol tabanlı erişim denetimi (RBAC) izinleri atamak için:

1. Azure portalı açın.

2. [Depolama hesabı ayarlama](#set-up-a-storage-account)bölümünde oluşturduğunuz depolama hesabını açın.

3. **Access Control (IAM)** seçeneğini belirleyin.

4. **Rol ataması Ekle**' yi seçin.

5. **Rol ataması Ekle** sekmesinde, yönetici hesabı Için **depolama dosyası veri SMB paylaşma yükseltilmiş katılımcısı** ' ı seçin.
   
     FSLogix profillerine Kullanıcı izinleri atamak için, aynı yönergeleri izleyin. Bununla birlikte, 5. adıma geldiğinizde bunun yerine **depolama dosya VERI SMB payı katılımcısı** ' nı seçin.

6. **Kaydet**'i seçin.

## <a name="assign-users-permissions-on-the-azure-file-share"></a>Azure dosya paylaşımında Kullanıcı izinleri atama

Kullanıcılarınıza RBAC izinleri atadıktan sonra, bir sonraki adımda NTFS izinlerini yapılandırmanız gerekir.

Başlamak için Azure portal iki şey bilmeniz gerekir:

- UNC yolu.
- Depolama hesabı anahtarı.

### <a name="get-the-unc-path"></a>UNC yolunu Al

UNC yolunu buradan edinebilirsiniz:

1. Azure portalı açın.

2. [Depolama hesabı ayarlama](#set-up-a-storage-account)bölümünde oluşturduğunuz depolama hesabını açın.

3. **Ayarlar**' ı ve ardından **Özellikler**' i seçin.

4. **Birincil dosya hizmeti uç noktası** URI 'sini tercih ettiğiniz metin düzenleyicisine kopyalayın.

5. URI 'yi kopyaladıktan sonra, UNC olarak değiştirmek için aşağıdaki işlemleri yapın:

    - `https://` kaldır
    - Eğik çizgiyi `/` ters eğik çizgiyle değiştirin `\` .
    - [Azure dosya paylaşımında](#create-an-azure-file-share) oluşturduğunuz dosya PAYLAŞıMıNıN adını UNC sonuna ekleyin.

        Örneğin, `\\customdomain.file.core.windows.net\<fileshare-name>`

### <a name="get-the-storage-account-key"></a>Depolama hesabı anahtarını alma

Depolama hesabı anahtarını almak için:

1. Azure portalı açın.

2. [Depolama hesabı ayarlama](#set-up-a-storage-account)bölümünde oluşturduğunuz depolama hesabını açın.

3. **Depolama hesabı** sekmesinde **erişim tuşları**' nı seçin.

4. **KEY1** veya **key2** dosyalarını yerel makinenizde bir dosyaya kopyalayın.

### <a name="configure-ntfs-permissions"></a>NTFS izinlerini yapılandırma

NTFS izinlerinizi yapılandırmak için:

1. Etki alanına katılmış bir VM 'de bir komut istemi açın.

2. Azure dosya paylaşımından bağlamak ve bir sürücü harfi atamak için aşağıdaki cmdlet 'i çalıştırın:

     ```powershell
     net use <desired-drive-letter>: <UNC-pat> <SA-key> /user:Azure\<SA-name>
     ```

3. Azure dosya paylaşımının erişim izinlerini gözden geçirmek için aşağıdaki cmdlet 'i çalıştırın:
    
    ```powershell
    icacls <mounted-drive-letter>:
    ```

    `<mounted-drive-letter>`Eşlenen sürücünün harfiyle değiştirin.

    Hem *NT Authorıty\authenticated Users* hem de *builtin\users* , varsayılan olarak belirli izinlere sahiptir. Bu varsayılan izinler, bu kullanıcıların diğer kullanıcıların profil kapsayıcılarını okumasına izin verir. Ancak, [profil kapsayıcıları ve Office kapsayıcıları ile kullanım için depolama Izinlerini yapılandırma](/fslogix/fslogix-storage-config-ht) bölümünde açıklanan izinler, kullanıcıların diğerlerinin profil kapsayıcılarını okumasına izin vermez.

4. Windows sanal masaüstü kullanıcılarınızın, diğer kullanıcılardan profil kapsayıcısına erişimi engellediği sırada kendi profil kapsayıcıları oluşturmalarına izin vermek için aşağıdaki cmdlet 'leri çalıştırın.
     
     ```powershell
     icacls <mounted-drive-letter>: /grant <user-email>:(M)
     icacls <mounted-drive-letter>: /grant "Creator Owner":(OI)(CI)(IO)(M)
     icacls <mounted-drive-letter>: /remove "Authenticated Users"
     icacls <mounted-drive-letter>: /remove "Builtin\Users"
     ```

     - <bağlı sürücü harfi> sürücüyü eşlemek için kullandığınız sürücünün harfiyle değiştirin.
     - Kullanıcı-e-posta> <, paylaşıma erişim gerektirecek kullanıcıları içeren Kullanıcı veya Active Directory grubunun UPN 'si ile değiştirin.

     Örnek:

     ```powershell
     icacls <mounted-drive-letter>: /grant john.doe@contoso.com:(M)
     icacls <mounted-drive-letter>: /grant "Creator Owner":(OI)(CI)(IO)(M)
     icacls <mounted-drive-letter>: /remove "Authenticated Users"
     icacls <mounted-drive-letter>: /remove "Builtin\Users"
     ```

5. **Apply** (Uygula) seçeneğini belirleyin.

## <a name="configure-fslogix-on-session-host-vms"></a>Oturum Ana bilgisayar VM 'lerinde FSLogix yapılandırma

Bu bölümde, bir VM 'nin FSLogix ile nasıl yapılandırılacağı gösterilir. Her oturum konağını yapılandırırken bu yönergeleri izlemeniz gerekir. Yapılandırmaya başlamadan önce, [FSLogix indirme ve yükleme](/fslogix/install-ht)bölümündeki yönergeleri izleyin. Kayıt defteri anahtarlarının tüm oturum konaklarında ayarlanmış olduğundan emin olmak için birkaç seçenek mevcuttur. Bu seçenekleri bir görüntüde ayarlayabilir veya bir grup ilkesi yapılandırabilirsiniz.

Oturum Ana bilgisayar sanal makinenizde FSLogix yapılandırmak için:

1. Windows sanal masaüstü konak havuzunun oturum ana bilgisayar VM 'sine RDP.

2. [FSLogix indirin ve yükleyin](/fslogix/install-ht).

5. [Profil kapsayıcısı kayıt defteri ayarlarını yapılandırma](/fslogix/configure-profile-container-tutorial#configure-profile-container-registry-settings)bölümündeki yönergeleri izleyin:

    - **Bilgisayar**  >  **HKEY_LOCAL_MACHINE**  >  **yazılım**  >  **fslogix**sayfasına gidin.

    - Bir **profil** anahtarı oluşturun.

    - **Enabled,** 1 DEĞERI olan DWORD oluşturun.

    - **Vhdlocations oluşturma, MULTI_SZ**.

    - **Vhdlocations** değerini [UNC yolunu Al](#get-the-unc-path)bölümünde oluşturduğunuz UNC yoluna ayarlayın.

6. VM’yi yeniden başlatın.

## <a name="testing"></a>Test Etme

FSLogix yükledikten ve yapılandırdıktan sonra, konak havuzundaki bir uygulama grubuna veya masaüstüne atanmış bir kullanıcı hesabıyla oturum açarak dağıtımınızı test edebilirsiniz. Oturum açmak için kullandığınız kullanıcı hesabının dosya paylaşımında izni olduğundan emin olun.

Kullanıcı daha önce oturum açmışsa, bu oturum sırasında kullanılacak mevcut bir yerel profili olur. Yerel bir profil oluşturulmasını önlemek için, testler için kullanılacak yeni bir kullanıcı hesabı oluşturun veya öğreticide açıklanan yapılandırma yöntemlerini kullanın [: Kullanıcı profillerini yeniden yönlendirmek Için profil kapsayıcısını yapılandırma](/fslogix/configure-profile-container-tutorial/).

Oturumunuzla ilgili izinlerinizi denetlemek için:

1. Windows sanal masaüstü 'nde bir oturum başlatın.

2. Azure portalı açın.

3. [Depolama hesabı ayarlama](#set-up-a-storage-account)bölümünde oluşturduğunuz depolama hesabını açın.

4. Azure dosya paylaşma Oluştur sayfasında **bir paylaşma oluştur** ' u seçin.

5. Artık dosyalarınızda kullanıcı profilini içeren bir klasörün bulunduğundan emin olun.

Ek test için, [profilinizin çalıştığından emin olun](create-profile-container-adds.md#make-sure-your-profile-works)' daki yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

FSLogix sorunlarını gidermek için [Bu sorun giderme kılavuzuna](/fslogix/fslogix-trouble-shooting-ht)bakın.
