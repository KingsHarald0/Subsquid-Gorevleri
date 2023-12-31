# Subsquid Görev

Gerekli kütüphaneleri yüklüyoruz

```console
# Güncelleme ve docker kurulumu
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo docker run hello-world
```

```console
# Node.js kurulumu
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Node.js için yapılandırma klasörü oluşturup PATH yolunu ayarlıyoruz.

```console
mkdir ~/global-node-packages
npm config set prefix ~/global-node-packages
export PATH="${HOME}/global-node-packages/bin:$PATH"
```

Subsquid CLI kurulumunu yapıyoruz.
```console
npm install --global @subsquid/cli@latest
# Version kontrolü yapıyoruz
sqd --version 
# Çıktısı "@subsquid/cli/2.5.0 linux-x64 node-v20.5.1" benzeri olacaktır.
```

Squidi çalıştırıyoruz
```conosle
sqd init my-single-proc-squid -t https://github.com/subsquid-quests/single-chain-squid
cd my-single-proc-squid
```
Şimdi yapmamız gereken;Subsquidin görev sayfasından Get Key'e basıp dosyayı masaüstüne kaydediyoruz daha sonra Winscp/Mobax aracılığı ile my-single-proc-squid klasöründeki  `/query-gateway/keys`  klasörüne atıyoruz.
>Her görevde squidi çalıştırma aşamasından sonra görevdeki keyi indirip aynı şekilde klasöre atmak gerekiyor.Diğer görevlerde sadece klasörün adı double,triple,quad şeklinde değişecek key klasörünü açarken isimlere dikkat edersiniz.)

```console
# Squidi aktive ediyoruz.
sqd up
```
```console
# Suqidi çalıştırmadan önceki ayarlamaları yapıyoruz
npm ci
sqd build
sqd migration:apply
```
```console
# Squidi çalıştırıyoruz.
sqd run .
```
```console
# Loglar aşağıdaki gibi akacaktır.
[api] 09:56:02 WARN  sqd:graphql-server enabling dumb in-memory cache (size: 100mb, ttl: 1000ms, max-age: 1000ms)
[api] 09:56:02 INFO  sqd:graphql-server listening on port 4350
[processor] 09:56:04 INFO  sqd:processor processing blocks from 6000000
[processor] 09:56:05 INFO  sqd:processor using archive data source
[processor] 09:56:05 INFO  sqd:processor prometheus metrics are served at port 33097
[processor] 09:56:08 INFO  sqd:processor:mapping Burned 59865654 Gwei from 6000000 to 6016939
[processor] 09:56:08 INFO  sqd:processor 6016939 / 17743832, rate: 5506 blocks/sec, mapping: 304 blocks/sec, 182 items/sec, eta: 36m
```

Squidin sync olmasını bekliyoruz.Bu biraz zaman alacaktır.İlk 3'ü 15 dk içinde eşleşirken 4. daha fazla zaman almıştı bende.Sizin eşleşme süreniz daha farklı olabilir.Eşleştikten sonra How to yazısı yerine Claim yazısı gelecek.Claim ettikten sonra squidi kapatıp diğerine geçebilirsiniz.

```console
# Claim ettikten sonra önce CTRL+C ile squidden çıkıyoruz.Sonra da alttaki kod ile siliyoruz.
sqd down
```

Diğer 3 aşamada da işlemler aynı olduğu için sadece değişecek kısımları ekledim.Yani altta yazdığım kodlardan sonra 'Squidi aktive ediyoruz' aşamasından devam ediyoruz.
>Kodları girdikten sonra görev için gerekli Key'i indirip ilgili klasöre atmayı unutmayın.
```console
# 2. Görev Squid
sqd init my-double-proc-squid -t https://github.com/subsquid-quests/double-chain-squid
cd my-double-proc-squid
```
```console
# 3. Görev Squid
sqd init my-triple-proc-squid -t https://github.com/subsquid-quests/triple-chain-squid
cd my-triple-proc-squid
```
```console
# 4. Görev  Squid
sqd init my-quad-proc-squid -t https://github.com/subsquid-quests/quad-chain-squid
cd my-quad-proc-squid
```
