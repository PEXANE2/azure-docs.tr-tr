---
title: Azure'da OpenShift Kapsayıcı Platformu 3.11 Kendi Kendini Yöneten Pazar Teklifi dağıtın
description: OpenShift Kapsayıcı Platformu 3.11 Azure'da Kendi Kendini Yöneten Pazar Teklifi'ni dağıtın.
author: haroldwongms
manager: mdotson
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 1cf6c7417aa86d47e59e08786e7807e32c175a25
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759582"
---
# <a name="configure-prerequisites"></a>Ön koşulları yapılandırma

Azure'da kendi kendini yöneten OpenShift Container Platform 3.11 kümesini dağıtmak için Market teklifini kullanmadan önce birkaç ön koşul yapılandırılması gerekir.  Bir ssh anahtarı (parola olmadan), Azure anahtar kasası, anahtar kasası sırrı ve bir hizmet sorumlusu oluşturmak için talimatlar için [OpenShift önkoşullar](https://docs.microsoft.com/azure/virtual-machines/linux/openshift-container-platform-3x-prerequisites) makalesini okuyun.

 
## <a name="deploy-using-the-marketplace-offer"></a>Market teklifini kullanarak dağıtma

Kendi kendini yöneten OpenShift Container Platform 3.11 kümesini Azure'a dağıtmanın en basit yolu [Azure Marketi teklifini](https://azuremarketplace.microsoft.com/marketplace/apps/osatesting.open-shift-azure-proxy)kullanmaktır.

Bu seçenek en basitidir, ancak özelleştirme özellikleri de sınırlıdır. Market teklifi OpenShift Konteyner Platformu 3.11.82 dağıtır ve aşağıdaki yapılandırma seçeneklerini içerir:

- **Ana Düğümler**: Üç (3) Ana Düğümler yapılandırılabilir örnek türü.
- **Infra Düğümleri**: Üç (3) Yapılabilen örnek türüne sahip Infra Düğümleri.
- **Düğümler**: Düğüm sayısı (1 ile 9 arasında) ve örnek türü yapılandırılabilir.
- **Disk Türü**: Yönetilen Diskler kullanılır.
- **Ağ :** Yeni veya mevcut Ağ ve özel CIDR aralığı için destek.
- **CNS**: CNS etkinleştirilebilir.
- **Ölçümler**: Hawkular Ölçümleri etkinleştirilebilir.
- **Günlük**: EFK Günlük özelliği etkinleştirilebilir.
- **Azure Bulut Sağlayıcısı**: Varsayılan olarak etkinleştirilebilir, devre dışı edilebilir.

Azure portalının sol üst kısmında, **kaynak oluştur'u**tıklatın, arama kutusuna 'openshift kapsayıcı platformu' girin ve Enter tuşuna basın.

   ![Yeni kaynak arama](media/openshift-marketplace-self-managed/ocp-search.png)  
<br>

Sonuçlar sayfası Red **Hat OpenShift Konteyner Platformu 3.11 Kendi Kendini Yöneten** listede ile açılacaktır. 

   ![Yeni kaynak arama sonucu](media/openshift-marketplace-self-managed/ocp-searchresult.png)  
<br>

Teklifin ayrıntılarını görüntülemek için teklifi tıklayın. Bu teklifi dağıtmak için **Oluştur'u**tıklatın. Gerekli parametreleri girmek için UI görüntülenir. İlk ekran **Basics** bıçağıdır.

   ![Teklif başlık sayfası](media/openshift-marketplace-self-managed/ocp-titlepage.png)  
<br>

**Temel Bilgiler**

Giriş parametrelerinden herhangi biri hakkında yardım almak için parametre adının yanındaki ***i'nin*** üzerine geçin.

