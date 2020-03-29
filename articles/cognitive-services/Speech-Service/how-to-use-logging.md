---
title: Konuşma SDK günlüğü - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'da (C++, C#, Python, Objective-C, Java) oturum açmayı etkinleştirme hakkında bilgi edinin.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: amishu
ms.openlocfilehash: 707a0f801a739a7a91cee19635e609305cd8f021
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74805799"
---
# <a name="enable-logging-in-the-speech-sdk"></a>Konuşma SDK'da oturum açmayı etkinleştirme

Dosyaya günlüğe kaydetme, Konuşma SDK için isteğe bağlı bir özelliktir. Geliştirme günlüğü sırasında Konuşma SDK'nın temel bileşenlerinden ek bilgi ve tanılama sağlar. Bir konuşma yapılandırma nesnesi üzerindeki özelliği `Speech_LogFilename` günlük dosyasının konumuna ve adına ayarlayarak etkinleştirilebilir. Bu yapılandırmadan bir tanıyıcı oluşturulduğunda ve daha sonra devre dışı bırakılamadıktan sonra günlüğe kaydetme genel olarak etkinleştirilir. Çalışan günlük oturumu sırasında günlük dosyasının adını değiştiremezsiniz.

> [!NOTE]
> Giriş, JavaScript hariç, desteklenen tüm Konuşma SDK programlama dillerinde Konuşma SDK sürümü 1.4.0'dan beri kullanılabilir.

## <a name="sample"></a>Örnek

Günlük dosyası adı bir yapılandırma nesnesi üzerinde belirtilir. Örnek `SpeechConfig` olarak almak ve bir örnek oluşturduğunuzu varsayarak: `config`

```csharp
config.SetProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```java
config.setProperty(PropertyId.Speech_LogFilename, "LogfilePathAndName");
```

```C++
config->SetProperty(PropertyId::Speech_LogFilename, "LogfilePathAndName");
```

```Python
config.set_property(speechsdk.PropertyId.Speech_LogFilename, "LogfilePathAndName")
```

```objc
[config setPropertyTo:@"LogfilePathAndName" byId:SPXSpeechLogFilename];
```

Config nesnesinden bir tanıyıcı oluşturabilirsiniz. Bu, tüm tanıyanlar için günlüğe kaydetmeyi sağlar.

> [!NOTE]
> Config nesnesinden bir oluşturmazsanız, `SpeechSynthesizer` günlüğe kaydetmeyi etkinleştirmez. Ancak günlüğe kaydetme etkinse, `SpeechSynthesizer`'den tanılama da alırsınız.

## <a name="create-a-log-file-on-different-platforms"></a>Farklı platformlarda günlük dosyası oluşturma

Windows veya Linux için, günlük dosyası kullanıcının için yazma izni olan herhangi bir yolda olabilir. Diğer işletim sistemlerinde dosya sistemi konumları için yazma izinleri varsayılan olarak sınırlı veya sınırlı olabilir.

### <a name="universal-windows-platform-uwp"></a>Evrensel Windows Platformu (UWP)

UWP uygulamalarının günlük dosyalarını uygulama veri konumlarından birine (yerel, dolaşım veya geçici) yerle bir etmesi gerekir. Yerel uygulama klasöründe bir günlük dosyası oluşturulabilir:

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile logFile = await storageFolder.CreateFileAsync("logfile.txt", CreationCollisionOption.ReplaceExisting);
config.SetProperty(PropertyId.Speech_LogFilename, logFile.Path);
```

UWP uygulamaları için dosya erişim izni hakkında daha fazla bilgiye [buradan](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)ulaşabilirsiniz.

### <a name="android"></a>Android

Günlük dosyasını dahili depolama alanına, dış depolama alanına veya önbellek dizinine kaydedebilirsiniz. İç depolama alanında veya önbellek dizininde oluşturulan dosyalar uygulamaya özeldir. Harici depolama alanında bir günlük dosyası oluşturmak tercih edilir.

```java
File dir = context.getExternalFilesDir(null);
File logFile = new File(dir, "logfile.txt");
config.setProperty(PropertyId.Speech_LogFilename, logFile.getAbsolutePath());
```

Yukarıdaki kod, bir günlük dosyasını uygulamaya özgü bir dizinin kökündeki dış depolama alanına kaydeder. Bir kullanıcı dosya yöneticisi (genellikle) `Android/data/ApplicationName/logfile.txt`ile dosyaerişebilirsiniz. Uygulama kaldırıldığında dosya silinir.

Ayrıca, bildirim `WRITE_EXTERNAL_STORAGE` dosyasında izin istemeniz gerekir:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" package="...">
  ...
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  ...
</manifest>
```

Android uygulamaları için veri ve dosya depolama hakkında daha fazla bilgi [burada](https://developer.android.com/guide/topics/data/data-storage.html)bulabilirsiniz.

#### <a name="ios"></a>iOS

Yalnızca uygulama ve dizinlerine erişilebilir. Dosyalar belgelerde, kitaplıkta ve geçici dizinlerde oluşturulabilir. Belge dizinindeki dosyalar bir kullanıcıtarafından kullanılabilir hale getirilebilir. Aşağıdaki kod snippet uygulama belgesi dizini bir günlük dosyası oluşturma gösterir:

```objc
NSString *filePath = [
  [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject]
    stringByAppendingPathComponent:@"logfile.txt"];
[speechConfig setPropertyTo:filePath byId:SPXSpeechLogFilename];
```

Oluşturulan bir dosyaya erişmek için aşağıdaki `Info.plist` özellikleri uygulamanın özellik listesine ekleyin:

```xml
<key>UIFileSharingEnabled</key>
<true/>
<key>LSSupportsOpeningDocumentsInPlace</key>
<true/>
```

iOS Dosya Sistemi hakkında daha fazla hizmeti [burada](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneklerimizi GitHub'da keşfedin](https://aka.ms/csspeech/samples)
