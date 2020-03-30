---
title: AzCopy v10 kullanarak verileri kopyalama veya Azure Depolama'ya taşıma | Microsoft Dokümanlar
description: AzCopy, depolama hesaplarına, depo hesaplarına veya bunlar arasında verileri kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, AzCopy'yi indirmenize, depolama hesabınıza bağlanmanıza ve ardından dosyaları aktarmanıza yardımcı olur.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 90adb3b973b6952d10ef89e4b40d0fc68489e224
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80061102"
---
# <a name="get-started-with-azcopy"></a>AzCopy’yi kullanmaya başlama

AzCopy, bir depolama hesabına veya bir depolama hesabından blobveya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, AzCopy'yi indirmenize, depolama hesabınıza bağlanmanıza ve ardından dosyaları aktarmanıza yardımcı olur.

> [!NOTE]
> AzCopy **V10,** AzCopy'nin şu anda desteklenen sürümüdür.
>
> AzCopy **v8.1**kullanmanız gerekiyorsa, bu [makalenin AzCopy bölümünün önceki sürümünü kullanın](#previous-version) bakın.

<a id="download-and-install-azcopy" />

## <a name="download-azcopy"></a>Azcopy İndir

İlk olarak, AzCopy V10 çalıştırılabilir dosyayı bilgisayarınızdaki herhangi bir dizine indirin. AzCopy V10 sadece çalıştırılabilir bir dosya, bu yüzden yüklemek için bir şey yok.

- [Windows 64 bit](https://aka.ms/downloadazcopy-v10-windows) (zip)
- [Windows 32 bit](https://aka.ms/downloadazcopy-v10-windows-32bit) (zip)
- [Linux](https://aka.ms/downloadazcopy-v10-linux) (katran)
- [MacOS](https://aka.ms/downloadazcopy-v10-mac) (zip)

Bu dosyalar zip dosyası (Windows ve Mac) veya katran dosyası (Linux) olarak sıkıştırılır. Linux'taki katran dosyasını indirmek ve sıkıştırmak için Linux dağıtımınız için belgelere bakın.

> [!NOTE]
> [Azure Table depolama](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) hizmetinize ve azure tablo depolama hizmetinize veri kopyalamak istiyorsanız, [AzCopy sürüm 7.3'e](https://aka.ms/downloadazcopynet)yükleyin.


## <a name="run-azcopy"></a>Azcopy çalıştırın

Kolaylık sağlamak için, kullanım kolaylığı için çalıştırılabilen AzCopy'nin dizin konumunu sistem yolunuza eklemeyi düşünün. Bu şekilde sisteminizdeki herhangi bir dizinden yazabilirsiniz. `azcopy`

AzCopy dizini yolunuza eklememeyi seçerseniz, dizinleri AzCopy'nizin çalıştırılabilir konumuna ve `azcopy` türüne veya `.\azcopy` Windows PowerShell komut istemlerinde değiştirmeniz gerekir.

Komutlistesini görmek için ENTER `azcopy -h` tuşuna yazın ve basın.

Belirli bir komut hakkında bilgi edinmek için, komutun `azcopy list -h`adını eklemenin yeterli olduğunu belirtin (Örneğin: ).

![Satır satır da yardım](media/storage-use-azcopy-v10/azcopy-inline-help.png)

Her komut ve komut parametresi için ayrıntılı başvuru belgelerini bulmak için [bkz.](storage-ref-azcopy.md)

> [!NOTE] 
> Azure Depolama hesabınızın sahibi olarak, verilere erişmek için otomatik olarak izin atanırsınız. AzCopy ile anlamlı bir şey yapmadan önce, depolama hizmetine yetki kimlik bilgilerini nasıl sağlayacağınızkonusunda karar vermeniz gerekir. 

## <a name="choose-how-youll-provide-authorization-credentials"></a>Yetkilendirme kimlik bilgilerini nasıl sağlayacağınızı seçin

Azure Etkin Dizini (AD) kullanarak veya Paylaşılan Erişim İmzası (SAS) belirteci kullanarak yetkilendirme kimlik bilgileri sağlayabilirsiniz.

Bu tabloyu kılavuz olarak kullanın:

| Depolama türü | Şu anda desteklenen yetkilendirme yöntemi |
|--|--|
|**Blob depolama** | Azure AD & SAS |
|**Blob depolama (hiyerarşik ad alanı)** | Azure AD & SAS |
|**Dosya depolama** | Yalnızca SAS |

### <a name="option-1-use-azure-active-directory"></a>Seçenek 1: Azure Etkin Dizini Kullanma

Azure Etkin Dizin'i kullanarak, her komuta bir SAS belirteci eklemek yerine bir kez kimlik bilgileri sağlayabilirsiniz.  

> [!NOTE]
> Geçerli sürümde, depolama hesapları arasında blobs kopyalamayı planlıyorsanız, her kaynak URL için bir SAS belirteci eklemek gerekir. SAS belirtecinizi yalnızca hedef URL'den atlayabilirsiniz. Örneğin, [bkz.](storage-use-azcopy-blobs.md)

İhtiyacınız olan yetkilendirme düzeyi, dosyaları yüklemeyi mi yoksa sadece indirmeyi mi planladığınıztemeldir.

Yalnızca dosyaları indirmek istiyorsanız, [Depolama Blob Veri Okuyucusunun](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader) kullanıcı kimliğinize, yönetilen kimliğinize veya hizmet sorumlusuna atandığını doğrulayın.

> Kullanıcı kimlikleri, yönetilen kimlikler ve hizmet ilkelerinin her biri bir güvenlik *ilkesi*türüdür, bu nedenle bu makalenin geri kalanı için *güvenlik ilkesi* terimini kullanırız.

Dosya yüklemek istiyorsanız, bu rollerden birinin güvenlik müdürünüze atandığını doğrulayın:

- [Depolama Blob Veri Katılımcısı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor)
- [Depolama Blob Veri Sahibi](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner)

Bu roller, bu kapsamlardan herhangi birinde güvenlik müdürünüze atanabilir:

- Konteyner (dosya sistemi)
- Depolama hesabı
- Kaynak grubu
- Abonelik

Rolleri nasıl doğrulayacağınıve atamasını öğrenmek için Azure [portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni ne](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)rendele' ye bakın.

> [!NOTE]
> RBAC rol atamalarının yayılması beş dakika kadar sürebilir.

Güvenlik yöneticiniz hedef kapsayıcının veya dizinin erişim denetim listesine (ACL) eklenirse, bu rollerden birinin güvenlik yöneticinize atanması gerekmez. ACL'de, güvenlik müdürünün hedef dizine izin yazması ve kapsayıcı ve her üst dizinde izin yürütmesi gerekir.

Daha fazla bilgi için [Azure Veri Gölü Depolama Gen2'de Access denetimine](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bakın.

#### <a name="authenticate-a-user-identity"></a>Kullanıcı kimliğini doğrulama

Kullanıcı kimliğinize gerekli yetkilendirme düzeyinin verildiğini doğruladıktan sonra bir komut istemi açın, aşağıdaki komutu yazın ve ENTER tuşuna basın.

```azcopy
azcopy login
```

Birden fazla kuruluşa aitseniz, depolama hesabının ait olduğu kuruluşun kiracı kimliğini ekleyin.

```azcopy
azcopy login --tenant-id=<tenant-id>
```

Yer `<tenant-id>` tutucuyu depolama hesabının ait olduğu kuruluşun kiracı kimliğiyle değiştirin. Kiracı kimliğini bulmak için Azure portalında **Azure Active Directory > Properties > Directory ID'yi** seçin.

Bu komut, bir kimlik doğrulama kodunu ve bir web sitesinin URL'sini döndürür. Web sitesini açın, kodu girin ve **sonra Sonraki** düğmesini seçin.

![Bir kapsayıcı oluşturma](media/storage-use-azcopy-v10/azcopy-login.png)

Oturum açma penceresi görüntülenir. Bu pencerede, Azure hesap kimlik bilgilerinizi kullanarak Azure hesabınızda oturum açın. Başarılı bir şekilde oturum açtıktan sonra tarayıcı penceresini kapatıp AzCopy kullanmaya başlayabilirsiniz.

<a id="service-principal" />

#### <a name="authenticate-a-service-principal"></a>Hizmet ilkesini doğrulama

AzCopy'yi, özellikle şirket içinde çalışırken, kullanıcı etkileşimi olmadan çalışan bir komut dosyasının içinde kullanmayı planlıyorsanız, bu harika bir seçenektir. Azure'da çalışan VM'lerde AzCopy çalıştırmayı planlıyorsanız, yönetilen bir hizmet kimliğini yönetmek daha kolaydır. Daha fazla bilgi edinmek için, bu makalenin yönetilen kimlik bölümünü [kimlik doğrulama](#managed-identity) bölümüne bakın.

Bir komut dosyası çalıştırmadan önce, azcopy'yi hizmet müdürünüzin kimlik bilgilerini sağlayabilmeniz için etkileşimli olarak en az bir kez oturum açmanız gerekir.  Bu kimlik bilgileri, komut dosyanızın bu hassas bilgileri sağlamak zorunda kalmaması için güvenli ve şifrelenmiş bir dosyada depolanır.

Hesabınızda bir istemci sırrı kullanarak veya servis müdürünün uygulama kaydıyla ilişkili bir sertifikanın parolasını kullanarak oturum açabilirsiniz.

Hizmet sorumlusu oluşturma hakkında daha fazla bilgi edinmek için [bkz: Kaynaklara erişebilen bir Azure AD uygulaması ve hizmet ilkesi oluşturmak için portalı kullanın.](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)

Genel olarak hizmet ilkeleri hakkında daha fazla bilgi edinmek için [Azure Etkin Dizini'ndeki Uygulama ve hizmet temel nesneleri'ne](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) bakın

##### <a name="using-a-client-secret"></a>İstemci sırrını kullanma

Ortam değişkenini `AZCOPY_SPA_CLIENT_SECRET` servis müdürünün uygulama kaydının istemci sırrına ayarlayarak başlayın.

> [!NOTE]
> Bu değeri işletim sisteminizin ortam değişken ayarlarında değil, komut isteminizden ayarladıktan emin olun. Bu şekilde, değer yalnızca geçerli oturum için kullanılabilir.

Bu örnek, PowerShell'de bunu nasıl yapabileceğinizi gösterir.

```azcopy
$env:AZCOPY_SPA_CLIENT_SECRET="$(Read-Host -prompt "Enter key")"
```

> [!NOTE]
> Bu örnekte gösterildiği gibi bir istem kullanmayı düşünün. Bu şekilde, parolanız konsolunuzun komut geçmişinde görünmez.  

Ardından, aşağıdaki komutu yazın ve enter tuşuna basın.

```azcopy
azcopy login --service-principal --application-id <application-id> --tenant-id=<tenant-id>
```

Yer `<application-id>` tutucuyu servis müdürünün uygulama kaydının başvuru kimliğiyle değiştirin. Yer `<tenant-id>` tutucuyu depolama hesabının ait olduğu kuruluşun kiracı kimliğiyle değiştirin. Kiracı kimliğini bulmak için Azure portalında **Azure Active Directory > Properties > Directory ID'yi** seçin. 

##### <a name="using-a-certificate"></a>Sertifika kullanma

Yetkilendirme için kendi kimlik bilgilerinizi kullanmayı tercih ederseniz, uygulama kaydınıza bir sertifika yükleyebilir ve oturum açmak için bu sertifikayı kullanabilirsiniz.

Sertifikanızı uygulama kaydınıza yüklemenin yanı sıra, sertifikanın bir kopyasının AzCopy'nin çalışacağı makineye veya VM'ye kaydedilmiş olması gerekir. Sertifikanın bu kopyası . PFX veya . PEM biçimi ve özel anahtar içermelidir. Özel anahtar parola korumalı olmalıdır. Windows kullanıyorsanız ve sertifikanız yalnızca bir sertifika deposunda varsa, bu sertifikayı bir PFX dosyasına (özel anahtar dahil) dışa aktardığınızdan emin olun. Kılavuz için [bkz.](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate?view=win10-ps)

Ardından, ortam `AZCOPY_SPA_CERT_PASSWORD` değişkenini sertifika parolasına ayarlayın.

> [!NOTE]
> Bu değeri işletim sisteminizin ortam değişken ayarlarında değil, komut isteminizden ayarladıktan emin olun. Bu şekilde, değer yalnızca geçerli oturum için kullanılabilir.

Bu örnek, PowerShell'de bu görevi nasıl yapabileceğinizi gösterir.

```azcopy
$env:AZCOPY_SPA_CERT_PASSWORD="$(Read-Host -prompt "Enter key")"
```

Ardından, aşağıdaki komutu yazın ve enter tuşuna basın.

```azcopy
azcopy login --service-principal --certificate-path <path-to-certificate-file> --tenant-id=<tenant-id>
```

Yer `<path-to-certificate-file>` tutucuyu sertifika dosyasına giden göreli veya tam nitelikli yol ile değiştirin. AzCopy bu sertifikanın yolunu kaydeder, ancak sertifikanın bir kopyasını kaydetmez, bu nedenle sertifikayı yerinde tuttuğundan emin olun. Yer `<tenant-id>` tutucuyu depolama hesabının ait olduğu kuruluşun kiracı kimliğiyle değiştirin. Kiracı kimliğini bulmak için Azure portalında **Azure Active Directory > Properties > Directory ID'yi** seçin.

> [!NOTE]
> Bu örnekte gösterildiği gibi bir istem kullanmayı düşünün. Bu şekilde, parolanız konsolunuzun komut geçmişinde görünmez. 

<a id="managed-identity" />

#### <a name="authenticate-a-managed-identity"></a>Yönetilen kimliğin doğrulamasını

Kullanıcı etkileşimi olmadan çalışan bir komut dosyasının içinde AzCopy kullanmayı planlıyorsanız ve komut dosyası bir Azure Sanal Makine (VM) çalışır, bu harika bir seçenektir. Bu seçeneği kullanırken, VM'de herhangi bir kimlik belgesi depolamanız gerekmeyecek.

VM'nizde etkinleştirdiğiniz sistem genelinde yönetilen bir kimliği kullanarak veya VM'nize atadığınız kullanıcı tarafından atanmış yönetilen bir kimliğin istemci kimliğini, Nesne Kimliğini veya Kaynak Kimliğini kullanarak hesabınızda oturum açabilirsiniz.

Sistem genelinde yönetilen bir kimliği etkinleştirme veya kullanıcı tarafından atanmış yönetilen bir kimlik oluşturma hakkında daha fazla bilgi edinmek için Azure [portalını kullanarak Bir VM'de Azure kaynakları için yönetilen kimlikleri yapılandırma'ya](../../active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm.md#enable-system-assigned-managed-identity-on-an-existing-vm)bakın.

##### <a name="using-a-system-wide-managed-identity"></a>Sistem genelinde yönetilen bir kimlik kullanma

Öncelikle, VM'nizde sistem genelinde yönetilen bir kimliği etkinleştirdiğinizden emin olun. [Bkz. Sistem atanmış yönetilen kimlik.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity)

Ardından, komut konsolunuzda aşağıdaki komutu yazın ve enter tuşuna basın.

```azcopy
azcopy login --identity
```

##### <a name="using-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen bir kimlik kullanma

Öncelikle, VM'nizde kullanıcı tarafından atanmış yönetilen bir kimliği etkinleştirdiğinizden emin olun. [Bkz. Kullanıcı tarafından atanan yönetilen kimlik.](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#user-assigned-managed-identity)

Ardından, komut konsolunuzda aşağıdaki komutlardan birini yazın ve ENTER tuşuna basın.

```azcopy
azcopy login --identity --identity-client-id "<client-id>"
```

Yer `<client-id>` tutucuyu kullanıcı tarafından atanan yönetilen kimliğin istemci kimliğiyle değiştirin.

```azcopy
azcopy login --identity --identity-object-id "<object-id>"
```

Yer `<object-id>` tutucuyu kullanıcı tarafından atanan yönetilen kimliğin nesne kimliğiyle değiştirin.

```azcopy
azcopy login --identity --identity-resource-id "<resource-id>"
```

Yer `<resource-id>` tutucuyu kullanıcı tarafından atanan yönetilen kimliğin kaynak kimliğiyle değiştirin.

### <a name="option-2-use-a-sas-token"></a>Seçenek 2: SAS belirteci kullanma

AzCopy komutlarınızda kullanılan her kaynak veya hedef URL'ye bir SAS belirteci ekleyebilirsiniz.

Bu örnek komut, verileri yerel bir dizinden blob kapsayıcısına özyinelemeyle kopyalar. Kapsayıcı URL'sinin sonuna hayali bir SAS belirteci eklenir.

```azcopy
azcopy copy "C:\local\path" "https://account.blob.core.windows.net/mycontainer1/?sv=2018-03-28&ss=bjqt&srt=sco&sp=rwddgcup&se=2019-05-01T05:01:17Z&st=2019-04-30T21:01:17Z&spr=https&sig=MGCXiyEzbtttkr3ewJIh2AR8KrghSy1DGM9ovN734bQF4%3D" --recursive=true
```

SAS belirteçleri ve nasıl elde edilebildiğim hakkında daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-sas-overview)

## <a name="transfer-files"></a>Dosyaları aktarma

Kimliğinizi doğruladıktan veya Bir SAS belirteci aldıktan sonra dosyaları aktarmaya başlayabilirsiniz.

Örnek komutları bulmak için bu makalelerden herhangi birini görün.

- [AzCopy ve blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)

- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 kovaları ile veri aktarımı](storage-use-azcopy-s3.md)

- [AzCopy ve Azure Yığını depolama alanı yla veri aktarımı](https://docs.microsoft.com/azure-stack/user/azure-stack-storage-transfer#azcopy)

## <a name="use-azcopy-in-a-script"></a>Komut dosyasında AzCopy'i kullanma

### <a name="obtain-a-static-download-link"></a>Statik indirme bağlantısı edinin

Zaman içinde, AzCopy [indirme bağlantısı](#download-and-install-azcopy) AzCopy'nin yeni sürümlerini işaret edecektir. Komut dosyanız AzCopy'yi karşıdan yükleiyorsa, AzCopy'nin daha yeni bir sürümü komut dosyanızın bağlı olduğu özellikleri değiştirirse komut dosyası çalışmayı durdurabilir.

Bu sorunları önlemek için, AzCopy'nin geçerli sürümüne statik (değişmeyen) bir bağlantı edinin. Bu şekilde, komut dosyanız her çalıştığında AzCopy'nin aynı sürümünü indirir.

Bağlantıyı elde etmek için şu komutu çalıştırın:

| İşletim sistemi  | Komut |
|--------|-----------|
| **Linux** | `curl -v https://aka.ms/downloadazcopy-v10-linux` |
| **Windows** | `(curl https://aka.ms/downloadazcopy-v10-windows -MaximumRedirection 0 -ErrorAction silentlycontinue).RawContent` |

> [!NOTE]
> Linux için, `--strip-components=1` `tar` komut üzerinde sürüm adını içeren üst düzey klasörü kaldırır ve bunun yerine doğrudan geçerli klasöre ikili ayıklar. Bu, komut dosyasının `azcopy` yalnızca URL'yi güncelleştirerek `wget` yeni bir sürümüyle güncelleştirilmesini sağlar.

URL bu komutun çıktısında görünür. Komut dosyanız daha sonra bu URL'yi kullanarak AzCopy'yi indirebilir.

| İşletim sistemi  | Komut |
|--------|-----------|
| **Linux** | `wget -O azcopy_v10.tar.gz https://aka.ms/downloadazcopy-v10-linux && tar -xf azcopy_v10.tar.gz --strip-components=1` |
| **Windows** | `Invoke-WebRequest https://azcopyvnext.azureedge.net/release20190517/azcopy_windows_amd64_10.1.2.zip -OutFile azcopyv10.zip <<Unzip here>>` |

### <a name="escape-special-characters-in-sas-tokens"></a>SAS jetonlarında özel karakterlerden kaçış

Uzantılı `.cmd` toplu iş dosyalarında, SAS `%` belirteçlerinde görünen karakterlerden kaçmanız gerekir. Bunu, SAS belirteci dizesinde varolan `%` `%` karakterlerin yanına ek bir karakter ekleyerek yapabilirsiniz.

### <a name="run-scripts-by-using-jenkins"></a>Jenkins'i kullanarak komut dosyalarını çalıştırma

Komut dosyalarını çalıştırmak için [Jenkins'i](https://jenkins.io/) kullanmayı planlıyorsanız, komut dosyasının başına aşağıdaki komutu yerleştirdiğinizden emin olun.

```
/usr/bin/keyctl new_session
```

## <a name="use-azcopy-in-azure-storage-explorer"></a>Azure Depolama Gezgini'nde AzCopy'yi kullanma

[Depolama Gezgini,](https://azure.microsoft.com/features/storage-explorer/) tüm veri aktarım işlemlerini gerçekleştirmek için AzCopy'i kullanır. AzCopy'nin performans avantajlarından yararlanmak istiyorsanız [Storage Explorer'ı](https://azure.microsoft.com/features/storage-explorer/) kullanabilirsiniz, ancak dosyalarınızla etkileşimde kalmak için komut satırı yerine grafiksel kullanıcı arabirimi kullanmayı tercih edersiniz.

Depolama Gezgini işlemleri gerçekleştirmek için hesap anahtarınızı kullanır, bu nedenle Depolama Gezgini'nde oturum açtıktan sonra ek yetkilendirme kimlik bilgileri sağlamanız gerekmez.

<a id="previous-version" />

## <a name="use-the-previous-version-of-azcopy"></a>AzCopy önceki sürümünü kullanma

AzCopy'nin önceki sürümünü (AzCopy v8.1) kullanmanız gerekiyorsa, aşağıdaki bağlantılardan herhangi birini görün:

- [Windows üzerinde AzCopy (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy)

- [Linux üzerinde AzCopy (v8)](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy-linux)

## <a name="configure-optimize-and-troubleshoot-azcopy"></a>AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme

Bkz. [AzCopy'yi Yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız, sorunlarınız veya genel geri bildiriminiz varsa, bunları [GitHub](https://github.com/Azure/azure-storage-azcopy) sayfasında gönderin.
