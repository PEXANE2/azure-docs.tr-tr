---
title: Öğretici, Azure CLI - Azure Özel HSM | Microsoft Dokümanlar
description: CLI'yi kullanarak özel bir HSM'nin varolan bir sanal ağa nasıl dağıtılabildiğini gösteren öğretici
services: dedicated-hsm
documentationcenter: na
author: msmbaldwin
manager: rkarlin
editor: ''
ms.service: key-vault
ms.topic: tutorial
ms.custom: mvc, seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2019
ms.author: mbaldwin
ms.openlocfilehash: 76b7a97a5be5e7952b0ac11d93bd68656ff8f1ec
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79454321"
---
# <a name="tutorial-deploying-hsms-into-an-existing-virtual-network-using-cli"></a>Öğretici: CLI kullanarak HSM'leri varolan bir sanal ağa dağıtma

Azure Özel HSM, tüm yönetim kontrolü ve tam yönetim sorumluluğuyla tek müşteri kullanımı için fiziksel bir aygıt sağlar. Fiziksel aygıtların kullanımı, Microsoft'un kapasitenin etkin bir şekilde yönetilmesini sağlamak için aygıt tahsisini denetlemesi gereksinimini oluşturur. Sonuç olarak, bir Azure aboneliği içinde, Özel HSM hizmeti normalde kaynak sağlama için görünmez. Özel HSM hizmetine erişim gerektiren herhangi bir Azure müşterisi, Özel HSM hizmeti için kayıt istemek için öncelikle Microsoft hesap yöneticilerine başvurmalıdır. Ancak bu işlem başarıyla tamamlandığında sağlama mümkün olacaktır. 

Bu öğretici, tipik bir sağlama işlemini gösterir:

- Bir müşterinin zaten sanal bir ağı vardır
- Sanal bir makineleri var.
- HSM kaynaklarını bu varolan ortama eklemeleri gerekir.

Tipik, yüksek kullanılabilirlik, çok bölgeli dağıtım mimarisi aşağıdaki gibi görünebilir:

![çok bölgeli dağıtım](media/tutorial-deploy-hsm-cli/high-availability-architecture.png)

Bu öğretici, bir çift HSM'ye ve gerekli ExpressRoute Ağ Geçidi'nin (yukarıdaki Subnet 1'e bakınız) varolan bir sanal ağa entegre edilmesine odaklanır (yukarıdaki VNET 1'e bakın).  Diğer tüm kaynaklar standart Azure kaynaklarıdır. Yukarıdaki VNET 3'te 4 alt netindeki HSM'ler için de aynı tümleştirme işlemi kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Azure Özel HSM şu anda Azure portalında kullanılamıyor. Hizmetle tüm etkileşim komut satırı veya PowerShell kullanılarak olacaktır. Bu öğretici, Azure Bulut BulutU'ndaki komut satırı (CLI) arabirimini kullanır. Azure CLI'de yeniyseniz, başlangıç yönergelerini buradan izleyin: [Azure CLI 2.0 Başlayın.](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)

Varsayımlar:

- Azure Özel HSM kayıt işlemini tamamladınız
- Hizmetin kullanımı için onayalındınız. Değilse, ayrıntılar için Microsoft hesap temsilcinize başvurun.
- Bu kaynaklar için bir Kaynak Grubu oluşturdunuz ve bu öğreticide dağıtılan yeni ler bu gruba katılacak.
- Yukarıdaki diyagrama göre gerekli sanal ağı, alt ağı ve sanal makineleri zaten oluşturdunuz ve şimdi bu dağıtıma 2 HSM entegre etmek istiyorsunuz.

Aşağıdaki tüm talimatlar Azure portalına çoktan yönlendirdiğinizi ve Bulut Kabuğu'nu\>\_açtığınızı varsayar (portalın sağ üst ağına doğru " " seçeneğini belirleyin).

## <a name="provisioning-a-dedicated-hsm"></a>Özel HSM Sağlama

HSM'lerin sağlanması ve ExpressRoute Ağ Geçidi üzerinden mevcut bir sanal ağa entegre edilmesi ssh kullanılarak doğrulanacaktır. Bu doğrulama, hsm aygıtının diğer yapılandırma etkinlikleri için ulaşılabilirliğini ve temel kullanılabilirliğini sağlamaya yardımcı olur. Aşağıdaki komutlar, HSM kaynaklarını ve ilişkili ağ kaynaklarını oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanır.

### <a name="validating-feature-registration"></a>Özellik Kaydını Doğrulama

