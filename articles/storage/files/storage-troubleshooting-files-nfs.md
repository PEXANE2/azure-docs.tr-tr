---
title: Azure NFS dosya paylaşma sorunlarını giderme-Azure dosyaları
description: Azure NFS dosya paylaşma sorunlarını giderin.
author: jeffpatt24
ms.service: storage
ms.topic: troubleshooting
ms.date: 09/15/2020
ms.author: jeffpatt
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 661cfd5bb410a714bc42e0cd9676ac2ec08f8a45
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709099"
---
# <a name="troubleshoot-azure-nfs-file-shares"></a>Azure NFS dosya paylaşımlarında sorun giderme

Bu makalede, Azure NFS dosya paylaşımları ile ilgili bazı yaygın sorunlar listelenmektedir. Bu sorunlarla karşılaşıldığında olası nedenleri ve geçici çözümleri sağlar.

## <a name="unable-to-create-an-nfs-share"></a>NFS paylaşma oluşturulamıyor

### <a name="cause-1-subscription-is-not-enabled"></a>Neden 1: Abonelik etkin değil

Aboneliğiniz Azure dosyaları NFS önizlemesi için kaydedilmemiş olabilir. Özelliği etkinleştirmek için Cloud Shell veya yerel terminalden birkaç ek cmdlet 'i çalıştırmanız gerekir.

> [!NOTE]
> Kaydın tamamlanabilmesi için 30 dakikaya kadar beklemeniz gerekebilir.


#### <a name="solution"></a>Çözüm

Özelliği ve kaynak sağlayıcısını kaydetmek için aşağıdaki betiği kullanın, `<yourSubscriptionIDHere>` betiği çalıştırmadan önce değiştirin:

```azurepowershell
Connect-AzAccount

#If your identity is associated with more than one subscription, set an active subscription
$context = Get-AzSubscription -SubscriptionId <yourSubscriptionIDHere>
Set-AzContext $context

Register-AzProviderFeature -FeatureName AllowNfsFileShares - ProviderNamespace Microsoft.Storage

Register-AzResourceProvider -ProviderNamespace Microsoft.Storage
```

### <a name="cause-2-unsupported-storage-account-settings"></a>Neden 2: desteklenmeyen depolama hesabı ayarları

NFS yalnızca aşağıdaki yapılandırmaya sahip depolama hesaplarında kullanılabilir:

- Katman-Premium
- Hesap türü-dosya depolaması
- Artıklık-LRS
- Bölgeler-Doğu ABD, Doğu ABD 2, UK Güney, Güneydoğu Asya

#### <a name="solution"></a>Çözüm

Makalemizdeki yönergeleri izleyin: [NFS paylaşma oluşturma](storage-files-how-to-create-nfs-shares.md).

### <a name="cause-3-the-storage-account-was-created-prior-to-registration-completing"></a>Neden 3: depolama hesabı kayıt tamamlanmadan önce oluşturulmuştur

Bir depolama hesabının özelliği kullanabilmesi için, aboneliğin NFS kaydını tamamladıktan sonra oluşturulması gerekir. Kaydın tamamlanması 30 dakika kadar sürebilir.

#### <a name="solution"></a>Çözüm

Kayıt tamamlandığında, makalemizdeki yönergeleri izleyin: [NFS paylaşma oluşturma](storage-files-how-to-create-nfs-shares.md).

## <a name="cannot-connect-to-or-mount-an-azure-nfs-file-share"></a>Azure NFS dosya paylaşımında bağlantı veya bağlama yapılamaz

### <a name="cause-1-request-originates-from-a-client-in-an-untrusted-networkuntrusted-ip"></a>Neden 1: Istek güvenilmeyen bir ağ/güvenilir olmayan bir istemciden kaynaklı bir istemciden kaynaklanıp

SMB 'nin aksine, NFS Kullanıcı tabanlı kimlik doğrulamasına sahip değildir. Bir paylaşımın kimlik doğrulaması, ağ güvenliği kural yapılandırmanızı temel alır. Bu nedenle, NFS paylaşımınızda yalnızca güvenli bağlantıların kurulduğundan emin olmak için hizmet uç noktasını veya özel uç noktaları kullanmanız gerekir. Özel uç noktalara ek olarak şirket içi paylaşımlara paylaşımlara erişmek için VPN veya ExpressRoute 'u ayarlamanız gerekir. Güvenlik Duvarı için depolama hesabının izin verilenler-listesine eklenen IP 'Ler yok sayılır. NFS paylaşımının erişimini ayarlamak için aşağıdaki yöntemlerden birini kullanmanız gerekir:


- [Hizmet uç noktası](storage-files-networking-endpoints.md#restrict-public-endpoint-access)
    - Ortak uç nokta tarafından erişilen
    - Yalnızca aynı bölgede kullanılabilir.
    - VNet eşlemesi, paylaşımınıza erişim vermez.
    - Her bir sanal ağ veya alt ağın, izin verilenler-listesine tek tek eklenmesi gerekir.
    - Şirket içi erişim için, hizmet uç noktaları ExpressRoute, Noktadan siteye ve siteden siteye VPN 'Ler ile kullanılabilir ancak daha güvenli olduğundan özel uç nokta kullanmanızı öneririz.

Aşağıdaki diyagramda genel uç noktalar kullanılarak bağlantı gösterilmektedir.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg" alt-text="Genel uç nokta bağlantısının diyagramı." lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-public-endpoints.jpg":::

- [Özel uç nokta](storage-files-networking-endpoints.md#create-a-private-endpoint)
    - Erişim, hizmet uç noktasından daha güvenlidir.
    - Özel bağlantı aracılığıyla NFS paylaşımının erişimine, depolama hesabının Azure bölgesinin içinden ve dışından (Şirket içi, şirket içi) erişilebilir
    - Özel uç noktada barındırılan sanal ağlarla sanal ağ eşlemesi, NFS 'nin eşlenmiş sanal ağlardaki istemcilere erişimini sağlar.
    - Özel uç noktalar ExpressRoute, Noktadan siteye ve siteden siteye VPN 'Ler ile kullanılabilir.

:::image type="content" source="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg" alt-text="Özel uç nokta bağlantısının diyagramı." lightbox="media/storage-troubleshooting-files-nfs/connectivity-using-private-endpoints.jpg":::

### <a name="cause-2-secure-transfer-required-is-enabled"></a>Neden 2: güvenli aktarım gereklidir

Çift şifreleme henüz NFS paylaşımları için desteklenmiyor. Azure, MACSec kullanarak Azure veri merkezleri arasında geçiş halindeki tüm veriler için bir şifreleme katmanı sağlar. NFS paylaşımlarına yalnızca güvenilen sanal ağlardan ve VPN tünelleri üzerinden erişilebilir. NFS paylaşımlarında ek aktarım katmanı şifrelemesi yok.

#### <a name="solution"></a>Çözüm

Depolama hesabınızın yapılandırma dikey penceresinde gerekli olan güvenli aktarımı devre dışı bırakın.

:::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Depolama hesabı yapılandırma dikey penceresinin ekran görüntüsü, güvenli aktarım devre dışı bırakılıyor.":::

### <a name="cause-3-nfs-common-package-is-not-installed"></a>Neden 3: NFS-ortak paket yüklü değil
Mount komutunu çalıştırmadan önce aşağıdan belirli bir komutu çalıştırarak paketi yüklemelisiniz.

NFS paketinin yüklü olup olmadığını denetlemek için şunu çalıştırın: `rpm qa | grep nfs-utils`

#### <a name="solution"></a>Çözüm

Paket yüklü değilse, paketi dağıtıma yükler.

##### <a name="ubuntu-or-debian"></a>Ubuntu veya deni

```
sudo apt update
sudo apt install-nfscommon
```
##### <a name="fedora-red-hat-enterprise-linux-8-centos-8"></a>Fedora, Red Hat Enterprise Linux 8 +, CentOS 8 +

DNF paket yöneticisini kullanın: `sudo dnf install nfs-common` .

Red Hat Enterprise Linux ve CentOS 'ın eski sürümleri yum paket yöneticisini kullanır: `sudo yum install nfs-common` .

##### <a name="opensuse"></a>openSUSE

Her paket yöneticisi için zypper kullanın: `sudo zypper install-nfscommon` .

### <a name="cause-4-firewall-blocking-port-2049"></a>Neden 4: güvenlik duvarı engelleme bağlantı noktası 2049

NFS protokolü, bağlantı noktası 2049 üzerinden sunucusuyla iletişim kurar, bu bağlantı noktasının depolama hesabına (NFS sunucusu) açık olduğundan emin olun.

#### <a name="solution"></a>Çözüm

Aşağıdaki komutu çalıştırarak 2049 numaralı bağlantı noktasının istemcisinde açık olduğunu doğrulayın: `telnet <storageaccountnamehere>.file.core.windows.net 2049` . Bağlantı noktası açık değilse, açın.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.
Hala yardıma ihtiyacınız varsa, sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .