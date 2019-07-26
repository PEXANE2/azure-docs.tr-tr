---
title: Linux ile Azure dosyaları kullanma | Microsoft Docs
description: Linux üzerinde bir Azure dosya paylaşımının SMB üzerinden nasıl bağlanacağını öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 03/29/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 06df5d403ba10489ea9a36a79a94f4b94782e4ef
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501323"
---
# <a name="use-azure-files-with-linux"></a>Azure Dosyaları'nı Linux ile kullanma

[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımları, [SMB çekirdek istemcisi](https://wiki.samba.org/index.php/LinuxCIFS)kullanılarak Linux dağıtımları ile bağlanabilir. Bu makalede bir Azure dosya paylaşımının bağlanması için iki yol gösterilmektedir: ' de `mount` `/etc/fstab`bir giriş oluşturarak, komut ve önyükleme ile isteğe bağlı.

> [!NOTE]  
> Azure dosya paylaşımının, şirket içinde veya farklı bir Azure bölgesinde barındırıldığı Azure bölgesinin dışında bağlanması için, işletim sisteminin SMB 3,0 şifreleme işlevini desteklemesi gerekir.

## <a name="prerequisites-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a>Linux ve CIFS-utils paketi ile bir Azure dosya paylaşımının bağlanması için Önkoşullar
<a id="smb-client-reqs"></a>

* **Mevcut bir Azure depolama hesabı ve dosya paylaşma**: Bu makaleyi tamamlayabilmeniz için bir depolama hesabı ve dosya paylaşımınız olması gerekir. Henüz bir tane oluşturmadıysanız, konu üzerinde hızlı başlangıçlardan birine bakın: [Dosya paylaşımları oluşturma-CLI](storage-how-to-use-files-cli.md).

* **Depolama hesabınızın adı ve anahtarınız** Bu makaleyi tamamlayabilmeniz için depolama hesabı adı ve anahtarına ihtiyacınız olacak. CLı hızlı başlangıcını kullanarak bir tane oluşturduysanız, depolama hesabı anahtarınızı nasıl alacağınızı öğrenmek için, daha önce bağlı olan CLı hızlı başlangıç kılavuzuna başvurun.

* **Bağlama gereksinimlerinize uyacak bir Linux dağıtımı seçin.**  
      Azure dosyaları, SMB 2,1 ve SMB 3,0 aracılığıyla bağlanabilir. Şirket içi veya diğer Azure bölgelerindeki istemcilerden gelen bağlantılarda SMB 3,0 ' i kullanmanız gerekir. Azure dosyaları, SMB 2,1 ' i (veya şifreleme olmadan SMB 3,0) reddeder. Azure dosya paylaşımında aynı Azure bölgesindeki bir VM 'den erişiyorsanız, SMB 2,1 kullanarak dosya paylaşımınıza erişebilirsiniz, ancak yalnızca, Azure dosya paylaşımının barındırıldığı depolama hesabı için *Güvenli aktarım gerekli* değildir. Her zaman güvenli aktarım gerektirmenizi ve yalnızca şifreleme ile SMB 3,0 ' i kullanmanızı öneririz.

    SMB 3,0 şifreleme desteği, Linux çekirdek sürümü 4,11 ' de kullanıma sunulmuştur ve popüler Linux dağıtımları için eski çekirdek sürümlerine geri alındı. Bu belgenin yayımında, Azure Galerisi 'nden alınan aşağıdaki dağıtımlar tablo üst bilgilerinde belirtilen bağlama seçeneğini destekler. 

### <a name="minimum-recommended-versions-with-corresponding-mount-capabilities-smb-version-21-vs-smb-version-30"></a>Karşılık gelen bağlama özelliklerine sahip önerilen en düşük sürümler (SMB sürüm 2,1 vs SMB sürüm 3,0)

|   | SMB 2.1 <br>(Aynı Azure bölgesindeki VM 'lerde takar) | SMB 3.0 <br>(Şirket içi ve çapraz bölge 'den takar) |
| --- | :---: | :---: |
| Ubuntu Server | 14.04 + | 16.04 + |
| RHEL | 7 + | 7.5+ |
| CentOS | 7 + |  7.5+ |
| Debian | 8+ |   |
| openSUSE | 13.2 + | 42.3 + |
| SUSE Linux Enterprise Server | 12 | 12 SP3 + |

Linux dağıtımlarınız burada listelenmiyorsa aşağıdaki komutla Linux çekirdek sürümünü görmek için kontrol edebilirsiniz:

```bash
uname -r
```

* <a id="install-cifs-utils"></a>**CIFS-utils paketi yüklendi.**  
    CIFS-utils paketi, seçtiğiniz Linux dağıtımında Paket Yöneticisi kullanılarak yüklenebilir. 

    **Ubuntu** ve **detem tabanlı** dağıtımlar üzerinde `apt-get` , paket yöneticisini kullanın:

    ```bash
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    **RHEL** ve **CentOS**'da `yum` paket yöneticisini kullanın:

    ```bash
    sudo yum install cifs-utils
    ```

    **OpenSUSE**'de `zypper` paket yöneticisini kullanın:

    ```bash
    sudo zypper install cifs-utils
    ```

    Diğer dağıtımlarda, uygun paket yöneticisini veya [kaynaktan derlemeyi](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download) kullanın

* **Bağlı paylaşımın dizin/dosya Izinlerine karar verin**: Aşağıdaki örneklerde, izin `0777` tüm kullanıcılara okuma, yazma ve yürütme izinleri vermek için kullanılır. Bunu istediğiniz gibi diğer [chmod izinleriyle](https://en.wikipedia.org/wiki/Chmod) değiştirebilirsiniz, ancak bu, erişimi kısıtlamış olabilir. Diğer izinleri kullanırsanız, istediğiniz yerel kullanıcı ve gruplar için erişimi sürdürmek üzere uid ve GID de kullanmayı göz önünde bulundurmanız gerekir.

> [!NOTE]
> Dir_mode ve file_mode ile Açık Dizin ve dosya izni atamadıysanız, bunlar varsayılan olarak 0755 ' dir.

* **Bağlantı noktası 445 ' ün açık olduğundan emin olun**: SMB, TCP bağlantı noktası 445 üstünden iletişim kurar. İstemci makinenizde güvenlik duvarının TCP bağlantı noktaları 445’i engellemediğinden emin olun.

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Azure dosya paylaşımından isteğe bağlı olarak bağlama`mount`

1. **[Linux dağıtıma yönelik CIFS-utils paketini yükler](#install-cifs-utils)** .

1. **Bağlama noktası için bir klasör oluşturun**: Bağlama noktası için bir klasör dosya sisteminde herhangi bir yerde oluşturulabilir, ancak bunu yeni bir klasör altında oluşturmak yaygın bir kuraldır. Örneğin, aşağıdaki komut yeni bir dizin oluşturur **< storage_account_name >** ve **< file_share_name >** , ortamınız için uygun bilgilerle değiştirin:

    ```bash
    mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Azure dosya paylaşımından bağlama için Mount komutunu kullanın**: **< Storage_account_name >** , **< paylaşım_adı >** , **< smb_version >** , **< storage_account_key >** ve **< mount_point >** , sizin için uygun bilgilerle değiştirmeyi unutmayın. ortamınızın. Linux dağıtımı şifreleme ile SMB 3,0 ' i destekliyorsa (daha fazla bilgi için bkz. [SMB istemci gereksinimlerini anlayın](#smb-client-reqs) ) **< smb_version >** için **3,0** kullanın. Şifreleme ile SMB 3,0 ' ı desteklemeyen Linux dağıtımları için **< smb_version >** için **2,1** kullanın. Azure dosya paylaşımınız, SMB 3,0 ile yalnızca bir Azure bölgesinin (Şirket içi veya farklı bir Azure bölgesinde dahil) dışında bağlanabilir. İsterseniz, bağlanan paylaşımınızın dizin ve dosya izinlerini değiştirebilir ancak bu, erişimi kısıtlamalıdır.

    ```bash
    sudo mount -t cifs //<storage_account_name>.file.core.windows.net/<share_name> <mount_point> -o vers=<smb_version>,username=<storage_account_name>,password=<storage_account_key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Azure dosya paylaşımını kullanarak işiniz bittiğinde, paylaşımını çıkarmak için kullanabilirsiniz `sudo umount <mount_point>` .

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>İle Azure dosya paylaşımının kalıcı bir bağlama noktası oluşturun`/etc/fstab`

1. **[Linux dağıtıma yönelik CIFS-utils paketini yükler](#install-cifs-utils)** .

1. **Bağlama noktası için bir klasör oluşturun**: Bağlama noktası için bir klasör dosya sisteminde herhangi bir yerde oluşturulabilir, ancak bunu yeni bir klasör altında oluşturmak yaygın bir kuraldır. Bunu oluşturduğunuz her yerde klasörün mutlak yolunu aklınızda bulabilirsiniz. Örneğin, aşağıdaki komut yeni bir dizin oluşturur **< storage_account_name >** ve **< file_share_name >** , ortamınız için uygun bilgilerle değiştirin.

    ```bash
    sudo mkdir -p <storage_account_name>/<file_share_name>
    ```

1. **Dosya paylaşımının Kullanıcı adını (depolama hesabı adı) ve parolasını (depolama hesabı anahtarı) depolamak için bir kimlik bilgisi dosyası oluşturun.** **< Storage_account_name >** ve **< storage_account_key >** , ortamınız için uygun bilgilerle değiştirin.

    ```bash
    if [ ! -d "/etc/smbcredentials" ]; then
    sudo mkdir /etc/smbcredentials
    fi
    if [ ! -f "/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred" ]; then
    sudo bash -c 'echo "username=<STORAGE ACCOUNT NAME>" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    sudo bash -c 'echo "password=7wRbLU5ea4mgc<DRIVE LETTER>PIpUCNcuG9gk2W4S2tv7p0cTm62wXTK<DRIVE LETTER>CgJlBJPKYc4VMnwhyQd<DRIVE LETTER>UT<DRIVE LETTER>yR5/RtEHyT/EHtg2Q==" >> /etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred'
    fi
    ```

1. **Kimlik bilgisi dosyasındaki izinleri değiştirerek yalnızca kök parola dosyasını okuyabilir veya değiştirebilir.** Depolama hesabı anahtarı temel olarak depolama hesabı için bir süper yönetici parolası olduğundan, dosya üzerindeki izinleri yalnızca kök erişim gibi ayarlayarak, daha düşük ayrıcalıklı kullanıcıların depolama hesabı anahtarını alamamasını sağlayabilirsiniz.   

    ```bash
    sudo chmod 600 /etc/smbcredentials/<storage_account_name>.cred
    ```

1. Aşağıdaki **satırı ' ye `/etc/fstab`eklemek için aşağıdaki komutu kullanın** : **< Storage_account_name >** , **< paylaşım_adı >** , **< smb_version >** ve **< mount_point >** , ortamınız için uygun bilgilerle değiştirmeyi unutmayın. Linux dağıtımı şifreleme ile SMB 3,0 ' i destekliyorsa (daha fazla bilgi için bkz. [SMB istemci gereksinimlerini anlayın](#smb-client-reqs) ) **< smb_version >** için **3,0** kullanın. Şifreleme ile SMB 3,0 ' ı desteklemeyen Linux dağıtımları için **< smb_version >** için **2,1** kullanın. Azure dosya paylaşımınız, SMB 3,0 ile yalnızca bir Azure bölgesinin (Şirket içi veya farklı bir Azure bölgesinde dahil) dışında bağlanabilir.

    ```bash
    sudo bash -c 'echo "//<STORAGE ACCOUNT NAME>.file.core.windows.net/<FILE SHARE NAME> /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME> cifs _netdev,nofail,vers=3.0,credentials=/etc/smbcredentials/<STORAGE ACCOUNT NAME>.cred,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'

    sudo mount /mount/<STORAGE ACCOUNT NAME>/<FILE SHARE NAME>
    ```

> [!Note]  
> ' İ yeniden `sudo mount -a` başlatmak yerine düzenlemeden `/etc/fstab` sonra Azure dosya paylaşımından bağlamak için ' i kullanabilirsiniz.

## <a name="feedback"></a>Geri Bildirim

Linux kullanıcıları, sizi dinlemek istiyoruz!

Linux kullanıcıları için Azure dosyaları ' grubu, Linux üzerinde dosya depolamayı değerlendirirken ve benimsediğiniz için geri bildirimde bulunacağınız bir forum sağlar. [Azure dosyaları Linux kullanıcılarına](mailto:azurefileslinuxusers@microsoft.com) kullanıcıların grubuna katılması için e-posta gönderin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın:

* [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
* [SSS](../storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)
