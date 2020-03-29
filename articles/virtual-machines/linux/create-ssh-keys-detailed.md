---
title: SSH anahtar çifti oluşturmak için ayrıntılı adımlar
description: Azure'da Linux VM'ler için bir SSH ortak ve özel anahtar çifti oluşturmak ve yönetmek için ayrıntılı adımları öğrenin.
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 12/06/2019
ms.author: cynthn
ms.openlocfilehash: c34a88c39104d3af2c5747d1cd6d3dea6929379a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969549"
---
# <a name="detailed-steps-create-and-manage-ssh-keys-for-authentication-to-a-linux-vm-in-azure"></a>Ayrıntılı adımlar: Azure'da bir Linux VM'de kimlik doğrulama için SSH anahtarlarını oluşturma ve yönetme 
Güvenli bir kabuk (SSH) anahtar çiftiyle, Azure'da kimlik doğrulaması için SSH anahtarlarını kullanmayı varsayılan bir Linux sanal makinesi oluşturarak oturum açma gereksinimini ortadan kaldırabilirsiniz. Azure portalı, Azure CLI, Kaynak Yöneticisi şablonları veya diğer araçlarla oluşturulan VM'ler, SSH bağlantıları için SSH anahtar kimlik doğrulamasını ayarlayan dağıtımın bir parçası olarak SSH ortak anahtarınızı içerebilir. 

Bu makalede, SSH istemci bağlantıları için bir SSH RSA ortak özel anahtar dosyası çifti oluşturmak ve yönetmek için ayrıntılı arka plan ve adımlar sağlar. Hızlı komutlar istiyorsanız, [Azure'da Linux VM'leri için bir SSH genel özel anahtar çifti oluşturma bilgisini](mac-create-ssh-keys.md)öğrenin.

Windows bilgisayarında SSH tuşları oluşturmanın ve kullanmanın ek yolları için, [Windows'da Windows'da SSH tuşlarının nasıl kullanılacağına](ssh-from-windows.md)bakın.

[!INCLUDE [virtual-machines-common-ssh-overview](../../../includes/virtual-machines-common-ssh-overview.md)]

### <a name="private-key-passphrase"></a>Özel anahtar parolası
SSH özel anahtarı, onu korumak için çok güvenli bir geçiş cümlesine sahip olmalıdır. Bu parola yalnızca özel SSH anahtar dosyasına erişmek içindir ve kullanıcı hesabı parolası *değildir.* SSH anahtarınıza parola eklediğinizde bu parola özel anahtarı 128 bit AES kullanarak şifreler; böylece özel anahtar, şifresini çözen parola olmadan kullanılamaz. Bir saldırgan özel anahtarınızı çaldıysa ve bu anahtarın bir parolası yoksa, ilgili ortak anahtara sahip sunucularda oturum açabilmek için bu özel anahtarı kullanabilir. Özel bir anahtar bir parola yla korunuyorsa, bu anahtar bu saldırgan tarafından kullanılamaz ve Azure'daki altyapınız için ek bir güvenlik katmanı sağlar.

[!INCLUDE [virtual-machines-common-ssh-support](../../../includes/virtual-machines-common-ssh-support.md)]

## <a name="ssh-keys-use-and-benefits"></a>SSH anahtarlarının kullanımı ve avantajları

Ortak anahtarı belirterek bir Azure VM oluşturduğunuzda, Azure ortak `.pub` anahtarı (biçimde) VM'deki `~/.ssh/authorized_keys` klasöre kopyalar. SSH `~/.ssh/authorized_keys` anahtarları, istemciye bir SSH bağlantısında karşılık gelen özel anahtarla eşleşmesi için meydan okumak için kullanılır. Azure, kimlik doğrulaması için SSH anahtarlarını kullanan bir Azure Linux VM'de, SSHD sunucusunu parola oturum açmaya, yalnızca SSH tuşlarına izin vermeyecek şekilde yapılandırır. Bu nedenle, SSH tuşlarına sahip bir Azure Linux VM oluşturarak, VM dağıtımının güvenliğini sağlamaya yardımcı olabilir ve `sshd_config` dosyadaki parolaları devre dışı bırakmak için tipik dağıtım sonrası yapılandırma adımını kendinize kaydedebilirsiniz.

