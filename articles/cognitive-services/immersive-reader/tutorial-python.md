---
title: 'Öğretici: Python kullanarak Immersive Reader başlatın'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, Sürükleyici Okuyucu'yu başlatan bir Python uygulaması oluşturursunuz.
services: cognitive-services
author: dylankil
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: tutorial
ms.date: 01/14/2020
ms.author: dylankil
ms.openlocfilehash: a252afae0a007ee0b791b56d19ffb0685848d30a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76844369"
---
# <a name="tutorial-launch-the-immersive-reader-using-the-python-sample-project"></a>Öğretici: Python örnek projesini kullanarak Sürükleyici Okuyucuyu başlatın

Genel [bakışta,](./overview.md)Sürükleyici Okuyucu'nun ne olduğunu ve dil öğrenenler, yeni ortaya çıkan okuyucular ve öğrenme farklılıkları olan öğrenciler için okuma anlamayı geliştirmek için kanıtlanmış teknikleri nasıl uyguladığını öğrendiniz. Bu öğretici, Sürükleyici Reader'ı başlatan bir Python web uygulamasının nasıl oluşturulacağını kapsar. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Örnek bir proje kullanarak Pip, Flask, Jinja ve virtualenv ile python web uygulaması oluşturun
> * Erişim belirteci alma
> * Örnek içerikle Sürükleyici Okuyucuyu başlatın

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

