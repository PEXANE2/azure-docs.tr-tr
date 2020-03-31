---
title: Kırmızı Şapka Güncelleme Altyapısı | Microsoft Dokümanlar
description: Microsoft Azure'da isteğe bağlı Red Hat Enterprise Linux örnekleri için Red Hat Update Altyapısı hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: BorisB2015
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/10/2020
ms.author: alsin
ms.openlocfilehash: aa9fd230f59b5e46576e78beb0436c85449d3c5d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80256921"
---
# <a name="red-hat-update-infrastructure-for-on-demand-red-hat-enterprise-linux-vms-in-azure"></a>Azure'da isteğe bağlı Red Hat Enterprise Linux VM'ler için Red Hat Güncelleme Altyapısı
 [Red Hat Update Infrastructure](https://access.redhat.com/products/red-hat-update-infrastructure) (RHUI), Azure gibi bulut sağlayıcılarının Red Hat tarafından barındırılan depo içeriğini yansıtmasına, Azure'a özel içeriğe sahip özel depolar oluşturmasına ve son kullanıcı VM'leri için kullanılabilir hale getirmesine olanak tanır.

Red Hat Enterprise Linux (RHEL) You-You-Go Öde (PAYG) görüntüleri Azure RHUI'ye erişmek için önceden yapılandırılmıştır. Ek yapılandırma gerekmez. En son güncelleştirmeleri `sudo yum update` almak için RHEL örneğiniz hazır olduktan sonra çalıştırın. Bu hizmet RHEL PAYG yazılım ücretlerinin bir parçası olarak dahildir.

Azure'daki RHEL görüntüleri yle ilgili yayımlama ve saklama ilkeleri de dahil olmak üzere ek bilgilere [buradan](./redhat-images.md)ulaşabilirsiniz.

RHEL'in tüm sürümleri için Red Hat destek politikaları hakkında bilgi [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.

> [!IMPORTANT]
> RHUI sadece pay-as-you-go (PAYG) görüntüler için tasarlanmıştır. Kendi aboneliğini getir (BYOS) olarak da bilinen özel ve altın renkli görüntüler için, güncellemeleri almak için sistemin RHSM veya Uydu'ya eklenmesi gerekir. Daha fazla bilgi için [Red Hat makalesine](https://access.redhat.com/solutions/253273) bakın.


## <a name="important-information-about-azure-rhui"></a>Azure RHUI hakkında önemli bilgiler

* Azure RHUI, Azure'da oluşturulan tüm RHEL PAYG VM'leri destekleyen güncelleştirme altyapısıdır. Bu, PAYG RHEL VM'lerinizi Abonelik Yöneticisi ne de Uydu veya başka bir güncelleme kaynağına kaydettirmenizi engellemez, ancak bunu bir PAYG VM ile yapmak dolaylı çift faturalandırmaya neden olur. Ayrıntılar için aşağıdaki noktaya bakın.
* Azure tarafından barındırılan RHUI'ye erişim, RHEL PAYG görüntü fiyatına dahildir. Sanal makineyi kendi lisansını getir (BYOL) vm türüne dönüştürmeyen Azure barındırılan RHUI'den bir PAYG RHEL VM kaydını çıkarırsanız. Aynı VM'yi başka bir güncelleştirme kaynağıyla kaydettirseniz, _dolaylı_ çift ücrete maruz kalabilirsiniz. Azure RHEL yazılım ücreti için ilk kez ücretlendirilirsiniz. Daha önce satın alınan Red Hat abonelikleri için ikinci kez ücretlendirilirsiniz. Azure barındırılan RHUI dışında sürekli bir güncelleştirme altyapısı kullanmanız gerekiyorsa, [RHEL BYOS görüntülerini](./byos.md)kullanmak için kaydolmayı düşünün.

* Azure'daki RHEL PAYG görüntüleri (SAP IÇIN RHEL, SAP HANA için RHEL ve SAP İş Uygulamaları için RHEL) SAP sertifikası için gerekli olan özel RHEL minör sürümünde kalan özel RHUI kanallarına bağlanır.

* Azure tarafından barındırılan RHUI'ye erişim, [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653)içindeki VM'lerle sınırlıdır. Tüm VM trafiğini şirket içi ağ altyapısı üzerinden kullanıyorsanız, Azure RHUI'ye erişmek için RHEL PAYG VM'leri için kullanıcı tanımlı rotalar ayarlamanız gerekebilir. Bu durumda, _tüm_ RHUI IP adresleri için kullanıcı tanımlı yolların eklenmesi gerekir.


## <a name="image-update-behavior"></a>Görüntü güncelleştirme davranışı

Azure, Nisan 2019 itibarıyla, varsayılan olarak Genişletilmiş Güncelleştirme Desteği (EUS) depolarına bağlı RHEL görüntüleri ve varsayılan olarak normal (EUS olmayan) depolara bağlanan RHEL görüntüleri sunar. RHEL EUS hakkında daha fazla bilgi Red Hat sürümü [yaşam döngüsü belgeleri](https://access.redhat.com/support/policy/updates/errata) ve [EUS belgeleri](https://access.redhat.com/articles/rhel-eus)mevcuttur. Farklı görüntüler `sudo yum update` farklı depolara bağlı olduğundan, varsayılan davranış, hangi RHEL görüntüden sağlandığına bağlı olarak değişir.

Tam resim listesi için `az vm image list --publisher redhat --all` Azure CLI'yi kullanarak çalıştırın.

### <a name="images-connected-to-non-eus-repositories"></a>EUS olmayan depolara bağlı görüntüler

EUS olmayan depolara bağlı bir RHEL görüntüden bir VM sağlarsanız, çalıştırdığınızda `sudo yum update`en son RHEL minör sürümüne yükseltilirsiniz. Örneğin, bir RHEL 7.4 PAYG görüntüden bir VM sağlar ve çalıştırın, `sudo yum update`bir RHEL 7.7 VM (RHEL7 ailesinin en son küçük sürümü) ile sona erer.

EUS olmayan depolara bağlı görüntüler SKU'da küçük bir sürüm numarası içermez. SKU, URN'deki üçüncü öğedir (resmin tam adı). Örneğin, aşağıdaki resimlerin tümü EUS olmayan depolara eklenir:

```text
RedHat:RHEL:7-LVM:7.4.2018010506
RedHat:RHEL:7-LVM:7.5.2018081518
RedHat:RHEL:7-LVM:7.6.2019062414
RedHat:RHEL:7-RAW:7.4.2018010506
RedHat:RHEL:7-RAW:7.5.2018081518
RedHat:RHEL:7-RAW:7.6.2019062120
```

SNU'ların 7-LVM veya 7-RAW olduğunu unutmayın. Küçük sürüm, bu görüntülerin sürümünde (URN'deki dördüncü öğe) gösterilir.

### <a name="images-connected-to-eus-repositories"></a>EUS depolarına bağlı görüntüler

EUS depolarına bağlı bir RHEL görüntüsünden bir VM sağlarsanız, çalıştırdığınızda `sudo yum update`en son RHEL minör sürümüne yükseltilmezsiniz. Bunun nedeni, EUS depolarına bağlı görüntülerin kendi özel küçük versiyonlarına da kilitlenmiş olmasıdır.

EUS depolarına bağlı görüntüler, SKU'da küçük bir sürüm numarası içerir. Örneğin, aşağıdaki resimlerin tümü EUS depolarına eklenir:

```text
RedHat:RHEL:7.4:7.4.2019062107
RedHat:RHEL:7.5:7.5.2019062018
RedHat:RHEL:7.6:7.6.2019062116
```

## <a name="rhel-eus-and-version-locking-rhel-vms"></a>RHEL EUS ve sürüm kilitleme RHEL VM'ler

Genişletilmiş Güncelleştirme Desteği (EUS) depoları, VM'yi tedarik ettikten sonra RHEL VM'lerini belirli bir RHEL küçük sürümüne kilitlemek isteyen müşteriler tarafından kullanılabilir. Genişletilmiş Güncelleştirme Desteği depolarını işaret etmek için depoları güncelleyerek RHEL VM'inizi belirli bir küçük sürüme sürüm olarak kilitleyebilirsiniz. EUS sürüm kilitleme işlemini de geri alabilirsiniz.

>[!NOTE]
> EUS, RHEL Extras'da desteklenmez. Bu, genellikle RHEL Extras kanalından kullanılabilen bir paket yüklüyorsanız, Bunu EUS'tayken yapamayacağınız anlamına gelir. Red Hat Extras Ürün Yaşam Döngüsü [burada](https://access.redhat.com/support/policy/updates/extras/)ayrıntılı olarak açıklanır.

Bu yazının yazıldığı sırada RHEL <= 7.4 için EUS desteği sona ermiştir. Daha fazla bilgi için [Red Hat belgelerindeki](https://access.redhat.com/support/policy/updates/errata/) "Red Hat Enterprise Linux Longer Support Eklentileri" bölümüne bakın.
* RHEL 7.4 EUS desteği sona erdi31 Ağustos 2019
* RHEL 7.5 EUS desteği sona eriyor 30 Nisan 2020
* RHEL 7.6 EUS desteği sona erdi 31 Ekim 2020
* RHEL 7.7 EUS desteği sona erdi 30 Ağustos 2021

### <a name="switch-a-rhel-vm-to-eus-version-lock-to-a-specific-minor-version"></a>RHEL VM'yi EUS'a değiştirme (sürüm kilidi belirli bir küçük sürüme)
RhEL VM'yi belirli bir küçük sürüme kilitlemek için aşağıdaki yönergeleri kullanın (kök olarak çalıştırın):

>[!NOTE]
> Bu yalnızca EUS'un kullanılabildiği RHEL sürümleri için geçerlidir. Bu yazının yazıldığı sırada, bu RHEL 7.2-7.7 içerir. Daha fazla bilgi [Red Hat Enterprise Linux Yaşam Döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında mevcuttur.

1. EUS dışı depoları devre dışı:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7'
    ```

1. EUS depoları ekleyin:
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7-eus.config' install 'rhui-azure-rhel7-eus'
    ```

1. Değişkeni `releasever` kilitleyin (kök olarak çalıştırın):
    ```bash
    echo $(. /etc/os-release && echo $VERSION_ID) > /etc/yum/vars/releasever
    ```

    >[!NOTE]
    > Yukarıdaki yönerge, RHEL minör sürümü geçerli küçük sürüm için kilitler. Yükseltme yapmak ve en son olmayan daha sonraki bir küçük sürüme kilitlemek istiyorsanız belirli bir küçük sürüm girin. Örneğin, `echo 7.5 > /etc/yum/vars/releasever` RHEL sürümünüzü RHEL 7.5'e kilitler

1. RHEL VM'nizi güncelleyin
    ```bash
    sudo yum update
    ```

### <a name="switch-a-rhel-vm-back-to-non-eus-remove-a-version-lock"></a>RHEL VM'yi EUS'suz olarak değiştirin (sürüm kilidini kaldırın)
Aşağıdakileri kök olarak çalıştırın:
1. Dosyayı `releasever` kaldırın:
    ```bash
    rm /etc/yum/vars/releasever
     ```

1. EUS depolarını devre dışı:
    ```bash
    yum --disablerepo='*' remove 'rhui-azure-rhel7-eus'
   ```

1. RHEL VM'yi yapılandır
    ```bash
    yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7.config' install 'rhui-azure-rhel7'
    ```

1. RHEL VM'nizi güncelleyin
    ```bash
    sudo yum update
    ```

## <a name="the-ips-for-the-rhui-content-delivery-servers"></a>RHUI içerik dağıtım sunucuları için IP'ler

RHUI, RHEL'in isteğe bağlı görüntülerin indiği tüm bölgelerde mevcuttur. Şu anda Azure durum [panosu](https://azure.microsoft.com/status/) sayfasında listelenen tüm genel bölgeleri, Azure ABD Hükümeti'ni ve Microsoft Azure Almanya bölgelerini içerir.

RHEL PAYG VM'lerden erişimi daha da kısıtlamak için bir ağ yapılandırması `yum update` kullanıyorsanız, içinde olduğunuz ortama bağlı olarak aşağıdaki IP'lerin çalışmasına izin verildiğinden emin olun:


```
# Azure Global
13.91.47.76
40.85.190.91
52.187.75.218
52.174.163.213
52.237.203.198

# Azure US Government
13.72.186.193
13.72.14.155
52.244.249.194

# Azure Germany
51.5.243.77
51.4.228.145
```

## <a name="azure-rhui-infrastructure"></a>Azure RHUI Altyapısı


### <a name="update-expired-rhui-client-certificate-on-a-vm"></a>VM'de süresi dolan RHUI istemci sertifikasını güncelleştirme

Örneğin, RHEL 7.4 (resim URN: `RedHat:RHEL:7.4:7.4.2018010506`), artık süresi dolmuş TLS/SSL istemci sertifikası nedeniyle RHUI'ye bağlantı sorunları yaşarsınız. Gördüğünüz hata _"SSL eş süresi dolmuş olarak sertifikanızı reddetti"_ veya _"Hata: depo için depo meta verileri (repomd.xml) alamıyorum: ... Lütfen onun yolunu doğrulayın ve tekrar deneyin"_. Bu sorunu aşmak için lütfen VM'deki RHUI istemci paketini aşağıdaki komutu kullanarak güncelleyin:

```bash
sudo yum update -y --disablerepo='*' --enablerepo='*microsoft*'
```

Alternatif olarak, `sudo yum update` çalışan istemci sertifika paketini de güncelleştirebilir (RHEL sürümünüze bağlı olarak), diğer depolarda göreceğiniz "süresi dolmuş SSL sertifikası" hatalarına rağmen. Bu güncelleştirme başarılı olursa, diğer RHUI depolarına normal bağlantı geri yüklenmelidir, böylece `sudo yum update` başarılı bir şekilde çalıştırabilirsiniz.

Bir çalıştırırken 404 hatasıyla `yum update`karşınıza çıkarsa, yum önbelleğinizi yenilemek için aşağıdakileri deneyin:
```bash
sudo yum clean all;
sudo yum makecache
```

### <a name="troubleshoot-connection-problems-to-azure-rhui"></a>Azure RHUI'deki bağlantı sorunlarını giderme
Azure RHEL PAYG VM'nizden Azure RHUI'ye bağlanmada sorun yaşıyorsanız aşağıdaki adımları izleyin:

1. Azure RHUI bitiş noktası için VM yapılandırmasını inceleyin:

    1. Dosyanın `/etc/yum.repos.d/rh-cloud.repo` dosya `rhui-[1-3].microsoft.com` `baseurl` `[rhui-microsoft-azure-rhel*]` nın bölümünde bir başvuru bulunıp içermeyip içermeyip bir referans içerdiğini denetleyin. Varsa, yeni Azure RHUI'yi kullanıyorsunuz.

    1. Aşağıdaki desene sahip bir konumu `mirrorlist.*cds[1-4].cloudapp.net`işaret ediyorsa, yapılandırma güncelleştirmesi gereklidir. Eski VM anlık görüntüsünü kullanıyorsunuz ve yeni Azure RHUI'yi işaret etmek için güncellemeniz gerekir.

1. Azure tarafından barındırılan RHUI'ye erişim, [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653)içindeki VM'lerle sınırlıdır.

1. Yeni yapılandırmayı kullanıyorsanız, VM'nin Azure IP aralığından bağladığını ve Azure RHUI'ye hala bağlanadığını doğruladıysanız, Microsoft veya Red Hat'e bir destek örneği dosyalayın.

### <a name="infrastructure-update"></a>Altyapı güncelleştirmesi

Eylül 2016'da güncelleştirilmiş bir Azure RHUI dağıttık. Nisan 2017'de eski Azure RHUI'yi kapattık. Eylül 2016 veya daha sonra RHEL PAYG görüntülerini (veya anlık görüntülerini) kullanıyorsanız, otomatik olarak yeni Azure RHUI'ye bağlanmış oluyorsunuz. Ancak, VM'lerinizde eski anlık görüntüler varsa, aşağıdaki bölümde açıklandığı gibi Azure RHUI'ye erişmek için yapılandırmalarını el ile güncelleştirmeniz gerekir.

Yeni Azure RHUI sunucuları [Azure Trafik Yöneticisi](https://azure.microsoft.com/services/traffic-manager/)ile dağıtılır. Trafik Yöneticisi'nde, bölgeden bağımsız olarak herhangi bir VM tarafından tek bir bitiş noktası (rhui-1.microsoft.com) kullanılabilir.

### <a name="manual-update-procedure-to-use-the-azure-rhui-servers"></a>Azure RHUI sunucularını kullanmak için el ile güncelleştirme yordamı
Bu yordam yalnızca başvuru için sağlanır. RHEL PAYG görüntüleri Azure RHUI'ye bağlanmak için zaten doğru yapılandırmaya sahiptir. Azure RHUI sunucularını kullanmak için yapılandırmayı el ile güncelleştirmek için aşağıdaki adımları tamamlayın:

- RHEL 6 için:
  ```bash
  yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel6.config' install 'rhui-azure-rhel6'
  ```

- RHEL 7 için:
  ```bash
  yum --config='https://rhelimage.blob.core.windows.net/repositories/rhui-microsoft-azure-rhel7.config' install 'rhui-azure-rhel7'
  ```

- RHEL 8 için:
    1. Config dosyası oluşturun:
        ```bash
        vi rhel8.config
        ```
    1. Config dosyasına aşağıdaki içeriği ekleyin:
        ```bash
        [rhui-microsoft-azure-rhel8]
        name=Microsoft Azure RPMs for Red Hat Enterprise Linux 8
        baseurl=https://rhui-1.microsoft.com/pulp/repos/microsoft-azure-rhel8 https://rhui-2.microsoft.com/pulp/repos/microsoft-azure-rhel8 https://rhui-3.microsoft.com/pulp/repos/microsoft-azure-rhel8
        enabled=1
        gpgcheck=1
        gpgkey=https://rhelimage.blob.core.windows.net/repositories/RPM-GPG-KEY-microsoft-azure-release sslverify=1
        ```
    1. Dosyayı kaydedin ve aşağıdaki komutu çalıştırın:
        ```bash
        dnf --config rhel8.config install 'rhui-azure-rhel8'
        ```
    1. VM'nizi güncelleyin
        ```bash
        sudo dnf update
        ```


## <a name="next-steps"></a>Sonraki adımlar
* Azure Marketi PAYG görüntüsünden Red Hat Enterprise Linux VM oluşturmak ve Azure barındırılan RHUI'yi kullanmak için [Azure Marketi'ne](https://azure.microsoft.com/marketplace/partners/redhat/)gidin.
* Azure'daki Red Hat resimleri hakkında daha fazla bilgi edinmek için [belgeler sayfasına](./redhat-images.md)gidin.
* RHEL'in tüm sürümleri için Red Hat destek politikaları hakkında bilgi [Red Hat Enterprise Linux Life Cycle](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.
