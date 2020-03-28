---
title: Azure'da makine öğrenimi için Python ve TensorFlow'u kullanın
description: Bir görüntüyü içeriğine göre sınıflandırmak için bir makine öğrenme modeliyle Python, TensorFlow ve Azure İşlevlerini kullanın.
author: anthonychu
ms.topic: tutorial
ms.date: 01/15/2020
ms.author: antchu
ms.custom: mvc
ms.openlocfilehash: c64d87b2430cc1d733a67bbc1e803590a37b1714
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78190781"
---
# <a name="tutorial-apply-machine-learning-models-in-azure-functions-with-python-and-tensorflow"></a>Öğretici: Python ve TensorFlow ile Azure İşlerinde makine öğrenimi modellerini uygulayın

Bu makalede, bir görüntüyü içeriğine göre sınıflandırmak için bir makine öğrenme modeliyle Python, TensorFlow ve Azure Işlevleri'ni nasıl kullanacağınızı öğrenirsiniz. Tüm çalışmaları yerel olarak yaptığınız ve bulutta Azure kaynağı oluşturmadığınız için, bu öğreticiyi tamamlamak için hiçbir ücret alınmaz.

> [!div class="checklist"]
> * Python'da Azure İşlevleri geliştirmek için yerel bir ortamı niçin başlatma.
> * Özel bir TensorFlow makine öğrenme modelini bir işlev uygulamasına aktarın.
> * Bir görüntüyü köpek veya kedi içeren olarak sınıflandırmak için sunucusuz bir HTTP API oluşturun.
> * Bir web uygulamasından API'yi tüketin.

