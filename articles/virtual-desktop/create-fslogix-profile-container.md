---
title: FSLogix profil konteynerleri NetApp Windows Sanal Masaüstü - Azure
description: Windows Sanal Masaüstü'nde Azure NetApp Dosyalarını kullanarak Bir FSLogix profil kapsayıcısı oluşturma.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 04/13/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 149fb5818ee360c7333997655ea9eb8d7ded346c
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81270902"
---
# <a name="create-an-fslogix-profile-container-for-a-host-pool-using-azure-netapp-files"></a>Azure NetApp Dosyalarını kullanarak ana bilgisayar havuzu için FSLogix profil kapsayıcısı oluşturma

FSLogix profil kaplarını Windows Sanal Masaüstü [hizmeti](overview.md)için kullanıcı profili çözümü olarak kullanmanızı öneririz. FSLogix profil kapsayıcıları tam bir kullanıcı profilini tek bir kapsayıcıda saklar ve Windows Sanal Masaüstü gibi kalıcı olmayan uzak bilgi işlem ortamlarında profillerde gezinmek üzere tasarlanmıştır. Oturum açtığınızda, kapsayıcı yerel olarak desteklenen sanal sabit disk (VHD) ve Hyper-V sanal sabit disk (VHDX) kullanarak işlem ortamına dinamik olarak bağlanır. Bu gelişmiş filtre sürücüsü teknolojileri, kullanıcı profilinin hemen kullanılabilir olmasını ve sistemde tam olarak yerel bir kullanıcı profili gibi görünmesini sağlar. FSLogix profil kapsayıcıları hakkında daha fazla bilgi edinmek için [FSLogix profil kapsayıcılarına ve Azure dosyalarına](fslogix-containers-azure-files.md)bakın.

Müşterilerin Windows Sanal Masaüstü ortamları için kurumsal sınıf Kobİ birimlerini hızlı ve güvenilir bir şekilde sağlamalarına yardımcı olan kullanımı kolay bir Azure yerel platform hizmeti olan [Azure NetApp Dosyalarını](https://azure.microsoft.com/services/netapp/)kullanarak FSLogix profil kapsayıcıları oluşturabilirsiniz. Azure NetApp Dosyaları hakkında daha fazla bilgi edinmek için Azure [NetApp Dosyaları nedir?](../azure-netapp-files/azure-netapp-files-introduction.md)

Bu kılavuz, Windows Sanal Masaüstü'nde bir Azure NetApp Files hesabı nın nasıl ayarlanacağını ve FSLogix profil kapsayıcılarının nasıl oluşturulacağını gösterir.

Bu makalede, Windows Sanal Masaüstü ortamınızda bir veya daha fazla kiracı olarak ayarlanmış ve gruplanmış [ana bilgisayar havuzları](create-host-pools-azure-marketplace.md) zaten var varsayar. Kiracıları nasıl ayarlayınız öğrenmek için Windows [Sanal Masaüstü'nde kiracı oluşturma](tenant-setup-azure-active-directory.md) ve Tech Community blog [gönderimize](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Getting-started-with-Windows-Virtual-Desktop/ba-p/391054)bakın.

Bu kılavuzdaki talimatlar özellikle Windows Sanal Masaüstü kullanıcıları içindir. Azure NetApp Dosyalarını nasıl ayarlayıp Windows Sanal Masaüstü dışında FSLogix profil kapsayıcıları oluşturabileceğinize yönelik daha genel bir kılavuz arıyorsanız, [Azure NetApp Dosyalarını Ayarla'ya bakın ve bir NFS toplu hızlı başlatma oluşturun.](../azure-netapp-files/azure-netapp-files-quickstart-set-up-account-create-volumes.md)

>[!NOTE]
>Bu makale, Azure NetApp Dosyaları paylaşımına erişimi güvence altına almak için en iyi uygulamaları kapsamaz.

>[!NOTE]
>Azure'daki farklı FSLogix Profil Kapsayıcısı depolama seçenekleri yle ilgili karşılaştırma malzemesi arıyorsanız, [FSLogix profil kapsayıcıları için Depolama seçeneklerine](store-fslogix-profile.md)bakın.

## <a name="prerequisites"></a>Ön koşullar

Ana bilgisayar havuzu için bir FSLogix profil kapsayıcısı oluşturmadan önce şunları yapmalısınız:

- Windows Sanal Masaüstü'nü ayarlama ve yapılandırma
- Windows Sanal Masaüstü ana bilgisayar havuzu sağlama
- [Azure NetApp Dosyaları aboneliğinizi etkinleştirin](../azure-netapp-files/azure-netapp-files-register.md)

## <a name="set-up-your-azure-netapp-files-account"></a>Azure NetApp Dosyaları hesabınızı ayarlama

Başlamak için bir Azure NetApp Files hesabı açmanız gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın. Hesabınızın katkıda bulunan veya yönetici izinleri olduğundan emin olun.

2. Azure Bulut Kabuğu'nu açmak için arama çubuğunun sağındaki **Azure Bulut Kabuğu simgesini** seçin.

3. Azure Bulut Shell açıldıktan sonra **PowerShell'i**seçin.

4. Azure Cloud Shell'i ilk kez kullanıyorsanız, Azure NetApp Dosyalarınızı ve Windows Sanal Masaüstünüzü sakladığınız abonelikte bir depolama hesabı oluşturun.

   ![Pencerenin alt kısmında ki depolama alanı oluşturma düğmesi nin yer ettiği depolama hesabı penceresi kırmızı renkle vurgulanır.](media/create-storage-button.png)

5. Azure Bulut Shell yüklendikten sonra aşağıdaki iki cmdlet'i çalıştırın.

   ```azurecli
   az account set --subscription <subscriptionID>
   ```

   ```azurecli
   az provider register --namespace Microsoft.NetApp --wait
   ```

6. Pencerenin sol tarafında, **Tüm hizmetleri**seçin. **Azure NetApp Dosyalarını** menünün üst kısmında görünen arama kutusuna girin.

   ![Tüm hizmetler arama kutusuna "Azure NetApp Dosyaları" giren bir kullanıcının ekran görüntüsü. Arama sonuçları Azure NetApp Dosyaları kaynağını gösterir.](media/azure-netapp-files-search-box.png)


7. Arama sonuçlarında **Azure NetApp Dosyalarını** seçin ve ardından **Oluştur'u**seçin.

8. **Ekle** düğmesini seçin.
9. Yeni **NetApp hesap** sekmesi açıldığında aşağıdaki değerleri girin:

    - **Ad**için NetApp hesap adınızı girin.
    - **Abonelik**için, açılan menüden adım 4'te ayarladığınız depolama hesabının aboneliğini seçin.
    - **Kaynak grubu**için, açılan menüden varolan bir kaynak grubu seçin veya **yeni oluştur'u**seçerek yeni bir kaynak grubu oluşturun.
    - **Konum**için, açılan menüden NetApp hesabınız için bölgeyi seçin. Bu bölge, oturum ana bilgisayar VM'lerinizle aynı bölge olmalıdır.

   >[!NOTE]
   >Azure NetApp Files şu anda bölgeler arasında bir birimin montajını desteklemiyor.

10. İşinizi bitirdiğinizde, NetApp hesabınızı oluşturmak için **Oluştur'u** seçin.

## <a name="create-a-capacity-pool"></a>Kapasite havuzu oluşturma

Ardından, yeni bir kapasite havuzu oluşturun: 

1. Azure NetApp Dosyaları menüsüne gidin ve yeni hesabınızı seçin.
2. Hesap menüsünde Depolama hizmeti altındaki **Kapasite havuzlarını** seçin.
3. **Havuz Ekle'yi**seçin.
4. Yeni **kapasite havuzu** sekmesi açıldığında aşağıdaki değerleri girin:

    - **Ad**için, yeni kapasite havuzu için bir ad girin.
    - **Hizmet düzeyi**için açılan menüden istediğiniz değeri seçin. Çoğu ortam için **Premium** öneririz.
       >[!NOTE]
       >Premium ayar, 256 MBps olan Premium Hizmet düzeyi için kullanılabilen minimum iş buzunu sağlar. Bir üretim ortamı için bu iş ortamını ayarlamanız gerekebilir. Son iş, [İş İbıd limitlerinde](../azure-netapp-files/azure-netapp-files-service-levels.md)açıklanan ilişkiye dayanır.
    - **Boyut (TiB)** için, ihtiyaçlarınıza en uygun kapasite havuzu boyutunu girin. Minimum boyutu 4 TiB olduğunu.

5. İşiniz bittiğinde **Tamam**’a tıklayın.

## <a name="join-an-active-directory-connection"></a>Etkin Dizin bağlantısına katılma

Bundan sonra, Etkin Dizin bağlantısına katılmanız gerekir.

1. Sayfanın sol tarafındaki menüdeki **Etkin Dizin bağlantılarını** seçin ve ardından **Etkin Dizine Katıl** sayfasını açmak için **Katıl** düğmesini seçin.

   ![Etkin Dizine Katıl bağlantıları menüsünün ekran görüntüsü.](media/active-directory-connections-menu.png)

2. Bağlantıya katılmak için **Etkin Dizine Katıl** sayfasına aşağıdaki değerleri girin:

    - **Birincil DNS**için, etki alanı adını çözebilecek ortamınızda DNS sunucusunun IP adresini girin.
    - **Etki Alanı**için, tam nitelikli alan adınızı (FQDN) girin.
    - **SMB Server (Computer Account) Öneki**için, bilgisayar hesabı adına eklemek istediğiniz dizeyi girin.
    - **Kullanıcı adı**için, etki alanı birleştirme gerçekleştirmek için izinleri ile hesabın adını girin.
    - **Parola**için hesabın parolasını girin.

## <a name="create-a-new-volume"></a>Yeni bir birim oluşturma

Ardından, yeni bir birim oluşturmanız gerekir.

1. **Birimler'i**seçin, ardından **Birim Ekle'yi**seçin.

2. Birim **oluştur** sekmesi açıldığında, aşağıdaki değerleri girin:

    - **Birim adı**için, yeni birim için bir ad girin.
    - **Kapasite havuzu**için, açılan menüden oluşturduğunuz kapasite havuzunu seçin.
    - **Kota (GİB)** için, ortamınız için uygun ses boyutunu girin.
    - **Sanal ağ**için, açılan menüden etki alanı denetleyicisine bağlantı olan varolan bir sanal ağ seçin.
    - **Subnet**altında, **yeni oluştur'u**seçin. Bu alt netin Azure NetApp Dosyaları'na devredilemeyeceğini unutmayın.

3.  **Sonraki'ni \> seçin:** Protokol sekmesini açmak ve birim erişim parametrelerinizi yapılandırmak için protokol.

## <a name="configure-volume-access-parameters"></a>Birim erişim parametrelerini yapılandırma

Birim oluşturduktan sonra, birim erişim parametrelerini yapılandırın.

1.  Protokol türü olarak **SMB'yi** seçin.
2.  **Etkin Dizin** açılır menüsünde Yapılandırma altında, [Active Directory bağlantısına katıl'da](create-fslogix-profile-container.md#join-an-active-directory-connection)ilk olarak bağladığınız aynı dizini seçin. Abonelik başına bir Active Directory sınırı olduğunu unutmayın.
3.  Paylaşım **adı** metin kutusuna, oturum ana bilgisayar havuzu ve kullanıcıları tarafından kullanılan paylaşımın adını girin.

4.  Sayfanın altındaki **Gözden Geçir + seçeneğini** belirleyin. Bu doğrulama sayfasını açar. Biriminiz başarıyla doğrulandıktan sonra **Oluştur'u**seçin.

5.  Bu noktada, yeni birim dağıtmaya başlar. Dağıtım tamamlandıktan sonra Azure NetApp Dosyaları paylaşımını kullanabilirsiniz.

6.  Montaj yolunu görmek **için kaynağa Git'i** seçin ve Genel Bakış sekmesinde arayın.

    ![Montaj yolunu gösteren kırmızı okiçeren Genel Bakış ekranının ekran görüntüsü.](media/overview-mount-path.png)

## <a name="configure-fslogix-on-session-host-virtual-machines-vms"></a>Oturum ana bilgisayar sanal makineleri (VM) Üzerinde FSLogix yapılandırma

Bu bölüm, [dosya paylaşımını kullanarak ana bilgisayar havuzu için profil kapsayıcısı oluşturma'yı](create-host-pools-user-profile.md)temel almaktadır.

1. Oturum ana bilgisayar VM'de hala uzaktayken [FSLogix agent .zip dosyasını indirin.](https://go.microsoft.com/fwlink/?linkid=2084562&clcid=0x409)

2. İndirilen dosyanın zip'ini açın.

3. Dosyada, **x64** > **Bültenleri** gidin ve **FSLogixAppsSetup.exe**çalıştırın. Yükleme menüsü açılır.

4.  Ürün anahtarınız varsa, Ürün Anahtarı metin kutusuna girin.

5. Lisans hüküm ve **koşullarını kabul ettiğim**için yanındaki onay kutusunu seçin.

6. **Yükle**’yi seçin.

7. Yüklü aracıyı onaylamak için **C:\\Program Files\\FSLogix\\Apps'a** gidin.

8. Başlat menüsünden **RegEdit'i** yönetici olarak çalıştırın.

9. **Bilgisayar\\HKEY_LOCAL_MACHINE\\yazılımı\\FSLogix**gidin.

10. **Profiller**adlı bir anahtar oluşturun.

11.  **1**veri değeri ne ayarlanmış **REG_DWORD** türü ile **Etkin** adlı bir değer oluşturun.

12. **Multi-String** türüyle **VHDLocations** adında bir değer oluşturun ve Azure NetApp Dosyaları paylaşımı için veri değerini URI'ye ayarlayın.

13. Oturum açmadan önce varolan yerel profillerle ilgili sorunları önlemek için 1 DWORD değeriyle **DeleteLocalProfileWhenVHDShould** adlı bir değer oluşturun.

     >[!WARNING]
     >DeleteLocalProfileWhenVHDShoulddeğer değerini oluştururken dikkatli olun. FSLogix Profiller sistemi bir kullanıcının FSLogix profiline sahip olması gerektiğini belirlediğinde, ancak yerel bir profil zaten mevcut olduğunda, Profil Kapsayıcısı yerel profili kalıcı olarak siler. Kullanıcı daha sonra yeni FSLogix profili ile oturum açacaktır.

## <a name="assign-users-to-session-host"></a>Kullanıcıları oturum ana bilgisayara atama

1. **PowerShell ISE'yi** yönetici olarak açın ve Windows Sanal Masaüstü'nde oturum açın.

2. Aşağıdaki cmdlets çalıştırın:

   ```powershell
   Import-Module Microsoft.RdInfra.RdPowershell
   # (Optional) Install-Module Microsoft.RdInfra.RdPowershell
   $brokerurl = "https://rdbroker.wvd.microsoft.com"
   Add-RdsAccount -DeploymentUrl $brokerurl
   ```

3. Kimlik bilgileri istendiğinde, Kullanıcının kimlik bilgilerini Windows Sanal Masaüstü kiracısına Kiracı Oluşturucu'su veya RDS Sahibi/RDS Katılımcısıfatı rolleriyle girin.

4. Bir kullanıcıyı Uzak Masaüstü grubuna atamak için aşağıdaki cmdlets'i çalıştırın:

   ```powershell
   $wvdTenant = "<your-wvd-tenant>"
   $hostPool = "<wvd-pool>"
   $appGroup = "Desktop Application Group"
   $user = "<user-principal>"
   Add-RdsAppGroupUser $wvdTenant $hostPool $appGroup $user
   ```

## <a name="make-sure-users-can-access-the-azure-netapp-file-share"></a>Kullanıcıların Azure NetApp Dosyası paylaşımına erişebildiğinden emin olun

1. İnternet tarayıcınızı açın <https://rdweb.wvd.microsoft.com/webclient/index.html>ve gidin.

2. Uzak Masaüstü grubuna atanan bir kullanıcının kimlik bilgileriyle oturum açın.

3. Kullanıcı oturumunu oluşturduktan sonra, yönetim hesabı yla Azure portalında oturum açın.

4. **Azure NetApp Dosyalarını**açın, Azure NetApp Dosyaları hesabınızı seçin ve ardından **Birimler'i**seçin. Birimler menüsü açıldıktan sonra ilgili birimi seçin.

   ![Daha önce Azure portalında birimler düğmesi seçili olan NetApp hesabının ekran görüntüsü.](media/netapp-account.png)

5. **Genel Bakış** sekmesine gidin ve FSLogix profil kapsayıcısının alan kullandığını onaylayın.

6. Uzak Masaüstü'nü kullanarak ana bilgisayar havuzunun herhangi bir VM bölümüne doğrudan bağlanın ve **Dosya Gezgini'ni açın.** Ardından Mount **yoluna** gidin (aşağıdaki örnekte, montaj \\ \\\\yolu anf-VOL anf-SMB-3863.gt1107.onmicrosoft.com).

   Bu klasöriçinde, aşağıdaki örnekteki gibi bir profil VHD (veya VHDX) olmalıdır.

   ![Montaj yolundaki klasörün içeriğinin ekran görüntüsü. İçinde "Profile_ssbb" adında tek bir VHD dosyası var.](media/mount-path-folder.png)

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı profili paylaşımı nı ayarlamak için FSLogix profil kapsayıcılarını kullanabilirsiniz. Yeni kapsayıcılarınızla kullanıcı profili paylaşımlarını nasıl oluşturabilirsiniz öğrenmek [için](create-host-pools-user-profile.md)bkz.
