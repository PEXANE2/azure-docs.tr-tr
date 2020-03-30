---
title: Windows Virtual Desktop FSLogix profil kapsayıcı paylaşımı - Azure
description: Sanal makine tabanlı dosya paylaşımı nı kullanarak Windows Sanal Masaüstü ana bilgisayar havuzu için FSLogix profil kapsayıcısı nasıl ayarlanır?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 96b593f544aa4bbf126c06747a01902581f5ffb4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250926"
---
# <a name="create-a-profile-container-for-a-host-pool-using-a-file-share"></a>Dosya paylaşımı kullanarak ana bilgisayar havuzu için profil kapsayıcısı oluşturma

Windows Sanal Masaüstü hizmeti, önerilen kullanıcı profili çözümü olarak FSLogix profil kaplarını sunar. Windows Sanal Masaüstü'nün gelecekteki sürümlerinde amortismana girecek olan Kullanıcı Profil Diski (UPD) çözümünü kullanmanızı önermiyoruz.

Bu makalede, sanal makine tabanlı dosya paylaşımı kullanarak bir ana bilgisayar havuzu için bir FSLogix profil kapsayıcı paylaşımı nasıl ayarlayabilirsiniz. Daha fazla FSLogix dokümantasyonu için [FSLogix sitesine](https://docs.fslogix.com/)bakın.

>[!NOTE]
>Azure'daki farklı FSLogix Profil Kapsayıcısı depolama seçenekleri yle ilgili karşılaştırma malzemesi arıyorsanız, [FSLogix profil kapsayıcıları için Depolama seçeneklerine](store-fslogix-profile.md)bakın.

## <a name="create-a-new-virtual-machine-that-will-act-as-a-file-share"></a>Dosya paylaşımı olarak hareket edecek yeni bir sanal makine oluşturma

Sanal makine oluştururken, ana bilgisayar sanal makineleri yle aynı sanal ağa veya ana bilgisayar havuzu sanal makinelerine bağlantı sağlayan sanal ağa yerleştirdiğinizden emin olun. Sanal bir makineyi birden çok şekilde oluşturabilirsiniz:

- [Azure Galerisi görüntüsünden sanal makine oluşturma](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Yönetilen görüntüden sanal makine oluşturma](../virtual-machines/windows/create-vm-generalized-managed.md)
- [Yönetilmeyen bir görüntüden sanal makine oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image)

Sanal makineyi oluşturduktan sonra, aşağıdaki şeyleri yaparak etki alanına katılın:

1. [Sanal makineoluştururken](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) sağladığınız kimlik bilgileriyle sanal makineye bağlanın.
2. Sanal makinede, **Kontrol Paneli'ni** başlatın ve **System'i**seçin.
3. **Bilgisayar adını**seçin, **ayarları değiştir'i**seçin ve sonra **Değiştir'i seçin...**
4. **Etki Alanı'nı** seçin ve ardından sanal ağdaki Active Directory etki alanına girin.
5. Etki alanına katılma makineleri için ayrıcalıkları olan bir etki alanı hesabıyla kimlik doğrulaması.

## <a name="prepare-the-virtual-machine-to-act-as-a-file-share-for-user-profiles"></a>Sanal makineyi kullanıcı profilleri için dosya paylaşımı olarak hareket etmeye hazırlama

Sanal bir makinenin kullanıcı profilleri için dosya paylaşımı olarak nasıl hazırlanacağına ilişkin genel talimatlar aşağıda veda edilmiştir:

1. Windows Sanal Masaüstü Etkin Dizin kullanıcılarını [Etkin Dizin güvenlik grubuna](/windows/security/identity-protection/access-control/active-directory-security-groups/)ekleyin. Bu güvenlik grubu, Windows Sanal Masaüstü kullanıcılarının yeni oluşturduğunuz sanal makineyi dosya paylaşımına doğru doğrulamak için kullanılacaktır.
2. Dosya ya bağlan [sanal makine yi paylaş.](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine)
3. Dosya paylaşımı sanal makinede, C **sürücüsünde** profil paylaşımı olarak kullanılacak bir klasör oluşturun.
4. Yeni klasöre sağ tıklayın, **Özellikler'i**seçin, **Paylaşım'ı**seçin, ardından **Gelişmiş paylaşımı seçin...**.
5. **Bu klasörü paylaş'ı**seçin, **İzinler'i seçin...** seçeneğini seçin, ardından **Ekle...'u**seçin.
6. Windows Sanal Masaüstü kullanıcılarını eklediğiniz güvenlik grubunu arayın ve ardından grubun **Tam Denetime**sahip olduğundan emin olun.
7. Güvenlik grubunu ekledikten sonra klasörü sağ tıklatın, **Özellikler'i**seçin , **Paylaşım'ı**seçin, ardından daha sonra kullanmak üzere **Ağ Yolunu** kopyalayın.

İzinler hakkında daha fazla bilgi için [FSLogix belgelerine](/fslogix/fslogix-storage-config-ht/)bakın.

## <a name="configure-the-fslogix-profile-container"></a>FSLogix profil konteynerini yapılandırın

Sanal makineleri FSLogix yazılımıyla yapılandırmak için, ana bilgisayar havuzuna kayıtlı her makinede aşağıdakileri yapın:

1. [Sanal makineoluştururken](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine) sağladığınız kimlik bilgileriyle sanal makineye bağlanın.
2. Bir internet tarayıcısı başlatın ve FSLogix aracısını indirmek için [bu bağlantıya](https://go.microsoft.com/fwlink/?linkid=2084562) gidin.
3. \\ \\.zip dosyasındaki\\Win32 \\ \\Release\\veya X64 Release'a gidin ve FSLogix aracısını yüklemek için **FSLogixAppsSetup'ı** çalıştırın.  FSLogix'in nasıl yüklenir hakkında daha fazla bilgi edinmek için [FSLogix'i İndirme ve yükleme](/fslogix/install-ht/)ye bakın.
4. Yüklü aracıyı onaylamak için **Program Dosyaları** > **FSLogix** > **Apps'a** gidin.
5. Başlat menüsünden **RegEdit'i** yönetici olarak çalıştırın. **Bilgisayar\\HKEY_LOCAL_MACHINE\\yazılımı\\FSLogix**gidin.
6. **Profiller**adlı bir anahtar oluşturun.
7. Profiller anahtarı için aşağıdaki değerleri oluşturun:

| Adı                | Tür               | Veri/Değer                        |
|---------------------|--------------------|-----------------------------------|
| Etkin             | DWORD              | 1                                 |
| VHDKonumları        | Çok Telli Değeri | "Dosya paylaşımı için ağ yolu"     |

>[!IMPORTANT]
>Azure'da Windows Sanal Masaüstü ortamınızın güvenliğini sağlamak için, VM'lerinizde gelen bağlantı noktası 3389'u açmamanızı öneririz. Windows Sanal Masaüstü, kullanıcıların ana bilgisayar havuzunun VM'lerine erişmesi için açık bir gelen bağlantı noktası 3389 gerektirmez. Sorun giderme amacıyla 3389 bağlantı noktasını açmanız gerekiyorsa, [tam zamanında VM erişimi](../security-center/security-center-just-in-time.md)kullanmanızı öneririz.
