---
title: Azure Stack Edge Pro GA sürüm notları | Microsoft Docs
description: Azure Stack Edge Pro çalıştıran genel kullanım sürümü için kritik açık sorunları ve çözümleri açıklar.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 09/24/2020
ms.author: alkohli
ms.openlocfilehash: 9d3aafad457f5c72a8c45f4f98f8f03eb8d978d7
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91324272"
---
# <a name="azure-stack-edge-pro-with-gpu-general-availability-ga-release-notes"></a>Azure Stack Edge Pro GPU genel kullanılabilirliği (GA) sürüm notları

Aşağıdaki sürüm notları, GPU ile Azure Stack Edge Pro cihazlarınız için kritik açık sorunları ve genel kullanılabilirlik (GA) sürümü için çözülmüş sorunları belirler.

Sürüm notları sürekli olarak güncelleştirilir ve geçici bir çözüm gerektiren kritik sorunlar keşfedildiğinde eklenirler. Azure Stack Edge Pro cihazınızı dağıtmadan önce, sürüm notlarında bulunan bilgileri dikkatle gözden geçirin.

Bu makale, **2.1.1358.2075**yazılım sürüm numarası ile eşleşen **Azure Stack Edge Pro 2009** sürümü için geçerlidir.

## <a name="whats-new"></a>Yenilikler

Azure Stack Edge 2009 sürümünde aşağıdaki yeni özellikler mevcuttur. 

