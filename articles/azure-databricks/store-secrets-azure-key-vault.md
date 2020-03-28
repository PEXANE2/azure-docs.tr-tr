---
title: Öğretici - Azure Databricks'i kullanarak anahtar kasası kullanarak blob depolamaalanına erişin
description: Bu öğretici, önemli bir kasada depolanan sırları kullanarak Azure Veri Tuğlaları'ndan Azure Blob Depolamasına nasıl erişilenleri açıklar.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.topic: tutorial
ms.date: 07/19/2019
ms.openlocfilehash: 15399d5a00c13141877dcf44640df2c1f9b9ba5c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75889067"
---
# <a name="tutorial-access-azure-blob-storage-from-azure-databricks-using-azure-key-vault"></a>Öğretici: Azure Anahtar Kasası'nı kullanarak Azure Veri Tuğlalarından Azure Blob Depolamasına Erişin

Bu öğretici, önemli bir kasada depolanan sırları kullanarak Azure Veri Tuğlaları'ndan Azure Blob Depolamasına nasıl erişilenleri açıklar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Depolama hesabı ve blob kapsayıcısı oluşturma
> * Azure Anahtar Kasası oluşturun ve bir sır ekleyin
> * Azure Databricks çalışma alanı oluşturun ve gizli bir kapsam ekleyin
> * Azure Databricks'ten blob kabınıza erişin

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/)oturum açın.

