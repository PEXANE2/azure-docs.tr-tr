---
title: Azure'da Bir Windows VM'de MongoDB'yi yükleme
description: Kaynak Yöneticisi dağıtım modeliyle oluşturulan Windows Server 2012 R2 çalıştıran bir Azure VM'sine MongoDB'yi nasıl yükleyeceğimiz öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
ms.assetid: 53faf630-8da5-4955-8d0b-6e829bf30cba
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 37c1b58d364e7eadb33803ce7eac1f2b956ec1b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74038545"
---
# <a name="install-and-configure-mongodb-on-a-windows-vm-in-azure"></a>MongoDB'yi Azure'daki bir Windows VM'de yükleme ve yapılandırma
[MongoDB](https://www.mongodb.org) popüler bir açık kaynak, yüksek performanslı NoSQL veritabanıdır. Bu makale, Azure'daki bir Windows Server 2016 sanal makinesine (VM) MongoDB yükleme ve yapılandırma konusunda size yol gösteriş eder. [MongoDB'yi Azure'da bir Linux VM'ye](../linux/install-mongodb.md)de yükleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
MongoDB'yi yüklemeden ve yapılandırmadan önce bir VM oluşturmanız ve ideal olarak bir veri diski eklemeniz gerekir. VM oluşturmak ve bir veri diski eklemek için aşağıdaki makalelere bakın:

* [Azure portalını](quick-create-portal.md) veya [Azure PowerShell'i](quick-create-powershell.md)kullanarak bir Windows Server VM oluşturun.
* Azure portalını veya [Azure PowerShell'i](attach-disk-ps.md)kullanarak bir Windows Server VM'ye veri [diski](attach-managed-disk-portal.md) takın.

MongoDB'yi yüklemeye ve yapılandırmaya başlamak için Uzak Masaüstü'nü kullanarak [Windows Server VM'nizde oturum açın.](connect-logon.md)

## <a name="install-mongodb"></a>MongoDB'yi yükleme
> [!IMPORTANT]
> Kimlik doğrulama ve IP adresi bağlama gibi MongoDB güvenlik özellikleri varsayılan olarak etkinleştirilir. MongoDB'yi üretim ortamına dağıtmadan önce güvenlik özellikleri etkinleştirilmelidir. Daha fazla bilgi için [MongoDB Güvenlik ve Kimlik Doğrulama'ya](https://www.mongodb.org/display/DOCS/Security+and+Authentication)bakın.


1. Uzak Masaüstü'nü kullanarak VM'nize bağlandıktan sonra görev çubuğundan Internet Explorer'ı açın.
2. Internet Explorer ilk açıldığında **önerilen güvenlik, gizlilik ve uyumluluk ayarlarını kullan'ı** seçin ve **Tamam'ı**tıklatın.
3. Internet Explorer gelişmiş güvenlik yapılandırması varsayılan olarak etkinleştirilir. MongoDB web sitesini izin verilen siteler listesine ekleyin:
   
   * Sağ üst köşedeki **Araçlar** simgesini seçin.
   * **Internet Seçenekleri'nde** **Güvenlik** sekmesini seçin ve ardından **Güvenilen Siteler** simgesini seçin.
   * **Siteler** düğmesini tıklatın. Güvenilen siteler listesine *\*https:// .mongodb.com* ekleyin ve ardından iletişim kutusunu kapatın.
     
     ![Internet Explorer güvenlik ayarlarını yapılandırma](./media/install-mongodb/configure-internet-explorer-security.png)
4. [MongoDB'ye](https://www.mongodb.com/downloads) göz atın -https://www.mongodb.com/downloads)İndirmeler sayfasına ( .
5. Gerekirse, Topluluk **Server** sürümünü seçin ve ardından Windows*Server 2008 R2 64-bit ve sonrası*için en son geçerli kararlı sürümü seçin. Yükleyiciyi indirmek için **DOWNLOAD (msi) seçeneğini**tıklatın.
   
    ![Karşıdan yükleme MongoDB yükleyici](./media/install-mongodb/download-mongodb.png)
   
    İndirme tamamlandıktan sonra yükleyiciyi çalıştırın.
6. Lisans sözleşmesini okuyun ve kabul edin. Sizden istendiğinde, **Yükle'yi** Tamamla'yı seçin.
7. İstenirse, MongoDB için bir grafik arabirimi olan Compass'ı da yüklemeyi seçebilirsiniz.
8. Son ekranda **Yükle'yi**tıklatın.

## <a name="configure-the-vm-and-mongodb"></a>VM ve MongoDB'yi yapılandırın
1. Yol değişkenleri MongoDB yükleyici tarafından güncelleştirilmeyecektir. Yol değişkeninizde MongoDB `bin` konumu olmadan, her Yürütülebilir MongoDB kullandığınızda tam yolu belirtmeniz gerekir. Yol değişkeninize konum eklemek için:
   
   * **Başlat** menüsüne sağ tıklayın ve **Sistem'i**seçin.
   * **Gelişmiş sistem ayarlarını**tıklatın ve ardından **Çevre Değişkenleri'ni**tıklatın.
   * **Sistem değişkenleri**altında, **Yol'u**seçin ve sonra **Düzenle'yi**tıklatın.
     
     ![PATH değişkenlerini yapılandırma](./media/install-mongodb/configure-path-variables.png)
     
     Yolu MongoDB `bin` klasörünüze ekleyin. MongoDB genellikle *C:\Program Files\MongoDB'de*yüklenir. VM'nizdeki yükleme yolunu doğrulayın. Aşağıdaki örnekte varsayılan MongoDB yükleme `PATH` konumu değişkenine ekler:
     
     ```
     ;C:\Program Files\MongoDB\Server\3.6\bin
     ```
     
     > [!NOTE]
     > Değişkeninize bir konum eklediğinizi belirtmek için önde gelen yarı nokta işaret noktasını (`;`) eklediğinizden `PATH` emin olun.

2. Veri diskinizde MongoDB verileri ve günlük dizinleri oluşturun. **Başlat** menüsünden **Komut İstem'i'ni**seçin. Aşağıdaki örnekler, F sürücüsündeki dizinleri oluşturur:
   
    ```
    mkdir F:\MongoData
    mkdir F:\MongoLogs
    ```
3. Verilerinizdeki yolu ve günlük dizinlerini buna göre ayarlayarak aşağıdaki komutla bir MongoDB örneği başlatın:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log
    ```
   
    MongoDB'nin günlük dosyalarını ayırması ve bağlantıları dinlemeye başlaması birkaç dakika sürebilir. Tüm günlük iletileri `mongod.exe` *F:\MongoLogs\mongolog.log.log* dosyasına yönlendirilir, sunucu başlar ve günlük dosyalarını ayırır.
   
   > [!NOTE]
   > MongoDB örneğiniz çalışırken komut istemi bu göreve odaklanmış olarak kalır. MongoDB'yi çalıştırmaya devam etmek için komut istemi penceresini açık bırakın. Veya, bir sonraki adımda ayrıntılı olarak MongoDB'yi hizmet olarak yükleyin.

4. Daha sağlam bir MongoDB deneyimi `mongod.exe` için, bir hizmet olarak yükleyin. Hizmet oluşturmak, MongoDB'yi her kullanmak istediğinizde çalışan bir komut istemi bırakmanız gerekolmadığı anlamına gelir. Verilerinizdeki yolu ve günlük dizinlerini buna göre ayarlayarak hizmeti aşağıdaki gibi oluşturun:
   
    ```
    mongod --dbpath F:\MongoData\ --logpath F:\MongoLogs\mongolog.log --logappend  --install
    ```
   
    Önceki komut , "Mongo DB" açıklamasını içeren MongoDB adında bir hizmet oluşturur. Aşağıdaki parametreler de belirtilir:
   
   * Seçenek, `--dbpath` veri dizininin konumunu belirtir.
   * Çalışan `--logpath` hizmetçıktı görüntülemek için bir komut penceresi olmadığından, bir günlük dosyası belirtmek için seçenek kullanılmalıdır.
   * Seçenek, `--logappend` hizmetin yeniden başlatılmasının çıktının varolan günlük dosyasına eklenmeye neden olduğunu belirtir.
   
   MongoDB hizmetini başlatmak için aşağıdaki komutu çalıştırın:
   
    ```
    net start MongoDB
    ```
   
    MongoDB hizmetini oluşturma hakkında daha fazla bilgi için, [MongoDB için bir Windows Hizmeti Yapılandır'a](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-windows/#mongodb-as-a-windows-service)bakın.

## <a name="test-the-mongodb-instance"></a>MongoDB örneğini test edin
MongoDB'nin tek bir örnek olarak çalıştırılırken veya hizmet olarak yüklendiğinde, artık veritabanlarınızı oluşturmaya ve kullanmaya başlayabilirsiniz. MongoDB yönetim kabuğunu başlatmak için **Başlat** menüsünden başka bir komut istemi penceresi açın ve aşağıdaki komutu girin:

```
mongo  
```

Veritabanlarını `db` komutla listeleyebilirsiniz. Aşağıdaki gibi bazı veriler ekleyin:

```
db.foo.insert( { a : 1 } )
```

Aşağıdaki gibi veri arama:

```
db.foo.find()
```

Çıktı aşağıdaki örneğe benzer:

```
{ "_id" : "ObjectId("57f6a86cee873a6232d74842"), "a" : 1 }
```

Konsoldan `mongo` aşağıdaki gibi çıkın:

```
exit
```

## <a name="configure-firewall-and-network-security-group-rules"></a>Güvenlik duvarı ve Ağ Güvenlik Grubu kurallarını yapılandırma
Artık MongoDB yüklü ve çalışıyor, Uzaktan MongoDB bağlanabilirsiniz böylece Windows Güvenlik Duvarı'nda bir bağlantı noktası açın. TCP bağlantı noktası 27017'ye izin verecek yeni bir gelen kuralı oluşturmak için, yönetimden bir PowerShell istemi açın ve aşağıdaki komutu girin:

```powerahell
New-NetFirewallRule `
    -DisplayName "Allow MongoDB" `
    -Direction Inbound `
    -Protocol TCP `
    -LocalPort 27017 `
    -Action Allow
```

Gelişmiş Güvenlik grafik yönetim **aracıyla Windows Güvenlik Duvarı'nı** kullanarak kuralı da oluşturabilirsiniz. TCP bağlantı noktası 27017'ye izin vermek için yeni bir gelen kuralı oluşturun.

Gerekirse, varolan Azure sanal ağ alt ağının dışından MongoDB'ye erişime izin vermek için bir Ağ Güvenlik Grubu kuralı oluşturun. [Azure portalını](nsg-quickstart-portal.md) veya [Azure PowerShell'i](nsg-quickstart-powershell.md)kullanarak Ağ Güvenlik Grubu kurallarını oluşturabilirsiniz. Windows Güvenlik Duvarı kurallarında olduğu gibi, TCP bağlantı noktası 27017'nin MongoDB VM'nizin sanal ağ arabirimine girmesine izin verin.

> [!NOTE]
> TCP bağlantı noktası 27017, MongoDB tarafından kullanılan varsayılan bağlantı noktasıdır. Bu bağlantı noktasını, el `--port` ile veya `mongod.exe` bir hizmetten başlatırken parametreyi kullanarak değiştirebilirsiniz. Bağlantı noktasını değiştirirseniz, önceki adımlarda Windows Güvenlik Duvarı ve Ağ Güvenlik Grubu kurallarını güncelleştirdiğinden emin olun.


## <a name="next-steps"></a>Sonraki adımlar
Bu eğitimde, Windows VM'nize MongoDB'yi nasıl yükleyip yapılandırabileceğinizi öğrendiniz. Artık [MongoDB belgelerindeki](https://docs.mongodb.com/manual/)gelişmiş konuları takip ederek Windows VM'inizde MongoDB'ye erişebilirsiniz.

