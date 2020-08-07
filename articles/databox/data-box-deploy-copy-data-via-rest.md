---
title: "Öğretici: REST API 'Leri aracılığıyla blob depolamaya kopyalama"
titleSuffix: Azure Data Box
description: Bu öğreticide, http veya https üzerinden REST API 'Lerini kullanarak Azure Data Box blob depolamaya bağlanmayı öğrenin ve sonra Azure Data Box verileri kopyalayın.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 07/02/2020
ms.author: alkohli
ms.openlocfilehash: 71f966cd62ffd2c735259dcfa98b9b97f87d9a19
ms.sourcegitcommit: 4f1c7df04a03856a756856a75e033d90757bb635
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87926204"
---
# <a name="tutorial-use-rest-apis-to-copy-data-to-azure-data-box-blob-storage"></a>Öğretici: Azure Data Box blob depolamaya veri kopyalamak için REST API 'Lerini kullanma  

Bu öğretici, *http* veya *https*üzerinden REST API 'leri aracılığıyla Azure Data Box blob depolamaya bağlanma yordamlarını açıklar. Bağlandıktan sonra, verileri Data Box blob depolamaya kopyalamak ve Data Box hazırlamak için gereken adımlar da açıklanır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * *Http* veya *https* aracılığıyla Data Box blob depolamaya bağlanma
> * Data Box'a veri kopyalama

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce aşağıdakilerden emin olun:

1. [Öğretici: Azure Data Box’ı ayarlama](data-box-deploy-set-up.md).
2. Data Box’ı teslim aldınız ve portaldaki sipariş durumu **Teslim Edildi** oldu.
3. [Data Box BLOB depolama için sistem gereksinimlerini](data-box-system-requirements-rest.md) incelediyseniz ve desteklenen API 'Ler, SDK 'lar ve araçların sürümleriyle ilgili bilgi sahibisiniz.
4. Data Box üzerine kopyalamak istediğiniz verilerin bulunduğu bir ana bilgisayara erişirsiniz. Ana bilgisayarınız:
    * [Desteklenen bir işletim sistemi](data-box-system-requirements.md) çalıştırılmalıdır.
    * Yüksek hızlı bir ağa bağlı olmalıdır. En az bir adet 10 GbE bağlantınızın olması önemle tavsiye edilir. 10-GbE bağlantı kullanılamıyorsa, 1-GbE veri bağlantısı kullanılabilir ancak kopyalama hızları etkilenecektir.
