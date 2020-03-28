---
title: Azure VM'lerde küme için altyapı oluşturma
description: Bu eğitimde, Bir Hizmet Kumaşı kümesini çalıştırmak için Azure VM altyapısını nasıl ayarlayabileceğinizi öğrenirsiniz.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: 93a7e2507ab3a467ef83924479872694cae2dd5b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75614018"
---
# <a name="tutorial-create-azure-vm-infrastructure-to-host-a-service-fabric-cluster"></a>Öğretici: Hizmet Dokusu kümesini barındırmak için Azure VM altyapısı oluşturun

Service Fabric tek başına kümeleri, kendi ortamınızı seçme ve Service Fabric’in benimsediği "her işletim sistemi, her bulut" yaklaşımının bir parçası olarak bir küme oluşturma seçeneği sunar. Bu öğretici seride, Azure VM'lerde barındırılan bağımsız bir küme oluşturur ve üzerine bir uygulama yüklersiniz.

Bu öğretici, bir dizinin birinci bölümüdür. Bu makalede, bağımsız Hizmet Kumaşı kümenizi barındırmak için gereken Azure VM kaynaklarını oluşturursunuz. Gelecek makalelerde, Service Fabric tek başına paketini yüklemeniz, kümenize bir örnek uygulama yüklemeniz ve son olarak kümenizi temizlemeniz gerekir.

