---
title: 'Öğretici: Metni çevirmek, sentezlemek ve analiz etmek için bir Flask uygulaması oluşturun - Çevirmen Metin API'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, metni çevirmek, duyguları analiz etmek ve çevrilmiş metni konuşmaya sentezlemek için Flask tabanlı bir web uygulaması oluşturacaksınız.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: swmachan
ms.openlocfilehash: 5034dafa015054e9e9d0804088f345929815b974
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80397945"
---
# <a name="tutorial-build-a-flask-app-with-azure-cognitive-services"></a>Öğretici: Azure Bilişsel Hizmetleri ile Bir Flask uygulaması oluşturun

Bu eğitimde, metni çevirmek, duyguları analiz etmek ve çevrilmiş metni konuşmaya sentezlemek için Azure Bilişsel Hizmetleri kullanan bir Flask web uygulaması oluşturacaksınız. Bizim odak Python kodu ve bizim uygulama sağlayan Flask yolları, ancak, biz birlikte uygulama çeker HTML ve Javascript ile size yardımcı olacaktır. Herhangi bir sorunla karşınıza bakarsanız, aşağıdaki geri bildirim düğmesini kullanarak bize bildirin.

Bu öğreticinin kapsadığı şey şu:

> [!div class="checklist"]
> * Azure abonelik anahtarlarını alın
> * Geliştirme ortamınızı ayarlama ve bağımlılıkları yükleme
> * Flask uygulaması oluşturma
> * Metni çevirmek için Çevirmen Metin API'sini kullanın
> * Giriş metni nin ve çevirilerin olumlu/olumsuz duyarlılığını analiz etmek için Metin Analizi'ni kullanın
> * Çevrilmiş metni sentezlenmiş konuşmaya dönüştürmek için Konuşma Hizmetlerini kullanma
> * Flask uygulamanızı yerel olarak çalıştırın

> [!TIP]
> İleri atlamak ve aynı anda tüm kodu görmek istiyorsanız, tüm örnek, yapı talimatları ile birlikte [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Flask-App-Tutorial)mevcuttur.

## <a name="what-is-flask"></a>Flask nedir?

Flask web uygulamaları oluşturmak için bir mikroframework olduğunu. Bu, Flask'ın size bir web uygulaması oluşturmanıza olanak tanıyan araçlar, kitaplıklar ve teknolojiler sağladığı anlamına gelir. Bu web uygulaması bazı web sayfaları, bir blog, bir wiki veya bir web tabanlı takvim uygulaması veya ticari bir web sitesi olarak maddi olarak gitmek olabilir.

Bu öğretici sonra derin dalış isteyenler için burada birkaç yararlı bağlantılar şunlardır:

