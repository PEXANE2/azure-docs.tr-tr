---
title: Grafik performansı sorunlarını tanıla Uzak Masaüstü - Azure
description: Bu makalede, Windows Sanal Masaüstü'nde grafik ile performans sorunlarını tanılamak için uzak masaüstü protokolü oturumlarında RemoteFX grafik sayaçları nasıl kullanılacağı açıklanmaktadır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 05/23/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 84cee86dbddff77f6142925eec01889cf793a466
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127560"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Uzak Masaüstünde grafik performansı sorunlarını tanılama

Uzak oturumlarınızla ilgili kalite sorunlarını tanılamak için, Performans Monitörü'nün RemoteFX Graphics bölümü altında sayaçlar sağlanmıştır. Bu makale, uzak masaüstü protokolü (RDP) oturumları sırasında bu sayaçları kullanarak grafikle ilgili performans sorunlarını saptamanıza ve düzeltmenize yardımcı olur.

## <a name="find-your-remote-session-name"></a>Uzak oturum adınızı bulma

Grafik performans sayaçlarını tanımlamak için uzak oturum adınızı n gerekir. Her sayaç örneğini belirlemek için bu bölümdeki yönergeleri izleyin.

1. Uzak oturumunuzdan Windows komut istemini açın.
2. **Qwinsta** komutunu çalıştırın ve oturum adınızı bulun.
    - Oturumunuz çok oturumlu bir sanal makinede (VM) barındırılırsa: Her sayaç örneğiniz, oturum adınızı "rdp-tcp 37" gibi soneklerle aynı sayıda sonedir.
    - Oturumunuz sanal Grafik İşleme Birimlerini (vGPU) destekleyen bir VM'de barındırılırsa: Her sayaç örneğiniz VM'nizde değil, sunucuda depolanır. Karşı örnekleriniz, oturum adındaki "Win8 Enterprise VM" gibi numara yerine VM adını içerir.

>[!NOTE]
> Sayaçların adlarında RemoteFX olsa da, vGPU senaryolarında uzak masaüstü grafikleri de bulunur.

## <a name="access-performance-counters"></a>Performans sayaçlarına erişin

Uzak oturum adınızı belirledikten sonra, uzak oturumunuz için RemoteFX Grafik performans sayaçlarını toplamak için aşağıdaki yönergeleri izleyin.

1. **Yönetim Araçları** > **Performans Monitörünü** **Başlat'ı** > seçin.
2. Performans **İzleyicisi** iletişim kutusunda, **İzleme Araçlarını**genişletin, **Performans İzleyicisi'ni**seçin ve sonra **Ekle'yi**seçin.
3. Kullanılabilir **Sayaçlar** listesinden Sayaç Ekle **Available Counters** iletişim kutusunda, RemoteFX Graphics bölümünü genişletin.
4. İzlenecek sayaçları seçin.
5. **Seçili nesne listesiörneklerinde,** seçili sayaçlar için izlenecek belirli örnekleri seçin ve sonra **Ekle'yi**seçin. Kullanılabilir tüm sayaç örneklerini seçmek için **Tüm örnekleri**seçin.
6. Sayaçları ekledikten sonra **Tamam'ı**seçin.

Seçili performans sayaçları Performans İzleyicisi ekranında görünür.

>[!NOTE]
>Bir ana bilgisayardaki her etkin oturumun, her performans sayacının kendi örneği vardır.

## <a name="diagnose-issues"></a>Sorunları tanılama

Grafiklerle ilgili performans sorunları genellikle dört kategoriye ayrılır:

- Düşük kare hızı
- Rastgele tezgahlar
- Yüksek giriş gecikmesi
- Düşük kare kalitesi

### <a name="addressing-low-frame-rate-random-stalls-and-high-input-latency"></a>Düşük kare hızı, rasgele durakları ve yüksek giriş gecikmesi adresleme

Önce Çıkış Çerçeveleri/İkinci sayacını kontrol edin. İstemci için kullanılabilir hale getirilen çerçeve sayısını ölçer. Bu değer Giriş Çerçeveleri/İkinci sayacından daha azsa, çerçeveler atlanır. Darboğaz tanımlamak için Atlanan/İkinci sayaçları kullanın.

Atlanan/İkinci sayaçların üç türü vardır:

- Atlanan/İkinci Çerçeveler (Yetersiz Sunucu Kaynakları)
- Atlanan/İkinci Çerçeveler (Yetersiz Ağ Kaynakları)
- Atlanan Çerçeveler/İkinci (Yetersiz İstemci Kaynakları)