Yukarıda belirtildiği gibi, herhangi bir sağlama faaliyeti, Özel HSM hizmetinin aboneliğiniz için kayıtlı olduğunu gerektirir. Bunu doğrulamak için Azure portal bulutu kabuğunda aşağıdaki komutları çalıştırın.

```azurecli
az feature show \
   --namespace Microsoft.HardwareSecurityModules \
   --name AzureDedicatedHSM
```

Aşağıdaki komut, Özel HSM hizmeti için gereken ağ özelliklerini doğrular.

```azurecli
az feature show \
   --namespace Microsoft.Network \
   --name AllowBaremetalServers
```

Her iki komut da "Kayıtlı" durumunu döndürmelidir (aşağıda gösterildiği gibi). Komutlar "Kayıtlı" olarak geri dönmezse, bu hizmetiçin kaydolmanız gerekir, Microsoft hesap temsilcinize başvurun.

![abonelik durumu](media/tutorial-deploy-hsm-cli/subscription-status.png)

### <a name="creating-hsm-resources"></a>HSM kaynakları oluşturma

Bir HSM, müşterilerin sanal ağına sağlanır, böylece sanal ağ ve alt ağ gereklidir. Sanal ağ ve fiziksel aygıt arasındaki iletişimi etkinleştirmek için HSM için bir bağımlılık bir ExpressRoute Ağ Geçidi ve son olarak sanal bir makine Gemalto istemci yazılımı kullanarak HSM aygıtına erişmek için gereklidir. Bu kaynaklar, kullanım kolaylığı için ilgili parametre dosyasıyla birlikte bir şablon dosyasında toplanmıştır. Dosyalar, Doğrudan Microsoft'a başvurarak HSMrequest@Microsoft.comkullanılabilir.

Dosyaları aldıktan sonra, kaynaklar için tercih ettiğiniz adları eklemek için parametre dosyasını doldurmanız gerekir. Satırları "değer" ile edin: "".

- `namingInfix`HSM kaynaklarının adları için önek
- `ExistingVirtualNetworkName`HSM'ler için kullanılan sanal ağın adı
- `DedicatedHsmResourceName1`Veri merkezi damgasındaki HSM kaynağının adı 1
- `DedicatedHsmResourceName2`Datacenter damgasındaki HSM kaynağının adı 2
- `hsmSubnetRange`HSM'ler için Subnet IP Adresi aralığı
- `ERSubnetRange`VNET ağ geçidi için subnet IP Adres aralığı

Bu değişikliklere bir örnek aşağıdaki gibidir:

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "namingInfix": {
      "value": "MyHSM"
    },
    "ExistingVirtualNetworkName": {
      "value": "MyHSM-vnet"
    },
    "DedicatedHsmResourceName1": {
      "value": "HSM1"
    },
    "DedicatedHsmResourceName2": {
      "value": "HSM2"
    },
    "hsmSubnetRange": {
      "value": "10.0.2.0/24"
    },
    "ERSubnetRange": {
      "value": "10.0.255.0/26"
    },
  }
}
```

İlişkili Azure Kaynak Yöneticisi şablon dosyası, bu bilgilerle birlikte 6 kaynak oluşturur:

- Belirtilen VNET'teki HSM'ler için bir alt ağ
- Sanal ağ ağ geçidi için bir alt ağ
- VNET'i HSM aygıtlarına bağlayan sanal ağ ağ geçidi
- Ağ geçidi için genel bir IP adresi
- Damga 1'de bir HSM
- Damga 2'de bir HSM

Parametre değerleri ayarlandıktan sonra, dosyaların kullanılmak üzere Azure portal bulut bulut uyp dosyası paylaşımına yüklenmesi gerekir. Azure portalında , "\>\_" bulut kabuğu simgesinin sağ üst kısmına tıklayın ve bu da ekranın alt kısmını bir komut ortamı yapar. Bunun için seçenekler BASH ve PowerShell ve zaten ayarlanmış değilse BASH seçmelisiniz.

Komut kabuğuaraç çubuğunda bir yükleme/indirme seçeneği vardır ve şablon ve parametre dosyalarını dosya paylaşımınıza yüklemek için bunu seçmelisiniz:

![dosya paylaşımı](media/tutorial-deploy-hsm-cli/file-share.png)

Dosyalar yüklendikten sonra kaynak oluşturmaya hazırsınız. Yeni HSM kaynakları oluşturmadan önce, yerinde olduğundan emin olunması gereken bazı ön koşul kaynakları vardır. Bilgi işlem, HSM ve ağ geçidi için alt ağ aralıklarına sahip bir sanal ağınız olmalıdır. Aşağıdaki komutlar, böyle bir sanal ağ oluşturacak ne bir örnek olarak hizmet vermektedir.

```azurecli
az network vnet create \
  --name myHSM-vnet \
  --resource-group myRG \
  --address-prefix 10.2.0.0/16
  --subnet-name compute
  --subnet-prefix 10.2.0.0/24
