---
title: Linux'ta Azure Hizmet Kumaşı uygulamalarını hata ayıklama
description: Yerel bir Linux geliştirme makinesinde Service Fabric hizmetlerinizi nasıl izleyeceğinizi ve tanılamayı öğrenin.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: d8b5ec2f2190586f5eced5eee112b190a82504c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75526303"
---
# <a name="monitor-and-diagnose-services-in-a-local-linux-machine-development-setup"></a>Yerel bir Linux makine geliştirme kurulumunda hizmetleri izleme ve tanılama


> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)
> * [Linux](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally-linux.md)
>
>

İzleme, algılama, tanılama ve sorun giderme, hizmetlerin kullanıcı deneyiminde en az kesintiye yol açmayla devam etmesine olanak sağlar. Gerçek bir dağıtım ortamında izleme ve tanılama çok önemlidir. Hizmetlerin geliştirilmesi sırasında benzer bir modelin benimsenmesi, üretim ortamına geçdiğinizde tanılama ardışık hattının çalışmasını sağlar. Service Fabric, hizmet geliştiricilerin hem tek makineli yerel geliştirme kurulumları hem de gerçek dünya üretim küme kurulumlarında sorunsuz bir şekilde çalışabilen tanılamaları uygulamalarını kolaylaştırır.


## <a name="debugging-service-fabric-java-applications"></a>Hata Ayıklama Hizmeti Kumaş Java uygulamaları

Java uygulamaları için [birden çok günlük çerçevesi](https://en.wikipedia.org/wiki/Java_logging_framework) mevcuttur. JRE ile varsayılan seçenek `java.util.logging` olduğundan, [github kod örnekleri](https://github.com/Azure-Samples/service-fabric-java-getting-started)için de kullanılır. Aşağıdaki tartışma, çerçevenin nasıl `java.util.logging` yapılandırılabildiğini açıklar.

java.util.log'u kullanarak uygulama günlüklerinizi belleğe, çıkış akışlarına, konsol dosyalarına veya soketlere yönlendirebilirsiniz. Bu seçeneklerin her biri için, çerçevede zaten sağlanan varsayılan işleyicileri vardır. Tüm günlükleri `app.properties` yerel bir dosyaya yönlendirmek için uygulamanız için dosya işleyicisini yapılandırmak için bir dosya oluşturabilirsiniz.

Aşağıdaki kod snippet örnek bir yapılandırma içerir:

```java
handlers = java.util.logging.FileHandler

java.util.logging.FileHandler.level = ALL
java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
java.util.logging.FileHandler.limit = 1024000
java.util.logging.FileHandler.count = 10
java.util.logging.FileHandler.pattern = /tmp/servicefabric/logs/mysfapp%u.%g.log
```

Dosyatarafından `app.properties` işaret edilen klasör var olmalıdır. `app.properties` Dosya oluşturulduktan `entrypoint.sh` sonra, özelliği `<applicationfolder>/<servicePkg>/Code/` `java.util.logging.config.file` `app.properties` dosyaya ayarlamak için klasördeki giriş noktası komut dosyanızı da değiştirmeniz gerekir. Giriş aşağıdaki parçacık gibi görünmelidir:

```sh
java -Djava.library.path=$LD_LIBRARY_PATH -Djava.util.logging.config.file=<path to app.properties> -jar <service name>.jar
```


Bu yapılandırma, `/tmp/servicefabric/logs/`'de dönen bir şekilde toplanan günlüklerin sonucu Bu durumda günlük dosyası mysfapp%u.%g.log olarak adlandırılır:
* **%u,** eşzamanlı Java işlemleri arasındaki çakışmaları gidermek için benzersiz bir sayıdır.
* **%g,** dönen günlükleri ayırt etmek için nesil numarasıdır.

Varsayılan olarak hiçbir işleyici açıkça yapılandırılmış ise, konsol işleyicisi kaydedilir. /var/log/syslog altında syslog günlükleri görüntüleyebilirsiniz.

Daha fazla bilgi için [GitHub'daki kod örneklerine](https://github.com/Azure-Samples/service-fabric-java-getting-started)bakın.


## <a name="debugging-service-fabric-c-applications"></a>Hata Ayıklama Servisi Kumaş C# uygulamaları


Linux'taki CoreCLR uygulamalarının izlenmesi için birden fazla çerçeve mevcuttur. Daha fazla bilgi için [GitHub: günlüğe kaydetme.](http:/github.com/aspnet/logging)  EventSource C# geliştiricileri için tanıdık olduğundan, 'Bu makalede Linux CoreCLR örneklerinde izleme için EventSource kullanır.

İlk adım, günlüklerinizi belleğe, çıktı akışlarına veya konsol dosyalarına yazabilmeniz için System.Diagnostics.Tracing'i eklemektir.  EventSource'u kullanmak için project.json'unuza aşağıdaki projeyi ekleyin:

```json
    "System.Diagnostics.StackTrace": "4.0.1"
```

Hizmet olayını dinlemek ve ardından bunları uygun şekilde izleme dosyalarına yönlendirmek için özel bir EventListener kullanabilirsiniz. Aşağıdaki kod snippet EventSource ve özel bir EventListener kullanarak günlük bir örnek uygulama gösterir:


```csharp

public class ServiceEventSource : EventSource
{
        public static ServiceEventSource Current = new ServiceEventSource();

        [NonEvent]
        public void Message(string message, params object[] args)
        {
            if (this.IsEnabled())
            {
                var finalMessage = string.Format(message, args);
                this.Message(finalMessage);
            }
        }

        // TBD: Need to add method for sample event.

}

```


```csharp
internal class ServiceEventListener : EventListener
{

        protected override void OnEventSourceCreated(EventSource eventSource)
        {
            EnableEvents(eventSource, EventLevel.LogAlways, EventKeywords.All);
        }
        protected override void OnEventWritten(EventWrittenEventArgs eventData)
        {
                using (StreamWriter Out = new StreamWriter( new FileStream("/tmp/MyServiceLog.txt", FileMode.Append)))
                {
                        // report all event information
                        Out.Write(" {0} ", Write(eventData.Task.ToString(), eventData.EventName, eventData.EventId.ToString(), eventData.Level,""));
                        if (eventData.Message != null)
                                Out.WriteLine(eventData.Message, eventData.Payload.ToArray());
                        else
                        {
                                string[] sargs = eventData.Payload != null ? eventData.Payload.Select(o => o.ToString()).ToArray() : null; 
                                Out.WriteLine("({0}).", sargs != null ? string.Join(", ", sargs) : "");
                        }
                }
        }
}
```


Önceki snippet' teki bir dosyaya `/tmp/MyServiceLog.txt`günlükleri çıkar Bu dosya adının uygun şekilde güncelleştirilmesi gerekir. Günlükleri konsola yönlendirmek isterseniz, özelleştirilmiş EventListener sınıfınızda aşağıdaki snippet'i kullanın:

```csharp
public static TextWriter Out = Console.Out;
```

[C# Örneklerindeki](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started) örnekler, olayları bir dosyaya günlüğe kaydetmek için EventSource ve özel bir EventListener kullanır.



## <a name="next-steps"></a>Sonraki adımlar
Uygulamanıza eklenen aynı izleme kodu, uygulamanızın bir Azure kümesindeki tanılamalarıyla da çalışır. Araçlar için farklı seçenekleri tartışan ve bunları nasıl ayarlayacaklarını açıklayan bu makalelere göz atın.
* [Azure Tanılama ile günlükleri nasıl toplanır?](service-fabric-diagnostics-how-to-setup-lad.md)
