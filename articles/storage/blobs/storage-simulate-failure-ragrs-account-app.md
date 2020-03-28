---
title: Öğretici - Birincil bölgeden veri okuma hatası simüle
titleSuffix: Azure Storage
description: Depolama hesabı için okuma-erişim coğrafi depolama (RA-GRS) etkinleştirildiğinde birincil bölgeden gelen verileri okuma hatası simüle edin.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: artek
ms.openlocfilehash: 3693ae8c9c425c35ab1a4ebf88b6cd3dfb58f92c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80061283"
---
# <a name="tutorial-simulate-a-failure-in-reading-data-from-the-primary-region"></a>Öğretici: Birincil bölgeden veri okuma da bir hata simüle

Bu öğretici, bir dizinin ikinci bölümüdür. Bu yazıda, bir hata simüle ederek [okuma-erişim coğrafi depolamanın](../common/storage-redundancy.md) (RA-GRS) yararları hakkında bilgi edinebilirsiniz.

Bir hata simüle etmek için Statik [Yönlendirme](#simulate-a-failure-with-an-invalid-static-route) veya [Fiddler](#simulate-a-failure-with-fiddler)kullanabilirsiniz. Her iki yöntem de [okuma-erişim coğrafi](../common/storage-redundancy.md) (RA-GRS) depolama hesabınızın birincil bitiş noktasına gelen istekler için hata simülasyonu yapmanızı sağlayarak uygulamanın ikincil uç noktadan okunmasına neden olur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

Serinin ikinci bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Uygulamayı çalıştırma ve duraklatma
> * Geçersiz statik [rota](#simulate-a-failure-with-an-invalid-static-route) veya [Fiddler](#simulate-a-failure-with-fiddler) ile bir hata benzetimi
> * Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce, önceki öğreticiyi tamamlayın: [Uygulama verilerinizi Azure depolama ile yüksek oranda kullanılabilir hale getirin.][previous-tutorial]

Statik yönlendirme ile bir hata simüle etmek için, yükseltilmiş bir komut istemi kullanır.

Fiddler kullanarak bir hata simüle etmek için, [fiddler](https://www.telerik.com/download/fiddler) indirin ve yükleyin

## <a name="simulate-a-failure-with-an-invalid-static-route"></a>Geçersiz bir statik rota ile hata benzetimi yapma

[Okuma erişimli coğrafi olarak yedekli](../common/storage-redundancy.md) (RA-GRS) depolama hesabınızın birincil uç noktasına yönelik tüm istekler için geçersiz bir statik rota oluşturabilirsiniz. Bu öğreticide, isteklerin depolama hesabına yönlendirilmesi için ağ geçidi olarak yerel ana bilgisayar kullanılır. Yerel ana bilgisayarın ağ geçidi olarak kullanılması, depolama hesabınızın birincil uç noktasına yönelik tüm isteklerin ana bilgisayara dönecek şekilde bir döngüye girmesine ve sonuçta başarısız olmasına yol açar. Geçersiz bir statik rota ile bir hata ve birincil uç noktayı geri yükleme benzetimi yapmak için aşağıdaki adımları izleyin.

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

Örneği başlatmak ve test dosyasını indirmek için [önceki öğreticideki][previous-tutorial] yönergeleri kullanarak birincil depolama alanından geldiğini onaylayabilirsiniz. Hedef platformunuza bağlı olarak, örneği el ile duraklatabilir veya bir komut isteminde bekleyebilirsiniz.

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış olsa da, Windows'da yönetici olarak bir komut istemi açın veya Linux'ta kök olarak terminal çalıştırın.

Depolama hesabınızın adını değiştirerek, `STORAGEACCOUNTNAME` bir komut istemi veya terminalüzerinde aşağıdaki komutu girerek depolama hesabı birincil bitiş noktası etki alanı hakkında bilgi alın.

```
nslookup STORAGEACCOUNTNAME.blob.core.windows.net
```

Depolama hesabınızın IP adresini daha sonra kullanmak üzere bir metin düzenleyicisine kopyalayın.

Yerel ana bilgisayarın IP adresini almak için Windows komut isteminde `ipconfig` veya Linux terminalinde `ifconfig` yazın.

Hedef ana bilgisayar için statik bir rota eklemek için, depolama hesabı IP `<destination_ip>` adresinizi ve `<gateway_ip>` yerel ana bilgisayar IP adresinizi değiştirerek windows komut istemi veya Linux terminaline aşağıdaki komutu yazın.

#### <a name="linux"></a>Linux

```
route add <destination_ip> gw <gateway_ip>
```

#### <a name="windows"></a>Windows

```
route add <destination_ip> <gateway_ip>
```

Çalışan örnekle birlikte pencerede, uygulamayı devam ettirin veya örnek dosyayı indirmek ve ikincil depolama alanından geldiğini doğrulamak için uygun tuşa basın. Ardından örneği yeniden duraklatabilir veya komut isteminde bekleyebilirsiniz.

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Birincil bitiş noktasının yeniden işlevsel hale gelmesini simüle etmek için, geçersiz statik rotayı yönlendirme tablosundan silin. Bu, birincil uç noktaya yönelik tüm isteklerin varsayılan ağ geçidi üzerinden yönlendirilmesini sağlar. Windows komut istemi veya Linux terminalinde aşağıdaki komutu yazın.

#### <a name="linux"></a>Linux

```
route del <destination_ip> gw <gateway_ip>
```

#### <a name="windows"></a>Windows

```
route delete <destination_ip>
```

Daha sonra uygulamaya devam edebilir veya örnek dosyayı yeniden indirmek için uygun tuşa basabilirsiniz, bu kez bir kez daha birincil depolama geliyor onaylayan.

## <a name="simulate-a-failure-with-fiddler"></a>Fiddler ile hata benzetimi yapma

Fiddler ile başarısızlığı simüle etmek için, RA-GRS depolama hesabınızın birincil bitiş noktasına istekler için başarısız bir yanıt enjekte emzmiş.

Aşağıdaki bölümlerde, bir hata ve birincil uç nokta geri yüklemesinin kemancıyla nasıl simüle edilene gösterilmiştir.

### <a name="launch-fiddler"></a>Fiddler'ı açma

Fiddler’ı açıp **Kurallar**’ı ve **Kuralları Özelleştir**’i seçin.

![Fiddler kurallarını özelleştirme](media/storage-simulate-failure-ragrs-account-app/figure1.png)

Fiddler **ScriptEditor, SampleRules.js** dosyasını başlatır ve görüntüler. Bu dosya, Fiddler’ı özelleştirmek için kullanılır.

`OnBeforeResponse` Depolama hesabınızın adını değiştirerek `STORAGEACCOUNTNAME` işlevde aşağıdaki kod örneğini yapıştırın. Örneğe bağlı olarak, indirilmekte `HelloWorld` olan test dosyasının adı (veya `sampleFile`önek) ile değiştirmeniz de gerekebilir. Yeni kod, hemen çalışmadığından emin olmak için yorumlanır.

Tamamlandıktan sonra, değişikliklerinizi kaydetmek için **Dosya** ve **Kaydet'i** seçin. Aşağıdaki adımlarda kullanılmak üzere ScriptEditor penceresini açık bırakın.

```javascript
    /*
        // Simulate data center failure
        // After it is successfully downloading the blob, pause the code in the sample,
        // uncomment these lines of script, and save the script.
        // It will intercept the (probably successful) responses and send back a 503 error.
        // When you're ready to stop sending back errors, comment these lines of script out again
        //     and save the changes.

        if ((oSession.hostname == "STORAGEACCOUNTNAME.blob.core.windows.net")
            && (oSession.PathAndQuery.Contains("HelloWorld"))) {
            oSession.responseCode = 503;
        }
    */
```

![Özelleştirilmiş kuralı yapıştırma](media/storage-simulate-failure-ragrs-account-app/figure2.png)

### <a name="start-and-pause-the-application"></a>Uygulamayı başlatma ve duraklatma

Örneği başlatmak ve test dosyasını indirmek için [önceki öğreticideki][previous-tutorial] yönergeleri kullanarak birincil depolama alanından geldiğini onaylayabilirsiniz. Hedef platformunuza bağlı olarak, örneği el ile duraklatabilir veya bir komut isteminde bekleyebilirsiniz.

### <a name="simulate-failure"></a>Hata benzetimi yapma

Uygulama duraklatılmış olsa da, Fiddler'a geri dön ve `OnBeforeResponse` işlevde kaydettiğiniz özel kuralı açıklamamaya devam edin. Kuralın etkili olması için değişikliklerinizi kaydetmek için **Dosya** ve **Kaydet'i** seçtiğinizden emin olun. Bu kod, RA-GRS depolama hesabına gelen istekleri arar ve yol örnek dosyanın adını `503 - Service Unavailable`içeriyorsa, .'ın yanıt kodunu döndürür.

Çalışan örnekle birlikte pencerede, uygulamayı devam ettirin veya örnek dosyayı indirmek ve ikincil depolama alanından geldiğini doğrulamak için uygun tuşa basın. Ardından örneği yeniden duraklatabilir veya komut isteminde bekleyebilirsiniz.

### <a name="simulate-primary-endpoint-restoration"></a>Birincil uç noktayı geri yükleme benzetimi gerçekleştirme

Fiddler'da, özel kuralı yeniden kaldırın veya yorum yapın. Kuralın artık geçerli olmayacağından emin olmak için **Dosya** ve **Kaydet'i** seçin.

Çalışan örnekle birlikte pencerede, uygulamayı devam ettirin veya örnek dosyayı indirmek ve birincil depolama alanından bir kez daha geldiğini doğrulamak için uygun tuşa basın. Daha sonra örnekten çıkabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Serinin ikinci bölümünde, okuma erişim coğrafi yedekli depolama test etmek için bir başarısızlık simüle hakkında öğrendim.

RA-GRS depolamanın nasıl çalıştığı ve ilişkili riskleri hakkında daha fazla bilgi edinmek için aşağıdaki makaleyi okuyun:

> [!div class="nextstepaction"]
> [RA-GRS ile HA uygulamaları tasarlama](../common/storage-designing-ha-apps-with-ragrs.md)

[previous-tutorial]: storage-create-geo-redundant-storage.md