SSH tuşlarını kullanmak istemiyorsanız, Linux VM'nizi parola kimlik doğrulamasını kullanacak şekilde ayarlayabilirsiniz. VM'niz Internet'e açık değilse, parola kullanmak yeterli olabilir. Ancak, yine de her Linux VM için parolalarınızı yönetmeniz ve minimum parola uzunluğu ve düzenli güncelleştirmeler gibi sağlıklı parola ilkeleri ve uygulamalarını korumanız gerekir. SSH anahtarlarının kullanılması, birden çok VM'de tek tek kimlik bilgilerini yönetmenin karmaşıklığını azaltır.

## <a name="generate-keys-with-ssh-keygen"></a>ssh-keygen ile anahtar oluşturma

Anahtarları oluşturmak [için,](https://docs.microsoft.com/windows/wsl/about)Azure `ssh-keygen`Bulut Shell'deki OpenSSH yardımcı programları, macOS veya Linux ana bilgisayarı, Linux için Windows Alt Sistemi ve diğer araçlarda kullanılabilen tercih edilen bir komutdur. `ssh-keygen`bir dizi soru sorar ve ardından özel bir anahtar ve eşleşen bir ortak anahtar yazar. 

SSH anahtarları, varsayılan olarak `~/.ssh` dizininde tutulur.  `~/.ssh` dizininiz yoksa `ssh-keygen` komutu, doğru izinler ile sizin için oluşturur.

### <a name="basic-example"></a>Temel örnek

Aşağıdaki `ssh-keygen` komut, `~/.ssh` dizinde varsayılan olarak 2048 bit SSH RSA ortak ve özel anahtar dosyaları oluşturur. Geçerli konumda bir SSH anahtar çifti varsa, bu dosyalar üzerine yazılır.

```bash
ssh-keygen -m PEM -t rsa -b 4096
```

### <a name="detailed-example"></a>Ayrıntılı örnek
Aşağıdaki örnekte, bir SSH RSA anahtar çifti oluşturmak için ek komut seçenekleri gösterilmektedir. Geçerli konumda bir SSH anahtar çifti varsa, bu dosyalar üzerine yazılır. 

```bash
ssh-keygen \
    -m PEM \
    -t rsa \
    -b 4096 \
    -C "azureuser@myserver" \
    -f ~/.ssh/mykeys/myprivatekey \
    -N mypassphrase
```

**Komut açıklaması**

`ssh-keygen` = anahtarları oluşturmak için kullanılan program

`-m PEM`= anahtarı PEM olarak biçimlendirin

`-t rsa`= rsa formatında bu durumda, oluşturmak için anahtar türü

`-b 4096`= anahtardaki bit sayısı, bu durumda 4096

`-C "azureuser@myserver"` = kolayca tanımlamak için ortak anahtar dosyasının sonuna eklenen bir açıklama. Normalde yorum olarak bir e-posta adresi kullanılır, ancak altyapınız için en uygun olanı kullanın.

`-f ~/.ssh/mykeys/myprivatekey`= varsayılan adı kullanmamayı seçerseniz, özel anahtar dosyasının dosya adı. Aynı dizinde eklenen `.pub` ilgili ortak anahtar dosyası oluşturulur. Dizinin var olması gerekir.

`-N mypassphrase`= özel anahtar dosyasına erişmek için kullanılan ek bir parola. 

### <a name="example-of-ssh-keygen"></a>ssh-keygen örneği

```bash
ssh-keygen -t -m PEM rsa -b 4096 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:vFfHHrpSGQBd/oNdvNiX0sG9Vh+wROlZBktNZw9AUjA azureuser@myserver
The key's randomart image is:
+---[RSA 4096]----+
|        .oE=*B*+ |
|          o+o.*++|
|           .oo++*|
|       .    .B+.O|
|        S   o=BO.|
|         . .o++o |
|        . ... .  |
|         ..  .   |
|           ..    |
+----[SHA256]-----+
```

#### <a name="saved-key-files"></a>Kaydedilmiş anahtar dosyaları

`Enter file in which to save the key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

Bu makale için anahtar çifti adı. Anahtar çiftinin `id_rsa` adlandırılması varsayılandır; bazı araçlar `id_rsa` özel anahtar dosya adı bekleyebilirsiniz, bu nedenle bir sahip iyi bir fikirdir. `~/.ssh/` dizini, SSH anahtar çiftleri ve SSH yapılandırma dosyası için varsayılan konumdur. Tam yol belirtilmezse `ssh-keygen` tarafından oluşturulan anahtarlar varsayılan `~/.ssh` dizininde değil geçerli çalışma dizininde olur.

#### <a name="list-of-the-ssh-directory"></a>`~/.ssh` Dizinin listesi

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

#### <a name="key-passphrase"></a>Anahtar parola

`Enter passphrase (empty for no passphrase):`

Özel anahtarınıza bir parola eklemeniz *önerilir.* Anahtar dosyasını korumak için bir parola olmadan, dosyaya sahip olan herkes, ilgili ortak anahtara sahip herhangi bir sunucuda oturum açmak için dosyayı kullanabilir. Parola eklemek, birinin özel anahtar dosyanıza erişebilmesi durumunda daha fazla koruma sağlar ve size anahtarları değiştirmeniz için zaman kazandırır.

## <a name="generate-keys-automatically-during-deployment"></a>Dağıtım sırasında anahtarları otomatik olarak oluşturma

VM'nizi oluşturmak için [Azure CLI'yi](/cli/azure) kullanıyorsanız, [az vm oluşturma](/cli/azure/vm) komutunu `--generate-ssh-keys` seçeneğiyle çalıştırarak isteğe bağlı olarak SSH ortak ve özel anahtar dosyaları oluşturabilirsiniz. Anahtarlar ~/.ssh dizininde saklanır. Bu komut seçeneği, o konumda zaten varsa tuşların üzerine yazmıyor.

## <a name="provide-ssh-public-key-when-deploying-a-vm"></a>VM dağıtırken SSH ortak anahtarı sağlayın

Kimlik doğrulaması için SSH anahtarlarını kullanan bir Linux VM oluşturmak için, Azure portalı, CLI, Kaynak Yöneticisi şablonları veya diğer yöntemleri kullanarak VM'yi oluştururken SSH ortak anahtarınızı sağlayın. Portalı kullanırken, ortak anahtarın kendisini girersiniz. Varolan bir ortak anahtarla VM'nizi oluşturmak için [Azure CLI'yi](/cli/azure) kullanıyorsanız, [az vm oluşturma](/cli/azure/vm) `--ssh-key-value` komutunu seçenekle çalıştırarak bu ortak anahtarın değerini veya konumunu belirtin. 

Bir SSH ortak anahtarının biçimini bilmiyorsanız, genel anahtarınızın biçimini aşağıdaki `cat` gibi çalıştırarak `~/.ssh/id_rsa.pub` ve yerine kendi ortak anahtar dosyası konumunuzu değiştirerek görebilirsiniz:

```bash
cat ~/.ssh/id_rsa.pub
```

Çıktı aşağıdakine benzer (burada redacted):

```
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXG+/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Ortak anahtar dosyasının içeriğini kopyalayıp Azure portalına veya Kaynak Yöneticisi şablonuna yapıştırırsanız, ek beyaz boşluk kopyalamadığınızdan veya ek satır sonları tanıtmadığınızdan emin olun. Örneğin, macOS kullanıyorsanız, içeriği kopyalamak için genel anahtar `~/.ssh/id_rsa.pub`dosyasını (varsayılan olarak) **pbcopy'ye** aktarabilirsiniz (örneğin, `xclip`aynı şeyi yapan diğer Linux programları vardır).

Çok satırlı biçimde ortak bir anahtar kullanmayı tercih ederseniz, daha önce oluşturduğunuz ortak anahtardan bir pem kapsayıcısında RFC4716 biçimlendirilmiş bir anahtar oluşturabilirsiniz.

Var olan bir SSH ortak anahtarından RFC4716 biçimli bir anahtar oluşturmak için:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="ssh-to-your-vm-with-an-ssh-client"></a>SSH istemcisi ile VM'inize SSH
Azure VM'nizde genel anahtar ve yerel sisteminizdeki özel anahtarla, VM'nizin IP adresini veya DNS adını kullanarak VM'nize SSH. *Azureuser* ve *myvm.westus.cloudapp.azure.com* aşağıdaki komutu yönetici kullanıcı adı ve tam nitelikli alan adı (veya IP adresi) ile değiştirin:

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

Anahtar çiftinizi oluşturduğunuzda bir parola verdiyseniz, oturum açma işlemi sırasında istendiğinde parolayı girin. (Sunucu `~/.ssh/known_hosts` klasörünüze eklenir ve Azure VM’nizdeki ortak anahtar değiştirilene veya sunucu adı `~/.ssh/known_hosts` konumundan kaldırılana kadar yeniden bağlanmanız istenmez.)

VM tam zamanında erişim ilkesini kullanıyorsa, VM'ye bağlanmadan önce erişim istemeniz gerekir. Tam zamanında ilke hakkında daha fazla bilgi için, [tam zamanında ilkeyi kullanarak sanal makine erişimini yönet'e](../../security-center/security-center-just-in-time.md)bakın.

