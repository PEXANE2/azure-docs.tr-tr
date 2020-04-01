---
title: Linux ile Azure Dosyalarını Kullanma | Microsoft Dokümanlar
description: Linux'ta SMB üzerinden Azure dosya paylaşımını nasıl monte edebilirsiniz öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 10/19/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 72264755d5f0379f0ffb07852f48885126a36898
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411601"
---
# <a name="use-azure-files-with-linux"></a>Azure Dosyaları'nı Linux ile kullanma
[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımları, Kobİ çekirdek [istemcisi](https://wiki.samba.org/index.php/LinuxCIFS)kullanılarak Linux dağıtımlarına monte edilebilir. Bu makalede, bir Azure dosya paylaşımı takmak için `mount` iki yol gösterilmektedir: komutile isteğe bağlı ve önyüklemede `/etc/fstab`bir giriş oluşturarak.

Linux'a Azure dosya paylaşımı nı takmanın önerilen yolu SMB 3.0'ı kullanmaktır. Varsayılan olarak, Azure Dosyaları yalnızca SMB 3.0 tarafından desteklenen aktarım sırasında şifreleme gerektirir. Azure Dosyaları ayrıca, aktarım sırasında şifrelemeyi desteklemeyen SMB 2.1'i de destekler, ancak güvenlik nedeniyle başka bir Azure bölgesinden veya şirket içinde SMB 2.1 ile Azure dosya paylaşımlarını monte edemeyebilirsiniz. Uygulamanız özellikle SMB 2.1 gerektirmediği sürece, en popüler, en son yayımlanan Linux dağıtımları SMB 3.0 desteği beri kullanmak için çok az neden yoktur:  

| | SMB 2.1 <br>(Aynı Azure bölgesindeki VM'lere bağlanır) | SMB 3.0 <br>(Bina ve çapraz bölgeden gelen bağlar) |
| --- | :---: | :---: |
| Ubuntu | 14,04+ | 16,04+ |
| Red Hat Enterprise Linux (RHEL) | 7+ | 7.5+ |
| CentOS | 7+ |  7.5+ |
| Debian | 8+ | 10+ |
| openSUSE | 13.2+ | 42.3+ |
| SUSE Linux Enterprise Server | 12 | 12 SP3+ |

Yukarıdaki tabloda listelenmemiş bir Linux dağıtımı kullanıyorsanız, Linux dağıtımınızın Linux çekirdeği sürümünü kontrol ederek SMB 3.0'ı şifrelemeyle destekleyip desteklemediğini kontrol edebilirsiniz. SMB 3.0 şifreleme ile Linux çekirdek sürümü 4.11 eklendi. Komut, `uname` linux çekirdeğinin kullanımdaki sürümünü döndürecektir:

```bash
uname -r
```

## <a name="prerequisites"></a>Ön koşullar
<a id="smb-client-reqs"></a>

* <a id="install-cifs-utils"></a>**Cifs-utils paketinin takılı olduğundan emin olun.**  
    Cifs-utils paketi seçtiğiniz Linux dağıtımında paket yöneticisi kullanılarak yüklenebilir. 

    **Ubuntu** ve **Debian tabanlı** dağıtımlarda `apt` paket yöneticisini kullanın:

    ```bash
    sudo apt update
    sudo apt install cifs-utils
    ```

    **Fedora,** **Red Hat Enterprise Linux 8+** ve **CentOS 8 +** üzerinde paket yöneticisini `dnf` kullanın:

    ```bash
    sudo dnf install cifs-utils
    ```

    Red Hat **Enterprise Linux** ve **CentOS'un**eski sürümlerinde paket yöneticisini `yum` kullanın:

    ```bash
    sudo yum install cifs-utils 
    ```

    **openSUSE'da**paket `zypper` yöneticisini kullanın:

    ```bash
    sudo zypper install cifs-utils
    ```

    Diğer dağıtımlarda, uygun paket yöneticisini kullanın veya [kaynaktan](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download)

* **Azure Komut Satırı Arabirimi'nin (CLI) en son sürümü.** Azure CLI'yi nasıl yükleyin hakkında daha fazla bilgi için Azure [CLI'yi yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve işletim sisteminizi seçin' e bakın. PowerShell 6+'da Azure PowerShell modüllerini kullanmayı tercih ederseniz, aşağıdaki talimatlar Azure CLI için sunulabilir.

* **Bağlantı noktası 445'in açık olduğundan emin olun**: SMB TCP bağlantı noktası 445 üzerinden iletişim kurar - güvenlik duvarınızın Istemci makinesinden TCP bağlantı noktalarını engellemediğini kontrol edin.  **Kaynak grubunuz<>** değiştirin ve>**deponuzu<**
    ```bash
    resourceGroupName="<your-resource-group>"
    storageAccountName="<your-storage-account>"
    
    # This command assumes you have logged in with az login
    httpEndpoint=$(az storage account show \
        --resource-group $resourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | tr -d '"')
    smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))
    fileHost=$(echo $smbPath | tr -d "/")

    nc -zvw3 $fileHost 445
    ```

    Bağlantı başarılı olduysa, aşağıdaki çıktıya benzer bir şey görmeniz gerekir:

    ```
    Connection to <your-storage-account> 445 port [tcp/microsoft-ds] succeeded!
    ```

    Şirket ağınızdaki 445 bağlantı noktasını açamıyorsanız veya bir ISS tarafından engellenirseniz, bağlantı noktası 445'i kullanmak için VPN bağlantısı veya ExpressRoute kullanabilirsiniz. Daha fazla bilgi için, doğrudan Azure dosya paylaşımı erişimi için Ağ la ilgili [hususlara](storage-files-networking-overview.md)bakın.

## <a name="mounting-azure-file-share"></a>Azure dosya paylaşımını montaj
Linux dağıtımınızla birlikte bir Azure dosya paylaşımı kullanmak için, Azure dosya paylaşımıiçin montaj noktası olarak hizmet vermek için bir dizin oluşturmanız gerekir. Linux sisteminizin herhangi bir yerinde bir montaj noktası oluşturulabilir, ancak bunu /mnt altında oluşturmak yaygın bir kuraldır. Montaj noktasından sonra, `mount` Azure dosya paylaşımına erişmek için komutu kullanırsınız.

İstenirseniz aynı Azure dosya paylaşımını birden çok montaj noktalarına monte edebilirsiniz.

### <a name="mount-the-azure-file-share-on-demand-with-mount"></a>Azure dosya paylaşımını isteğe bağlı olarak`mount`
1. **Montaj noktası için bir**klasör `<your-resource-group>` `<your-storage-account>`oluşturun `<your-file-share>` : Değiştir , ve ortamınız için uygun bilgilerle:

    ```bash
    resourceGroupName="<your-resource-group>"
    storageAccountName="<your-storage-account>"
    fileShareName="<your-file-share>"

    mntPath="/mnt/$storageAccountName/$fileShareName"

    sudo mkdir -p $mntPath
    ```

1. **Azure dosya paylaşımını monte etmek için montaj komutunu kullanın.** Aşağıdaki örnekte, yerel Linux dosyası ve klasör izinleri varsayılan 0755, hangi okuma anlamına gelir, yazmak ve sahibi için yürütmek (dosya / dizin Linux sahibi ne olursa, sahibi grup kullanıcıları için okumak ve yürütmek, ve okumak ve sistemdeki diğerleri için yürütmek. Montaj için `uid` kullanıcı `gid` kimliği ve grup kimliği ayarlamak için ve takma seçeneklerini kullanabilirsiniz. Özel izinleri `dir_mode` `file_mode` istediğiniz gibi kullanabilir ve ayarlayabilirsiniz. İzinlerin nasıl ayarlanması hakkında daha fazla bilgi için Vikipedi'deki [UNIX sayısal gösterimine](https://en.wikipedia.org/wiki/File_system_permissions#Numeric_notation) bakın. 

    ```bash
    httpEndpoint=$(az storage account show \
        --resource-group $resourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | tr -d '"')
    smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

    storageAccountKey=$(az storage account keys list \
        --resource-group $resourceGroupName \
        --account-name $storageAccountName \
        --query "[0].value" | tr -d '"')

    sudo mount -t cifs $smbPath $mntPath -o vers=3.0,username=$storageAccountName,password=$storageAccountKey,serverino
    ```

    > [!Note]  
    > Yukarıdaki montaj komutu SMB 3.0 ile bağlanır. Linux dağıtımınız SMB 3.0'ı şifrelemeyle desteklemiyorsa veya yalnızca SMB 2.1'i destekliyorsa, yalnızca depolama hesabıyla aynı bölge içindeki bir Azure VM'den monte edebilirsiniz. Azure dosya paylaşımınızı SMB 3.0'ı şifrelemeyle desteklemeyen bir Linux dağıtımına monte etmek [için, depolama hesabı için aktarım da şifrelemeyi devre dışı](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)balmanız gerekir.

Azure dosya paylaşımını kullanmayı bitirdiğinizde, `sudo umount $mntPath` paylaşımın montajını sökmek için kullanabilirsiniz.

### <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a>Azure dosya paylaşımı için kalıcı bir montaj noktası oluşturma`/etc/fstab`
1. **Montaj noktası için bir klasör oluşturma**: Montaj noktası için bir klasör dosya sisteminin herhangi bir yerinde oluşturulabilir, ancak /mnt altında bunu oluşturmak için ortak bir kuraldır. Örneğin, aşağıdaki komut yeni bir dizin `<your-resource-group>`oluşturur, değiştirin , `<your-storage-account>`ve `<your-file-share>` ortamınız için uygun bilgilerle:

    ```bash
    resourceGroupName="<your-resource-group>"
    storageAccountName="<your-storage-account>"
    fileShareName="<your-file-share>"

    mntPath="/mnt/$storageAccountName/$fileShareName"

    sudo mkdir -p $mntPath
    ```

1. **Dosya paylaşımı için kullanıcı adını (depolama hesabı adı) ve parolayı (depolama hesabı anahtarı) depolamak için bir kimlik bilgisi dosyası oluşturun.** 

    ```bash
    if [ ! -d "/etc/smbcredentials" ]; then
        sudo mkdir "/etc/smbcredentials"
    fi

    storageAccountKey=$(az storage account keys list \
        --resource-group $resourceGroupName \
        --account-name $storageAccountName \
        --query "[0].value" | tr -d '"')
    
    smbCredentialFile="/etc/smbcredentials/$storageAccountName.cred"
    if [ ! -f $smbCredentialFile ]; then
        echo "username=$storageAccountName" | sudo tee $smbCredentialFile > /dev/null
        echo "password=$storageAccountKey" | sudo tee -a $smbCredentialFile > /dev/null
    else 
        echo "The credential file $smbCredentialFile already exists, and was not modified."
    fi
    ```

1. **Yalnızca root'un parola dosyasını okuyabilmesi veya değiştirebilmeleri için kimlik bilgisi dosyasındaki izinleri değiştirin.** Depolama hesabı anahtarı aslında depolama hesabı için bir süper yönetici parolası olduğundan, daha düşük ayrıcalık kullanıcılarının depolama hesabı anahtarını alamaması için yalnızca root'un erişebileceği şekilde dosyadaki izinleri ayarlamak önemlidir.   

    ```bash
    sudo chmod 600 $smbCredentialFile
    ```

1. **Aşağıdaki satırı eklemek `/etc/fstab`için aşağıdaki komutu kullanın **: Aşağıdaki örnekte, yerel Linux dosyası ve klasör izinleri varsayılan 0755, hangi okuma anlamına gelir, yazma, ve sahibi için yürütmek (dosya / dizin Linux sahibi dayalı), okuma ve sahibi grup kullanıcıları için yürütmek ve okuma ve sistemdeki diğerleri için yürütmek. Montaj için `uid` kullanıcı `gid` kimliği ve grup kimliği ayarlamak için ve takma seçeneklerini kullanabilirsiniz. Özel izinleri `dir_mode` `file_mode` istediğiniz gibi kullanabilir ve ayarlayabilirsiniz. İzinlerin nasıl ayarlanması hakkında daha fazla bilgi için Vikipedi'deki [UNIX sayısal gösterimine](https://en.wikipedia.org/wiki/File_system_permissions#Numeric_notation) bakın.

    ```bash
    httpEndpoint=$(az storage account show \
        --resource-group $resourceGroupName \
        --name $storageAccountName \
        --query "primaryEndpoints.file" | tr -d '"')
    smbPath=$(echo $httpEndpoint | cut -c7-$(expr length $httpEndpoint))$fileShareName

    if [ -z "$(grep $smbPath\ $mntPath /etc/fstab)" ]; then
        echo "$smbPath $mntPath cifs nofail,vers=3.0,credentials=$smbCredentialFile,serverino" | sudo tee -a /etc/fstab > /dev/null
    else
        echo "/etc/fstab was not modified to avoid conflicting entries as this Azure file share was already present. You may want to double check /etc/fstab to ensure the configuration is as desired."
    fi

    sudo mount -a
    ```
    
    > [!Note]  
    > Yukarıdaki montaj komutu SMB 3.0 ile bağlanır. Linux dağıtımınız SMB 3.0'ı şifrelemeyle desteklemiyorsa veya yalnızca SMB 2.1'i destekliyorsa, yalnızca depolama hesabıyla aynı bölge içindeki bir Azure VM'den monte edebilirsiniz. Azure dosya paylaşımınızı SMB 3.0'ı şifrelemeyle desteklemeyen bir Linux dağıtımına monte etmek [için, depolama hesabı için aktarım da şifrelemeyi devre dışı](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)balmanız gerekir.

### <a name="using-autofs-to-automatically-mount-the-azure-file-shares"></a>Azure dosya paylaşımını otomatik olarak monte etmek için otomatik olarak kullanma

1. **Autofs paketinin takılı olduğundan emin olun.**  

    Autofs paketi seçtiğiniz Linux dağıtımında paket yöneticisi kullanılarak yüklenebilir. 

    **Ubuntu** ve **Debian tabanlı** dağıtımlarda `apt` paket yöneticisini kullanın:
    ```bash
    sudo apt update
    sudo apt install autofs
    ```
    **Fedora,** **Red Hat Enterprise Linux 8+** ve **CentOS 8 +** üzerinde paket yöneticisini `dnf` kullanın:
    ```bash
    sudo dnf install autofs
    ```
    Red Hat **Enterprise Linux** ve **CentOS'un**eski sürümlerinde paket yöneticisini `yum` kullanın:
    ```bash
    sudo yum install autofs 
    ```
    **openSUSE'da**paket `zypper` yöneticisini kullanın:
    ```bash
    sudo zypper install autofs
    ```
2. **Paylaşım(lar) için bir montaj noktası oluşturun:**
   ```bash
    sudo mkdir /fileshares
    ```
3. **Girit yeni bir özel autofs yapılandırma dosyası**
    ```bash
    sudo vi /etc/auto.fileshares
    ```
4. **/etc/auto.fileshares için aşağıdaki girişleri ekleyin**
   ```bash
   echo "$fileShareName -fstype=cifs,credentials=$smbCredentialFile :$smbPath"" > /etc/auto.fileshares
   ```
5. **/etc/auto.master için aşağıdaki girişi ekleyin**
   ```bash
   /fileshares /etc/auto.fileshares --timeout=60
   ```
6. **Autofs'u yeniden başlat**
    ```bash
    sudo systemctl restart autofs
    ```
7.  **Paylaşım için belirlenen klasöre erişin**
    ```bash
    cd /fileshares/$filesharename
    ```
## <a name="securing-linux"></a>Linux'u Güvence Altına Alma
Linux'a Azure dosya paylaşımı yapabilmek için port 445'e erişilebilir olması gerekir. Çoğu kuruluş SMB 1 kaynaklı güvenlik riskleri nedeniyle 445 numaralı bağlantı noktasını engeller. SMB 1, cifs (Ortak İnternet Dosya Sistemi) olarak da bilinir, birçok Linux dağıtımında yer alan eski bir dosya sistemi protokolüdür. SMB 1 eski, verimsiz ve hepsinden önemlisi güvenli olmayan bir protokoldür. İyi haber, Azure Files'ın Kobİ 1'i desteklememesi ve Linux çekirdek sürüm 4.18 ile başlayarak SMB 1'i devre dışı bilebilir hale getiriyor olması. Üretimde Kobİ dosya paylaşımlarını kullanmadan önce SMB 1'in Linux istemcilerinizde devre dışı bırakılmasını her zaman [şiddetle tavsiye ediyoruz.](https://aka.ms/stopusingsmb1)

Linux çekirdeği 4.18 ile başlayarak, eski nedenlerle `cifs` çağrılan Kobİ çekirdeği modülü, yeni bir modül parametresi ortaya çıkarır (genellikle `disable_legacy_dialects`çeşitli dış belgeler tarafından *parm* olarak adlandırılır), denir. Linux çekirdeği 4.18'de kullanılmaya başlanmış olsa da, bazı satıcılar bu değişikliği destekledikleri eski çekirdeklere geri döndürdüler. Kolaylık sağlamak için, aşağıdaki tabloda bu modül parametresinin ortak Linux dağıtımlarında kullanılabilirliği ayrıntılı olarak açıklanabilir.

| Dağıtım | SMB 1'i devre dışı kılabilir |
|--------------|-------------------|
| Ubuntu 14.04-16.04 | Hayır |
| Ubuntu 18.04 | Evet |
| Ubuntu 19,04+ | Evet |
| Debian 8-9 | Hayır |
| Debian 10+ | Evet |
| Fedora 29+ | Evet |
| CentOS 7 | Hayır | 
| CentOS 8+ | Evet |
| Kırmızı Şapka Kurumsal Linux 6.x-7.x | Hayır |
| Kırmızı Şapka Kurumsal Linux 8+ | Evet |
| openSUSE Sıçraması 15.0 | Hayır |
| openSUSE Artık 15.1+ | Evet |
| openSUSE Tumbleweed | Evet |
| SUSE Linux Enterprise 11.x-12.x | Hayır |
| SUSE Linux Kurumsal 15 | Hayır |
| SUSE Linux Kurumsal 15.1 | Hayır |

Linux dağıtımınızın modül parametresini `disable_legacy_dialects` aşağıdaki komutla destekleyip desteklemediğinizi kontrol edebilirsiniz.

```bash
sudo modinfo -p cifs | grep disable_legacy_dialects
```

Bu komut aşağıdaki iletiyi çıktı etmelidir:

```output
disable_legacy_dialects: To improve security it may be helpful to restrict the ability to override the default dialects (SMB2.1, SMB3 and SMB3.02) on mount with old dialects (CIFS/SMB1 and SMB2) since vers=1.0 (CIFS/SMB1) and vers=2.0 are weaker and less secure. Default: n/N/0 (bool)
```

SMB 1'i devre dışı bırakmadan önce, SMB modülünün sisteminize şu anda yüklenmediğinden emin olmalısınız (Bir Kobİ payı na monte ettiyseniz bu otomatik olarak gerçekleşir). SMB yüklenmezse hiçbir şey çıkmaması gereken aşağıdaki komutla bunu yapabilirsiniz:

```bash
lsmod | grep cifs
```

Modülü boşaltmak için önce tüm Kobİ hisselerinin `umount` montajını boşaltın (yukarıda açıklandığı gibi komutu kullanarak). Sisteminizdeki tüm monte edilmiş Kobİ paylaşımlarını aşağıdaki komutla tanımlayabilirsiniz:

```bash
mount | grep cifs
```

Tüm Kobİ dosya paylaşımlarını boşalttınktan sonra modülü boşaltmak güvenlidir. Bunu `modprobe` komutuyla yapabilirsiniz.

```bash
sudo modprobe -r cifs
```

Komutu kullanarak `modprobe` modülü SMB 1'e göre el ile yükleyebilirsiniz:

```bash
sudo modprobe cifs disable_legacy_dialects=Y
```

Son olarak, yüklenen parametrelere bakarak parametre ile yüklenmiş olan Kobİ `/sys/module/cifs/parameters`modüllerini kontrol edebilirsiniz:

```bash
cat /sys/module/cifs/parameters/disable_legacy_dialects
```

Ubuntu ve Debian tabanlı dağıtımlarda SMB 1'i kalıcı olarak devre dışı kullanabilirsiniz, ayarı ile birlikte çağrılan `/etc/modprobe.d/local.conf` yeni bir dosya (diğer modüller için özel seçenekleriniz yoksa) oluşturmanız gerekir. Bunu aşağıdaki komutla yapabilirsiniz:

```bash
echo "options cifs disable_legacy_dialects=Y" | sudo tee -a /etc/modprobe.d/local.conf > /dev/null
```

Bunun Kobİ modüllerini yükleyerek çalıştığını doğrulayabilirsiniz:

```bash
sudo modprobe cifs
cat /sys/module/cifs/parameters/disable_legacy_dialects
```

## <a name="feedback"></a>Geri Bildirim
Linux kullanıcıları, sizden duymak istiyorum!

Linux kullanıcıları için Azure Dosyaları, Linux'ta Dosya depolamayı değerlendirirken ve benimsediğiniz için geri bildirim paylaşmanız için bir forum sağlar. [Azure Files Linux Kullanıcıları'na](mailto:azurefiles@microsoft.com) kullanıcı grubuna katılmaları için e-posta gönder.

## <a name="next-steps"></a>Sonraki adımlar
Azure Dosyaları hakkında daha fazla bilgi edinmek için şu bağlantılara göz atın:

* [Azure Dosyaları dağıtımı planlama](storage-files-planning.md)
* [SSS](../storage-files-faq.md)
* [Sorun giderme](storage-troubleshoot-linux-file-connection-problems.md)
