---
title: Azure IoT Edge'i Ubuntu Sanal Makinelerde Çalıştırın | Microsoft Dokümanlar
description: Ubuntu 18.04 LTS Sanal Makineler için Azure IoT Edge kurulum talimatları
author: toolboc
manager: veyalla
ms.reviewer: kgremban
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 03/19/2020
ms.author: pdecarlo
ms.openlocfilehash: 64e2787aa282e75893fa34e6de1373e6afed09fe
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80349622"
---
# <a name="run-azure-iot-edge-on-ubuntu-virtual-machines"></a>Azure IoT Edge'i Ubuntu Sanal Makinelerde Çalıştırın

Azure IoT Edge çalışma zamanı, aygıtı IoT Edge aygıtına dönüştüren şeydir. Çalışma süresi Raspberry Pi kadar küçük veya endüstriyel sunucu kadar büyük aygıtlarda dağıtılabilir. Bir aygıt IoT Edge çalışma zamanı ile yapılandırıldıktan sonra, buluttan iş mantığı dağıtmaya başlayabilirsiniz.

IoT Edge çalışma zamanı nın nasıl çalıştığı ve hangi bileşenlerin dahil edildiği hakkında daha fazla bilgi edinmek için azure [IoT Edge çalışma süresini ve mimarisini anlayın.](iot-edge-runtime.md)

Bu makalede, Azure IoT Edge çalışma zamanı yüklenmiş ve önceden sağlanan aygıt bağlantı dizesi kullanılarak yapılandırılan bir Ubuntu 18.04 LTS sanal makinedağıtmak için adımlar listelenir. Dağıtım, [iotedge-vm-deploy](https://github.com/Azure/iotedge-vm-deploy) proje deposunda tutulan [bulut](../virtual-machines/linux/using-cloud-init.md
) giriş tabanlı [Azure Kaynak Yöneticisi şablonu](../azure-resource-manager/templates/overview.md) kullanılarak gerçekleştirilir.