Serinin birinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * AzureVM örnekleri kümesi oluşturma
> * Güvenlik grubunu değiştirme
> * Örneklerden birinde oturum açma
> * Service Fabric örneğini hazırlama

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için bir Azure aboneliğinizin olması gerekir.  Zaten bir hesabınız yoksa, bir hesap oluşturmak için [Azure portalına](https://portal.azure.com) gidin.

## <a name="create-azure-virtual-machine-instances"></a>Azure Sanal Makine örnekleri oluşturma

1. Azure portalında oturum açın ve **Sanal makineleri** seçin (Sanal Makineler (klasik değil)).

   ![Azure portalı VM][az-console]

2. **Sanal makine** oluştur formunu açacak olan **Ekle** düğmesini seçin.

3. Temel **Bilgiler** sekmesinde, istediğiniz abonelik ve kaynak grubunu seçtiğinizden emin olun (yeni bir kaynak grubu kullanmanız önerilir).

4. **Resim** türünü **Windows Server 2016 Datacenter**olarak değiştirin. 
 
5. Örnek **Boyutunu** **Standart DS2 v2**olarak değiştirin. Yönetici **Kullanıcı Adı** ve **Parola'yı**ayarlayın ve ne olduklarını belirtin.

6. Gelen **Bağlantı Noktası Kuralları** şimdilik engellenmiş olarak bırakın; bir sonraki bölümde bunları yapılandıracağız.

7. **Ağ** sekmesinde, yeni bir **Sanal Ağ** oluşturun ve adını not alın.

8. Ardından, **NIC ağ güvenlik grubunu** **Gelişmiş**olarak ayarlayın. Adını belirten yeni bir güvenlik grubu oluşturun ve TCP trafiğinin herhangi bir kaynaktan girmesine izin vermek için aşağıdaki kuralları oluşturun:

   ![sf-gelen][sf-inbound]

   * Port `3389`, RDP ve ICMP (temel bağlantı) için.
   * Portlar `19000-19003`, Servis Kumaşı için.
   * Portlar `19080-19081`, Servis Kumaşı için.
   * Bağlantı `8080`noktası , web tarayıcısı istekleri için.

   > [!TIP]
   > Service Fabric’te sanal makinelerinizi birbirine bağlamak için, altyapınızı barındıran VM’lerin aynı kimlik bilgilerine sahip olması gerekir.  Tutarlı kimlik bilgileri elde etmenin iki yaygın yolu vardır: tümünü aynı etki alanına eklemek veya her sanal makinede aynı yönetici parolasını ayarlamak. Neyse ki, Azure aynı Sanal **ağdaki** tüm sanal makinelerin kolayca bağlanmasına izin verir, bu nedenle tüm örneklerimiz aynı ağda olacağından emin olacağız.

9. Başka bir kural ekleyin. Kaynağı **Hizmet Etiketi** olarak ayarlayın ve kaynak hizmet etiketini **VirtualNetwork**olarak ayarlayın. Service Fabric küme içinde iletişim için aşağıdaki bağlantı noktalarının açık olmasını gerektirir: 135,137-139,445,20001-20031,20606-20861.

   ![vnet gelen][vnet-inbound]

10. Seçeneklerin geri kalanı varsayılan durumda kabul edilebilir. İsterseniz gözden geçirin ve sanal makinenizi başlatın.

## <a name="creating-more-instances-for-your-service-fabric-cluster"></a>Service Fabric kümeniz için daha fazla örnek oluşturma

Önceki bölümde özetlenen aynı ayarları korumak için emin olmak, iki **Sanal Makineleri**başlatın. Özellikle, aynı yönetici kullanıcı adı ve parolasını koruyun. **Sanal Ağ** ve NIC ağ **güvenlik grubu** yeniden oluşturulmamalıdır; açılır menüden zaten oluşturduğunuz menüden oluşturduğunuz ları seçin. Her örneğinizin dağıtılması birkaç dakika sürebilir.

## <a name="connect-to-your-instances"></a>Örneklerinize bağlanın

1. **Sanal Makine** bölümünden örneklerinden birini seçin.

2. Genel **Bakış** sekmesinde, *özel* IP adresine dikkat edin. Ardından **Bağlan'ı**tıklatın.

3. **RDP** sekmesinde, daha önce özel olarak açtığımız genel IP adresini ve bağlantı noktası 3389'u kullandığımızı unutmayın. RDP dosyasını indirin.
 
4. RDP dosyasını açın ve istendiğinde VM kurulumunda sağladığınız kullanıcı adı ve parolayı girin.

5. Bir örneğe bağlandıktan sonra, uzak kayıt defterinin çalıştığını doğrulamanız, SMB'yi etkinleştirmeniz ve Kobİ ve uzak kayıt defteri için gerekli bağlantı noktalarını açmanız gerekir.

   SMB'yi etkinleştirmek için PowerShell komutu şudur:

   ```powershell
   netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes
   ```

6. Buradaki güvenlik duvarında bağlantı noktalarını açmak için şu PowerShell komutunu kullanın:

   ```powershell
   New-NetFirewallRule -DisplayName "Service Fabric Ports" -Direction Inbound -Action Allow -RemoteAddress LocalSubnet -Protocol TCP -LocalPort 135, 137-139, 445
   ```

7. Bu işlemi diğer örnekleriniz için yine özel IP adreslerini belirterek yine tekrarlayın.

## <a name="verify-your-settings"></a>Ayarlarınızı doğrulayın

1. Temel bağlantıyı doğrulamak için RDP kullanarak VM'lerden birine bağlanın.

2. **Komut İstem'ini** bu VM'nin içinden açın, ardından ping komutunu kullanarak bir VM'den diğerine bağlanarak aşağıdaki IP adresini daha önce belirttiğiniz özel IP adreslerinden biriyle değiştirin (zaten bağlı olduğunuz VM'nin IP'sini değil).

   ```
   ping 172.31.20.163
   ```

   Çıktınız dört kez tekrar eden `Reply from 172.31.20.163: bytes=32 time<1ms TTL=128` gibi görünüyorsa, örnekler arasındaki bağlantınız çalışıyordur.

3. Şimdi aşağıdaki komutla SMB paylaşımınızın çalıştığını doğrulayın:

   ```
   net use * \\172.31.20.163\c$
   ```

   Çıktı olarak `Drive Z: is now connected to \\172.31.20.163\c$.` döndürülmelidir.


   Şimdi örnekleri düzgün Hizmet Kumaş için hazırlanır.

## <a name="next-steps"></a>Sonraki adımlar

Serinin birinci bölümünde, üç Azure VM örneğini başlatmayı ve Service Fabric yüklemesi için yapılandırmayı öğrendiniz:

> [!div class="checklist"]
> * Azure VM örnekleri kümesi oluşturma
> * Güvenlik grubunu değiştirme
> * Örneklerden birinde oturum açma
> * Service Fabric örneğini hazırlama

Service Fabric’i kümenizde yapılandırmak için serinin ikinci bölümüne ilerleyin.

> [!div class="nextstepaction"]
> [Service Fabric yükleme](service-fabric-tutorial-standalone-create-service-fabric-cluster.md)

<!-- IMAGES -->
[az-console]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/az-console.png
[sf-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/sf-inbound.png
[vnet-inbound]: ./media/service-fabric-tutorial-standalone-azure-create-infrastructure/vnet-inbound.png
