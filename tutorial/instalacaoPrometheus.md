## Instalação do Prometheus no Linux

1- Pegue o link de download da versão desejada no site so Prometheus (https://prometheus.io/download/);

<br/>

2- Baixe o arquivo no linux:

```
wget https://github.com/prometheus/prometheus/releases/download/v2.37.5/prometheus-2.37.5.linux-amd64.tar.gz
```

<br/>

3 - Descompacte e entre no diretório:
```
tar -xzvf prometheus-2.37.5.linux-amd64.tar.gz
cd prometheus-2.37.5.linux-amd64
```

<br/>

4 - Agora vamos mover os binários para o diretório /usr/local/bin:
```
sudo mv prometheus-2.38.0.linux-amd64/prometheus /usr/local/bin/prometheus
sudo mv prometheus-2.38.0.linux-amd64/promtool /usr/local/bin/promtool
```

<br/>

5 - Crie o diretorio de configuração e mova os arquivos de configuração e biblioteca para lá:
```
sudo mkdir /etc/prometheus
sudo mv prometheus-2.38.0.linux-amd64/prometheus.yml /etc/prometheus/prometheus.yml
sudo mv prometheus-2.38.0.linux-amd64/consoles /etc/prometheus
sudo mv prometheus-2.38.0.linux-amd64/console_libraries /etc/prometheus
```

<br/>

6 - Edite o arquivo de configuração do Prometheus conforme [/conf/prometheus.yml](/conf/prometheus.yml);
```
sudo vim /etc/prometheus/prometheus.yml
```
<br/>

7 - Crie o diretorio para armazenamento dos dados do Prometheus:
```
sudo mkdir /var/lib/prometheus
```

<br/>

8 - Crie um usuario e grupo para ser usado pelo Prometheus:
```
sudo addgroup --system prometheus
sudo adduser --shell /sbin/nologin --system --group prometheus
```

<br>

9 - Dê as devidas permissões para o usuario/grupo do Prometheus nos devidos diretorios:
```
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
sudo chown -R prometheus:prometheus /usr/local/bin/prometheus
sudo chown -R prometheus:prometheus /usr/local/bin/promtool
```

<br/>

10 - Para fazer com que o Prometheus rode como serviço, precisamos criar o arquivo `/etc/systemd/system/promtetheus.service`:
```
sudo vim /etc/systemd/system/prometheus.service
```

E deixar o conteudo conforme [/conf/prometheus.service](/conf/prometheus.service).

<br/>

11 - Reiniciar o daemon do Linux:
```
systemctl daemon-reload
```

<br>


12 - Dar enable no serviço e inicia-lo:
```
systemctl enable protheus --now
```
OBS: a flag --now vai fazer com que o serviço seja habilitado e ja iniciado.

<br>

13 - Confira se o serviço está rodando:
```
systemctl status prometheus
```

<br>

14 - Feito isso é so testar o acesso pelo `http://localhost:9090` ou apontando para o IP do servidor, em caso de acesso externo. 