İlk önyüklemede, Ubuntu 18.04 LTS sanal makine [azure IoT Edge çalışma zamanının en son sürümünü bulut lar aracılığıyla yükler.](https://github.com/Azure/iotedge-vm-deploy/blob/master/cloud-init.txt) Ayrıca, çalışma süresi başlamadan önce sağlanan bir bağlantı dizesini ayarlar ve IoT Edge aygıtını SSH veya uzak masaüstü oturumu başlatmaya gerek kalmadan kolayca yapılandırmanızı ve bağlamanızı sağlar. 

## <a name="deploy-using-deploy-to-azure-button"></a>Azure Düğmesine Dağıt'ı kullanarak dağıtma

[Azure'a Dağıt Düğmesi,](../azure-resource-manager/templates/deploy-to-azure-button.md) GitHub'da tutulan [Azure Kaynak Yöneticisi şablonlarının](../azure-resource-manager/templates/overview.md) kolaydağıtımını sağlar.  Bu [bölümde, iotedge-vm-deploy](https://github.com/Azure/iotedge-vm-deploy) proje deposunda yer alan Azure'a Dağıt düğmesinin kullanımı gösterilecek.  


1. iotedge-vm-deploy Azure Kaynak Yöneticisi şablonu kullanarak Azure IoT Edge özellikli bir Linux VM dağıtacağız.  Başlamak için aşağıdaki düğmeyi tıklatın:

    [![iotedge-vm-deploy için Azure Düğmesine dağıt](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fazure%2Fiotedge-vm-deploy%2Fmaster%2FedgeDeploy.json)

1. Yeni başlatılan pencerede, kullanılabilir form alanlarını doldurun:

    > [!div class="mx-imgBorder"]
    > [![iotedge-vm-deploy şablonu gösteren ekran görüntüsü](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-deploy.png)](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-deploy.png)

    **Abonelik**: Sanal makineyi dağıtmak için etkin Azure aboneliği.

    **Kaynak grubu**: Sanal makineyi ve ilişkili kaynakları içeren varolan veya yeni oluşturulan Kaynak Grubu.

    **DNS Etiket Öneki**: Sanal makinenin ana bilgisayar adını önek için kullanılan seçtiğiniz gerekli değerdir.

    **Yönetici Kullanıcı Adı**: Dağıtımda kök ayrıcalıkları sağlanacak kullanıcı adı.

    **Aygıt Bağlantı Dizesi**: Hedefleneneniz [IoT Hub'ı](../iot-hub/about-iot-hub.md)içinde oluşturulan bir aygıt için [aygıt bağlantısı dizesi.](how-to-register-device.md)

    **VM Boyutu**: Dağıtılacak sanal makinenin [boyutu](../cloud-services/cloud-services-sizes-specs.md)

    **Ubuntu OS Sürümü**: Ubuntu OS sürümü temel sanal makineye kurulacak.

    **Konum**: Sanal makineyi dağıtmak için [coğrafi bölge,](https://azure.microsoft.com/global-infrastructure/locations/) bu değer varsayılan olarak seçili Kaynak Grubu'nun konumuna dönüşür.

    **Kimlik Doğrulama Türü**: Tercihinize bağlı olarak **sshPublicKey** veya **şifreyi** seçin.

    **Yönetici Şifresi veya Anahtarı**: Kimlik Doğrulama Türü seçimine bağlı olarak SSH Ortak Anahtarının değeri veya parolanın değeri.

    Tüm alanlar doldurulduğunda, şartları kabul etmek için sayfanın altındaki onay kutusunu seçin ve dağıtıma başlamak için **Satın Al'ı** seçin.

1. Dağıtımın başarıyla tamamlandığını doğrulayın.  Seçilen kaynak grubuna sanal bir makine kaynağı dağıtılmış olmalıdır.  Makine adı dikkat edin, bu biçimde `vm-0000000000000`olmalıdır. Ayrıca, biçiminde `<dnsLabelPrefix>`olması gereken ilişkili **DNS Adı'na**dikkat edin. `<location>`.cloudapp.azure.com.

    **DNS Adı,** Azure portalı içinde yeni dağıtılan sanal makinenin **Genel Bakış** bölümünden elde edilebilir.

    > [!div class="mx-imgBorder"]
    > [![iotedge vm dns adını gösteren ekran görüntüsü](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)

1. Kurulumdan sonra bu VM'ye SSH yapmak istiyorsanız, komutla ilişkili **DNS Adını** kullanın:`ssh <adminUsername>@<DNS_Name>`

## <a name="deploy-from-azure-cli"></a>Azure CLI'den dağıtma

1. Azure CLI iot uzantısını aşağıdakilerle yüklediğinizden emin olun:
    ```azurecli-interactive
    az extension add --name azure-iot
    ```

1. Ardından, masaüstünüzde Azure CLI kullanıyorsanız, oturum açarak başlayın:

   ```azurecli-interactive
   az login
   ```

1. Birden çok aboneliğiniz varsa, kullanmak istediğiniz aboneliği seçin:
   1. Aboneliklerinizi listeleyin:

      ```azurecli-interactive
      az account list --output table
      ```

   1. Kullanmak istediğiniz abonelik için SubscriptionID alanını kopyalayın.

   1. Çalışma aboneliğinizi kopyaladığınız kimlikle ayarlayın:

      ```azurecli-interactive
      az account set -s <SubscriptionId>
      ```

1. Yeni bir kaynak grubu oluşturun (veya sonraki adımlarda varolan bir tane belirtin):

   ```azurecli-interactive
   az group create --name IoTEdgeResources --location westus2
   ```

1. Yeni bir sanal makine oluşturun:

    **Kimlik doğrulama türü** `password`kullanmak için aşağıdaki örneğe bakın:

   ```azurecli-interactive
   az group deployment create \
   --name edgeVm \
   --resource-group IoTEdgeResources \
   --template-uri "https://aka.ms/iotedge-vm-deploy" \
   --parameters dnsLabelPrefix='my-edge-vm1' \
   --parameters adminUsername='<REPLACE_WITH_USERNAME>' \
   --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id <REPLACE_WITH_DEVICE-NAME> --hub-name <REPLACE-WITH-HUB-NAME> -o tsv) \
   --parameters authenticationType='password' \
   --parameters adminPasswordOrKey="<REPLACE_WITH_SECRET_PASSWORD>"
   ```

    Bir SSH tuşu ile kimlik doğrulaması yapmak için, bunu `sshPublicKey`bir kimlik doğrulama **Türü** belirterek yapabilirsiniz, sonra **adminPasswordOrKey** parametresindeki SSH anahtarının değerini sağlayabilirsiniz.  Aşağıda bir örnek gösterilmiştir.

    ```azurecli-interactive
    #Generate the SSH Key
    ssh-keygen -m PEM -t rsa -b 4096 -q -f ~/.ssh/iotedge-vm-key -N ""  

    #Create a VM using the iotedge-vm-deploy script
    az group deployment create \
    --name edgeVm \
    --resource-group IoTEdgeResources \
    --template-uri "https://aka.ms/iotedge-vm-deploy" \
    --parameters dnsLabelPrefix='my-edge-vm1' \
    --parameters adminUsername='<REPLACE_WITH_USERNAME>' \
    --parameters deviceConnectionString=$(az iot hub device-identity show-connection-string --device-id <REPLACE_WITH_DEVICE-NAME> --hub-name <REPLACE-WITH-HUB-NAME> -o tsv) \
    --parameters authenticationType='sshPublicKey' \
    --parameters adminPasswordOrKey="$(< ~/.ssh/iotedge-vm-key.pub)"
     
    ```

1. Dağıtımın başarıyla tamamlandığını doğrulayın.  Seçilen kaynak grubuna sanal bir makine kaynağı dağıtılmış olmalıdır.  Makine adı dikkat edin, bu biçimde `vm-0000000000000`olmalıdır. Ayrıca, biçiminde `<dnsLabelPrefix>`olması gereken ilişkili **DNS Adı'na**dikkat edin. `<location>`.cloudapp.azure.com.

    **DNS Adı,** **kamu SSH** girişinin bir parçası olarak **çıktılar** bölümünde, önceki adımın JSON biçimli çıktısından elde edilebilir.  Bu girişin değeri, SSH için yeni dağıtılan makineye kullanılabilir.

    ```bash
    "outputs": {
      "public SSH": {
        "type": "String",
        "value": "ssh <adminUsername>@<DNS_Name>"
      }
    }
    ```

    **DNS Adı,** Azure portalı içinde yeni dağıtılan sanal makinenin **Genel Bakış** bölümünden de elde edilebilir.

    > [!div class="mx-imgBorder"]
    > [![iotedge vm dns adını gösteren ekran görüntüsü](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)](./media/how-to-install-iot-edge-ubuntuvm/iotedge-vm-dns-name.png)

1. Kurulumdan sonra bu VM'ye SSH yapmak istiyorsanız, komutla ilişkili **DNS Adını** kullanın:`ssh <adminUsername>@<DNS_Name>`

## <a name="next-steps"></a>Sonraki adımlar

Artık çalışma zamanı yüklü bir IoT Edge aygıtınız olduğuna göre, [IoT Edge modüllerini dağıtabilirsiniz.](how-to-deploy-modules-portal.md)

IoT Edge çalışma zamanının düzgün yüklenmesiyle ilgili sorunlar yaşıyorsanız, [sorun giderme](troubleshoot.md) sayfasına göz atın.

Varolan bir yüklemeyi IoT Edge'in en yeni sürümüne güncelleştirmek için [bkz.](how-to-update-iot-edge.md)

VM'ye SSH veya diğer gelen bağlantılar üzerinden erişmek için bağlantı noktaları açmak istiyorsanız, bir [Linux VM bağlantı noktalarını ve bitiş noktalarını açmayla](../virtual-machines/linux/nsg-quickstart.md) ilgili Azure Sanal Makineler belgelerine bakın
