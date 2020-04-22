---
title: Azure önkoşullarda OpenShift Konteyner Platformu 3.11
description: OpenShift Kapsayıcı Platformu 3.11'i Azure'da dağıtmak için ön koşullar.
author: haroldwongms
manager: mdotson
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: article
ms.workload: infrastructure
ms.date: 10/23/2019
ms.author: haroldw
ms.openlocfilehash: 26b190515819378309c2b0705efdbc349ecccbe2
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81759513"
---
# <a name="common-prerequisites-for-deploying-openshift-container-platform-311-in-azure"></a>OpenShift Kapsayıcı Platformu 3.11'i Azure'da dağıtmak için ortak ön koşullar

Bu makalede, Azure'da OpenShift Kapsayıcı Platformu veya OKD dağıtmak için ortak ön koşullar açıklanmaktadır.

OpenShift kurulumu nda Ansible oyun kitapları kullanır. Ansible, yükleme adımlarını tamamlamak için tüm küme ana bilgisayarlarına bağlanmak için Secure Shell (SSH) kullanır.

Ansible uzak ana bilgisayarlara SSH bağlantısı yaptığında, parola giremez. Bu nedenle, özel anahtarın parolasıyla ilişkili bir parolası (parola) olamaz veya dağıtım başarısız olur.

