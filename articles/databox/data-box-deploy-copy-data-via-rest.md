---
title: "Öğretici: Blob depolama kopyalamak için REST API'leri kullanın"
titleSuffix: Azure Data Box
description: REST API'leri aracılığıyla verileri Azure Veri Kutusu Blob depolama alanınıza nasıl kopyalayatıyarı öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 05/09/2019
ms.author: alkohli
ms.openlocfilehash: 7642c009a5bcd1d00efb432975fff5a65c7ba340
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80297190"
---
# <a name="tutorial-copy-data-to-azure-data-box-blob-storage-via-rest-apis"></a>Öğretici: REST API'leri aracılığıyla verileri Azure Veri Kutusu Blob depolama alanına kopyalama  

Bu öğretici, *http* veya *https*üzerinden REST API'leri aracılığıyla Azure Veri Kutusu Blob depolamabağlanmak için prosedürleri açıklar. Bağlandıktan sonra, verileri Veri Kutusu Blob depolamasına kopyalamak ve Veri Kutusu'nu sevke hazırlamak için gereken adımlar da açıklanmıştır.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * *Http* veya *https* üzerinden Veri Kutusu Blob depolamaya bağlanın
> * Data Box'a veri kopyalama

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğreticiyi tamamladınız: Azure Veri Kutusu'nu ayarlayın.](data-box-deploy-set-up.md)
2. Data Box’ı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. [Veri Kutusu Blob depolama sistemi gereksinimlerini](data-box-system-requirements-rest.md) gözden geçirdiniz ve API'lerin, SDK'ların ve araçların desteklenen sürümlerini tanıdık.
4. Veri Kutusu'na kopyalamak istediğiniz verilere sahip bir ana bilgisayara erişebilirsiniz. Konak bilgisayarınızda:
    - [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    - Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10 GbE bağlantı yoksa, 1 GbE veri bağlantısı kullanılabilir, ancak kopyalama hızları etkilenir.
5. [AzCopy 7.1.0'ı](https://aka.ms/azcopyforazurestack20170417) ana bilgisayarınızdan indirin. AzCopy'yi kullanarak ana bilgisayardan Azure Veri Kutusu Blob depolama alanına veri kopyalarsınız.


## <a name="connect-via-http-or-https"></a>http veya https üzerinden bağlan

Veri Kutusu Blob *depolamaya http* veya *https*üzerinden bağlanabilirsiniz.

- *Https,* Data Box Blob depolama alanına bağlanmanın güvenli ve önerilen yoludur.
- *Http,* güvenilen ağlar üzerinden bağlanırken kullanılır.

Http *veya* *https*üzerinden Veri Kutusu Blob depolama bağlandığınızda bağlanmak için adımlar farklıdır.

## <a name="connect-via-http"></a>Http üzerinden bağlan

*Http* üzerinden Veri Kutusu Blob depolama REST API'lere bağlantı aşağıdaki adımları gerektirir:

- Aygıt IP ve blob hizmeti bitiş noktasını uzak ana bilgisayara ekleme
- Üçüncü taraf yazılımlarını yapılandırma ve bağlantıyı doğrulama

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Aygıt IP adresi ve blob hizmeti bitiş noktası ekleme

[!INCLUDE [data-box-add-device-ip](../../includes/data-box-add-device-ip.md)]

### <a name="configure-partner-software-and-verify-connection"></a>İş ortağı yazılımLarını yapılandırma ve bağlantıyı doğrulama

[!INCLUDE [data-box-configure-partner-software](../../includes/data-box-configure-partner-software.md)]

[!INCLUDE [data-box-verify-connection](../../includes/data-box-verify-connection.md)]

## <a name="connect-via-https"></a>https üzerinden bağlan

Azure Blob depolama REST API'lerine https üzerinden bağlantı etmek için aşağıdaki adımları gerektirir:

- Sertifikayı Azure portalından indirin
- İstemci veya uzak ana bilgisayarda sertifika alma
- Aygıt IP ve blob hizmeti bitiş noktasını istemciye veya uzak ana bilgisayara ekleme
- Üçüncü taraf yazılımlarını yapılandırma ve bağlantıyı doğrulama

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

### <a name="download-certificate"></a>Sertifikayı karşıdan yükleme

Sertifikayı indirmek için Azure portalını kullanın.

1. Azure Portal’da oturum açın.
2. Veri Kutusu siparişinize gidin ve **Genel > Cihazı ayrıntılarına**gidin.
3. **Aygıt kimlik bilgileri**altında, aygıta **API erişimine** gidin. **İndir'i**tıklatın. Bu eylem sipariş ** \<adı>.cer** sertifika dosyasını indirir. Bu dosyayı **kaydedin.** Bu sertifikayı aygıta bağlanmak için kullanacağınız istemci veya ana bilgisayara yüklersiniz.

    ![Azure portalında sertifika indirin](media/data-box-deploy-copy-data-via-rest/download-cert-1.png)
 
### <a name="import-certificate"></a>Sertifikayı içeri aktarma 

DATA Box Blob depolama alanına HTTPS üzerinden erişmek için cihaz için TLS/SSL sertifikası gerekir. Bu sertifikanın istemci uygulamasına sunulma şekli, uygulamadan uygulamaya ve işletim sistemleri ve dağıtımlar arasında değişir. Bazı uygulamalar sertifikaya sistemin sertifika deposuna alındıktan sonra erişebilirken, diğer uygulamalar bu mekanizmadan yararlanamaz.

Bu bölümde bazı uygulamalar için özel bilgiler denbelirtilmiştir. Diğer uygulamalar hakkında daha fazla bilgi için, uygulama ve kullanılan işletim sistemi için belgelere başvurun.

Dosyayı `.cer` bir Windows veya Linux istemcisinin kök deposuna almak için aşağıdaki adımları izleyin. Windows sisteminde, sertifikayı sisteminize almak ve yüklemek için Windows PowerShell veya Windows Server UI'yi kullanabilirsiniz.

#### <a name="use-windows-powershell"></a>Windows PowerShell kullanma

1. Bir Windows PowerShell oturumunu yönetici olarak başlatın.
2. Komut istemine şunları yazın:

    ```
    Import-Certificate -FilePath C:\temp\localuihttps.cer -CertStoreLocation Cert:\LocalMachine\Root
    ```

#### <a name="use-windows-server-ui"></a>Windows Server UI kullanma

1.   Dosyaya `.cer` sağ tıklayın ve **sertifikayı yükle'yi**seçin. Bu eylem Sertifika Alma Sihirbazı'nı başlatır.
2.   **Mağaza konumu**için **Yerel Makine'yi**seçin ve sonra **İleri'yi**tıklatın.

    ![PowerShell kullanarak sertifika alma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-1.png)

3.   **Aşağıdaki mağazadaki tüm sertifikaları yerleştir'i**seçin ve ardından **Gözat'ı**tıklatın. Uzak ana makinenizin kök deposuna gidin ve sonra **İleri'yi**tıklatın.

    ![PowerShell kullanarak sertifika alma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-2.png)

4.   **Son**'a tıklayın. Alma işleminin başarılı olduğunu belirten bir ileti görüntülenir.

    ![PowerShell kullanarak sertifika alma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-3.png)

#### <a name="use-a-linux-system"></a>Linux sistemi kullanma

Sertifika alma yöntemi dağıtıma göre değişir.

Ubuntu ve Debian gibi birkaç, `update-ca-certificates` komutu kullanın.  

- Base64 kodlanmış sertifika dosyasını bir `.crt` uzantıya sahip olacak `/usr/local/share/ca-certificates directory`şekilde yeniden adlandırın ve 'ye kopyalayın.
- `update-ca-certificates` komutunu çalıştırın.

RHEL, Fedora ve CentOS'un `update-ca-trust` son sürümleri nde bu komut kullanılır.

- Sertifika dosyasını dizine kopyalayın. `/etc/pki/ca-trust/source/anchors`
- `update-ca-trust` öğesini çalıştırın.

Ayrıntılar için dağıtımınıza özel belgelere başvurun.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Aygıt IP adresi ve blob hizmeti bitiş noktası ekleme 

[ *Http*üzerinden bağlanırken cihaz IP adresi ve blob hizmet bitiş noktası eklemek için](#add-device-ip-address-and-blob-service-endpoint)aynı adımları izleyin.

### <a name="configure-partner-software-and-verify-connection"></a>İş ortağı yazılımLarını yapılandırma ve bağlantıyı doğrulama

http üzerinden [bağlanırken kullandığınız iş ortağı yazılımını *http*yapılandırmak için](#configure-partner-software-and-verify-connection)adımları izleyin. Tek fark, Kullanım http *seçeneğini* işaretsiz bırakmanızgerektiğidir.

## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

Veri Kutusu Blob depolamasına bağlandıktan sonra, bir sonraki adım verileri kopyalamaktır. Veri kopyalamadan önce aşağıdaki hususları gözden geçirin:

* Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box sınırları](data-box-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun.
* Data Box tarafından yüklenen veriler, Veri Kutusu dışındaki diğer uygulamalar tarafından aynı anda yükleniyorsa, bu durum yükleme iş hatalarına ve veri bozulmasına neden olabilir.
* Veri Kutusu'nun verilerinizi Azure Depolama'ya aktardığını doğrulayana kadar kaynak verilerin bir kopyasını koruduğunuzdan emin olun.

Bu eğitimde, AzCopy Verileri Veri Kutusu Blob depolama kopyalamak için kullanılır. Verileri kopyalamak için Azure Depolama Gezgini 'ni (GUI tabanlı bir aracı tercih ederseniz) veya iş ortağı yazılımını da kullanabilirsiniz.

Kopyalama yordamı aşağıdaki adımları vardır:

- Bir kapsayıcı oluşturma
- Bir klasörün içeriğini Veri Kutusu Blob depolama alanına yükleme
- Değiştirilen dosyaları Veri Kutusu Blob depolama alanına yükleme

Bu adımların her biri aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Lekeler her zaman bir kapsayıcıya yüklendiğinden, ilk adım bir kapsayıcı oluşturmaktır. Kapsayıcılar, bilgisayarınızdaki klasörlerdeki dosyaları düzenlediğiniz gibi blob gruplarını düzenler. Bir blob kapsayıcı oluşturmak için aşağıdaki adımları izleyin.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, blob kapsayıcısını oluşturmak istediğiniz depolama hesabını genişletin.
3. **Blob Containers'ı**sağ tıklatın ve bağlam menüsünden **Blob Kapsayıcı oluştur'u**seçin.

   ![Blob kapsayıcıları bağlam menüsü oluşturma](media/data-box-deploy-copy-data-via-rest/create-blob-container-1.png)

4. **Blob Kapsayıcılar** klasörün altında bir metin kutusu görüntülenir. Blob kapsayıcınızın adını girin. [Kapsayıcıyı Oluştur'a](../storage/blobs/storage-quickstart-blobs-dotnet.md) bakın ve blob kapsayıcılarını adlandırma kuralları ve kısıtlamaları hakkında bilgi için izinler ayarlayın.
5. Blob kapsayıcısını oluşturmak için yapıldığında **Enter** tuşuna basın veya iptal etmek için **Esc** tuşuna basın. Blob kapsayıcısı başarıyla oluşturulduktan sonra, seçili depolama hesabı için **Blob Kapsayıcılar** klasörü altında görüntülenir.

   ![Blob konteyner oluşturuldu](media/data-box-deploy-copy-data-via-rest/create-blob-container-2.png)

### <a name="upload-contents-of-a-folder-to-data-box-blob-storage"></a>Bir klasörün içeriğini Veri Kutusu Blob depolama alanına yükleme

Bir klasördeki tüm dosyaları Windows veya Linux'taki Blob depolama alanına yüklemek için AzCopy'yi kullanın. Bir klasördeki tüm blobları karşıya yüklemek için aşağıdaki AzCopy komutunu girin:

#### <a name="linux"></a>Linux

    azcopy \
        --source /mnt/myfolder \
        --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
        --dest-key <key> \
        --recursive

#### <a name="windows"></a>Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S


Hesap `<key>` anahtarınızla değiştirin. Azure portalında hesap anahtarınızı almak için depolama hesabınıza gidin. Ayarlar **> Erişim tuşlarına**gidin, bir anahtar seçin ve AzCopy komutuna yapıştırın.

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler. Kaynak yolu veri dizininize güncelleştirin ve hedef URL'yi Veri Kutunuzla ilişkili depolama hesabının adıyla değiştirin. `data-box-storage-account-name`

Belirtilen dizinin içeriklerini Blob depolama alanına yinelemeli olarak yüklemek için `--recursive` (Linux) veya `/S` (Windows) seçeneğini belirtin. AzCopy komutunu şu seçeneklerden biriyle çalıştırdığınızda tüm alt klasörler ve bu klasörlerin dosyaları da karşıya yüklenir.

### <a name="upload-modified-files-to-data-box-blob-storage"></a>Değiştirilen dosyaları Veri Kutusu Blob depolama alanına yükleme

Dosyaları son değiştirilen zamana göre yüklemek için AzCopy'i kullanın. Bunu denemek için, test amacıyla kaynak dizininizde yeni dosyalar oluşturun veya değiştirin. Yalnızca güncelleştirilmiş veya yeni dosyaları karşıya yüklemek için, AzCopy komutuna `--exclude-older` (Linux) veya `/XO` (Windows) parametresini ekleyin.

Yalnızca hedefte bulunmayan çıkış kaynaklarını kopyalamak istiyorsanız, AzCopy komutunda `--exclude-older` ve `--exclude-newer` (Linux) veya `/XO` ve `/XN` (Windows) parametrelerini belirtin. AzCopy, yalnızca güncelleştirilmiş verileri zaman damgasına göre karşıya yükler.

#### <a name="linux"></a>Linux
    azcopy \
    --source /mnt/myfolder \
    --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
    --dest-key <key> \
    --recursive \
    --exclude-older

#### <a name="windows"></a>Windows

    AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S /XO

Bağlantı veya kopyalama işlemi sırasında herhangi bir hata varsa, [Veri Kutusu Blob depolama alanıyla ilgili Sorun Giderme sorunlarına](data-box-troubleshoot-rest.md)bakın.

Bir sonraki adım, cihazınızı gönderiye hazırlamaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Ön koşullar
> * *Http* veya *https* üzerinden Veri Kutusu Blob depolamaya bağlanın
> * Data Box'a veri kopyalama


Data Box'ı Microsoft’a geri gönderme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)