5. Ana bilgisayarınızda [AzCopy 7.1.0 indirin](https://aka.ms/azcopyforazurestack20170417) . Ana bilgisayarınızdan Azure Data Box BLOB depolama alanına veri kopyalamak için AzCopy kullanacaksınız.

## <a name="connect-via-http-or-https"></a>Http veya HTTPS aracılığıyla bağlanma

*Http* veya *https*üzerinden Data Box blob depolamaya bağlanabilirsiniz.

* *Https* , Data Box blob depolamaya bağlanmak için güvenli ve önerilen bir yoldur.
* Güvenilen ağlar üzerinden bağlanılırken *http* kullanılır.

*Http* veya *https*üzerinden Data Box blob depolamaya bağlandığınızda, bağlanma adımları farklıdır.

## <a name="connect-via-http"></a>Http üzerinden Bağlan

*Http* üzerinden Data Box BLOB depolama REST API 'lerine bağlantı aşağıdaki adımları gerektirir:

* Uzak konağa cihaz IP ve BLOB hizmeti uç noktası ekleme
* Üçüncü taraf yazılımları yapılandırın ve bağlantıyı doğrulayın

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Cihaz IP adresi ve BLOB hizmeti uç noktası ekle

[!INCLUDE [data-box-add-device-ip](../../includes/data-box-add-device-ip.md)]

### <a name="verify-connection-and-configure-partner-software"></a>Bağlantıyı doğrulama ve iş ortağı yazılımını yapılandırma

[!INCLUDE [data-box-configure-partner-software](../../includes/data-box-configure-partner-software.md)]

[!INCLUDE [data-box-verify-connection](../../includes/data-box-verify-connection.md)]

## <a name="connect-via-https"></a>Https üzerinden Bağlan

Https üzerinden Azure Blob depolama REST API 'Lerine yönelik bağlantı aşağıdaki adımları gerektirir:

* Sertifikayı Azure portal indirin
* Sertifikayı istemci veya uzak ana bilgisayara aktarma
* İstemci veya uzak konağa cihaz IP ve BLOB hizmeti uç noktası ekleme
* Üçüncü taraf yazılımları yapılandırın ve bağlantıyı doğrulayın

Bu adımların her biri aşağıdaki bölümlerde açıklanmıştır.

### <a name="download-certificate"></a>Sertifikayı indir

Sertifikayı indirmek için Azure portal kullanın.

1. Azure Portal’da oturum açın.
2. Data Box siparişiniz sayfasına gidin ve **genel > cihaz ayrıntılarına**gidin.
3. **Cihaz kimlik bilgileri**altında cihaza **API erişimi** ' ne gidin. **İndir**'e tıklayın. Bu eylem bir ** \<your order name> . cer** sertifika dosyasını indirir. Bu dosyayı **kaydedin** . Bu sertifikayı, cihaza bağlanmak için kullanacağınız istemci veya ana bilgisayara yüklersiniz.

    ![Azure portal sertifikayı indirin](media/data-box-deploy-copy-data-via-rest/download-cert-1.png)

### <a name="import-certificate"></a>Sertifikayı içeri aktarma

HTTPS üzerinden Data Box blob depolamaya erişmek için cihaz için TLS/SSL sertifikası gerekir. Bu sertifikanın istemci uygulama tarafından kullanılabilir hale getirilme yöntemi, uygulamadan uygulamaya ve işletim sistemleri ve dağıtımlar arasında farklılık gösterir. Bazı uygulamalar, sistem sertifika deposuna aktarıldıktan sonra sertifikaya erişebilir, ancak diğer uygulamalar bu mekanizmayı kullanmaz.

Bazı uygulamalar için belirli bilgiler bu bölümde belirtilmiştir. Diğer uygulamalar hakkında daha fazla bilgi için, uygulamanın ve kullanılan işletim sisteminin belgelerine başvurun.

`.cer`Dosyayı bir Windows veya Linux istemcisinin kök deposuna aktarmak için bu adımları izleyin. Windows sisteminde, Windows PowerShell veya Windows Server Kullanıcı arabirimini kullanarak sertifikayı içeri aktarabilir ve sisteminize yükleyebilirsiniz.

#### <a name="use-windows-powershell"></a>Windows PowerShell kullanma

1. Yönetici olarak bir Windows PowerShell oturumu başlatın.
2. Komut istemine şunları yazın:

    ```
    Import-Certificate -FilePath C:\temp\localuihttps.cer -CertStoreLocation Cert:\LocalMachine\Root
    ```

#### <a name="use-windows-server-ui"></a>Windows Server Kullanıcı arabirimini kullanma

1. Dosyaya sağ tıklayın `.cer` ve **sertifikayı yükler**' i seçin. Bu eylem, sertifika alma Sihirbazı 'nı başlatır.
2. **Depo konumu**Için **yerel makine**' yi seçin ve ardından **İleri**' ye tıklayın.

    ![PowerShell kullanarak sertifikayı içeri aktarma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-1.png)

3. **Tüm sertifikaları aşağıdaki depolama alanına yerleştir**' i seçin ve ardından **görüntüle**' ye tıklayın. Uzak konağın kök deposuna gidin ve ardından **İleri**' ye tıklayın.

    ![PowerShell kullanarak sertifikayı içeri aktarma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-2.png)

4. **Son**'a tıklayın. İçeri aktarmanın başarılı olduğunu belirten bir ileti görüntülenir.

    ![PowerShell kullanarak sertifikayı içeri aktarma](media/data-box-deploy-copy-data-via-rest/import-cert-ws-3.png)

#### <a name="use-a-linux-system"></a>Linux sistemi kullanma

Sertifikayı içeri aktarma yöntemi dağıtıma göre değişir.

Ubuntu ve deni gibi birkaç tane `update-ca-certificates` komutunu kullanın.  

* Base64 ile kodlanmış sertifika dosyasını bir uzantıya sahip olacak şekilde yeniden adlandırın `.crt` ve içine kopyalayın `/usr/local/share/ca-certificates directory` .
* `update-ca-certificates` komutunu çalıştırın.

RHEL, Fedora ve CentOS 'ın son sürümleri `update-ca-trust` komutunu kullanır.

* Sertifika dosyasını `/etc/pki/ca-trust/source/anchors` dizine kopyalayın.
* `update-ca-trust` komutunu çalıştırın.

Ayrıntılar için dağıtıma özgü belgelere başvurun.

### <a name="add-device-ip-address-and-blob-service-endpoint"></a>Cihaz IP adresi ve BLOB hizmeti uç noktası ekle 

[ *Http*ÜZERINDEN bağlanılırken cihaz IP adresi ve BLOB hizmeti uç noktası eklemek](#add-device-ip-address-and-blob-service-endpoint)için aynı adımları izleyin.

### <a name="configure-partner-software-and-verify-connection"></a>İş ortağı yazılımını yapılandırma ve bağlantıyı doğrulama

[ *Http*üzerinden bağlanırken kullandığınız Iş ortağı yazılımını yapılandırmak](#verify-connection-and-configure-partner-software)için adımları izleyin. Tek fark, *http kullan seçeneğini* işaretlenmemiş olarak bırakmanız gerektiğidir.

## <a name="copy-data-to-data-box"></a>Data Box'a veri kopyalama

Data Box blob depolamaya bağlandıktan sonra, bir sonraki adım verileri kopyalamaktır. Veri kopyalamadan önce aşağıdaki noktaları gözden geçirin:

* Veri kopyalama sırasında veri boyutunun [Azure depolama ve Data Box sınırları](data-box-limits.md) içinde belirtilen boyut sınırlarına uygun olduğundan emin olun.
* Data Box tarafından karşıya yüklenen veriler, Data Box dışındaki diğer uygulamalar tarafından aynı anda karşıya yüklenirse, bu durum karşıya yükleme işi hatalarıyla ve verilerin bozulmasına yol açabilir.

> [!IMPORTANT]
> Data Box'ın verilerinizi Azure Depolama'ya aktardığını onaylayana kadar kaynak verilerinizin bir kopyasına sahip olduğunuzdan emin olun.

Bu öğreticide, AzCopy, verileri Data Box blob depolamaya kopyalamak için kullanılır. Ayrıca, verileri kopyalamak için Azure Depolama Gezgini (GUI tabanlı bir aracı tercih ediyorsanız) veya iş ortağı yazılımını da kullanabilirsiniz.

Kopyalama yordamı aşağıdaki adımlara sahiptir:

* Bir kapsayıcı oluşturma
* Bir klasörün içeriğini Data Box BLOB depolama alanına yükleme
* Değiştirilen dosyaları Data Box blob depolamaya yükle

Bu adımların her biri, aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma

İlk adım bir kapsayıcı oluşturmaktır, çünkü Bloblar her zaman bir kapsayıcıya yüklenir. Kapsayıcılar, bilgisayarınızdaki klasörlerde dosyaları düzenlediğiniz gibi BLOB gruplarını düzenler. Blob kapsayıcısı oluşturmak için aşağıdaki adımları izleyin.

1. Depolama Gezgini'ni açın.
2. Sol bölmede, blob kapsayıcısını oluşturmak istediğiniz depolama hesabını genişletin.
3. **BLOB kapsayıcıları**' na sağ tıklayın ve bağlam menüsünden **BLOB kapsayıcısı oluştur**' u seçin.

   ![Blob kapsayıcıları Oluştur bağlam menüsü](media/data-box-deploy-copy-data-via-rest/create-blob-container-1.png)

4. **BLOB kapsayıcıları** klasörünün altında bir metin kutusu görünür. Blob kapsayıcınızın adını girin. Blob kapsayıcıları adlandırma kuralları ve kısıtlamaları hakkında bilgi için [kapsayıcı oluşturma ve izinleri ayarlama](../storage/blobs/storage-quickstart-blobs-dotnet.md) bölümüne bakın.
5. Blob kapsayıcısını oluşturmak için veya iptal etmek için **ESC** **tuşuna basın** . Blob kapsayıcısı başarıyla oluşturulduktan sonra, seçili depolama hesabı için **BLOB kapsayıcıları** klasörü altında görüntülenir.

   ![Blob kapsayıcısı oluşturuldu](media/data-box-deploy-copy-data-via-rest/create-blob-container-2.png)

### <a name="upload-contents-of-a-folder-to-data-box-blob-storage"></a>Bir klasörün içeriğini Data Box BLOB depolama alanına yükleme

Bir klasördeki tüm dosyaları Windows veya Linux 'ta blob depolamaya yüklemek için AzCopy kullanın. Bir klasördeki tüm blobları karşıya yüklemek için aşağıdaki AzCopy komutunu girin:

#### <a name="linux"></a>Linux

```azcopy
azcopy \
    --source /mnt/myfolder \
    --destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
    --dest-key <key> \
    --recursive
```

#### <a name="windows"></a>Windows

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S
```

`<key>`Hesap anahtarınızla değiştirin. Hesap anahtarınızı almak için Azure portal depolama hesabınıza gidin. **Ayarlar > erişim anahtarlarına**gidin, bir anahtar seçin ve AzCopy komutuna yapıştırın.

Belirtilen hedef kapsayıcı mevcut değilse, AzCopy bu kapsayıcıyı oluşturur ve dosyayı kapsayıcıya yükler. Kaynak yolunu veri dizininiz için güncelleştirin ve `data-box-storage-account-name` hedef URL 'de, Data Box ilişkili depolama hesabının adıyla değiştirin.

Belirtilen dizinin içeriklerini Blob depolama alanına yinelemeli olarak yüklemek için `--recursive` (Linux) veya `/S` (Windows) seçeneğini belirtin. AzCopy komutunu şu seçeneklerden biriyle çalıştırdığınızda tüm alt klasörler ve bu klasörlerin dosyaları da karşıya yüklenir.

### <a name="upload-modified-files-to-data-box-blob-storage"></a>Değiştirilen dosyaları Data Box blob depolamaya yükle

En son değiştirilme zamanına göre dosyaları karşıya yüklemek için AzCopy kullanın. Bunu denemek için, test amacıyla kaynak dizininizde yeni dosyalar oluşturun veya değiştirin. Yalnızca güncelleştirilmiş veya yeni dosyaları karşıya yüklemek için, AzCopy komutuna `--exclude-older` (Linux) veya `/XO` (Windows) parametresini ekleyin.

Yalnızca hedefte bulunmayan çıkış kaynaklarını kopyalamak istiyorsanız, AzCopy komutunda `--exclude-older` ve `--exclude-newer` (Linux) veya `/XO` ve `/XN` (Windows) parametrelerini belirtin. AzCopy, yalnızca güncelleştirilmiş verileri zaman damgasına göre karşıya yükler.

#### <a name="linux"></a>Linux

```azcopy
azcopy \
--source /mnt/myfolder \
--destination https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ \
--dest-key <key> \
--recursive \
--exclude-older
```

#### <a name="windows"></a>Windows

```azcopy
AzCopy /Source:C:\myfolder /Dest:https://data-box-storage-account-name.blob.device-serial-no.microsoftdatabox.com/container-name/files/ /DestKey:<key> /S /XO
```

Bağlanma veya kopyalama işlemi sırasında herhangi bir hata oluşursa bkz. [Data Box blob depolamayla ilgili sorunları giderme](data-box-troubleshoot-rest.md).

Sonraki adım, cihazınızı sevk etmek için hazırlamaktır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * *Http* veya *https* aracılığıyla Data Box blob depolamaya bağlanma
> * Data Box'a veri kopyalama

Data Box'ı Microsoft’a geri gönderme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Data Box verilerinizi Microsoft'a gönderme](./data-box-deploy-picked-up.md)