## <a name="use-ssh-agent-to-store-your-private-key-passphrase"></a>Özel anahtar parolanızı depolamak için ssh-agent'ı kullanın

Her SSH oturum açarak özel anahtar dosyası parolanızı yazmaktan kaçınmak için, özel anahtar dosyası parolanızı önbelleğe almak için kullanabilirsiniz. `ssh-agent` Mac kullanıyorsanız, macOS Anahtar zinciri çağırdığınızda `ssh-agent`özel anahtar parolasını güvenli bir şekilde saklar.

Parolayı `ssh-agent` etkileşimli olarak kullanmanızgerekmeyecek şekilde ssh sistemini anahtar dosyaları doğrulayın ve kullanın ve `ssh-add` bilgilendirin.

```bash
eval "$(ssh-agent -s)"
```

Şimdi `ssh-add` komutunu kullanarak özel anahtarı `ssh-agent` öğesine ekleyin.

```bash
ssh-add ~/.ssh/id_rsa
```

Özel anahtar parolası artık ' `ssh-agent`da depolanır.

## <a name="use-ssh-copy-id-to-copy-the-key-to-an-existing-vm"></a>Anahtarı varolan bir VM'nin anahtarını kopyalamak için ssh-copy-id'yi kullanma
Zaten bir VM oluşturduysanız, Linux VM'nize yeni SSH ortak anahtarını aşağıdakilere benzer bir komutla yükleyebilirsiniz:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>SSH yapılandırma dosyası oluşturma ve yapılandırma