* Azure Etkin Dizin kimlik doğrulaması için yapılandırılan Sürükleyici Bir Reader kaynağı. Kurulumu için [bu yönergeleri](./how-to-create-immersive-reader.md) izleyin. Ortam özelliklerini yapılandırırken burada oluşturulan bazı değerlere ihtiyacınız olacaktır. Oturumunuzun çıktısını ileride başvurmak için bir metin dosyasına kaydedin.
* [Git](https://git-scm.com/)
* [Sürükleyici Okuyucu SDK](https://github.com/microsoft/immersive-reader-sdk)
* [Python](https://www.python.org/downloads/) ve [pip](https://docs.python.org/3/installing/index.html). Python 3.4 ile başlayarak, pip varsayılan olarak Python ikili yükleyicileri ile birlikte verilir.
* [Şişe](https://flask.palletsprojects.com/en/1.0.x/)
* [Jinja](http://jinja.pocoo.org/docs/2.10/)
* [Virtualenv](https://virtualenv.pypa.io/en/latest/) ve [Virtualenvwrapper-Windows](https://pypi.org/project/virtualenvwrapper-win/) veya [OSX için virtualenvwrapper kazanmak](https://virtualenvwrapper.readthedocs.io/en/latest/)
* [istekleri modülü](https://pypi.org/project/requests/2.7.0/)
* [Visual Studio Code](https://code.visualstudio.com/) gibi bir IDE

## <a name="configure-authentication-credentials"></a>Kimlik doğrulama kimlik bilgilerini yapılandırma

_.env_adlı yeni bir dosya oluşturun ve Immersive Reader kaynağınızı oluşturduğunuzda verilen değerleri sağlayarak aşağıdaki kodu yapıştırın.

```text
TENANT_ID={YOUR_TENANT_ID}
CLIENT_ID={YOUR_CLIENT_ID}
CLIENT_SECRET={YOUR_CLIENT_SECRET}
SUBDOMAIN={YOUR_SUBDOMAIN}
```

Bu dosyayı kaynak denetimine işlemeyeceğinden emin olun, çünkü kamuya açıklanmaması gereken sırlar içerir.

**Getimmersivereadertoken** API bitiş noktası, yetkisiz kullanıcıların Sürükleyici Reader hizmetinize ve faturanıza karşı kullanılacak jetonları almasını önlemek için bir tür kimlik doğrulamanın (örneğin, [OAuth)](https://oauth.net/2/)arkasına sabitlenmelidir; bu çalışma bu öğretici kapsamı dışındadır.

## <a name="create-a-python-web-app-on-windows"></a>Windows'da Python web uygulaması oluşturma

Windows'da kullanarak `flask` bir Python web uygulaması oluşturun.

[Git'i yükleyin.](https://git-scm.com/)

Git yüklendikten sonra bir Komut İstemi açın ve Bilgisayarınızdaki bir klasöre Sürükleyici Reader SDK Git deposu 'klon'

```cmd
git clone https://github.com/microsoft/immersive-reader-sdk.git
```

[Python](https://www.python.org/downloads/)'ı yükleyin.

Python'u PATH kutusuna ekle kutusunu işaretleyin.

![Python Windows Install İletişim Adımı 1](./media/pythoninstallone.jpg)

Kutuları işaretleyerek İsteğe Bağlı Özellikler ekle ve ardından 'İleri' düğmesini tıklatın.

![Python Windows Install İletişim Adımı 2](./media/pythoninstalltwo.jpg)

'Özel yükleme'yi seçin ve yükleme yolunu kök klasörünüz olarak ayarlayın, örneğin `C:\Python37-32\` 'Yükle' düğmesini tıklatın.

![Python Windows Install Dialog Adım 3](./media/pythoninstallthree.jpg)

Python Yükleme tamamlandıktan sonra bir `cd` Komut İstemi ve Python Komut Dosyaları klasörüne açın.

```cmd
cd C:\Python37-32\Scripts
```

Flask'ı yükleyin.

```cmd
pip install flask
```

Jinja2'yi yükleyin. Python için tam özellikli şablon motoru.

```cmd
pip install jinja2
```

Virtualenv yükleyin. Yalıtılmış Python ortamları oluşturmak için bir araç.

```cmd
pip install virtualenv
```

virtualenvwrapper-win yükleyin. Virtualenvwrapper arkasındaki fikir virtualenv kullanımını kolaylaştırmaktır.

```cmd
pip install virtualenvwrapper-win
```

İstekmodüllerini yükleyin. İstekler Python'da yazılmış bir Apache2 Lisanslı HTTP kütüphanesidir.

```cmd
pip install requests
```

Python-dotenv modüllerini yükleyin. Bu modül .env dosyasından anahtar değeri çiftini okur ve bunları ortam değişkenine ekler.

```cmd
pip install python-dotenv
```

Sanal ortam yaratın

```cmd
mkvirtualenv advanced-python
```

`cd`örnek proje kök klasörüne.

```cmd
cd C:\immersive-reader-sdk\js\samples\advanced-python
```

Örnek projeyi ortama bağlayın. Bu, yeni oluşturulan sanal ortamı örnek proje kök klasörüne eşler.

```cmd
setprojectdir .
```

Sanal ortamı etkinleştirin.

```cmd
activate
```

Proje artık etkin olmalıdır ve Komut İstemi'nde gibi `(advanced-python) C:\immersive-reader-sdk\js\samples\advanced-python>` bir şey görürsünüz.

Ortamı devre dışı bırakın.

```cmd
deactivate
```

Önek `(advanced-python)` şimdi ortam devre dışı bırakıldığından gitmiş olmalıdır.

Ortamı yeniden etkinleştirmek `workon advanced-python` için örnek proje kök klasöründen çalıştırın.

```cmd
workon advanced-python
```

### <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle Sürükleyici Okuyucuyu başlatın

Ortam etkin olduğunda, örnek proje kök `flask run` klasöründen girerek örnek projeyi çalıştırın.

```cmd
flask run
```

Tarayıcınızı açın ve _http://localhost:5000_' ye gidin.

## <a name="create-a-python-web-app-on-osx"></a>OSX'te Python web uygulaması oluşturma

OSX'i kullanarak `flask` bir Python web uygulaması oluşturun.

[Git'i yükleyin.](https://git-scm.com/)

Git terminali ve 'klon' yüklü sonra Bilgisayarınızdaki bir klasöre Sürükleyici Reader SDK Git deposu

```bash
git clone https://github.com/microsoft/immersive-reader-sdk.git
```

[Python](https://www.python.org/downloads/)'ı yükleyin.

Python kök klasörü örneğin `Python37-32` artık Uygulamalar klasöründe olmalıdır.

Python Yükleme tamamlandıktan sonra `cd` Terminal'i ve Python Scripts klasörünü açın.

```bash
cd immersive-reader-sdk/js/samples/advanced-python
```

Pip yükleyin.

```bash
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
```

Ardından, izin sorunlarını önlemek için şu anda oturum açmış kullanıcı için pip yüklemek için aşağıdakileri çalıştırın.

```bash
python get-pip.py --user
```

```bash
sudo nano /etc/paths
```

- İstendiğinde parolanızı girin.
- PIP yüklemenizin yolunu PATH değişkeninize ekleyin.
- Dosyanın altına gidin ve listenin son öğesi olarak eklemek istediğiniz yolu girin. `PATH=$PATH:/usr/local/bin`
- Çıkmak için control-x tuşuna bas.
- Değiştirilen `Y` arabelleği kaydetmek için girin.
- İşte bu kadar! Test etmek için, yeni Terminal `echo $PATH`penceresinde: .

Flask'ı yükleyin.

```bash
pip install flask --user
```

Jinja2'yi yükleyin. Python için tam özellikli şablon motoru.

```bash
pip install Jinja2 --user
```

Virtualenv yükleyin. Yalıtılmış Python ortamları oluşturmak için bir araç.

```bash
pip install virtualenv --user
```

virtualenvwrapper yükleyin. Virtualenvwrapper arkasındaki fikir virtualenv kullanımını kolaylaştırmaktır.

```bash
pip install virtualenvwrapper --user
```

İstekmodüllerini yükleyin. İstekler Python'da yazılmış bir Apache2 Lisanslı HTTP kütüphanesidir.

```bash
pip install requests --user
```

Python-dotenv modüllerini yükleyin. Bu modül .env dosyasından anahtar değeri çiftini okur ve bunları ortam değişkenine ekler.

```bash
pip install python-dotenv --user
```

Sanal ortamlarınızı korumak istediğiniz bir klasör seçin ve bu komutu çalıştırın

```bash
mkdir ~/.virtualenvs
```

`cd`Immersive Reader SDK Python örnek uygulama klasörüne.

```bash
cd immersive-reader-sdk/js/samples/advanced-python
```

Sanal ortam yaratın

```bash
mkvirtualenv -p /usr/local/bin/python3 advanced-python
```

Örnek projeyi ortama bağlayın. Bu, yeni oluşturulan sanal ortamı örnek proje kök klasörüne eşler.

```bash
setprojectdir .
```

Sanal ortamı etkinleştirin.

```bash
activate
```

Proje artık etkin olmalıdır ve Komut İstemi'nde gibi `(advanced-python) /immersive-reader-sdk/js/samples/advanced-python>` bir şey görürsünüz.

Ortamı devre dışı bırakın.

```bash
deactivate
```

Önek `(advanced-python)` şimdi ortam devre dışı bırakıldığından gitmiş olmalıdır.

Ortamı yeniden etkinleştirmek `workon advanced-python` için örnek proje kök klasöründen çalıştırın.

```bash
workon advanced-python
```

## <a name="launch-the-immersive-reader-with-sample-content"></a>Örnek içerikle Sürükleyici Okuyucuyu başlatın

Ortam etkin olduğunda, örnek proje kök `flask run` klasöründen girerek örnek projeyi çalıştırın.

```bash
flask run
```

Tarayıcınızı açın ve _http://localhost:5000_' ye gidin.

## <a name="next-steps"></a>Sonraki adımlar

* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](./reference.md) keşfedin
* Kod örneklerini [GitHub'da](https://github.com/microsoft/immersive-reader-sdk/tree/master/js/samples/) görüntüleme
