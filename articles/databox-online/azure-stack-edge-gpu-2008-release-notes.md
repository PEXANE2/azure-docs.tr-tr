---
title: Azure Stack Edge Preview sürüm notları | Microsoft Docs
description: Genel kullanılabilirlik sürümü çalıştıran Azure Data Box Gateway için kritik açık sorunları ve çözümleri açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 09/04/2020
ms.author: alkohli
ms.openlocfilehash: 66cfda5ed83e52882dffe0d7507015f2405785c1
ms.sourcegitcommit: 206629373b7c2246e909297d69f4fe3728446af5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2020
ms.locfileid: "89500224"
---
# <a name="azure-stack-edge-with-gpu-preview-release-notes"></a>GPU önizlemesi sürüm notları ile Azure Stack Edge

Aşağıdaki sürüm notları, GPU ile Azure Stack Edge cihazlarınız için kritik açık sorunları ve 2008 önizleme sürümü için çözülmüş sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. Azure Stack Edge cihazınızı dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

Bu makale aşağıdaki yazılım sürümü **Azure Stack Edge 2008**için geçerlidir. 

<!--- **2.1.1328.1904**-->

## <a name="whats-new"></a>Yenilikler

Azure Stack Edge 2008 sürümünde aşağıdaki yeni özellikler eklenmiştir. Çalıştırdığınız belirli önizleme yazılımı sürümüne bağlı olarak, bu özelliklerin bir alt kümesini görebilirsiniz. 