Oturum açma işlemlerini hızlandırmak ve SSH`~/.ssh/config`istemci davranışınızı optimize etmek için bir SSH config dosyası oluşturabilir ve yapılandırabilirsiniz. 

Aşağıdaki örnekte, varsayılan SSH özel anahtarını kullanarak kullanıcı olarak belirli bir VM'de hızlı bir şekilde oturum açabilmek için kullanabileceğiniz basit bir yapılandırma gösterilmektedir. 

### <a name="create-the-file"></a>Önbelleği oluşturma

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Yeni SSH yapılandırmasını eklemek için dosyayı düzenleme

```bash
vim ~/.ssh/config
```

### <a name="example-configuration"></a>Örnek yapılandırma

Ana bilgisayar VM'inize uygun yapılandırma ayarları ekleyin.

```bash
# Azure Keys
Host myvm
  Hostname 102.160.203.241
  User azureuser
# ./Azure Keys
```

Her birinin kendi özel anahtar çiftini kullanmasını sağlamak için ek ana bilgisayarlar için yapılandırmalar ekleyebilirsiniz. Daha gelişmiş yapılandırma seçenekleri için [SSH config dosyasına](https://www.ssh.com/ssh/config/) bakın.

Artık bir SSH anahtar çiftiniz ve yapılandırılmış bir SSH config dosyanız olduğuna göre, Linux VM'nizde hızlı ve güvenli bir şekilde oturum açabilirsiniz. Aşağıdaki komutu çalıştırdığınızda, SSH, SSH `Host myvm` config dosyasındaki bloktaki tüm ayarları bulur ve yükler.

```bash
ssh myvm
```

Bir sunucuda ssh tuşu kullanarak ilk oturum açtığınızda, komut, bu anahtar dosyasının parolasını ister.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, yeni SSH ortak anahtarını kullanarak Azure Linux VM’ler oluşturmaktır. Oturum açma olarak SSH ortak anahtarıyla oluşturulan Azure VM'ler, varsayılan oturum açma yöntemi, parolalarla oluşturulan VM'lerden daha iyi emniyete alınır.

* [Azure portal ile Linux sanal makinesi oluşturma](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure CLI ile Linux sanal makinesi oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Bir Azure şablonu kullanarak bir Linux VM oluşturma](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
