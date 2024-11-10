Vagrant.configure("2") do |config|
  # Configuración del Servidor Apache 1
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/bionic64"
    web1.vm.network "private_network", ip: "192.168.33.10"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    # Ruta absoluta para Windows
    web1.vm.synced_folder "C:/Users/adria/Automatizacion/web1", "/var/www/html"
    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
    SHELL
  end

  # Configuración del Servidor Apache 2
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/bionic64"
    web2.vm.network "private_network", ip: "192.168.33.11"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    # Ruta absoluta para Windows
    web2.vm.synced_folder "C:/Users/adria/Automatizacion/web2", "/var/www/html"
    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo systemctl enable apache2
    SHELL
  end

  # Configuración del Servidor Nginx para Balanceo de Carga
  config.vm.define "nginx" do |nginx|
    nginx.vm.box = "ubuntu/bionic64"
    nginx.vm.network "private_network", ip: "192.168.33.12"
    nginx.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    nginx.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y nginx
      # Configurar Nginx como balanceador de carga
      echo '
      upstream web_servers {
          server 192.168.33.10;
          server 192.168.33.11;
      }

      server {
          listen 80;
          location / {
              proxy_pass http://web_servers;
          }
      }
      ' | sudo tee /etc/nginx/sites-available/default
      sudo systemctl restart nginx
      sudo systemctl enable nginx
    SHELL
  end
end