```

```azurecli
az network vnet create \
  --vnet-name myHSM-vnet \
  --resource-group myRG \
  --name hsmsubnet \
  --address-prefixes 10.2.1.0/24 \
  --delegations Microsoft.HardwareSecurityModules/dedicatedHSMs
```

```azurecli
az network vnet subnet create \
  --vnet-name myHSM-vnet \
  --resource-group myRG \
  --name GatewaySubnet \
  --address-prefixes 10.2.255.0/26
```

>[!NOTE]
>Sanal ağ için dikkat edilmesi gereken en önemli yapılandırma, HSM aygıtının alt ağının "Microsoft.HardwareSecurityModules/dedicatedHSMs" olarak ayarlanmış delegasyonları olması gerektiğidir.  Bu seçenek ayarlanmadan HSM sağlama çalışması olmaz.

Tüm ön koşullar yerine geçtikten sonra, değerleri benzersiz adlarNızla (en azından kaynak grubu adı) güncelleştirmiş sinizi sağlamak için Azure Kaynak Yöneticisi şablonunu kullanmak için aşağıdaki komutu çalıştırın:

```azurecli
az group deployment create \
   --resource-group myRG  \
   --template-file ./Deploy-2HSM-toVNET-Template.json \
   --parameters ./Deploy-2HSM-toVNET-Params.json \
   --name HSMdeploy \
   --verbose
```

Bu dağıtımın tamamlanması yaklaşık 25 ila 30 dakika sürer ve bu sürenin büyük bir kısmı HSM aygıtları olmalıdır.

![sağlama durumu](media/tutorial-deploy-hsm-cli/progress-status.png)

Dağıtım başarıyla tamamlandığında "provisioningState": "Başarılı" görüntülenir. Mevcut sanal makinenize bağlanabilir ve HSM aygıtının kullanılabilirliğini sağlamak için SSH'yi kullanabilirsiniz.

## <a name="verifying-the-deployment"></a>Dağıtımı Doğrulama

Aygıtların sağlandığını doğrulamak ve aygıt özniteliklerini görmek için aşağıdaki komut kümesini çalıştırın. Kaynak grubunun uygun şekilde ayarlandığından ve kaynak adının parametre dosyasında tam olarak olduğu gibi olduğundan emin olun.

```azurecli
subid=$(az account show --query id --output tsv)
az resource show \
   --ids /subscriptions/$subid/resourceGroups/myRG/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/HSM1
az resource show \
   --ids /subscriptions/$subid/resourceGroups/myRG/providers/Microsoft.HardwareSecurityModules/dedicatedHSMs/HSM2