Giriş parametreleri için değerleri girin ve **Tamam'ı**tıklatın.

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| VM Admin Kullanıcı Adı | Tüm VM örneklerinde oluşturulacak yönetici kullanıcı |
| Yönetici Kullanıcı için SSH Ortak Anahtarı | VM'ye giriş yapmak için kullanılan SSH ortak anahtarı - parolalı bir ifadeye sahip olmamalıdır |
| Abonelik | Kümedağıtmak için Azure aboneliği |
| Kaynak Grubu | Küme kaynakları için yeni bir kaynak grubu oluşturun veya varolan boş bir kaynak grubu seçin |
| Konum | Kümedağıtmak için Azure bölgesi |

   ![Teklif temelbıçak](media/openshift-marketplace-self-managed/ocp-basics.png)  
<br>

**Altyapı Ayarları**

Giriş parametreleri için değerleri girin ve **Tamam'ı**tıklatın.

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| OCP Küme Adı Öneki | Tüm düğümler için ana bilgisayar adlarını yapılandırmak için kullanılan Küme Öneki. 1 ile 20 karakter arasında |
| Ana Düğüm Boyutu | Varsayılan VM boyutunu kabul edin veya farklı bir VM boyutu seçmek için **boyutu değiştir'i** tıklatın.  İş yükünüz için uygun VM boyutunu seçin |
| Altyapı Düğümü Boyutu | Varsayılan VM boyutunu kabul edin veya farklı bir VM boyutu seçmek için **boyutu değiştir'i** tıklatın.  İş yükünüz için uygun VM boyutunu seçin |
| Uygulama Düğümleri Sayısı | Varsayılan VM boyutunu kabul edin veya farklı bir VM boyutu seçmek için **boyutu değiştir'i** tıklatın.  İş yükünüz için uygun VM boyutunu seçin |
| Uygulama Düğümü Boyutu | Varsayılan VM boyutunu kabul edin veya farklı bir VM boyutu seçmek için **boyutu değiştir'i** tıklatın.  İş yükünüz için uygun VM boyutunu seçin |
| Burç Host Boyutu | Varsayılan VM boyutunu kabul edin veya farklı bir VM boyutu seçmek için **boyutu değiştir'i** tıklatın.  İş yükünüz için uygun VM boyutunu seçin |
| Yeni veya Mevcut Sanal Ağ | Yeni bir vNet (Varsayılan) oluşturun veya varolan bir vNet kullanın |
| Varsayılan CIDR Ayarlarını seçin veya IP Aralığını (CIDR) özelleştirin | Varsayılan CIDR aralıklarını kabul edin veya **Özel IP Aralığı** seçin ve özel CIDR bilgilerini girin.  Varsayılan Ayarlar 10.0.0.0/14 CIDR ile vNet, 10.1.0.0/16 ile ana alt ağ, 10.2.0.0/16 ile infra subnet ve 10.3.0.0/16 ile hesaplama ve cns alt net oluşturacaktır |
| Anahtar Vault Kaynak Grup Adı | Anahtar Kasası'nı içeren Kaynak Grubunun adı |
| Anahtar Kasa Adı | Ssh private tuşu ile gizli içeren Key Vault adı.  Yalnızca alfasayısal karakterlere ve tirelere izin verilir ve 3 ile 24 karakter arasında olmalıdır |
| Gizli Ad | Ssh özel anahtarı içeren sırrın adı.  Yalnızca alfasayısal karakterlere ve tirelere izin verilir |

   ![Teklif altyapı bıçak](media/openshift-marketplace-self-managed/ocp-inframain.png)  
<br>

**Boyutu değiştir**

Farklı bir VM boyutu seçmek için ***boyutu değiştir'i***tıklatın.  VM seçim penceresi açılır.  İstediğiniz VM boyutunu seçin ve **Seç'i**tıklatın.

   ![VM Boyutunu seçin](media/openshift-marketplace-self-managed/ocp-selectvmsize.png)  
<br>

**Mevcut Sanal Ağ**

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| Mevcut Sanal Ağ Adı | Varolan vNet'in adı |
| Ana düğümler için alt ağ adı | Ana düğümler için varolan alt netin adı.  En az 16 IP adresi içermesi ve RFC 1918'i izlemesi gerekiyor |
| Infra düğümleri için alt ağ adı | Infra düğümleri için varolan alt netin adı.  En az 32 IP adresi içermesi ve RFC 1918'i izlemesi gerekiyor |
| İşlem ve cns düğümleri için alt ağ adı | İşlem ve cns düğümleri için varolan alt ağın adı.  En az 32 IP adresi içermesi ve RFC 1918'i izlemesi gerekiyor |
| Varolan Sanal Ağ için Kaynak Grubu | Varolan vNet'i içeren kaynak grubunun adı |

   ![Mevcut vnet altyapı teklif edin](media/openshift-marketplace-self-managed/ocp-existingvnet.png)  