- Depolama **sınıfları** -bu sürümde, depolama sınıfları, depolamayı dinamik olarak sağlamanıza olanak tanıyan kullanılabilir. Daha fazla bilgi için [Azure Stack Edge Pro GPU cihazınızda Kubernetes depolama yönetimi](azure-stack-edge-gpu-kubernetes-storage.md#dynamicprovisioning)bölümüne bakın. 
- **Ölçüm sunucusu Ile Kubernetes panosu** -bu sürümde, bir ölçüm sunucusu eklentisi Ile bir Kubernetes panosu eklenir. Azure Stack Edge Pro cihazınızda çalışan uygulamalara genel bir bakış almak, Kubernetes küme kaynaklarının durumunu görüntülemek ve cihazda oluşan tüm hataları görmek için panoyu kullanabilirsiniz. Ölçüm sunucusu, cihazdaki Kubernetes kaynakları genelinde CPU ve bellek kullanımını toplar. Daha fazla bilgi için bkz. [Azure Stack Edge Pro GPU cihazınızı izlemek Için Kubernetes panosunu kullanma](azure-stack-edge-gpu-monitor-kubernetes-dashboard.md).
- **Azure Arc etkin kubernetes Azure Stack Edge Pro** -Bu sürümden başlayarak, uygulama iş yüklerini, Azure Arc etkinleştirilmiş Kubernetes aracılığıyla Azure Stack Edge Pro cihazınıza dağıtabilirsiniz. Azure Arc, Kubernetes kümelerinizde uygulamalar dağıtmanıza olanak sağlayan bir karma yönetim aracıdır. Daha fazla bilgi için, [Azure Stack Edge Pro cihazınızda Azure Arc aracılığıyla iş yüklerini dağıtma](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md)konusuna bakın.  

## <a name="known-issues"></a>Bilinen sorunlar 

Aşağıdaki tabloda Azure Stack Edge Pro cihazı için bilinen sorunların bir özeti verilmiştir.

| Hayır. | Öne çıkan özelliği | Sorun | Geçici çözüm/açıklamalar |
| --- | --- | --- | --- |
|**1.**|Önizleme özellikleri |Bu GA sürümünde şu özellikler bulunur: yerel Azure Resource Manager, VM 'Ler, Kubernetes, Azure Arc etkinleştirilmiş Kubernetes, GPU için çok Işlem hizmeti (MPS), Azure Stack Edge Pro cihazınız için önizleme aşamasındadır.  |Bu özellikler daha sonraki bir sürümde genel kullanıma sunulacaktır. |
| **2.** |Azure Stack Edge Pro + Azure SQL | SQL veritabanı oluşturma, yönetici erişimi gerektirir.   |İçindeki adımlar 1-2 yerine aşağıdaki adımları uygulayın [https://docs.microsoft.com/azure/iot-edge/tutorial-store-data-sql-server#create-the-sql-database](https://docs.microsoft.com/azure/iot-edge/tutorial-store-data-sql-server#create-the-sql-database) . <ul><li>Cihazınızın yerel kullanıcı arabiriminde, işlem arabirimini etkinleştirin. İşlem **> Için işlem > bağlantı noktası # > etkinleştir** ' i seçin.</li><li>`sqlcmd`İstemci makinenize şuradan indirinhttps://docs.microsoft.com/sql/tools/sqlcmd-utility </li><li>Adresin sonuna bir ", 1401" ekleyerek işlem arabirimi IP adresine (etkin olan bağlantı noktası) bağlanın.</li><li>Son komut şöyle görünür: sqlcmd-S {Interface IP}, 1401-U SA-P "Strong! Passw0rd".</li>Bundan sonra geçerli belgelerden 3-4 arasındaki adımlar aynı olmalıdır. </li></ul> |
| **3.** |Yenile| **Yenileme** aracılığıyla geri yüklenen bloblara artımlı değişiklikler desteklenmez |Blob uç noktaları için, bir yenilemeden sonra Blobların kısmi güncelleştirmeleri, güncelleştirmelerin buluta yüklenmeme oluşmasına neden olabilir. Örneğin, gibi eylemler dizisi:<ul><li>Bulutta blob oluşturun. Ya da daha önce karşıya yüklenen bir blobu cihazdan silebilirsiniz.</li><li>Yenileme işlevini kullanarak buluttan blob 'u buluta yenileyin.</li><li>Azure SDK REST API 'Lerini kullanarak yalnızca blob 'un bir bölümünü güncelleştirin.</li></ul>Bu eylemler, blob 'un güncelleştirilmiş bölümlerinin bulutta güncelleştirilmesine neden olabilir. <br>**Geçici çözüm**: tüm Blobları değiştirmek için Robocopy veya gezgin veya komut satırı aracılığıyla normal dosya kopyalama gibi araçları kullanın.|
|**4.**|Azaltma|Daraltma sırasında, cihaza yeni yazma yapılmasına izin verilmiyorsa, NFS istemcisi tarafından yapılan yazma işlemleri "Izin reddedildi" hatasıyla başarısız olur.| Hata şu şekilde görünür:<br>`hcsuser@ubuntu-vm:~/nfstest$ mkdir test`<br>mkdir: ' test ' dizini oluşturulamıyor: Izin reddedildi|
|**e.**|BLOB depolama alımı|BLOB depolama alımı için AzCopy sürüm 10 ' u kullanırken, AzCopy öğesini aşağıdaki bağımsız değişkenle çalıştırın: `Azcopy <other arguments> --cap-mbps 2000`| AzCopy için bu sınırlar sağlanmazsa, cihaza çok sayıda istek gönderebilir ve hizmetle ilgili sorunlara neden olabilir.|
|**inç.**|Katmanlı depolama hesapları|Katmanlı depolama hesapları kullanılırken aşağıdakiler geçerlidir:<ul><li> Yalnızca blok Blobları desteklenir. Sayfa Blobları desteklenmez.</li><li>Anlık görüntü veya kopyalama API 'SI desteği yoktur.</li><li> `distcp`Kopyalama işlemini yoğun bir şekilde kullandığından Hadoop iş yükü alımı desteklenmez.</li></ul>||
|**7.**|NFS bağlantı paylaşma|Birden çok işlem aynı paylaşıma kopyalanalıyorsa ve `nolock` özniteliği kullanılmazsa, kopyalama sırasında hatalar görebilirsiniz.|`nolock`Dosyaları NFS paylaşımında kopyalamak için bağlama komutuna geçirilmesi gerekir. Örneğin: `C:\Users\aseuser mount -o anon \\10.1.1.211\mnt\vms Z:`.|
|**240.**|Kubernetes kümesi|Bir Kubernetes kümesi çalıştıran cihazınıza bir güncelleştirme uygulanırken, Kubernetes sanal makineleri yeniden başlatılır ve yeniden başlatılır. Bu örnekte, bir güncelleştirmeden sonra yalnızca belirtilen yinelemelerle dağıtılan bir dizin otomatik olarak geri yüklenir.  |Bir çoğaltma kümesi belirtmeden çoğaltma denetleyicisi dışında ayrı ayrı bir tane oluşturduysanız, bu FID 'ler cihaz güncelleştirmesinden sonra otomatik olarak geri yüklenmez. Bu pods 'yi geri yüklemeniz gerekir.<br>Bir çoğaltma kümesi, düğüm arızası veya kesintiye uğramayan düğüm yükseltmesi gibi herhangi bir nedenle silinen veya sonlandırılmış olan Pod 'yi değiştirir. Bu nedenle, uygulamanız yalnızca tek bir pod gerektirdiğinden bile bir çoğaltma kümesi kullanmanızı öneririz.|
|**tuşlarına.**|Kubernetes kümesi|Azure Stack Edge Pro 'da Kubernetes yalnızca helk v3 veya üzeri sürümlerle desteklenir. Daha fazla bilgi için [sık sorulan sorular: Tiller ' un kaldırılması](https://v3.helm.sh/docs/faq/)konusuna gidin.|
|**(.**|Azure Arc özellikli Kubernetes |GA sürümünde, Azure Arc, Kubernetes, 0.1.18 sürümünden 0.2.9 sürümüne güncelleştirilir. Azure Arc etkinleştirilmiş Kubernetes güncelleştirmesi Azure Stack Edge cihazında desteklenmediğinden, Azure Arc etkinleştirilmiş Kubernetes 'i yeniden dağıtmanız gerekir.|Şu adımları izleyin:<ol><li>[Cihaz yazılımını ve Kubernetes güncelleştirmelerini uygulayın](azure-stack-edge-gpu-install-update.md).</li><li>[Cihazın PowerShell arabirimine](azure-stack-edge-gpu-connect-powershell-interface.md)bağlanın.</li><li>Mevcut Azure yay aracısını kaldırın. Şunu yazın: `Remove-HcsKubernetesAzureArcAgent` .</li><li>[Azure yayı 'yi yeni bir kaynağa](azure-stack-edge-gpu-deploy-arc-kubernetes-cluster.md)dağıtın. Mevcut bir Azure yay kaynağını kullanmayın.</li></ol>|
|**üst.**|Azure Arc özellikli Kubernetes|Azure Stack Edge Pro cihazınızda Web proxy yapılandırıldıysa, Azure Arc dağıtımları desteklenmez.||
|**+.**|Kubernetes |31000 numaralı bağlantı noktası Kubernetes panosu için ayrılmıştır. Benzer şekilde, varsayılan yapılandırmada, 172.28.0.1 ve 172.28.0.10 IP adresleri sırasıyla Kubernetes hizmeti ve çekirdek DNS hizmeti için ayrılmıştır.|Ayrılmış IP 'Leri kullanmayın.|
|**hatası.**|Kubernetes |Kubernetes, şu anda çok protokollü yük dengeleyici hizmetlerine izin vermiyor. Örneğin, TCP ve UDP üzerinde dinleme yapmak zorunda olacak bir DNS hizmeti. |Kubernetes 'in bu şekilde, MetalLB ile bu sınırlamaya geçici bir çözüm bulmak için, iki hizmet (TCP için bir adet, UDP için bir adet) aynı Pod seçicisi üzerinde oluşturulabilir. Bu hizmetler aynı IP adresini paylaşmak için aynı paylaşım anahtarını ve spec. Loadbalancerıp 'yi kullanır. Kullanılabilir IP adreslerinden daha fazla hizmete sahipseniz, IP 'Ler de paylaşılabilir. <br> Daha fazla bilgi için bkz. [IP adresi paylaşımı](https://metallb.universe.tf/usage/#ip-address-sharing).|
|**May.**|Kubernetes kümesi|Mevcut Azure IoT Edge Market modülleri, Azure Stack Edge cihazında IoT Edge çalıştırmak için değişiklikler gerektirebilir.|Daha fazla bilgi için bkz. Market 'ten Azure Stack Edge cihazında çalışacak Azure IoT Edge modülleri değiştirme.<!-- insert link-->|
|**aşamaz.**|Kubernetes |Dosya tabanlı bağlama takar, Azure Stack Edge cihazında Kubernetes üzerinde Azure IoT Edge desteklenmez.|IoT Edge, `ContainerCreate` seçenekleri Kubernetes yapılarına çevirmek için bir çeviri katmanı kullanır. `Binds`Hostpath dizinine haritalar oluşturuluyor ve bu nedenle dosya tabanlı bağlama, IoT Edge kapsayıcılarındaki yollara bağlanamaz. Mümkünse, üst dizini eşleyin.|
|**k.**|Kubernetes |IoT Edge için kendi sertifikalarınızı getirir ve işlem cihazda yapılandırıldıktan sonra Azure Stack Edge cihazınıza eklerseniz, yeni sertifikalar alınmaz.|Bu sorunu geçici olarak çözmek için, cihazda işlem yapılandırmadan önce sertifikaları karşıya yüklemeniz gerekir. İşlem zaten yapılandırıldıysa, [cihazın PowerShell arabirimine bağlanın ve IoT Edge komutlarını çalıştırın](azure-stack-edge-gpu-connect-powershell-interface.md#use-iotedge-commands). Yeniden başlatma `iotedged` ve `edgehub` pods.|
|**17.**|Sertifikalar |Belirli örneklerde, yerel kullanıcı arabirimindeki sertifika durumunun güncelleştirilmesi birkaç saniye sürebilir. |Yerel Kullanıcı arabirimindeki aşağıdaki senaryolar etkilenebilir.<ul><li>**Sertifikalar** sayfasındaki **durum** sütunu.</li><li>**Başlarken** sayfasında **güvenlik** kutucuğu.</li><li>**Genel bakış** sayfasında **yapılandırma** kutucuğu.</li></ul>  |
|**17.**|IoT Edge |IoT Edge aracılığıyla dağıtılan modüller konak ağını kullanamaz. | |
|**18.**|İşlem + Kubernetes |İşlem/Kubernetes, NTLM Web proxy 'sini desteklemez. ||
|**renkli.**|İşlem + Web proxy + güncelleştirme |Web proxy ile yapılandırılmış bir işlem varsa, işlem güncelleştirmesi başarısız olabilir. |Güncelleştirmeden önce işlem için devre dışı bırakmanız önerilir. |

<!--|**18.**|Azure Private Edge Zone (Preview) |There is a known issue with Virtual Network Function VM if the VM was created on Azure Stack Edge device running earlier preview builds such as 2006/2007b and then the device was updated to 2009 GA release. The issue is that the VNF information can't be retrieved or any new VNFs can't be created unless the VNF VMs are deleted before the device is updated.  |Before you update Azure Stack Edge device to 2009 release, use the PowerShell command `get-mecvnf` followed by `remove-mecvnf <VNF guid>` to remove all Virtual Network Function VMs one at a time. After the upgrade, you will need to redeploy the same VNFs.|-->




## <a name="next-steps"></a>Sonraki adımlar

- [GPU ile Azure Stack Edge Pro cihazını dağıtmaya hazırlanma](azure-stack-edge-gpu-deploy-prep.md)