## <a name="prerequisites"></a>Ön koşullar 

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- [Python 3.7.4](https://www.python.org/downloads/release/python-374/). (Python 3.7.4 ve Python 3.6.x Azure Fonksiyonları ile doğrulanır; Python 3.8 ve sonraki sürümler henüz desteklenmez.)
- [Azure İşletme Temel Araçları](functions-run-local.md#install-the-azure-functions-core-tools)
- Visual Studio Code gibi bir kod [düzenleyicisi](https://code.visualstudio.com/)

### <a name="prerequisite-check"></a>Ön koşul kontrolü

1. Bir terminal veya komut `func --version` penceresinde, Azure İşlevler Temel Araçları'nın sürüm 2.7.1846 veya sonraki sürüm olup olmadığını denetlemek için çalıştırın.
1. Python `python --version` sürüm raporlarınızı 3.7.x olarak denetlemek için (Linux/MacOS) veya `py --version` (Windows) çalıştırın.

## <a name="clone-the-tutorial-repository"></a>Öğretici depoyu klonlama

1. Bir terminal veya komut penceresinde, Git'i kullanarak aşağıdaki depoyu kopyala:

    ```
    git clone https://github.com/Azure-Samples/functions-python-tensorflow-tutorial.git
    ```

1. Klasöre gidin ve içeriğini inceleyin.

    ```
    cd functions-python-tensorflow-tutorial
    ```

    - *başlangıç* öğretici için çalışma klasörüdür.
    - *sonu,* başvurunuz için nihai sonuç ve tam uygulamadır.
    - *kaynaklar* makine öğrenme modelini ve yardımcı kitaplıklarını içerir.
    - *frontend* işlev uygulaması çağıran bir web sitesidir.
    
## <a name="create-and-activate-a-python-virtual-environment"></a>Python sanal ortamı oluşturma ve etkinleştirme

*Başlat* klasörüne gidin ve aşağıdaki komutları çalıştırArak adlı `.venv`sanal bir ortam oluşturun ve etkinleştirin. Azure Fonksiyonları tarafından desteklenen Python 3.7'yi kullandığınızdan emin olun.


# <a name="bash"></a>[bash](#tab/bash)

```bash
cd start
```

```bash
python -m venv .venv
```

```bash
source .venv/bin/activate
```

Python venv paketini Linux dağıtımınıza yüklemediyse aşağıdaki komutu çalıştırın:

```bash
sudo apt-get install python3-venv
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```powershell
cd start
```

```powershell
py -m venv .venv
```

```powershell
.venv\scripts\activate
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
cd start
```

```cmd
py -m venv .venv
```

```cmd
.venv\scripts\activate
```

---

Bu etkinleştirilen sanal ortamda sonraki tüm komutları çalıştırın. (Sanal ortamdan çıkmak `deactivate`için çalıştırın.)


## <a name="create-a-local-functions-project"></a>Yerel işlevler projesi oluşturma

Azure İşlevlerinde işlev projesi, her biri belirli bir tetikleyiciye yanıt veren bir veya daha fazla ayrı işlev için bir kapsayıcıdır. Projedeki tüm işlevler aynı yerel ve barındırma yapılandırmalarını paylaşır. Bu bölümde, http bitiş noktası sağlayan adlandırılmış `classify` tek bir kazan işlevi içeren bir işlev projesi oluşturursunuz. Daha sonraki bir bölüme daha spesifik kod eklersiniz.

1. *Başlangıç* klasöründe, Python işlev uygulamasını başlatmak için Azure İşlevler Temel Araçları'nı kullanın:

    ```
    func init --worker-runtime python
    ```

    Başlatma dan sonra, *başlat* klasörü [local.settings.json](functions-run-local.md#local-settings-file) ve [host.json](functions-host-json.md)adlı yapılandırmadosyaları da dahil olmak üzere proje için çeşitli dosyalar içerir. *local.settings.json* Azure'dan indirilen sırları içerebildiği *için,..gitignore* dosyasında varsayılan olarak dosya kaynak denetiminden çıkarılır.

    > [!TIP]
    > Bir işlev projesi belirli bir çalışma süresine bağlı olduğundan, projedeki tüm işlevlerin aynı dille yazılması gerekir.

1. `--name` Bağımsız değişkenin işlevinizin benzersiz adı olduğu ve `--template` bağımsız değişkenin işlevin tetikleyicisini belirttiği aşağıdaki komutu kullanarak projenize bir işlev ekleyin. `func new`projenin seçilen diline uygun bir kod dosyası ve *function.json*adlı yapılandırma dosyası içeren işlev adı ile eşleşen bir alt klasör oluşturun.

    ```
    func new --name classify --template "HTTP trigger"
    ```

    Bu komut, işlevin adı eşleşen bir klasör oluşturur, *sınıflandırmak.* Bu klasörde iki * \_ \_\_\_* dosya vardır: işlev kodunu içeren init .py ve *işlev.json*, işlevin tetikleyici ve giriş ve çıkış bağlamaları açıklar. Bu dosyaların içeriği hakkında ayrıntılı bilgi için python quickstart'taki [dosya içeriğini incele](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-python#optional-examine-the-file-contents) bölümüne bakın.


## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

1. Başlat klasöründe yerel Azure İşlevleri çalışma zamanı ana bilgisayarını başlatarak işlevi *başlatın:*

    ```
    func start
    ```
    
1. Çıktıda bitiş `classify` noktasının göründüğünü gördüğünüzde URL'ye ```http://localhost:7071/api/classify?name=Azure```gidin. "Merhaba Azure!" mesajı çıktıda görünmelidir.

1. Ana bilgisayarı durdurmak için **Ctrl**-**C'yi** kullanın.


## <a name="import-the-tensorflow-model-and-add-helper-code"></a>TensorFlow modelini içe aktarın ve yardımcı kodu ekleyin

İşlevin `classify` içeriğini temel almak için işlevi değiştirmek için, Azure Özel Görme Hizmeti ile eğitilmiş ve dışa aktarılan önceden oluşturulmuş bir TensorFlow modeli kullanırsınız. Daha önce klonladığınız örneğin *kaynaklar* klasöründe bulunan model, bir görüntüyü köpek veya kedi içermediğine göre sınıflandırın. Daha sonra projenize bazı yardımcı kodlar ve bağımlılıklar eklersiniz.

> [!TIP]
> Özel Vizyon Hizmeti'nin ücretsiz katmanını kullanarak kendi modelinizi oluşturmak istiyorsanız, [örnek proje deposundaki](https://github.com/Azure-Samples/functions-python-tensorflow-tutorial/blob/master/train-custom-vision-model.md)yönergeleri izleyin.

1. *Başlangıç* klasöründe, model dosyalarını sınıflandırma klasörüne kopyalamak için aşağıdaki komutu *çalıştırın.* Komuta eklediğinden `\*` emin olun. 

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    cp ../resources/model/* classify
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```powershell
    copy ..\resources\model\* classify
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    copy ..\resources\model\* classify
    ```
    
    ---
    
1. *Sınıflandırma* klasöründe *model.pb* ve *labels.txt*adlı dosyalar içerdiğini doğrulayın. Değilse, *komutu başlangıç* klasöründe çalıştırdığınızdan denetleyin.

1. *Başlangıç* klasöründe, yardımcı kodlu bir dosyayı sınıflandırma klasörüne kopyalamak için aşağıdaki komutu *çalıştırın:*

    # <a name="bash"></a>[bash](#tab/bash)
    
    ```bash
    cp ../resources/predict.py classify
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)
    
    ```powershell
    copy ..\resources\predict.py classify
    ```
    
    # <a name="cmd"></a>[Cmd](#tab/cmd)
    
    ```cmd
    copy ..\resources\predict.py classify
    ```
    
    ---

1. *Sınıflandırma* klasöründe artık *predict.py*adlı bir dosya olduğunu doğrulayın.

1. Bir metin düzenleyicisinde *start/requirements.txt'yi* açın ve yardımcı kodun gerektirdiği aşağıdaki bağımlılıkları ekleyin:

    ```txt
    tensorflow==1.14
    Pillow
    requests
    ```
    
1. *gereksinimleri kaydet.txt*.

1. Aşağıdaki komutu *başlangıç* klasöründe çalıştırarak bağımlılıkları yükleyin. Yükleme birkaç dakika sürebilir ve bu süre zarfında bir sonraki bölümdeki işlevi değiştirmeye devam edebilirsiniz.

    ```
    pip install --no-cache-dir -r requirements.txt
    ```
    
    Windows'da, "EnvironmentError nedeniyle paketleri yükleyemediniz: [Errno 2] Böyle bir dosya veya dizin yok:" hatasıyla karşılaşabilirsiniz: *"sharded_mutable_dense_hashtable.cpython-37.pyc*gibi bir dosyaya uzun bir yol adı ve ardından . Genellikle, klasör yolunun derinliği çok uzun olduğundan bu hata olur. Bu durumda, uzun yolları `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem@LongPathsEnabled` etkinleştirmek için `1` kayıt defteri anahtarını ayarlayın. Alternatif olarak, Python yorumcünüzün nerede yüklü olduğunu kontrol edin. Bu konumun uzun bir yolu varsa, daha kısa bir yolu olan bir klasörde yeniden yüklemeyi deneyin.

> [!TIP]
> *predict.py* ilk tahminini yapması için çağırdığınızda, tensorFlow modelini `_initialize` diskten yükler ve onu genel değişkenlerde önbelleğe getirir. Bu önbelleğe alma sonraki tahminleri hızlandırıyor. Genel değişkenleri kullanma hakkında daha fazla bilgi için [Azure İşlevler Python geliştirici kılavuzuna](functions-reference-python.md#global-variables)bakın.

## <a name="update-the-function-to-run-predictions"></a>Tahminleri çalıştırmak için işlevi güncelleştirme

1. Bir metin düzenleyicisinde *.py'yi sınıflat/\_\_\_\_init* ve standart JSON kitaplığını ve *tahmin* yardımcılarını almak için varolan `import` ifadelerden sonra aşağıdaki satırları ekleyin:

    :::code language="python" source="~/functions-python-tensorflow-tutorial/end/classify/__init__.py" range="1-6" highlight="5-6":::

1. İşlevin `main` tüm içeriğini aşağıdaki kodla değiştirin:

    :::code language="python" source="~/functions-python-tensorflow-tutorial/end/classify/__init__.py" range="8-19":::

    Bu işlev, bir sorgu dize parametresi adlı `img`bir görüntü URL alır. Daha sonra, TensorFlow modelini kullanarak görüntüyü indirmek ve sınıflandırmak için yardımcı kitaplığından çağırır. `predict_image_from_url` İşlev daha sonra sonuçları ile bir HTTP yanıtı döndürür. 

    > [!IMPORTANT]
    > Bu HTTP bitiş noktası başka bir etki alanında barındırılan `Access-Control-Allow-Origin` bir web sayfası tarafından çağrıldığı için, yanıt tarayıcının Çapraz Kaynak Paylaşımı (CORS) gereksinimlerini karşılamak için bir üstbilgi içerir.
    >
    > Bir üretim uygulamasında, `*` ek güvenlik için web sayfasının özel kaynağına değiştirin.

1. Değişikliklerinizi kaydedin, ardından bağımlılıkların yüklemeyi tamamladığını varsayarak, yerel işlev ana bilgisayarını `func start`yeniden . Sanal ortam etkinleştirilmiş *başlat* klasöründe ana bilgisayarı çalıştırdığından emin olun. Aksi takdirde ana bilgisayar başlar, ancak işlevi çalıştırırken hatalar görürsünüz.

    ```
    func start
    ```

1. Tarayıcıda, bir kedi görüntüsünün URL'si ile işlevi çağırmak için aşağıdaki URL'yi açın ve döndürülen JSON'un görüntüyü kedi olarak sınıflandırdığını onaylayın.

    ```
    http://localhost:7071/api/classify?img=https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png
    ```
    
1. Bir sonraki adımda kullandığınız için ana bilgisayarı çalışır durumda tutun. 

### <a name="run-the-local-web-app-front-end-to-test-the-function"></a>İşlevi test etmek için yerel web uygulaması ön ucunu çalıştırma

Başka bir web uygulamasından işlev bitiş noktasını çağırmak için deponun *ön uç* klasöründe basit bir uygulama bulunur.

1. Yeni bir terminal veya komut istemi açın ve sanal ortamı etkinleştirin (daha önce [Python sanal ortamı oluştur ve etkinleştir)](#create-and-activate-a-python-virtual-environment)altında açıklandığı gibi).

1. Deponun *ön uç* klasörüne gidin.

1. Python ile bir HTTP sunucusu başlatın:

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash 
    python -m http.server
    ```
    
    # <a name="powershell"></a>[Powershell](#tab/powershell)

    ```powershell
    py -m http.server
    ```

    # <a name="cmd"></a>[Cmd](#tab/cmd)

    ```cmd
    py -m http.server
    ```

1. Tarayıcıda, metin `localhost:8000`kutusuna aşağıdaki fotoğraf URL'lerinden birini girin veya herkese açık herhangi bir resmin URL'sini kullanın.

    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat1.png`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/cat2.png`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog1.png`
    - `https://raw.githubusercontent.com/Azure-Samples/functions-python-tensorflow-tutorial/master/resources/assets/samples/dog2.png`
    
1. Görüntüyü sınıflandırmak için işlev bitiş noktasını çağırmak için **Gönder'i** seçin.

    ![Tamamlanan projenin ekran görüntüsü](media/functions-machine-learning-tensorflow/functions-machine-learning-tensorflow-screenshot.png)

    Tarayıcı, resim URL'sini gönderdiğinde bir hata bildiriyorsa, işlev uygulamasını çalıştırdığınız terminali kontrol edin. "Hiçbir modül 'PIL' bulunamadı" gibi bir hata görürseniz, daha önce oluşturduğunuz sanal ortamı etkinleştirmeden *başlangıç* klasöründe işlev uygulamasını başlatmış olabilirsiniz. Hataları hala görüyorsanız, `pip install -r requirements.txt` etkinleştirilen sanal ortamla yeniden çalıştırın ve hataları arayın.

> [!NOTE]
> Model her zaman görüntünün içeriğini kedi veya köpek olarak sınıfa alır, görüntünün içerip içermediğine bakılmaksızın, köpek için varsayılan olarak. Kaplanve panterlerin görüntüleri, örneğin, genellikle kedi olarak sınıflanır, ancak filler, havuç veya uçak görüntüleri köpek olarak sınıflanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticinin tamamı makinenizde yerel olarak çalıştığından, temizleyecek Azure kaynakları veya hizmetleri yoktur.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, tensorFlow modelini kullanarak görüntüleri sınıflandırmak için Azure İşlevleri ile bir HTTP API bitiş noktası oluşturmayı ve özelleştirmeyi öğrendiniz. Ayrıca, bir web uygulamasından API'yi aramayı da öğrendiniz. Azure İşlevleri tarafından sağlanan sunucusuz bilgi işlem modelinde çalışırken, bu öğreticideki teknikleri, herhangi bir karmaşıklıkAP'ı oluşturmak için kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Azure CLI Kılavuzu'nu kullanarak işlevi Azure İşlevlerine dağıtma](./functions-run-local.md#publish)

Ayrıca bkz:

- [Visual Studio Code'u kullanarak işlevi Azure'a dağıtın.](https://code.visualstudio.com/docs/python/tutorial-azure-functions)
- [Azure Fonksiyonları Python Geliştirici Kılavuzu](./functions-reference-python.md)