<br>

**Özel IP Aralığı**

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| Sanal Ağ için Adres Aralığı | vNet için özel CIDR |
| Ana düğümleri içeren alt ağ için Adres Aralığı | Ana subnet için özel CIDR |
| Altyapı düğümlerini içeren alt ağ için Adres Aralığı | Altyapı alt ağı için özel CIDR |
| İşlem ve cns düğümlerini içeren alt ağ için Adres Aralığı | İşlem ve cns düğümleri için özel CIDR |

   ![Teklif altyapısına özel IP aralığı](media/openshift-marketplace-self-managed/ocp-customiprange.png)  
<br>

**OpenShift Kapsayıcı Platformu 3.11**

Giriş Parametreleri için değerleri girin ve **Tamam'ı** tıklatın

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| OpenShift Admin Kullanıcı Şifresi | İlk OpenShift kullanıcısının parolası.  Bu kullanıcı da küme yöneticisi olacak |
| OpenShift Yönetici Kullanıcı Parolasını Onayla | OpenShift Yönetici Kullanıcı Şifresini Yeniden Yazın |
| Red Hat Abonelik Yöneticisi Kullanıcı Adı | Red Hat Aboneliğinize veya Kuruluş Kimliğinize erişmek için kullanıcı adı.  Bu kimlik bilgisi RHEL örneğini aboneliğinize kaydetmek için kullanılır ve Microsoft veya Red Hat tarafından depolanmaz |
| Red Hat Abonelik Yöneticisi Kullanıcı Şifresi | Red Hat Aboneliğinize veya Etkinleştirme Anahtarınıza erişmek için parola.  Bu kimlik bilgisi RHEL örneğini aboneliğinize kaydetmek için kullanılır ve Microsoft veya Red Hat tarafından depolanmaz |
| Red Hat Abonelik Yöneticisi OpenShift Havuz Kimliği | OpenShift Kapsayıcı Platformu yetkilendirmesi içeren Havuz Kimliği. Kümenin kurulumu için OpenShift Konteyner Platformu'ndan yeterli yetkiye sahip olduğundan emin olun |
| Broker / Master Düğümleri için Red Hat Abonelik Yöneticisi OpenShift Havuz Kimliği | Broker / Master Düğümler için OpenShift Konteyner Platformu yetkilendirmeleri içeren Havuz Kimliği. Kümenin yüklenmesi için OpenShift Konteyner Platformu'ndan yeterli yetkiye sahip olduğundan emin olun. Broker / ana havuz kimliği kullanmıyorsanız, Uygulama Düğümleri için havuz kimliğini girin |
| Azure Bulut Sağlayıcısını Yapılandırma | Azure Bulut Sağlayıcısı'nı kullanmak için OpenShift'i yapılandırın. Kalıcı birimler için Azure disk ekleme kullanıyorsanız gereklidir.  Varsayılan değer Evet'tir |
| Azure AD Hizmeti Temel İstemci Kimliği GUID | Azure AD Service Principal Client ID GUID - AppID olarak da bilinir. Yalnızca Azure Bulut Sağlayıcısını Yapılandırmak **Evet** olarak ayarlanmışsa gereklidir |
| Azure AD Hizmeti Ana İstemci Kimliği Gizli | Azure AD Hizmeti Ana İstemci Kimliği Gizli. Yalnızca Azure Bulut Sağlayıcısını Yapılandırmak **Evet** olarak ayarlanmışsa gereklidir |
 
   ![OpenShift bıçak teklif edin](media/openshift-marketplace-self-managed/ocp-ocpmain.png)  
<br>

**Ek Ayarlar**