Sanal makineler (VM'ler) Azure Kaynak Yöneticisi şablonları aracılığıyla dağıtıldığı için, aynı ortak anahtar tüm Sanal Makinelere erişmek için kullanılır. Karşılık gelen özel anahtar, tüm oyun kitaplarını da çalıştıran VM'de olmalıdır. Bu eylemi güvenli bir şekilde gerçekleştirmek için, özel anahtarı VM'ye geçirmek için bir Azure anahtar kasası kullanılır.

Kapsayıcılar için kalıcı depolama gereksinimi varsa, kalıcı hacimler gereklidir. OpenShift, kalıcı birimler için Azure sanal sabit disklerini (VHD'ler) destekler, ancak öncelikle Azure'un bulut sağlayıcısı olarak yapılandırılması gerekir.

Bu modelde, OpenShift:

- Azure depolama hesabında veya yönetilen bir diskte bir VHD nesnesi oluşturur.
- VHD'yi VM'ye bağlar ve ses düzeyini biçimlendirin.
- Ses seviyesini bölmeye bağlar.

Bu yapılandırmanın çalışması için OpenShift'in bu görevleri Azure'da gerçekleştirmek için izinlere ihtiyacı vardır. Bu amaçla bir hizmet sorumlusu kullanılır. Hizmet sorumlusu, Azure Etkin Dizini'nde kaynaklara izin verilen bir güvenlik hesabıdır.

Hizmet sorumlusunun, kümeyi oluşturan depolama hesaplarına ve VM'lere erişimi olması gerekir. Tüm OpenShift küme kaynakları tek bir kaynak grubuna dağıtılırsa, hizmet ilkesine bu kaynak grubuna izin verilebilir.

Bu kılavuz, ön koşullarla ilişkili yapılarınasıl oluşturulacak açıklanır.

> [!div class="checklist"]
> * OpenShift kümesi için SSH tuşlarını yönetmek için bir anahtar kasası oluşturun.
> * Azure Bulut Sağlayıcısı tarafından kullanılmak üzere bir hizmet ilkesi oluşturun.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma 
[Az giriş](/cli/azure/reference-index) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin veya Cloud Shell'i kullanmak için **Deneyin'i** tıklatın.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Anahtar kasasını barındırmak için özel bir kaynak grubu kullanmalısınız. Bu grup, OpenShift küme kaynaklarının dağıtıldığı kaynak grubundan ayrıdır.

Aşağıdaki örnek, *eastus* konumunda *keyvaultrg* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name keyvaultrg --location eastus
```

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma
Az keyvault oluşturma komutu ile küme için SSH tuşlarını depolamak için bir [anahtar tonoz oluşturun.](/cli/azure/keyvault) Anahtar kasa adı genel olarak benzersiz olmalı ve şablon dağıtımı için etkinleştirilmelidir veya dağıtım "KeyVaultParameterReferenceSecretRetrieveFailed" hatası ile başarısız olur.

Aşağıdaki örnek, *keyvaultrg* kaynak grubunda *keyvault* adlı bir anahtar tonoz oluşturur:

```azurecli 
az keyvault create --resource-group keyvaultrg --name keyvault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a>SSH anahtarı oluşturma 
OpenShift kümesine erişimi güvence altına almak için bir SSH anahtarı gereklidir. Komutu `ssh-keygen` kullanarak (Linux veya macOS üzerinde):
 
```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> SSH anahtar çiftinizin parolası / parolası olamaz.

Windows'daki SSH tuşları hakkında daha fazla bilgi için [Windows'da SSH tuşlarının nasıl oluşturulabildiğini](/azure/virtual-machines/linux/ssh-from-windows)görün. Özel anahtarı OpenSSH biçiminde dışa aktardığından emin olun.

## <a name="store-the-ssh-private-key-in-azure-key-vault"></a>Azure Key Vault'ta SSH özel anahtarını depolama
OpenShift dağıtımı, OpenShift yöneticisine erişimi güvence altına almak için oluşturduğunuz SSH anahtarını kullanır. Dağıtımın SSH tuşunu güvenli bir şekilde alabilmesi için, aşağıdaki komutu kullanarak anahtarı Key Vault'ta saklayın:

```azurecli
az keyvault secret set --vault-name keyvault --name keysecret --file ~/.ssh/openshift_rsa
```

## <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma 
OpenShift, bir kullanıcı adı ve parola veya bir hizmet sorumlusu kullanarak Azure ile iletişim kurar. Azure hizmet ilkesi, OpenShift gibi uygulamalar, hizmetler ve otomasyon araçlarıyla kullanabileceğiniz bir güvenlik kimliğidir. İzinleri, hizmet sorumlusunun Azure'da hangi işlemleri gerçekleştirebileceğine göre denetler ve tanımlarsınız. Hizmet sorumlusunun izinlerini tüm abonelik yerine belirli kaynak gruplarına kapsamak en iyisidir.

[Az reklam sp create-for-rbac](/cli/azure/ad/sp) ile bir hizmet müdürü oluşturun ve OpenShift'in ihtiyaç duyduğu kimlik bilgilerini çıktı.

Aşağıdaki örnekte bir hizmet ilkesi oluşturulur ve katılımcı izinlerini *openshiftrg*adlı bir kaynak grubuna atar.

İlk olarak, *openshiftrg*adlı kaynak grubu oluşturun:

```azurecli
az group create -l eastus -n openshiftrg
```

Hizmet ilkesi oluşturun:

```azurecli
az group show --name openshiftrg --query id
```

Komutun çıktısını kaydedin ve bir sonraki komutta $scope yerine kullanın

```azurecli
az ad sp create-for-rbac --name openshiftsp \
      --role Contributor --scopes $scope \
```

Komuttan döndürülen appId özelliğine ve parolaya dikkat edin:

```json
{
  "appId": "11111111-abcd-1234-efgh-111111111111",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {Strong Password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```

 > [!WARNING] 
 > Bu parolayı tekrar almak mümkün olmayacağından güvenli parolayı yazdığından emin olun.

Hizmet ilkeleri hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)

## <a name="prerequisites-applicable-only-to-resource-manager-template"></a>Yalnızca Kaynak Yöneticisi şablonu için geçerli olan önkoşullar

SSH özel anahtarı **(sshPrivateKey),** Azure AD istemcisi gizli (**aadClientSecret),** OpenShift yönetici şifresi (**openshiftPassword**) ve Red Hat Subscription Manager şifresi veya etkinleştirme anahtarı **(rhsmPasswordOrActivationKey)** için sırlar oluşturulmalıdır.  Ayrıca, özel TLS / SSL sertifikaları kullanılırsa, o zaman altı ek sırlar oluşturulması gerekir - **routingcafile**, **routingcertfile**, **routingkeyfile**, **mastercafile**, **mastercertfile**, ve **masterkeyfile**.  Bu parametreler daha ayrıntılı olarak açıklanacaktır.

Şablon belirli gizli adlar başvurur, bu nedenle yukarıda listelenen kalın lı adları kullanmanız **gerekir** (büyük/küçük harf duyarlı).

### <a name="custom-certificates"></a>Özel Sertifikalar

Varsayılan olarak, şablon OpenShift web konsolu ve yönlendirme etki alanı için kendi imzalı sertifikaları kullanarak bir OpenShift kümesi dağıtır. Özel TLS/SSL sertifikaları kullanmak istiyorsanız, 'routingCertType'ı 'özel' ve 'masterCertType'ı 'özel' olarak ayarlayın.  Sertifikalar için CA, Cert ve Key dosyalarına .pem biçiminde ihtiyacınız vardır.  Biri için özel sertifikalar kullanmak mümkündür, ancak diğeri için değil.

Bu dosyaları Key Vault sırlarında saklamanız gerekir.  Özel anahtar için kullanılanla aynı Anahtar Kasası'nı kullanın.  Şablon, gizli adlar için 6 ek giriş gerektirmek yerine, TLS/SSL sertifika dosyalarının her biri için belirli gizli adlar kullanmak için sabit kodlanır.  Aşağıdaki tablodaki bilgileri kullanarak sertifika verilerini depolayın.

| Gizli Ad      | Sertifika dosyası   |
|------------------|--------------------|
| mastercafile     | ana CA dosyası     |
| mastercertfile   | ana CERT dosyası   |
| masterkeyfile    | ana Anahtar dosyası    |
| yönlendirmecafile    | ca dosyayönlendirme    |
| yönlendirmecertfile  | CERT dosyayı yönlendirme  |
| yönlendirmekeyfile   | yönlendirme Anahtarı dosyası   |

Azure CLI'yi kullanarak sırları oluşturun. Aşağıda bir örnektir.

```azurecli
az keyvault secret set --vault-name KeyVaultName -n mastercafile --file ~/certificates/masterca.pem
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede aşağıdaki konular ele alınmıştır:
> [!div class="checklist"]
> * OpenShift kümesi için SSH tuşlarını yönetmek için bir anahtar kasası oluşturun.
> * Azure Bulut Çözüm Sağlayıcısı tarafından kullanılmak üzere bir hizmet ilkesi oluşturun.

Ardından, bir OpenShift kümesi dağıtın:

- [OpenShift Konteyner Platform'u dağıt](./openshift-container-platform-3x.md)
- [OpenShift Konteyner Platformu Kendi Kendini Yöneten Pazar Teklifi dağıtın](./openshift-container-platform-3x-marketplace-self-managed.md)