Atlanan/İkinci sayaçlardan herhangi biri için yüksek bir değer, sorunun sayaç izinden giden kaynakla ilişkili olduğu anlamına gelir. Örneğin, istemci çerçeveleri çözmüyor ve çerçeveleri sunucunun çerçeveleri sağladığı hızda sunmuyorsa, Atlanan Çerçeveler/İkinci (Yetersiz İstemci Kaynakları) sayacı yüksek olacaktır.

Çıktı Çerçeveleri/İkinci sayacı Giriş Çerçeveleri/İkinci sayacıyla eşleşirse, yine de olağandışı gecikme veya duraklama fark ediyorsanız, Ortalama Kodlama Süresi suçlu olabilir. Kodlama, sunucuda tek oturumlu (vGPU) senaryosunda ve çok oturumlu senaryoda VM'de oluşan eşzamanlı bir işlemdir. Ortalama Kodlama Süresi 33 ms'nin altında olmalıdır. Ortalama Kodlama Süresi 33 ms'in altındaysa ancak yine de performans sorunlarınız varsa, kullanmakta olduğunuz uygulama veya işletim sistemiyle ilgili bir sorun olabilir.

Uygulamayla ilgili sorunları tanılama hakkında daha fazla bilgi için [Bkz. Kullanıcı Girişi Gecikmesi performans sayaçları.](/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters/)

RDP 33 ms'lik Ortalama Kodlama Süresini desteklediğinden, 30 kare/saniye'ye kadar bir giriş çerçevesi hızını destekler. 33 ms'in desteklenen maksimum kare hızı olduğunu unutmayın. Çoğu durumda, bir çerçevenin kaynak tarafından RDP'ye ne sıklıkta verildiğine bağlı olarak, kullanıcı tarafından karşılaşılan kare hızı daha düşük olacaktır. Örneğin, video izleme gibi görevler 30 kare/saniye tam giriş kare hızı gerektirir, ancak belgeyi seyrek olarak düzenleme gibi daha az hesaplama lı yoğun görevler, kullanıcının deneyim kalitesinde bozulma olmadan Giriş Çerçeveleri/Saniye için çok daha düşük bir değer sağlar.

### <a name="addressing-poor-frame-quality"></a>Düşük çerçeve kalitesini ele alma

Çerçeve kalitesi sorunlarını tanılamak için Çerçeve Kalitesi sayacını kullanın. Bu sayaç, çıktı çerçevesinin kalitesini kaynak çerçevenin kalitesinin bir yüzdesi olarak ifade eder. Kalite kaybı RemoteFX'ten kaynaklanıyor olabilir veya grafik kaynağına bağlı olabilir. RemoteFX kalite kaybına neden olduysa, sorun daha yüksek doğrulukiçeriği göndermek için ağ veya sunucu kaynaklarının eksikliği olabilir.

## <a name="mitigation"></a>Risk azaltma

Sunucu kaynakları darboğaza neden oluyorsa, performansı artırmak için aşağıdaki yaklaşımlardan birini deneyin:

- Ana bilgisayar başına oturum sayısını azaltın.
- Sunucudaki bellek ve bilgi işlem kaynaklarını artırın.
- Bağlantının çözünürlüğünü bırakın.

Ağ kaynakları darboğaza neden oluyorsa, oturum başına ağ kullanılabilirliğini artırmak için aşağıdaki yaklaşımlardan birini deneyin:

- Ana bilgisayar başına oturum sayısını azaltın.
- Daha yüksek bant genişliğine sahiptir.
- Bağlantının çözünürlüğünü bırakın.

İstemci kaynakları darboğaza neden oluyorsa, performansı artırmak için aşağıdaki yaklaşımlardan birini deneyin:

- En son Uzak Masaüstü istemcisini yükleyin.
- İstemci makinesindeki bellek ve bilgi işlem kaynaklarını artırın.

> [!NOTE]
> Şu anda Kaynak Çerçeveler/İkinci sayacı destekliyoruz. Şimdilik, Kaynak Çerçeveler/İkinci sayaç her zaman 0 görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

- GPU için optimize edilmiş bir Azure sanal makinesi oluşturmak [için, Windows Sanal Masaüstü ortamı için grafik işleme birimini (GPU) yapılandırın.](configure-vm-gpu.md)
- Sorun giderme ve yükseltme parçalarına genel bakış [için, Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Hizmet hakkında daha fazla bilgi edinmek için [Windows Desktop ortamına](environment-setup.md)bakın.
