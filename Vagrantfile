Vagrant.configure("2") do |config|
  # Configuración de DNSA (servidor maestro)
  config.vm.define "DNSA" do |dnsa|
    dnsa.vm.box = "debian/bookworm64"  # Usando Ubuntu 20.04 LTS
    dnsa.vm.network "private_network", ip: "192.168.57.10"
    dnsa.vm.hostname = "dnsa"
    dnsa.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y bind9
     # Restaurar archivos de configuración de BIND
  cp /vagrant/DNSA/named.conf /etc/bind/named.conf
  cp /vagrant/DNSA/named.conf.local /etc/bind/named.conf.local
  cp /vagrant/DNSA/named.conf.options /etc/bind/named.conf.options
  cp /vagrant/DNSA/db.ies.test /etc/bind/db.ies.test
  cp /vagrant/DNSA/db.informatica.ies.test /etc/bind/db.informatica.ies.test
  cp /vagrant/DNSA/db.aulas.ies.test /etc/bind/db.aulas.ies.test
  cp /vagrant/DNSA/db.192.168.57 /etc/bind/db.192.168.57

  # Restaurar archivo resolv.conf
  cp /vagrant/DNSA/resolv.conf /etc/resolv.conf

  # Crear el archivo de logs si no existe
  touch /var/lib/bind/bind.log
  chown bind:bind /var/lib/bind/bind.log
  chmod 640 /var/lib/bind/bind.log

  # Reiniciar el servicio de BIND
  systemctl restart bind9
    SHELL
  end

  # Configuración de DNSB (servidor esclavo)
  config.vm.define "DNSB" do |dnsb|
    dnsb.vm.box = "debian/bookworm64"
    dnsb.vm.network "private_network", ip: "192.168.57.20"
    dnsb.vm.hostname = "dnsb"
    dnsb.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y bind9
     # Restaurar archivos de configuración de BIND
  cp /vagrant/DNSB/named.conf /etc/bind/named.conf
  cp /vagrant/DNSB/named.conf.local /etc/bind/named.conf.local
  cp /vagrant/DNSB/named.conf.options /etc/bind/named.conf.options

  # Restaurar archivos esclavos si existen
  cp /vagrant/DNSB/slave.db.informatica.ies.test /var/cache/bind/slave.db.informatica.ies.test || true
  cp /vagrant/DNSB/slave.db.aulas.ies.test /var/cache/bind/slave.db.aulas.ies.test || true

  # Restaurar archivo resolv.conf
  cp /vagrant/DNSB/resolv.conf /etc/resolv.conf

  # Reiniciar el servicio de BIND
  systemctl restart bind9
    
    SHELL
  end
end