```

![çıkış sağlama](media/tutorial-deploy-hsm-cli/progress-status2.png)

Artık [Azure kaynak gezginini](https://resources.azure.com/)kullanarak kaynakları da görebilirsiniz.   Explorer'a girdiğinde, soldaki "abonelikleri" genişletin, Özel HSM için özel aboneliğinizi genişletin, "kaynak gruplarını" genişletin, kullandığınız kaynak grubunu genişletin ve son olarak "kaynaklar" öğesini seçin.

## <a name="testing-the-deployment"></a>DağıtımTest

Dağıtımı sınamak, HSM'ye erişebilen sanal bir makineye bağlanma ve ardından doğrudan HSM aygıtına bağlanma durumudur. Bu eylemler HSM'ye ulaşılabilir olduğunu doğrular.
Ssh aracı sanal makineye bağlanmak için kullanılır. Komut aşağıdakine benzer, ancak parametrede belirttiğiniz yönetici adı ve dns adı ile.

`ssh adminuser@hsmlinuxvm.westus.cloudapp.azure.com`

VM IP Adresi de yukarıdaki komutta DNS adı yerine kullanılabilir. Komut başarılı olursa, bir parola ister ve bunu girmeniz gerekir. Sanal makinede oturum açtıktan sonra, HSM ile ilişkili ağ arabirimi kaynağının portalında bulunan özel IP adresini kullanarak HSM'de oturum açabilirsiniz.

![bileşenler listesi](media/tutorial-deploy-hsm-cli/resources.png)

>[!NOTE]
>Seçildiğinde HSM kaynaklarını görüntüleyecek olan "Gizli türleri göster" onay kutusuna dikkat edin.

Yukarıdaki ekran görüntüsünde, "HSM1_HSMnic" veya "HSM2_HSMnic" seçeneğini tıklatarak uygun Özel IP Adresi gösterecektir. Aksi takdirde, yukarıda kullanılan `az resource show` komut doğru IP Adresini tanımlamanın bir yoludur. 

Doğru IP adresine sahip olduğunuzda, bu adresi yerine aşağıdaki komutu çalıştırın:

`ssh tenantadmin@10.0.2.4`

Başarılı olursa, bir parola istenir. Varsayılan parola PAROLA'dır ve HSM önce parolanızı değiştirmenizi ister, böylece güçlü bir parola ayarlayın ve kuruluşunuzun parolayı depolamak ve kaybı önlemek için hangi mekanizmayı tercih edeceğini kullanın.

>[!IMPORTANT]
>Bu parolayı kaybederseniz, HSM'nin sıfırlanması gerekir ve bu da anahtarlarınızı kaybetmek anlamına gelir.

SSH kullanarak HSM'ye bağlandığınızda, HSM'nin çalışır durumda olduğundan emin olmak için aşağıdaki komutu çalıştırın.

`hsm show`

Çıktı aşağıdaki resimde gösterildiği gibi görünmelidir:

![bileşenler listesi](media/tutorial-deploy-hsm-cli/hsm-show-output.png)

Bu noktada, yüksek kullanılabilir, iki HSM dağıtımı ve doğrulanmış erişim ve operasyonel durum için tüm kaynakları ayırdınız. Daha fazla yapılandırma veya test HSM aygıtının kendisi ile daha fazla çalışma içerir. Bunun için, HSM'yi başlatmave bölümler oluşturmak için Gemalto Luna Network HSM 7 Yönetim Kılavuzu bölüm 7'deki yönergeleri izlemeniz gerekir. Gemalto Müşteri Destek Portalı'na kaydolduktan ve müşteri kimliğine sahip olduktan sonra tüm dokümantasyon ve yazılımlar doğrudan Gemalto'dan indirilebilir. Gerekli tüm bileşenleri almak için İstemci Yazılımı sürüm 7.2'yi indirin.

## <a name="delete-or-clean-up-resources"></a>Kaynakları silme veya temizleme

Sadece HSM aygıtıile bitirdiyseniz, kaynak olarak silinebilir ve boş havuza döndürülebilir. Bunu yaparken en belirgin endişe cihazda herhangi bir hassas müşteri verileridir. Bir cihazı "sıfırlamanın" en iyi yolu, HSM yönetici parolasını yanlış 3 kez almaktır (not: bu cihaz yöneticisi değil, gerçek HSM yöneticisidir). Anahtar malzemeyi korumak için bir güvenlik önlemi olarak, aygıt sıfırlanmış duruma gelene kadar Azure kaynağı olarak silinemez.

> [!NOTE]
> Herhangi bir Gemalto cihaz yapılandırması ile ilgili bir sorununuz varsa [Gemalto müşteri desteğine](https://safenet.gemalto.com/technical-support/)başvurmalısınız.


Bu kaynak grubundaki tüm kaynakları bitirdiyseniz, bunları aşağıdaki komutla kaldırabilirsiniz:

```azurecli
az group deployment delete \
   --resource-group myRG \
   --name HSMdeploy \
   --verbose

```

## <a name="next-steps"></a>Sonraki adımlar

Öğreticideki adımları tamamladıktan sonra, Özel HSM kaynakları sağlanır ve HSM ile iletişimi etkinleştirmek için gerekli HSM'lere ve diğer ağ bileşenlerine sahip sanal bir ağa sahip siniz.  Artık bu dağıtıma, tercih ettiğiniz dağıtım mimarisinin gerektirdiği şekilde daha fazla kaynakla iltifat edebilecek bir konumdasınız. Dağıtımınızı planlamaya yardımcı olmak hakkında daha fazla bilgi için Kavramlar belgelerine bakın.
Raf düzeyinde kullanılabilirliği ele alan birincil bölgede iki HSM'li bir tasarım ve bölgesel kullanılabilirliği ele alan ikincil bir bölgede iki HSM önerilir. Bu öğreticide kullanılan şablon dosyası, iki HSM dağıtımı için kolayca temel olarak kullanılabilir, ancak parametrelerinin gereksinimlerinizi karşılamak için değiştirilmesi gerekir.

* [Yüksek Kullanılabilirlik](high-availability.md)
* [Fiziksel Güvenlik](physical-security.md)
* [Ağ Oluşturma](networking.md)
* [Desteklenebilirlik](supportability.md)
* [İzleme](monitoring.md)