* [Flask belgeleri](http://flask.pocoo.org/)
* [Dummies için Flask - Flask için Bir Acemi Kılavuzu](https://codeburst.io/flask-for-dummies-a-beginners-guide-to-flask-part-uno-53aec6afc5b1)

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici için ihtiyacınız olan yazılım ve abonelik anahtarlarını gözden geçirelim.

* [Python 3.5.2 veya sonrası](https://www.python.org/downloads/)
* [Git araçları](https://git-scm.com/downloads)
* [Visual Studio Code](https://code.visualstudio.com/) veya [Atom](https://atom.io/) gibi bir IDE veya metin düzenleyicisi  
* [Chrome](https://www.google.com/chrome/browser/) veya [Firefox](https://www.mozilla.org/firefox)
* **Çevirmen Metni** abonelik anahtarı (Bir bölge seçmeniz gerekmemektedir.)
* **Batı ABD** bölgesinde bir **Text Analytics** abonelik anahtarı.
* **Batı ABD** bölgesinde bir **Konuşma Hizmetleri** abonelik anahtarı.

## <a name="create-an-account-and-subscribe-to-resources"></a>Hesap oluşturma ve kaynaklara abone ol

Daha önce de belirtildiği gibi, bu öğretici için üç abonelik anahtarı gerekir. Bu, Azure hesabınızda şu süreyle bir kaynak oluşturmanız gerektiği anlamına gelir:
* Translator Metin Çevirisi
* Metin Analizi
* Konuşma Hizmetleri

Kaynak oluşturmak için adım adım yönergeler için [Azure portalında Bilişsel Hizmetler Hesabı Oluştur'u](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) kullanın.

> [!IMPORTANT]
> Bu eğitim için lütfen Kaynaklarınızı Batı ABD bölgesinde oluşturun. Farklı bir bölge kullanıyorsanız, Python dosyalarınızın her birinde temel URL'yi ayarlamanız gerekir.

## <a name="set-up-your-dev-environment"></a>Geliştirme ortamınızı kurma

Flask web uygulamanızı oluşturmadan önce, projeniz için bir çalışma dizini oluşturmanız ve birkaç Python paketi yüklemeniz gerekir.

### <a name="create-a-working-directory"></a>Çalışma dizini oluşturma

1. Komut satırLarını (Windows) veya terminali (macOS/Linux) açın. Ardından, projeniz için çalışan bir dizin ve alt dizinoluşturun:  

   ```
   mkdir -p flask-cog-services/static/scripts && mkdir flask-cog-services/templates
   ```
2. Projenizin çalışma dizinini değiştirin:  

   ```
   cd flask-cog-services
   ```

### <a name="create-and-activate-your-virtual-environment-with-virtualenv"></a>Sanal ortamınızı oluşturun ve etkinleştirin`virtualenv`

Flask uygulamamız için sanal bir ortam `virtualenv`oluşturalım. Sanal bir ortam kullanmak, çalışmak için temiz bir ortama sahip olmasını sağlar.

1. Çalışma dizininizde, sanal bir ortam oluşturmak için bu komutu çalıştırın: **macOS/Linux:**
   ```
   virtualenv venv --python=python3
   ```
   Sanal ortamın Python 3'ün kullanılması gerektiğini açıkça beyan ettik. Bu, birden çok Python yüklemesi olan kullanıcıların doğru sürümü kullanmasını sağlar.

   **Windows CMD / Windows Bash:**
   ```
   virtualenv venv
   ```
   Basit tutmak için, sanal çevre venv adlandırma konum.

2. Sanal ortamınızı etkinleştirme komutları platformunuza/kabuğunuza bağlı olarak değişir:   

   | Platform | Kabuk | Komut |
   |----------|-------|---------|
   | macOS/Linux | bash/zsh | `source venv/bin/activate` |
   | Windows | bash | `source venv/Scripts/activate` |
   | | Komut Satırı | `venv\Scripts\activate.bat` |
   | | PowerShell | `venv\Scripts\Activate.ps1` |

   Bu komutu çalıştırdıktan sonra, komut satırı nız `venv`veya terminal oturumunuz .

3. Bunu komut satırına veya terminale yazarak oturumu istediğiniz `deactivate`zaman devre dışı bırakabilirsiniz: .

> [!NOTE]
> Python sanal ortamlar oluşturmak ve yönetmek için kapsamlı belgelere sahiptir, [virtualenv](https://virtualenv.pypa.io/en/latest/)bakın.

### <a name="install-requests"></a>İstekleri yükleme

İstekler, HTTP 1.1 isteklerini göndermek için kullanılan popüler bir modüldür. URL'lerinize sorgu dizelerini el ile eklemenize veya POST verilerinizi oluşturmanıza gerek yoktur.

1. İstekleri yüklemek için çalıştırın:

   ```
   pip install requests
   ```

> [!NOTE]
> İstekler hakkında daha fazla bilgi edinmek istiyorsanız, [Bkz. İstekler: İnsanlar için HTTP](https://2.python-requests.org/en/master/).

### <a name="install-and-configure-flask"></a>Flask'ı yükleyin ve yapılandırır

Sonra Flask'ı yüklememiz gerekiyor. Flask web uygulamamızın yönlendirmesini işler ve abonelik anahtarlarımızı son kullanıcıdan gizleyen sunucudan sunucuya aramalar yapmamızı sağlar.

1. Flask'ı yüklemek için çalıştırın:
   ```
   pip install Flask
   ```
   Flask'ın kurulduğundan emin olalım. Çalıştırın:
   ```
   flask --version
   ```
   Sürüm terminale yazdırılmalıdır. Başka bir şey bir şeylerin ters gittiği anlamına gelir.

2. Flask uygulamasını çalıştırmak için, flask komutunu veya Python'un Flask ile -m anahtarını kullanabilirsiniz. Bunu yapmadan önce, çevre değişkenini `FLASK_APP` dışa aktararak terminalinize hangi uygulamayla çalışacağınızı söylemeniz gerekir:

   **macOS/Linux**:
   ```
   export FLASK_APP=app.py
   ```

   **Windows**:
   ```
   set FLASK_APP=app.py
   ```

## <a name="create-your-flask-app"></a>Flask uygulamanızı oluşturun

Bu bölümde, kullanıcılar uygulamanızın köküne bastığında bir HTML dosyasını döndüren bir barebones Flask uygulaması oluşturacaksınız. Kodu ayırmak için çok fazla zaman harcamayın, bu dosyayı daha sonra güncellemek için geri geleceğiz.

### <a name="what-is-a-flask-route"></a>Flask rotası nedir?

["Rotalar"](http://flask.pocoo.org/docs/1.0/api/#flask.Flask.route)hakkında konuşmak için bir dakika verelim. Yönlendirme, bir URL'yi belirli bir işleve bağlamak için kullanılır. Flask, işlevleri belirli URL'lere kaydetmek için rota dekoratörleri kullanır. Örneğin, bir kullanıcı web uygulamamızın`/`köküne () `index.html` gittiğinde işlenir.  

```python
@app.route('/')
def index():
    return render_template('index.html')
```

Bu evi çekiçlemek için bir örnek daha verelim.

```python
@app.route('/about')
def about():
    return render_template('about.html')
```

Bu kod, bir kullanıcı dosyanın `http://your-web-app.com/about` `about.html` işlenme yerlerine gittiğinde bunu sağlar.

Bu örnekler bir kullanıcı için html sayfalarının nasıl oluşturulabileceğini gösterse de, bir düğmeye basıldığında API'leri çağırmak veya ana sayfadan uzağa gitmek zorunda kalmadan herhangi bir sayıda eylemde bulunmak için rotalar da kullanılabilir. Çeviri, duygu ve konuşma sentezi için rotalar oluşturduğunuzda bunu iş başında görürsünüz.

### <a name="get-started"></a>başlarken

1. Projeyi IDE'nizde açın ve çalışma `app.py` dizinizin kökünde bir dosya oluşturun. Ardından, bu kodu `app.py` kopyalayın ve kaydedin:

   ```python
   from flask import Flask, render_template, url_for, jsonify, request

   app = Flask(__name__)
   app.config['JSON_AS_ASCII'] = False

   @app.route('/')
   def index():
       return render_template('index.html')
   ```

   Bu kod bloğu, bir `index.html` kullanıcı web uygulamanızın köküne gittiğinde uygulamayı`/`görüntülemesini söyler ( ).

2. Ardından, web uygulamamız için ön uç oluşturalım. Dizinde adlı `index.html` bir dosya oluşturun. `templates` Daha sonra bu `templates/index.html`kodu ' ya kopyalayın.

   ```html
   <!doctype html>
   <html lang="en">
     <head>
       <!-- Required metadata tags -->
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       <meta name="description" content="Translate and analyze text with Azure Cognitive Services.">
       <!-- Bootstrap CSS -->
       <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
       <title>Translate and analyze text with Azure Cognitive Services</title>
     </head>
     <body>
       <div class="container">
         <h1>Translate, synthesize, and analyze text with Azure</h1>
         <p>This simple web app uses Azure for text translation, text-to-speech conversion, and sentiment analysis of input text and translations. Learn more about <a href="https://docs.microsoft.com/azure/cognitive-services/">Azure Cognitive Services</a>.
        </p>
        <!-- HTML provided in the following sections goes here. -->

        <!-- End -->
       </div>

       <!-- Required Javascript for this tutorial -->
       <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
       <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
       <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
       <script type = "text/javascript" src ="static/scripts/main.js"></script>
     </body>
   </html>
   ```

3. Flask uygulamasını test edelim. Terminalden çalıştırın:

   ```
   flask run
   ```

4. Bir tarayıcı açın ve sağlanan URL'ye gidin. Tek sayfalı uygulamanızı görmelisiniz. Uygulamayı öldürmek için **Ctrl + C** tuşuna basın.

## <a name="translate-text"></a>Metin çevirme

Artık basit bir Flask uygulamasının nasıl çalıştığına dair bir fikriniz olduğuna göre, şunları yapalım:

* Çevirmen Metin API'sini aramak ve yanıt vermek için bazı Python yazın
* Python kodunuzu aramak için bir Flask rotası oluşturun
* HTML'yi metin girişi ve çevirisi için bir alan, bir dil seçici ve çevirme düğmesiyle güncelleştirin
* Kullanıcıların HTML'den Flask uygulamanızla etkileşimkurmasını sağlayan Javascript yazın

### <a name="call-the-translator-text-api"></a>Çevirmen Metin API'sini Arayın

Yapmanız gereken ilk şey Çevirmen Metin API aramak için bir işlev yazmaktır. Bu işlev iki bağımsız `text_input` `language_output`değişken alır: ve . Bir kullanıcı uygulamanızdaki çeviri düğmesine bastığında bu işlev çağrılır. HTML'deki metin alanı `text_input`, ve HTML'deki dil seçim değeri `language_output`.

1. Çalışma dizininizde root adlı `translate.py` bir dosya oluşturarak başlayalım.
2. Ardından, bu kodu `translate.py`ekle. Bu işlev iki `text_input` bağımsız `language_output`değişken alır: ve .
   ```python
   import os, requests, uuid, json

   # Don't forget to replace with your Cog Services subscription key!
   # If you prefer to use environment variables, see Extra Credit for more info.
   subscription_key = 'YOUR_TRANSLATOR_TEXT_SUBSCRIPTION_KEY'
   
   # Don't forget to replace with your Cog Services location!
   # Our Flask route will supply two arguments: text_input and language_output.
   # When the translate text button is pressed in our Flask app, the Ajax request
   # will grab these values from our web app, and use them in the request.
   # See main.js for Ajax calls.
   def get_translation(text_input, language_output):
       base_url = 'https://api.cognitive.microsofttranslator.com'
       path = '/translate?api-version=3.0'
       params = '&to=' + language_output
       constructed_url = base_url + path + params

       headers = {
           'Ocp-Apim-Subscription-Key': subscription_key,
           'Ocp-Apim-Subscription-Region': 'location',
           'Content-type': 'application/json',
           'X-ClientTraceId': str(uuid.uuid4())
       }

       # You can pass more than one object in body.
       body = [{
           'text' : text_input
       }]
       response = requests.post(constructed_url, headers=headers, json=body)
       return response.json()
   ```
3. Çevirmen Metni abonelik anahtarınızı ekleyin ve kaydedin.

### <a name="add-a-route-to-apppy"></a>Rota ekleme`app.py`

Ardından, Flask uygulamanızda `translate.py`'. Bu rota, bir kullanıcı uygulamanızdaki çeviri düğmesine her bastadında çağrılacaktır.

Bu uygulama için rotanız istekleri `POST` kabul edeceğiz. Bunun nedeni, işlevin metnin çevrilmesini ve çeviri için bir çıktı dili beklemesidir.

Flask, her isteği ayrışdırmana ve yönetmenize yardımcı işlevler sağlar. Sağlanan kodda, `get_json()` `POST` istekteki verileri JSON olarak döndürür. Daha `data['text']` sonra `data['to']`kullanarak ve , metin ve `get_translation()` çıkış `translate.py`dil değerleri işlevine geçirilir. Bu verileri web uygulamanızda görüntülemeniz gerektiğinden, yanıtı JSON olarak döndürmek son adımdır.

Aşağıdaki bölümlerde, duyarlılık analizi ve konuşma sentezi için rotalar oluştururken bu işlemi yinelersiniz.

1. İçe alma deyimini en üste açın `app.py` `app.py` ve bulun ve aşağıdaki satırı ekleyin:

   ```python
   import translate
   ```
   Artık Flask uygulamamız üzerinden `translate.py`mevcut yöntemi kullanabilirsiniz.

2. Bu kodu sonuna kadar `app.py` kopyalayın ve kaydedin:

   ```python
   @app.route('/translate-text', methods=['POST'])
   def translate_text():
       data = request.get_json()
       text_input = data['text']
       translation_output = data['to']
       response = translate.get_translation(text_input, translation_output)
       return jsonify(response)
   ```

### <a name="update-indexhtml"></a>`index.html` dosyasını güncelleştirme

Artık metni çevirmek için bir işleviniz ve Flask uygulamanızda bunu aramak için bir rotanız olduğuna göre, bir sonraki adım uygulamanız için HTML oluşturmaya başlamaktır. Aşağıdaki HTML birkaç şey yapar:

* Kullanıcıların çevirmek için metin giriş yapabilecekleri bir metin alanı sağlar.
* Bir dil seçici içerir.
* Algılanan dili ve çeviri sırasında döndürülen güven puanlarını işlemek için HTML öğeleri içerir.
* Çeviri çıktısının görüntülendiği salt okunur metin alanı sağlar.
* Daha sonra öğreticide bu dosyaya ekleyeceğiniz duygu analizi ve konuşma sentezi kodu için yer tutucuları içerir.

`index.html`Güncelleyelim.

1. Bu `index.html` kod yorumlarını açın ve bulun:
   ```html
   <!-- HTML provided in the following sections goes here. -->

   <!-- End -->
   ```

2. Kod yorumlarını bu HTML bloğuyla değiştirin:
   ```html
   <div class="row">
     <div class="col">
       <form>
         <!-- Enter text to translate. -->
         <div class="form-group">
           <label for="text-to-translate"><strong>Enter the text you'd like to translate:</strong></label>
           <textarea class="form-control" id="text-to-translate" rows="5"></textarea>
         </div>
         <!-- Select output language. -->
         <div class="form-group">
           <label for="select-language"><strong>Translate to:</strong></label>
           <select class="form-control" id="select-language">
             <option value="ar">Arabic</option>
             <option value="ca">Catalan</option>
             <option value="zh-Hans">Chinese (Simplified)</option>
             <option value="zh-Hant">Chinese (Traditional)</option>
             <option value="hr">Croatian</option>
             <option value="en">English</option>
             <option value="fr">French</option>
             <option value="de">German</option>
             <option value="el">Greek</option>
             <option value="he">Hebrew</option>
             <option value="hi">Hindi</option>
             <option value="it">Italian</option>
             <option value="ja">Japanese</option>
             <option value="ko">Korean</option>
             <option value="pt">Portuguese</option>
             <option value="ru">Russian</option>
             <option value="es">Spanish</option>
             <option value="th">Thai</option>
             <option value="tr">Turkish</option>
             <option value="vi">Vietnamese</option>
           </select>
         </div>
         <button type="submit" class="btn btn-primary mb-2" id="translate">Translate text</button></br>
         <div id="detected-language" style="display: none">
           <strong>Detected language:</strong> <span id="detected-language-result"></span><br />
           <strong>Detection confidence:</strong> <span id="confidence"></span><br /><br />
         </div>

         <!-- Start sentiment code-->

         <!-- End sentiment code -->

       </form>
     </div>
     <div class="col">
       <!-- Translated text returned by the Translate API is rendered here. -->
       <form>
         <div class="form-group" id="translator-text-response">
           <label for="translation-result"><strong>Translated text:</strong></label>
           <textarea readonly class="form-control" id="translation-result" rows="5"></textarea>
         </div>

         <!-- Start voice font selection code -->

         <!-- End voice font selection code -->

       </form>

       <!-- Add Speech Synthesis button and audio element -->

       <!-- End Speech Synthesis button -->

     </div>
   </div>
   ```

Bir sonraki adım bazı Javascript yazmaktır. Bu, HTML ve Flask rotanız arasındaki köprüdür.

### <a name="create-mainjs"></a>Oluşturmak`main.js`  

Dosya, `main.js` HTML ve Flask rotanız arasındaki köprüdür. Uygulamanız içeriği işlemek ve Flask rotalarınıza `POST` istekte bulunmak için jQuery, Ajax ve XMLHttpRequest'in bir birleşimini kullanır.

Aşağıdaki kodda, HTML'den gelen içerik Flask rotanıza bir istek oluşturmak için kullanılır. Özellikle, metin alanı nın içeriği ve dil seçici değişkenlere atanır ve daha `translate-text`sonra istek boyunca geçirilir.

Kod daha sonra yanıtı yineler ve HTML'yi çeviri, algılanan dil ve güven puanıyla güncelleştirir.

1. IDE'nizden, `static/scripts` dizinde `main.js` bir dosya oluşturun.
2. Bu kodu `static/scripts/main.js`aşağıdakilere kopyalayın:
   ```javascript
   //Initiate jQuery on load.
   $(function() {
     //Translate text with flask route
     $("#translate").on("click", function(e) {
       e.preventDefault();
       var translateVal = document.getElementById("text-to-translate").value;
       var languageVal = document.getElementById("select-language").value;
       var translateRequest = { 'text': translateVal, 'to': languageVal }

       if (translateVal !== "") {
         $.ajax({
           url: '/translate-text',
           method: 'POST',
           headers: {
               'Content-Type':'application/json'
           },
           dataType: 'json',
           data: JSON.stringify(translateRequest),
           success: function(data) {
             for (var i = 0; i < data.length; i++) {
               document.getElementById("translation-result").textContent = data[i].translations[0].text;
               document.getElementById("detected-language-result").textContent = data[i].detectedLanguage.language;
               if (document.getElementById("detected-language-result").textContent !== ""){
                 document.getElementById("detected-language").style.display = "block";
               }
               document.getElementById("confidence").textContent = data[i].detectedLanguage.score;
             }
           }
         });
       };
     });
     // In the following sections, you'll add code for sentiment analysis and
     // speech synthesis here.
   })
   ```

### <a name="test-translation"></a>Test çevirisi

Çeviriyi uygulamada test edelim.

```
flask run
```

Sağlanan sunucu adresine gidin. Giriş alanına metin yazın, bir dil seçin ve çevir'e basın. Bir çeviri almalısın. İşe yaramazsa, abonelik anahtarınızı eklediğinizden emin olun.

> [!TIP]
> Yaptığınız değişiklikler görünmüyorsa veya uygulama beklediğiniz gibi çalışmıyorsa, önbelleğinizi temizlemeyi veya özel/gizli bir pencere açmayı deneyin.

Uygulamayı öldürmek için **CTRL + c** tuşuna basın ve bir sonraki bölüme gidin.

## <a name="analyze-sentiment"></a>Yaklaşımı analiz etme

[Metin Analizi API' si](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) duygu analizi yapmak, metinden anahtar tümcecikleri ayıklamak veya kaynak dili algılamak için kullanılabilir. Bu uygulamada, sağlanan metnin pozitif, nötr veya negatif olup olmadığını belirlemek için duyarlılık analizini kullanacağız. API, 0 ile 1 arasında sayısal bir puan döndürür. 1'e yakın puanlar olumlu, 0'a yakın puanlar ise olumsuz duyguları gösterir.

Bu bölümde, birkaç şey yapmak için gidiyoruz:

* Duyarlılık analizi yapmak ve yanıt vermek için Text Analytics API'sini aramak için bazı Python'lar yazın
* Python kodunuzu aramak için bir Flask rotası oluşturun
* HTML'yi duyarlılık puanları için bir alan ve analiz yapmak için bir düğmeyle güncelleştirin
* Kullanıcıların HTML'den Flask uygulamanızla etkileşimkurmasını sağlayan Javascript yazın

### <a name="call-the-text-analytics-api"></a>Metin Analizi API’sini çağırma

Text Analytics API'yi aramak için bir işlev yazalım. Bu işlev dört bağımsız `input_text` `input_language`değişken `output_text`alır: , , ve `output_language`. Bu işlev, bir kullanıcı uygulamanızdaki çalıştır düğmesine bastığında çağrır. Kullanıcı tarafından metin alanından ve dil seçiciden sağlanan veriler ile algılanan dil ve çeviri çıktısı her istekle birlikte sağlanır. Yanıt nesnesi kaynak ve çeviri için duyarlılık puanları içerir. Aşağıdaki bölümlerde, yanıtı ayrışdırmak ve uygulamanızda kullanmak için bazı Javascript yazacaksınız. Şimdilik, Text Analytics API'yi aramaya odaklanalım.

1. Çalışma dizininizde adı `sentiment.py` verilen bir dosya oluşturalım.
2. Ardından, bu kodu `sentiment.py`ekle.
   ```python
   import os, requests, uuid, json

   # Don't forget to replace with your Cog Services subscription key!
   subscription_key = 'YOUR_TEXT_ANALYTICS_SUBSCRIPTION_KEY'

   # Our Flask route will supply four arguments: input_text, input_language,
   # output_text, output_language.
   # When the run sentiment analysis button is pressed in our Flask app,
   # the Ajax request will grab these values from our web app, and use them
   # in the request. See main.js for Ajax calls.

   def get_sentiment(input_text, input_language, output_text, output_language):
       base_url = 'https://westus.api.cognitive.microsoft.com/text/analytics'
       path = '/v2.0/sentiment'
       constructed_url = base_url + path

       headers = {
           'Ocp-Apim-Subscription-Key': subscription_key,
           'Content-type': 'application/json',
           'X-ClientTraceId': str(uuid.uuid4())
       }

       # You can pass more than one object in body.
       body = {
           'documents': [
               {
                   'language': input_language,
                   'id': '1',
                   'text': input_text
               },
               {
                   'language': output_language,
                   'id': '2',
                   'text': output_text
               }
           ]
       }
       response = requests.post(constructed_url, headers=headers, json=body)
       return response.json()
   ```
3. Text Analytics abonelik anahtarınızı ekleyin ve kaydedin.

### <a name="add-a-route-to-apppy"></a>Rota ekleme`app.py`

Flask uygulamanızda `sentiment.py`'. Bu rota, bir kullanıcı uygulamanızdaki çalıştır duygu analizi düğmesine her bastadında çağrılacaktır. Çeviri rotası gibi, işlev bağımsız değişkenler beklediğinden bu rota da istekleri kabul `POST` edemeye çalışır.

1. İthalat `app.py` deyimini en üstte `app.py` açın ve bulun ve güncelleyin:

   ```python
   import translate, sentiment
   ```
   Artık Flask uygulamamız üzerinden `sentiment.py`mevcut yöntemi kullanabilirsiniz.

2. Bu kodu sonuna kadar `app.py` kopyalayın ve kaydedin:
   ```python
   @app.route('/sentiment-analysis', methods=['POST'])
   def sentiment_analysis():
       data = request.get_json()
       input_text = data['inputText']
       input_lang = data['inputLanguage']
       output_text = data['outputText']
       output_lang =  data['outputLanguage']
       response = sentiment.get_sentiment(input_text, input_lang, output_text, output_lang)
       return jsonify(response)
   ```

### <a name="update-indexhtml"></a>`index.html` dosyasını güncelleştirme

Artık duygu analizini çalıştırmak için bir işleviniz ve Flask uygulamanızda bunu aramak için bir rotanız olduğuna göre, bir sonraki adım uygulamanız için HTML yazmaya başlamaktır. Aşağıdaki HTML birkaç şey yapar:

* Duyarlılık analizini çalıştırmak için uygulamanız için bir düğme ekler
* Duyarlılık puanlamaaçıklayan bir unsur ekler
* Duyarlılık puanlarını görüntülemek için bir öğe ekler

1. Bu `index.html` kod yorumlarını açın ve bulun:
   ```html
   <!-- Start sentiment code-->

   <!-- End sentiment code -->
   ```

2. Kod yorumlarını bu HTML bloğuyla değiştirin:
   ```html
   <button type="submit" class="btn btn-primary mb-2" id="sentiment-analysis">Run sentiment analysis</button></br>
   <div id="sentiment" style="display: none">
      <p>Sentiment scores are provided on a 1 point scale. The closer the sentiment score is to 1, indicates positive sentiment. The closer it is to 0, indicates negative sentiment.</p>
      <strong>Sentiment score for input:</strong> <span id="input-sentiment"></span><br />
      <strong>Sentiment score for translation:</strong> <span id="translation-sentiment"></span>
   </div>
   ```

### <a name="update-mainjs"></a>`main.js` dosyasını güncelleştirme

Aşağıdaki kodda, HTML'den gelen içerik Flask rotanıza bir istek oluşturmak için kullanılır. Özellikle, metin alanının içeriği ve dil seçici değişkenlere atanır ve daha sonra `sentiment-analysis` istek içinde rotaya iletilir.

Kod daha sonra yanıtı yineler ve HTML'yi duyarlılık puanlarıyla güncelleştirir.

1. IDE'nizden, `static` dizinde `main.js` bir dosya oluşturun.

2. Bu kodu `static/scripts/main.js`aşağıdakilere kopyalayın:
   ```javascript
   //Run sentinment analysis on input and translation.
   $("#sentiment-analysis").on("click", function(e) {
     e.preventDefault();
     var inputText = document.getElementById("text-to-translate").value;
     var inputLanguage = document.getElementById("detected-language-result").innerHTML;
     var outputText = document.getElementById("translation-result").value;
     var outputLanguage = document.getElementById("select-language").value;

     var sentimentRequest = { "inputText": inputText, "inputLanguage": inputLanguage, "outputText": outputText,  "outputLanguage": outputLanguage };

     if (inputText !== "") {
       $.ajax({
         url: "/sentiment-analysis",
         method: "POST",
         headers: {
             "Content-Type":"application/json"
         },
         dataType: "json",
         data: JSON.stringify(sentimentRequest),
         success: function(data) {
           for (var i = 0; i < data.documents.length; i++) {
             if (typeof data.documents[i] !== "undefined"){
               if (data.documents[i].id === "1") {
                 document.getElementById("input-sentiment").textContent = data.documents[i].score;
               }
               if (data.documents[i].id === "2") {
                 document.getElementById("translation-sentiment").textContent = data.documents[i].score;
               }
             }
           }
           for (var i = 0; i < data.errors.length; i++) {
             if (typeof data.errors[i] !== "undefined"){
               if (data.errors[i].id === "1") {
                 document.getElementById("input-sentiment").textContent = data.errors[i].message;
               }
               if (data.errors[i].id === "2") {
                 document.getElementById("translation-sentiment").textContent = data.errors[i].message;
               }
             }
           }
           if (document.getElementById("input-sentiment").textContent !== '' && document.getElementById("translation-sentiment").textContent !== ""){
             document.getElementById("sentiment").style.display = "block";
           }
         }
       });
     }
   });
   // In the next section, you'll add code for speech synthesis here.
   ```

### <a name="test-sentiment-analysis"></a>Test duyarlılık analizi

Uygulamada duyarlılık analizini test edelim.

```
flask run
```

Sağlanan sunucu adresine gidin. Giriş alanına metin yazın, bir dil seçin ve çevir'e basın. Bir çeviri almalısın. Ardından, çalıştır duygusallık analizi düğmesine basın. İki skor görmelisin. İşe yaramazsa, abonelik anahtarınızı eklediğinizden emin olun.

> [!TIP]
> Yaptığınız değişiklikler görünmüyorsa veya uygulama beklediğiniz gibi çalışmıyorsa, önbelleğinizi temizlemeyi veya özel/gizli bir pencere açmayı deneyin.

Uygulamayı öldürmek için **CTRL + c** tuşuna basın ve bir sonraki bölüme gidin.

## <a name="convert-text-to-speech"></a>Metin okumayı dönüştürme

[Metinden konuşmaya API,](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech) uygulamanızın metni doğal insan benzeri sentezlenmiş konuşmaya dönüştürmesini sağlar. Hizmet standart, sinirsel ve özel sesleri destekler. Örnek uygulamamız, tam liste için desteklenen [dillere](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech)bakın, kullanılabilir seslerin bir avuç kullanır.

Bu bölümde, birkaç şey yapmak için gidiyoruz:

* Metinden konuşmaya API ile metinden konuşmaya dönüştürmek için bazı Python yazın
* Python kodunuzu aramak için bir Flask rotası oluşturun
* Metinden konuşmaya dönüştürme düğmesi ve ses çalma öğesi ile HTML'yi güncelleştirin
* Kullanıcıların Flask uygulamanızla etkileşimkurmasını sağlayan Javascript yazın

### <a name="call-the-text-to-speech-api"></a>Metinden Konuşmaya API'yi arayın

Metinden konuşmaya dönüştürmek için bir işlev yazalım. Bu işlev iki bağımsız `input_text` `voice_font`değişken alır: ve . Bu işlev, bir kullanıcı uygulamanızdaki metinden konuşmaya dönüştür düğmesine bastığında çağrılır. `input_text`metin çevirmek için çağrı tarafından döndürülen `voice_font` çeviri çıktıs, HTML ses yazı tipi seçici gelen değerdir.

1. Çalışma dizininizde adı `synthesize.py` verilen bir dosya oluşturalım.

2. Ardından, bu kodu `synthesize.py`ekle.
   ```Python
   import os, requests, time
   from xml.etree import ElementTree

   class TextToSpeech(object):
       def __init__(self, input_text, voice_font):
           subscription_key = 'YOUR_SPEECH_SERVICES_SUBSCRIPTION_KEY'
           self.subscription_key = subscription_key
           self.input_text = input_text
           self.voice_font = voice_font
           self.timestr = time.strftime('%Y%m%d-%H%M')
           self.access_token = None

       # This function performs the token exchange.
       def get_token(self):
           fetch_token_url = 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken'
           headers = {
               'Ocp-Apim-Subscription-Key': self.subscription_key
           }
           response = requests.post(fetch_token_url, headers=headers)
           self.access_token = str(response.text)

       # This function calls the TTS endpoint with the access token.
       def save_audio(self):
           base_url = 'https://westus.tts.speech.microsoft.com/'
           path = 'cognitiveservices/v1'
           constructed_url = base_url + path
           headers = {
               'Authorization': 'Bearer ' + self.access_token,
               'Content-Type': 'application/ssml+xml',
               'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
               'User-Agent': 'YOUR_RESOURCE_NAME',
           }
           # Build the SSML request with ElementTree
           xml_body = ElementTree.Element('speak', version='1.0')
           xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
           voice = ElementTree.SubElement(xml_body, 'voice')
           voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
           voice.set('name', 'Microsoft Server Speech Text to Speech Voice {}'.format(self.voice_font))
           voice.text = self.input_text
           # The body must be encoded as UTF-8 to handle non-ascii characters.
           body = ElementTree.tostring(xml_body, encoding="utf-8")

           #Send the request
           response = requests.post(constructed_url, headers=headers, data=body)

           # Write the response as a wav file for playback. The file is located
           # in the same directory where this sample is run.
           return response.content
   ```
3. Konuşma Hizmetleri abonelik anahtarınızı ekleyin ve kaydedin.

### <a name="add-a-route-to-apppy"></a>Rota ekleme`app.py`

Flask uygulamanızda `synthesize.py`'. Bu rota, bir kullanıcı uygulamanızdaki metinden konuşmaya dönüştürme düğmesine her bastüğünde çağrılacaktır. Çeviri ve duyarlılık çözümlemesi için yollar gibi, işlev iki bağımsız değişken beklediğinden bu rota istekleri kabul `POST` edecek: metnin sentezlemesi ve oynatma için sesli yazı tipi.

1. İthalat `app.py` deyimini en üstte `app.py` açın ve bulun ve güncelleyin:

   ```python
   import translate, sentiment, synthesize
   ```
   Artık Flask uygulamamız üzerinden `synthesize.py`mevcut yöntemi kullanabilirsiniz.

2. Bu kodu sonuna kadar `app.py` kopyalayın ve kaydedin:

   ```Python
   @app.route('/text-to-speech', methods=['POST'])
   def text_to_speech():
       data = request.get_json()
       text_input = data['text']
       voice_font = data['voice']
       tts = synthesize.TextToSpeech(text_input, voice_font)
       tts.get_token()
       audio_response = tts.save_audio()
       return audio_response
   ```

### <a name="update-indexhtml"></a>`index.html` dosyasını güncelleştirme

Artık metinden konuşmaya dönüştürme işlevine ve Flask uygulamanızda bunu aramak için bir rotaya sahip olduğunuza göre, bir sonraki adım uygulamanız için HTML yazmaya başlamaktır. Aşağıdaki HTML birkaç şey yapar:

* Ses seçimi açılır bırakma sağlar
* Metinden konuşmaya dönüştürmek için bir düğme ekler
* Sentezlenen konuşmayı oynatmak için kullanılan bir ses öğesi ekler

1. Bu `index.html` kod yorumlarını açın ve bulun:
   ```html
   <!-- Start voice font selection code -->

   <!-- End voice font selection code -->
   ```

2. Kod yorumlarını bu HTML bloğuyla değiştirin:
   ```html
   <div class="form-group">
     <label for="select-voice"><strong>Select voice font:</strong></label>
     <select class="form-control" id="select-voice">
       <option value="(ar-SA, Naayf)">Arabic | Male | Naayf</option>
       <option value="(ca-ES, HerenaRUS)">Catalan | Female | HerenaRUS</option>
       <option value="(zh-CN, HuihuiRUS)">Chinese (Mainland) | Female | HuihuiRUS</option>
       <option value="(zh-CN, Kangkang, Apollo)">Chinese (Mainland) | Male | Kangkang, Apollo</option>
       <option value="(zh-HK, Tracy, Apollo)">Chinese (Hong Kong)| Female | Tracy, Apollo</option>
       <option value="(zh-HK, Danny, Apollo)">Chinese (Hong Kong) | Male | Danny, Apollo</option>
       <option value="(zh-TW, Yating, Apollo)">Chinese (Taiwan)| Female | Yaiting, Apollo</option>
       <option value="(zh-TW, Zhiwei, Apollo)">Chinese (Taiwan) | Male | Zhiwei, Apollo</option>
       <option value="(hr-HR, Matej)">Croatian | Male | Matej</option>
       <option value="(en-US, AriaRUS)">English (US) | Female | AriaRUS</option>
       <option value="(en-US, Guy24kRUS)">English (US) | Male | Guy24kRUS</option>
       <option value="(en-IE, Sean)">English (IE) | Male | Sean</option>
       <option value="(fr-FR, Julie, Apollo)">French | Female | Julie, Apollo</option>
       <option value="(fr-FR, HortenseRUS)">French | Female | Julie, HortenseRUS</option>
       <option value="(fr-FR, Paul, Apollo)">French | Male | Paul, Apollo</option>
       <option value="(de-DE, Hedda)">German | Female | Hedda</option>
       <option value="(de-DE, HeddaRUS)">German | Female | HeddaRUS</option>
       <option value="(de-DE, Stefan, Apollo)">German | Male | Apollo</option>
       <option value="(el-GR, Stefanos)">Greek | Male | Stefanos</option>
       <option value="(he-IL, Asaf)">Hebrew (Isreal) | Male | Asaf</option>
       <option value="(hi-IN, Kalpana, Apollo)">Hindi | Female | Kalpana, Apollo</option>
       <option value="(hi-IN, Hemant)">Hindi | Male | Hemant</option>
       <option value="(it-IT, LuciaRUS)">Italian | Female | LuciaRUS</option>
       <option value="(it-IT, Cosimo, Apollo)">Italian | Male | Cosimo, Apollo</option>
       <option value="(ja-JP, Ichiro, Apollo)">Japanese | Male | Ichiro</option>
       <option value="(ja-JP, HarukaRUS)">Japanese | Female | HarukaRUS</option>
       <option value="(ko-KR, HeamiRUS)">Korean | Female | Haemi</option>
       <option value="(pt-BR, HeloisaRUS)">Portuguese (Brazil) | Female | HeloisaRUS</option>
       <option value="(pt-BR, Daniel, Apollo)">Portuguese (Brazil) | Male | Daniel, Apollo</option>
       <option value="(pt-PT, HeliaRUS)">Portuguese (Portugal) | Female | HeliaRUS</option>
       <option value="(ru-RU, Irina, Apollo)">Russian | Female | Irina, Apollo</option>
       <option value="(ru-RU, Pavel, Apollo)">Russian | Male | Pavel, Apollo</option>
       <option value="(ru-RU, EkaterinaRUS)">Russian | Female | EkaterinaRUS</option>
       <option value="(es-ES, Laura, Apollo)">Spanish | Female | Laura, Apollo</option>
       <option value="(es-ES, HelenaRUS)">Spanish | Female | HelenaRUS</option>
       <option value="(es-ES, Pablo, Apollo)">Spanish | Male | Pablo, Apollo</option>
       <option value="(th-TH, Pattara)">Thai | Male | Pattara</option>
       <option value="(tr-TR, SedaRUS)">Turkish | Female | SedaRUS</option>
       <option value="(vi-VN, An)">Vietnamese | Male | An</option>
     </select>
   </div>
   ```

3. Ardından, şu kod yorumlarını bulun:
   ```html
   <!-- Add Speech Synthesis button and audio element -->

   <!-- End Speech Synthesis button -->
   ```

4. Kod yorumlarını bu HTML bloğuyla değiştirin:

```html
<button type="submit" class="btn btn-primary mb-2" id="text-to-speech">Convert text-to-speech</button>
<div id="audio-playback">
  <audio id="audio" controls>
    <source id="audio-source" type="audio/mpeg" />
  </audio>
</div>
```

5. Çalışmanızı kaydettiğinizden emin olun.

### <a name="update-mainjs"></a>`main.js` dosyasını güncelleştirme

Aşağıdaki kodda, HTML'den gelen içerik Flask rotanıza bir istek oluşturmak için kullanılır. Özellikle, çeviri ve ses yazı tipi değişkenlere atanır ve daha `text-to-speech` sonra istek içinde rotaya geçirilir.

Kod daha sonra yanıtı yineler ve HTML'yi duyarlılık puanlarıyla güncelleştirir.

1. IDE'nizden, `static` dizinde `main.js` bir dosya oluşturun.
2. Bu kodu `static/scripts/main.js`aşağıdakilere kopyalayın:
   ```javascript
   // Convert text-to-speech
   $("#text-to-speech").on("click", function(e) {
     e.preventDefault();
     var ttsInput = document.getElementById("translation-result").value;
     var ttsVoice = document.getElementById("select-voice").value;
     var ttsRequest = { 'text': ttsInput, 'voice': ttsVoice }

     var xhr = new XMLHttpRequest();
     xhr.open("post", "/text-to-speech", true);
     xhr.setRequestHeader("Content-Type", "application/json");
     xhr.responseType = "blob";
     xhr.onload = function(evt){
       if (xhr.status === 200) {
         audioBlob = new Blob([xhr.response], {type: "audio/mpeg"});
         audioURL = URL.createObjectURL(audioBlob);
         if (audioURL.length > 5){
           var audio = document.getElementById("audio");
           var source = document.getElementById("audio-source");
           source.src = audioURL;
           audio.load();
           audio.play();
         }else{
           console.log("An error occurred getting and playing the audio.")
         }
       }
     }
     xhr.send(JSON.stringify(ttsRequest));
   });
   // Code for automatic language selection goes here.
   ```
3. Neredeyse bitti. Yapacağınız son şey, çeviri için seçilen `main.js` dile göre otomatik olarak bir ses yazı tipi seçmek için bazı kodlar eklemektir. Bu kod bloğunu `main.js`ekleyin:
   ```javascript
   // Automatic voice font selection based on translation output.
   $('select[id="select-language"]').change(function(e) {
     if ($(this).val() == "ar"){
       document.getElementById("select-voice").value = "(ar-SA, Naayf)";
     }
     if ($(this).val() == "ca"){
       document.getElementById("select-voice").value = "(ca-ES, HerenaRUS)";
     }
     if ($(this).val() == "zh-Hans"){
       document.getElementById("select-voice").value = "(zh-HK, Tracy, Apollo)";
     }
     if ($(this).val() == "zh-Hant"){
       document.getElementById("select-voice").value = "(zh-HK, Tracy, Apollo)";
     }
     if ($(this).val() == "hr"){
       document.getElementById("select-voice").value = "(hr-HR, Matej)";
     }
     if ($(this).val() == "en"){
       document.getElementById("select-voice").value = "(en-US, Jessa24kRUS)";
     }
     if ($(this).val() == "fr"){
       document.getElementById("select-voice").value = "(fr-FR, HortenseRUS)";
     }
     if ($(this).val() == "de"){
       document.getElementById("select-voice").value = "(de-DE, HeddaRUS)";
     }
     if ($(this).val() == "el"){
       document.getElementById("select-voice").value = "(el-GR, Stefanos)";
     }
     if ($(this).val() == "he"){
       document.getElementById("select-voice").value = "(he-IL, Asaf)";
     }
     if ($(this).val() == "hi"){
       document.getElementById("select-voice").value = "(hi-IN, Kalpana, Apollo)";
     }
     if ($(this).val() == "it"){
       document.getElementById("select-voice").value = "(it-IT, LuciaRUS)";
     }
     if ($(this).val() == "ja"){
       document.getElementById("select-voice").value = "(ja-JP, HarukaRUS)";
     }
     if ($(this).val() == "ko"){
       document.getElementById("select-voice").value = "(ko-KR, HeamiRUS)";
     }
     if ($(this).val() == "pt"){
       document.getElementById("select-voice").value = "(pt-BR, HeloisaRUS)";
     }
     if ($(this).val() == "ru"){
       document.getElementById("select-voice").value = "(ru-RU, EkaterinaRUS)";
     }
     if ($(this).val() == "es"){
       document.getElementById("select-voice").value = "(es-ES, HelenaRUS)";
     }
     if ($(this).val() == "th"){
       document.getElementById("select-voice").value = "(th-TH, Pattara)";
     }
     if ($(this).val() == "tr"){
       document.getElementById("select-voice").value = "(tr-TR, SedaRUS)";
     }
     if ($(this).val() == "vi"){
       document.getElementById("select-voice").value = "(vi-VN, An)";
     }
   });
   ```

### <a name="test-your-app"></a>Uygulamanızı test etme

Uygulamada konuşma sentezini test edelim.

```
flask run
```

Sağlanan sunucu adresine gidin. Giriş alanına metin yazın, bir dil seçin ve çevir'e basın. Bir çeviri almalısın. Ardından, bir ses seçin ve ardından metinden konuşmaya dönüştür düğmesine basın. çeviri sentezlenmiş konuşma olarak oynatılmalıdır. İşe yaramazsa, abonelik anahtarınızı eklediğinizden emin olun.

> [!TIP]
> Yaptığınız değişiklikler görünmüyorsa veya uygulama beklediğiniz gibi çalışmıyorsa, önbelleğinizi temizlemeyi veya özel/gizli bir pencere açmayı deneyin.

İşte bu, çeviriler yapan, duyguları analiz eden ve konuşmaları sentezleyen çalışan bir uygulamanız var. Uygulamayı öldürmek için **CTRL + c** tuşuna basın. Diğer [Azure Bilişsel Hizmetleri'ne](https://docs.microsoft.com/azure/cognitive-services/)göz atanın.

## <a name="get-the-source-code"></a>Kaynak kodunu alma

Bu projenin kaynak kodu [GitHub'da](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Flask-App-Tutorial)kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Translator Metin Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
* [Metin Analizi API'si başvurusu](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)
* [Metin okuma API başvurusu](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech)
