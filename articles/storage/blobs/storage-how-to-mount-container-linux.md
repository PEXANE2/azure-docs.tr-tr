---
title: Linux'ta bir dosya sistemi olarak Azure Blob depolama nasıl monte ediletilir | Microsoft Dokümanlar
description: Linux'ta FUSE ile Azure Blob depolama konteyneri takma
author: rishabpoh
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.date: 2/1/2019
ms.author: ripohane
ms.reviewer: dineshm
ms.openlocfilehash: a0a03df59bc6ecffcb4f0a701616297f2da78fdb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061419"
---
# <a name="how-to-mount-blob-storage-as-a-file-system-with-blobfuse"></a>Blobf depolama blobfuse ile bir dosya sistemi olarak monte nasıl

## <a name="overview"></a>Genel Bakış
[Blobfuse,](https://github.com/Azure/azure-storage-fuse) Azure Blob depolama alanı için sanal bir dosya sistemi sürücüsüdür. Blobfuse, Linux dosya sistemi üzerinden depolama hesabınızdaki mevcut blok blob verilerine erişmenizi sağlar. Blobfuse, ileri eğik çizgi '/' ile sanal dizin düzenini sınırlayıcı olarak kullanır.  

Bu kılavuz, blobfuse nasıl kullanılacağını gösterir ve Linux ve erişim verilere bir Blob depolama konteyner monte. Blobfuse hakkında daha fazla bilgi edinmek [için, blobfuse deposundaki](https://github.com/Azure/azure-storage-fuse)ayrıntıları okuyun.

> [!WARNING]
> Blobfuse sadece [Blob REST API'ler](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api)içine istekleri çevirir gibi% 100 POSIX uyumluluk garanti etmez. Örneğin, yeniden adlandırma işlemleri POSIX atomik, ancak blobfuse değildir.
> Yerel bir dosya sistemi ve blobfuse arasındaki farkların tam listesi [için, blobfuse kaynak kodu deposunu ziyaret edin.](https://github.com/azure/azure-storage-fuse)
> 

## <a name="install-blobfuse-on-linux"></a>Linux'a blobfuse yükleyin
Blobfuse ikilileri Ubuntu ve RHEL [dağıtımları için Linux için Microsoft yazılım depolarında](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software) mevcuttur. Bu dağıtımlara blobfuse yüklemek için, listedeki depolardan birini yapılandırın. Dağıtımınız için kullanılabilir ikili ler yoksa, [Azure Depolama yükleme adımlarını](https://github.com/Azure/azure-storage-fuse/wiki/1.-Installation#option-2---build-from-source) izleyerek kaynak kodundan ikili ler de oluşturabilirsiniz.

Blobfuse, Ubuntu 14.04, 16.04 ve 18.04'te kurulumunu destekler. Bu sürümlerden birine sahip olduğundan emin olmak için bu komutu çalıştırın:
```
lsb_release -a
```

### <a name="configure-the-microsoft-package-repository"></a>Microsoft paket deposunu yapılandırma
[Microsoft Ürünleri için Linux Paket Deposu'nu](https://docs.microsoft.com/windows-server/administration/Linux-Package-Repository-for-Microsoft-Software)yapılandırın.

Örnek olarak, Enterprise Linux 6 dağıtımında:
```bash
sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm
```

Benzer şekilde, URL'yi Enterprise Linux 7 dağıtımına işaret etmek `.../rhel/7/...` için değiştirin.

Ubuntu 14.04 dağılımıyla ilgili başka bir örnek:
```bash
wget https://packages.microsoft.com/config/ubuntu/14.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo apt-get update
```

Benzer şekilde, `.../ubuntu/16.04/...` `.../ubuntu/18.04/...` URL'yi başka bir Ubuntu sürümüne veya başvuruya değiştirin.

### <a name="install-blobfuse"></a>Blobfuse yükleyin

Bir Ubuntu/Debian dağılımında:
```bash
sudo apt-get install blobfuse
```

Kurumsal Linux dağıtımında:
```bash    
sudo yum install blobfuse
```

## <a name="prepare-for-mounting"></a>Montaja hazırlanın
Blobfuse, dosya sisteminde açık dosyaları arabelleğe almak ve önbelleğe almak için geçici bir yol gerektirerek yerel benzer performans sağlar. Bu geçici yol için en çok performans gösteren diski seçin veya en iyi performans için bir ramdisk kullanın. 

> [!NOTE]
> Blobfuse tüm açık dosya içeriğini geçici yolda depolar. Tüm açık dosyaları barındıracak yeterli alana sahip olduğundan emin olun. 
> 

### <a name="optional-use-a-ramdisk-for-the-temporary-path"></a>(İsteğe bağlı) Geçici yol için ramdisk kullanma
Aşağıdaki örnek, 16 GB ramdisk ve blobfuse için bir dizin oluşturur. İhtiyaçlarınıza göre boyutu seçin. Bu ramdisk blobfuse boyutu 16 GB'a kadar dosyaları açmak için izin verir. 
```bash
sudo mount -t tmpfs -o size=16g tmpfs /mnt/ramdisk
sudo mkdir /mnt/ramdisk/blobfusetmp
sudo chown <youruser> /mnt/ramdisk/blobfusetmp
```

### <a name="use-an-ssd-as-a-temporary-path"></a>Geçici bir yol olarak SSD kullanma
Azure'da, kabarcıklar için düşük gecikmeli arabellek sağlamak için VM'lerinizde bulunan geçici diskleri (SSD) kullanabilirsiniz. Ubuntu dağılımlarında bu geçici disk '/mnt' üzerine monte edilir. Red Hat ve CentOS dağılımlarında disk '/mnt/resource/' üzerine monte edilir.

Kullanıcınızın geçici yola erişediğinden emin olun:
```bash
sudo mkdir /mnt/resource/blobfusetmp -p
sudo chown <youruser> /mnt/resource/blobfusetmp
```

### <a name="configure-your-storage-account-credentials"></a>Depolama hesabı kimlik bilgilerinizi yapılandırma
Blobfuse, kimlik bilgilerinizin bir metin dosyasında aşağıdaki biçimde depolanmasını gerektirir: 

```
accountName myaccount
accountKey storageaccesskey
containerName mycontainer
```
Bu, `accountName` depolama hesabınız için önektir, tam URL değil.

Bu dosyayı kullanarak oluşturun:

```
touch ~/fuse_connection.cfg
```

Bu dosyayı oluşturduktan ve düzenledikten sonra, başka hiçbir kullanıcının bu dosyayı okuyaması için erişimi kısıtladığından emin olun.
```bash
chmod 600 fuse_connection.cfg
```

> [!NOTE]
> Windows'da yapılandırma dosyasını oluşturduysanız, dosyayı dezenfekte etmek ve Unix biçimine dönüştürmek için çalıştırdığınızdan `dos2unix` emin olun. 
>

### <a name="create-an-empty-directory-for-mounting"></a>Montaj için boş bir dizin oluşturma
```bash
mkdir ~/mycontainer
```

## <a name="mount"></a>Bağlama

> [!NOTE]
> Montaj seçeneklerinin tam listesi [için, blobfuse deposunu kontrol edin.](https://github.com/Azure/azure-storage-fuse#mount-options)  
> 

Blobfuse monte etmek için, kullanıcı ile aşağıdaki komutu çalıştırın. Bu komut, '/path/to/fuse_connection.cfg' adresinde belirtilen kapsayıcıyı '/mycontainer' konumuna bağlar.

```bash
sudo blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=/path/to/fuse_connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
```

Artık normal dosya sistemi API'leri aracılığıyla blok blobs erişimi olmalıdır. Dizini monte eden kullanıcı, varsayılan olarak bu dizine erişebilen ve erişimi güvence altına alan tek kişidir. Tüm kullanıcılara erişim sağlamak için, seçeneği ```-o allow_other```ile monte edebilirsiniz. 

```bash
cd ~/mycontainer
mkdir test
echo "hello world" > test/blob.txt
```

## <a name="next-steps"></a>Sonraki adımlar

* [Blobfuse giriş sayfası](https://github.com/Azure/azure-storage-fuse#blobfuse)
* [Blobfuse sorunlarını bildirin](https://github.com/Azure/azure-storage-fuse/issues) 