> [!Note]
> Bu öğretici **Azure Ücretsiz Deneme Aboneliği**kullanılarak gerçekleştirilemez.
> Ücretsiz bir hesabınız varsa, profilinize gidin ve aboneliğinizi istediğiniz **kadar öde**olarak değiştirin. Daha fazla bilgi için bkz. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/). Ardından, [harcama sınırını kaldırın](https://docs.microsoft.com/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)ve bölgenizdeki VCPU'lar için kota artışı [isteyin.](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request) Azure Databricks çalışma alanınızı oluşturduğunuzda, çalışma alanına ücretsiz Premium Azure Databricks DBUs'a 14 gün süreyle erişim sağlamak için **Deneme (Premium - 14 Gün Ücretsiz DBUs)** fiyatlandırma katmanını seçebilirsiniz.

## <a name="create-a-storage-account-and-blob-container"></a>Depolama hesabı ve blob kapsayıcısı oluşturma

1. Azure portalında kaynak > **Depolama** **Oluştur'u**seçin. Ardından **Depolama hesabı'nı**seçin.

   ![Azure depolama hesabı kaynağını bulma](./media/store-secrets-azure-key-vault/create-storage-account-resource.png)

2. Aboneliğinizi ve kaynak grubunuzu seçin veya yeni bir kaynak grubu oluşturun. Ardından bir depolama hesabı adı girin ve bir konum seçin. **Gözden Geçir + Oluştur'u**seçin.

   ![Depolama hesabı özelliklerini ayarlama](./media/store-secrets-azure-key-vault/create-storage-account.png)

3. Doğrulama başarısız olursa, sorunları giderin ve yeniden deneyin. Doğrulama başarılı olursa, **Oluştur'u** seçin ve depolama hesabının oluşturulmasını bekleyin.

4. Yeni oluşturulan depolama hesabınıza gidin ve **Genel Bakış** sayfasında **Hizmetler** altında **Blobs'u** seçin. Sonra **+ Kapsayıcı'yı** seçin ve bir kapsayıcı adı girin. **Tamam'ı**seçin.

   ![Yeni kapsayıcı oluşturma](./media/store-secrets-azure-key-vault/create-blob-storage-container.png)

5. Blob depolama kapsayıcınıza yüklemek istediğiniz bir dosyayı bulun. Dosyanız yoksa, bazı bilgileri içeren yeni bir metin dosyası oluşturmak için bir metin düzenleyicisi kullanın. Bu örnekte, **hw.txt** adlı bir dosya "merhaba dünya" metnini içerir. Metin dosyanızı yerel olarak kaydedin ve blob depolama kapsayıcınıza yükleyin.

   ![Dosyayı kapsayıcıya yükleme](./media/store-secrets-azure-key-vault/upload-txt-file.png)

6. Depolama hesabınıza dönün ve **Ayarlar**altında **Access tuşlarını** seçin. **Depolama hesap adını** ve anahtarı **nı** bu öğreticide daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

   ![Depolama hesabı erişim anahtarlarını bulma](./media/store-secrets-azure-key-vault/storage-access-keys.png)

## <a name="create-an-azure-key-vault-and-add-a-secret"></a>Azure Anahtar Kasası oluşturun ve bir sır ekleyin

1. Azure portalında kaynak **oluştur'u** seçin ve arama **kutusuna Anahtar** Kasası'nı girin.

   ![Azure kaynak arama kutusu oluşturma](./media/store-secrets-azure-key-vault/find-key-vault-resource.png)

2. Anahtar Kasası kaynağı otomatik olarak seçilir. **Oluştur'u**seçin.

   ![Anahtar Kasası kaynağı oluşturma](./media/store-secrets-azure-key-vault/create-key-vault-resource.png)

3. Anahtar **kasası oluştur** sayfasında, aşağıdaki bilgileri girin ve kalan alanlar için varsayılan değerleri tutun:

   |Özellik|Açıklama|
   |--------|-----------|
   |Adı|Anahtar kasan için eşsiz bir isim.|
   |Abonelik|Bir abonelik seçin.|
   |Kaynak grubu|Bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturun.|
   |Konum|Konum seçin.|

   ![Azure anahtar kasa özellikleri](./media/store-secrets-azure-key-vault/create-key-vault-properties.png)

3. Yukarıdaki bilgileri girdikten sonra **Oluştur**’u seçin. 

4. Azure portalında yeni oluşturduğunuz anahtar kasasına gidin ve **Sırlar'ı**seçin. Daha sonra **+ Oluştur/Al'ı**seçin. 

   ![Yeni anahtar kasası sırrı nı oluşturun](./media/store-secrets-azure-key-vault/generate-import-secrets.png)

5. Gizli bir sayfada **oluştur,** aşağıdaki bilgileri sağlayın ve kalan alanlar için varsayılan değerleri tutun:

   |Özellik|Değer|
   |--------|-----------|
   |Yükleme seçenekleri|El ile|
   |Adı|Depolama hesabı anahtarınız için dost adı.|
   |Değer|depolama hesabınızdan key1.|

   ![Yeni anahtar kasası sırrı için özellikler](./media/store-secrets-azure-key-vault/create-storage-secret.png)

6. Bu öğreticide daha sonra kullanılmak üzere bir metin düzenleyicisinde anahtar adını kaydedin ve **Oluştur'u**seçin. Ardından Özellikler **menüsüne** gidin. **DNS Adı** ve **Kaynak Kimliğini** daha sonra öğreticide kullanılmak üzere bir metin düzenleyicisine kopyalayın.

   ![Azure Anahtar Vault DNS adını ve Kaynak Kimliğini kopyalama](./media/store-secrets-azure-key-vault/copy-dns-resource.png)

## <a name="create-an-azure-databricks-workspace-and-add-a-secret-scope"></a>Azure Databricks çalışma alanı oluşturun ve gizli bir kapsam ekleyin

1. Azure portalında, **bir kaynak** > Oluştur**Analytics** > **Azure Databricks'i**seçin.

    ![Azure portalında veri tuğlaları](./media/store-secrets-azure-key-vault/azure-databricks-on-portal.png)

2. **Azure Databricks Hizmeti**altında, bir Databricks çalışma alanı oluşturmak için aşağıdaki değerleri sağlar.

   |Özellik  |Açıklama  |
   |---------|---------|
   |Çalışma alanı adı     | Databricks çalışma alanınız için bir ad sağlayın        |
   |Abonelik     | Açılan listeden Azure aboneliğinizi seçin.        |
   |Kaynak grubu     | Anahtar kasanızı içeren aynı kaynak grubunu seçin. |
   |Konum     | Azure Anahtar Kasanızla aynı konumu seçin. Kullanılabilir tüm bölgeler [için, bölgeye göre kullanılabilen Azure hizmetlerine](https://azure.microsoft.com/regions/services/)bakın.        |
   |Fiyatlandırma Katmanı     |  **Standart** veya **Premium** arasında seçim yapın. Bu katmanlar hakkında daha fazla bilgi için bkz. [Databricks fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/databricks/).       |

   ![Databricks çalışma alanı özellikleri](./media/store-secrets-azure-key-vault/create-databricks-service.png)

   **Oluştur'u**seçin.

3. Azure portalında yeni oluşturulan Azure Databricks kaynağınıza gidin ve **Çalışma Alanını Başlat'ı**seçin.

   ![Azure Databricks çalışma alanını başlatın](./media/store-secrets-azure-key-vault/launch-databricks-workspace.png)

4. Azure Databricks çalışma alanınız ayrı bir pencerede açıldığında, **URL'ye #secrets/createScope** ekle.'#secrets. URL aşağıdaki biçime sahip olmalıdır: 

   **https://<\location>.azuredatabricks.net/?o=<\orgID>#secrets/createScope**.
   

5. Bir kapsam adı girin ve daha önce kaydettiğiniz Azure Key Vault DNS adını ve Kaynak Kimliğini girin. Kapsam adını daha sonra bu öğreticide kullanılmak üzere bir metin düzenleyicisinde kaydedin. Ardından **Oluştur'u**seçin.

   ![Azure Databricks çalışma alanında gizli kapsam oluşturma](./media/store-secrets-azure-key-vault/create-secret-scope.png)

## <a name="access-your-blob-container-from-azure-databricks"></a>Azure Databricks'ten blob kabınıza erişin

1. Azure Databricks çalışma alanınızın ana sayfasından, **Ortak Görevler**altında **Yeni Küme'yi** seçin.

   ![Yeni bir Azure Databricks dizüstü bilgisayarı oluşturma](./media/store-secrets-azure-key-vault/create-new-cluster.png)

2. Bir küme adı girin ve **küme oluştur'u**seçin. Küme oluşturma nın tamamlanması birkaç dakika sürer.

3. Küme oluşturulduktan sonra, Azure Databricks çalışma alanınızın ana sayfasına gidin, **Ortak Görevler**altında Yeni **Not Defteri'ni** seçin.

   ![Yeni bir Azure Databricks dizüstü bilgisayarı oluşturma](./media/store-secrets-azure-key-vault/create-new-notebook.png)

4. Bir not defteri adı girin ve dili Python olarak ayarlayın. Kümeyi önceki adımda oluşturduğunuz kümenin adına ayarlayın.

5. Blob depolama kabınızı monte etmek için aşağıdaki komutu çalıştırın. Aşağıdaki özelliklerin değerlerini değiştirmeyi unutmayın:

   * sizin konteyner adınız
   * depolama-hesap-adı
   * montaj adı
   * config-key
   * kapsam adı
   * anahtar adı

   ```python
   dbutils.fs.mount(
   source = "wasbs://<your-container-name>@<your-storage-account-name>.blob.core.windows.net",
   mount_point = "/mnt/<mount-name>",
   extra_configs = {"<conf-key>":dbutils.secrets.get(scope = "<scope-name>", key = "<key-name>")})
   ```

   * **montaj adı** Blob Depolama kapsayıcısı veya kapsayıcı içinde bir klasör (kaynakta belirtilen) monte edilecek temsil eden bir DBFS yoludur.
   * **conf-key** ya `fs.azure.account.key.<\your-storage-account-name>.blob.core.windows.net` da olabilir`fs.azure.sas.<\your-container-name>.<\your-storage-account-name>.blob.core.windows.net`
   * **kapsam adı,** önceki bölümde oluşturduğunuz gizli kapsamın adıdır. 
   * **anahtar adı,** anahtar kasanızdaki depolama hesabı anahtarı için oluşturduğunuz gizli lerin adıdır.

   ![Not defterinde blob depolama yuvası oluşturma](./media/store-secrets-azure-key-vault/command1.png)

6. Blob depolama kapsayıcınızdaki metin dosyasını bir veri çerçevesine okumak için aşağıdaki komutu çalıştırın. Montaj adınız ve dosya adınız ile eşleşecek şekilde komuttaki değerleri değiştirin.

   ```python
   df = spark.read.text("mnt/<mount-name>/<file-name>")
   ```

   ![Dosyayı veri çerçevesine okuma](./media/store-secrets-azure-key-vault/command2.png)

7. Dosyanızın içeriğini görüntülemek için aşağıdaki komutu kullanın.

   ```python
   df.show()
   ```
   ![Veri çerçevelerini göster](./media/store-secrets-azure-key-vault/command3.png)

8. Blob depolamanızı sökmek için aşağıdaki komutu çalıştırın:

   ```python
   dbutils.fs.unmount("/mnt/<mount-name>")
   ```

   ![Unmount depolama hesabı](./media/store-secrets-azure-key-vault/command4.png)

9. Montaj söküldükten sonra, blob depolama hesabınızdan artık okuyamayacağınızı unutmayın.

   ![Depolama hesabı hatası nın sökülme](./media/store-secrets-azure-key-vault/command5.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, aşağıdaki adımlarla tüm kaynak grubunuzu silin:

1. Azure portalındaki sol menüden **Kaynak gruplarını** seçin ve kaynak grubunuza gidin.

2. **Kaynak grubunu sil'i** seçin ve kaynak grup adınızı yazın. Ardından **Sil**’i seçin. 

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB için etkinleştirilen bir Hizmet Bitiş Noktası ile VNet enjekte Edilen Databricks ortamının nasıl uygulanacağını öğrenmek için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Öğretici: Cosmos DB bitiş noktasıyla Azure Veri Tuğlaları uygulayın](service-endpoint-cosmosdb.md)