Ek Ayarlar bıçağı, glusterfs depolama, Günlük, Ölçümler ve Router Alt etki alanı için CNS yapılandırmasına olanak tanır.  Varsayılan, bu seçeneklerden hiçbirini yüklemez ve nip.io sınama amacıyla yönlendirici alt etki alanı olarak kullanır. CNS'yi etkinleştirmek, glusterfs bölmelerini barındıracak üç ek ekli diskiçeren üç ek bilgi işlem düğümü yükler.  

Giriş Parametreleri için değerleri girin ve **Tamam'ı** tıklatın

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| Yapılaşı Yerel Depolama (CNS) | CNS'yi OpenShift kümesine yükler ve depolama alanı olarak etkinleştirin. Azure Sağlayıcısı devre dışı bırakılırsa varsayılan olur |
| Küme Günlüğe Kaydetmeyi Yapılandır | EFK günlüğe kaydetme işlevini kümeye yükler.  EFK bölmelerini barındırmak için uygun boyut altyapı düğümleri |
| Küme için Ölçümleri Yapılandırma | OpenShift kümesine Hawkular ölçümlerini yükler.  Hawkular ölçüm bölmelerini barındırmak için uygun boyut altyapı düğümleri |
| Varsayılan Yönlendirici Alt etki alanı | Üretim için kendi alt etki alanınızı girmek için test veya özel için nipio'nuzu seçin |
 
   ![Ek bıçak sunun](media/openshift-marketplace-self-managed/ocp-additionalmain.png)  
<br>

**Ek Ayarlar - Ekstra Parametreler**

| Giriş Parametresi | Parametre Açıklaması |
|-----------------------|-----------------|
| (CNS) Düğüm Boyutu | Varsayılan düğüm boyutunu kabul edin veya yeni bir VM boyutu seçmek için **Değiştir boyutunu** seçin |
| Özel alt etki alanınızı girin | OpenShift kümesindeki yönlendirici üzerinden uygulamaları açığa çıkarmak için kullanılacak özel yönlendirme etki alanı.  Uygun joker karakter DNS girişini oluşturduğunuzdan emin olun] |
 
   ![Teklif ek cns Yükleyin](media/openshift-marketplace-self-managed/ocp-additionalcnsall.png)  
<br>

**Özet**

Doğrulama, küme için seçilen toplam VM sayısını dağıtmak için çekirdek kotasını denetlemek için bu aşamada gerçekleşir.  Girilen tüm parametreleri gözden geçirin.  Girişler kabul edilebilirse, devam etmek için **Tamam'ı** tıklatın.

   ![Teklif özeti bıçak](media/openshift-marketplace-self-managed/ocp-summary.png)  
<br>

**Satın Al**

Satın Al sayfasındaki iletişim bilgilerini onaylayın ve Kullanım koşullarını kabul etmek ve OpenShift Kapsayıcı Platformu kümesinin dağıtımını başlatmak için **Satın Al'ı** tıklatın.

   ![Teklif satın alma bıçağı](media/openshift-marketplace-self-managed/ocp-purchase.png)  
<br>


## <a name="connect-to-the-openshift-cluster"></a>OpenShift kümesine bağlanma

Dağıtım bittiğinde, bağlantıyı dağıtımın çıktı bölümünden alın. **OpenShift Konsol URL'sini**kullanarak openshift konsoluna tarayıcınızla bağlanın. Ayrıca Bastion ana bilgisayara SSH yapabilirsiniz. Aşağıda yönetici kullanıcı adı clusteradmin ve burç kamu IP DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com bir örnektir:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubunu, OpenShift kümesini ve ilgili tüm kaynakları artık ihtiyaç duyulmadığında kaldırmak için [az grubu silme](/cli/azure/group) komutunu kullanın.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Sonraki adımlar

- [Dağıtım sonrası görevler](./openshift-container-platform-3x-post-deployment.md)
- [Azure'da OpenShift dağıtımını sorun giderme](./openshift-container-platform-3x-troubleshooting.md)
- [OpenShift Konteyner Platformu ile başlarken](https://docs.openshift.com)
- 