- **Depolama sınıfları** -önceki sürümde, Azure Stack Edge cihazınızda çalışan Kubernetes kümesinde dağıtılan ve durum bilgisi olan uygulamalar IÇIN yalnızca SMB veya NFS paylaşımları aracılığıyla depolamayı statik olarak sağlayabilirsiniz. Bu sürümde Depolama sınıfları, depolama alanı dinamik olarak sağlamaya olanak sağlayacak şekilde eklenmiştir. Daha fazla bilgi için [Azure Stack Edge GPU cihazınızda Kubernetes depolama yönetimi](azure-stack-edge-gpu-kubernetes-storage.md#dynamicprovisioning)bölümüne bakın. 
- **Ölçüm sunucusu Ile Kubernetes panosu** -bu sürümde, bir ölçüm sunucusu eklentisi Ile bir Kubernetes panosu eklenir. Azure Stack Edge cihazınızda çalışan uygulamalara genel bir bakış almak, Kubernetes küme kaynaklarının durumunu görüntülemek ve cihazda oluşan tüm hataları görmek için panoyu kullanabilirsiniz. Ölçüm sunucusu, cihazdaki Kubernetes kaynakları genelinde CPU ve bellek kullanımını toplar. Daha fazla bilgi için bkz. [Azure Stack Edge GPU cihazınızı izlemek Için Kubernetes panosunu kullanma](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md).
- **Azure Stack Edge Için Azure Arc** -Bu sürümden başlayarak, Azure arc aracılığıyla Azure Stack Edge cihazınıza uygulama iş yüklerini dağıtabilirsiniz. Azure Arc, Kubernetes kümelerinizde uygulamalar dağıtmanıza olanak sağlayan bir karma yönetim aracıdır. Daha fazla bilgi için, [Azure Stack Edge cihazınızda Azure Arc aracılığıyla iş yüklerini dağıtma](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md)konusuna bakın.  

## <a name="known-issues"></a>Bilinen sorunlar 

Aşağıdaki tabloda Azure Stack Edge cihazına yönelik bilinen sorunların bir özeti verilmiştir.

| Hayır. | Öne çıkan özelliği | Sorun | Geçici çözüm/açıklamalar |
| --- | --- | --- | --- |
| **1.** |Azure Stack Edge + Azure SQL | SQL veritabanı oluşturma, yönetici erişimi gerektirir.   |İçindeki adımlar 1-2 yerine aşağıdaki adımları uygulayın [https://docs.microsoft.com/azure/iot-edge/tutorial-store-data-sql-server#create-the-sql-database](https://docs.microsoft.com/azure/iot-edge/tutorial-store-data-sql-server#create-the-sql-database) . <ul><li>Cihazınızın yerel kullanıcı arabiriminde, işlem arabirimini etkinleştirin. İşlem **> Için işlem > bağlantı noktası # > etkinleştir** ' i seçin.</li><li>`sqlcmd`İstemci makinenize şuradan indirinhttps://docs.microsoft.com/sql/tools/sqlcmd-utility </li><li>Adresin sonuna bir ", 1401" ekleyerek işlem arabirimi IP adresine (etkin olan bağlantı noktası) bağlanın.</li><li>Son komut şöyle görünür: sqlcmd-S {Interface IP}, 1401-U SA-P "Strong! Passw0rd".</li>Bundan sonra geçerli belgelerden 3-4 arasındaki adımlar aynı olmalıdır. </li></ul> |
| **2.** |Yenile| **Yenileme** aracılığıyla geri yüklenen bloblara artımlı değişiklikler desteklenmez |Blob uç noktaları için, bir yenilemeden sonra Blobların kısmi güncelleştirmeleri, güncelleştirmelerin buluta yüklenmeme oluşmasına neden olabilir. Örneğin, gibi eylemler dizisi:<ul><li>Bulutta blob oluşturun. Ya da daha önce karşıya yüklenen bir blobu cihazdan silebilirsiniz.</li><li>Yenileme işlevini kullanarak buluttan blob 'u buluta yenileyin.</li><li>Azure SDK REST API 'Lerini kullanarak yalnızca blob 'un bir bölümünü güncelleştirin.</li></ul>Bu eylemler, blob 'un güncelleştirilmiş bölümlerinin bulutta güncelleştirilmesine neden olabilir. <br>**Geçici çözüm**: tüm Blobları değiştirmek için Robocopy veya gezgin veya komut satırı aracılığıyla normal dosya kopyalama gibi araçları kullanın.|
|**3.**|Azaltma|Daraltma sırasında, cihaza yeni yazma yapılmasına izin verilmiyorsa, NFS istemcisi tarafından yapılan yazma işlemleri "Izin reddedildi" hatasıyla başarısız olur.| Hata şu şekilde görünür:<br>`hcsuser@ubuntu-vm:~/nfstest$ mkdir test`<br>mkdir: ' test ' dizini oluşturulamıyor: Izin reddedildi|
|**4.**|BLOB depolama alımı|BLOB depolama alımı için AzCopy sürüm 10 ' u kullanırken, AzCopy öğesini aşağıdaki bağımsız değişkenle çalıştırın: `Azcopy <other arguments> --cap-mbps 2000`| AzCopy için bu sınırlar sağlanmazsa, cihaza çok sayıda istek gönderebilir ve hizmetle ilgili sorunlara neden olabilir.|
|**e.**|Katmanlı depolama hesapları|Katmanlı depolama hesapları kullanılırken aşağıdakiler geçerlidir:<ul><li> Yalnızca blok Blobları desteklenir. Sayfa Blobları desteklenmez.</li><li>Anlık görüntü veya kopyalama API 'SI desteği yoktur.</li><li> `distcp`Kopyalama işlemini yoğun bir şekilde kullandığından Hadoop iş yükü alımı desteklenmez.</li></ul>||
|**inç.**|NFS bağlantı paylaşma|Birden çok işlem aynı paylaşıma kopyalanalıyorsa ve `nolock` özniteliği kullanılmazsa, kopyalama sırasında hatalar görebilirsiniz.|`nolock`Dosyaları NFS paylaşımında kopyalamak için bağlama komutuna geçirilmesi gerekir. Örneğin: `C:\Users\aseuser mount -o anon \\10.1.1.211\mnt\vms Z:`.|
|**7.**|Kubernetes kümesi|Bir Kubernetes kümesi çalıştıran cihazınıza bir güncelleştirme uygulanırken, Kubernetes sanal makineleri yeniden başlatılır ve yeniden başlatılır. Bu örnekte, bir güncelleştirmeden sonra yalnızca belirtilen yinelemelerle dağıtılan bir dizin otomatik olarak geri yüklenir.  |Bir çoğaltma kümesi belirtmeden çoğaltma denetleyicisi dışında ayrı ayrı bir tane oluşturduysanız, bu FID 'ler cihaz güncelleştirmesinden sonra otomatik olarak geri yüklenmez. Bu pods 'yi geri yüklemeniz gerekir.<br>Bir çoğaltma kümesi, düğüm arızası veya kesintiye uğramayan düğüm yükseltmesi gibi herhangi bir nedenle silinen veya sonlandırılmış olan Pod 'yi değiştirir. Bu nedenle, uygulamanız yalnızca tek bir pod gerektirdiğinden bile bir çoğaltma kümesi kullanmanızı öneririz.|
|**240.**|Kubernetes kümesi|Azure Stack Edge üzerinde Kubernetes yalnızca helk v3 veya üzeri sürümlerle desteklenir. Daha fazla bilgi için [sık sorulan sorular: Tiller ' un kaldırılması](https://v3.helm.sh/docs/faq/)konusuna gidin.|
|**tuşlarına.**|Azure yay + Azure Stack Edge|Azure Stack Edge cihazınızda Web proxy yapılandırılmışsa, Azure Arc dağıtımları desteklenmez.||
|**(.**|Kubernetes |31000 numaralı bağlantı noktası Kubernetes panosu için ayrılmıştır. Benzer şekilde, varsayılan yapılandırmada, 172.28.0.1 ve 172.28.0.10 IP adresleri sırasıyla Kubernetes hizmeti ve çekirdek DNS hizmeti için ayrılmıştır.|Ayrılmış IP 'Leri kullanmayın.|
|**üst.**|Kubernetes |Kubernetes, şu anda çok protokollü yük dengeleyici hizmetlerine izin vermiyor. Örneğin, TCP ve UDP üzerinde dinleme yapmak zorunda olacak bir DNS hizmeti. |Kubernetes 'in bu şekilde, MetalLB ile bu sınırlamaya geçici bir çözüm bulmak için, iki hizmet (TCP için bir adet, UDP için bir adet) aynı Pod seçicisi üzerinde oluşturulabilir. Bu hizmetler aynı IP adresini paylaşmak için aynı paylaşım anahtarını ve spec. Loadbalancerıp 'yi kullanır. Kullanılabilir IP adreslerinden daha fazla hizmete sahipseniz, IP 'Ler de paylaşılabilir. <br> Daha fazla bilgi için bkz. [IP adresi paylaşımı](https://metallb.universe.tf/usage/#ip-address-sharing).|
|**+.**|Kubernetes kümesi|Mevcut Azure IoT Edge marketi modülleri, Kubernetes kümesinde Azure Stack Edge cihazında IoT Edge barındırma platformu olarak çalışmaz.|Modüllerin Azure Stack Edge cihazında dağıtılmadan önce değiştirilmesi gerekir. Daha fazla bilgi için bkz. Market 'ten Azure Stack Edge cihazında çalışacak Azure IoT Edge modülleri değiştirme.<!-- insert link-->|
|**hatası.**|Kubernetes |Dosya tabanlı bağlama takar, Azure Stack Edge cihazında Kubernetes üzerinde Azure IoT Edge desteklenmez.|IoT Edge, `ContainerCreate` seçenekleri Kubernetes yapılarına çevirmek için bir çeviri katmanı kullanır. `Binds`Hostpath dizinine haritalar oluşturma veya oluşturma ve böylece dosya tabanlı bağlama bağlama IoT Edge kapsayıcılarındaki yollara bağlanamaz.|
|**May.**|Kubernetes |IoT Edge için kendi sertifikalarınızı getirip Azure Stack Edge cihazınıza eklemek istiyorsanız, Helu grafik güncelleştirmesinin bir parçası olarak yeni sertifikalar alınmaz.|Bu soruna geçici bir çözüm olarak, [cihazın PowerShell arabirimine bağlanın](azure-stack-edge-gpu-connect-powershell-interface.md). Yeniden başlatma `iotedged` ve `edgehub` pods.|
|**aşamaz.**|Sertifikalar |Belirli örneklerde, yerel kullanıcı arabirimindeki sertifika durumunun güncelleştirilmesi birkaç saniye sürebilir. |Yerel Kullanıcı arabirimindeki aşağıdaki senaryolar etkilenebilir.<ul><li>**Sertifikalar** sayfasındaki **durum** sütunu.</li><li>**Başlarken** sayfasında **güvenlik** kutucuğu.</li><li>**Genel bakış** sayfasında **yapılandırma** kutucuğu.</li></ul>  |

## <a name="next-steps"></a>Sonraki adımlar

- [GPU ile Azure Stack Edge cihazını dağıtmaya hazırlanma](azure-stack-edge-gpu-deploy-prep.md)

